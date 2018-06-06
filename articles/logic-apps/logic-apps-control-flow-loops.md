---
title: Adicionar ciclos que repita ações ou processam matrizes - Azure Logic Apps | Microsoft Docs
description: Como criar ciclos que repita ações de fluxo de trabalho ou processam as matrizes das Logic Apps do Azure
services: logic-apps
ms.service: logic-apps
author: ecfan
ms.author: estfan
manager: cfowler
ms.date: 03/05/2018
ms.topic: article
ms.reviewer: klam, LADocs
ms.suite: integration
ms.openlocfilehash: 26db63ed7706bdff9eb5c037a906fbc252b83a9a
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34725960"
---
# <a name="create-loops-that-repeat-workflow-actions-or-process-arrays-in-azure-logic-apps"></a>Criar ciclos que repita ações de fluxo de trabalho ou processam as matrizes das Logic Apps do Azure

Para iterar através de matrizes na sua aplicação lógica, pode utilizar um ["Foreach" ciclo](#foreach-loop) ou um [sequencial "Foreach" ciclo](#sequential-foreach-loop). As iterações para um ciclo de "Foreach" standard executam em paralelo, durante a execução de iterações para um ciclo de "Foreach" sequencial um de cada vez. Para o número máximo de itens de matriz "Foreach" ciclos podem processar numa aplicação lógica única executar, consulte [limites e configuração](../logic-apps/logic-apps-limits-and-config.md). 

> [!TIP] 
> Se tiver um acionador que recebe uma matriz e pretende executar um fluxo de trabalho para cada item de matriz, pode *debatch* essa matriz com o [ **SplitOn** acionar propriedade](../logic-apps/logic-apps-workflow-actions-triggers.md#split-on-debatch). 
  
Repetir ações até que a condição é cumprida ou algum Estado é alterado, utilize um ["Até" ciclo](#until-loop). A aplicação lógica efetua primeiro todas as ações dentro do ciclo e, em seguida, verifica a condição como último passo. Se a condição for satisfeita, deixa de ciclo. Caso contrário, o ciclo repete-se. Para o número máximo de "Até" ciclos numa aplicação lógica única executar, consulte [limites e configuração](../logic-apps/logic-apps-limits-and-config.md). 

## <a name="prerequisites"></a>Pré-requisitos

* Uma subscrição do Azure. Se não tiver uma subscrição, [inscreva-se numa conta do Azure gratuita](https://azure.microsoft.com/free/). 

* Conhecimento básico sobre [como criar aplicações lógicas](../logic-apps/quickstart-create-first-logic-app-workflow.md)

<a name="foreach-loop"></a>

## <a name="foreach-loop"></a>Ciclo de "Foreach"

Repetir ações para cada item numa matriz, utilize um ciclo de "Foreach" no seu fluxo de trabalho de aplicação lógica. Pode incluir várias ações num ciclo "Foreach" e pode ser aninhado "Foreach" ciclos no interior entre si. Por predefinição, os ciclos num ciclo "Foreach" padrão são executadas em paralelo. Para obter o número máximo de paralelo ciclos que podem executar ciclos "Foreach", consulte [limites e configuração](../logic-apps/logic-apps-limits-and-config.md).

> [!NOTE] 
> Um ciclo de "Foreach" funciona apenas com uma matriz e utilizam as ações do ciclo de `@item()` referência para processar cada item na matriz. Se especificar dados que não está a ser uma matriz, o fluxo de trabalho de aplicação lógica falha. 

Por exemplo, esta aplicação lógica envia-lhe um resumo diário do feed RSS de um Web site. A aplicação utiliza um ciclo de "Foreach" que envia uma mensagem de e-mail cada novo item encontrado.

1. [Criar esta aplicação de lógica de exemplo](../logic-apps/quickstart-create-first-logic-app-workflow.md) com uma conta do Outlook.com ou Outlook do Office 365.

2. Entre o RSS acionar e enviar correio eletrónico ação, adicione um ciclo de "Foreach". 

   Para adicionar um ciclo de entre os passos, mova o ponteiro sobre na seta para onde pretende adicionar o ciclo. 
   Escolha o **sinal** (**+**) que é apresentado, em seguida, escolha **adicionar um para cada**.

   ![Adicionar um ciclo de "Foreach" entre os passos](media/logic-apps-control-flow-loops/add-for-each-loop.png)

3. Agora, crie o ciclo. Em **selecionar uma saída dos passos anteriores** depois do **adicionar conteúdo dinâmico** é apresentada a lista, selecione o **Feed ligações** matriz, o que é o resultado de Acionador RSS. 

   ![Selecione na lista de conteúdo dinâmica](media/logic-apps-control-flow-loops/for-each-loop-dynamic-content-list.png)

   > [!NOTE] 
   > Pode selecionar *apenas* matriz produz do passo anterior.

   A matriz selecionada aparece agora aqui:

   ![Selecione matriz](media/logic-apps-control-flow-loops/for-each-loop-select-array.png)

4. Para efetuar uma ação cada item de matriz, arraste o **enviar um e-mail** ação para o **para cada** ciclo. 

   A aplicação lógica pode ter um aspeto semelhante neste exemplo:

   ![Adicione passos à "Foreach" de ciclo](media/logic-apps-control-flow-loops/for-each-loop-with-step.png)

5. Guarde a aplicação lógica. Para testar manualmente a sua aplicação lógica, na barra de ferramentas estruturador, escolha **executar**.

<a name="for-each-json"></a>

## <a name="foreach-loop-definition-json"></a>Definição de ciclo de "Foreach" (JSON)

Se estiver a trabalhar na vista de código para a sua aplicação lógica, pode definir o `Foreach` cíclicas na definição de JSON da sua aplicação lógica em vez disso, por exemplo:

``` json
"actions": {
    "myForEachLoopName": {
        "type": "Foreach",
        "actions": {
            "Send_an_email": {
                "type": "ApiConnection",
                "inputs": {
                    "body": {
                        "Body": "@{item()}",
                        "Subject": "New CNN post @{triggerBody()?['publishDate']}",
                        "To": "me@contoso.com"
                    },
                    "host": {
                        "api": {
                            "runtimeUrl": "https://logic-apis-westus.azure-apim.net/apim/office365"
                        },
                        "connection": {
                            "name": "@parameters('$connections')['office365']['connectionId']"
                        }
                    },
                    "method": "post",
                    "path": "/Mail"
                },
                "runAfter": {}
            }
        },
        "foreach": "@triggerBody()?['links']",
        "runAfter": {},
    }
},
```

<a name="sequential-foreach-loop"></a>

## <a name="foreach-loop-sequential"></a>Ciclo de "Foreach": sequenciais

Por predefinição, cada ciclo num ciclo "Foreach" é executado em paralelo para cada item de matriz. Para cada ciclo são executados sequencialmente, defina o **sequencial** opção no seu ciclo de "Foreach".

1. Canto superior direito o ciclo, escolha **reticências** (**...** ) > **Definições**.

   ![Ciclo de "Foreach", escolha "..." > "Definições"](media/logic-apps-control-flow-loops/for-each-loop-settings.png)

2. Ative o **sequencial** definição, em seguida, escolha **feito**.

   ![Ative a definição sequenciais do ciclo de "Foreach"](media/logic-apps-control-flow-loops/for-each-loop-sequential-setting.png)

Também pode definir o **operationOptions** parâmetro `Sequential` na definição de JSON da sua aplicação lógica. Por exemplo:

``` json
"actions": {
    "myForEachLoopName": {
        "type": "Foreach",
        "actions": {
            "Send_an_email": {               
            }
        },
        "foreach": "@triggerBody()?['links']",
        "runAfter": {},
        "operationOptions": "Sequential"
    }
},
```

<a name="until-loop"></a>

## <a name="until-loop"></a>"Até" ciclo
  
Repetir ações até que a condição é cumprida ou algum Estado é alterado, utilize um ciclo "Até" no seu fluxo de trabalho de aplicação lógica. Eis alguns casos de utilização comum, onde pode utilizar um ciclo "Até":

* Quando obtiver resposta que pretende que a chamada um ponto final.
* Criar um registo numa base de dados, aguarde até que um campo específico, em que seja aprovado registo e continuar o processamento. 

Por exemplo, às 8:00 por dia, esta aplicação lógica incrementa uma variável até que o valor da variável de é igual a 10. Em seguida, a aplicação lógica envia uma mensagem de e-mail confirma que o valor atual. Embora este exemplo utiliza o Outlook do Office 365, pode utilizar qualquer fornecedor de correio eletrónico suportado pelo Logic Apps ([rever os conectores listam aqui](https://docs.microsoft.com/connectors/)). Se utilizar outra conta de e-mail, os passos gerais são os mesmos, mas a IU poderá ser ligeiramente diferente. 

1. Criar uma aplicação lógica em branco. No Designer de aplicação lógica, procure "recurrence" e selecione este acionador: **agenda - periodicidade** 

   ![Adicionar "Agenda - Recurrence" acionador](./media/logic-apps-control-flow-loops/do-until-loop-add-trigger.png)

2. Especifique quando o acionador desencadeado ao definir o intervalo, a frequência e a hora do dia. Para definir a hora, escolha **Mostrar opções avançadas**.

   ![Adicionar "Agenda - Recurrence" acionador](./media/logic-apps-control-flow-loops/do-until-loop-set-trigger-properties.png)

   | Propriedade | Valor |
   | -------- | ----- |
   | **Intervalo** | 1 | 
   | **Frequência** | Dia |
   | **At these hours** (A estas horas) | 8 |
   ||| 

3. Sob o acionador, escolha **novo passo** > **adicionar uma ação**. Procure "variáveis" e, em seguida, selecione esta ação: **variáveis - inicializar variável**

   ![Adicione "Variáveis - inicializar variável" ação](./media/logic-apps-control-flow-loops/do-until-loop-add-variable.png)

4. Configure a variável com estes valores:

   ![Defina as propriedades variável](./media/logic-apps-control-flow-loops/do-until-loop-set-variable-properties.png)

   | Propriedade | Valor | Descrição |
   | -------- | ----- | ----------- |
   | **Nome** | Limite | Nome da variável | 
   | **Tipo** | Número inteiro | Tipo de dados da variável | 
   | **Valor** | 0 | A variável do valor inicial | 
   |||| 

5. Sob o **inicializar variável** ação, escolha **novo passo** > **mais**. Selecione este ciclo: **adicionar um efetue até**

   ![Adicionar "fazer até" de ciclo](./media/logic-apps-control-flow-loops/do-until-loop-add-until-loop.png)

6. Criar uma condição de saída do ciclo, selecionando o **limite** variável e o **é igual** operador. Introduza **10** como o valor de comparação.

   ![Criar uma condição de saída para parar de ciclo](./media/logic-apps-control-flow-loops/do-until-loop-settings.png)

7. Dentro do ciclo, escolha **adicionar uma ação**. Procure "variáveis" e, em seguida, adicione esta ação: **variáveis - variável de incremento**

   ![Adicionar ação para incrementando variável](./media/logic-apps-control-flow-loops/do-until-loop-increment-variable.png)

8. Para **nome**, selecione o **limite** variável. Para **valor**, introduza "1". 

   ![Incremento "Limite" por 1](./media/logic-apps-control-flow-loops/do-until-loop-increment-variable-settings.png)

9. Em mas fora do ciclo, adicione uma ação que envia correio eletrónico. Se tal lhe for solicitado, inicie sessão na sua conta de e-mail.

   ![Adicionar ação que envia correio eletrónico](media/logic-apps-control-flow-loops/do-until-loop-send-email.png)

10. Defina as propriedades do e-mail. Adicionar o **limite** variável para o assunto. Dessa forma, pode confirmar que valor atual da variável cumpre a condição especificada, por exemplo:

    ![Configurar propriedades de correio eletrónico](./media/logic-apps-control-flow-loops/do-until-loop-send-email-settings.png)

    | Propriedade | Valor | Descrição |
    | -------- | ----- | ----------- | 
    | **Para** | *<email-address@domain>* | endereço de e-mail do destinatário. Para fins de teste, utilize o seu próprio endereço de correio eletrónico. | 
    | **Assunto** | O valor atual para "Limite de" é **limite** | Especifique o assunto do e-mail. Neste exemplo, certifique-se de que inclui o **limite** variável. | 
    | **Corpo** | <*email-content*> | Especifique o conteúdo de mensagem de e-mail que pretende enviar. Neste exemplo, introduza qualquer texto que quiser. | 
    |||| 

11. Guarde a aplicação lógica. Para testar manualmente a sua aplicação lógica, na barra de ferramentas estruturador, escolha **executar**.

    Depois da lógica de entra em execução, receberá um e-mail com o conteúdo que especificou:

    ![E-mail recebido](./media/logic-apps-control-flow-loops/do-until-loop-sent-email.png)

## <a name="prevent-endless-loops"></a>Impedir ciclos endless

Um ciclo "Até" tem limites de predefinição parar a execução se qualquer uma das seguintes condições:

| Propriedade | Valor predefinido | Descrição | 
| -------- | ------------- | ----------- | 
| **Contagem** | 60 | O número máximo de ciclos executar antes de sai do ciclo. A predefinição é 60 ciclos. | 
| **Tempo limite** | PT1H | A quantidade máxima de tempo para executar um ciclo antes do ciclo sai. A predefinição é uma hora e é especificada no formato ISO 8601. <p>O valor de tempo limite é avaliado para cada ciclo de ciclo. Se qualquer ação no ciclo demora mais que o limite de tempo limite, o ciclo atual não parar, mas o próximo ciclo não iniciar, porque não é cumprida a condição de limite. | 
|||| 

Para alterar estes limites predefinido, escolher **Mostrar opções avançadas** na forma de ação de ciclo.

<a name="until-json"></a>

## <a name="until-definition-json"></a>"Até" definição (JSON)

Se estiver a trabalhar na vista de código para a sua aplicação lógica, pode definir um `Until` cíclicas na definição de JSON da sua aplicação lógica em vez disso, por exemplo:

``` json
"actions": {
    "Initialize_variable": {
        // Definition for initialize variable action
    },
    "Send_an_email": {
        // Definition for send email action
    },
    "Until": {
        "type": "Until",
        "actions": {
            "Increment_variable": {
                "type": "IncrementVariable",
                "inputs": {
                    "name": "Limit",
                    "value": 1
                },
                "runAfter": {}
            }
        },
        "expression": "@equals(variables('Limit'), 10)",
        // To prevent endless loops, an "Until" loop 
        // includes these default limits that stop the loop. 
        "limit": { 
            "count": 60,
            "timeout": "PT1H"
        },
        "runAfter": {
            "Initialize_variable": [
                "Succeeded"
            ]
        },
    }
},
```

Noutro exemplo, este ciclo "Até" chama um ponto final de HTTP que cria um recurso e para quando devolve o corpo da resposta HTTP com o estado de "Concluída". Para evitar os ciclos endless, o ciclo também deixa de se qualquer uma das seguintes condições:

* O ciclo foi executada 10 vezes especificado pelo `count` atributo. A predefinição é 60 vezes. 
* O ciclo tentou executar de duas horas, conforme especificado pelo `timeout` atributo no formato ISO 8601. A predefinição é uma hora.
  
``` json
"actions": {
    "myUntilLoopName": {
        "type": "Until",
        "actions": {
            "Create_new_resource": {
                "type": "Http",
                "inputs": {
                    "body": {
                        "resourceId": "@triggerBody()"
                    },
                    "url": "https://domain.com/provisionResource/create-resource",
                    "body": {
                        "resourceId": "@triggerBody()"
                    }
                },
                "runAfter": {},
                "type": "ApiConnection"
            }
        },
        "expression": "@equals(triggerBody(), 'Completed')",
        "limit": {
            "count": 10,
            "timeout": "PT2H"
        },
        "runAfter": {}
    }
},
```

## <a name="get-support"></a>Obter suporte

* Relativamente a dúvidas, visite o [fórum do Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Para submeter ou votar em funcionalidades e sugestões, [site de comentários do utilizador do Azure Logic Apps](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Passos Seguintes

* [Executar passos com base numa condição (instruções condicionais)](../logic-apps/logic-apps-control-flow-conditional-statement.md)
* [Executar passos com base nos valores diferentes (comutador instruções)](../logic-apps/logic-apps-control-flow-switch-statement.md)
* [Executar ou merge passos paralelos (ramos)](../logic-apps/logic-apps-control-flow-branches.md)
* [Executar passos com base no estado da ação agrupada (âmbitos)](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md)
