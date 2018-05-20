---
title: Criar um cluster do Service Fabric do Azure com o nome comum do certificado | Microsoft Docs
description: Saiba como criar um cluster do Service Fabric com o nome comum do certificado a partir de um modelo.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/24/2018
ms.author: ryanwi
ms.openlocfilehash: 8725dd1931b120b0369d0810fa49108a00c71e8e
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
---
# <a name="deploy-a-service-fabric-cluster-that-uses-certificate-common-name-instead-of-thumbprint"></a>Implementar um cluster de Service Fabric utiliza o nome comum do certificado em vez de impressão digital
Não existem dois certificados podem ter o mesmo thumbprint, o que torna difícil rollover de certificado de cluster ou de gestão. No entanto, vários certificados, podem ter o mesmo nome comum ou o assunto.  Um cluster utilizando nomes comum do certificado torna muito mais simples a gestão de certificados. Este artigo descreve como implementar um cluster do Service Fabric para utilizar o nome comum do certificado em vez do thumbprint do certificado.
 
## <a name="get-a-certificate"></a>Obter um certificado
Em primeiro lugar, obter um certificado de um [autoridade (AC) de certificado](https://wikipedia.org/wiki/Certificate_authority).  O nome comum do certificado deve ser o nome de anfitrião do cluster.  Por exemplo, "myclustername.southcentralus.cloudapp.azure.com".  

Para fins de teste, pode obter um certificado assinado pela AC de uma autoridade de certificação livre ou abrir.

> [!NOTE]
> Certificados autoassinados, incluindo as que são gerados quando implementar um cluster do Service Fabric no portal do Azure, não são suportados.

## <a name="upload-the-certificate-to-a-key-vault"></a>Carregue o certificado para um cofre de chaves
No Azure, um cluster do Service Fabric é implementado num conjunto de dimensionamento de máquina virtual.  Carregue o certificado para um cofre de chaves.  Quando implementa o cluster, instala o certificado no conjunto de dimensionamento de máquina virtual que o cluster está em execução.

```powershell
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser -Force

$SubscriptionId  =  "<subscription ID>"

# Sign in to your Azure account and select your subscription
Login-AzureRmAccount -SubscriptionId $SubscriptionId

$region = "southcentralus"
$KeyVaultResourceGroupName  = "mykeyvaultgroup"
$VaultName = "mykeyvault"
$certFilename = "C:\users\sfuser\myclustercert.pfx"
$certname = "myclustercert"
$Password  = "P@ssw0rd!123"

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
```

## <a name="download-and-update-a-sample-template"></a>Transferir e atualizar um modelo de exemplo
Este artigo utiliza o [exemplo 5-nó de cluster segura](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Windows-1-NodeTypes-Secure) modelo e os parâmetros do modelo. Transferir o *azuredeploy. JSON* e *azuredeploy.parameters.json* ficheiros para o seu computador.

### <a name="update-parameters-file"></a>Atualizar o ficheiro de parâmetros
Primeiro, abra o *azuredeploy.parameters.json* ficheiro num editor de texto e adicione o seguinte valor de parâmetro:
```json
"certificateCommonName": {
    "value": "myclustername.southcentralus.cloudapp.azure.com"
},
```

Em seguida, configure o *certificateCommonName*, *sourceVaultValue*, e *certificateUrlValue* valores de parâmetros para aqueles obtidos pelo script anterior:
```json
"certificateCommonName": {
    "value": "myclustername.southcentralus.cloudapp.azure.com"
},
"sourceVaultValue": {
  "value": "/subscriptions/<subscription>/resourceGroups/testvaultgroup/providers/Microsoft.KeyVault/vaults/testvault"
},
"certificateUrlValue": {
  "value": "https://testvault.vault.azure.net:443/secrets/testcert/5c882b7192224447bbaecd5a46962655"
},
```

### <a name="update-the-template-file"></a>Atualizar o ficheiro de modelo
Em seguida, abra o *azuredeploy. JSON* ficheiro num editor de texto e disponibilizar atualizações de três para suportar o nome comum do certificado.

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

2. Defina o valor da *sfrpApiVersion* variável para "2018-02-01":
    ```json
    "sfrpApiVersion": "2018-02-01",
    ```

3. No **Microsoft.Compute/virtualMachineScaleSets** recurso, atualize a extensão de máquina virtual para utilizar o nome comum nas definições de certificado em vez do thumbprint.  No **virtualMachineProfile**->**extenstionProfile**->**extensões**->**propriedades** -> **definições**->**certificado**, adicionar `"commonNames": ["[parameters('certificateCommonName')]"],` e remover `"thumbprint": "[parameters('certificateThumbprint')]",`.
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

4.  No **Microsoft.ServiceFabric/clusters** recurso, a API de atualização de versão para "2018-02-01".  Também adicionar um **certificateCommonNames** definição com um **commonNames** propriedade e remover o **certificado** definição (com a propriedade thumbprint) como no seguinte exemplo:
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
# Variables.
$groupname = "testclustergroup"
$clusterloc="southcentralus"  
$id="<subscription ID"

# Sign in to your Azure account and select your subscription
Login-AzureRmAccount -SubscriptionId $id 

# Create a new resource group and deploy the cluster.
New-AzureRmResourceGroup -Name $groupname -Location $clusterloc

New-AzureRmResourceGroupDeployment -ResourceGroupName $groupname -TemplateParameterFile "C:\temp\cluster\AzureDeploy.Parameters.json" -TemplateFile "C:\temp\cluster\AzureDeploy.json" -Verbose
```

## <a name="next-steps"></a>Passos Seguintes
* Saiba mais sobre [cluster segurança](service-fabric-cluster-security.md).
* Saiba como [rollover um certificado de cluster](service-fabric-cluster-rollover-cert-cn.md)
* [Atualizar e gerir certificados de cluster](service-fabric-cluster-security-update-certs-azure.md)

[image1]: .\media\service-fabric-cluster-change-cert-thumbprint-to-cn\PortalViewTemplates.png