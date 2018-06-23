---
title: A deteção de idioma procedimentos na API de REST de análise de texto (cognitivos serviços da Microsoft no Azure) | Microsoft Docs
description: Como detetar idioma utilizando a API de REST de análise de texto no Microsoft serviços cognitivos no Azure neste tutorial de instruções.
services: cognitive-services
author: HeidiSteen
manager: cgronlun
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: article
ms.date: 3/07/2018
ms.author: heidist
ms.openlocfilehash: f8e2d9a36533c298addcf42d3cb2061e9c2d1ac7
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352231"
---
# <a name="how-to-detect-language-in-text-analytics"></a>Como detetar idioma na análise de texto

O [API de deteção de linguagem](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c7) avalia o texto de entrada e para cada documento e devolve os identificadores de idioma com uma pontuação que indica a força da análise. Análise de texto reconhece até 120 idiomas.

Esta capacidade é útil para conteúdo armazena que recolher texto arbitrário, onde o idioma é desconhecido. Pode analisar os resultados nesta análise para determinar o idioma que é utilizado no documento de entrada. A resposta também devolve uma pontuação que reflete a confiança do modelo (um valor entre 0 e 1).

## <a name="preparation"></a>Preparação

Tem de ter documentos JSON neste formato: id, o texto

Tamanho do documento tem de ser em 5000 carateres por documento e podem ter até 1000 itens (IDs) por coleção. A coleção é submetida no corpo do pedido. Segue-se um exemplo de conteúdo que pode submeter para deteção de idioma.

   ```
    {
        "documents": [
            {
                "id": "1",
                "text": "This document is in English."
            },
            {
                "id": "2",
                "text": "Este documento está en inglés."
            },
            {
                "id": "3",
                "text": "Ce document est en anglais."
            },
            {
                "id": "4",
                "text": "本文件为英文"
            },                
            {
                "id": "5",
                "text": "Этот документ находится на английском языке."
            }
        ]
    }
```

## <a name="step-1-structure-the-request"></a>Passo 1: Estrutura de pedido

Podem ser encontrados detalhes na definição de pedido no [como chamar a API de análise de texto](text-analytics-how-to-call-api.md). Os seguintes pontos são restated para sua comodidade:

+ Criar um **POST** pedido. Reveja a documentação da API para este pedido: [API de deteção de idioma](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c7)

+ Defina o ponto final de HTTP para a deteção de idioma. Tem de incluir o `/languages` recursos: `https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/languages`

+ Defina um cabeçalho de pedido para incluir a chave de acesso para operações de análise de texto. Para obter mais informações, consulte [como encontrar pontos finais e chaves de acesso](text-analytics-how-to-access-key.md).

+ No corpo do pedido, forneça a coleção de documentos JSON que preparou para esta análise

> [!Tip]
> Utilize [Postman](text-analytics-how-to-call-api.md) ou abrir o **consola de teste de API** no [documentação](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c7) a estrutura de um pedido e PUBLICÁ-la para o serviço.

## <a name="step-2-post-the-request"></a>Passo 2: Publicar o pedido

Análise é executada após a receção do pedido. O serviço aceita até 100 pedidos por minuto. Cada pedido pode ter um máximo de 1 MB.

Recuperar-se de que o serviço está sem monitorização de estado. Nenhum dado é armazenado na sua conta. Os resultados são devolvidos imediatamente na resposta.


## <a name="step-3-view-results"></a>Passo 3: Ver resultados

Todos os pedidos POST devolverem um JSON formatado resposta com os IDs e detectadas propriedades.

Resultado é devolvido imediatamente. Pode transmitir os resultados para uma aplicação que aceite JSON ou guardar o resultado para um ficheiro no sistema local e, em seguida, importe-o para uma aplicação que permite-lhe ordenar, procurar e manipular os dados.

Os resultados para o pedido de exemplo deverá ser semelhante o seguinte JSON. Tenha em atenção que se trata de um documento com vários itens. O resultado é em inglês. Os identificadores de idioma incluem um nome amigável e um código de idioma no [ISO 639-1](https://www.iso.org/standard/22109.html) formato.

Uma pontuação positiva de 1.0 expresse precisa o nível de confiança possíveis mais elevado da análise.



```
{
    "documents": [
        {
            "id": "1",
            "detectedLanguages": [
                {
                    "name": "English",
                    "iso6391Name": "en",
                    "score": 1
                }
            ]
        },
        {
            "id": "2",
            "detectedLanguages": [
                {
                    "name": "Spanish",
                    "iso6391Name": "es",
                    "score": 1
                }
            ]
        },
        {
            "id": "3",
            "detectedLanguages": [
                {
                    "name": "French",
                    "iso6391Name": "fr",
                    "score": 1
                }
            ]
        },
        {
            "id": "4",
            "detectedLanguages": [
                {
                    "name": "Chinese_Simplified",
                    "iso6391Name": "zh_chs",
                    "score": 1
                }
            ]
        },
        {
            "id": "5",
            "detectedLanguages": [
                {
                    "name": "Russian",
                    "iso6391Name": "ru",
                    "score": 1
                }
            ]
        }
    ],
```

### <a name="ambiguous-content"></a>Conteúdo ambíguo

Se o analisador de não é possível analisar a entrada (por exemplo, suponha submetido um bloco de texto consiste apenas em numerais Arabic), devolve `(Unknown)`.

```
    {
      "id": "5",
      "detectedLanguages": [
        {
          "name": "(Unknown)",
          "iso6391Name": "(Unknown)",
          "score": "NaN"
        }
      ]
```
### <a name="mixed-language-content"></a>Linguagem mista conteúda

Conteúdo misto de idioma no mesmo documento devolve o idioma com a representação maior no conteúdo, mas com uma classificação positiva inferior, ao refletir a força marginal de nessa avaliação. No exemplo seguinte, a entrada é um blend do inglês, espanhol e francês. O analisador de contagens de carateres em cada segmento para determinar o idioma predominant.

**Input (Entrada)**

```
{
  "documents": [
    {
      "id": "1",
      "text": "Hello, I would like to take a class at your University. ¿Se ofrecen clases en español? Es mi primera lengua y más fácil para escribir. Que diriez-vous des cours en français?"
    }
  ]
}
```

**Saída**

A saída resultante é constituído por predominant idioma, com uma pontuação de menor que 1,0, que indica um nível de confiança mais fraco.

```
{
  "documents": [
    {
      "id": "1",
      "detectedLanguages": [
        {
          "name": "Spanish",
          "iso6391Name": "es",
          "score": 0.9375
        }
      ]
    }
  ],
  "errors": []
}
```

## <a name="summary"></a>Resumo

Neste artigo, aprendeu conceitos e fluxo de trabalho para a deteção de idioma através da análise do texto nos serviços cognitivos. Seguem-se um lembrete rápido dos objetivos principais anteriormente explicado e demonstrou:

+ [Deteção de idioma API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c7) está disponível para 120 idiomas.
+ Documentos JSON no corpo do pedido incluem um id e texto.
+ É pedido POST para um `/languages` ponto final, utilizando um personalizado [aceder à chave e um ponto final](text-analytics-how-to-access-key.md) que é válido para a sua subscrição.
+ Saída de resposta, que consiste em identificadores de idioma para cada ID do documento, pode ser transmitida para qualquer aplicação que aceite JSON, incluindo o Excel e o Power BI, o nome algumas.

## <a name="see-also"></a>Consulte também 

 [Descrição geral da análise de texto](../overview.md)  
 [Perguntas mais frequentes (FAQ)](../text-analytics-resource-faq.md)</br>
 [Página de produto de análise de texto](//go.microsoft.com/fwlink/?LinkID=759712) 

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Analisar dados de sentimento](text-analytics-how-to-sentiment-analysis.md)
