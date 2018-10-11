---
title: Segredo do Key Vault com o modelo Azure Resource Manager | Documentos da Microsoft
description: Mostra como transmitir um segredo a partir de um cofre de chaves como um parâmetro durante a implementação.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/10/2018
ms.author: tomfitz
ms.openlocfilehash: 06719f3a92dae805081ea85c346df97ebed0e0dc
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/10/2018
ms.locfileid: "49078075"
---
# <a name="use-azure-key-vault-to-pass-secure-parameter-value-during-deployment"></a>Utilizar o Azure Key Vault para transmitir o valor do parâmetro segura durante a implementação

Quando precisar passar um valor seguro (como uma palavra-passe) como um parâmetro durante a implementação, pode recuperar o valor de uma [do Azure Key Vault](../key-vault/key-vault-whatis.md). Obter o valor referenciando o Cofre de chaves e o segredo no seu ficheiro de parâmetros. O valor nunca está exposto porque só fazem referência a sua ID de Cofre de chaves. O Cofre de chaves pode existir numa subscrição diferente que o grupo de recursos que está a implementar.

## <a name="deploy-a-key-vault-and-secret"></a>Implementar um cofre de chaves e um segredo

Para criar um cofre de chaves e o segredo, utilize o CLI do Azure ou o PowerShell. `enabledForTemplateDeployment` é uma propriedade de Cofre de chaves. Para acessar os segredos dentro deste cofre de chaves de implementação do Resource Manager, `enabledForTemplateDeployment` tem de ser `true`. 

O script de PowerShell do Azure e CLI do Azure de exemplo seguinte demonstra como criar um cofre de chaves e um segredo.

Para a CLI do Azure, utilize:

```azurecli-interactive
keyVaultName='{your-unique-vault-name}'
resourceGroupName='{your-resource-group-name}'
location='centralus'
userPrincipalName='{your-email-address-associated-with-your-subscription}'

# Create a resource group
az group create --name $resourceGroupName --location $location

# Create a Key Vault
az keyvault create \
  --name $keyVaultName \
  --resource-group $resourceGroupName \
  --location $location \
  --enabled-for-template-deployment true
az keyvault set-policy --upn $userPrincipalName --name $keyVaultName --secret-permissions set delete get list

# Create a secret with the name, vmAdminPassword
password=$(openssl rand -base64 32)
echo $password
az keyvault secret set --vault-name $keyVaultName --name 'vmAdminPassword' --value $password
```

Para o PowerShell, utilize:

```azurepowershell-interactive
$keyVaultName = "{your-unique-vault-name}"
$resourceGroupName="{your-resource-group-name}"
$location='Central US'
$userPrincipalName='{your-email-address-associated-with-your-subscription}'

New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

New-AzureRmKeyVault `
  -VaultName $keyVaultName `
  -resourceGroupName $resourceGroupName `
  -Location $location `
  -EnabledForTemplateDeployment
Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -UserPrincipalName $userPrincipalName -PermissionsToSecrets set,delete,get,list

$password = openssl rand -base64 32
echo $password
$secretvalue = ConvertTo-SecureString $password -AsPlainText -Force
Set-AzureKeyVaultSecret -VaultName $keyVaultName -Name "vmAdminPassword" -SecretValue $secretvalue
```

Se executar o script do PowerShell fora do Cloud Shell, utilize o seguinte comando para gerar palavra-passe em vez disso:

```powershell
Add-Type -AssemblyName System.Web
[System.Web.Security.Membership]::GeneratePassword(16,3)
```

Para utilizar o modelo do Resource Manager: veja [Tutorial: integrar o Azure Key Vault na implementação de modelo do Resource Manager](./resource-manager-tutorial-use-key-vault.md#prepare-the-key-vault).

> [!NOTE]
> Cada serviço do Azure tem requisitos de palavra-passe específica. Por exemplo, os requisitos da máquina virtual do Azure podem ser encontrados em [quais são os requisitos de palavra-passe ao criar uma VM?](../virtual-machines/windows/faq.md#what-are-the-password-requirements-when-creating-a-vm).

## <a name="enable-access-to-the-secret"></a>Ativar o acesso para o segredo

Diferente de definição `enabledForTemplateDeployment` para `true`, o utilizador a implementar o modelo tem de ter o `Microsoft.KeyVault/vaults/deploy/action` permissão de âmbito que contém o Cofre de chaves, incluindo o grupo de recursos e Cofre de chaves. O [proprietário](../role-based-access-control/built-in-roles.md#owner) e [contribuinte](../role-based-access-control/built-in-roles.md#contributor) ambas as funções de concedem o acesso. Se criar o Cofre de chaves, é o proprietário para que tenha a permissão. Se o Cofre de chaves numa subscrição diferente, o proprietário do Cofre de chaves tem de ser grand o acesso.

O procedimento seguinte mostra como criar uma função com o mínimo permssion e como atribuir o utilizador
1. Crie um ficheiro JSON da definição de função personalizada:

    ```json
    {
      "Name": "Key Vault resource manager template deployment operator",
      "IsCustom": true,
      "Description": "Lets you deploy a resource manager template with the access to the secrets in the Key Vault.",
      "Actions": [
        "Microsoft.KeyVault/vaults/deploy/action"
      ],
      "NotActions": [],
      "DataActions": [],
      "NotDataActions": [],
      "AssignableScopes": [
        "/subscriptions/00000000-0000-0000-0000-000000000000"
      ]
    }
    ```
    Substitua "00000000-0000-0000-0000-000000000000" com o ID de subscrição do utilizador que precisa para implementar os modelos.

2. Crie a nova função com o ficheiro JSON:

    ```azurepowershell
    $resourceGroupName= "<Resource Group Name>" # the resource group which contains the Key Vault
    $userPrincipalName = "<Email Address of the deployment operator>"
    New-AzureRmRoleDefinition -InputFile "<PathToTheJSONFile>" 
    New-AzureRmRoleAssignment -ResourceGroupName $resourceGroupName -RoleDefinitionName "Key Vault resource manager template deployment operator" -SignInName $userPrincipalName
    ```

    O `New-AzureRmRoleAssignment` exemplo atribuir a função personalizada para o usuário no nível do grupo de recursos.  

Ao utilizar um cofre de chaves com o modelo para um [Managed Application](../managed-applications/overview.md), tem de conceder acesso para o **fornecedor de recursos da aplicação** principal de serviço. Para obter mais informações, consulte [segredo do Cofre de chaves de acesso ao implementar aplicações geridas do Azure](../managed-applications/key-vault-access.md).

## <a name="reference-a-secret-with-static-id"></a>Referenciar um segredo com o ID estático

O modelo que recebe um segredo do Cofre de chaves é como qualquer outro modelo. Isso ocorre porque **referenciar o Cofre de chaves no ficheiro de parâmetros, não o modelo.** A imagem seguinte mostra como o ficheiro de parâmetros referencia o segredo e passará esse valor para o modelo.

![ID estático](./media/resource-manager-keyvault-parameter/statickeyvault.png)

Por exemplo, o [seguindo o modelo](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/keyvaultparameter/sqlserver.json) implementa uma base de dados SQL, que inclui uma palavra-passe de administrador. O parâmetro de palavra-passe é definido como uma cadeia segura. No entanto, o modelo não especifica onde vêm esse valor.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "adminLogin": {
      "type": "string"
    },
    "adminPassword": {
      "type": "securestring"
    },
    "sqlServerName": {
      "type": "string"
    }
  },
  "resources": [
    {
      "name": "[parameters('sqlServerName')]",
      "type": "Microsoft.Sql/servers",
      "apiVersion": "2015-05-01-preview",
      "location": "[resourceGroup().location]",
      "tags": {},
      "properties": {
        "administratorLogin": "[parameters('adminLogin')]",
        "administratorLoginPassword": "[parameters('adminPassword')]",
        "version": "12.0"
      }
    }
  ],
  "outputs": {
  }
}
```

Agora, crie um ficheiro de parâmetros para o modelo anterior. No ficheiro de parâmetros, especifique um parâmetro que corresponda ao nome do parâmetro no modelo. Para o valor do parâmetro, referenciar o segredo do key vault. Referenciar o segredo, passando o identificador de recurso do Cofre de chaves e o nome do segredo do. Na [seguindo o ficheiro de parâmetros](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/keyvaultparameter/sqlserver.parameters.json), o segredo do Cofre de chaves tem de existir e fornecer um valor estático para o seu ID de recurso. Copie esse arquivo localmente e defina o ID de subscrição, o nome do cofre e o nome do SQL server.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "adminLogin": {
            "value": "exampleadmin"
        },
        "adminPassword": {
            "reference": {
              "keyVault": {
                "id": "/subscriptions/<subscription-id>/resourceGroups/examplegroup/providers/Microsoft.KeyVault/vaults/<vault-name>"
              },
              "secretName": "examplesecret"
            }
        },
        "sqlServerName": {
            "value": "<your-server-name>"
        }
    }
}
```

Se precisar de utilizar uma versão do segredo do que não seja a versão atual, utilize o `secretVersion` propriedade.

```json
"secretName": "examplesecret",
"secretVersion": "cd91b2b7e10e492ebb870a6ee0591b68"
```

Agora, implementar o modelo e passar o ficheiro de parâmetros. Pode utilizar o modelo de exemplo do GitHub, mas tem de utilizar um ficheiro de parâmetros local com os valores definidos para o seu ambiente.

Para a CLI do Azure, utilize:

```azurecli-interactive
az group create --name datagroup --location $location
az group deployment create \
    --name exampledeployment \
    --resource-group datagroup \
    --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/keyvaultparameter/sqlserver.json \
    --parameters @sqlserver.parameters.json
```

Para o PowerShell, utilize:

```powershell-interactive
New-AzureRmResourceGroup -Name datagroup -Location $location
New-AzureRmResourceGroupDeployment `
  -Name exampledeployment `
  -ResourceGroupName datagroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/keyvaultparameter/sqlserver.json `
  -TemplateParameterFile sqlserver.parameters.json
```

## <a name="reference-a-secret-with-dynamic-id"></a>Um segredo com o ID de dinâmica de referência

A secção anterior mostrou como passar uma ID de recurso estático para o segredo do Cofre de chaves do parâmetro. No entanto, em alguns cenários, precisa fazer referência um segredo do Cofre de chaves que varia com base na implementação atual. Em alternativa, pode querer simplesmente passar valores de parâmetros para o modelo em vez de criar um parâmetro de referência no ficheiro de parâmetros. Em ambos os casos, pode gerar dinamicamente o ID de recurso para um segredo do Cofre de chaves utilizando um modelo ligado.

Dinamicamente não é possível gerar o ID de recurso no ficheiro de parâmetros, uma vez que não são permitidas expressões de modelo no ficheiro de parâmetros. 

No modelo principal, adicione o modelo ligado e passar um parâmetro que contém o ID de recurso gerado dinamicamente. A imagem seguinte mostra como um parâmetro no modelo ligado referencia o segredo.

![ID dinâmico](./media/resource-manager-keyvault-parameter/dynamickeyvault.png)

O [seguindo o modelo](https://github.com/Azure/azure-quickstart-templates/tree/master/201-key-vault-use-dynamic-id) dinamicamente cria o ID de Cofre de chaves e passa-o como um parâmetro.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]",
            "metadata": {
                "description": "The location where the resources will be deployed."
            }
        },
        "vaultName": {
            "type": "string",
            "metadata": {
                "description": "The name of the keyvault that contains the secret."
            }
        },
        "secretName": {
            "type": "string",
            "metadata": {
                "description": "The name of the secret."
            }
        },
        "vaultResourceGroupName": {
            "type": "string",
            "metadata": {
                "description": "The name of the resource group that contains the keyvault."
            }
        },
        "vaultSubscription": {
            "type": "string",
            "defaultValue": "[subscription().subscriptionId]",
            "metadata": {
                "description": "The name of the subscription that contains the keyvault."
            }
        },
        "_artifactsLocation": {
            "type": "string",
            "metadata": {
                "description": "The base URI where artifacts required by this template are located. When the template is deployed using the accompanying scripts, a private location in the subscription will be used and this value will be automatically generated."
            },
            "defaultValue": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-key-vault-use-dynamic-id/"
        },
        "_artifactsLocationSasToken": {
            "type": "securestring",
            "metadata": {
                "description": "The sasToken required to access _artifactsLocation.  When the template is deployed using the accompanying scripts, a sasToken will be automatically generated."
            },
            "defaultValue": ""
        }
    },
    "resources": [
        {
            "apiVersion": "2018-05-01",
            "name": "dynamicSecret",
            "type": "Microsoft.Resources/deployments",
            "properties": {
                "mode": "Incremental",
                "templateLink": {
                    "contentVersion": "1.0.0.0",
                    "uri": "[uri(parameters('_artifactsLocation'), concat('./nested/sqlserver.json', parameters('_artifactsLocationSasToken')))]"
                },
                "parameters": {
                    "location": {
                        "value": "[parameters('location')]"
                    },
                    "adminLogin": {
                        "value": "ghuser"
                    },
                    "adminPassword": {
                        "reference": {
                            "keyVault": {
                                "id": "[resourceId(parameters('vaultSubscription'), parameters('vaultResourceGroupName'), 'Microsoft.KeyVault/vaults', parameters('vaultName'))]"
                            },
                            "secretName": "[parameters('secretName')]"
                        }
                    }
                }
            }
        }
    ],
    "outputs": {
        "sqlFQDN": {
            "type": "string",
            "value": "[reference('dynamicSecret').outputs.sqlFQDN.value]"
        }
    }
}
```

Implementar o modelo anterior e forneça valores para os parâmetros. Pode utilizar o modelo de exemplo do GitHub, mas tem de fornecer valores de parâmetro para o seu ambiente.

Para a CLI do Azure, utilize:

```azurecli-interactive
az group create --name datagroup --location $location
az group deployment create \
    --name exampledeployment \
    --resource-group datagroup \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-key-vault-use-dynamic-id/azuredeploy.json \
    --parameters vaultName=<your-vault> vaultResourceGroupName=examplegroup secretName=examplesecret
```

Para o PowerShell, utilize:

```powershell
New-AzureRmResourceGroup -Name datagroup -Location $location
New-AzureRmResourceGroupDeployment `
  -Name exampledeployment `
  -ResourceGroupName datagroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-key-vault-use-dynamic-id/azuredeploy.json `
  -vaultName <your-vault> -vaultResourceGroupName examplegroup -secretName examplesecret
```

## <a name="next-steps"></a>Passos Seguintes
* Para obter informações gerais sobre cofres de chaves, consulte [introdução ao Azure Key Vault](../key-vault/key-vault-get-started.md).
* Para obter exemplos completos de segredos da chave de referência, consulte [exemplos de Key Vault](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples).
