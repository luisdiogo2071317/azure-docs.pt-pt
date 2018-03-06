---
title: Paralela ramos - Azure Logic Apps | Microsoft Docs
description: Criar ou associar ramos paralelos nas logic apps
services: logic-apps
keywords: ramos, processamento efetuada em paralelo
documentationcenter: 
author: ecfan
manager: anneta
editor: 
ms.assetid: 
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/05/2018
ms.author: estfan; LADocs
ms.openlocfilehash: 9a836b707a576b9a938f43397ef35c00aeb476bf
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/05/2018
---
# <a name="create-or-join-parallel-branches-in-your-logic-app"></a>Criar ou associar ramos paralelos na sua aplicação lógica

Por predefinição, as ações numa aplicação lógica executam sequencialmente. Para efetuar ações independentes ao mesmo tempo, pode criar [paralela ramos](#parallel-branches)e, em seguida, [associação dessas ramos](#join-branches) mais à frente do fluxo. 

> [!TIP] 
> Se tiver um acionador que recebe uma matriz e pretende executar um fluxo de trabalho para cada item de matriz, pode *debatch* essa matriz com o [ **SplitOn** acionar propriedade](../logic-apps/logic-apps-workflow-actions-triggers.md#split-on-debatch).

## <a name="prerequisites"></a>Pré-requisitos

* Uma subscrição do Azure. Se não tiver uma subscrição, [inscreva-se numa conta do Azure gratuita](https://azure.microsoft.com/free/). 

* Conhecimento básico sobre [como criar aplicações lógicas](../logic-apps/quickstart-create-first-logic-app-workflow.md)

<a name="parallel-branches"></a>

## <a name="add-a-parallel-branch"></a>Adicionar um ramo paralelo

Para executar passos independentes ao mesmo tempo, pode adicionar ramos paralelos junto de um passo existente. 

![Execute os passos em paralelo](media/logic-apps-control-flow-branches/parallel.png)

A aplicação lógica aguarda para todos os ramos concluir antes de continuar o fluxo de trabalho.
Paralela ramos executados apenas quando os respetivos `runAfter` valores de propriedade corresponde ao estado do passo de principal foi terminada. Por exemplo, ambos `branchAction1` e `branchAction2` estiver configurado para ser executado apenas quando o `parentAction` for concluída com `Succeded` estado.

> [!NOTE]
> Antes de começar, a aplicação lógica já tem de ter um passo onde pode adicionar ramos paralelos.

1. No <a href="https://portal.azure.com" target="_blank">portal do Azure</a>, abra a aplicação lógica no Designer de aplicação lógica.

2. Mova o rato sobre a seta acima o passo em que pretende adicionar os ramos paralelos.

3. Escolha o **plus** início de sessão (**+**), escolha **adicionar um ramo paralelo**e selecione o item que pretende adicionar.

   ![Adicionar ramo paralelo](media/logic-apps-control-flow-branches/add-parallel-branch.png)

   O item selecionado aparece um ramo paralelo.

4. Para cada ramificação paralela, adicione os passos que pretender. Para adicionar uma ação sequencial para um ramo paralelo, mova o rato sob a ação onde pretende adicionar a ação sequencial. Escolha o **plus** (**+**) início de sessão e o passo de que pretende adicionar.

   ![Adicione o passo sequencial para a qual ramificar paralela](media/logic-apps-control-flow-branches/add-sequential-action-parallel-branch.png)

5. A intercalação ramos novamente em conjunto, [associar os ramos paralelos](#join-branches). 

<a name="parallel-json"></a>

## <a name="parallel-branch-definition-json"></a>Definição de sucursal paralelo (JSON)

Se estiver a trabalhar na vista de código, pode definir a estrutura paralela na definição de JSON da sua aplicação lógica em vez disso, por exemplo:

``` json
{
  "triggers": {
    "myTrigger": { }
  },
  "actions": {
    "parentAction": {
      "type": "<action-type>",
      "inputs": { },
      "runAfter": {}
    },
    "branchAction1": {
      "type": "<action-type>",
      "inputs": { },
      "runAfter": {
        "parentAction": [
          "Succeeded"
        ]
      }
    },
    "branchAction2": {
      "type": "<action-type>",
      "inputs": { },
      "runAfter": {
        "parentAction": [
          "Succeeded"
        ]
      }
    }
  },
  "outputs": {}
}
```

<a name="join-branches"></a>

## <a name="join-parallel-branches"></a>Associar ramos paralelos

A intercalação ramos paralelos em conjunto, basta adicione um passo na parte inferior em todos os ramos. Este passo é executado após todos os concluir os ramos paralelos em execução.

![Associar ramos paralelos](media/logic-apps-control-flow-branches/join.png)

1. No [portal do Azure](https://portal.azure.com), localize e abra a sua aplicação lógica no Designer de aplicação lógica. 

2. Nos ramos paralelos que pretende associar, adicione o passo de que pretende efetuar.

   ![Adicione um passo que associa os ramos paralelos](media/logic-apps-control-flow-branches/join-steps.png)

   Os ramos paralelos são Unidos.

<a name="join-json"></a>

## <a name="join-definition-json"></a>Associar definição (JSON)

Se estiver a trabalhar na vista de código, pode definir a estrutura de associação na definição de JSON da sua aplicação lógica em vez disso, por exemplo:

``` json
{
  "triggers": {
    "myTrigger": { }
  },
  "actions": {
    "parentAction": {
      "type": "<action-type>",
      "inputs": { },
      "runAfter": {}
    },
    "branchAction1": {
      "type": "<action-type>",
      "inputs": { },
      "runAfter": {
        "parentAction": [
          "Succeeded"
        ]
      }
    },
    "branchAction2": {
      "type": "<action-type>",
      "inputs": { },
      "runAfter": {
        "parentAction": [
          "Succeeded"
        ]
      }
    },
    "joinAction": {
      "type": "<action-type>",
      "inputs": { },
      "runAfter": {
        "branchAction1": [
          "Succeeded"
        ],
        "branchAction2": [
          "Succeeded"
        ]
      }
    }
  },
  "outputs": {}
}
```

## <a name="get-support"></a>Obter suporte

* Relativamente a dúvidas, visite o [fórum do Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Para submeter ou votar em funcionalidades e sugestões, visite o [site de comentários do utilizador do Azure Logic Apps](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Passos Seguintes

* [Executar passos com base numa condição (instruções condicionais)](../logic-apps/logic-apps-control-flow-conditional-statement.md)
* [Executar passos com base nos valores diferentes (comutador instruções)](../logic-apps/logic-apps-control-flow-switch-statement.md)
* [Executar e repita os passos (ciclos)](../logic-apps/logic-apps-control-flow-loops.md)
* [Executar passos com base no estado da ação agrupada (âmbitos)](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md)