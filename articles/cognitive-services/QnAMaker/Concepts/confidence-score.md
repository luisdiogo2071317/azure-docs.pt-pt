---
title: Pontuação de confiança - QnA Maker
titleSuffix: Azure Cognitive Services
description: Uma pontuação de confiança indica o grau de correspondência entre a pergunta de utilizador e a resposta devolvida.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: tulasim
ms.openlocfilehash: 33da5cf5724b8314ce813f12eb077d9a15ec1b2a
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "47041528"
---
# <a name="confidence-score"></a>Pontuação de Confiança

Uma pontuação de confiança indica o grau de correspondência entre a pergunta de utilizador e a resposta devolvida.

Quando uma consulta de utilizador é comparada com conteúdo de base de dados de conhecimento, podem existir mais do que uma resposta devolvida. As respostas são retornadas numa ordem classificada de diminuir a pontuação de confiança.

Uma pontuação de confiança é entre 0 e 100.

|Valor de pontuação|Confiança|
|--|--|
|100|Uma correspondência exata de consulta de utilizador e uma pergunta KB|
|90|A maioria das palavras de confiança elevada - corresponder|
|40-60|Justo confiança - importante de correspondem de palavras|
|10|Confiança baixa - palavras importantes não correspondem|
|0|Nenhuma correspondência do word|


## <a name="similar-confidence-scores"></a>Pontuações de confiança semelhantes
Quando várias respostas tem uma pontuação de confiança semelhante, é provável que a consulta era demasiado genérico e, portanto, correspondentes com probabilidade igual com várias respostas. Tente estruturar seu QnAs melhor para que cada entidade do QnA tem uma intenção distinta.


## <a name="improving-confidence-scores"></a>Melhorar as pontuações de confiança
Para melhorar a pontuação de confiança de uma determinada resposta a uma consulta de utilizador, pode adicionar a consulta de utilizador para a base de dados de conhecimento como uma alternativa pergunta em que a resposta.
   
## <a name="confidence-score-differences"></a>Diferenças de pontuação de confiança
A pontuação de confiança de uma resposta pode ser alteradas negligibly o teste e a versão publicada da base de dados de conhecimento, mesmo que o conteúdo é o mesmo. Isto acontece porque o conteúdo de teste e a base de dados de conhecimento publicada estão localizados em diferentes índices da Azure Search.

Veja aqui como a [publicar](../How-To/publish-knowledge-base.md) funciona a operação.


## <a name="no-match-found"></a>Nenhuma correspondência localizada
Quando for encontrada nenhuma correspondência de boa pelo classificador, é devolvida a classificação de confiança de 0,0 ou "None" e a resposta padrão não é "Nenhuma boa correspondência encontrada no artigo do BDC". Pode substituir esta resposta predefinida no código de aplicação ou bot chamar o ponto final. Como alternativa, também pode definir a resposta de substituição no Azure, e isso muda a predefinição para todas as bases de dados de conhecimento implementado num determinado serviço QnA Maker.

1. Vá para o [portal do Azure](http://portal.azure.com) e navegue para o grupo de recursos que representa o serviço QnA Maker que criou.

2. Clique para abrir o **serviço de aplicações**.

    ![Serviço de aplicações de acesso](../media/qnamaker-concepts-confidencescore/set-default-response.png)

3. Clique em **as configurações do aplicativo** e editar a **DefaultAnswer** campo para a resposta padrão pretendido. Clique em **Guardar**.

    ![Alterar a resposta predefinida](../media/qnamaker-concepts-confidencescore/change-response.png)

4. Reinicie o serviço de aplicações

    ![Reiniciar o serviço de aplicações do QnA Maker](../media/qnamaker-faq/qnamaker-appservice-restart.png)


## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Origens de dados suportadas](./data-sources-supported.md)

## <a name="see-also"></a>Consulte também 

[Descrição geral do Criador de FAQ](../Overview/overview.md)
