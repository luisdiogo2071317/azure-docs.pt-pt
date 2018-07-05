---
title: Utilizar metadados na sua base de dados de conhecimento, juntamente com a API de GenerateAnswer | Documentos da Microsoft
description: Utilizando os metadados de API de GenerateAnswer
services: cognitive-services
author: pchoudhari
manager: rsrikan
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 05/18/2018
ms.author: pchoudh
ms.openlocfilehash: 8ad51edde1fc90329e908f8150aadcbf4f401126
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37448225"
---
# <a name="using-metadata-and-the-generateanswer-api"></a>Usando a API de GenerateAnswer e metadados

A ferramenta QnA Maker permite-lhe adicionar metadados, na forma de pares chave/valor, a seus conjuntos de pergunta/resposta. Estas informações podem ser utilizadas de várias formas, como a filtragem de resultados de consultas do utilizador, adaptativo determinados resultados e armazenando informações adicionais que podem ser utilizadas em conversas de acompanhamento. Para obter mais informações, consulte [base de dados de conhecimento](../Concepts/knowledge-base.md).

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
    - **ID da base de dados de conhecimento** (cadeia): O GUID para a sua base de dados de conhecimento.
    - **Ponto final do QnAMaker** (cadeia): O nome de anfitrião do ponto final implementado na sua subscrição do Azure.
- **Cabeçalhos de pedido**
    - **Tipo de conteúdo** (cadeia): O tipo de suporte do corpo enviado para a API.
    - **Autorização** (cadeia): A chave de ponto final (EndpointKey xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx).
- **Corpo do pedido**
    - **pergunta** (cadeia): uma pergunta de utilizador para ser consultados em relação a sua base de dados de conhecimento.
    - **parte superior** (opcional, número inteiro): O número de resultados classificados a incluir na saída. O valor predefinido é 1.
    - **ID de utilizador** (opcional, cadeia de caracteres): um ID exclusivo para identificar o utilizador. Este ID será registado nos logs de bate-papo.
    - **strictFilters** (opcional, cadeia de caracteres): se for especificado, informa ao QnA Maker para devolver apenas as respostas que tenham os metadados especificados. Para obter mais informações, veja a seguir.
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
        - **pontuação**: uma pontuação de classificação de entre 0 e 100.
        - **perguntas**: as perguntas fornecidas pelo usuário.
        - **origem**: O nome da origem do qual a resposta foi extraída ou guardada na base de dados de conhecimento.
        - **Metadados**: os metadados associados a resposta.
            - nome: nome de metadados. (string, comprimento máximo: 100, necessária)
            - valor: valor de metadados. (string, comprimento máximo: 100, necessária)
        - **ID**: um ID exclusivo atribuído para a resposta.
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

Considere o abaixo dados de FAQ de restaurantes em Hyderabad. Adicione metadados a sua base de dados de conhecimento ao clicar no ícone de engrenagem.

![adicionar metadados](../media/qnamaker-how-to-metadata-usage/add-metadata.png)

### <a name="filter-results-with-strictfilters"></a>Filtrar os resultados com strictFilters

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

> [!div class="nextstepaction"]
> [Criar uma base de dados de conhecimento](./create-knowledge-base.md)
