---
title: Utilize Azure geridos identidade de serviço de API Management do Azure | Microsoft Docs
description: Saiba como utilizar uma identidade de serviço gerida do Azure na API Management
services: api-management
documentationcenter: ''
author: miaojiang
manager: anneta
editor: ''
ms.service: api-management
ms.workload: integration
ms.topic: article
ms.date: 10/18/2017
ms.author: apimpm
ms.openlocfilehash: 98aa70935a3efbbe2edb07aade85fa3ea17ce786
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
ms.locfileid: "32150435"
---
# <a name="use-azure-managed-service-identity-in-azure-api-management"></a>Utilizar identidade de serviço gerida do Azure na API Management do Azure

Este artigo mostra-lhe como criar uma identidade de serviço gerida para uma instância de serviço de API Management e como aceder a outros recursos. Uma identidade de serviço geridas gerada pelo Azure Active Directory (Azure AD) permite que a instância de API Management fácil e segura aceder a outros recursos do Azure AD protegida, tais como o Cofre de chaves do Azure. Esta identidade de serviço geridas é gerida pelo Azure e não necessita de aprovisionar ou rodar os segredos. Para obter mais informações sobre a identidade de serviço gerida do Azure, consulte [identidade de serviço geridas para recursos do Azure](../active-directory/msi-overview.md).

## <a name="create-a-managed-service-identity-for-an-api-management-instance"></a>Criar uma identidade de serviço gerida para uma instância de API Management

### <a name="using-the-azure-portal"></a>Utilizar o portal do Azure

Para configurar uma identidade de serviço geridas no portal, irá criar primeiro uma instância de API Management como normal e, em seguida, ativar a funcionalidade.

1. Crie uma instância de API Management no portal como faria normalmente. Navegue para o mesmo no portal.
2. Selecione **identidade do serviço gerido**.
3. Alternar o registo com o Azure Active Directory no. Clique em Guardar.

![Ativar MSI](./media/api-management-msi/enable-msi.png)

### <a name="using-the-azure-resource-manager-template"></a>Utilizando o modelo Azure Resource Manager

Pode criar uma instância de API Management com uma identidade, incluindo a seguinte propriedade na definição do recurso: 

```json
"identity" : {
    "type" : "SystemAssigned"
}
```

Isto indica do Azure para criar e gerir a identidade para a instância de API Management. 

Por exemplo, um modelo Azure Resource Manager completo aspeto que poderá ter o seguinte:

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "0.9.0.0"
    },
    "resources": [
        {
            "apiVersion": "2017-03-01",
            "name": "contoso",
            "type": "Microsoft.ApiManagement/service",
            "location": "[resourceGroup().location]",
            "tags": {},
            "sku": {
                "name": "Developer",
                "capacity": "1"
            },
            "properties": {
                "publisherEmail": "admin@contoso.com",
                "publisherName": "Contoso"
            },
            "identity": { 
                "type": "systemAssigned" 
            }
        }
    ]
}
```
## <a name="use-the-managed-service-identity-to-access-other-resources"></a>Utilizar a identidade de serviço gerida para aceder a outros recursos

> [!NOTE]
> Atualmente, a identidade de serviço geridas pode ser utilizada para obter certificados a partir do Cofre de chaves do Azure para nomes de domínio personalizado de API Management. Cenários mais serão suportados em breve.
> 
>


### <a name="obtain-a-certificate-from-azure-key-vault"></a>Obter um certificado a partir do Cofre de chaves do Azure

#### <a name="prerequisites"></a>Pré-requisitos
1. O Cofre de chaves que contém o certificado pfx tem de ser na mesma subscrição do Azure e o mesmo grupo de recursos que o serviço de API Management. Este é um requisito do modelo Azure Resource Manager. 
2. O tipo de conteúdo do segredo tem de ser *application/x-pkcs12*. Pode utilizar o script seguinte para carregar o certificado:

```powershell
$pfxFilePath = "PFX_CERTIFICATE_FILE_PATH" # Change this path 
$pwd = "PFX_CERTIFICATE_PASSWORD" # Change this password 
$flag = [System.Security.Cryptography.X509Certificates.X509KeyStorageFlags]::Exportable 
$collection = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2Collection 
$collection.Import($pfxFilePath, $pwd, $flag) 
$pkcs12ContentType = [System.Security.Cryptography.X509Certificates.X509ContentType]::Pkcs12 
$clearBytes = $collection.Export($pkcs12ContentType) 
$fileContentEncoded = [System.Convert]::ToBase64String($clearBytes) 
$secret = ConvertTo-SecureString -String $fileContentEncoded -AsPlainText –Force 
$secretContentType = 'application/x-pkcs12' 
Set-AzureKeyVaultSecret -VaultName KEY_VAULT_NAME -Name KEY_VAULT_SECRET_NAME -SecretValue $Secret -ContentType $secretContentType
```

> [!Important]
> Se a versão de objeto do certificado não for fornecida, gestão de API irá obter automaticamente a versão mais recente do certificado após ser carregada para o Cofre de chaves. 

O exemplo seguinte mostra um modelo Azure Resource Manager que contém os seguintes passos:

1. Crie uma instância de API Management com uma identidade de serviço geridas.
2. Atualizar as políticas de acesso de uma instância do Cofre de chaves do Azure e permitir que a instância da API de gestão a obter segredos do mesmo.
3. Atualize a instância de API Management, definindo um nome de domínio personalizado através de um certificado na instância do Cofre de chaves.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "publisherEmail": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "The email address of the owner of the service"
            }
        },
        "publisherName": {
            "type": "string",
            "defaultValue": "Contoso",
            "minLength": 1,
            "metadata": {
                "description": "The name of the owner of the service"
            }
        },
        "sku": {
            "type": "string",
            "allowedValues": ["Developer",
            "Standard",
            "Premium"],
            "defaultValue": "Developer",
            "metadata": {
                "description": "The pricing tier of this API Management service"
            }
        },
        "skuCount": {
            "type": "int",
            "defaultValue": 1,
            "metadata": {
                "description": "The instance size of this API Management service."
            }
        },
        "keyVaultName": {
            "type": "string",
            "metadata": {
                "description": "Name of the vault"
            }
        },
        "proxyCustomHostname1": {
            "type": "string",
            "metadata": {
                "description": "Proxy Custom hostname."
            }
        },
        "keyVaultIdToCertificate": {
            "type": "string",
            "metadata": {
                "description": "Reference to the KeyVault certificate."
            }
        }
    },
    "variables": {
        "apiManagementServiceName": "[concat('apiservice', uniqueString(resourceGroup().id))]",
        "apimServiceIdentityResourceId": "[concat(resourceId('Microsoft.ApiManagement/service', variables('apiManagementServiceName')),'/providers/Microsoft.ManagedIdentity/Identities/default')]"
    },
    "resources": [{
        "apiVersion": "2017-03-01",
        "name": "[variables('apiManagementServiceName')]",
        "type": "Microsoft.ApiManagement/service",
        "location": "[resourceGroup().location]",
        "tags": {
            
        },
        "sku": {
            "name": "[parameters('sku')]",
            "capacity": "[parameters('skuCount')]"
        },
        "properties": {
            "publisherEmail": "[parameters('publisherEmail')]",
            "publisherName": "[parameters('publisherName')]"
        },
        "identity": {
            "type": "systemAssigned"
        }
    },
    {
        "type": "Microsoft.KeyVault/vaults/accessPolicies",
        "name": "[concat(parameters('keyVaultName'), '/add')]",
        "apiVersion": "2015-06-01",        
      "dependsOn": [
        "[resourceId('Microsoft.ApiManagement/service', variables('apiManagementServiceName'))]"
      ],
        "properties": {
            "accessPolicies": [{
                "tenantId": "[reference(variables('apimServiceIdentityResourceId'), '2015-08-31-PREVIEW').tenantId]",
                "objectId": "[reference(variables('apimServiceIdentityResourceId'), '2015-08-31-PREVIEW').principalId]",
                "permissions": {
                    "secrets": ["get"]
                }
            }]
        }
    },
    { 
      "apiVersion": "2017-05-10", 
      "name": "apimWithKeyVault", 
      "type": "Microsoft.Resources/deployments",
      "dependsOn": [
        "[resourceId('Microsoft.ApiManagement/service', variables('apiManagementServiceName'))]"
      ],
      "properties": { 
        "mode": "incremental", 
        "templateLink": {
          "uri": "https://raw.githubusercontent.com/solankisamir/arm-templates/master/basicapim.keyvault.json",
          "contentVersion": "1.0.0.0"
        }, 
        "parameters": {
            "publisherEmail": { "value": "[parameters('publisherEmail')]"},
            "publisherName": { "value": "[parameters('publisherName')]"},
            "sku": { "value": "[parameters('sku')]"},
            "skuCount": { "value": "[parameters('skuCount')]"},
            "proxyCustomHostname1": {"value" : "[parameters('proxyCustomHostname1')]"},
            "keyVaultIdToCertificate": {"value" : "[parameters('keyVaultIdToCertificate')]"}
        }
      } 
    }]
}
```

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre a identidade de serviço gerida do Azure:

* [Identidade de serviço geridas para recursos do Azure](../active-directory/msi-overview.md)
* [Modelos Azure Resource Manager](https://github.com/Azure/azure-quickstart-templates)

