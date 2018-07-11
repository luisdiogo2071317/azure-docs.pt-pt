---
title: Sobre compreensão de idiomas (LUIS) no Azure | Documentos da Microsoft
description: Saiba como utilizar a compreensão de idiomas (LUIS) para trazer o poder do machine learning para seus aplicativos.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/22/2017
ms.author: v-geberr
ms.openlocfilehash: 1c68c586fb799a540f70804d181aa66b2bda9e97
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/10/2018
ms.locfileid: "37952543"
---
# <a name="what-is-language-understanding-luis"></a>O que é a compreensão de idiomas (LUIS)?
Compreensão de idiomas (LUIS) é um serviço baseado na nuvem que se aplica a aprendizagem automática personalizada ao texto de linguagem natural conversacional, um utilizador para prever o significado geral e extrair informações relevantes e detalhadas. 

Uma aplicação de cliente para LUIS pode ser qualquer conversacional aplicativo que se comunique com um utilizador em linguagem natural para concluir uma tarefa. Exemplos de aplicativos de cliente incluem aplicações de redes sociais, chatbots e aplicativos de área de trabalho habilitado para fala.  

![Imagem conceptual de 3 aplicativos manutenção informações de informações LUIS](./media/luis-overview/luis-entry-point.png)

## <a name="what-is-a-luis-app"></a>O que é uma aplicação do LUIS?
Uma aplicação do LUIS contém um modelo de linguagem natural de domínios específicos que é criar. Pode iniciar a sua aplicação LUIS com um modelo de domínio pré-criado, crie a sua própria ou combiná-los partes de um domínio pré-criado com suas próprias informações personalizadas.

[Modelos de domínio pré-criado](luis-how-to-use-prebuilt-domains.md) incluem todas essas peças para si e são uma ótima maneira de começar a utilizar o LUIS rapidamente.

A aplicação do LUIS também contém as definições de integração [colaboradores](luis-concept-collaborator.md), e [versões](luis-concept-version.md).

## <a name="using-a-luis-app"></a>Utilização de uma aplicação LUIS
<a name="Accessing-LUIS"></a> Assim que a sua aplicação LUIS é publicada, a aplicação cliente envia expressões com para o LUIS [ponto final de API] [ endpoint-apis] e recebe os resultados da predição como respostas JSON.

Em primeiro lugar no diagrama seguinte, chatbot seu cliente envia o texto do utilizador de que uma pessoa quer em suas próprias palavras para LUIS numa solicitação HTTP. Em segundo lugar, LUIS aplica-se seu modelo de aprender a linguagem natural dar sentido a entrada do usuário e retorna uma resposta de formato de JavaScript Object Notation (JSON). Em terceiro lugar, o seu chatbot de cliente utiliza a resposta JSON para satisfazer os pedidos do utilizador. 

![Imagens conceitual de trabalhar com Chatbot de LUIS](./media/luis-overview/luis-overview-process-2.png)

### <a name="example-of-json-endpoint-response"></a>Exemplo de resposta do ponto final JSON

A resposta do ponto final JSON, no mínimo contém a expressão de consulta e a parte superior a intenção de classificação. 

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
Um modelo começa com uma lista de intenções de utilizador geral, chamado _intenções_, como "Reservar vôo" ou "Contacte o suporte técnico ajuda". Forneça o texto de exemplo do usuário, chamado _expressões de exemplo_ para os objetivos. Em seguida, marcamos significativas palavras ou frases na expressão, chamado _entidades_.


Um modelo inclui:

* **[objetivos](#intents)**: categorias de intenções de utilizador (ação pretendida ou resultado)
* **[entidades](#entities)**: tipos específicos de dados em expressões com como nome, e-mail ou número
* **[expressões com de exemplo](#example-utterances)**: texto de exemplo, um usuário insere no aplicativo cliente

### <a name="intents"></a>Intenções 
Uma [intenção](luis-how-to-add-intents.md), abreviação de _intenção_, é um objetivo ou objetivo expressos na expressão de um utilizador, tais como reserva um vôo, pagando uma fatura ou ao localizar um artigo de notícias. Criar um objetivo de cada ação. Uma aplicação de viagens de LUIS pode definir um objetivo com o nome "BookFlight." A aplicação cliente pode utilizar a parte superior a intenção de classificação para acionar uma ação. Por exemplo, quando a intenção de "BookFlight" é devolvida do LUIS, a aplicação cliente poderia acionar uma chamada à API para um serviço externo para reserva um pedido de suporte do plano.

### <a name="entities"></a>Entidades
Uma [entidade](luis-how-to-add-entities.md) representa informações detalhadas encontradas dentro da expressão que é relevante para o pedido do utilizador. Por exemplo, na expressão "Livro um pedido para Paris", um pedido de suporte único é solicitado, e "Paris" for uma localização. Duas entidades encontram-se "um pedido de suporte" que indica um pedido de suporte único e "Paris", que indica que o destino. 

Depois de LUIS devolve as entidades encontradas na expressão do usuário, a aplicação cliente pode utilizar a lista de entidades como parâmetros para acionar uma ação. Por exemplo, reserva um voo requer entidades, como o destino de viagens, a data e a companhia aérea.

LUIS proporciona várias formas de identificar e categorizar entidades.

* **Entidades pré-concebidas** LUIS tem muitos modelos de domínio pré-criado incluindo objetivos, expressões, e [entidades pré-concebidas](luis-prebuilt-entities.md). Pode utilizar as entidades pré-concebidas sem ter de utilizar as intenções e expressões com do modelo criados previamente. Entidades pré-concebidas poupar tempo.

* **Entidades personalizadas** LUIS dá-lhe várias formas de identificar o seu próprio [entidades](luis-concept-entity-types.md) incluindo entidades aprendidas por máquina, entidades literais ou específicas e uma combinação de literal e aprendeu por máquina.

### <a name="example-utterances"></a>Expressões de exemplo
Um exemplo [expressão](luis-how-to-add-example-utterances.md) é a introdução de texto do usuário que a aplicação cliente tem de compreender. Pode ser uma frase, como "Reservar um pedido de suporte para Paris", ou um fragmento de uma frase, como "Reserva" ou "Voo Paris." Expressões com nem sempre são bem formados e podem existir muitas variações de expressão para um objetivo específico. Adicionar expressões de exemplo de 10 a 20 em cada intenção e marcar entidades em cada expressão.

|Expressão de utilizador de exemplo|Intenção|Entidades|
|-----------|-----------|-----------|
|"Programar um vôo para __Seattle__?"|BookFlight|Seattle|
|"Quando faz a sua loja __abra__?"|StoreHoursAndLocation|Abrir|
|"Agendar uma reunião em __1 pm__ com __Bob__ na distribuição de"|ScheduleMeeting|1 pm, Bob|

## <a name="improve-prediction-accuracy"></a>Melhorar a precisão da predição
Após a sua aplicação LUIS é publicada e recebe as expressões de utilizador real, LUIS, fornece vários métodos para melhorar a exatidão da previsão: [aprendizagem ativa](#active-learning) de expressões de ponto de extremidade, [frase listas](#phrase-lists) para o domínio inclusão, do Word e [padrões](#patterns) para reduzir o número de expressões com necessários.

### <a name="active-learning"></a>Aprendizagem ativa
Na [aprendizagem ativa](luis-how-to-review-endoint-utt.md) processo, LUIS, pode adaptar-se a sua aplicação LUIS para expressões do mundo real com selecionando expressões que recebeu no ponto final para revisão. Pode aceitar ou corrija a predição de ponto de extremidade, voltar a preparar e volte a publicar. LUIS aprende rapidamente com esse processo interativo, levando a quantidade mínima de seu tempo e esforço. 

### <a name="phrase-lists"></a>Listas de expressões 
LUIS fornece [frases listas](luis-concept-feature.md) para que pode indicar importantes palavras ou frases ao seu domínio de modelo. LUIS utiliza estas listas para adicionar o significado adicional a essas palavras e frases que caso contrário, não seriam encontrados no modelo.

### <a name="patterns"></a>Padrões 
Padrões permitem-lhe simplificar a coleção de expressão de um objetivo em comum [modelos](luis-concept-patterns.md) de escolha do word e a ordem das palavras. Isso permite que o LUIS saber mais rápida, pelo que precisam de menos expressões de exemplo para os objetivos. Os padrões são um sistema híbrido de expressões regulares e expressões aprendidas por máquina. 

<a name="using-luis"></a>

## <a name="authoring-and-accessing-luis"></a>Criação e acessar o LUIS
Criar a sua aplicação LUIS partir do site do LUIS ou programaticamente com a [criação](https://aka.ms/luis-authoring-apis) APIs ou utilização consoante a necessidade de criação. Aceder à aplicação LUIS publicada pela consulta [ponto final](https://aka.ms/luis-endpoint-apis). 

LUIS fornece três Web sites em todo o mundo, consoante a sua região de criação. A região de criação determina a região do Azure, onde pode publicar a aplicação do LUIS.
<!--
|Authoring region|Publishing region(s)|
|--|--|
|[www.luis.ai](https://www.luis.ai)|**U.S.**<br>West US<br>West US 2<br>East US<br>East US 2<br>South Central US<br>West Central US<br><br>**Asia**<br>Southeast Asia<br>East Asia<br><br>**South America**<br>Brazil South |
|[au.luis.ai](https://au.luis.ai)|Australia East|
|[eu.luis.ai](https://eu.luis.ai)|West Europe<br>North Europe|
-->

Saiba mais [mais](luis-reference-regions.md) sobre a criação e publicação de regiões.

## <a name="what-technologies-work-with-luis"></a>Quais tecnologias funcionam com os LUIS?
Várias tecnologias da Microsoft de trabalho com os LUIS:

* [A API de verificação ortográfica do Bing](../bing-spell-check/proof-text.md) fornece a correção de texto antes de predição. 
* [Arquitetura de bot] [ bot-framework] permite que um chatbot conversar com um usuário por meio da introdução de texto. Selecione [3.x](https://github.com/Microsoft/BotBuilder) ou [4.x](https://github.com/Microsoft/botbuilder-dotnet) SDK para uma experiência completa de bot.
* [A ferramenta QnA Maker] [ qnamaker] permite vários tipos de texto para combinar numa base de dados de conhecimento de perguntas e respostas.
* [Voz](../Speech/home.md) converte os pedidos de um idioma falado em texto. Após a conversão de texto, LUIS processa as solicitações. Ver [SDK de voz](https://aka.ms/csspeech) para obter mais informações.
* [Análise de texto](../text-analytics/overview.md) fornece sentimentos e a chave da análise de dados extração de expressões.

## <a name="next-steps"></a>Passos Seguintes
Criar uma nova aplicação LUIS com um [criados previamente](luis-get-started-create-app.md) ou [personalizado](luis-quickstart-intents-only.md) domínio.

<!-- Reference-style links -->
[bot-framework]: https://docs.microsoft.com/bot-framework/
[flow]: https://docs.microsoft.com/connectors/luis/
[authoring-apis]: https://aka.ms/luis-authoring-api
[endpoint-apis]: https://aka.ms/luis-endpoint-apis
[qnamaker]: https://qnamaker.ai/