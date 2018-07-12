---
title: Implementar recursos com o PowerShell e o modelo | Documentos da Microsoft
description: Utilize o Azure Resource Manager e o Azure PowerShell para implementar um recursos no Azure. Os recursos são definidos num modelo do Resource Manager.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 55903f35-6c16-4c6d-bf52-dbf365605c3f
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/06/2017
ms.author: tomfitz
ms.openlocfilehash: 714f5f2d4aa1635da8aa62139be5c80e7761ae27
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38486745"
---
# <a name="deploy-resources-with-resource-manager-templates-and-azure-powershell"></a>Implementar recursos com modelos do Resource Manager e do Azure PowerShell

Este artigo explica como utilizar o Azure PowerShell com modelos do Resource Manager para implementar os seus recursos no Azure. Se não estiver familiarizado com os conceitos de implementar e gerir as suas soluções do Azure, veja [descrição geral do Azure Resource Manager](resource-group-overview.md).

O modelo do Resource Manager, implementar podem ser um ficheiro local no seu computador ou um arquivo externo que está localizado num repositório, como o GitHub. O modelo implementa neste artigo está disponível na [modelo de exemplo](#sample-template) secção, ou como [modelo de conta de armazenamento no GitHub](https://github.com/Azure/azure-quickstart-templates/blob/master/101-storage-account-create/azuredeploy.json).

Caso seja necessário, instale o módulo Azure PowerShell com as instruções no [guia do Azure PowerShell](/powershell/azure/overview) e, em seguida, execute `Connect-AzureRmAccount` para criar uma ligação ao Azure.

<a id="deploy-local-template" />

## <a name="deploy-a-template-from-your-local-machine"></a>Implementar um modelo a partir do seu computador local

Quando implementar recursos no Azure,:

1. Iniciar sessão na sua conta do Azure
2. Crie um grupo de recursos que funciona como contêiner para os recursos implementados. O nome do grupo de recursos só pode incluir carateres alfanuméricos, pontos finais, carateres de sublinhado, hífenes e parênteses. Pode ser até 90 carateres. Não pode terminar com um período.
3. Implementar o modelo que define os recursos para criar o grupo de recursos

Um modelo pode incluir parâmetros que lhe permitem personalizar a implementação. Por exemplo, pode fornecer valores que são adaptadas para um ambiente específico (por exemplo, desenvolvimento, teste e produção). O modelo de exemplo define um parâmetro para o SKU da conta de armazenamento.

O exemplo seguinte cria um grupo de recursos e implementa um modelo a partir do seu computador local:

```powershell
Connect-AzureRmAccount

Select-AzureRmSubscription -SubscriptionName <yourSubscriptionName>
 
New-AzureRmResourceGroup -Name ExampleResourceGroup -Location "South Central US"
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateFile c:\MyTemplates\storage.json -storageAccountType Standard_GRS
```

A implementação pode demorar alguns minutos a concluir. Quando terminar, verá uma mensagem que inclui o resultado:

```powershell
ProvisioningState       : Succeeded
```

## <a name="deploy-a-template-from-an-external-source"></a>Implementar um modelo a partir de uma origem externa

Em vez de armazenar modelos do Resource Manager no seu computador local, pode armazená-las num local externo. Pode armazenar modelos num repositório de controle de origem (por exemplo, o GitHub). Em alternativa, pode armazená-los numa conta de armazenamento do Azure para acesso partilhado na sua organização.

Para implementar um modelo externo, utilize o **TemplateUri** parâmetro. Utilize o URI no exemplo para implementar o modelo de exemplo do GitHub.

```powershell
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json `
  -storageAccountType Standard_GRS
```

O exemplo anterior exige um URI publicamente acessível para o modelo, que funciona na maioria dos cenários, porque o modelo não deve incluir dados confidenciais. Se tiver de especificar os dados confidenciais (como uma palavra-passe de administrador), passe esse valor como um parâmetro seguro. No entanto, se não pretender que o modelo para estar acessível publicamente, pode protegê-los armazenando-os num contentor do armazenamento privado. Para obter informações sobre como implementar um modelo que precisa de um token de assinatura (SAS) de acesso partilhado, consulte [implementar modelo privado com o SAS token](resource-manager-powershell-sas-token.md).

[!INCLUDE [resource-manager-cloud-shell-deploy.md](../../includes/resource-manager-cloud-shell-deploy.md)]

No Cloud Shell, utilize os seguintes comandos:

```powershell
New-AzureRmResourceGroup -Name ExampleResourceGroup -Location "South Central US"
New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateUri <copied URL> `
  -storageAccountType Standard_GRS
```

## <a name="deploy-to-more-than-one-resource-group-or-subscription"></a>Implementar a mais do que um grupo de recursos ou subscrição

Normalmente, implementa todos os recursos no seu modelo para um grupo de recursos. No entanto, existem cenários onde pretende implementar um conjunto de recursos em conjunto, mas colocá-los em diferentes grupos de recursos ou subscrições. Pode implementar em apenas cinco grupos de recursos numa única implementação. Para obter mais informações, consulte [recursos do Azure implementar a mais de uma subscrição ou grupo de recursos](resource-manager-cross-resource-group-deployment.md).

<a id="parameter-file" />

## <a name="parameter-files"></a>Ficheiros de parâmetros

Em vez de passar parâmetros como valores de inline no seu script, talvez ache mais fácil de utilizar um ficheiro JSON que contém os valores de parâmetro. O ficheiro de parâmetros tem de estar no seguinte formato:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
     "storageAccountType": {
         "value": "Standard_GRS"
     }
  }
}
```

Observe que a secção de parâmetros inclui um nome de parâmetro que corresponda ao parâmetro definido no seu modelo (storageAccountType). O ficheiro de parâmetros contém um valor para o parâmetro. Este valor é automaticamente passado para o modelo durante a implementação. Pode criar vários ficheiros de parâmetros para diferentes cenários de implementação e, em seguida, passar o ficheiro de parâmetros adequada. 

Copie o exemplo anterior e guarde-o como um arquivo chamado `storage.parameters.json`.

Para passar um ficheiro de parâmetros local, utilize o **TemplateParameterFile** parâmetro:

```powershell
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateFile c:\MyTemplates\storage.json `
  -TemplateParameterFile c:\MyTemplates\storage.parameters.json
```

Para passar um ficheiro de parâmetros externo, utilize o **TemplateParameterUri** parâmetro:

```powershell
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json `
  -TemplateParameterUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.parameters.json
```

Pode usar parâmetros inline e um ficheiro de parâmetros local na mesma operação de implementação. Por exemplo, pode especificar alguns valores no ficheiro de parâmetros local e adicionar outros embutidos valores durante a implementação. Se fornecer valores para um parâmetro no ficheiro de parâmetros local e inline, o valor de inline tem precedência.

No entanto, quando utiliza um ficheiro de parâmetros externo, não poder passar os outros valores de qualquer inline ou a partir de um ficheiro local. Se especificar um ficheiro de parâmetros nos **TemplateParameterUri** parâmetro, inline todos os parâmetros são ignorados. Fornece todos os valores de parâmetros no ficheiro externo. Se o seu modelo inclui um valor confidencial que não pode incluir no ficheiro de parâmetros, adicione esse valor para um cofre de chaves ou fornecer dinamicamente tudo embutido de valores de parâmetro.

Se o seu modelo inclui um parâmetro com o mesmo nome que um dos parâmetros no comando do PowerShell, PowerShell apresenta o parâmetro a partir do seu modelo com o sufixo **FromTemplate**. Por exemplo, um parâmetro denominado **ResourceGroupName** em seu conflitos de modelo com o **ResourceGroupName** parâmetro no [New-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/new-azurermresourcegroupdeployment) cmdlet. Lhe for pedido para fornecer um valor para **ResourceGroupNameFromTemplate**. Em geral, deve evitar essa confusão ao não nomear os parâmetros com o mesmo nome como parâmetros utilizados para operações de implementação.

## <a name="test-a-template-deployment"></a>Testar uma implementação de modelo

Para testar seus valores de modelo e o parâmetro sem ter de implementar, na verdade, todos os recursos, utilize [Test-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/test-azurermresourcegroupdeployment). 

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateFile c:\MyTemplates\storage.json -storageAccountType Standard_GRS
```

Se não são detetados erros, o comando terminar sem uma resposta. Se for detetado um erro, o comando devolve uma mensagem de erro. Por exemplo, se tentar passar um valor incorreto para a conta de armazenamento SKU, devolve o erro seguinte:

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName testgroup `
  -TemplateFile c:\MyTemplates\storage.json -storageAccountType badSku

Code    : InvalidTemplate
Message : Deployment template validation failed: 'The provided value 'badSku' for the template parameter 'storageAccountType'
          at line '15' and column '24' is not valid. The parameter value is not part of the allowed value(s):
          'Standard_LRS,Standard_ZRS,Standard_GRS,Standard_RAGRS,Premium_LRS'.'.
Details :
```

Se o seu modelo tem um erro de sintaxe, o comando devolve um erro que indica que não foi possível analisar o modelo. A mensagem indica o número de linha e a posição de erro de análise.

```powershell
Test-AzureRmResourceGroupDeployment : After parsing a value an unexpected character was encountered: 
  ". Path 'variables', line 31, position 3.
```

[!INCLUDE [resource-manager-deployments](../../includes/resource-manager-deployments.md)]

Para utilizar o modo de conclusão, utilize o `Mode` parâmetro:

```powershell
New-AzureRmResourceGroupDeployment -Mode Complete -Name ExampleDeployment `
  -ResourceGroupName ExampleResourceGroup -TemplateFile c:\MyTemplates\storage.json 
```

## <a name="sample-template"></a>Modelo de exemplo

O modelo seguinte é utilizado para os exemplos neste artigo. Copie e guarde-o como um arquivo chamado storage.json. Para compreender como criar este modelo, veja [criar o primeiro modelo do Azure Resource Manager](resource-manager-create-first-template.md).  

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageAccountType": {
      "type": "string",
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS",
        "Standard_ZRS",
        "Premium_LRS"
      ],
      "metadata": {
        "description": "Storage Account type"
      }
    }
  },
  "variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]"
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[variables('storageAccountName')]",
      "apiVersion": "2016-01-01",
      "location": "[resourceGroup().location]",
      "sku": {
          "name": "[parameters('storageAccountType')]"
      },
      "kind": "Storage", 
      "properties": {
      }
    }
  ],
  "outputs": {
      "storageAccountName": {
          "type": "string",
          "value": "[variables('storageAccountName')]"
      }
  }
}
```

## <a name="next-steps"></a>Passos Seguintes
* Os exemplos neste artigo implementar recursos no grupo de recursos na sua subscrição predefinida. Para utilizar uma subscrição diferente, consulte [gerir várias subscrições do Azure](/powershell/azure/manage-subscriptions-azureps).
* Para obter um script de exemplo completo que implementa um modelo, consulte [script de implementação de modelo do Resource Manager](resource-manager-samples-powershell-deploy.md).
* Para compreender como definir parâmetros no seu modelo, veja [compreender a estrutura e a sintaxe de modelos Azure Resource Manager](resource-group-authoring-templates.md).
* Para obter dicas sobre como resolver erros comuns de implementação, consulte [resolver erros comuns de implementação do Azure com o Azure Resource Manager](resource-manager-common-deployment-errors.md).
* Para obter informações sobre como implementar um modelo que necessita de um token SAS, consulte [implementar modelo privado com o SAS token](resource-manager-powershell-sas-token.md).
* Para obter documentação de orientação sobre como as empresas podem utilizar o Resource Manager para gerir subscrições de forma eficaz, consulte [Azure enterprise scaffold - prescriptive subscription governance (Andaime empresarial do Azure - governação de subscrições prescritivas)](/azure/architecture/cloud-adoption-guide/subscription-governance).

