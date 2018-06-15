---
title: Atualizar um cluster do Service Fabric do Azure para utilizar o nome comum do certificado | Microsoft Docs
description: Saiba como mudar de um cluster do Service Fabric da utilização de thumbprints de certificado a utilizar o nome comum do certificado.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: aljo
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/24/2018
ms.author: ryanwi
ms.openlocfilehash: 39dc5800edd743cdc950c7a96f7633fb4c0a7c45
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
ms.locfileid: "34210346"
---
# <a name="change-cluster-from-certificate-thumbprint-to-common-name"></a>Altere o cluster de thumbprint do certificado para o nome comum
Não existem dois certificados podem ter o mesmo thumbprint, o que torna difícil rollover de certificado de cluster ou de gestão. No entanto, vários certificados, podem ter o mesmo nome comum ou o assunto.  Mudança de um cluster da utilização de thumbprints de certificado para utilizar nomes comuns do certificado implementado facilita a gestão de certificados muito mais simples. Este artigo descreve como atualizar um cluster do Service Fabric em execução para utilizar o nome comum do certificado em vez do thumbprint do certificado.
 
## <a name="get-a-certificate"></a>Obter um certificado
Em primeiro lugar, obter um certificado de um [autoridade (AC) de certificado](https://wikipedia.org/wiki/Certificate_authority).  O nome comum do certificado deve ser o nome de anfitrião do cluster.  Por exemplo, "myclustername.southcentralus.cloudapp.azure.com".  

Para fins de teste, pode obter um certificado assinado pela AC de uma autoridade de certificação livre ou abrir.

> [!NOTE]
> Certificados autoassinados, incluindo as que são gerados quando implementar um cluster do Service Fabric no portal do Azure, não são suportados.

## <a name="upload-the-certificate-and-install-it-in-the-scale-set"></a>Carregue o certificado e instalá-lo no conjunto de dimensionamento
No Azure, um cluster do Service Fabric é implementado num conjunto de dimensionamento de máquina virtual.  Carregue o certificado para um cofre de chaves e, em seguida, instalá-lo no conjunto de dimensionamento de máquina virtual que o cluster está em execução.

```powershell
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser -Force

$SubscriptionId  =  "<subscription ID>"

# Sign in to your Azure account and select your subscription
Login-AzureRmAccount -SubscriptionId $SubscriptionId

$region = "southcentralus"
$KeyVaultResourceGroupName  = "mykeyvaultgropu"
$VaultName = "mykeyvault"
$certFilename = "C:\users\sfuser\myclustercert.pfx"
$certname = "myclustercert"
$Password  = "P@ssw0rd!123"
$VmssResourceGroupName     = "myclustergroup"
$VmssName                  = "prnninnxj"

# Create new Resource Group 
New-AzureRmResourceGroup -Name $KeyVaultResourceGroupName -Location $region

# Create the new key vault
$newKeyVault = New-AzureRmKeyVault -VaultName $VaultName -ResourceGroupName $KeyVaultResourceGroupName -Location $region -EnabledForDeployment 
$resourceId = $newKeyVault.ResourceId 

# Add the certificate to the key vault.
$PasswordSec = ConvertTo-SecureString -String $Password -AsPlainText -Force
$KVSecret = Import-AzureKeyVaultCertificate -VaultName $vaultName -Name $certName  -FilePath $certFilename -Password $PasswordSec

$CertificateThumbprint = $KVSecret.Thumbprint
$CertificateURL = $KVSecret.SecretId
$SourceVault = $resourceId
$CommName    = $KVSecret.Certificate.SubjectName.Name

Write-Host "CertificateThumbprint    :"  $CertificateThumbprint
Write-Host "CertificateURL           :"  $CertificateURL
Write-Host "SourceVault              :"  $SourceVault
Write-Host "Common Name              :"  $CommName    

Set-StrictMode -Version 3
$ErrorActionPreference = "Stop"

$certConfig = New-AzureRmVmssVaultCertificateConfig -CertificateUrl $CertificateURL -CertificateStore "My"

# Get current VM scale set 
$vmss = Get-AzureRmVmss -ResourceGroupName $VmssResourceGroupName -VMScaleSetName $VmssName

# Add new secret to the VM scale set.
$vmss = Add-AzureRmVmssSecret -VirtualMachineScaleSet $vmss -SourceVaultId $SourceVault -VaultCertificate $certConfig

# Update the VM scale set 
Update-AzureRmVmss -ResourceGroupName $VmssResourceGroupName -Name $VmssName -VirtualMachineScaleSet $vmss  -Verbose
```

## <a name="download-and-update-the-template-from-the-portal"></a>Transferir e atualizar o modelo do portal
O certificado foi instalado no conjunto de dimensionamento subjacente, mas também tem de atualizar o cluster do Service Fabric para utilizar esse certificado e o nome comum.  Agora, transfira o modelo para a implementação de cluster.  Inicie sessão no [portal do Azure](https://portal.azure.com) e navegue para o grupo de recursos que aloja o cluster.  No **definições**, selecione **implementações**.  Selecione a implementação mais recente e clique em **ver modelo**.

![Modelos de vista][image1]

Transferir os ficheiros JSON do modelo e os parâmetros para o seu computador local.

Em primeiro lugar, abra o ficheiro de parâmetros num editor de texto e adicione o seguinte valor de parâmetro:
```json
"certificateCommonName": {
    "value": "myclustername.southcentralus.cloudapp.azure.com"
},
```

Em seguida, abra o ficheiro de modelo num editor de texto e introduza três atualizações para suportar o nome comum do certificado.

1. No **parâmetros** secção, adicione um *certificateCommonName* parâmetro:
    ```json
    "certificateCommonName": {
      "type": "string",
      "metadata": {
        "description": "Certificate Commonname"
      }
    },
    ```

    Considere também a remover o *certificateThumbprint*, já não poderá ser necessário.

2. No **Microsoft.Compute/virtualMachineScaleSets** recurso, atualize a extensão de máquina virtual para utilizar o nome comum nas definições de certificado em vez do thumbprint.  No **virtualMachineProfile**->**extenstionProfile**->**extensões**->**propriedades** -> **definições**->**certificado**, adicionar `"commonNames": ["[parameters('certificateCommonName')]"],` e remover `"thumbprint": "[parameters('certificateThumbprint')]",`.
    ```json
    "virtualMachineProfile": {
              "extensionProfile": {
                "extensions": [
                  {
                    "name": "[concat('ServiceFabricNodeVmExt','_vmNodeType0Name')]",
                    "properties": {
                      "type": "ServiceFabricNode",
                      "autoUpgradeMinorVersion": true,
                      "protectedSettings": {
                        "StorageAccountKey1": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('supportLogStorageAccountName')),'2015-05-01-preview').key1]",
                        "StorageAccountKey2": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('supportLogStorageAccountName')),'2015-05-01-preview').key2]"
                      },
                      "publisher": "Microsoft.Azure.ServiceFabric",
                      "settings": {
                        "clusterEndpoint": "[reference(parameters('clusterName')).clusterEndpoint]",
                        "nodeTypeRef": "[variables('vmNodeType0Name')]",
                        "dataPath": "D:\\SvcFab",
                        "durabilityLevel": "Bronze",
                        "enableParallelJobs": true,
                        "nicPrefixOverride": "[variables('subnet0Prefix')]",
                        "certificate": {
                          "commonNames": ["[parameters('certificateCommonName')]"],                          
                          "x509StoreName": "[parameters('certificateStoreValue')]"
                        }
                      },
                      "typeHandlerVersion": "1.0"
                    }
                  },
    ```

3.  No **Microsoft.ServiceFabric/clusters** recurso, a API de atualização de versão para "2018-02-01".  Também adicionar um **certificateCommonNames** definição com um **commonNames** propriedade e remover o **certificado** definição (com a propriedade thumbprint) como no seguinte exemplo:
    ```json
    {
        "apiVersion": "2018-02-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        "dependsOn": [
        "[concat('Microsoft.Storage/storageAccounts/', variables('supportLogStorageAccountName'))]"
        ],
        "properties": {
        "addonFeatures": [
            "DnsService",
            "RepairManager"
        ],        
        "certificateCommonNames": {
            "commonNames": [
            {
                "certificateCommonName": "[parameters('certificateCommonName')]",
                "certificateIssuerThumbprint": ""
            }
            ],
            "x509StoreName": "[parameters('certificateStoreValue')]"
        },
        ...
    ```

## <a name="deploy-the-updated-template"></a>Implementar o modelo atualizado
Volte a implementar o modelo atualizado depois de efetuar as alterações.

```powershell
$groupname = "sfclustertutorialgroup"
$clusterloc="southcentralus"

New-AzureRmResourceGroup -Name $groupname -Location $clusterloc

New-AzureRmResourceGroupDeployment -ResourceGroupName $groupname -TemplateParameterFile "C:\temp\cluster\parameters.json" -TemplateFile "C:\temp\cluster\template.json" -Verbose
```

## <a name="next-steps"></a>Passos Seguintes
* Saiba mais sobre [cluster segurança](service-fabric-cluster-security.md).
* Saiba como [rollover um certificado de cluster](service-fabric-cluster-rollover-cert-cn.md)
* [Atualizar e gerir certificados de cluster](service-fabric-cluster-security-update-certs-azure.md)

[image1]: .\media\service-fabric-cluster-change-cert-thumbprint-to-cn\PortalViewTemplates.png