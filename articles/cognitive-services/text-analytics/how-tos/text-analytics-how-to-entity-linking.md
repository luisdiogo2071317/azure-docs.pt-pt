---
title: Utilizar a entidade de ligação com a API de análise de texto
titleSuffix: Azure Cognitive Services
description: Saiba como identificar e resolver entidades usando a API de REST de análise de texto.
services: cognitive-services
author: ashmaka
manager: cgronlun
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: article
ms.date: 09/12/2018
ms.author: ashmaka
ms.openlocfilehash: ad2168806f9ddd124faf66cdb5a0f51ed13dfadc
ms.sourcegitcommit: 616e63d6258f036a2863acd96b73770e35ff54f8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/14/2018
ms.locfileid: "45604744"
---
# <a name="how-to-identify-linked-entities-in-text-analytics-preview"></a>Como identificar as entidades associadas na análise de texto (pré-visualização)

O [API do Entity Linking](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/5ac4251d5b4ccd1554da7634) usa texto não estruturado e, para cada documento JSON, retorna uma lista de entidades a ambigüidade removidas com ligações para obter mais informações na web (Wikipédia e Bing). 

## <a name="entity-linking-vs-named-entity-recognition"></a>Entity Linking vs. Reconhecimento de Entidades Nomeadas

No processamento de linguagem natural, podem facilmente ser confundidos os conceitos de ligação de entidades e o reconhecimento de entidades (NER). Na versão de pré-visualização de análise de texto `entities` ponto final, do entity linking apenas é suportado.

Entidade de ligação é a capacidade de identificar e desambiguar a identidade de uma entidade encontrada no texto (por exemplo, determinar se o "Mars" está a ser utilizado como o planeta ou como o Deus Roman da war). Este processo requer a presença de um conhecimento base para a qual reconhecido entidades são vinculadas - Wikipedia é utilizada como a base de dados de conhecimento para o `entities` ponto final de análise de texto.

### <a name="language-support"></a>Suporte de idiomas

Através do entity linking em várias linguagens requer a utilização de uma base de dados de conhecimento correspondente em cada linguagem. Para a entidade de ligação na análise de texto, isso significa que todos os idiomas que é suportado pelo `entities` ponto final de ligações para o corpo da Wikipedia correspondente nesse idioma. Uma vez que o tamanho do conjunto de documentos de varia entre as linguagens, espera-se que a entidade de recolhimento da funcionalidade de ligação também podem variar.


## <a name="preparation"></a>Preparação

Tem de ter documentos JSON no seguinte formato: id, o texto, o idioma

Para idiomas atualmente suportados, consulte [esta lista](../text-analytics-supported-languages.md).

Tamanho do documento tem de ter menos de 5000 carateres por documento e pode ter até 1.000 itens (IDs) por coleção. A coleção for submetida no corpo do pedido. O exemplo seguinte é uma ilustração de conteúdo que pode enviar para o final de ligação de entidade.

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
    
## <a name="step-1-structure-the-request"></a>Passo 1: Estruturar o pedido

Obter detalhes sobre a definição de pedido podem ser encontrados na [como chamar a API de análise de texto](text-analytics-how-to-call-api.md). Os pontos seguintes são expressas novamente para sua comodidade:

+ Criar uma **POST** pedido. Reveja a documentação da API para este pedido: [API do Entity Linking](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/5ac4251d5b4ccd1554da7634)

+ Defina o ponto de final HTTP para extração de expressões-chave. Tem de incluir o `/entities` recursos: `https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/entities`

+ Defina um cabeçalho de pedido para incluir a chave de acesso para operações de análise de texto. Para obter mais informações, consulte [como encontrar pontos de extremidade e chaves de acesso](text-analytics-how-to-access-key.md).

+ No corpo do pedido, forneça a coleção de documentos JSON que preparou para esta análise

> [!Tip]
> Uso [Postman](text-analytics-how-to-call-api.md) ou abrir o **consola de teste de API** no [documentação](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/5ac4251d5b4ccd1554da7634) para estruturar uma solicitação e POSTÁ-lo para o serviço.

## <a name="step-2-post-the-request"></a>Passo 2: Publicar o pedido

Análise é executada após a receção do pedido. O serviço aceita até 100 pedidos por minuto. Cada pedido pode ter um máximo de 1 MB.

Lembre-se de que o serviço está sem monitoração de estado. Nenhum dado é armazenado na sua conta. Os resultados são retornados imediatamente na resposta.

## <a name="step-3-view-results"></a>Passo 3: Ver resultados

Todos os pedidos POST devolvem um JSON formatado de resposta com os IDs e detetado propriedades.

Saída é retornada imediatamente. Pode transmitir os resultados para uma aplicação que aceite JSON ou salvar a saída num arquivo no sistema local e, em seguida, importá-lo num aplicativo que permite-lhe ordenar, procure e manipular os dados.

Um exemplo da saída para a entidade de ligação é mostrado a seguir:

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

Se estiver disponível, a resposta inclui o ID da Wikipedia, o URL da Wikipédia e o ID de Bing para cada entidade detetada. Estes podem ser utilizados para melhorar ainda mais a sua aplicação com informações sobre a entidade ligada.


## <a name="summary"></a>Resumo

Neste artigo, aprendeu conceitos e fluxo de trabalho para a ligação de entidades com a análise de texto nos serviços cognitivos. Em Resumo:

+ [API do Entity Linking](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/5ac4251d5b4ccd1554da7634) está disponível para os idiomas selecionados.
+ Documentos JSON no corpo do pedido incluem um código de id, texto e idiomas.
+ É uma solicitação POST para um `/entities` ponto de extremidade, usando um personalizadas [aceder a chave e um ponto de extremidade](text-analytics-how-to-access-key.md) que é válido para a sua subscrição.
+ Saída de resposta, que consiste em entidades associadas (incluindo confiança pontuações, deslocamentos e ligações web, para cada ID de documento) pode ser utilizada em qualquer aplicação

## <a name="see-also"></a>Consulte também 

 [Descrição Geral da Análise de Texto](../overview.md)  
 [Perguntas Mais Frequentes (FAQ)](../text-analytics-resource-faq.md)</br>
 [Página de produto de análise de texto](//go.microsoft.com/fwlink/?LinkID=759712) 

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [API de análise de texto](//westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6)
