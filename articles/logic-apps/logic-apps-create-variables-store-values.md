---
title: Criar variáveis para guardar valores - Azure Logic Apps | Microsoft Docs
description: Para guardar e gerir valores através da criação de variáveis no Azure Logic Apps
services: logic-apps
author: ecfan
manager: jeconnoc
ms.author: estfan
ms.topic: article
ms.date: 05/30/2018
ms.service: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
ms.openlocfilehash: 0efce9fbbbd241f335f08bb258b6ba343982fdb9
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35299193"
---
# <a name="create-variables-for-saving-and-managing-values-in-azure-logic-apps"></a>Criar variáveis para guardar e gerir os valores no Azure Logic Apps

Este artigo mostra como pode armazenar e trabalhar com valores em toda a sua aplicação lógica através da criação de variáveis. Por exemplo, as variáveis podem ajudá-lo contabilizar o número de vezes que é executado um ciclo. Quando iterating através de uma matriz ou a verificação de uma matriz de um item específico, pode utilizar uma variável para referenciar o número de índice para cada item de matriz. 

Pode criar variáveis para tipos de dados, tais como número inteiro, vírgula flutuante, booleano, string, matriz e objeto. Depois de criar uma variável, pode realizar outras tarefas, por exemplo:

* Obter ou valor da variável de referência.
* Aumentar ou diminuir a variável por um valor constante, também conhecido como *incremento* e *diminuir*.
* Atribua um valor diferente para a variável.
* Inserir ou *acrescentar* valor da variável de como a hora da última numa cadeia ou matriz.

Variáveis existem e estão globais apenas dentro da instância da aplicação lógica que as cria. Além disso, estes sem mantenham em todas as iterações de ciclo dentro de uma instância da aplicação lógica. Quando referenciar uma variável, utilize o nome da variável como o token, não a ação nome, o qual é a forma habitual saídas de uma ação de referência.

Se não tiver uma subscrição do Azure ainda, <a href="https://azure.microsoft.com/free/" target="_blank">inscrever-se numa conta do Azure gratuita</a>. 

## <a name="prerequisites"></a>Pré-requisitos

Para seguir este artigo, seguem-se os itens que precisa de:

* A aplicação de lógica onde pretende criar uma variável 

  Se estiver familiarizado com as logic apps, reveja [que é o Azure Logic Apps](../logic-apps/logic-apps-overview.md) e [início rápido: criar a sua primeira aplicação de lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md).

* A [acionador](../logic-apps/logic-apps-overview.md#logic-app-concepts) como o primeiro passo na sua aplicação lógica 

  Antes de poder adicionar ações para criar e trabalhar com variáveis, a sua aplicação lógica deve começar com um acionador.

<a name="create-variable"></a>

## <a name="initialize-variable"></a>Inicializar variável

Pode criar uma variável e declarar o respetivo tipo de dados e o valor inicial - todos os dentro de uma ação na sua aplicação lógica. Apenas podem declarar as variáveis a nível global, não no âmbitos, condições e ciclos. 

1. No <a href="https://portal.azure.com" target="_blank">portal do Azure</a> ou Visual Studio, abra a aplicação lógica no Designer de aplicação lógica. 

   Este exemplo utiliza o portal do Azure e uma aplicação lógica com um acionador existente.

2. Na sua aplicação lógica, sob o passo em que pretende adicionar uma variável, siga um destes passos: 

   * Para adicionar uma ação em último passo, escolha **novo passo** > **adicionar uma ação**.

     ![Adicionar ação](./media/logic-apps-create-variables-store-values/add-action.png)

   * Para adicionar uma ação entre os passos, mova o rato sobre a seta para a ligação, pelo que é apresentado o sinal de adição (+). 
   Escolha o sinal de adição e, em seguida, escolha **adicionar uma ação**.

3. Na caixa de pesquisa, introduza "variáveis" como o filtro. Na lista de ações, selecione **variáveis - inicializar variável**.

   ![Selecione a ação](./media/logic-apps-create-variables-store-values/select-initialize-variable-action.png)

4. Forneça estas informações para a variável de:

   | Propriedade | Necessário | Valor |  Descrição |
   |----------|----------|-------|--------------|
   | Nome | Sim | <*nome da variável*> | O nome da variável incrementar | 
   | Tipo | Sim | <*tipo de variável*> | O tipo de dados para a variável | 
   | Valor | Não | <*valor inicial*> | O valor iniciar da variável <p><p>**Sugestão**: embora opcional, definir este valor como uma melhor prática, para que saiba sempre o valor inicial para a variável. | 
   ||||| 

   ![Inicializar variável](./media/logic-apps-create-variables-store-values/initialize-variable.png)

5. Agora continue a adicionar as ações que pretende. Quando tiver terminado, na barra de ferramentas estruturador, escolha **guardar**.

Se mudar do estruturador no Editor de vista de código, eis a forma como o **inicializar variável** ação apresentado no interior a definição da aplicação lógica, que está no formato de JavaScript Object Notation (JSON):

```json
"actions": {
   "Initialize_variable": {
      "type": "InitializeVariable",
      "inputs": {
         "variables": [ {
               "name": "Count",
               "type": "Integer",
               "value": 0
          } ]
      },
      "runAfter": {}
   }
},
```

Seguem-se exemplos de alguns tipos de variável:

*Variável de cadeia*

```json
"actions": {
   "Initialize_variable": {
      "type": "InitializeVariable",
      "inputs": {
         "variables": [ {
               "name": "myStringVariable",
               "type": "String",
               "value": "lorem ipsum"
          } ]
      },
      "runAfter": {}
   }
},
```

*Variável booleano*

```json
"actions": {
   "Initialize_variable": {
      "type": "InitializeVariable",
      "inputs": {
         "variables": [ {
               "name": "myBooleanVariable",
               "type": "Boolean",
               "value": false
          } ]
      },
      "runAfter": {}
   }
},
```

*Matriz com números inteiros*

```json
"actions": {
   "Initialize_variable": {
      "type": "InitializeVariable",
      "inputs": {
         "variables": [ {
               "name": "myArrayVariable",
               "type": "Array",
               "value": [1, 2, 3]
          } ]
      },
      "runAfter": {}
   }
},
```

*Matriz de cadeias de*

```json
"actions": {
   "Initialize_variable": {
      "type": "InitializeVariable",
      "inputs": {
         "variables": [ {
               "name": "myArrayVariable",
               "type": "Array",
               "value": ["red", "orange", "yellow"]
          } ]
      },
      "runAfter": {}
   }
},
```

<a name="get-value"></a>

## <a name="get-the-variables-value"></a>Obter o valor da variável

Para obter ou conteúdo de uma variável de referência, também pode utilizar o [variables() função](../logic-apps/workflow-definition-language-functions-reference.md#variables) no Designer de aplicação lógica e o editor de vista de código.
Quando referenciar uma variável, utilize o nome da variável como o token, não a ação nome, o qual é a forma habitual saídas de uma ação de referência. 

Por exemplo, esta expressão obtém os itens da variável de matriz [criado anteriormente neste artigo](#append-value) utilizando o **variables()** função. O **string()** função devolve o conteúdo da variável no formato de cadeia: `"1, 2, 3, red"`

```json
@{string(variables('myArrayVariable'))}
```

<a name="increment-value"></a>

## <a name="increment-variable"></a>Incrementar variaável 

Para aumentar ou *incremento* uma variável por um valor constante, adicione o **variáveis - variável de incremento** ação à sua aplicação lógica. Esta ação funciona apenas com variáveis de número inteiro e número de vírgula flutuante.

1. No Designer de aplicação lógica, sob o passo em que pretende aumentar uma variável existente, escolha **novo passo** > **adicionar uma ação**. 

   Por exemplo, esta aplicação lógica já tem um acionador e uma ação que criou uma variável. Por isso, adicione uma ação de novo nestes passos:

   ![Adicionar ação](./media/logic-apps-create-variables-store-values/add-increment-variable-action.png)

   Para adicionar uma ação entre passos existentes, mova o rato sobre a seta para a ligação para que o sinal de adição (+) é apresentado. Escolha o sinal de adição e, em seguida, escolha **adicionar uma ação**.

2. Na caixa de pesquisa, introduza "variável de incremento" como o filtro. Na lista de ações, selecione **variáveis - variável de incremento**.

   ![Selecione a ação "Variável de incremento"](./media/logic-apps-create-variables-store-values/select-increment-variable-action.png)

3. Forneça estas informações para incrementando a variável de:

   | Propriedade | Necessário | Valor |  Descrição |
   |----------|----------|-------|--------------|
   | Nome | Sim | <*nome da variável*> | O nome da variável incrementar | 
   | Valor | Não | <*valor de incremento*> | O valor utilizado para incrementando a variável. O valor predefinido é um. <p><p>**Sugestão**: embora opcional, definir este valor como uma melhor prática, para que saiba sempre o valor específico de incrementando a variável. | 
   |||| 

   Por exemplo: 
   
   ![Exemplo de valor de incremento](./media/logic-apps-create-variables-store-values/increment-variable-action-information.png)

4. Quando tiver terminado, na barra de ferramentas estruturador, escolha **guardar**. 

Se mudar do estruturador no Editor de vista de código, eis a forma como o **variável de incremento** ação apresentado no interior a definição da aplicação lógica, que está no formato JSON:

```json
"actions": {
   "Increment_variable": {
      "type": "IncrementVariable",
      "inputs": {
         "name": "Count",
         "value": 1
      },
      "runAfter": {}
   }
},
```

## <a name="example-create-loop-counter"></a>Exemplo: Criar o contador de ciclo

As variáveis são frequentemente utilizadas para o número de vezes que é executado um ciclo de contagem. Este exemplo mostra como criar e utilizar variáveis para esta tarefa através da criação de um ciclo de contagens de anexos de uma mensagem de e-mail.

1. No portal do Azure, crie uma aplicação lógica em branco. Adicione um acionador que verifica a existência de novo e-mail e quaisquer anexos. 

   Este exemplo utiliza o acionador do Outlook do Office 365 para **quando chega um novo e-mail**. 
   Pode configurar este acionador a acionar apenas quando a mensagem de e-mail tem de anexos.
   No entanto, pode utilizar qualquer conector que verifica para novas mensagens de correio eletrónico com anexos, tais como o conector do Outlook.com.

2. O acionador, escolha **Mostrar opções avançadas**. Para que o acionador, verifique a existência de anexos e transmita os anexos para fluxo de trabalho da sua aplicação lógica, selecione **Sim** para estas propriedades:
   
   * **Tem Anexo** 
   * **Incluir Anexos** 

   ![Procurar e incluir anexos](./media/logic-apps-create-variables-store-values/check-include-attachments.png)

3. Adicionar o [ **inicializar variável** ação](#create-variable). Criar uma variável de número inteiro denominada **contagem** com um zero iniciar valor.

   ![Adicionar ação "Inicializar variável"](./media/logic-apps-create-variables-store-values/initialize-variable.png)

4. Para ciclo através de cada anexo, adicione um *para cada* ciclo escolhendo **novo passo** > **mais** > **adicionar um para cada**.

   ![Adicionar um ciclo "para cada"](./media/logic-apps-create-variables-store-values/add-loop.png)

5. Num ciclo, clique dentro da **selecionar uma saída dos passos anteriores** caixa. Quando for apresentada a lista de conteúdo dinâmica, selecione **anexos**. 

   ![Selecionar "Attachments"](./media/logic-apps-create-variables-store-values/select-attachments.png)

   O **anexos** campo passa uma matriz que tenha os anexos de e-mail de saída do accionador no seu ciclo.

6. Num ciclo "para cada", selecione **adicionar uma ação**. 

   ![Selecione "Adicionar uma ação"](./media/logic-apps-create-variables-store-values/add-action-2.png)

7. Na caixa de pesquisa, introduza "variável de incremento" como o filtro. Na lista de ações, selecione **variáveis - variável de incremento**.

   > [!NOTE]
   > Certifique-se de que o **variável de incremento** ação aparece dentro do ciclo. Se for apresentada a ação fora do ciclo, arraste a ação em ciclo.

8. No **variável de incremento** ação, do **nome** lista, selecione o **contagem** variável. 

   ![Selecione a variável de "Contagem"](./media/logic-apps-create-variables-store-values/add-increment-variable-example.png)

9. Em ciclo, adicione qualquer ação que envia-lhe o número de anexos. Na sua ação, incluem o valor da **contagem** variável, por exemplo: 

   ![Adicionar uma ação que envia resultados](./media/logic-apps-create-variables-store-values/send-email-results.png)

10. Guarde a aplicação lógica. Na barra de ferramentas do estruturador, escolha **Guardar**. 

### <a name="test-your-logic-app"></a>Testar a sua aplicação lógica

1. Se a sua aplicação lógica não está ativada, no menu aplicação lógica, escolha **descrição geral**. Na barra de ferramentas, escolha **ativar**. 

2. Na barra de ferramentas Designer de aplicação lógica, escolha **executar**. Este passo manualmente inicia a sua aplicação lógica.

3. Envie um e-mail com um ou mais anexos para a conta de e-mail que utilizou neste exemplo.

   Este passo é acionado acionador da aplicação lógica, que cria e executa uma instância de fluxo de trabalho da sua aplicação lógica.
   Como resultado, a aplicação lógica envia-lhe uma mensagem ou e-mail que mostra o número de anexos de e-mail que é enviado.

Se mudar do estruturador no Editor de vista de código, eis a forma como o ciclo "para cada" é apresentada com o **variável de incremento** ação dentro a definição da aplicação lógica, que está no formato JSON.

```json
"actions": {
   "For_each": {
      "type": "Foreach",
      "actions": {
         "Increment_variable": {
           "type": "IncrementVariable",
            "inputs": {
               "name": "Count",
               "value": 1
            },
            "runAfter": {}
         }
      },
      "foreach": "@triggerBody()?['Attachments']",
      "runAfter": {
         "Initialize_variable": [ "Succeeded" ]
      }
   }
},
```

<a name="decrement-value"></a>

## <a name="decrement-variable"></a>Variável de diminuição

Para diminuir ou *diminuir* uma variável por um valor constante, siga os passos para [aumentar uma variável](#increment-value) exceto que, localize e selecione o **variáveis - Decremento variável**ação em vez disso. Esta ação funciona apenas com variáveis de número inteiro e número de vírgula flutuante.

Seguem-se as propriedades para o **variável Decremento** ação:

| Propriedade | Necessário | Valor |  Descrição |
|----------|----------|-------|--------------|
| Nome | Sim | <*nome da variável*> | O nome da variável diminuir | 
| Valor | Não | <*valor de incremento*> | O valor de decrementing a variável. O valor predefinido é um. <p><p>**Sugestão**: embora opcional, definir este valor como uma melhor prática, para que saiba sempre o valor específico de decrementing a variável. | 
||||| 

Se mudar do estruturador no Editor de vista de código, eis a forma como o **variável Decremento** ação apresentado no interior a definição da aplicação lógica, que está no formato JSON.

```json
"actions": {
   "Decrement_variable": {
      "type": "DecrementVariable",
      "inputs": {
         "name": "Count",
         "value": 1
      },
      "runAfter": {}
   }
},
```


<a name="assign-value"></a>

## <a name="set-variable"></a>Definir variável

Para atribuir um valor diferente para uma variável existente, siga os passos para [aumentar uma variável](#increment-value) exceto que: 

1. Localize e selecione o **variáveis - definir variável** ação em vez disso. 

2. Forneça o nome da variável e o valor que pretende atribuir. O novo valor e a variável tem de ter os mesmo tipo de dados.
O valor é necessário porque esta ação não tem um valor predefinido. 

Seguem-se as propriedades para o **definir variável** ação:

| Propriedade | Necessário | Valor |  Descrição | 
|----------|----------|-------|--------------| 
| Nome | Sim | <*nome da variável*> | O nome da variável alterar | 
| Valor | Sim | <*novo valor*> | O valor que pretende atribuir a variável. Ambos têm de ter os mesmo tipo de dados. | 
||||| 

> [!NOTE]
> A menos que estiver incrementando ou decrementing variáveis, alterar as variáveis no interior de ciclos *poderá* criar resultados inesperados, porque os ciclos são executadas em paralelo, ou em simultâneo, por predefinição. Para estes casos, tente definir o seu ciclo de são executados sequencialmente. Por exemplo, se pretender que o valor da variável dentro do ciclo de referência e a esperar o mesmo valor no início e no fim dessa instância de ciclo, siga estes passos para alterar como executa o ciclo: 
>
> 1. No canto superior direito de seu ciclo, escolha o botão de reticências (…) e, em seguida, escolha **definições**.
> 
> 2. Em **o controlo de simultaneidade**, alterar o **substituir predefinido** definição **no**.
>
> 3. Arraste o **grau de paralelismo** controlo de deslize para **1**.

Se mudar do estruturador no Editor de vista de código, eis a forma como o **definir variável** ação apresentado no interior a definição da aplicação lógica, que está no formato JSON. Neste exemplo altera o valor atual da variável de "Contagem" para outro valor. 

```json
"actions": {
   "Initialize_variable": {
      "type": "InitializeVariable",
      "inputs": {
         "variables": [ {
               "name": "Count",
               "type": "Integer",
               "value": 0
          } ]
      },
      "runAfter": {}
   },
   "Set_variable": {
      "type": "SetVariable",
      "inputs": {
         "name": "Count",
         "value": 100
      },
      "runAfter": {
         "Initialize_variable": [ "Succeeded" ]
      }
   }
},
```

<a name="append-value"></a>

## <a name="append-to-variable"></a>Acrescentar a variável

Para as variáveis que armazenam as cadeias ou matrizes, pode inserir ou *acrescentar* valor de uma variável como o último item essas cadeias ou matrizes. Pode seguir os passos para [aumentar uma variável](#increment-value) exceto que, em vez disso, siga estes passos: 

1. Localize e selecione uma destas ações com base na sua variável de uma cadeia ou uma matriz: 

  * **Variáveis - acrescentar a variável de cadeia**
  * **Variáveis - acrescentar a variável de matriz** 

2. Forneça o valor a acrescentar como o último item na cadeia ou matriz. Este valor é preciso. 

Seguem-se as propriedades para o **acrescentar a...**  ações:

| Propriedade | Necessário | Valor |  Descrição | 
|----------|----------|-------|--------------| 
| Nome | Sim | <*nome da variável*> | O nome da variável alterar | 
| Valor | Sim | <*acrescentar-valor*> | O valor que pretende anexar, que pode ter qualquer tipo | 
|||||  

Se mudar do estruturador no Editor de vista de código, eis a forma como o **acrescentar a variável de matriz** ação apresentado no interior a definição da aplicação lógica, que está no formato JSON.
Este exemplo cria uma variável de matriz e adiciona um valor diferente do último item na matriz. O resultado é uma variável atualizada que contém esta matriz de: `[1,2,3,"red"]` 

```json
"actions": {
   "Initialize_variable": {
      "type": "InitializeVariable",
      "inputs": {
         "variables": [ {
            "name": "myArrayVariable",
            "type": "Array",
            "value": [1, 2, 3]
         } ]
      },
      "runAfter": {}
   },
   "Append_to_array_variable": {
      "type": "AppendToArrayVariable",
      "inputs": {
         "name": "myArrayVariable",
         "value": "red"
      },
      "runAfter": {
        "Initialize_variable": [ "Succeeded" ]
      }
   }
},
```

## <a name="get-support"></a>Obter suporte

* Relativamente a dúvidas, visite o [fórum do Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Para submeter ou votar em ideias para funcionalidades, visite o [site de comentários dos utilizadores do Logic Apps](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Passos Seguintes

* Saiba mais sobre [conectores Logic Apps](../connectors/apis-list.md)
