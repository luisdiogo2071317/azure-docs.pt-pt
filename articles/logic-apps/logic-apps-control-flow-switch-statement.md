---
title: "Comutador instruções - passos de execução com base nos valores específicos - Azure Logic Apps | Microsoft Docs"
description: "Executar vários passos com base nos valores de objetos, expressões ou tokens nas logic apps"
services: logic-apps
keywords: "Falha da instrução"
author: ecfan
manager: anneta
editor: 
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/05/2018
ms.author: estfan; LADocs
ms.openlocfilehash: e1f515189be8a5659af0f6c29b3fac0550abc9f9
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/05/2018
---
# <a name="switch-statements-run-different-steps-based-on-specific-values-in-logic-apps"></a>Instruções de comutador: executar vários passos com base nos valores específicos as logic apps

Para executar vários passos com base nos valores de um objeto, expressão ou token, utilize um *comutador* instrução. Esta estrutura avalia o objeto, expressão ou token, escolhe o cenário que corresponde ao resultado e executa os passos para apenas nesse caso. Quando é executada da instrução switch, apenas um cenário deve corresponder ao resultado.

Por exemplo, suponha que pretende que uma aplicação lógica que utiliza diferentes passos com base na opção selecionada no e-mail. Neste exemplo, a aplicação lógica verifica o feed para novo conteúdo RSS de um Web site. Quando um novo item é apresentado no feed RSS, a aplicação lógica envia e-mail para um aprovador. Com base em se o aprovador seleciona "Aprovar" ou "Rejeitar", a aplicação lógica segue vários passos.

> [!TIP]
> Como todos os idiomas de programação, declarações de comutador suportam apenas os operadores de igualdade. Se precisar de outros operadores relacionais, tais como "superior", utilize um [instrução condicional](#conditions).
> Para garantir um comportamento de execução determinista, casos tem de conter um valor exclusivo e estático em vez de tokens dinâmicos ou expressões.

## <a name="prerequisites"></a>Pré-requisitos

* Uma subscrição do Azure. Se não tiver uma subscrição, [inscreva-se numa conta do Azure gratuita](https://azure.microsoft.com/free/).

* A seguir o exemplo neste artigo, [criar esta aplicação de lógica de exemplo](../logic-apps/quickstart-create-first-logic-app-workflow.md) com uma conta do Outlook.com ou Outlook do Office 365.

  1. Quando adiciona a ação para enviar correio eletrónico, selecione **enviar um e-mail de aprovação** em vez disso.

     ![Selecione "Enviar um e-mail de aprovação"](./media/logic-apps-control-flow-switch-statement/send-approval-email-action.png)

  2. Forneça os campos obrigatórios, como o endereço de e-mail para a pessoa que obtém a mensagem de e-mail de aprovação. 
  Em **User Options**, introduza "Aprovar, rejeitar".

     ![Introduza os detalhes de e-mail](./media/logic-apps-control-flow-switch-statement/send-approval-email-details.png)

## <a name="add-a-switch-statement"></a>Adicione uma instrução de comutador

1. No fim do fluxo de trabalho de exemplo, escolha **+ novo passo** > **... Mais** > **adicionar minúsculas**. 

   ![Adicione uma instrução de comutador](./media/logic-apps-control-flow-switch-statement/add-switch-statement.png)

   Uma instrução de comutador é apresentada com um cenário e um caso de predefinição. 
   Uma instrução requer, pelo menos, um cenário plus o caso de predefinição. 

   Quando pretender adicionar uma instrução de comutador entre os passos, mova o ponteiro sobre na seta para onde pretende adicionar a instrução de comutador. 
   Escolha o **sinal** (**+**) que é apresentado, em seguida, escolha **adicionar minúsculas**.

4. No **no** caixa, selecione o **SelectedOption** campo cuja saída determina a ação a executar. 
   
   Pode selecionar o campo do **adicionar conteúdo dinâmico** lista aparece.

5. Para processar os casos em que seleciona o aprovador `Approve` ou `Reject`, adicione outro cenário entre **caso** e **predefinido**. 
   
6. Adicione estas ações para os casos correspondentes:

   | # Maiúsculas | **SelectedOption** | Ação |
   |:------ |:-------------------|:------ |
   | Caso 1 | **Aprovar** | Adicionar o Outlook **enviar um e-mail** ação para o envio de detalhes sobre o item RSS apenas quando o aprovador selecionado **aprovar**. |
   | Cenário 2 | **Reject** | Adicionar o Outlook **enviar um e-mail** ação para notificar outros aprovadores que o item RSS foi rejeitado. |
   | Predefinição | \<Nenhum\> | Nenhuma ação necessária. Neste exemplo, o **predefinido** maiúsculas e minúsculas estão vazia porque **SelectedOption** tem apenas duas opções. |
   |         |          |

   ![Falha da instrução](./media/logic-apps-control-flow-switch-statement/switch.png)

7. Guarde a aplicação lógica. 

   Para testar manualmente neste exemplo, escolha **executar** até que a aplicação lógica localiza um novo item RSS e envia um e-mail de aprovação. 
   Selecione **aprovar** para observar os resultados.

## <a name="json-definition"></a>Definição JSON

Vamos ver agora que criou uma aplicação lógica, utilizando uma instrução de comutador, a definição de alto nível código atrás da instrução switch.

``` json
"Switch": {
   "type": "Switch",
   "expression": "@body('Send_approval_email')?['SelectedOption']",
   "cases": {
      "Case" : {
         "actions" : {
           "Send_an_email": { }
         },
         "case" : "Approve"
      },
      "Case_2" : {
         "actions" : {
           "Send_an_email_2": { }
         },
         "case" : "Reject"
      }
   },
   "default": {
      "actions": {}
   },
   "runAfter": {
      "Send_approval_email": [
         "Succeeded"
      ]
   }
}
```

| Etiqueta              | Descrição |
| :----------------- | :---------- |
| `"Switch"`         | O nome da instrução switch, que pode mudar o nome de para legibilidade |
| `"type": "Switch"` | Especifica que a ação de uma instrução de comutador |
| `"expression"`     | Neste exemplo, especifica a opção de selecionado o aprovador que é avaliada em comparação com cada caso como declarada mais tarde na definição do |
| `"cases"` | Define qualquer número de cenários. Para cada caso, `"Case_*"` é o nome predefinido para esse cenário, o que pode mudar o nome de para legibilidade |
| `"case"` | Especifica o valor o caso, o que deve ser um valor de constante e é exclusivo e da instrução switch utiliza para comparação. Se nenhum casos correspondem o resultado da expressão de comutador, as ações no `"default"` secção são executados.
|           |         |

## <a name="get-support"></a>Obter suporte

* Relativamente a dúvidas, visite o [fórum do Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Para submeter ou votar em funcionalidades ou sugestões, visite o [site de comentários do utilizador do Azure Logic Apps](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Passos Seguintes

* [Executar passos com base numa condição (instruções condicionais)](../logic-apps/logic-apps-control-flow-conditional-statement.md)
* [Executar e repita os passos (ciclos)](../logic-apps/logic-apps-control-flow-loops.md)
* [Executar ou merge passos paralelos (ramos)](../logic-apps/logic-apps-control-flow-branches.md)
* [Executar passos com base no estado da ação agrupada (âmbitos)](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md)