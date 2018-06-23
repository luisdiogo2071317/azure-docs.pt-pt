---
title: Glossário - serviços cognitivos Microsoft | Microsoft Docs
titleSuffix: Azure
description: Glossário
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 04/21/2018
ms.author: saneppal
ms.openlocfilehash: e28cddec005cb6ba99b9f60d8b03a11f1bc97062
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354109"
---
# <a name="glossary"></a>Glossário

## <a name="qna-maker-service"></a>Serviço de QnA Maker
Um serviço de QnA Maker é um pré-requisito para começar a utilizar Maker de QnA. A compra de uma camada de QnA Maker configura a recursos na sua subscrição do Azure para criar e gerir a sua base de dados de conhecimento. Cada conta de utilizador de QnA Maker pode criar vários serviços de QnA Maker na sua subscrição do Azure.

## <a name="knowledge-base"></a>Base de dados de conhecimento
Uma base de dados de conhecimento é o repositório de perguntas e respostas criadas, mantidas e utilizadas através de QnA Maker. Cada escalão de QnA Maker pode ser utilizado para várias bases de dados de conhecimento.

## <a name="endpoint"></a>Ponto Final
Um ponto final de HTTP baseado em REST o conteúdo da base de dados de conhecimento que pode ser integrado na sua aplicação, normalmente, um bot chat de manutenção. 

## <a name="test-knowledge-base"></a>Base de dados de conhecimento de teste
Uma base de dados de conhecimento tem dois Estados - teste e publicado. A base de dados de conhecimento de teste é a versão que está a ser editado, guardada e testada para precisão e amplitude de respostas. As alterações efetuadas para a base de dados de conhecimento de teste não afetam o utilizador final do seu bot aplicação/chat.

## <a name="published-knowledge-base"></a>Base de dados de conhecimento publicados
Uma base de dados de conhecimento tem dois Estados - teste e a publicada.  A base de dados de conhecimento publicada é a versão que é utilizada na sua aplicação/bot de chat. A ação de publicação de uma base de dados de conhecimento coloca o conteúdo da base de dados de conhecimento de teste a versão publicada da base de dados de conhecimento. Uma vez que a base de dados de conhecimento publicada a versão que utiliza a aplicação através do ponto final, deve ter cuidado para garantir que o conteúdo está correto e bem testada.

## <a name="query"></a>Consulta
Uma consulta do utilizador é à pergunta sobre que o utilizador final ou tester pede-lhe a base de dados de conhecimento. A consulta é frequentemente um formato de linguagem natural ou alguns palavras-chave que representam a pergunta.

## <a name="response"></a>Resposta
A resposta é a resposta obtida a partir da base de dados de conhecimento, com base na correspondência melhor para uma consulta de determinado utilizador.

## <a name="confidence-score"></a>Pontuação de Confiança
A classificação de confiança da resposta é um valor numérico entre 0 e 100, 100 a ser uma correspondência exata de consulta entre consulta do utilizador e uma pergunta na base de dados de conhecimento, que serviu de resposta é a resposta correta, adequada para uma consulta de determinado utilizador. Respostas normalmente estão ordenadas pela classificação de confiança e uma com a classificação superior de confiança é servida como a resposta de predefinição.
