---
title: Adicionar declarações condicionais fluxos de trabalho - Azure Logic Apps | Microsoft Docs
description: Como criar condições que controlam ações em fluxos de trabalho no Azure Logic Apps
services: logic-apps
ms.service: logic-apps
author: ecfan
ms.author: estfan
manager: cfowler
ms.date: 03/05/2018
ms.topic: article
ms.reviewer: klam, LADocs
ms.suite: integration
ms.openlocfilehash: 5629c743434a0fe53f35da29343945951bbc5dd7
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34725735"
---
# <a name="create-conditional-statements-that-control-workflow-actions-in-azure-logic-apps"></a>Criar instruções condicionais que controlam ações de fluxo de trabalho no Azure Logic Apps

Para executar ações específicas na sua aplicação lógica apenas após a transmitir uma condição especificada, adicione um *instrução condicional*. Esta estrutura compara os dados no fluxo de trabalho contra os campos ou valores específicos. Em seguida, pode definir diferentes ações que são executadas com base em se pretende ou não os dados cumprem a condição. Podem aninhar condições dentro entre si.

Por exemplo, suponha que tem uma aplicação lógica que envia mensagens de correio eletrónico demasiados quando novos itens são apresentados no feed RSS de um Web site. Pode adicionar uma instrução condicional para enviar correio eletrónico apenas quando o novo item inclui uma cadeia específica. 

> [!TIP]
> Para executar vários passos com base nos valores específicos diferentes, utilize um [ *comutador instrução* ](../logic-apps/logic-apps-control-flow-switch-statement.md) em vez disso.

## <a name="prerequisites"></a>Pré-requisitos

* Uma subscrição do Azure. Se não tiver uma subscrição, [inscreva-se numa conta do Azure gratuita](https://azure.microsoft.com/free/).

* Conhecimento básico sobre [como criar aplicações lógicas](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* A seguir o exemplo neste artigo, [criar esta aplicação de lógica de exemplo](../logic-apps/quickstart-create-first-logic-app-workflow.md) com uma conta do Outlook.com ou Outlook do Office 365.

## <a name="add-a-condition"></a>Adicionar uma condição

1. No <a href="https://portal.azure.com" target="_blank">portal do Azure</a>, abra a aplicação lógica no Designer de aplicação lógica.

2. Adicione uma condição na localização que pretende. 

   Para adicionar uma condição entre os passos, mova o ponteiro sobre na seta para onde pretende adicionar a condição. Escolha o **sinal** (**+**) que é apresentado, em seguida, escolha **adicionar uma condição**. Por exemplo:

   ![Adicionar condição entre os passos](./media/logic-apps-control-flow-conditional-statement/add-condition.png)

   Quando pretender adicionar uma condição no fim do fluxo de trabalho, na parte inferior da sua aplicação lógica, escolha **+ novo passo** > **adicionar uma condição**.

3. Em **condição**, criar a condição. 

   1. Na caixa de esquerda, especifique os dados ou o campo que pretende comparar.

      Do **adicionar conteúdo dinâmico** lista, pode selecionar existentes campos da sua aplicação lógica.

   2. Na lista de média, selecione a operação a realizar. 
   3. Na caixa de direita, especifique um valor ou o campo como os critérios.

   Por exemplo:

   ![Editar condição no modo básico](./media/logic-apps-control-flow-conditional-statement/edit-condition-basic-mode.png)

   Segue-se a condição concluída:

   ![Concluir condição](./media/logic-apps-control-flow-conditional-statement/edit-condition-basic-mode-2.png)

   > [!TIP]
   > Para criar uma condição mais avançada ou utilizar expressões, escolha **editar no modo avançado**. Pode utilizar expressões definidas pelo [linguagem de definição de fluxo de trabalho](../logic-apps/logic-apps-workflow-definition-language.md).
   > 
   > Por exemplo:
   >
   > ![Editar condição no código](./media/logic-apps-control-flow-conditional-statement/edit-condition-advanced-mode.png)

5. Em **se Sim** e **não se**, adicionar os passos para efetuar com base em se a condição é cumprida. Por exemplo:

   ![Condição com Sim e não existem caminhos](./media/logic-apps-control-flow-conditional-statement/condition-yes-no-path.png)

   > [!TIP]
   > Pode arrastar ações existentes para o **se Sim** e **não se** caminhos.

6. Guarde a aplicação lógica.

Agora esta aplicação lógica só envia correio quando os novos itens no feed RSS satisfazem a condição.

## <a name="json-definition"></a>Definição JSON

Vamos ver agora que criou uma aplicação lógica, utilizando uma instrução condicional, a definição de alto nível código atrás a instrução condicional.

``` json
"actions": {
  "myConditionName": {
    "type": "If",
    "expression": "@contains(triggerBody()?['summary'], 'Microsoft')",
    "actions": {
      "Send_an_email": {
        "inputs": { },
        "runAfter": {}
      }
    },
    "runAfter": {}
  }
},
```

## <a name="get-support"></a>Obter suporte

* Relativamente a dúvidas, visite o [fórum do Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Para submeter ou votar em funcionalidades e sugestões, visite o [site de comentários do utilizador do Azure Logic Apps](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Passos Seguintes

* [Executar passos com base nos valores diferentes (comutador instruções)](../logic-apps/logic-apps-control-flow-switch-statement.md)
* [Executar e repita os passos (ciclos)](../logic-apps/logic-apps-control-flow-loops.md)
* [Executar ou merge passos paralelos (ramos)](../logic-apps/logic-apps-control-flow-branches.md)
* [Executar passos com base no estado da ação agrupada (âmbitos)](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md)
