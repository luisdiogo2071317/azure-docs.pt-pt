---
title: "Não foram encontrados erros de recursos do Azure | Microsoft Docs"
description: "Descreve como resolver erros quando não é possível localizar um recurso."
services: azure-resource-manager,azure-portal
documentationcenter: 
author: tfitzmac
manager: timlt
editor: 
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: support-article
ms.date: 03/08/2018
ms.author: tomfitz
ms.openlocfilehash: 6844c1c2938873b0a74fe66e846dc733a4bd6ff7
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/12/2018
---
# <a name="resolve-not-found-errors-for-azure-resources"></a>Resolva os erros não encontrados para recursos do Azure

Este artigo descreve os erros que poderão surgir quando não é possível encontrar um recurso durante a implementação.

## <a name="symptom"></a>Sintoma

Quando o modelo inclui o nome de um recurso que não pode ser resolvido, receberá um erro semelhante a:

```
Code=NotFound;
Message=Cannot find ServerFarm with name exampleplan.
```

Se tentar utilizar o [referência](resource-group-template-functions-resource.md#reference) ou [listKeys](resource-group-template-functions-resource.md#listkeys) funciona com um recurso que não pode ser resolvido, receberá o seguinte erro:

```
Code=ResourceNotFound;
Message=The Resource 'Microsoft.Storage/storageAccounts/{storage name}' under resource
group {resource group name} was not found.
```

## <a name="cause"></a>Causa

Gestor de recursos tem de obter as propriedades de um recurso, mas não é possível identificar o recurso na sua subscrição.

## <a name="solution-1---set-dependencies"></a>Solução 1 - dependências de conjunto

Se estiver a tentar implementar o recurso em falta no modelo, verifique se tem de adicionar uma dependência. Gestor de recursos otimiza a implementação através da criação de recursos em paralelo, sempre que possível. Se um recurso tem de ser implementado após a outro recurso, tem de utilizar o **dependsOn** elemento no modelo. Por exemplo, quando implementar uma aplicação web, o plano de serviço aplicacional tem de existir. Se ainda não especificou que a aplicação web depende do plano do App Service, o Gestor de recursos cria os dois recursos ao mesmo tempo. Receberá um erro a indicar que o serviço de aplicações não é possível localizar o recurso do plano, porque não existe ainda ao tentar definir uma propriedade na aplicação web. Impedir que este erro ao definir a dependência na aplicação web.

```json
{
  "apiVersion": "2015-08-01",
  "type": "Microsoft.Web/sites",
  "dependsOn": [
    "[variables('hostingPlanName')]"
  ],
  ...
}
```

No entanto, pretende evite definir dependências que não são necessários. Quando tiver dependências desnecessárias, prolongar a duração da implementação ao impedir que os recursos que não são dependentes entre si de que está a ser implementado em paralelo. Além disso, pode criar dependências circulares que bloquear a implementação. O [referência](resource-group-template-functions-resource.md#reference) função cria uma dependência implícita no recurso referenciado, quando esse recurso é implementado no mesmo modelo. Por conseguinte, pode ter mais de dependências que as dependências especificados no **dependsOn** propriedade. O [resourceId](resource-group-template-functions-resource.md#resourceid) função não criar uma dependência implícita ou a validar que o recurso existe.

Quando tiver problemas de dependência, terá de obter conhecimentos aprofundados sobre a ordem de implementação de recursos. Para ver a ordem das operações de implementação:

1. Selecione o histórico de implementação para o grupo de recursos.

   ![Selecione o histórico de implementação](./media/resource-manager-not-found-errors/select-deployment.png)

2. Selecione uma implementação do histórico e selecione **eventos**.

   ![Selecione os eventos de implementação](./media/resource-manager-not-found-errors/select-deployment-events.png)

3. Examine a sequência de eventos para cada recurso. Preste atenção para o estado de cada operação. Por exemplo, a imagem seguinte mostra as três contas do storage implementada em paralelo. Tenha em atenção que as três contas de armazenamento são iniciadas ao mesmo tempo.

   ![Implementação paralela](./media/resource-manager-not-found-errors/deployment-events-parallel.png)

   A imagem seguinte mostra os três contas de armazenamento que não são implementadas em paralelo. A segunda conta de armazenamento depende a primeira conta de armazenamento e a conta de armazenamento terceira depende a segunda conta de armazenamento. A primeira conta de armazenamento é iniciada, aceite e concluída antes da próxima é iniciada.

   ![implementação sequencial](./media/resource-manager-not-found-errors/deployment-events-sequence.png)

## <a name="solution-2---get-resource-from-different-resource-group"></a>Solução 2 - resource de get do grupo de recursos diferente

Quando o recurso existe num grupo de recursos diferente daquela que está a ser implementada, utilize o [resourceId função](resource-group-template-functions-resource.md#resourceid) para obter o nome completamente qualificado do recurso.

```json
"properties": {
    "name": "[parameters('siteName')]",
    "serverFarmId": "[resourceId('plangroup', 'Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
}
```

## <a name="solution-3---check-reference-function"></a>Solução 3 - a função de referência de verificação

Procure uma expressão que inclui o [referência](resource-group-template-functions-resource.md#reference) função. Os valores que fornece variam com base em se o recurso está no mesmo modelo, grupo de recursos e subscrição. Volte a verificar que está a fornecer os valores de parâmetros necessários para o seu cenário. Se o recurso está num grupo de recursos diferente, forneça o ID de recurso completo. Por exemplo, para fazer referência a uma conta de armazenamento noutro grupo de recursos, utilize:

```json
"[reference(resourceId('exampleResourceGroup', 'Microsoft.Storage/storageAccounts', 'myStorage'), '2017-06-01')]"
```