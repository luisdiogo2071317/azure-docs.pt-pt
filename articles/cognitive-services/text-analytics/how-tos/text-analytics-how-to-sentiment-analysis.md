---
title: Análise de dados de sentimento procedimentos na API de REST de análise de texto (cognitivos serviços da Microsoft no Azure) | Microsoft Docs
description: Como detetar sentimento utilizando a API de REST de análise de texto no Microsoft serviços cognitivos no Azure neste tutorial de instruções.
services: cognitive-services
author: HeidiSteen
manager: cgronlun
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: article
ms.date: 12/11/2017
ms.author: heidist
ms.openlocfilehash: 7ffd8bbe47409b459fdd308cd8d670d32f56649b
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352237"
---
# <a name="how-to-detect-sentiment-in-text-analytics"></a>Como detetar sentimento na análise de texto

O [API de análise de dados de sentimento](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c9) avalia a introdução de texto e devolve uma classificação de dados de sentimento para cada documento, entre 0 (negativo) e 1 (positivos).

Esta capacidade é útil para detetar sentimento positivo e negativo de redes sociais, revisões de cliente e fóruns de debate. Conteúdo é fornecido por si; modelos e dados de formação são fornecidos pelo serviço.

Atualmente, suporta a análise de dados de sentimento do inglês, alemão, espanhol e francês. Outros idiomas estão na pré-visualização. Para obter mais informações, consulte [idiomas suportados](../text-analytics-supported-languages.md).

## <a name="concepts"></a>Conceitos

Análise de texto utiliza um algoritmo de classificação de aprendizagem para gerar uma classificação de dados de sentimento entre 0 e 1. Pontuações próximo como 1 indicam sentimento positivo, enquanto pontuações próximo de 0 indicam sentimento negativo. O modelo é pretrained com um corpo de um vasto conjunto de texto com associações de sentimento. Atualmente, não é possível fornecer os seus próprios dados de formação. O modelo utiliza uma combinação de técnicas durante a análise de texto, incluindo o processamento de texto, análise de parte de voz, colocação do word e associações do word. Para obter mais informações sobre o algoritmo, consulte [análise de texto de introdução](https://blogs.technet.microsoft.com/machinelearning/2015/04/08/introducing-text-analytics-in-the-azure-ml-marketplace/).

Análise de dados de sentimento é efetuada em todo o documento, por oposição a extrair dados de sentimento para uma determinada entidade no texto. Na prática, não há uma tendência para classificação precisão para melhorar quando documentos contêm uma ou duas frases em vez de um bloco grande de texto. Durante uma fase de avaliação objectivity, o modelo determina se um documento como um todo é objetivo ou se contém dados de sentimento. Um documento que tem principalmente objetivo não não progresso para a frase de deteção de dados de sentimento, causando uma pontuação.50, com qualquer processamento adicional. Para documentos continuar no pipeline, a fase seguinte gera uma pontuação acima ou abaixo.50, consoante o grau de sentimento detetado no documento.

## <a name="preparation"></a>Preparação

Análise de dados de sentimento produz um resultado de qualidade superior quando atribui-lhe segmentos mais pequenos de texto para trabalhar. Este é oposta de extração de expressão de chave, que tem o melhor desempenho em blocos maior de texto. Para obter os melhores resultados de ambas as operações, considere reestruturar entradas em conformidade.

Tem de ter documentos JSON neste formato: id, o texto, o idioma

Tamanho do documento tem de ser em 5000 carateres por documento e podem ter até 1000 itens (IDs) por coleção. A coleção é submetida no corpo do pedido. Segue-se um exemplo de conteúdo que pode submeter para análise de dados de sentimento.

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

+ Criar um **POST** pedido. Reveja a documentação da API para este pedido: [sentimento Analysis API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c9)

+ Defina o ponto final de HTTP para extração de expressão de chave. Tem de incluir o `/sentiment` recursos: `https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/sentiment`

+ Defina um cabeçalho de pedido para incluir a chave de acesso para operações de análise de texto. Para obter mais informações, consulte [como encontrar pontos finais e chaves de acesso](text-analytics-how-to-access-key.md).

+ No corpo do pedido, forneça a coleção de documentos JSON que preparou para esta análise.

> [!Tip]
> Utilize [Postman](text-analytics-how-to-call-api.md) ou abrir o **consola de teste de API** no [documentação](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c9) para estruturar o pedido e PUBLICÁ-la para o serviço.

## <a name="step-2-post-the-request"></a>Passo 2: Publicar o pedido

Análise é executada após a receção do pedido. O serviço aceita até 100 pedidos por minuto. Cada pedido pode ter um máximo de 1 MB.

Recuperar-se de que o serviço está sem monitorização de estado. Nenhum dado é armazenado na sua conta. Os resultados são devolvidos imediatamente na resposta.


## <a name="step-3-view-results"></a>Passo 3: Ver resultados

O analisador de sentimento classifica texto como predominantemente positivo ou negativo, atribuir uma pontuação no intervalo de 0 a 1. Os valores próximo 0,5 são independente ou indeterminado. Uma pontuação de 0,5 indica neutrality. Quando uma cadeia não pode ser analisada para dados de sentimento ou não tem nenhum sentimento, a classificação é sempre 0,5 exatamente. Por exemplo, se passar de uma cadeia Espanhol com um código de idioma inglês, a classificação é 0,5.

Resultado é devolvido imediatamente. Pode transmitir os resultados para uma aplicação que aceite JSON ou guardar o resultado para um ficheiro no sistema local e, em seguida, importe-o para uma aplicação que permite-lhe ordenar, procurar e manipular os dados.

O exemplo seguinte mostra a resposta para a coleção de documentos neste artigo.

```
{
    "documents": [
        {
            "score": 0.9999237060546875,
            "id": "1"
        },
        {
            "score": 0.0000540316104888916,
            "id": "2"
        },
        {
            "score": 0.99990355968475342,
            "id": "3"
        },
        {
            "score": 0.980544924736023,
            "id": "4"
        },
        {
            "score": 0.99996328353881836,
            "id": "5"
        }
    ],
    "errors": []
}
```

## <a name="summary"></a>Resumo

Neste artigo, aprendeu conceitos e fluxo de trabalho para análise de dados de sentimento através da análise do texto nos serviços cognitivos. Em Resumo:

+ [Análise de dados de sentimento API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c9) está disponível para os idiomas selecionados.
+ Documentos JSON no corpo do pedido incluem um código de id, o texto e o idioma.
+ É pedido POST para um `/sentiment` ponto final, utilizando um personalizado [aceder à chave e um ponto final](text-analytics-how-to-access-key.md) que é válido para a sua subscrição.
+ Saída de resposta, composta por uma classificação de dados de sentimento para cada ID do documento, pode ser transmitida para qualquer aplicação que aceite JSON, incluindo o Excel e o Power BI, o nome algumas.

## <a name="see-also"></a>Consulte também 

 [Descrição geral da análise de texto](../overview.md)  
 [Perguntas mais frequentes (FAQ)](../text-analytics-resource-faq.md)</br>
 [Página de produto de análise de texto](//go.microsoft.com/fwlink/?LinkID=759712) 

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Extrair expressões chaves](text-analytics-how-to-keyword-extraction.md)
