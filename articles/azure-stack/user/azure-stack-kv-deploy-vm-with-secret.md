---
title: Implementar uma VM com palavra-passe armazenada em segurança na pilha do Azure | Microsoft Docs
description: Saiba como implementar uma VM com uma palavra-passe armazenada no Cofre de chaves de pilha do Azure
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 23322a49-fb7e-4dc2-8d0e-43de8cd41f80
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/07/2018
ms.author: mabrigg
ms.openlocfilehash: 4239eb31afd4abc8b3555f0ee353f5d96716d623
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/11/2018
---
# <a name="create-a-virtual-machine-using-a-secure-password-stored-in-azure-stack-key-vault"></a>Criar uma máquina virtual utilizando uma palavra-passe segura armazenada no Cofre de chaves de pilha do Azure

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

Passos neste artigo, através de implementar uma máquina virtual de Windows Server utilizando uma palavra-passe armazenada no Cofre de chaves de pilha do Azure. A utilização de uma palavra-passe do Cofre de chaves é mais segura do que a transmissão de uma palavra-passe de texto simples.

## <a name="overview"></a>Descrição geral

Pode armazenar valores como uma palavra-passe como um segredo no Cofre de chaves pilha do Azure. Depois de criar um segredo, pode referenciá-lo em modelos do Azure Resource Manager. A utilização de segredos com o Resource Manager fornece as seguintes vantagens:

* Não tem de introduzir manualmente segredo sempre que implementar um recurso.
* Pode especificar quais os utilizadores ou principais de serviço podem aceder a um segredo.

## <a name="prerequisites"></a>Pré-requisitos

* Tem de subscrever uma oferta, que inclui o serviço Cofre de chaves.
* [Instale o PowerShell para a pilha do Azure.](azure-stack-powershell-install.md)
* [Configure o Azure pilha ambiente do utilizador do PowerShell.](azure-stack-powershell-configure-user.md)

Os passos seguintes descrevem o processo necessário para criar uma máquina virtual ao obter a palavra-passe armazenada no Cofre de chaves:

1. Crie um cofre de chave secreta.
2. Atualize o ficheiro azuredeploy.parameters.json.
3. Implemente o modelo.

>[NOTA] Pode utilizar estes passos o Kit de desenvolvimento de pilha do Azure ou a partir de um cliente externo se estiver ligado através de VPN.

## <a name="create-a-key-vault-secret"></a>Criar um cofre de chave secreta

O script seguinte cria um cofre de chaves e armazena uma palavra-passe no Cofre de chaves como um segredo. Utilize o `-EnabledForDeployment` parâmetro ao criar o Cofre de chaves. Este parâmetro certifica-se de que o Cofre de chaves pode ser referenciado a partir de modelos Azure Resource Manager.

```powershell

$vaultName = "contosovault"
$resourceGroup = "contosovaultrg"
$location = "local"
$secretName = "MySecret"

New-AzureRmResourceGroup `
  -Name $resourceGroup `
  -Location $location

New-AzureRmKeyVault `
  -VaultName $vaultName `
  -ResourceGroupName $resourceGroup `
  -Location $location
  -EnabledForTemplateDeployment

$secretValue = ConvertTo-SecureString -String '<Password for your virtual machine>' -AsPlainText -Force

Set-AzureKeyVaultSecret `
  -VaultName $vaultName `
  -Name $secretName `
  -SecretValue $secretValue

```

Quando executar o script anterior, o resultado inclui o URI do segredo. Anote este URI. Tem de referenciá-lo no [máquina virtual de implementar o Windows com palavras-passe no modelo do Cofre de chaves](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-vm-windows-create-passwordfromkv). Transferir o [101-vm--palavra-passe segura](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-vm-windows-create-passwordfromkv) pasta no seu computador de desenvolvimento. Esta pasta contém os `azuredeploy.json` e `azuredeploy.parameters.json` ficheiros, que irá precisar nos passos.

Modificar o `azuredeploy.parameters.json` ficheiros, de acordo com os valores de ambiente. Os parâmetros do especial interesse são o nome do cofre, o grupo de recursos do cofre e o segredo do URI (como gerado pelo script anterior). O ficheiro seguinte está um exemplo de um ficheiro de parâmetros:

## <a name="update-the-azuredeployparametersjson-file"></a>Atualizar o ficheiro azuredeploy.parameters.json

Atualize o ficheiro de azuredeploy.parameters.json com o URI de KeyVault, secretName, adminUsername dos valores da máquina virtual, de acordo com o seu ambiente. O ficheiro JSON seguinte mostra um exemplo do ficheiro de parâmetros de modelo:

```json
{
    "$schema":  "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion":  "1.0.0.0",
    "parameters":  {
       "adminUsername":  {
         "value":  "demouser"
          },
         "adminPassword":  {
           "reference":  {
              "keyVault":  {
                "id":  "/subscriptions/xxxxxx/resourceGroups/RgKvPwd/providers/Microsoft.KeyVault/vaults/KvPwd"
                },
              "secretName":  "MySecret"
           }
         },
       "dnsLabelPrefix":  {
          "value":  "mydns123456"
        },
        "windowsOSVersion":  {
          "value":  "2016-Datacenter"
        }
    }
}

```

## <a name="template-deployment"></a>Implementação de modelos

Agora, implemente o modelo utilizando o seguinte script do PowerShell:

```powershell
New-AzureRmResourceGroupDeployment `
  -Name KVPwdDeployment `
  -ResourceGroupName $resourceGroup `
  -TemplateFile "<Fully qualified path to the azuredeploy.json file>" `
  -TemplateParameterFile "<Fully qualified path to the azuredeploy.parameters.json file>"
```

Quando o modelo é implementado com êxito, o que resulta no seguinte resultado:

![Saída de implementação](media/azure-stack-kv-deploy-vm-with-secret/deployment-output.png)

## <a name="next-steps"></a>Passos Seguintes

[Implementar uma aplicação de exemplo com o Cofre de chaves](azure-stack-kv-sample-app.md)

[Implementar uma VM com um certificado do Cofre de chaves](azure-stack-kv-push-secret-into-vm.md)
