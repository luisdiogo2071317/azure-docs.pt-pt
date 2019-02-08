---
title: Metadados com GenerateAnswer API - QnA Maker
titleSuffix: Azure Cognitive Services
description: A ferramenta QnA Maker permite-lhe adicionar metadados, na forma de pares chave/valor, a seus conjuntos de pergunta/resposta. Estas informações podem ser utilizadas para filtrar os resultados de consultas do utilizador e para armazenar informações adicionais que podem ser utilizadas no seguimento conversas.
services: cognitive-services
author: tulasim88
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 12/18/2018
ms.author: tulasim88
ms.openlocfilehash: 1294b714c217178d53ed69cc886cd89f23620274
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/07/2019
ms.locfileid: "55859492"
---
# <a name="using-metadata-and-the-generateanswer-api"></a>Usando a API de GenerateAnswer e metadados

A ferramenta QnA Maker permite-lhe adicionar metadados, na forma de pares de chave e valor, a seus conjuntos de pergunta/resposta. Estas informações podem ser utilizadas para filtrar os resultados de consultas do utilizador e para armazenar informações adicionais que podem ser utilizadas no seguimento conversas. Para obter mais informações, consulte [base de dados de conhecimento](../Concepts/knowledge-base.md).

## <a name="qna-entity"></a>Entidade de QnA

Em primeiro lugar é importante compreender como o QnA Maker armazena os dados de pergunta/resposta. A ilustração seguinte mostra uma entidade de QnA:

![Entidade de QnA](../media/qnamaker-how-to-metadata-usage/qna-entity.png)

Cada entidade do QnA tem um ID exclusivo e persistente. O ID pode ser utilizado para disponibilizar as atualizações para uma entidade específica do QnA.

## <a name="generateanswer-api"></a>GenerateAnswer API

Utilize a API de GenerateAnswer na sua aplicação ou Bot para consultar a base de dados de conhecimento com uma pergunta de utilizador para obter a melhor correspondência dos conjuntos de pergunta/resposta.

### <a name="generateanswer-endpoint"></a>Ponto final de GenerateAnswer

Depois de publicar a sua base de dados de conhecimento, a partir do [portal do QnA Maker](https://www.qnamaker.ai), ou utilizando o [API](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff), pode obter os detalhes do ponto final do GenerateAnswer.

Para obter os detalhes do seu ponto final:
1. Inicie sessão no [ https://www.qnamaker.ai ](https://www.qnamaker.ai).
2. Na **meu bases de dados de conhecimento**, clique em **View Code** para sua base de dados de conhecimento.
![meu bases de dados de conhecimento](../media/qnamaker-how-to-metadata-usage/my-knowledge-bases.png)
3. Obtenha os detalhes do seu ponto final de GenerateAnswer.

![Detalhes do ponto final](../media/qnamaker-how-to-metadata-usage/view-code.png)

Também pode obter os detalhes do seu ponto final da **definições** separador da sua base de dados de conhecimento.

### <a name="generateanswer-request"></a>Pedido de GenerateAnswer

Chamar GenerateAnswer com um pedido HTTP POST. Para o código de exemplo que mostra como chamar GenerateAnswer, consulte a [inícios Rápidos](../quickstarts/csharp.md).

- **URL do pedido**: ponto final do criador de https://{QnA} /knowledgebases/ {ID da base de dados de conhecimento} / generateAnswer

- **Parâmetros do pedido**: 
    - **ID da base de dados de conhecimento** (cadeia): O GUID de sua base de dados de conhecimento.
    - **Ponto final do QnAMaker** (cadeia): O nome de anfitrião do ponto final implementado na sua subscrição do Azure.
- **Cabeçalhos de pedido**
    - **Content-Type** (string): O tipo de suporte do corpo enviado para a API.
    - **Autorização** (cadeia): A chave de ponto final (EndpointKey xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx).
- **Corpo do pedido**
    - **pergunta** (cadeia): Uma pergunta de utilizador para ser consultados em relação a sua base de dados de conhecimento.
    - **parte superior** (opcional, número inteiro): O número de resultados classificados a incluir na saída. O valor predefinido é 1.
    - **userId** (optional, string): Um ID exclusivo para identificar o utilizador. Este ID será registado nos logs de bate-papo.
    - **strictFilters** (optional, string): Se for especificado, informa ao QnA Maker para devolver apenas as respostas que tenham os metadados especificados. Para obter mais informações, veja a seguir.
    ```json
    {
        "question": "qna maker and luis",
        "top": 6,
        "strictFilters": [
        {
            "name": "category",
            "value": "api"
        }],
        "userId": "sd53lsY="
    }
    ```

### <a name="generateanswer-response"></a>Resposta de GenerateAnswer

- **A resposta 200** -uma chamada bem-sucedida devolve o resultado da pergunta. A resposta contém os seguintes campos:
    - **respostas** -uma lista de respostas para a consulta de utilizador, ordenados por ordem decrescente de classificação de pontuação.
        - **pontuação**: Uma pontuação de classificação, entre 0 e 100.
        - **perguntas**: As perguntas fornecidas pelo usuário.
        - **answer**: A resposta à pergunta.
        - **origem**: O nome da origem do qual a resposta foi extraída ou guardada na base de dados de conhecimento.
        - **metadata**: Os metadados associados com a resposta.
            - name: Nome de metadados. (string, comprimento máximo: 100, necessária)
            - Valor: Valor de metadados. (string, comprimento máximo: 100, necessária)
        - **Id**: Um ID exclusivo atribuído para a resposta.
    ```json
    {
        "answers": [
            {
                "score": 28.54820341616869,
                "Id": 20,
                "answer": "There is no direct integration of LUIS with QnA Maker. But, in your bot code, you can use LUIS and QnA Maker together. [View a sample bot](https://github.com/Microsoft/BotBuilder-CognitiveServices/tree/master/Node/samples/QnAMaker/QnAWithLUIS)",
                "source": "Custom Editorial",
                "questions": [
                    "How can I integrate LUIS with QnA Maker?"
                ],
                "metadata": [
                    {
                        "name": "category",
                        "value": "api"
                    }
                ]
            }
        ]
    }
    ```

## <a name="metadata-example"></a>Exemplo de metadados

Considere o abaixo dados de FAQ. Adicione metadados a sua base de dados de conhecimento ao clicar no ícone de metadados.

![adicionar metadados](../media/qnamaker-how-to-metadata-usage/add-metadata.png)

### <a name="filter-results-with-strictfilters-for-metadata-tags"></a>Filtrar os resultados com strictFilters para marcas de metadados

Considere a pergunta do utilizador "Quando o feche esta hotel?" em que a intenção é implícito para o restaurante "Paraíso."

Uma vez que os resultados são necessários apenas para o restaurante "Paraíso", pode definir um filtro na chamada GenerateAnswer nos metadados "Restaurante Name", da seguinte forma.

```json
{
    "question": "When does this hotel close?",
    "top": 1,
    "strictFilters": [
      {
        "name": "restaurant",
        "value": "paradise"
      }]
}
```

### <a name="keep-context"></a>Manter o contexto
A resposta para o GenerateAnswer contém as informações de metadados correspondente do conjunto correspondente de pergunta/resposta, da seguinte forma.

```json
{
    "answers": [
        {
            "questions": [
                "What is the closing time?"
            ],
            "answer": "10.30 PM",
            "score": 100,
            "id": 1,
            "source": "Editorial",
            "metadata": [
                {
                    "name": "restaurant",
                    "value": "paradise"
                },
                {
                    "name": "location",
                    "value": "secunderabad"
                }
            ]
        }
    ]
}
```

Estas informações podem ser utilizadas para registar o contexto da conversa anterior para utilização posterior conversas. 

## <a name="next-steps"></a>Passos Seguintes

Página de publicação também fornece informações para gerar uma resposta com [Postman](../Quickstarts/get-answer-from-kb-using-postman.md) e [cURL](../Quickstarts/get-answer-from-kb-using-curl.md). 

> [!div class="nextstepaction"]
> [Criar uma base de dados de conhecimento](./create-knowledge-base.md)
