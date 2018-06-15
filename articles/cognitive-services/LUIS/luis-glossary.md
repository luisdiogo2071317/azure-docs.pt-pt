---
title: Glossário para o serviço de API do idioma compreender (LUIS) | Microsoft Docs
description: O glossário explica certos termos que podem surgir à medida que trabalha com o serviço de API LUIS.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: v-geberr
ms.openlocfilehash: 420f268090dbcfcc4f2fa7383b8b8892952030ca
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "35355760"
---
# <a name="glossary"></a>Glossário

## <a name="active-version"></a>Versão do Active Directory

A versão de LUIS Active Directory é a versão que recebe todas as alterações ao modelo. No [LUIS](luis-reference-regions.md) Web site, se pretender efetuar alterações a uma versão que não é a versão do Active Directory, terá de definir primeiro essa versão como o Active Directory. 

## <a name="authoring"></a>Criação

Criação é a capacidade de criar, gerir e implementar uma [LUIS aplicação](#luis-app), ou utilizando o [LUIS](luis-reference-regions.md) Web site ou a [criação APIs](https://aka.ms/luis-authoring-api). 

## <a name="authoring-key"></a>Chave de criação

Anteriormente denominado "Programmatic" chave. Utilizado para criar a aplicação. Não é utilizado para consultas de ponto final de nível de produção. Para obter mais informações, consulte [chave limites](luis-boundaries.md#key-limits).   

## <a name="batch-test-json-file"></a>Ficheiro JSON de texto do batch

O ficheiro batch é uma matriz JSON. Cada elemento da matriz tem três propriedades: `text`, `intent`, e `entities`. O `entities` propriedade é uma matriz. A matriz pode estar vazia. Se o `entities` matriz não está vazia, tem de identificar com precisão as entidades.

```JSON
[
    {
        "text": "drive me home",
        "intent": "None",
        "entities": []
    },
    {
        "text": "book a flight to orlando on the 25th",
        "intent": "BookFlight",
        "entities": [
            {
                "entity": "orlando",
                "type": "Location",
                "startIndex": 18,
                "endIndex": 25
            }
        ]
    }
]

```


## <a name="collaborator"></a>Colaborador

Não é um funcionário o [proprietário](#owner) da aplicação, mas tem as mesmas permissões para adicionar, editar e eliminar o pendentes, entidades, utterances.

## <a name="currently-editing"></a>Atualmente a editar

Igual ao [versão Active Directory](#active-version)

## <a name="domain"></a>domínio

No contexto LUIS, um **domínio** é uma área de dados de conhecimento. O domínio é específico para a sua área de aplicação de dados de conhecimento. Isto pode ser uma área geral, tais como a aplicação levar agente. Uma aplicação de agente levar também pode ser específica para apenas as áreas de informações da sua empresa, como localizações geográficas específicas, idiomas e serviços. 

## <a name="endpoint"></a>ponto final

O [LUIS endpoint](https://aka.ms/luis-endpoint-apis) URL é onde pode submete consultas LUIS após o [LUIS aplicação](#luis-app) é criados e publicados. O URL de ponto final contém a região da aplicação publicada, bem como o ID de aplicação. Pode encontrar o ponto final no **[publicar](publishapp.md)** página da sua aplicação, na tabela de recursos e as chaves ou pode obter o URL do ponto final do [obter as informações da aplicação](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c37) API.

Um ponto final de exemplo que se pareça com:

`https://<region>.api.cognitive.microsoft.com/luis/v2.0/apps/<appID>?subscription-key=<subscriptionID>&verbose=true&timezoneOffset=0&q=<utterance>`

|Parâmetro de cadeia de consulta|descrição|
|--|--|
|Região| [região publicada](luis-reference-regions.md#publishing-regions) |
|appID | ID da aplicação LUIS |
|SubscriptionID | Chave de subscrição de LUIS criada no portal do Azure |
|Q | utterance |
|timezoneOffset| minutes|

## <a name="entity"></a>Entidade

[Entidades](luis-concept-entity-types.md) são importantes palavras no [utterances](luis-concept-utterance.md) descrevem que informações relevantes para o [intenção](luis-concept-intent.md), e, por vezes, são essenciais para a mesma. Uma entidade é, essencialmente, um tipo de dados no LUIS. 

## <a name="f-measure"></a>Medida de F

No [batch testar][batch-testing], uma medida de precisão o teste.

## <a name="false-negative"></a>Falso negativo (TW)

No [batch testar][batch-testing], os pontos de dados representam utterances na qual a aplicação incorretamente prever a ausência de intenção/entidade de destino.

## <a name="false-positive"></a>Falsos positivos (TP)

No [batch testar][batch-testing], os pontos de dados representam utterances na qual a aplicação incorretamente prever a existência de intenção/entidade de destino.

## <a name="features"></a>Funcionalidades

No machine learning, um [funcionalidade](luis-concept-feature.md) é um trait distintivo ou um atributo de dados situa do seu sistema.

## <a name="intent"></a>Objetivo

Um [intenção](luis-concept-intent.md) representa uma tarefa ou ação que o utilizador pretende efetuar. É um objetivo ou objetivo expressado na entrada de um utilizador, tais como booking um voo, prestando uma fatura ou ao localizar um artigo de notícias de última hora. Nos LUIS, a intenção predição é baseada no utterance completo. As entidades, por comparação, são peças de uma utterance.

## <a name="labeling"></a>etiquetagem

Etiquetagem é o processo de associar uma palavra ou frase numa intenção [utterance](#utterance) com um [entidade](#entity) (tipo de dados). 

## <a name="luis-app"></a>Aplicação de LUIS

Uma aplicação de LUIS é um modelo de dados de formação para processamento de linguagem natural incluindo [pendentes](#intent), [entidades](#entity)e identificados [utterances](#utterance).

## <a name="owner"></a>Proprietário

Cada aplicação tem um proprietário que seja a pessoa que criou a aplicação. O proprietário pode adicioná [colaboradores](#collaborator).

## <a name="pattern"></a>Padrões
A funcionalidade de padrão anterior é substituída por [padrões](luis-concept-patterns.md). Utilize padrões para melhorar a exatidão da previsão fornecendo menos exemplos de formação. 

## <a name="phrase-list"></a>Lista de expressão

A [lista frase](luis-concept-feature.md#what-is-a-phrase-list-feature) inclui um grupo de valores (palavras ou frases reconhecíveis) que pertencem à mesma classe e tem de ser tratados da mesma forma (por exemplo, nomes de cidades ou produtos). Uma lista de permutáveis é tratada como sinónimos. 

## <a name="prebuilt-domains"></a>Domínio prebuilt

A [domínio prebuilt](luis-how-to-use-prebuilt-domains.md) é uma aplicação de LUIS configurada para um domínio específico, como automatização inicial (HomeAutomation) ou reservas restaurante (RestaurantReservation). O pendentes, utterances e entidades estão configuradas para este domínio. 

## <a name="prebuilt-entity"></a>Entidade prebuilt

A [entidade prebuilt](pre-builtentities.md) é uma entidade LUIS fornece para tipos comuns de informações como o número, o URL e o e-mail. Optar por adicionar uma entidade prebuilt à sua aplicação. 

## <a name="precision"></a>precisão
No [batch testar][batch-testing], precisão (também denominada positivo preditivo) é a fração do utterances relevantes entre os utterances obtidos.

## <a name="programmatic-key"></a>Chave programático

Cujo nome foi alterado para [criação chave](#authoring-key). 

## <a name="publish"></a>Publicar

Publicar significa efetuar um LUIS [versão Active Directory](#active-version) disponível no teste ou de produção [endpoint](#endpoint).  

## <a name="quota"></a>quota

Quota de LUIS é a limitação do [camada de subscrição do Azure](https://aka.ms/luis-price-tier). A quota de LUIS pode ser limitada por ambos os pedidos por segundo (429 de estado de HTTP) e o total de pedidos num mês (estado de HTTP 403). 

## <a name="recall"></a>Recuperar
No [batch testar][batch-testing], recuperar (também conhecido como sensibilidade), que é a capacidade de LUIS generalizar. 

## <a name="semantic-dictionary"></a>Dicionário de semântico
Um dicionário semântico é fornecido na página de entidade de lista, bem como a página de lista de expressão. O dicionário semântico fornece sugestões de palavras com base no âmbito atual.

## <a name="sentiment-analysis"></a>Análise de dados de sentimento
Análise de dados de sentimento fornece valores positivos ou negativos de utterances fornecidas pelo [análise de texto](https://azure.microsoft.com/services/cognitive-services/text-analytics/). 

## <a name="speech-priming"></a>Priming de reconhecimento de voz

Priming de reconhecimento de voz permite que o serviço de reconhecimento de voz primed com o seu modelo LUIS. Consulte [ativar priming de reconhecimento de voz ](publishapp.md#enable-speech-priming).

## <a name="spelling-correction"></a>Correção de ortografia

Na página de publicar, ative [Verificador de ortográfica do Bing](publishapp.md#enable-bing-spell-checker) para corrigir com erros ortográficos palavras no utterances antes de predição. 

## <a name="starter-key"></a>Chave de arranque

Igual ao [chave programático](#programmatic-key), cujo nome foi alterado a chave de criação.

## <a name="subscription-key"></a>Chave de subscrição

A chave de subscrição é a chave associada ao serviço LUIS [criada no Azure](luis-how-to-azure-subscription.md). Esta chave não estiver a [criação chave](#programmatic-key). Se tiver uma chave de subscrição, deve ser utilizada para quaisquer pedidos de ponto final em vez da chave de criação. Pode ver a sua chave de subscrição atual no interior do URL de ponto final na parte inferior [ **publicar aplicação** página](publishapp.md) no [LUIS](luis-reference-regions.md) Web site. É o valor de **chave de subscrição** nome/valor par. 

## <a name="test"></a>Teste

[Testar](train-test.md#test-your-app) uma aplicação LUIS significa resulta de transmitir um utterance ao LUIS e visualizar o JSON.

## <a name="timezoneoffset"></a>Desvio de fuso horário

O ponto final inclui timezoneOffset. Este é o número em minutos que pretende adicionar ou remover o datetimeV2 prebuilt entidade. Por exemplo, se for o utterance "altura é agora?", datetimeV2 devolvido é o tempo atual para o pedido de cliente. Se o pedido de cliente é proveniente de um bot ou outra aplicação que não é o mesmo que o utilizador do seu bot, deverá passar no desvio entre o bot e o utilizador. 

Consulte [alteração fuso horário da entidade prebuilt datetimeV2](luis-concept-data-alteration.md?#change-time-zone-of-prebuilt-datetimev2-entity).

## <a name="token"></a>Token
Um token é a unidade de menor que pode ser assinalada como uma entidade. Atomização baseia-se a aplicação [cultura](luis-supported-languages.md#tokenization).

## <a name="train"></a>Formação

Formação é o processo de teaching LUIS sobre as alterações para o [versão Active Directory](#active-version) desde a última formação.

## <a name="true-negative"></a>Verdadeiro negativo (TW)

No [batch testar][batch-testing], os pontos de dados representam utterances na qual a aplicação corretamente prever a ausência de intenção/entidade de destino.

## <a name="true-positive"></a>Verdadeiro positivo (TP)

No [batch testar][batch-testing], os pontos de dados representam utterances na qual a aplicação corretamente prever a existência de intenção/entidade de destino.

## <a name="utterance"></a>Utterance

Um utterance é uma expressão de linguagem natural como "permissões de livro 2 para Seattle seguinte Terça-feira". Utterances de exemplo são adicionadas para o objetivo. 

## <a name="version"></a>Versão

Um LUIS [versão](luis-how-to-manage-versions.md) é um modelo de dados específicos associado a um ID de aplicação LUIS e o ponto final publicado. Todas as aplicações de LUIS tem, pelo menos, uma versão.

[batch-testing]: train-test.md#batch-testing
