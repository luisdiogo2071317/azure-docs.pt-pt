---
title: Adicionar loops que repetir ações ou processam matrizes - Azure Logic Apps | Documentos da Microsoft
description: Como criar loops que repetir ações de fluxo de trabalho ou processam matrizes no Azure Logic Apps
services: logic-apps
ms.service: logic-apps
author: ecfan
ms.author: estfan
manager: jeconnoc
ms.date: 03/05/2018
ms.topic: article
ms.reviewer: klam, LADocs
ms.suite: integration
ms.openlocfilehash: 5ba5e5abef4ebdc58c44cbe7f5ba584efe8abfc7
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/30/2018
ms.locfileid: "50233111"
---
# <a name="create-loops-that-repeat-workflow-actions-or-process-arrays-in-azure-logic-apps"></a>Criar ciclos que repetir ações de fluxo de trabalho ou processam matrizes no Azure Logic Apps

Para iterar através de matrizes na sua aplicação lógica, pode utilizar um ["Foreach" loop](#foreach-loop) ou uma [sequencial "Foreach" loop](#sequential-foreach-loop). As iterações no loop "Foreach" padrão executam em paralelo, enquanto as iterações no loop seqüencial "Foreach" executam um de cada vez. Para o número máximo de itens de matriz que loops "Foreach" podem processar na execução de aplicação lógica única, consulte [limites e configuração](../logic-apps/logic-apps-limits-and-config.md). 

> [!TIP] 
> Se tiver um acionador que recebe uma matriz e quer executar um fluxo de trabalho para cada item da matriz, pode *debatch* essa matriz com o [ **SplitOn** acionar propriedade](../logic-apps/logic-apps-workflow-actions-triggers.md#split-on-debatch). 
  
Para repetir ações até que a condição é cumprida ou algum Estado foi alterado, utilize um ["loop até"](#until-loop). A aplicação lógica pela primeira vez executa todas as ações dentro do loop e, em seguida, verifica a condição como última etapa. Se a condição for cumprida, o loop para. Caso contrário, o loop se repetir. Para o número máximo de "Até" loops numa aplicação lógica única executar, veja [limites e configuração](../logic-apps/logic-apps-limits-and-config.md). 

## <a name="prerequisites"></a>Pré-requisitos

* Uma subscrição do Azure. Se não tiver uma subscrição, [inscreva-se numa conta do Azure gratuita](https://azure.microsoft.com/free/). 

* Conhecimento básico sobre [como criar aplicações lógicas](../logic-apps/quickstart-create-first-logic-app-workflow.md)

<a name="foreach-loop"></a>

## <a name="foreach-loop"></a>Loop "Foreach"

Para repetir ações para cada item numa matriz, use um loop "Foreach" no seu fluxo de trabalho de aplicação lógica. Pode incluir várias ações num loop "Foreach" e pode aninhar loops de "Foreach" dentro de uns dos outros. Por predefinição, os ciclos num loop de "Foreach" padrão são executadas em paralelo. Para o número máximo de paralelo ciclos esse "Foreach" podem executar loops, consulte [limites e configuração](../logic-apps/logic-apps-limits-and-config.md).

> [!NOTE] 
> Um loop "Foreach" funciona apenas com uma matriz e ações no loop utilizam o `@item()` referência para processar cada item na matriz. Se especificar dados que não esteja numa matriz, o fluxo de trabalho de aplicação lógica falha. 

Por exemplo, esta aplicação lógica envia-lhe um resumo diário do feed RSS de um site. A aplicação utiliza um loop "Foreach" que envia um e-mail cada novo item encontrado.

1. [Criar esta aplicação de lógica de exemplo](../logic-apps/quickstart-create-first-logic-app-workflow.md) com uma conta do Outlook.com ou o Outlook do Office 365.

2. Entre o RSS acionar e enviar a ação de e-mail, adicionar um loop "Foreach". 

   Para adicionar um ciclo entre etapas, mova o ponteiro sobre a seta para onde pretende adicionar o loop. 
   Escolha o **sinal** (**+**) que aparece, em seguida, escolha **adicionar um para cada**.

   ![Adicionar um loop "Foreach" entre passos](media/logic-apps-control-flow-loops/add-for-each-loop.png)

3. Agora, crie o loop. Sob **selecionar uma saída dos passos anteriores** depois do **adicionar conteúdo dinâmico** é apresentada a lista, selecione o **ligações de Feed** matriz, o que é o resultado do acionador RSS. 

   ![Selecione na lista de conteúdo dinâmica](media/logic-apps-control-flow-loops/for-each-loop-dynamic-content-list.png)

   > [!NOTE] 
   > Pode selecionar *apenas* matriz gera a saída do passo anterior.

   A matriz de selecionado aparece agora aqui:

   ![Selecione a matriz](media/logic-apps-control-flow-loops/for-each-loop-select-array.png)

4. Para executar uma ação em cada item da matriz, arraste o **enviar um e-mail** ação para o **para cada** loop. 

   A aplicação lógica pode ser algo semelhante a este exemplo:

   ![Adicione passos à loop "Foreach"](media/logic-apps-control-flow-loops/for-each-loop-with-step.png)

5. Guarde a aplicação lógica. Para testar manualmente a sua aplicação lógica, na barra de ferramentas da estruturador, escolha **executar**.

<a name="for-each-json"></a>

## <a name="foreach-loop-definition-json"></a>Definição de loop "Foreach" (JSON)

Se estiver a trabalhar na vista de código para a aplicação lógica, pode definir o `Foreach` um loop na definição de JSON da sua aplicação lógica em vez disso, por exemplo:

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

## <a name="foreach-loop-sequential"></a>Loop "Foreach": sequenciais

Por predefinição, cada ciclo num loop "Foreach" é executada em paralelo para cada item da matriz. Para cada ciclo são executados sequencialmente, defina o **Sequential** opção em seu loop de "Foreach".

1. No canto direito superior do ciclo, escolha **reticências** (**...** ) > **Definições**.

   ![No loop "Foreach", escolha "..." > "Definições"](media/logic-apps-control-flow-loops/for-each-loop-settings.png)

2. Ativar a **Sequential** definir, em seguida, escolha **feito**.

   ![Ativar definição de sequenciais do ciclo de "Foreach"](media/logic-apps-control-flow-loops/for-each-loop-sequential-setting.png)

Também pode definir o **operationOptions** parâmetro para `Sequential` na definição de JSON da sua aplicação lógica. Por exemplo:

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

## <a name="until-loop"></a>"Até" loop
  
Para repetir ações até que a condição é cumprida ou algum Estado foi alterado, use um loop "Até" no seu fluxo de trabalho de aplicação lógica. Aqui estão alguns casos de utilização comuns onde pode usar um loop "Até":

* Chame um ponto final até obter a resposta que pretende.
* Criar um registo numa base de dados, aguarde até que um campo específico em que o registo seja aprovado e continue o processamento. 

Por exemplo, às 8:00, todos os dias, esta aplicação lógica incrementa uma variável até que o valor da variável é igual a 10. Em seguida, a aplicação lógica envia um e-mail que confirma o valor atual. Embora este exemplo utiliza o Outlook do Office 365, pode utilizar qualquer fornecedor de e-mail suportada pelo Logic Apps ([reveja a lista de conectores aqui](https://docs.microsoft.com/connectors/)). Se utilizar outra conta de e-mail, os passos gerais são os mesmos, mas a IU poderá ser ligeiramente diferente. 

1. Criar uma aplicação lógica em branco. No Estruturador da aplicação lógica, procure "recurrence" e selecione este acionador: **agenda - periodicidade** 

   ![Adicionar acionador "– a periodicidade da agenda"](./media/logic-apps-control-flow-loops/do-until-loop-add-trigger.png)

2. Especifique quando o acionador é acionado ao definir o intervalo, a frequência e a hora do dia. Para definir a hora, escolha **Mostrar opções avançadas**.

   ![Adicionar acionador "– a periodicidade da agenda"](./media/logic-apps-control-flow-loops/do-until-loop-set-trigger-properties.png)

   | Propriedade | Valor |
   | -------- | ----- |
   | **Intervalo** | 1 | 
   | **Frequência** | Dia |
   | **At these hours** (A estas horas) | 8 |
   ||| 

3. No acionador, escolha **novo passo** > **adicionar uma ação**. Procure "variables" e, em seguida, selecione a ação: **Variables - Initialize variable**

   ![Adicionar "Variables - Initialize variable" ação](./media/logic-apps-control-flow-loops/do-until-loop-add-variable.png)

4. Configure a sua variável com estes valores:

   ![Definir propriedades de variável](./media/logic-apps-control-flow-loops/do-until-loop-set-variable-properties.png)

   | Propriedade | Valor | Descrição |
   | -------- | ----- | ----------- |
   | **Nome** | Limite | Nome da variável | 
   | **Tipo** | Número inteiro | Tipo de dados da variável | 
   | **Valor** | 0 | A variável do valor inicial | 
   |||| 

5. Sob o **Initialize variable** ação, escolha **novo passo** > **mais**. Selecione esse loop: **adicionar um fazer até**

   ![Adicionar ciclo "fazer até"](./media/logic-apps-control-flow-loops/do-until-loop-add-until-loop.png)

6. Criar a condição de saída do ciclo, selecionando o **limite** variável e o **equivale** operador. Introduza **10** como o valor de comparação.

   ![Criar a condição de saída para interromper o loop](./media/logic-apps-control-flow-loops/do-until-loop-settings.png)

7. Dentro do loop, escolha **adicionar uma ação**. Procure "variables" e, em seguida, adicione esta ação: **Variables - incrementar variaável**

   ![Adicionar ação por incrementar a variável](./media/logic-apps-control-flow-loops/do-until-loop-increment-variable.png)

8. Para **Name**, selecione a **limite** variável. Para **valor**, introduza "1". 

   ![Incremento "Limite" em 1](./media/logic-apps-control-flow-loops/do-until-loop-increment-variable-settings.png)

9. Em mas fora do loop, adicione uma ação que envia um e-mail. Se tal lhe for solicitado, inicie sessão na sua conta de e-mail.

   ![Adicionar ação que envia um e-mail](media/logic-apps-control-flow-loops/do-until-loop-send-email.png)

10. Defina as propriedades da mensagem de e-mail. Adicionar a **limite** variável para o assunto. Dessa forma, pode confirmar que valor atual da variável cumpre a condição especificada, por exemplo:

    ![Configurar propriedades de e-mail](./media/logic-apps-control-flow-loops/do-until-loop-send-email-settings.png)

    | Propriedade | Valor | Descrição |
    | -------- | ----- | ----------- | 
    | **Para** | *<email-address@domain>* | endereço de e-mail do destinatário. Para fins de teste, utilize o seu endereço de e-mail. | 
    | **Assunto** | O valor atual para "Limite de" é **limite** | Especifique o assunto do e-mail. Neste exemplo, certifique-se de que inclui a **limite** variável. | 
    | **Corpo** | <*email-content*> | Especifique o conteúdo da mensagem de e-mail que pretende enviar. Neste exemplo, introduza qualquer texto que desejar. | 
    |||| 

11. Guarde a aplicação lógica. Para testar manualmente a sua aplicação lógica, na barra de ferramentas da estruturador, escolha **executar**.

    Depois da lógica entrar em execução, receberá um e-mail com o conteúdo que especificou:

    ![E-mail recebido](./media/logic-apps-control-flow-loops/do-until-loop-sent-email.png)

## <a name="prevent-endless-loops"></a>Impedir que os loops intermináveis

Um loop "Até" tem limites predefinidos que pare a execução se alguma das seguintes condições:

| Propriedade | Valor predefinido | Descrição | 
| -------- | ------------- | ----------- | 
| **Contagem** | 60 | O número máximo de loops, que são executadas antes do loop é encerrado. A predefinição é 60 ciclos. | 
| **Tempo limite** | PT1H | A quantidade máxima de tempo para executar um loop antes do loop é encerrado. A predefinição é de uma hora e é especificada no formato ISO 8601. <p>O valor de tempo limite é avaliado para cada ciclo de loop. Se qualquer ação no loop demora mais tempo do que o limite de tempo limite, não para o ciclo de atual, mas o próximo ciclo não iniciar, porque a condição de limite não for cumprida. | 
|||| 

Para alterar esses limites predefinidos, escolha **Mostrar opções avançadas** na forma da ação de loop.

<a name="until-json"></a>

## <a name="until-definition-json"></a>"Até" definição (JSON)

Se estiver a trabalhar na vista de código para a aplicação lógica, pode definir um `Until` um loop na definição de JSON da sua aplicação lógica em vez disso, por exemplo:

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

Noutro exemplo, esse loop "Até" chama um ponto final HTTP que cria um recurso e termina quando o corpo da resposta HTTP retorna com o estado de "Concluído". Para impedir que os loops intermináveis, o loop para também se alguma das seguintes condições:

* O loop foi executado 10 vezes conforme especificado pelo `count` atributo. A predefinição é 60 vezes. 
* O loop tentou executar durante duas horas como especificado pelo `timeout` atributo no formato ISO 8601. A predefinição é de uma hora.
  
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
* Para submeter ou votar em recursos e sugestões, [site de comentários de utilizadores do Azure Logic Apps](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Passos Seguintes

* [Execute os passos com base numa condição (instruções condicionais)](../logic-apps/logic-apps-control-flow-conditional-statement.md)
* [Execute os passos com base nos valores diferentes (declarações do comutador)](../logic-apps/logic-apps-control-flow-switch-statement.md)
* [Executar ou unir a passos paralelos (ramos)](../logic-apps/logic-apps-control-flow-branches.md)
* [Execute os passos com base no estado da ação agrupados (âmbitos)](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md)
