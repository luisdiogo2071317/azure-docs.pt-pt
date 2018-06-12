---
title: Adicionar âmbitos executam ações com base no estado do grupo - Azure Logic Apps | Microsoft Docs
description: Como criar âmbitos que executam ações de fluxo de trabalho com base no estado de ação de grupo no Azure Logic Apps
services: logic-apps
ms.service: logic-apps
author: ecfan
ms.author: estfan
manager: jeconnoc
ms.date: 03/05/2018
ms.topic: article
ms.reviewer: klam, LADocs
ms.suite: integration
ms.openlocfilehash: 1258175eb3d28d39be8be08498ba8d2e0998aa43
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35298819"
---
# <a name="create-scopes-that-run-workflow-actions-based-on-group-status-in-azure-logic-apps"></a>Crie âmbitos que executam ações de fluxo de trabalho com base no estado de grupo no Azure Logic Apps

Para executar ações apenas depois de outro grupo de ações ou não bem-sucedidos, grupo essas ações dentro de um *âmbito*. Esta estrutura é útil quando pretender organizar ações como um grupo lógico, avaliar o estado desse grupo e executar ações que são baseadas no estado do âmbito. Depois de todas as ações num âmbito termine a execução, o âmbito também obtém o seu próprio Estado. Por exemplo, pode utilizar âmbitos quando pretender implementar [exceções e processamento de erros](../logic-apps/logic-apps-exception-handling.md#scopes). 

Para verificar o estado de um âmbito, pode utilizar os mesmos critérios que utilizar para determinar uma lógica das aplicações executam Estado, tais como "Com êxito", "Falha", "Cancelada" e assim sucessivamente. Por predefinição, quando as ações do âmbito for bem-sucedida, o âmbito está marcada como "Com êxito." Mas quando qualquer ação no âmbito de falha ou cancelamento, estado do âmbito está marcada como "Falhado". Para os limites nos âmbitos de mensagens em fila, consulte [limites e configuração](../logic-apps/logic-apps-limits-and-config.md). 

Por exemplo, aqui é uma aplicação de lógica de alto nível que utiliza um âmbito para executar as ações específicas e uma condição para verificar o estado do âmbito. Se quaisquer ações no âmbito falharam ou terminar inesperadamente, o âmbito está marcado como "Falha" ou "Abortado", respetivamente, e a aplicação lógica envia uma mensagem "O âmbito falhou". Se tiver êxito todas as ações âmbito, a aplicação lógica envia uma mensagem "O âmbito concluída com êxito".

![Configurar o acionador "Agenda - Recurrence"](./media/logic-apps-control-flow-run-steps-group-scopes/scope-high-level.png)

## <a name="prerequisites"></a>Pré-requisitos

Para seguir o exemplo neste artigo, terá destes itens:

* Uma subscrição do Azure. Se não tiver uma subscrição, [inscreva-se numa conta do Azure gratuita](https://azure.microsoft.com/free/). 

* Uma conta de e-mail a partir de qualquer fornecedor de correio eletrónico suportado pelo Logic Apps. Este exemplo utiliza Outlook.com. Se utilizar um fornecedor diferente, o fluxo geral permanece o mesmo, mas a sua IU aparece diferentes.

* Uma chave do Bing Maps. Para obter esta chave, consulte <a href="https://msdn.microsoft.com/library/ff428642.aspx" target="_blank">obter uma chave do Bing Maps</a>.

* Conhecimento básico sobre [como criar aplicações lógicas](../logic-apps/quickstart-create-first-logic-app-workflow.md)

## <a name="create-sample-logic-app"></a>Criar aplicação de lógica de exemplo

Em primeiro lugar, crie esta aplicação de lógica de exemplo para que possa adicionar um âmbito mais tarde:

![Criar aplicação de lógica de exemplo](./media/logic-apps-control-flow-run-steps-group-scopes/finished-sample-app.png)

* A **agenda - periodicidade** acionador que verifica o serviço do Bing Maps um intervalo que especificar
* A **Bing Maps - Get rota** ação que verifica a hora de levar entre duas localizações
* Uma instrução condicional que verifica se o tempo de levar excede o tempo de levar especificado
* Uma ação que envia que e-mail levar tempo atual excede o tempo especificado

Pode guardar a sua aplicação lógica em qualquer altura, por isso, muitas vezes, a guardar o seu trabalho.

1. Iniciar sessão para o <a href="https://portal.azure.com" target="_blank">portal do Azure</a>, se ainda não o fez. Criar uma aplicação lógica em branco.

2. Adicionar o **agenda - periodicidade** acionador com estas definições: **intervalo** = "1" e **frequência** = "Minutos"

   ![Configurar o acionador "Agenda - Recurrence"](./media/logic-apps-control-flow-run-steps-group-scopes/recurrence.png)

   > [!TIP]
   > Para visualmente simplificar a sua vista e ocultar detalhes de cada ação no designer, fechar a forma de cada ação à medida que estes passos.

3. Adicionar o **Bing Maps - Get rota** ação. 

   1. Se ainda não tiver uma ligação do Bing Maps, está a pedido para criar uma ligação.

      | Definição | Valor | Descrição |
      | ------- | ----- | ----------- |
      | **Nome da Ligação** | BingMapsConnection | Indique um nome para a ligação. | 
      | **Chave de API** | <*your-Bing-Maps-key*> | Introduza a chave do Mapas Bing que recebeu anteriormente. | 
      ||||  

   2. Configurar a sua **Get rota** ação, como mostrado a tabela abaixo esta imagem:

      ![Configurar "Bing Maps - Get rota" ação](./media/logic-apps-control-flow-run-steps-group-scopes/get-route.png) 

      Para obter mais informações sobre estes parâmetros, veja [Calculate a route](https://msdn.microsoft.com/library/ff701717.aspx) (Calcular um percurso).

      | Definição | Valor | Descrição |
      | ------- | ----- | ----------- |
      | **Waypoint 1** | <*start*> | Introduza a origem da rota. | 
      | **Waypoint 2** | <*Fim*> | Introduza o destino da rota. | 
      | **Avoid** | Nenhuma | Introduza itens para evitar no seu rota, como highways, tolls e assim sucessivamente. Para os valores possíveis, consulte [calcular uma rota](https://msdn.microsoft.com/library/ff701717.aspx). | 
      | **Optimize** | timeWithTraffic | Selecione um parâmetro para otimizar a rota, como a distância, o período de tempo com as informações atuais do tráfego e assim sucessivamente. Este exemplo utiliza este valor: "timeWithTraffic" | 
      | **Distance unit** | <*your-preference*> | Introduza a unidade de distância para calcular a rota. Este exemplo utiliza este valor: "Mile" | 
      | **Travel mode** | Driving | Introduza o modo de levar a rota. Este exemplo utiliza este valor "Driving" | 
      | **Transit Date-Time** | Nenhuma | Aplica-se apenas no modo trânsito. | 
      | **Tipo de tipo de data de trânsito** | Nenhuma | Aplica-se apenas no modo trânsito. | 
      ||||  

4. Adicione uma condição para Veri se a hora atual do levar com tráfego excede um tempo especificado. Neste exemplo, siga os passos nesta imagem:

   ![Criar a condição](./media/logic-apps-control-flow-run-steps-group-scopes/build-condition.png)

   1. Mudar o nome da condição com esta descrição: **se o tráfego de tempo mais do que o período de tempo especificado**

   2. Na lista de parâmetros, selecione o **levar duração tráfego** campo, o que está em segundos. 

   3. Para o operador de comparação, selecione este operador: **é maior do que**

   4. Para o valor de comparação, introduza **600**, que está a ser segundos e equivalente para 10 minutos.

5. A condição **se for verdadeiro** sucursal, adicionar uma ação "enviar correio eletrónico" para o seu fornecedor de correio eletrónico. Configure esta ação com os detalhes conforme mostrado na tabela nesta imagem:

   ![Adicionar ação "Enviar uma mensagem de e-mail" a "se for verdadeiro" sucursal](./media/logic-apps-control-flow-run-steps-group-scopes/send-email.png)

   1. Para o **para** campo, introduza o seu endereço de e-mail para fins de teste.

   2. Para o **requerente** campo, introduza este texto:

      ```Time to leave: Traffic more than 10 minutes```

   3. Para o **corpo** campo, introduza este texto com um espaço à direita: 

      ```Travel time: ```

      Enquanto o cursor é apresentado no **corpo** campo, a lista de conteúdo dinâmica permanece aberta, para que possa selecionar quaisquer parâmetros que estão disponíveis neste momento.

   4. Na lista de conteúdo dinâmico, escolha **Expressão**.

   5. Localize e selecione o **div (-)** função.

   6. Enquanto o cursor está dentro de parênteses a função, escolha **conteúdo dinâmico** para que possa adicionar o **tráfego duração tráfego** parâmetro seguinte.

   7. Em **Get rota** na lista de parâmetros dinâmicos, selecione o **tráfego duração tráfego** campo.

      ![Selecione "Tráfego de duração de tráfego"](./media/logic-apps-control-flow-run-steps-group-scopes/send-email-2.png)

   8. Depois do campo é resolvido para o formato JSON, adicione um **vírgulas** (```,```) seguido do número ```60``` para que a converter o valor do **tráfego duração tráfego** de segundos de minutos. 
   
      ```
      div(body('Get_route')?['travelDurationTraffic'],60)
      ```

      A expressão agora parece que este exemplo:

      ![Expressão de conclusão](./media/logic-apps-control-flow-run-steps-group-scopes/send-email-3.png)  

   9. Certifique-se que escolha **OK** quando tiver terminado.

  10. Depois da expressão é resolvido, adicione este texto com um espaço à esquerda: ``` minutes```
  
      O **corpo** campo aspeto agora neste exemplo:

      ![Campo de "Corpo" concluído](./media/logic-apps-control-flow-run-steps-group-scopes/send-email-4.png)

6. Guarde a aplicação lógica.

Em seguida, adicione um âmbito para que possam grupo ações específicas e avaliar o respetivo estado.

## <a name="add-a-scope"></a>Adicionar âmbito

1. Se ainda não o tiver feito, abra a aplicação lógica no Designer de aplicação lógica. 

2. Adicione um âmbito na localização de fluxo de trabalho que pretende. Por exemplo:

   * Para adicionar um âmbito de entre os passos existentes no fluxo de trabalho de aplicação lógica, mova o ponteiro sobre na seta para onde pretende adicionar o âmbito. 
   Escolha o **sinal** (**+**) > **adicionar um âmbito**.

     ![Adicionar âmbito](./media/logic-apps-control-flow-run-steps-group-scopes/add-scope.png)

     Quando pretender adicionar um âmbito no fim do fluxo de trabalho, na parte inferior da sua aplicação lógica, escolha **+ novo passo** > **... Mais** > **adicionar um âmbito**.

3. Agora, adicione os passos ou arraste passos existentes que pretende executar no âmbito. Neste exemplo, arraste estas ações para o âmbito:
      
   * **Obter a rota**
   * **Se o tráfego de tempo mais do que o período de tempo especificado**, que inclui tanto o **verdadeiro** e **falso** ramos

   A aplicação lógica aspeto agora neste exemplo:

   ![Âmbito adicionado](./media/logic-apps-control-flow-run-steps-group-scopes/scope-added.png)

4. No âmbito, adicione uma condição que verifica o estado do âmbito. Mudar o nome da condição com esta descrição: **caso de falha de âmbito**

   ![Adicionar condição para verificar o estado de âmbito](./media/logic-apps-control-flow-run-steps-group-scopes/add-condition-check-scope-status.png)
  
5. Criar expressão que verifica se o âmbito é igual a `Failed` ou `Aborted`.

   ![Adicionar a expressão que verifica o estado do âmbito](./media/logic-apps-control-flow-run-steps-group-scopes/build-expression-check-scope-status.png)

   Ou, para introduzir esta expressão como texto, escolha **editar no modo avançado**.

   ```@equals('@result(''Scope'')[0][''status'']', 'Failed, Aborted')```

6. No **se for verdadeiro** e **se for FALSO** ramos, adicionar as ações que pretende efetuar, por exemplo, enviam correio eletrónico ou uma mensagem.

   ![Adicionar a expressão que verifica o estado do âmbito](./media/logic-apps-control-flow-run-steps-group-scopes/handle-true-false-branches.png)

7. Guarde a aplicação lógica.

A aplicação de lógica terminar agora expandido parece que este exemplo com todas as formas:

![Aplicação de lógica de terminar com âmbito](./media/logic-apps-control-flow-run-steps-group-scopes/scopes-overview.png)

## <a name="test-your-work"></a>Testar o seu trabalho

Na barra de ferramentas estruturador, escolha **executar**. Se todas as ações âmbito tiver êxito, receberá uma mensagem "O âmbito concluída com êxito". Se todas as ações de âmbito não tiver êxito, poderá receber uma mensagem de "Âmbito falhou". 

<a name="scopes-json"></a>

## <a name="json-definition"></a>Definição JSON

Se estiver a trabalhar na vista de código, pode definir uma estrutura de âmbito na definição de JSON da sua aplicação lógica em vez disso. Por exemplo, eis a definição de JSON para o acionador e ações na aplicação lógica anterior:

``` json
"triggers": {
  "Recurrence": {
    "type": "Recurrence",
    "recurrence": {
       "frequency": "Minute",
       "interval": 1
    },
  }
}
```

```json
"actions": {
  "If_scope_failed": {
    "type": "If",
    "actions": {
      "Scope_failed": {
        "type": "ApiConnection",
        "inputs": {
          "body": {
            "Body": "Scope failed",
            "Subject": "Scope failed",
            "To": "<your-email@domain.com>"
          },
          "host": {
            "connection": {
              "name": "@parameters('$connections')['outlook']['connectionId']"
            }
          },
          "method": "post",
          "path": "/Mail"
        },
        "runAfter": {}
      }
    },
    "else": {
      "actions": {
        "Scope_succeded": {
          "type": "ApiConnection",
          "inputs": {
            "body": {
              "Body": "None",
              "Subject": "Scope succeeded",
              "To": "<your-email@domain.com>"
            },
            "host": {
              "connection": {
               "name": "@parameters('$connections')['outlook']['connectionId']"
              }
            },
            "method": "post",
            "path": "/Mail"
          },
          "runAfter": {}
        }
      }
    },
    "expression": "@equals('@result(''Scope'')[0][''status'']', 'Failed, Aborted')",
    "runAfter": {
      "Scope": [
        "Succeeded"
      ]
    }
  },
  "Scope": {
    "type": "Scope",
    "actions": {
      "Get_route": {
        "type": "ApiConnection",
        "inputs": {
          "host": {
            "connection": {
              "name": "@parameters('$connections')['bingmaps']['connectionId']"
            }
          },
          "method": "get",
          "path": "/REST/V1/Routes/Driving",
          "queries": {
            "distanceUnit": "Mile",
            "optimize": "timeWithTraffic",
            "travelMode": "Driving",
            "wp.0": "<start>",
            "wp.1": "<end>"
          }
        },
        "runAfter": {}
      },
      "If_traffic_time_more_than_specified_time": {
        "type": "If",
        "actions": {
          "Send_mail_when_traffic_exceeds_10_minutes": {
            "type": "ApiConnection",
            "inputs": {
              "body": {
                 "Body": "Travel time:@{div(body('Get_route')?['travelDurationTraffic'], 60)} minutes",
                 "Subject": "Time to leave: Traffic more than 10 minutes",
                 "To": "<your-email@domain.com>"
              },
              "host": {
                "connection": {
                   "name": "@parameters('$connections')['outlook']['connectionId']"
                }
              },
              "method": "post",
              "path": "/Mail"
            },
            "runAfter": {}
          }
        },
        "expression": "@greater(body('Get_route')?['travelDurationTraffic'], 600)",
        "runAfter": {
          "Get_route": [
            "Succeeded"
          ]
        }
      }
    },
    "runAfter": {}
  }
}
```

## <a name="get-support"></a>Obter suporte

* Relativamente a dúvidas, visite o [fórum do Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Para submeter ou votar em funcionalidades e sugestões, visite o [site de comentários do utilizador do Azure Logic Apps](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Passos Seguintes

* [Executar passos com base numa condição (instruções condicionais)](../logic-apps/logic-apps-control-flow-conditional-statement.md)
* [Executar passos com base nos valores diferentes (comutador instruções)](../logic-apps/logic-apps-control-flow-switch-statement.md)
* [Executar e repita os passos (ciclos)](../logic-apps/logic-apps-control-flow-loops.md)
* [Executar ou merge passos paralelos (ramos)](../logic-apps/logic-apps-control-flow-branches.md)
