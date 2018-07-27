---
title: Adicionar e executar código personalizado no Azure Logic Apps com as funções do Azure | Documentos da Microsoft
description: Saiba como adicionar e executar fragmentos de código personalizado no Azure Logic Apps com as funções do Azure
services: logic-apps
ms.service: logic-apps
author: ecfan
ms.author: estfan
manager: jeconnoc
ms.topic: article
ms.date: 07/25/2018
ms.reviewer: klam, LADocs
ms.suite: integration
ms.openlocfilehash: 20ad738541554279ff9fd6dd6babe90a38676c00
ms.sourcegitcommit: a5eb246d79a462519775a9705ebf562f0444e4ec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/26/2018
ms.locfileid: "39263195"
---
# <a name="add-and-run-custom-code-snippets-in-azure-logic-apps-with-azure-functions"></a>Adicionar e executar fragmentos de código personalizado no Azure Logic Apps com as funções do Azure

Quando deseja criar e executar apenas suficiente código que corrige um problema específico nas suas aplicações lógicas, pode criar suas próprias funções usando [as funções do Azure](../azure-functions/functions-overview.md). Este serviço fornece a capacidade para criar e executar fragmentos de código personalizado escritos com node. js ou c# nas suas aplicações lógicas, sem se preocupar sobre a criação de uma aplicação completa ou a infraestrutura para a execução do seu código. As funções do Azure fornece computação sem servidor na cloud e é útil para realizar tarefas, como nestes exemplos:

* Estenda o comportamento da sua aplicação lógica com as funções suportadas pelo node. js ou c#.
* Realizar cálculos em seu fluxo de trabalho de aplicação lógica.
* Aplicar a formatação avançada ou campos nas suas aplicações lógicas de computação.

Também pode [chamar aplicações lógicas do dentro das funções do Azure](#call-logic-app).

## <a name="prerequisites"></a>Pré-requisitos

Para seguir este artigo, aqui estão os itens que precisa:

* Se não tiver uma subscrição do Azure, <a href="https://azure.microsoft.com/free/" target="_blank">Inscreva-se uma conta gratuita do Azure</a>. 

* A aplicação de lógica onde pretende adicionar a função

  Se estiver familiarizado com aplicações lógicas, reveja [o que é o Azure Logic Apps](../logic-apps/logic-apps-overview.md) e [guia de início rápido: criar a sua primeira aplicação lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md).

* R [acionador](../logic-apps/logic-apps-overview.md#logic-app-concepts) como a primeira etapa na sua aplicação lógica 

  Antes de poder adicionar ações para executar funções, tem de iniciar a aplicação lógica com um acionador.

* Uma aplicação de função do Azure, que é um contentor para as funções do Azure e a função do Azure. Se não tiver uma aplicação de funções, deve [primeiro a criar a sua aplicação function app](../azure-functions/functions-create-first-azure-function.md). Pode, em seguida, criar a sua função seja [separadamente fora da sua aplicação lógica](#create-function-external), ou [de dentro da sua aplicação lógica](#create-function-designer) no Estruturador da aplicação lógica.

  Aplicações de novas e existentes funções do Azure e as funções têm os mesmos requisitos para trabalhar com as logic apps:

  * A aplicação de função têm de pertencer à mesma subscrição do Azure que a sua aplicação lógica.

  * A função tem de utilizar o **webhook genérico** modelo de função por qualquer **JavaScript** ou **c#**. Este modelo pode aceitar o conteúdo que tenha `application/json` tipo da sua aplicação lógica. Estes modelos também ajudam o Estruturador da aplicação lógica encontrar e mostrar as funções personalizadas que pode criar com estes modelos quando adicionar essas funções às suas aplicações lógicas.

  * Verifique se seu modelo de função **modo** estiver definida como **Webhook** e o **tipo de Webhook** propriedade está definida como **JSON genérica**.

    1. Inicie sessão no <a href="https://portal.azure.com" target="_blank">portal do Azure</a>.
    2. No menu principal do Azure, selecione **aplicações de funções**. 
    3. Na **aplicações Function App** lista, selecione a sua aplicação de função, expanda a sua função e selecione **integrar**. 
    4. Verifique o seu modelo **modo** estiver definida como **Webhook** e que o **tipo de Webhook** propriedade está definida como **JSON genérica**. 

  * Se a sua função tem um [definição de API](../azure-functions/functions-openapi-definition.md), anteriormente conhecido como um [ficheiro Swagger](http://swagger.io/), o estruturador de aplicações lógicas oferece uma experiência mais rica para trabalhar com os parâmetros da função. 
  Antes da aplicação lógica pode localizar e acessar as funções que tenham uma descrição do Swagger, [configure a sua aplicação de função, seguindo estes passos](#function-swagger).

<a name="create-function-external"></a>

## <a name="create-functions-outside-logic-apps"></a>Criar aplicações lógicas fora de funções

Na <a href="https://portal.azure.com" target="_blank">portal do Azure</a>, criar a sua aplicação de função do Azure, que tem de ter a mesma subscrição do Azure que a sua aplicação lógica e, em seguida, criar a sua função do Azure. Se estiver familiarizado com as funções do Azure, saiba como [criar a primeira função no portal do Azure](../azure-functions/functions-create-first-azure-function.md), mas tenha em atenção estes requisitos para a criação de funções do Azure que pode adicionar e chamar a partir de aplicações lógicas.

* Certifique-se de que seleciona os **webhook genérico** modelo de função por qualquer **JavaScript** ou **c#**.

  ![Webhook genérico - JavaScript ou em C#](./media/logic-apps-azure-functions/generic-webhook.png)

* Depois de criar a função do Azure, verifique se o modelo **modo** e **tipo de Webhook** propriedades estão definidas corretamente.

  1. Na **aplicações Function App** listar, expanda a sua função e selecione **integrar**. 

  2. Verifique se seu modelo **modo** estiver definida como **Webhook** e que o **tipo de Webhook** propriedade está definida como **JSON genérica**. 

     ![Propriedades do seu modelo de função "Integrar"](./media/logic-apps-azure-functions/function-integrate-properties.png)

<a name="function-swagger"></a>

* Opcionalmente, se [gerar uma definição de API](../azure-functions/functions-openapi-definition.md), anteriormente conhecido como um [ficheiro Swagger](http://swagger.io/), por sua função, pode obter uma experiência mais rica ao trabalhar com parâmetros de função no Designer de aplicações lógicas. Para configurar a sua aplicação de funções para que a aplicação lógica pode encontrar e aceder às funções que tenham uma descrição do Swagger:

  * Certifique-se de que a sua aplicação function app está ativamente em execução.

  * Na sua aplicação de função, configure [Cross-Origin Resource Sharing (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) para que todas as origens são permitidas:

    1. A partir do **aplicações Function App** , selecione a aplicação de função > **recursos da plataforma** > **CORS**.

       ![Selecione a sua aplicação de função > "Funcionalidades de plataforma" > "CORS"](./media/logic-apps-azure-functions/function-platform-features-cors.png)

    2. Sob **CORS**, adicione o `*` caráter universal de caracteres, mas remova todas as outras origens na lista e escolha **guardar**.

       ![Selecione a sua aplicação de função > "Funcionalidades de plataforma" > "CORS"](./media/logic-apps-azure-functions/function-platform-features-cors-origins.png)

### <a name="access-property-values-inside-http-requests"></a>Valores de propriedade de acesso no interior de pedidos de HTTP

Funções de Webhook podem aceitar pedidos HTTP como entradas e passar esses pedidos para outras funções. Por exemplo, embora tenha de Logic Apps [funções que converter valores de DateTime](../logic-apps/workflow-definition-language-functions-reference.md), esta função de JavaScript de exemplo básico mostra como pode acessar uma propriedade dentro de um objeto de pedido que é passado para a função e executar operações no esse valor de propriedade. Para aceder às propriedades dentro de objetos, este exemplo utiliza a [operador de ponto (.)](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/Property_accessors): 

```javascript
function convertToDateString(request, response){
   var data = request.body;
   response = {
      body: data.date.ToDateString();
   }
}
```

Eis o que acontece dentro desta função:

1. A função cria um `data` variável e atribui o `body` objeto dentro de `request` objeto para essa variável. A função usa o operador de ponto (.) para fazer referência a `body` dentro de objeto a `request` objeto: 

   ```javascript
   var data = request.body;
   ```

2. A função agora pode aceder a `date` propriedade através do `data` variável e converter valor de propriedade do tipo DateTime DateString escreva chamando o `ToDateString()` função. A função também devolve o resultado através do `body` propriedade na resposta da função: 

   ```javascript
   body: data.date.ToDateString();
   ```

Agora que criou sua função do Azure, siga os passos para saber como [adicione funções para aplicações lógicas](#add-function-logic-app).

<a name="create-function-designer"></a>

## <a name="create-functions-inside-logic-apps"></a>Crie funções dentro de aplicações lógicas

Antes de poder criar uma função do Azure a partir de dentro da sua aplicação lógica no Estruturador da aplicação lógica, primeiro tem de ter uma aplicação de função do Azure, o que é um contentor para as suas funções. Se não tiver uma aplicação de funções, primeiro a criar essa aplicação de função. Ver [criar a primeira função no portal do Azure](../azure-functions/functions-create-first-azure-function.md). 

1. Na <a href="https://portal.azure.com" target="_blank">portal do Azure</a>, abra a aplicação lógica no Estruturador da aplicação lógica. 

2. No passo para criar e adicionar a função onde pretende, escolha **novo passo** > **adicionar uma ação**. 

3. Na caixa de pesquisa, introduza "azure functions" como o filtro.
Na lista de ações, selecione a ação: **escolher uma função do Azure - as funções do Azure** 

   ![Localizar "Azure functions"](./media/logic-apps-azure-functions/find-azure-functions-action.png)

4. Na lista de aplicações de função, selecione a sua aplicação de função. Depois das ações é aberta a lista, selecione a ação: **as funções do Azure - criar uma nova função**

   ![Selecione a sua aplicação de função](./media/logic-apps-azure-functions/select-function-app-create-function.png)

5. No editor de definição de função, defina sua função:

   1. Na **nome da função** caixa, indique um nome para a função. 

   2. Na **código** caixa, adicione o código de função para o modelo, incluindo a resposta e a carga que pretende devolvido à sua aplicação lógica após a conclusão da sua função em execução. 
   O objeto de contexto no código de modelo descreve a mensagem e o conteúdo que a aplicação lógica passa para a sua função, por exemplo:

      ![Definir a sua função](./media/logic-apps-azure-functions/function-definition.png)

      Dentro da sua função, pode referenciar as propriedades no objeto de contexto, utilize esta sintaxe:

      ```text
      context.<token-name>.<property-name>
      ```
      Neste exemplo, eis a sintaxe que usaria:

      ```text
      context.body.content
      ```

   3. Quando tiver terminado, escolha **Create** (Criar).

6. Na **corpo do pedido** caixa, especifique o objeto de contexto para passar como entrada, a função do que têm de ser formatado em JavaScript Object Notation (JSON). Ao clicar no **corpo do pedido** , lista de conteúdo dinâmico é aberta a caixa pelo que pode selecionar tokens para propriedades disponíveis dos passos anteriores. 

   Neste exemplo, o objeto no passa a **corpo** tokens de Acionador de e-mail:  

   ![Exemplo de "Corpo do pedido" - payload de objeto de contexto](./media/logic-apps-azure-functions/function-request-body-example.png)

   Com base no conteúdo no objeto de contexto, o Estruturador da aplicação lógica gera um modelo de função que, em seguida, pode editar inline. 
   O Logic Apps também cria variáveis com base no objeto de contexto de entrada.

   Neste exemplo, o objeto de contexto não é converter como uma cadeia de caracteres, para que o conteúdo é adicionado diretamente para o payload JSON. 
   No entanto, se o objeto não é um token JSON, que tem de ser uma cadeia de caracteres, um objeto JSON ou uma matriz JSON, obterá um erro. 
   Converter o objeto de contexto como uma cadeia de caracteres, adicione aspas, por exemplo:

   ![Converter o objeto como cadeia de caracteres](./media/logic-apps-azure-functions/function-request-body-string-cast-example.png)

7. Para especificar outros detalhes, como o método para utilização, cabeçalhos de pedido ou parâmetros de consulta, escolha **Mostrar opções avançadas**.

<a name="add-function-logic-app"></a>

## <a name="add-existing-functions-to-logic-apps"></a>Adicionar funções existentes para aplicações lógicas

Para chamar as funções do Azure existentes a partir de aplicações lógicas, pode adicionar as funções do Azure, como qualquer outra ação no Estruturador da aplicação lógica. 

1. Na <a href="https://portal.azure.com" target="_blank">portal do Azure</a>, abra a aplicação lógica no Estruturador da aplicação lógica. 

2. No passo em que pretende adicionar a função, escolha **novo passo** > **adicionar uma ação**. 

3. Na caixa de pesquisa, introduza "azure functions" como o filtro.
Na lista de ações, selecione a ação: **escolher uma função do Azure - as funções do Azure** 

   ![Localizar "Azure functions"](./media/logic-apps-azure-functions/find-azure-functions-action.png)

4. Na lista de aplicações de função, selecione a sua aplicação de função. Depois de aparece na lista de funções, selecione a sua função. 

   ![Selecione a aplicação de funções e a função do Azure](./media/logic-apps-azure-functions/select-function-app-existing-function.png)

   Para as funções que têm as definições da API (Swagger descrições) e que são [para que a sua aplicação lógica pode encontrar e aceder essas funções](#function-swagger), pode selecionar **ações de Swagger**:

   ![Selecione a aplicação de funções, "Swagger ações" "e sua função do Azure](./media/logic-apps-azure-functions/select-function-app-existing-function-swagger.png)

5. Na **corpo do pedido** caixa, especifique o objeto de contexto para passar como entrada, a função do que têm de ser formatado em JavaScript Object Notation (JSON). Este objeto de contexto descreve a mensagem e o conteúdo que a aplicação lógica envia para a sua função. 

   Ao clicar no **corpo do pedido** , lista de conteúdo dinâmico é aberta a caixa pelo que pode selecionar tokens para propriedades disponíveis dos passos anteriores. 
   Neste exemplo, o objeto no passa a **corpo** tokens de Acionador de e-mail:

   ![Exemplo de "Corpo do pedido" - payload de objeto de contexto](./media/logic-apps-azure-functions/function-request-body-example.png)

   Neste exemplo, o objeto de contexto não é converter como uma cadeia de caracteres, para que o conteúdo é adicionado diretamente para o payload JSON. 
   No entanto, se o objeto não é um token JSON, que tem de ser uma cadeia de caracteres, um objeto JSON ou uma matriz JSON, obterá um erro. 
   Converter o objeto de contexto como uma cadeia de caracteres, adicione aspas, por exemplo:

   ![Converter o objeto como cadeia de caracteres](./media/logic-apps-azure-functions/function-request-body-string-cast-example.png)

6. Para especificar outros detalhes, como o método para utilização, cabeçalhos de pedido ou parâmetros de consulta, escolha **Mostrar opções avançadas**.

<a name="call-logic-app"></a>

## <a name="call-logic-apps-from-functions"></a>Chamar aplicações lógicas a partir de funções

Para acionar uma aplicação lógica a partir de dentro de uma função do Azure, essa aplicação lógica tem de ter um ponto de extremidade pode ser chamado, ou, mais especificamente, um **pedir** acionador. Em seguida, de dentro da sua função, envie um pedido HTTP POST para o URL para que **pedido** acionar e incluem o payload desejar essa aplicação lógica para processar. Para obter mais informações, consulte [chamar, acionar, ou aninhar aplicações lógicas](../logic-apps/logic-apps-http-endpoint.md). 

## <a name="get-support"></a>Obter suporte

* Relativamente a dúvidas, visite o [fórum do Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Para submeter ou votar em ideias para funcionalidades, visite o [site de comentários dos utilizadores do Logic Apps](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Passos Seguintes

* Saiba mais sobre [conectores do Logic Apps](../connectors/apis-list.md)
