---
title: Confiança pontuação - serviços cognitivos Microsoft | Microsoft Docs
titleSuffix: Azure
description: Explicar pontuação de confiança
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 04/21/2018
ms.author: saneppal
ms.openlocfilehash: c97bdb7e57275ebd1893bc28248c4ecc6b35c153
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354061"
---
# <a name="confidence-score"></a>Pontuação de Confiança

Uma pontuação de confiança indica o grau de correspondência entre a pergunta de utilizador e a resposta devolvida.

Quando uma consulta do utilizador é comparada com o conteúdo da base de dados de conhecimento, podem existir mais do que uma resposta devolvida. As respostas são devolvidas por uma ordem classificada de diminuição pontuação de confiança.

É uma pontuação de confiança entre 0 e 100.

|Valor de pontuação|Confiança|
|--|--|
|100|Uma correspondência exata de consulta do utilizador e uma pergunta KB|
|90|Corresponde a maioria das palavras de confiança elevada-|
|40-60|Correspondem a confiança justa - palavras importantes|
|10|Confiança baixa - palavras importantes não correspondem|
|0|Nenhuma correspondência da palavra|


## <a name="similar-confidence-scores"></a>Semelhante pontuações de confiança
Quando várias respostas têm uma classificação de confiança semelhante, é provável que a consulta foi demasiado genérico e, por conseguinte, correspondentes com probabilidade igual com várias respostas. Tente estrutura sua QnAs melhor para que cada entidade de QnA tenha um objetivo distinto.


## <a name="improving-confidence-scores"></a>Melhorar as pontuações de confiança
Para melhorar a classificação de confiança de uma determinada resposta a uma consulta do utilizador, pode adicionar a consulta do utilizador para a base de dados de conhecimento como uma questão alternativo nessa resposta.
   
## <a name="confidence-score-differences"></a>Diferenças de pontuação de confiança
A classificação de confiança de uma resposta pode alterar negligibly entre o teste e a versão publicada da base de dados de conhecimento, mesmo que o conteúdo é o mesmo. Isto acontece porque o conteúdo de teste e a base de dados de conhecimento publicada estão localizados os índices de pesquisa do Azure diferentes.

Consulte aqui o [publicar](../How-To/publish-knowledge-base.md) funciona a operação.


## <a name="no-match-found"></a>Nenhuma correspondência localizada
Quando não é encontrada nenhuma correspondência boa pelo ranker, é devolvida a classificação de confiança de 0,0 ou "None" e a resposta de predefinido é "Existe correspondência de boa encontrada no KB". Pode substituir esta resposta predefinido no código da aplicação ou bot chamar o ponto final. Em alternativa, também pode definir a resposta de substituição no Azure e muda a predefinição para todas as bases de dados de conhecimento implementados num determinado serviço de QnA Maker.

1. Vá para o [portal do Azure](http://portal.azure.com) e navegue para o grupo de recursos que representa o serviço de QnA Maker que criou.

2. Clique para abrir o **do serviço de aplicações**.

    ![Aceder ao serviço de aplicações](../media/qnamaker-concepts-confidencescore/set-default-response.png)

3. Clique em **definições da aplicação** e editar o **DefaultAnswer** campo para a resposta de predefinição pretendido. Clique em **Guardar**.

    ![Resposta de predefinição de alteração](../media/qnamaker-concepts-confidencescore/change-response.png)

4. Reinicie o serviço de aplicações

    ![Reiniciar o serviço de aplicações Maker de QnA](../media/qnamaker-faq/qnamaker-appservice-restart.png)


## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Origens de dados suportadas](./data-sources-supported.md)

## <a name="see-also"></a>Consulte também 

[Descrição geral de QnA Maker](../Overview/overview.md)
