---
title: Implementar uma máquina virtual com um certificado armazenado em segurança na pilha do Azure | Microsoft Docs
description: Saiba como implementar uma máquina virtual e emitir um certificado no mesmo, utilizando um cofre de chaves na pilha do Azure
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 46590eb1-1746-4ecf-a9e5-41609fde8e89
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/10/2018
ms.author: mabrigg
ms.openlocfilehash: 3950c9dfc5ff5f7ea1d170da086b4f97048ed81c
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/11/2018
ms.locfileid: "34069038"
---
# <a name="create-a-virtual-machine-and-install-a-certificate-retrieved-from-an-azure-stack-key-vault"></a>Criar uma máquina virtual e instale um certificado obtido a partir de um cofre de chaves de pilha do Azure

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

Saiba como criar uma máquina virtual de pilha do Azure (VM) com um certificado de chave de cofre instalado.

## <a name="overview"></a>Descrição geral

Os certificados são utilizados em vários cenários, como autenticar para o Active Directory ou encriptar tráfego web. Pode armazenar com segurança certificados como segredos no Cofre de chaves pilha do Azure. As vantagens de utilizar o Cofre de chaves de pilha do Azure são:

* Certificados não são expostos num script, o histórico da linha de comandos ou o modelo.
* O processo de gestão de certificados simplificado.
* Tem controlo de chaves que aceder a certificados.

### <a name="process-description"></a>Descrição do processo

Os passos seguintes descrevem o processo necessário para emitir um certificado para a máquina virtual:

1. Crie um cofre de chave secreta.
2. Atualize o ficheiro azuredeploy.parameters.json.
3. Implementar o modelo

>[!NOTE]
>Pode utilizar estes passos o Kit de desenvolvimento de pilha do Azure ou a partir de um cliente externo se estiver ligado através de VPN.

## <a name="prerequisites"></a>Pré-requisitos

* Tem de subscrever uma oferta, que inclui o serviço Cofre de chaves.
* [Instale o PowerShell para a pilha do Azure.](azure-stack-powershell-install.md)
* [Configurar o Azure pilha ambiente do utilizador do PowerShell](azure-stack-powershell-configure-user.md)

## <a name="create-a-key-vault-secret"></a>Criar um cofre de chave secreta

O script seguinte cria um certificado no formato. pfx, cria um cofre de chaves e armazena o certificado no Cofre de chaves como um segredo.

>[!IMPORTANT]
>Tem de utilizar o `-EnabledForDeployment` parâmetro ao criar a chave de falhas. Este parâmetro assegura que o Cofre de chaves pode ser referenciado a partir de modelos Azure Resource Manager.

```powershell

# Create a certificate in the .pfx format
New-SelfSignedCertificate `
  -certstorelocation cert:\LocalMachine\My `
  -dnsname contoso.microsoft.com

$pwd = ConvertTo-SecureString `
  -String "<Password used to export the certificate>" `
  -Force `
  -AsPlainText

Export-PfxCertificate `
  -cert "cert:\localMachine\my\<Certificate Thumbprint that was created in the previous step>" `
  -FilePath "<Fully qualified path where the exported certificate can be stored>" `
  -Password $pwd

# Create a key vault and upload the certificate into the key vault as a secret
$vaultName = "contosovault"
$resourceGroup = "contosovaultrg"
$location = "local"
$secretName = "servicecert"
$fileName = "<Fully qualified path where the exported certificate can be stored>"
$certPassword = "<Password used to export the certificate>"

$fileContentBytes = get-content $fileName `
  -Encoding Byte

$fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)
$jsonObject = @"
{
"data": "$filecontentencoded",
"dataType" :"pfx",
"password": "$certPassword"
}
"@
$jsonObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
$jsonEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)

New-AzureRmResourceGroup `
  -Name $resourceGroup `
  -Location $location

New-AzureRmKeyVault `
  -VaultName $vaultName `
  -ResourceGroupName $resourceGroup `
  -Location $location `
  -sku standard `
  -EnabledForDeployment

$secret = ConvertTo-SecureString `
  -String $jsonEncoded `
  -AsPlainText -Force

Set-AzureKeyVaultSecret `
  -VaultName $vaultName `
  -Name $secretName `
   -SecretValue $secret

```

Quando executar o script anterior, o resultado inclui o URI do segredo. Anote este URI. Tem de referenciá-lo no [: certificado Push inválido para o modelo do Windows Resource Manager](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/201-vm-windows-pushcertificate). Transferir o [modelo de vm push-certificado windows](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/201-vm-windows-pushcertificate) pasta no seu computador de desenvolvimento. Esta pasta contém os `azuredeploy.json` e `azuredeploy.parameters.json` ficheiros, que irá precisar nos passos.

Modificar o `azuredeploy.parameters.json` ficheiros, de acordo com os valores de ambiente. Os parâmetros do especial interesse são o nome do cofre, o grupo de recursos do cofre e o segredo do URI (como gerado pelo script anterior). O ficheiro seguinte está um exemplo de um ficheiro de parâmetros:

## <a name="update-the-azuredeployparametersjson-file"></a>Atualizar o ficheiro azuredeploy.parameters.json

Atualize o ficheiro de azuredeploy.parameters.json com o vaultName, URI secreta, VmName e outros valores de acordo com o seu ambiente. O ficheiro JSON seguinte mostra um exemplo do ficheiro de parâmetros de modelo:

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "newStorageAccountName": {
      "value": "kvstorage01"
    },
    "vmName": {
      "value": "VM1"
    },
    "vmSize": {
      "value": "Standard_D1_v2"
    },
    "adminUserName": {
      "value": "demouser"
    },
    "adminPassword": {
      "value": "demouser@123"
    },
    "vaultName": {
      "value": "contosovault"
    },
    "vaultResourceGroup": {
      "value": "contosovaultrg"
    },
    "secretUrlWithVersion": {
      "value": "https://testkv001.vault.local.azurestack.external/secrets/testcert002/82afeeb84f4442329ce06593502e7840"
    }
  }
}
```

## <a name="deploy-the-template"></a>Implementar o modelo

Implemente o modelo utilizando o seguinte script do PowerShell:

```powershell
# Deploy a Resource Manager template to create a VM and push the secret onto it
New-AzureRmResourceGroupDeployment `
  -Name KVDeployment `
  -ResourceGroupName $resourceGroup `
  -TemplateFile "<Fully qualified path to the azuredeploy.json file>" `
  -TemplateParameterFile "<Fully qualified path to the azuredeploy.parameters.json file>"
```

Quando o modelo é implementado com êxito, o que resulta no seguinte resultado:

![Resultados de implementação do modelo](media/azure-stack-kv-push-secret-into-vm/deployment-output.png)

Pilha do Azure envia o certificado para a máquina virtual durante a implementação. Localização do certificado depende do sistema de operativo a VM:

* No Windows, o certificado foi adicionado para a localização do certificado LocalMachine, com o arquivo de certificados que o utilizador forneceu.
* No Linux, o certificado é colocado sob o diretório de /var/lib/waagent, com o nome de ficheiro &lt;UppercaseThumbprint&gt;. crt para o X509 ficheiro de certificado e &lt;UppercaseThumbprint&gt;.prv para a chave privada .

## <a name="retire-certificates"></a>Extinguir os certificados

Extinguir os certificados faz parte do processo de gestão de certificados. Não é possível eliminar a versão mais antiga de um certificado, mas pode desativá-lo utilizando o `Set-AzureKeyVaultSecretAttribute` cmdlet.

O exemplo seguinte mostra como desativar um certificados. Utilizar os seus próprios valores para o **VaultName**, **nome**, e **versão** parâmetros.

```powershell
Set-AzureKeyVaultSecretAttribute -VaultName contosovault -Name servicecert -Version e3391a126b65414f93f6f9806743a1f7 -Enable 0
```

## <a name="next-steps"></a>Passos Seguintes

* [Implementar uma VM com uma palavra-passe do Cofre de Chaves](azure-stack-kv-deploy-vm-with-secret.md)
* [Permitir que uma aplicação aceder ao Cofre de chaves](azure-stack-kv-sample-app.md)
