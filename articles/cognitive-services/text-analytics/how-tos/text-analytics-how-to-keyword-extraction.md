---
title: Extração de expressão de chave procedimentos na API de REST de análise de texto (cognitivos serviços da Microsoft no Azure) | Microsoft Docs
description: Como extrair expressões chaves, utilizando a API de REST de análise de texto no Microsoft serviços cognitivos no Azure neste tutorial de instruções.
services: cognitive-services
author: HeidiSteen
manager: cgronlun
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: article
ms.date: 3/07/2018
ms.author: heidist
ms.openlocfilehash: 78b100e737242fa9f56e50275ef2038d8895349e
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352232"
---
# <a name="how-to-extract-key-phrases-in-text-analytics"></a>Como extrair a chaves expressões na análise de texto

O [API de extração de expressão de chave](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6) avalia o texto não estruturado e para cada documento JSON, devolve uma lista de expressões de chaves. 

Esta capacidade é útil se precisar de identificar rapidamente os pontos principais de uma coleção de documentos. Por exemplo, texto de entrada especificado "o prato foi delicious e ocorreram wonderful pessoal", o serviço devolve os pontos de talking principais: "prato" e "wonderful pessoal".

Atualmente, extração de expressão de chave de suporte inglês, alemão, espanhol e japonês. Outros idiomas estão na pré-visualização. Para obter mais informações, consulte [idiomas suportados](../text-analytics-supported-languages.md).

## <a name="preparation"></a>Preparação

Extração de expressão de chave funciona melhor quando atribui-lhe maiores segmentos de texto para trabalhar. Este é oposta da análise de dados de sentimento, que tem o melhor desempenho em blocos mais pequenos de texto. Para obter os melhores resultados de ambas as operações, considere reestruturar entradas em conformidade.

Tem de ter documentos JSON neste formato: id, o texto, o idioma

Tamanho do documento tem de ser em 5000 carateres por documento e podem ter até 1000 itens (IDs) por coleção. A coleção é submetida no corpo do pedido. O exemplo seguinte é uma ilustração de conteúdo que pode submeter para extração de expressão de chave.

```
    {
        "documents": [
            {
                "language": "en",
                "id": "1",
                "text": "We love this trail and make the trip every year. The views are breathtaking and well worth the hike!"
            },
            {
                "language": "en",
                "id": "2",
                "text": "Poorly marked trails! I thought we were goners. Worst hike ever."
            },
            {
                "language": "en",
                "id": "3",
                "text": "Everyone in my family liked the trail but thought it was too challenging for the less athletic among us. Not necessarily recommended for small children."
            },
            {
                "language": "en",
                "id": "4",
                "text": "It was foggy so we missed the spectacular views, but the trail was ok. Worth checking out if you are in the area."
            },                
            {
                "language": "en",
                "id": "5",
                "text": "This is my favorite trail. It has beautiful views and many places to stop and rest"
            }
        ]
    }
```    
    
## <a name="step-1-structure-the-request"></a>Passo 1: Estrutura de pedido

Podem ser encontrados detalhes na definição de pedido no [como chamar a API de análise de texto](text-analytics-how-to-call-api.md). Os seguintes pontos são restated para sua comodidade:

+ Criar um **POST** pedido. Reveja a documentação da API para este pedido: [chave de API de expressões](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6)

+ Defina o ponto final de HTTP para extração de expressão de chave. Tem de incluir o `/keyphrases` recursos: `https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/keyPhrases`

+ Defina um cabeçalho de pedido para incluir a chave de acesso para operações de análise de texto. Para obter mais informações, consulte [como encontrar pontos finais e chaves de acesso](text-analytics-how-to-access-key.md).

+ No corpo do pedido, forneça a coleção de documentos JSON que preparou para esta análise

> [!Tip]
> Utilize [Postman](text-analytics-how-to-call-api.md) ou abrir o **consola de teste de API** no [documentação](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6) a estrutura de um pedido e PUBLICÁ-la para o serviço.

## <a name="step-2-post-the-request"></a>Passo 2: Publicar o pedido

Análise é executada após a receção do pedido. O serviço aceita até 100 pedidos por minuto. Cada pedido pode ter um máximo de 1 MB.

Recuperar-se de que o serviço está sem monitorização de estado. Nenhum dado é armazenado na sua conta. Os resultados são devolvidos imediatamente na resposta.

## <a name="step-3-view-results"></a>Passo 3: Ver resultados

Todos os pedidos POST devolverem um JSON formatado resposta com os IDs e detectadas propriedades.

Resultado é devolvido imediatamente. Pode transmitir os resultados para uma aplicação que aceite JSON ou guardar o resultado para um ficheiro no sistema local e, em seguida, importe-o para uma aplicação que permite-lhe ordenar, procurar e manipular os dados.

Um exemplo da saída para extração de expressão de chave é apresentado seguinte:

```
    "documents": [
        {
            "keyPhrases": [
                "year",
                "trail",
                "trip",
                "views"
            ],
            "id": "1"
        },
        {
            "keyPhrases": [
                "marked trails",
                "Worst hike",
                "goners"
            ],
            "id": "2"
        },
        {
            "keyPhrases": [
                "trail",
                "small children",
                "family"
            ],
            "id": "3"
        },
        {
            "keyPhrases": [
                "spectacular views",
                "trail",
                "area"
            ],
            "id": "4"
        },
        {
            "keyPhrases": [
                "places",
                "beautiful views",
                "favorite trail"
            ],
            "id": "5"
        }
```

Conforme indicado, o analisador de localiza rejeita não essencial palavras e mantém termos simples ou expressões que parecem ser o assunto ou o objeto de uma frase. 

## <a name="summary"></a>Resumo

Neste artigo, aprendeu conceitos e fluxo de trabalho para extração de expressão de chave através da análise do texto nos serviços cognitivos. Em Resumo:

+ [A chave de API de extração de expressão](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6) está disponível para os idiomas selecionados.
+ Documentos JSON no corpo do pedido incluem um código de id, o texto e o idioma.
+ É pedido POST para um `/keyphrases` ponto final, utilizando um personalizado [aceder à chave e um ponto final](text-analytics-how-to-access-key.md) que é válido para a sua subscrição.
+ Saída de resposta, composta por chave palavras e expressões diferentes para cada ID do documento, pode ser transmitida para qualquer aplicação que aceite JSON, incluindo o Excel e o Power BI, o nome algumas.

## <a name="see-also"></a>Consulte também 

 [Descrição geral da análise de texto](../overview.md)  
 [Perguntas mais frequentes (FAQ)](../text-analytics-resource-faq.md)</br>
 [Página de produto de análise de texto](//go.microsoft.com/fwlink/?LinkID=759712) 

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Análise de texto API](//westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6)
