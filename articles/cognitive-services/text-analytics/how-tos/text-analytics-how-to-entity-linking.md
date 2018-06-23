---
title: Como utilizar a entidade de ligação na API de REST de análise de texto (cognitivos serviços da Microsoft no Azure) | Microsoft Docs
description: Saiba como identificar e resolver entidades utilizando a API de REST de análise de texto no Microsoft serviços cognitivos no Azure neste tutorial de instruções.
services: cognitive-services
author: ashmaka
manager: cgronlun
ms.service: cognitive-services
ms.technology: text-analytics
ms.topic: article
ms.date: 5/02/2018
ms.author: ashmaka
ms.openlocfilehash: 55bec1a0223b70749a97a30e2da92ef15128038c
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352712"
---
# <a name="how-to-identify-linked-entities-in-text-analytics-preview"></a>Como identificar ligadas entidades na análise de texto (pré-visualização)

O [API de associação de entidade](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/5ac4251d5b4ccd1554da7634) utiliza o texto não estruturado e para cada documento JSON, devolve uma lista de entidades resolver esta ambiguidade com ligações para obter mais informações na web (Wikipedia e Bing). 

## <a name="entity-linking-vs-named-entity-recognition"></a>Entidade Linking vs. Reconhecimento de Entidades Nomeadas

No processamento de linguagem natural, os conceitos de entidade de ligação e de reconhecimento de entidade com nome (NER) podem facilmente ser confundidos. Na versão de pré-visualização de análise de texto `entities` ponto final, apenas entidades da ligação é suportada.

Entidade de ligação é a capacidade para identificar e ambiguidade a identidade de uma entidade encontrada no texto (por exemplo, determinar se o "Mars" está a ser utilizado como o planet ou como o god Roman de war). Este processo requer a presença de um conhecimento base que reconhecido entidades estão ligadas - Wikipedia é utilizado como a base de dados de conhecimento para o `entities` ponto final de análise de texto.

### <a name="language-support"></a>Suporte de idiomas

Utilizar a entidade de ligação em vários idiomas requer a utilização de uma base de dados de conhecimento correspondente em cada idioma. Para a entidade de ligação na análise de texto, isto significa que todos os idiomas que é suportado pelo `entities` endpoint vai ligar para o corpus Wikipedia correspondente nesse idioma. Uma vez que o tamanho do corpora varia entre idiomas, é esperado que a entidade da devolução de chamada da funcionalidade de ligação também variam.


## <a name="preparation"></a>Preparação

Tem de ter documentos JSON neste formato: id, o texto, o idioma

Para os idiomas atualmente suportados, consulte [esta lista](../text-analytics-supported-languages.md).

Tamanho do documento tem de ser em 5000 carateres por documento e podem ter até 1000 itens (IDs) por coleção. A coleção é submetida no corpo do pedido. O exemplo seguinte é uma ilustração de conteúdo que pode submeter ao fim de ligação de entidade.

```
{"documents": [{"id": "1",
                "language": "en",
                "text": "I really enjoy the new XBox One S. It has a clean look, it has 4K/HDR resolution and it is affordable."
                },
               {"id": "2",
                "language": "en",
                "text": "The Seattle Seahawks won the Super Bowl in 2014."
                }
               ]
}
```    
    
## <a name="step-1-structure-the-request"></a>Passo 1: Estrutura de pedido

Podem ser encontrados detalhes na definição de pedido no [como chamar a API de análise de texto](text-analytics-how-to-call-api.md). Os seguintes pontos são restated para sua comodidade:

+ Criar um **POST** pedido. Reveja a documentação da API para este pedido: [API de associação de entidade](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/5ac4251d5b4ccd1554da7634)

+ Defina o ponto final de HTTP para extração de expressão de chave. Tem de incluir o `/entities` recursos: `https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/entities`

+ Defina um cabeçalho de pedido para incluir a chave de acesso para operações de análise de texto. Para obter mais informações, consulte [como encontrar pontos finais e chaves de acesso](text-analytics-how-to-access-key.md).

+ No corpo do pedido, forneça a coleção de documentos JSON que preparou para esta análise

> [!Tip]
> Utilize [Postman](text-analytics-how-to-call-api.md) ou abrir o **consola de teste de API** no [documentação](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/5ac4251d5b4ccd1554da7634) a estrutura de um pedido e PUBLICÁ-la para o serviço.

## <a name="step-2-post-the-request"></a>Passo 2: Publicar o pedido

Análise é executada após a receção do pedido. O serviço aceita até 100 pedidos por minuto. Cada pedido pode ter um máximo de 1 MB.

Recuperar-se de que o serviço está sem monitorização de estado. Nenhum dado é armazenado na sua conta. Os resultados são devolvidos imediatamente na resposta.

## <a name="step-3-view-results"></a>Passo 3: Ver resultados

Todos os pedidos POST devolverem um JSON formatado resposta com os IDs e detectadas propriedades.

Resultado é devolvido imediatamente. Pode transmitir os resultados para uma aplicação que aceite JSON ou guardar o resultado para um ficheiro no sistema local e, em seguida, importe-o para uma aplicação que permite-lhe ordenar, procurar e manipular os dados.

Um exemplo de saída para a entidade de ligação é apresentado a seguinte:

```
{
    "documents": [
        {
            "id": "1",
            "entities": [
                {
                    "name": "Xbox One",
                    "matches": [
                        {
                            "text": "XBox One",
                            "offset": 23,
                            "length": 8
                        }
                    ],
                    "wikipediaLanguage": "en",
                    "wikipediaId": "Xbox One",
                    "wikipediaUrl": "https://en.wikipedia.org/wiki/Xbox_One",
                    "bingId": "446bb4df-4999-4243-84c0-74e0f6c60e75"
                },
                {
                    "name": "Ultra-high-definition television",
                    "matches": [
                        {
                            "text": "4K",
                            "offset": 63,
                            "length": 2
                        }
                    ],
                    "wikipediaLanguage": "en",
                    "wikipediaId": "Ultra-high-definition television",
                    "wikipediaUrl": "https://en.wikipedia.org/wiki/Ultra-high-definition_television",
                    "bingId": "7ee02026-b6ec-878b-f4de-f0bc7b0ab8c4"
                }
            ]
        },
        {
            "id": "2",
            "entities": [
                {
                    "name": "2013 Seattle Seahawks season",
                    "matches": [
                        {
                            "text": "Seattle Seahawks",
                            "offset": 4,
                            "length": 16
                        }
                    ],
                    "wikipediaLanguage": "en",
                    "wikipediaId": "2013 Seattle Seahawks season",
                    "wikipediaUrl": "https://en.wikipedia.org/wiki/2013_Seattle_Seahawks_season",
                    "bingId": "eb637865-4722-4eca-be9e-0ac0c376d361"
                }
            ]
        }
    ],
    "errors": []
}
```

Se estiver disponível, a resposta inclui o ID de Wikipedia, o Wikipedia URL e o ID de Bing para cada entidade detetada. Estes podem ser utilizados para melhorar ainda mais a sua aplicação com informações sobre a entidade ligada.


## <a name="summary"></a>Resumo

Neste artigo, aprendeu conceitos e fluxo de trabalho para a entidade de ligação através da análise do texto nos serviços cognitivos. Em Resumo:

+ [API de associação de entidade](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/5ac4251d5b4ccd1554da7634) está disponível para os idiomas selecionados.
+ Documentos JSON no corpo do pedido incluem um código de id, o texto e o idioma.
+ É pedido POST para um `/entities` ponto final, utilizando um personalizado [aceder à chave e um ponto final](text-analytics-how-to-access-key.md) que é válido para a sua subscrição.
+ Resultado da resposta, que consiste em entidades ligadas (incluindo confiança pontuações, desvios e ligações web, para cada documento ID) pode ser utilizado em qualquer aplicação

## <a name="see-also"></a>Consulte também 

 [Descrição geral da análise de texto](../overview.md)  
 [Perguntas mais frequentes (FAQ)](../text-analytics-resource-faq.md)</br>
 [Página de produto de análise de texto](//go.microsoft.com/fwlink/?LinkID=759712) 

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Análise de texto API](//westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6)
