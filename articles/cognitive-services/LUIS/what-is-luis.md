---
title: O que é o Language Understanding (LUIS) - Serviços Cognitivos do Azure | Microsoft Docs
description: O Language Understanding (LUIS) é um serviço de API baseado na cloud que aplica inteligência de machine learning personalizada aos textos conversacionais de idioma natural do utilizador, para prever o sentido geral e extrair informações relevantes e detalhadas. Uma aplicação cliente para o LUIS é qualquer aplicação conversacional que comunique com um utilizador num idioma natural para concluir uma tarefa. Alguns exemplos de aplicações cliente incluem aplicações de redes sociais, bots de chat e aplicações para ambiente de trabalho com fala ativada.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: overview
ms.date: 10/06/2018
ms.author: diberry
ms.openlocfilehash: 0520c00ab20ca7210b3bb13567f9998e7231be43
ms.sourcegitcommit: 0bb8db9fe3369ee90f4a5973a69c26bff43eae00
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/08/2018
ms.locfileid: "48867679"
---
# <a name="what-is-language-understanding-luis"></a>O que é o Language Understanding (LUIS)?

O Language Understanding (LUIS) é um serviço de API baseado na cloud que aplica inteligência de machine learning personalizada aos textos conversacionais de idioma natural do utilizador, para prever o sentido geral e extrair informações relevantes e detalhadas. 

Uma aplicação cliente para o LUIS é qualquer aplicação conversacional que comunique com um utilizador num idioma natural para concluir uma tarefa. Alguns exemplos de aplicações cliente incluem aplicações de redes sociais, bots de chat e aplicações para ambiente de trabalho com fala ativada.  

![Imagem conceptual de três aplicações cliente que trabalham com o Language Understanding (LUIS) dos Serviços Cognitivos](./media/luis-overview/luis-entry-point.png "Conceptual image of 3 client applications working with Cognitive Services Language Understanding (LUIS)")

## <a name="use-luis-in-a-chat-bot"></a>Utilizar o LUIS num bot de chat

<a name="Accessing-LUIS"></a>

Quando a aplicação LUIS for publicada, uma aplicação cliente envia expressões (texto) para a [API][endpoint-apis] de ponto final de processamento de idioma natural do LUIS e recebe os resultados como respostas JSON. Os bots de chat são uma aplicação cliente comum para o LUIS.


![Imagens conceptuais do LUIS a funcionar com o bot de chat para prever o texto do utilizador com compreensão de idioma natural (NLP)](./media/luis-overview/luis-overview-process-2.png "Conceptual imagery of LUIS working with Chat bot to predict user text with natural language understanding (NLP")

|Passo|Ação|
|:--|:--|
|1|A aplicação cliente envia a _expressão_ do utilizador (o texto nas palavras do utilizador), "I want to call my HR rep" (“Quero ligar ao meu representante de RH”) ao ponto final do LUIS como um pedido HTTP.|
|2|O LUIS aplica o modelo aprendido ao texto em idioma natural para proporcionar a compreensão inteligente relativa à entrada do utilizador. O LUIS devolve uma resposta formatada em JSON, com uma intenção principal, “HRContact”. A resposta mínima do ponto final do JSON contém a expressão da consulta e a intenção com a melhor classificação. Também pode extrair dados, como a entidade Tipo de Contacto.|
|3|A aplicação cliente utiliza a resposta JSON para tomar decisões quanto à forma como satisfaz os pedidos do utilizador. Essas decisões podem incluir uma árvore de decisões no código da arquitetura do bot e chamadas para outros serviços. |

A aplicação LUIS proporciona inteligência para que a aplicação cliente possa tomar decisões informadas. O LUIS não fornece essas decisões. 

<!--

### Example of JSON endpoint response

The minimum JSON endpoint response contains the query utterance, and the top scoring intent. It can also extract data such as the following **Contact Type** entity. 

```JSON
{
  "query": "I want to call my HR rep.",
  "topScoringIntent": {
    "intent": "HRContact",
    "score": 0.921233
  },
  "entities": [
    {
      "entity": "call",
      "type": "Contact Type",
      "startIndex": 10,
      "endIndex": 13,
      "score": 0.7615982
    }
  ]
}
```
-->

<a name="Key-LUIS-concepts"></a>
<a name="what-is-a-luis-model"></a>

## <a name="natural-language-processing"></a>Processamento de linguagem natural

As aplicações de LUIS contêm um modelo de idioma natural específico de domínio. Pode iniciar as aplicações LUIS com um modelo de domínio pré-criado, criar o seu próprio modelo ou misturar partes de um domínio pré-criado com as suas informações personalizadas.

* **Modelo pré-criado** O LUIS tem muitos modelos de domínio pré-criados, incluindo intenções, expressões e entidades pré-criadas. Pode utilizar as entidades pré-criadas sem utilizar as intenções e as expressões do modelo pré-criado. Os [modelos de domínio pré-criados](luis-how-to-use-prebuilt-domains.md) incluem todo o design para si e são uma excelente forma de começar a utilizar o LUIS rapidamente.

* **Entidades personalizadas** O LUIS oferece-lhe vários modos de identificar as suas próprias intenções e entidades personalizadas, incluindo entidades de aprendizagem de machine learning, e entidades específicas ou literais ou uma combinação de entidades de aprendizagem de machine learning e literais.

## <a name="build-the-luis-model"></a>Compilar o modelo do LUIS
Compile o modelo com as APIs de [criação](https://aka.ms/luis-authoring-apis) ou com o portal do LUIS.

O modelo do LUIS começa com categorias de intenções do utilizador, denominadas **[intenções](luis-concept-intent.md)**. Cada intenção precisa de exemplos de **[expressões](luis-concept-utterance.md)** do utilizador. Cada expressão pode fornecer diversos dados que têm de ser extraídos com as **[entidades](luis-concept-entity-types.md)**. 

|Exemplo de expressão do utilizador|Intenção|Entidades|
|-----------|-----------|-----------|
|"Reservar um bilhete de avião para __Seattle__?"|ReservarBilhete|Seattle|
|"Quando é que a loja __abre__?"|HoráriodaLojaeLocalização|abre|
|"Agendar reunião às __13:00__ com o __João__ da Distribuição”|AgendarReunião|13:00, João|

<!--
## What is a natural language model?

A model begins with a list of general user intentions, called _intents_, such as "Book Flight" or "Contact Help Desk." You provide user's example text, called _example utterances_ for the intents. Then mark significant words or phrases in the utterance, called _entities_.


A model includes:

* **[intents](#intents)**: categories of user intentions (overall intended action or result)
* **[entities](#entities)**: specific types of data in utterances such as number, email, or name contained in text
* **[example utterances](#example-utterances)**: example text a user might enter in the client application

### Intents 

An [intent](luis-how-to-add-intents.md), short for _intention_, is a purpose or goal expressed in a user's utterance, such as booking a flight, paying a bill, or finding a news article. You create an intent for each action. A LUIS travel app may define an intent named "BookFlight." Each prediction query includes the top scored intent. 

The client application can use the top scoring intent to trigger an action. For example, when "BookFlight" intent is returned from LUIS, a client application could trigger an API call to an external service for booking a plane ticket.

### Entities

An [entity](luis-how-to-add-entities.md) represents detailed information found within the utterance that is relevant to the user's request. For example, in the utterance "Book a ticket to Paris",  a single ticket is requested, and "Paris" is a location. Two entities are found "a ticket" indicating a single ticket and "Paris" indicating the destination. 

After LUIS returns the entities found in the user’s utterance, the client application can use the list of entities as parameters to trigger an action. For example, booking a flight requires entities like the travel destination, date, and airline.
-->
<!--
### Example utterances

An example [utterance](luis-how-to-add-example-utterances.md) is text input from the user that the client application needs to understand. It may be a sentence, like "Book a ticket to Paris", or a fragment of a sentence, like "Booking" or "Paris flight." Utterances aren't always well-formed, and there can be many utterance variations for a particular intent. Add 10 to 20 example utterances to each intent and mark entities in every utterance.

|Example user utterance|Intent|Entities|
|-----------|-----------|-----------|
|"Book a flight to __Seattle__?"|BookFlight|Seattle|
|"When does your store __open__?"|StoreHoursAndLocation|open|
|"Schedule a meeting at __1pm__ with __Bob__ in Distribution"|ScheduleMeeting|1pm, Bob|
-->
## <a name="query-prediction-endpoint"></a>Ponto final de predição da consulta

Após a compilação e publicação do modelo no ponto final, a aplicação cliente envia expressões para a API de [ponto final](https://aka.ms/luis-endpoint-apis) de predição publicada. A API aplica o modelo ao texto, para análise. A API responde com os resultados da predição em formato JSON.  

A resposta mínima do ponto final do JSON contém a expressão da consulta e a intenção com a melhor classificação. Também pode extrair dados, como a entidade **Contact Type** seguinte. 

```JSON
{
  "query": "I want to call my HR rep.",
  "topScoringIntent": {
    "intent": "HRContact",
    "score": 0.921233
  },
  "entities": [
    {
      "entity": "call",
      "type": "Contact Type",
      "startIndex": 10,
      "endIndex": 13,
      "score": 0.7615982
    }
  ]
}
```

## <a name="improve-model-prediction"></a>Melhorar a predição do modelo

Depois de um modelo do LUIS ser publicado e receber expressões reais do utilizador, o LUIS proporciona vários métodos para melhorar a exatidão da predição: [aprendizagem ativa](luis-concept-review-endpoint-utterances.md) de expressões de ponto final, [listas de expressões](luis-concept-feature.md) para inclusão de palavras do domínio e [padrões](luis-concept-patterns.md) para reduzir o número de expressões necessárias.
<!--
### Active learning

In the [active learning](luis-how-to-review-endoint-utt.md) process, LUIS allows you to adapt the LUIS app to real-world utterances by selecting utterances it received at the endpoint for your review. You can accept or correct the endpoint prediction, retrain, and republish. LUIS learns quickly with this iterative process, taking the minimum amount of your time and effort. 

### Phrase lists 

LUIS provides [phrases lists](luis-concept-feature.md) so you can indicate important words or phrases of the model. LUIS uses these lists to add additional significance to those words and phrases that would otherwise not be found in the model.

### Patterns 

Patterns allow you to simplify an intent's utterance collection into common [templates](luis-concept-patterns.md) of word choice and word order. This allows LUIS to learn quicker by needing fewer example utterances for the intents. Patterns are a hybrid system of regular expressions and machine-learned expressions. 
-->
<a name="using-luis"></a>
<!--
## Authoring and accessing models
Author LUIS from the [authoring](https://aka.ms/luis-authoring-apis) APIs or from the LUIS portal. Query the published prediction endpoint of the model from the [endpoint](https://aka.ms/luis-endpoint-apis) APIs.
-->

## <a name="development-lifecycle"></a>Ciclo de vida de desenvolvimento
O Language Understanding Intelligent Service (LUIS) disponibiliza ferramentas, controlo de versões e colaboração com outros criadores de LUIS para integrar o ciclo de vida de desenvolvimento completo ao nível da aplicação cliente e do modelo de linguagem. 

## <a name="implementing-luis"></a>Implementar o LUIS
O LUIS, uma API REST, pode ser utilizado com qualquer produto, serviço ou arquitetura que faça pedidos HTTP. A lista seguinte contém os principais produtos e serviços Microsoft que são utilizados com o LUIS.

A aplicação cliente principal do LUIS é:
* [Bot de aplicação Web](https://docs.microsoft.com/azure/bot-service/?view=azure-bot-service-3.0) - cria rapidamente um bot de chat ativado para o LUIS, para falar com um utilizador através de introdução de texto. Utiliza o [Bot Framework][bot-framework] versão [3.x](https://github.com/Microsoft/BotBuilder) ou [4.x](https://github.com/Microsoft/botbuilder-dotnet) para proporcionar uma experiência de bot completa.

Ferramentas para utilizar o LUIS de forma rápida e fácil com um bot:
* [CLI do LUIS](https://github.com/Microsoft/botbuilder-tools/tree/master/packages/LUIS) – O pacote NPM disponibiliza a criação e a predição como ferramentas de linha de comandos autónomas ou como importação. 
* [LUISGen](https://github.com/Microsoft/botbuilder-tools/tree/master/packages/LUISGen)– O LUISGen é uma ferramenta para gerar código fonte TypeScript e em C# com tipos de dados inflexíveis a partir de um modelo exportado do LUIS.
* O [Dispatch](https://aka.ms/dispatch-tool) permite utilizar uma variedade de aplicações do LUIS e de Criador de FAQ a partir de uma aplicação principal através da utilização do modelo de dispatcher.
* [LUDown](https://github.com/Microsoft/botbuilder-tools/tree/master/packages/Ludown) – O LUDown é uma ferramenta de linha de comandos que ajuda a gerir modelos de linguagem para o bot.

Outros Serviços Cognitivos utilizados com o LUIS:
* [Criador de FAQ][qnamaker] - permite combinar vários tipos de textos numa base de conhecimento de perguntas e respostas.
* [API de Verificação de Ortografia do BingI](../bing-spell-check/proof-text.md) - fornece correção de textos antes da predição. 
* [Speech service](../Speech-Service/overview.md) - converte os pedidos de idioma falado em texto. 
* [Conversation learner](https://docs.microsoft.com/azure/cognitive-services/labs/conversation-learner/overview) - permite-lhe criar conversações de bot mais depressa com o LUIS.
* [Project personality chat](https://docs.microsoft.com/azure/cognitive-services/project-personality-chat/overview) - para processar conversa fiada do bot.
<!--
## Other ways of implementing LUIS

A client application for LUIS is:
* [Windows Mixed Reality](https://docs.microsoft.com/windows/mixed-reality/) - learn more with this [Mixed reality course](https://docs.microsoft.com/windows/mixed-reality/mr-azure-303) with LUIS. 


Labs: 

-->
## <a name="next-steps"></a>Passos seguintes

Crie uma aplicação do LUIS nova com um domínio [pré-criado](luis-get-started-create-app.md) ou [personalizado](luis-quickstart-intents-only.md). [Consulte o ponto final da predição](luis-get-started-cs-get-intent.md) de uma aplicação IoT pública.

<!-- Reference-style links -->
[bot-framework]: https://docs.microsoft.com/bot-framework/
[flow]: https://docs.microsoft.com/connectors/luis/
[authoring-apis]: https://aka.ms/luis-authoring-api
[endpoint-apis]: https://aka.ms/luis-endpoint-apis
[qnamaker]: https://qnamaker.ai/