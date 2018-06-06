---
title: Adicionar declarações de comutador para fluxos de trabalho - Azure Logic Apps | Microsoft Docs
description: Como criar instruções de comutador que controlam ações de fluxo de trabalho com base nos valores específicos do Azure Logic Apps
services: logic-apps
ms.service: logic-apps
author: ecfan
ms.author: estfan
manager: cfowler
ms.date: 03/05/2018
ms.topic: article
ms.reviewer: klam, LADocs
ms.suite: integration
ms.openlocfilehash: 2ffac49d14e05ff252d6cd0e90fc23d77ac0caff
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34726079"
---
# <a name="create-switch-statements-that-run-workflow-actions-based-on-specific-values-in-azure-logic-apps"></a>Criar as instruções de comutador que executam ações de fluxo de trabalho com base nos valores específicos do Azure Logic Apps

Para executar ações específicas com base nos valores de objetos, expressões ou tokens, adicione um *comutador* instrução. Esta estrutura avalia o objeto, expressão ou token, escolhe o cenário que corresponde ao resultado e executa as ações específicas apenas para esse cenário. Quando é executada da instrução switch, apenas um cenário deve corresponder ao resultado.

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
   | Cenário 2 | **Rejeitar** | Adicionar o Outlook **enviar um e-mail** ação para notificar outros aprovadores que o item RSS foi rejeitado. |
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
