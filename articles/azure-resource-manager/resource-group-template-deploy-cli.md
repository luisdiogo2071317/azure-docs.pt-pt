---
title: Implementar recursos com a CLI do Azure e o modelo | Documentos da Microsoft
description: Utilize o Azure Resource Manager e a CLI do Azure para implementar um recursos no Azure. Os recursos são definidos num modelo do Resource Manager.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
ms.assetid: 493b7932-8d1e-4499-912c-26098282ec95
ms.service: azure-resource-manager
ms.devlang: azurecli
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/06/2018
ms.author: tomfitz
ms.openlocfilehash: 8c3d208b12166a590c68753fb4f58c9bb6e55610
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/26/2018
ms.locfileid: "47225536"
---
# <a name="deploy-resources-with-resource-manager-templates-and-azure-cli"></a>Implementar recursos com modelos do Resource Manager e do CLI do Azure

Este artigo explica como utilizar a CLI do Azure com modelos do Resource Manager para implementar os seus recursos no Azure. Se não estiver familiarizado com os conceitos de implementar e gerir as suas soluções do Azure, veja [descrição geral do Azure Resource Manager](resource-group-overview.md).  

O modelo do Resource Manager, implementar podem ser um ficheiro local no seu computador ou um arquivo externo que está localizado num repositório, como o GitHub. O modelo implementa neste artigo está disponível na [modelo de exemplo](#sample-template) secção, ou como um [modelo de conta de armazenamento no GitHub](https://github.com/Azure/azure-quickstart-templates/blob/master/101-storage-account-create/azuredeploy.json).

[!INCLUDE [sample-cli-install](../../includes/sample-cli-install.md)]

Se não tiver a CLI do Azure instalada, pode utilizar o [Cloud Shell](#deploy-template-from-cloud-shell).

## <a name="deploy-local-template"></a>Implementar o modelo de local

Quando implementar recursos no Azure,:

1. Inicie sessão na sua conta do Azure
2. Crie um grupo de recursos que funciona como contêiner para os recursos implementados. O nome do grupo de recursos só pode incluir carateres alfanuméricos, pontos finais, carateres de sublinhado, hífenes e parênteses. Pode ser até 90 carateres. Não pode terminar com um período.
3. Implementar o modelo que define os recursos para criar o grupo de recursos

Um modelo pode incluir parâmetros que lhe permitem personalizar a implementação. Por exemplo, pode fornecer valores que são adaptadas para um ambiente específico (por exemplo, desenvolvimento, teste e produção). O modelo de exemplo define um parâmetro para o SKU da conta de armazenamento. 

O exemplo seguinte cria um grupo de recursos e implementa um modelo a partir do seu computador local:

```azurecli-interactive
az group create --name ExampleGroup --location "Central US"
az group deployment create \
  --name ExampleDeployment \
  --resource-group ExampleGroup \
  --template-file storage.json \
  --parameters storageAccountType=Standard_GRS
```

A implementação pode demorar alguns minutos a concluir. Quando terminar, verá uma mensagem que inclui o resultado:

```azurecli
"provisioningState": "Succeeded",
```

## <a name="deploy-external-template"></a>Implementar modelo externo

Em vez de armazenar modelos do Resource Manager no seu computador local, pode armazená-las num local externo. Pode armazenar modelos num repositório de controle de origem (por exemplo, o GitHub). Em alternativa, pode armazená-los numa conta de armazenamento do Azure para acesso partilhado na sua organização.

Para implementar um modelo externo, utilize o **uri de modelo** parâmetro. Utilize o URI no exemplo para implementar o modelo de exemplo do GitHub.
   
```azurecli-interactive
az group create --name ExampleGroup --location "Central US"
az group deployment create \
  --name ExampleDeployment \
  --resource-group ExampleGroup \
  --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json" \
  --parameters storageAccountType=Standard_GRS
```

O exemplo anterior exige um URI publicamente acessível para o modelo, que funciona na maioria dos cenários, porque o modelo não deve incluir dados confidenciais. Se tiver de especificar os dados confidenciais (como uma palavra-passe de administrador), passe esse valor como um parâmetro seguro. No entanto, se não pretender que o modelo para estar acessível publicamente, pode protegê-los armazenando-os num contentor do armazenamento privado. Para obter informações sobre como implementar um modelo que precisa de um token de assinatura (SAS) de acesso partilhado, consulte [implementar modelo privado com o SAS token](resource-manager-cli-sas-token.md).

[!INCLUDE [resource-manager-cloud-shell-deploy.md](../../includes/resource-manager-cloud-shell-deploy.md)]

No Cloud Shell, utilize os seguintes comandos:

```azurecli-interactive
az group create --name examplegroup --location "South Central US"
az group deployment create --resource-group examplegroup \
  --template-uri <copied URL> \
  --parameters storageAccountType=Standard_GRS
```

## <a name="deploy-to-more-than-one-resource-group-or-subscription"></a>Implementar a mais do que um grupo de recursos ou subscrição

Normalmente, implementa todos os recursos no seu modelo para um grupo de recursos. No entanto, existem cenários onde pretende implementar um conjunto de recursos em conjunto, mas colocá-los em diferentes grupos de recursos ou subscrições. Pode implementar em apenas cinco grupos de recursos numa única implementação. Para obter mais informações, consulte [recursos do Azure implementar a mais de uma subscrição ou grupo de recursos](resource-manager-cross-resource-group-deployment.md).

## <a name="redeploy-when-deployment-fails"></a>Implementar novamente quando ocorre uma falha de implementação

Para implementações que falharam, pode especificar que uma implementação anterior do seu histórico de implementação é automaticamente implementada novamente. Para utilizar esta opção, as suas implementações tem de ter nomes exclusivos para que eles podem ser identificados na história. Se não tem nomes exclusivos, a falha na implementação atual poderá substituir a implementação bem-sucedida anterior na história. Só pode utilizar esta opção com implementações de nível de raiz. Implementações a partir de um modelo aninhado não estão disponíveis para reimplantação.

Para Reimplementar a última implementação efetuada com êxito, adicione o `--rollback-on-error` parâmetro como um sinalizador.

```azurecli-interactive
az group deployment create \
  --name ExampleDeployment \
  --resource-group ExampleGroup \
  --template-file storage.json \
  --parameters storageAccountType=Standard_GRS \
  --rollback-on-error
```

Para Reimplementar uma implementação específica, utilize o `--rollback-on-error` parâmetro e forneça o nome da implementação.

```azurecli-interactive
az group deployment create \
  --name ExampleDeployment02 \
  --resource-group ExampleGroup \
  --template-file storage.json \
  --parameters storageAccountType=Standard_GRS \
  --rollback-on-error ExampleDeployment01
```

A implementação especificada tem de ter foi concluída com êxito.

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

Para passar um ficheiro de parâmetros local, use `@` para especificar um ficheiro local com o nome storage.parameters.json.

```azurecli-interactive
az group deployment create \
  --name ExampleDeployment \
  --resource-group ExampleGroup \
  --template-file storage.json \
  --parameters @storage.parameters.json
```

## <a name="test-a-template-deployment"></a>Testar uma implementação de modelo

Para testar seus valores de modelo e o parâmetro sem ter de implementar, na verdade, todos os recursos, utilize [validar a implementação do grupo az](/cli/azure/group/deployment#az-group-deployment-validate). 

```azurecli-interactive
az group deployment validate \
  --resource-group ExampleGroup \
  --template-file storage.json \
  --parameters @storage.parameters.json
```

Se não são detetados erros, o comando devolve informações sobre a implementação de teste. Em particular, tenha em atenção que o **erro** valor é nulo.

```azurecli
{
  "error": null,
  "properties": {
      ...
```

Se for detetado um erro, o comando devolve uma mensagem de erro. Por exemplo, se tentar passar um valor incorreto para a conta de armazenamento SKU, devolve o erro seguinte:

```azurecli
{
  "error": {
    "code": "InvalidTemplate",
    "details": null,
    "message": "Deployment template validation failed: 'The provided value 'badSKU' for the template parameter 
      'storageAccountType' at line '13' and column '20' is not valid. The parameter value is not part of the allowed 
      value(s): 'Standard_LRS,Standard_ZRS,Standard_GRS,Standard_RAGRS,Premium_LRS'.'.",
    "target": null
  },
  "properties": null
}
```

Se o seu modelo tem um erro de sintaxe, o comando devolve um erro que indica que não foi possível analisar o modelo. A mensagem indica o número de linha e a posição de erro de análise.

```azurecli
{
  "error": {
    "code": "InvalidTemplate",
    "details": null,
    "message": "Deployment template parse failed: 'After parsing a value an unexpected character was encountered:
      \". Path 'variables', line 31, position 3.'.",
    "target": null
  },
  "properties": null
}
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
* Os exemplos neste artigo implementar recursos no grupo de recursos na sua subscrição predefinida. Para utilizar uma subscrição diferente, consulte [gerir várias subscrições do Azure](/cli/azure/manage-azure-subscriptions-azure-cli).
* Para especificar como lidar com recursos que existe no grupo de recursos, mas não estão definidas no modelo, consulte [modos de implementação Azure Resource Manager](deployment-modes.md).
* Para compreender como definir parâmetros no seu modelo, veja [compreender a estrutura e a sintaxe de modelos Azure Resource Manager](resource-group-authoring-templates.md).
* Para obter dicas sobre como resolver erros comuns de implementação, consulte [resolver erros comuns de implementação do Azure com o Azure Resource Manager](resource-manager-common-deployment-errors.md).
* Para obter informações sobre como implementar um modelo que necessita de um token SAS, consulte [implementar modelo privado com o SAS token](resource-manager-cli-sas-token.md).
* Para implementação com segurança o seu serviço em várias regiões, consulte [Gestor de implementação do Azure](deployment-manager-overview.md).
