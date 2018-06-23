---
title: Texto análise descrição geral da API - serviços cognitivos do Azure | Microsoft Docs
description: API de análise de texto no Azure serviços cognitivos para análise de dados de sentimento, extração de expressão de chave e deteção de idioma.
services: cognitive-services
author: ashmaka
manager: cgronlun
ms.service: cognitive-services
ms.technology: text-analytics
ms.topic: article
ms.date: 5/02/2018
ms.author: ashmaka
ms.openlocfilehash: db5ea943f270aa512afb508668aec90cc4c90df4
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355190"
---
# <a name="what-is-text-analytics-api-version-20"></a>O que é a versão 2.0 do texto análise API?

A API de análise de texto é um serviço baseado na nuvem que fornece avançada linguagem natural processamento através de texto em bruto e inclui quatro funções principais: análise de dados de sentimento, extração de expressão de chave, deteção de idioma e entidade de ligação.

A API é copiada por recursos [serviços cognitivos Microsoft](https://docs.microsoft.com/azure/cognitive-services/), uma coleção de machine learning e algoritmos de AI na nuvem, prontamente consumíveis nos seus projetos de programação.

## <a name="capabilities-in-text-analytics"></a>Capacidades de análise de texto

Análise de texto pode significar diversos elementos, mas serviços cognitivos, a API de análise de texto fornece quatro tipos de análise, tal como descrito na seguinte tabela.

| Operações| Descrição | APIs |
|-----------|-------------|------|
|[**Análise de dados de sentimento**](how-tos/text-analytics-how-to-sentiment-analysis.md) | Determinar o que os clientes considerar a marca ou tópico ao analisar o texto não processado para processáveis sobre dados de sentimento positivo ou negativo. Esta API devolve uma classificação de dados de sentimento entre 0 e 1 para cada documento, em que 1 é mais positivo.<br /> Os modelos de análise são pretrained utilizando um corpo de um vasto conjunto de tecnologias de texto e de linguagem natural a partir do Microsoft. Para [selecionado idiomas](text-analytics-supported-languages.md), a API pode analisar e Pontuar qualquer texto em bruto que fornece, diretamente a devolver resultados para a aplicação de chamada. | [REST](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c9) <br /> [.NET](https://docs.microsoft.com/en-us/azure/cognitive-services/text-analytics/quickstarts/csharp#install-the-nuget-sdk-package)  |
|[**Extração de expressão de chave**](how-tos/text-analytics-how-to-keyword-extraction.md) | Extrair automaticamente expressões chaves para identificar rapidamente os pontos principais. Por exemplo, para o texto de entrada "o prato foi delicious e ocorreram wonderful pessoal", a API devolve os pontos de talking principais: "prato" e "wonderful pessoal".  | [REST](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6) <br /> [.NET](https://docs.microsoft.com/en-us/azure/cognitive-services/text-analytics/quickstarts/csharp#install-the-nuget-sdk-package) |
|[**Deteção de idioma**](how-tos/text-analytics-how-to-language-detection.md) | Até 120 idiomas, para detetar o idioma que o texto de entrada está escrito e um código de idioma individual para cada documento submetido o pedido de relatório. O código de idioma se encontra emparelhado com uma pontuação que indica a força da pontuação. | [REST](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c7) <br />  [.NET](https://docs.microsoft.com/en-us/azure/cognitive-services/text-analytics/quickstarts/csharp#install-the-nuget-sdk-package) | 
|[**Entidade de ligação (pré-visualização)**](how-tos/text-analytics-how-to-entity-linking.md) | Identifica conhecidas entidades no texto e ligação para obter mais informações sobre o web. Entidade de ligação reconhece e disambiguates quando um termo é utilizado como uma das entidades distinguishable separadamente, verbos e outras formas de word. | [REST](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/5ac4251d5b4ccd1554da7634) | 

## <a name="typical-workflow"></a>Fluxo de trabalho típico

O fluxo de trabalho é simple: submeter dados de Analysis Services e o identificador de saídas no seu código. Analisadores são consumidos como-está, sem qualquer configuração adicional ou a personalização.

1. [Inscrever-se](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) para um [chave de acesso](how-tos/text-analytics-how-to-access-key.md). A chave tem de ser transmitida em cada pedido.

2. [Formular um pedido](how-tos/text-analytics-how-to-call-api.md#json-schema) que contém os dados como texto não estruturado não processado, no JSON.

3. Publique o pedido para o ponto final estabelecido durante a inscrição, acrescentando o recurso pretendido: análise de dados de sentimento, extração de expressão de chave, deteção de idioma ou identificação de entidade.

4. Transmitir ou armazenar a resposta localmente. Consoante o pedido, os resultados são uma classificação de dados de sentimento, uma coleção de expressões extraídos de chaves ou um código de idioma.

Resultado é devolvido como um único documento JSON, com os resultados para cada documento de texto publicados, com base no ID. Pode, subsequentemente, analisar, visualizar ou categorizar os resultados em conhecimentos acionáveis.

Dados não são armazenados na sua conta. Operações executadas pela API de análise de texto são sem monitorização de estado, o que significa que o texto que fornece é processado e os resultados são devolvidos imediatamente.

<a name="supported-languages"></a>

## <a name="supported-languages"></a>Linguagens suportadas

Esta secção foi movida para um artigo separado para melhor capacidade de deteção. Consulte [idiomas suportados na API de análise de texto](text-analytics-supported-languages.md) para este conteúdo.

<a name="data-limits"></a>

## <a name="data-limits"></a>Limites de dados

Todos os pontos finais da API de análise de texto aceitam dados não processados de texto. O limite atual é 5000 carateres para cada documento; Se precisar de analisar documentos maior, pode dividi-los em segmentos mais pequenos. Se ainda precisa de um limite superior, [contacte-nos](https://azure.microsoft.com/overview/sales-number/) para que pode discutimos os seus requisitos.

| Limite | Valor |
|------------------------|---------------|
| Tamanho máximo de um único documento | 5000 carateres, medido pela `String.Length`. |
| Tamanho máximo do pedido completo | 1 MB |
| Número máximo de documentos num pedido | 1000 documentos |

O limite de taxa é 100 chamadas por minuto. Tenha em atenção que pode submeter uma grande quantidade de documentos numa única chamada (até 1000 documentos).

## <a name="unicode-encoding"></a>A codificação de Unicode

A API de análise de texto utiliza Unicode codificação de representação de texto e cálculos de contagem de carateres. Pedidos podem ser submetidos em UTF-8 e UTF-16 com nenhuma mensuráveis diferenças na contagem de carateres. Pontos de código Unicode são utilizados como o heuristic carateres sendo considerados equivalentes para efeitos de limites de dados de análise de texto. Se utilizar `String.Length` para obter a contagem de caráter, que está a utilizar o mesmo método que utilizamos para medir o tamanho dos dados.

## <a name="next-steps"></a>Passos Seguintes

Em primeiro lugar, tente o [demonstração interativa](https://azure.microsoft.com/services/cognitive-services/text-analytics/). Pode colar uma entrada de texto (máximo de 5000 caráter) para detetar o idioma (até 120), calcular uma classificação de dados de sentimento ou extrair expressões chaves. Sem inscrição necessário.

Quando estiver pronto para chamar a API diretamente:

+ [Inscrever-se](how-tos/text-analytics-how-to-signup.md) para acesso da chave e reveja os passos para [chamar a API](how-tos/text-analytics-how-to-call-api.md).

+ [Início Rápido](quickstarts/csharp.md) é obter instruções sobre a API REST chama escrito em c#. Saiba como submeter o texto, escolha uma análise e ver resultados com código mínimas.

+ [Documentação de referência da API](//go.microsoft.com/fwlink/?LinkID=759346) fornece a documentação técnica para as APIs. A documentação suporta chamadas incorporadas, para que pode chamar a API a partir de cada página de documentação.

+ [Externo & Comunidade conteúdo](text-analytics-resource-external-community.md) fornece uma lista de mensagens de blogue e vídeos que demonstra como utilizar a análise de texto com outras tecnologias e ferramentas.

## <a name="see-also"></a>Consulte também

 [Página de documentação dos serviços cognitivos](https://docs.microsoft.com/azure/cognitive-services/)
