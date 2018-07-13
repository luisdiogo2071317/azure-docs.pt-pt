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
ms.date: 07/09/2018
ms.author: tomfitz
ms.openlocfilehash: 3a29319a0d478537dfc4905ee77865b8fea64587
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38598412"
---
# <a name="use-azure-key-vault-to-pass-secure-parameter-value-during-deployment"></a>Utilizar o Azure Key Vault para transmitir o valor do parâmetro segura durante a implementação

Quando precisar passar um valor seguro (como uma palavra-passe) como um parâmetro durante a implementação, pode recuperar o valor de uma [do Azure Key Vault](../key-vault/key-vault-whatis.md). Obter o valor referenciando o Cofre de chaves e o segredo no seu ficheiro de parâmetros. O valor nunca está exposto porque só fazem referência a sua ID de Cofre de chaves. O Cofre de chaves pode existir numa subscrição diferente que o grupo de recursos que está a implementar.

## <a name="enable-access-to-the-secret"></a>Ativar o acesso para o segredo

Existem duas condições importantes que tem de existir para aceder a um cofre de chaves durante a implementação do modelo:

1. A propriedade de Cofre de chaves `enabledForTemplateDeployment` tem de ser `true`.
2. O utilizador a implementar o modelo tem de ter acesso para o segredo. O utilizador tem de ter o `Microsoft.KeyVault/vaults/deploy/action` permissão para o Cofre de chaves. O [proprietário](../role-based-access-control/built-in-roles.md#owner) e [contribuinte](../role-based-access-control/built-in-roles.md#contributor) ambas as funções de concedem o acesso.

Ao utilizar um cofre de chaves com o modelo para um [Managed Application](../managed-applications/overview.md), tem de conceder acesso para o **fornecedor de recursos da aplicação** principal de serviço. Para obter mais informações, consulte [segredo do Cofre de chaves de acesso ao implementar aplicações geridas do Azure](../managed-applications/key-vault-access.md).


## <a name="deploy-a-key-vault-and-secret"></a>Implementar um cofre de chaves e um segredo

Para criar um cofre de chaves e o segredo, utilize o CLI do Azure ou o PowerShell. Tenha em atenção que o Cofre de chaves está ativado para implementação do modelo. 

Para a CLI do Azure, utilize:

```azurecli-interactive
vaultname={your-unique-vault-name}
password={password-value}

az group create --name examplegroup --location 'South Central US'
az keyvault create \
  --name $vaultname \
  --resource-group examplegroup \
  --location 'South Central US' \
  --enabled-for-template-deployment true
az keyvault secret set --vault-name $vaultname --name examplesecret --value $password
```

Para o PowerShell, utilize:

```powershell
$vaultname = "{your-unique-vault-name}"
$password = "{password-value}"

New-AzureRmResourceGroup -Name examplegroup -Location "South Central US"
New-AzureRmKeyVault `
  -VaultName $vaultname `
  -ResourceGroupName examplegroup `
  -Location "South Central US" `
  -EnabledForTemplateDeployment
$secretvalue = ConvertTo-SecureString $password -AsPlainText -Force
Set-AzureKeyVaultSecret -VaultName $vaultname -Name "examplesecret" -SecretValue $secretvalue
```

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
az group create --name datagroup --location "South Central US"
az group deployment create \
    --name exampledeployment \
    --resource-group datagroup \
    --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/keyvaultparameter/sqlserver.json \
    --parameters @sqlserver.parameters.json
```

Para o PowerShell, utilize:

```powershell
New-AzureRmResourceGroup -Name datagroup -Location "South Central US"
New-AzureRmResourceGroupDeployment `
  -Name exampledeployment `
  -ResourceGroupName datagroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/keyvaultparameter/sqlserver.json `
  -TemplateParameterFile sqlserver.parameters.json
```

## <a name="reference-a-secret-with-dynamic-id"></a>Um segredo com o ID de dinâmica de referência

A secção anterior mostrou como passar uma ID de recurso estático para o segredo do Cofre de chaves. No entanto, em alguns cenários, precisa fazer referência um segredo do Cofre de chaves que varia com base na implementação atual. Nesse caso, não pode codificar o ID de recurso no ficheiro de parâmetros. Infelizmente, dinamicamente não é possível gerar o ID de recurso no ficheiro de parâmetros porque não são permitidas expressões de modelo no ficheiro de parâmetros.

Para gerar dinamicamente o ID de recurso para um segredo do Cofre de chaves, tem de mover o recurso que tem o segredo para um modelo ligado. No modelo principal, adicione o modelo ligado e passar um parâmetro que contém o ID de recurso gerado dinamicamente. A imagem seguinte mostra como um parâmetro no modelo ligado referencia o segredo.

![ID dinâmico](./media/resource-manager-keyvault-parameter/dynamickeyvault.png)

O modelo ligado tem de estar disponível por meio de um URI externo. Normalmente, adicione o seu modelo para uma conta de armazenamento e acessá-los em URI, como `https://<storage-name>.blob.core.windows.net/templatecontainer/sqlserver.json`.

O [seguindo o modelo](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/keyvaultparameter/sqlserver-dynamic-id.json) dinamicamente cria o ID de Cofre de chaves e passa-o como um parâmetro. Ele contém ligações para uma [modelo de exemplo](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/keyvaultparameter/sqlserver.json) no GitHub.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "vaultName": {
        "type": "string"
      },
      "vaultResourceGroup": {
        "type": "string"
      },
      "secretName": {
        "type": "string"
      },
      "adminLogin": {
        "type": "string"
      },
      "sqlServerName": {
        "type": "string"
      }
    },
    "resources": [
    {
      "apiVersion": "2015-01-01",
      "name": "nestedTemplate",
      "type": "Microsoft.Resources/deployments",
      "properties": {
        "mode": "incremental",
        "templateLink": {
          "uri": "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/keyvaultparameter/sqlserver.json",
          "contentVersion": "1.0.0.0"
        },
        "parameters": {
          "adminPassword": {
            "reference": {
              "keyVault": {
                "id": "[resourceId(subscription().subscriptionId,  parameters('vaultResourceGroup'), 'Microsoft.KeyVault/vaults', parameters('vaultName'))]"
              },
              "secretName": "[parameters('secretName')]"
            }
          },
          "adminLogin": { "value": "[parameters('adminLogin')]" },
          "sqlServerName": {"value": "[parameters('sqlServerName')]"}
        }
      }
    }],
    "outputs": {}
}
```

Implementar o modelo anterior e forneça valores para os parâmetros. Pode utilizar o modelo de exemplo do GitHub, mas tem de fornecer valores de parâmetro para o seu ambiente.

Para a CLI do Azure, utilize:

```azurecli-interactive
az group create --name datagroup --location "South Central US"
az group deployment create \
    --name exampledeployment \
    --resource-group datagroup \
    --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/keyvaultparameter/sqlserver-dynamic-id.json \
    --parameters vaultName=<your-vault> vaultResourceGroup=examplegroup secretName=examplesecret adminLogin=exampleadmin sqlServerName=<server-name>
```

Para o PowerShell, utilize:

```powershell
New-AzureRmResourceGroup -Name datagroup -Location "South Central US"
New-AzureRmResourceGroupDeployment `
  -Name exampledeployment `
  -ResourceGroupName datagroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/keyvaultparameter/sqlserver-dynamic-id.json `
  -vaultName <your-vault> -vaultResourceGroup examplegroup -secretName examplesecret -adminLogin exampleadmin -sqlServerName <server-name>
```

## <a name="next-steps"></a>Passos Seguintes
* Para obter informações gerais sobre cofres de chaves, consulte [introdução ao Azure Key Vault](../key-vault/key-vault-get-started.md).
* Para obter exemplos completos de segredos da chave de referência, consulte [exemplos de Key Vault](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples).
