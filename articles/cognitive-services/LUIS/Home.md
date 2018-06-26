---
title: Sobre compreensão de idiomas (LUIS) no Azure | Microsoft Docs
description: Saiba como utilizar a compreensão de idiomas (LUIS) para colocar a potência de aprendizagem automática para as suas aplicações.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/22/2017
ms.author: v-geberr
ms.openlocfilehash: bbd0a532e54f9b221739c8ae9ff097fe44fdc4df
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/25/2018
ms.locfileid: "36751600"
---
# <a name="what-is-language-understanding-luis"></a>O que é a compreensão de idiomas (LUIS)?
Compreensão de idiomas (LUIS) é um serviço baseado na nuvem que se aplica a aprendizagem personalizada para texto linguagem natural conversational, um utilizador para prever significado geral e extrair informações relevantes, detalhadas. 

Uma aplicação de cliente para LUIS pode ser qualquer conversational aplicação que comunica com um utilizador na linguagem natural para concluir uma tarefa. Exemplos de aplicações de cliente incluem aplicações de redes sociais, chatbots e aplicações de ambiente de trabalho com capacidade para reconhecimento de voz.  

![Imagem conceptual de 3 aplicações feeding informações informações LUIS](./media/luis-overview/luis-entry-point.png)

## <a name="what-is-a-luis-app"></a>O que é uma aplicação LUIS?
Uma aplicação LUIS contém um modelo de linguagem natural a específicas do domínio que crie. Pode iniciar a sua aplicação LUIS com um modelo de domínio prebuilt, criar a sua própria ou, se misturem partes de um domínio prebuilt com as suas próprias informações personalizadas.

[Modelos de domínio prebuilt](luis-how-to-use-prebuilt-domains.md) incluir todos os estas duas para si e são uma excelente forma de começar a utilizar LUIS rapidamente.

A aplicação de LUIS também contém as definições de integração, [colaboradores](luis-concept-collaborator.md), e [versões](luis-concept-version.md).

## <a name="using-a-luis-app"></a>Através de uma aplicação LUIS
<a name="Accessing-LUIS"></a> Assim que a sua aplicação LUIS for publicada, a aplicação cliente envia utterances para o LUIS [ponto final de API] [ endpoint-apis] e recebe os resultados de predição como respostas JSON.

No diagrama seguinte, primeiro chatbot o cliente envia texto de utilizador do que uma pessoa que pretende no seus próprio palavras a LUIS num pedido de HTTP. Segundo, LUIS aplica-se o seu modelo adquirido para a linguagem natural para fazer sentido do utilizador de entrada e devolve uma resposta de formato de JavaScript Object Notation (JSON). Terceira, chatbot o cliente utiliza a resposta JSON para satisfazer os pedidos do utilizador. 

![Imagery conceptual de LUIS trabalhar com Chatbot](./media/luis-overview/luis-overview-process-2.png)

### <a name="example-of-json-endpoint-response"></a>Exemplo de resposta de ponto final JSON

Contém a resposta do ponto final JSON, no mínimo a utterance de consulta e, na parte superior da classificação de intenção. 

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

<a name="Key-LUIS-concepts"></a>
<a name="what-is-a-luis-model"></a>
## <a name="what-is-a-natural-language-model"></a>O que é um modelo de linguagem natural?
Um modelo começa com uma lista de intenções gerais de utilizadores, denominado _pendentes_, tal como "Livro voo" ou "Contacte o suporte técnico." Forneça o texto de exemplo do utilizador, denominado _utterances exemplo_ para os pendentes. Em seguida, marcar significativas palavras ou frases reconhecíveis no utterance, denominado _entidades_.


Um modelo inclui:

* **[pendentes](#intents)**: categorias de intenções de utilizador (ação pretendida ou resultado)
* **[entidades](#entities)**: tipos específicos de dados no utterances como número, o e-mail ou o nome
* **[utterances exemplo](#example-utterances)**: texto de exemplo, um utilizador introduz na aplicação cliente

### <a name="intents"></a>Intenções 
Um [intenção](luis-how-to-add-intents.md), abreviado para _intenção_, é um objetivo ou objetivo expresso em utterance de um utilizador, tais como booking um voo, prestando uma fatura ou ao localizar um artigo de notícias de última hora. Criar um objetivo para cada ação. Uma aplicação de levar LUIS pode definir um objetivo com o nome "BookFlight." A aplicação cliente pode utilizar a parte superior da classificação de intenção para acionar uma ação. Por exemplo, quando "BookFlight" intenção é devolvida de LUIS, a aplicação cliente poderia acionar uma chamada de API a um serviço para booking uma permissão de plane externo.

### <a name="entities"></a>Entidades
Um [entidade](luis-how-to-add-entities.md) representa informações detalhadas encontradas utterance relevantes para o pedido do utilizador. Por exemplo, no utterance "Livro um pedido para Paris", é pedido um único pedido de suporte e "Paris" é uma localização. Duas entidades encontram-se "uma permissão" que indica que um único pedido de suporte e "Paris", que indica que o destino. 

Depois de LUIS devolve entidades encontradas no utterance do utilizador, a aplicação cliente pode utilizar a lista de entidades como parâmetros para acionar uma ação. Por exemplo, entidades, como o destino de levar, data e companhia aérea requer booking um voo.

LUIS fornece várias formas para identificar e categorizar entidades.

* **Entidades prebuilt** LUIS tem muitos modelos de domínio prebuilt incluindo pendentes, utterances, e [entidades prebuilt](pre-builtentities.md). Pode utilizar as entidades prebuilt sem ter de utilizar o utterances do modelo de prebuilt e pendentes. As entidades prebuilt poupar tempo.

* **Entidades personalizadas** LUIS dá-lhe várias formas para identificar a seus próprios personalizada [entidades](luis-concept-entity-types.md) incluindo entidades aprendidas de máquina, entidades literal ou específicas e uma combinação de aprendidas de máquina e literal.

### <a name="example-utterances"></a>Utterances de exemplo
Um exemplo [utterance](luis-how-to-add-example-utterances.md) seja a entrada de texto do utilizador que a aplicação de cliente tem de compreender. Poderá ser uma frase, como "Livro um pedido de suporte para Paris" ou um fragmento de uma frase, como "Booking" ou "Voo Paris." Utterances sempre não são bem formados e podem existir muitas variações de utterance para um objetivo específico. Adicionar 10 a 20 utterances de exemplo para cada intenção e marque as entidades de cada utterance.

|Utterance de utilizador de exemplo|Objetivo|Entidades|
|-----------|-----------|-----------|
|"Livro um voo para __Seattle__?"|BookFlight|Seattle|
|"Ao seu arquivo __abrir__?"|StoreHoursAndLocation|Abrir|
|"Agendar uma reunião no __1 pm__ com __Bernardo__ distribuição"|ScheduleMeeting|1 pm, Bernardo|

## <a name="improve-prediction-accuracy"></a>Melhorar a exatidão da previsão
Depois da aplicação de LUIS é publicada e recebe utterances de utilizador reais, LUIS fornece vários métodos para melhorar a exatidão da previsão: [learning Active Directory](#active-learning) de utterances de ponto final, [frase listas](#phrase-lists) para o domínio inclusão, do Word e [padrões](#patterns) para reduzir o número de utterances necessário.

### <a name="active-learning"></a>Aprendizagem ativa
No [learning Active Directory](label-suggested-utterances.md) processo, o LUIS permite-lhe para se adaptar a sua aplicação LUIS reais utterances selecionando utterances recebeu o ponto final para a revisão. Pode aceitar ou corrija a predição de ponto final, reparametrização dos e voltar a publicar. LUIS aprende rapidamente com este processo iterativo, efetuando a quantidade mínima do tempo e esforço. 

### <a name="phrase-lists"></a>Apresenta uma lista de expressão 
Fornece LUIS [expressões com listas](luis-concept-feature.md) pelo que pode indicar importantes palavras ou frases reconhecíveis ao seu domínio de modelo. LUIS utiliza estas listas para adicionar significância adicional a essas palavras e expressões que seriam caso contrário não encontrados no modelo.

### <a name="patterns"></a>Padrões 
Padrões permitem-lhe simplificar a coleção de utterance do objetivo para comuns [modelos](luis-concept-patterns.md) de opção de word e a ordem do word. Isto permite LUIS saber mais rápida, pelo que necessita de menos utterances de exemplo para os pendentes. Padrões são um sistema híbrida de expressões de aprendidas de máquina e expressões regulares. 

<a name="using-luis"></a>

## <a name="authoring-and-accessing-luis"></a>Criação e aceder a LUIS
Criar a sua aplicação LUIS do Web site LUIS ou através de programação com o [criação](https://aka.ms/luis-authoring-apis) APIs ou utilização consoante a necessidade de criação. Aceder à sua aplicação LUIS publicada pela consulta [endpoint](https://aka.ms/luis-endpoint-apis). 

LUIS fornece três sites em todo o mundo, consoante a sua região de criação. A criação região determina a região do Azure, onde pode publicar a aplicação de LUIS.
<!--
|Authoring region|Publishing region(s)|
|--|--|
|[www.luis.ai](https://www.luis.ai)|**U.S.**<br>West US<br>West US 2<br>East US<br>East US 2<br>South Central US<br>West Central US<br><br>**Asia**<br>Southeast Asia<br>East Asia<br><br>**South America**<br>Brazil South |
|[au.luis.ai](https://au.luis.ai)|Australia East|
|[eu.luis.ai](https://eu.luis.ai)|West Europe<br>North Europe|
-->

Saiba [mais](luis-reference-regions.md) sobre a criação e da publicação regiões.

## <a name="what-technologies-work-with-luis"></a>Que tecnologias trabalham com LUIS?
Várias tecnologias da Microsoft trabalham com LUIS:

* [A API de verificação ortográfica do Bing](../bing-spell-check/proof-text.md) fornece a correção de texto antes de predição. 
* [Bot Framework] [ bot-framework] permite um chatbot falar com um utilizador através de introdução de texto. Selecione [3](https://github.com/Microsoft/BotBuilder) ou [4. x](https://github.com/Microsoft/botbuilder-dotnet) SDK para uma experiência de bot concluída.
* [Maker de QnA] [ qnamaker] permite vários tipos de texto para combinar para uma base de dados de conhecimento de pergunta e resposta.
* [Voz](../Speech/home.md) converte idioma ditas pedidos em texto. Depois, convertidos para texto, LUIS processa os pedidos. Consulte [SDK de reconhecimento de voz](https://aka.ms/csspeech) para obter mais informações.
* [Análise de texto](../text-analytics/overview.md) fornece dados de sentimento Analysis Services e a chave frase dados extração.

## <a name="next-steps"></a>Passos Seguintes
Criar uma nova aplicação LUIS com um [prebuilt](luis-get-started-create-app.md) ou [personalizado](luis-quickstart-intents-only.md) domínio.

<!-- Reference-style links -->
[bot-framework]: https://docs.microsoft.com/bot-framework/
[flow]: https://docs.microsoft.com/connectors/luis/
[authoring-apis]: https://aka.ms/luis-authoring-api
[endpoint-apis]: https://aka.ms/luis-endpoint-apis
[qnamaker]: https://qnamaker.ai/