---
title: Utilizar metadados na sua base de dados de conhecimento, juntamente com a API de GenerateAnswer | Microsoft Docs
description: Através de metadados de API GenerateAnswer
services: cognitive-services
author: pchoudhari
manager: rsrikan
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 05/18/2018
ms.author: pchoudh
ms.openlocfilehash: 94e3632884d7033971ff1c45b455afb9a09ee798
ms.sourcegitcommit: c722760331294bc8532f8ddc01ed5aa8b9778dec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "35356040"
---
# <a name="using-metadata-and-the-generateanswer-api"></a>Utilizar metadados e a API de GenerateAnswer

Maker de QnA permite-lhe adicionar metadados, sob a forma de pares chave/valor, para os conjuntos de pergunta/resposta. Estas informações podem ser utilizadas de várias formas como filtrar resultados para consultas de utilizador, os aumentos determinados resultados e armazenando informações adicionais que podem ser utilizadas no seguimento conversações. Para obter mais informações, consulte [base de dados de conhecimento](../Concepts/knowledge-base.md).

## <a name="qna-entity"></a>Entidade de QnA

Pela primeira vez é importante compreender a forma como Maker de QnA armazena os dados de pergunta/resposta. A ilustração seguinte mostra uma entidade de QnA:

![Entidade de QnA](../media/qnamaker-how-to-metadata-usage/qna-entity.png)

Cada entidade de QnA tem um ID exclusivo e persistente. O ID pode ser utilizado para efetuar atualizações para uma entidade de QnA específica.

## <a name="generateanswer-api"></a>GenerateAnswer API

Utilize a API de GenerateAnswer no seu Bot ou aplicação para consultar a base de dados de conhecimento com uma pergunta de utilizador para obter a melhor correspondência dos conjuntos de pergunta/resposta.

### <a name="generateanswer-endpoint"></a>Ponto final de GenerateAnswer

Depois de publicar a base de dados de conhecimento, a partir do [portal de QnA Maker](https://www.qnamaker.ai), ou utilizando o [API](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff), pode obter os detalhes do seu ponto final GenerateAnswer.

Para obter os detalhes do ponto final:
1. Inicie sessão no [ https://www.qnamaker.ai ](https://www.qnamaker.ai).
2. No **meu bases de dados de conhecimento**, clique em **Vista código** para a base de dados de conhecimento.
![a minha bases de dados de conhecimento](../media/qnamaker-how-to-metadata-usage/my-knowledge-bases.png)
3. Obter os detalhes do ponto final GenerateAnswer.

![Detalhes do ponto final](../media/qnamaker-how-to-metadata-usage/view-code.png)

Também pode obter os detalhes do ponto final do **definições** separador da sua base de dados de conhecimento.

### <a name="generateanswer-request"></a>Pedido de GenerateAnswer

Chamar GenerateAnswer com um pedido POST de HTTP. Para o código de exemplo que mostra como chamar GenerateAnswer, consulte o [inícios Rápidos](../quickstarts/csharp.md).

- **O URL de pedido**: ponto final de Maker https://{QnA} /knowledgebases/ {ID de base de dados de conhecimento} / generateAnswer

- **Os parâmetros do pedido**: 
    - **ID de base de dados de conhecimento** (cadeia): O GUID para a base de dados de conhecimento.
    - **Ponto final de QnAMaker** (cadeia): O nome de anfitrião do ponto final implementado na sua subscrição do Azure.
- **Cabeçalhos de pedido**
    - **Tipo de conteúdo** (cadeia): O tipo de suporte do corpo enviado para a API.
    - **Autorização** (cadeia): A chave de ponto final.
- **Corpo do pedido**
    - **pergunta** (cadeia): uma pergunta de utilizador a ser consultados em relação a sua base de dados de conhecimento.
    - **parte superior** (opcional, número inteiro): O número de resultados classificados para incluir na saída. O valor predefinido é 1.
    - **userId** (opcional, cadeia): um ID exclusivo para identificar o utilizador. Este ID será registada nos registos de chat.
    - **strictFilters** (opcional, cadeia): se for especificado, indica Maker de QnA para devolver apenas as respostas que tenham os metadados especificados. Para obter mais informações, veja a seguir.
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

- **Resposta 200** -uma chamada com êxito devolve o resultado da pergunta. A resposta contém os seguintes campos:
    - **respostas** -uma lista de respostas para a consulta do utilizador, ordenados diminuir a ordem de classificação de pontuação.
        - **pontuação**: uma pontuação de classificação entre 0 e 100.
        - **perguntas**: as perguntas fornecidas pelo utilizador.
        - **origem**: O nome da origem de que a resposta foi extraída ou guardada na base de dados de conhecimento.
        - **Metadados**: os metadados associados a resposta.
            - nome: nome de metadados. (cadeia, comprimento máximo: 100, necessário)
            - valor: valor de metadados. (cadeia, comprimento máximo: 100, necessário)
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

Considere o abaixo dados FAQ para restaurants no Hyderabad. Adicione metadados para a base de dados de conhecimento clicando no ícone de equipamento.

![adicionar metadados](../media/qnamaker-how-to-metadata-usage/add-metadata.png)

### <a name="filter-results-with-strictfilters"></a>Resultados do filtro com strictFilters

Considere a pergunta de utilizador "Funciona quando fechar este átrios?" onde a intenção é implícito para o restaurante "Paradise."

Uma vez que os resultados são necessários apenas para o restaurante "Paradise", pode definir um filtro na chamada de GenerateAnswer nos metadados "restaurante" nome, da seguinte forma.

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

### <a name="keep-context"></a>Manter contexto
A resposta para o GenerateAnswer contém as informações de metadados correspondente do conjunto de pergunta/resposta correspondente, da seguinte forma.

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

Estas informações podem ser utilizadas para registar o contexto da conversação anterior para utilização em conversações posteriores. 

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Criar uma base de dados de conhecimento](./create-knowledge-base.md)
