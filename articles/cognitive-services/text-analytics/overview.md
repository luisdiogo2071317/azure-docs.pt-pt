---
title: Descrição geral análise de texto - serviços cognitivos do Azure | Documentos da Microsoft
description: Análise de texto nos serviços cognitivos do Azure para análise de sentimentos, extração de expressões-chave, deteção de idioma e ligação de entidades.
services: cognitive-services
author: ashmaka
manager: cgronlun
ms.service: cognitive-services
ms.technology: text-analytics
ms.topic: article
ms.date: 8/30/2018
ms.author: ashmaka
ms.openlocfilehash: 71af2bcbf58279681bbea66d4f76e951a2efce59
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/31/2018
ms.locfileid: "43341491"
---
# <a name="what-is-text-analytics"></a>O que é a análise de texto?

O serviço de análise de texto fornece processamento de texto não estruturado bruto avançado de linguagem natural. Ele inclui quatro funções principais: análise de sentimentos, extração de expressões-chave, deteção de idioma e ligação de entidades.

## <a name="analyze-sentiment"></a>Analisar sentimento

[Descubra](how-tos/text-analytics-how-to-sentiment-analysis.md) o que os clientes pensam da sua marca ou tópico através da análise de texto não processado para dicas sobre o sentimento positivo ou negativo. Esta API devolve uma pontuação de sentimento entre 0 e 1 para cada documento, em que 1 é o mais positivo.<br />
Os modelos de análise são pré-preparadas com um corpo extenso de tecnologias de texto e de linguagem natural da Microsoft a utilizar. Para [selecionado idiomas](text-analytics-supported-languages.md), a API pode analisar e classificar qualquer texto não processado que fornecer.

## <a name="extract-key-phrases"></a>Extrair expressões-chave

Automaticamente [extrair expressões-chave](how-tos/text-analytics-how-to-keyword-extraction.md) a identificar rapidamente os pontos principais. Por exemplo, com a introdução de texto "alimentar foi delicious e havia maravilhoso equipe", o serviço de análise de texto devolve os principais pontos de conversa: "food" e "funcionário maravilhoso".

## <a name="detect-language"></a>Detetar idioma

Para até 120 idiomas, [detetar](how-tos/text-analytics-how-to-language-detection.md) quais idiomas a introdução de texto é escrito em e um código de idioma único para cada documento enviado no pedido de relatório. O código de idioma é emparelhado com uma pontuação indicando a força da pontuação.

## <a name="identify-linked-entities-preview"></a>Identificar as entidades associadas (pré-visualização)

[Identificar](how-tos/text-analytics-how-to-entity-linking.md) entidades conhecidas no seu texto e a ligação para obter mais informações na web. Ligação de entidades reconhece e remove a ambigüidade dos quando um termo é usado como uma das entidades distintas, verbos e outras formas de palavras.

## <a name="typical-workflow"></a>Fluxo de trabalho típico

O fluxo de trabalho é simples: envia dados para análise e identificador de saídas em seu código. Analisadores são consumidos como-é, sem qualquer configuração adicional ou personalização.

1. [Inscreva-se](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) para uma [chave de acesso](how-tos/text-analytics-how-to-access-key.md). A chave tem de ser transmitida em cada pedido.

2. [Criar um pedido de](how-tos/text-analytics-how-to-call-api.md#json-schema) em JSON que contém os dados como texto não estruturado bruto.

3. Publicar o pedido para o ponto de extremidade estabelecido durante a inscrição, acrescentando a API que pretende chamar: análise de sentimentos, extração de expressões-chave, deteção de idioma ou identificação de entidade.

4. Stream ou armazenar a resposta localmente. Dependendo do pedido, os resultados são uma pontuação de sentimento, uma coleção de expressões-chave extraído ou um código de idioma.

Resultado é devolvido como um único documento JSON, com resultados para cada documento de texto postado, com base no ID. Em seguida, pode analisar, visualizar ou categorizar os resultados em informações acionáveis.

Operações feitas pelo serviço de análise de texto são sem monitoração de estado. Dados não são armazenados na sua conta.

<a name="data-limits"></a>

## <a name="specifications"></a>Especificações

### <a name="supported-languages"></a>Linguagens suportadas

Ver [idiomas suportados na análise de texto](text-analytics-supported-languages.md).

### <a name="data-limits"></a>Limites de dados

Todos os pontos de extremidade do serviço de análise de texto aceitam dados de texto não processado. O limite atual é de 5000 carateres para cada documento; Se precisar de analisar documentos maiores, poderá dividi-las em segmentos mais pequenos. Se necessitar de um limite maior, ainda [contacte-nos](https://azure.microsoft.com/overview/sales-number/) , de modo que podemos analisar os seus requisitos.

| Limite | Valor |
|------------------------|---------------|
| Tamanho máximo de um documento único | 5000 carateres conforme medido pela `String.Length`. |
| Tamanho máximo do pedido completo | 1 MB |
| Número máximo de documentos num pedido | 1000 documentos |

O limite de taxa é de 100 chamadas por minuto. Tenha em atenção de que pode enviar uma grande quantidade de documentos numa única chamada (até 1000 documentos).

### <a name="unicode-encoding"></a>Codificação Unicode

O serviço de análise de texto usa codificação Unicode para cálculos de contagem de caracteres e representação de texto. Pode submeter pedidos em UTF-8 ou UTF-16, com nenhuma diferenças mensuráveis na contagem de caracteres. Se usar `String.Length` para obter a contagem de caracteres, estiver a utilizar o mesmo método que usamos para medir o tamanho dos dados.

## <a name="next-steps"></a>Passos Seguintes

Em primeiro lugar, tente o [demonstração interativa](https://azure.microsoft.com/services/cognitive-services/text-analytics/). Pode colar uma entrada de texto (máximo de 5.000 caracteres) para detetar o idioma (até 120), calcular uma pontuação de sentimento, extrair expressões-chave ou identificar entidades associadas. Sem precisar de inscrição é necessário.

Quando estiver pronto para chamar o serviço de análise de texto diretamente:

+ [Inscreva-se](how-tos/text-analytics-how-to-signup.md) para um acesso de chave e reveja os passos para [chamar a API](how-tos/text-analytics-how-to-call-api.md).

+ [Início Rápido](quickstarts/csharp.md) é um passo a passo da REST API chama escrita em c#. Aprenda a submeter o texto, escolher uma análise e ver os resultados com pouco código.

+ [Documentação de referência de API](//go.microsoft.com/fwlink/?LinkID=759346) fornece a documentação técnica para as APIs REST. A documentação oferece suporte a chamadas incorporadas, para que possa chamar a API de cada página de documentação.

+ [Externa & conteúdo da Comunidade](text-analytics-resource-external-community.md) fornece uma lista de postagens de blog e vídeos que demonstram como utilizar a análise de texto com outras ferramentas e tecnologias.

## <a name="see-also"></a>Consulte também

 [Página de documentação dos serviços cognitiva](https://docs.microsoft.com/azure/cognitive-services/)
