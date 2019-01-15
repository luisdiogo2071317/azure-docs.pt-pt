---
title: Implementar uma VM do Azure de um usuário VHD | Documentos da Microsoft
description: Explica como implementar uma imagem VHD de utilizador para criar uma instância de VM do Azure.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 11/29/2018
ms.author: pbutlerm
ms.openlocfilehash: 48be60a7ba5770f8c329cb6323a5caa8fcf7f961
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/14/2019
ms.locfileid: "54265061"
---
# <a name="deploy-an-azure-vm-from-a-user-vhd"></a>Implementar uma VM do Azure a partir de um VHD do utilizador

Este artigo explica como implementar uma imagem generalizada do VHD para criar um novo recurso de VM do Azure, utilizando o modelo do Azure Resource Manager fornecidos e o script do Azure PowerShell.


## <a name="vhd-deployment-template"></a>Modelo de implementação de VHD

Copiar o modelo Azure Resource Manager para [implementação de VHD](cpp-deploy-json-template.md) num ficheiro local com o nome `VHDtoImage.json`.  Edite este ficheiro para fornecer valores para os seguintes parâmetros. 

|  **Parâmetro**             |   **Descrição**                                                              |
|  -------------             |   ---------------                                                              |
| ResourceGroupName          | Nome do grupo de recursos do Azure existente.  Normalmente, utiliza o mesmo RG associado com o seu Cofre de chaves  |
| TemplateFile               | Nome do caminho completo para o ficheiro `VHDtoImage.json`                                    |
| userStorageAccountName     | Nome da conta de armazenamento                                                    |
| sNameForPublicIP           | Nome DNS para o IP público. Tem de estar em minúsculas                                  |
| subscriptionId             | Identificador de subscrição do Azure                                                  |
| Localização                   | Localização geográfica do Azure padrão do grupo de recursos                       |
| vmName                     | Nome da máquina virtual                                                    |
| vaultName                  | Nome do Cofre de chaves                                                          |
| vaultResourceGroup         | Grupo de recursos do Cofre de chaves
| certificateUrl             | URL do certificado, incluindo a versão armazenada no Cofre de chaves, por exemplo:  `https://testault.vault.azure.net/secrets/testcert/b621es1db241e56a72d037479xab1r7` |
| vhdUrl                     | URL do disco rígido virtual                                                   |
| vmSize                     | Tamanho da instância de máquina virtual                                           |
| publicIPAddressName        | Nome do endereço IP público                                                  |
| virtualNetworkName         | Nome da rede virtual                                                    |
| nicName                    | Nome do cartão de interface de rede para a rede virtual                     |
| adminUserName              | Nome de utilizador da conta de administrador                                          |
| adminPassword              | Palavra-passe do administrador                                                          |
|  |  |


## <a name="powershell-script"></a>Script do PowerShell

Copie e edite o seguinte script para fornecer valores para o `$storageaccount` e `$vhdUrl` variáveis.  Executá-lo para criar um recurso de VM do Azure a partir de seu VHD generalizado existente.

```powershell
# storage account of existing generalized VHD 
$storageaccount = "testwinrm11815"

# generalized VHD URL
$vhdUrl = "https://testwinrm11815.blob.core.windows.net/vhds/testvm1234562016651857.vhd"

echo "New-AzureRMResourceGroupDeployment -Name "dplisvvm$postfix" -ResourceGroupName "$rgName" -TemplateFile "C:\certLocation\VHDtoImage.json" -userStorageAccountName "$storageaccount" -dnsNameForPublicIP "$vmName" -subscriptionId "$mysubid" -location "$location" -vmName "$vmName" -vaultName "$kvname" -vaultResourceGroup "$rgName" -certificateUrl $objAzureKeyVaultSecret.Id  -vhdUrl "$vhdUrl" -vmSize "Standard_A2" -publicIPAddressName "myPublicIP1" -virtualNetworkName "myVNET1" -nicName "myNIC1" -adminUserName "isv" -adminPassword $pwd"

#deploying VM with existing VHD
New-AzureRMResourceGroupDeployment -Name "dplisvvm$postfix" -ResourceGroupName "$rgName" -TemplateFile "C:\certLocation\VHDtoImage.json" -userStorageAccountName "$storageaccount" -dnsNameForPublicIP "$vmName" -subscriptionId "$mysubid" -location "$location" -vmName "$vmName" -vaultName "$kvname" -vaultResourceGroup "$rgName" -certificateUrl $objAzureKeyVaultSecret.Id  -vhdUrl "$vhdUrl" -vmSize "Standard_A2" -publicIPAddressName "myPublicIP1" -virtualNetworkName "myVNET1" -nicName "myNIC1" -adminUserName "isv" -adminPassword $pwd 

```

## <a name="next-steps"></a>Passos Seguintes

Depois da VM é implementada, está pronto para [certificar a sua imagem VM](./cpp-certify-vm.md).
