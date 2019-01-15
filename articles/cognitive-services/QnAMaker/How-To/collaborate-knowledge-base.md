---
title: Colaborar na base de dados de conhecimento - Qna Maker
titleSuffix: Azure Cognitive Services
description: A ferramenta QnA Maker permite que várias pessoas colaborar numa base de dados de conhecimento. Esta funcionalidade é fornecida com o controlo de acesso.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 01/14/2019
ms.author: tulasim
ms.openlocfilehash: de9ba7e9a30c38bf096b7fc0a25a3925d938f604
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/14/2019
ms.locfileid: "54265316"
---
# <a name="collaborate-on-your-knowledge-base"></a>Colaborar na sua base de dados de conhecimento

A ferramenta QnA Maker permite que várias pessoas colaborar numa base de dados de conhecimento. Esta funcionalidade é fornecida com o Azure [controlo de acesso baseado em funções](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure). 

Execute os seguintes passos para partilhar o seu serviço QnA Maker com alguém:

1. Inicie sessão no portal do Azure e aceda ao seu recurso do QnA Maker.

    ![Lista de recursos do QnA Maker](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-resource-list.PNG)

2. Vá para o **controlo de acesso (IAM)** separador.

    ![A ferramenta QnA Maker IAM](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam.PNG)

3. Selecione **Adicionar**.

    ![Adicionar o QnA Maker IAM](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam-add.PNG)

4. Selecione o **proprietário** ou o **contribuinte** função. Não é possível conceder acesso só de leitura por meio do controle de acesso baseado em funções. Função de proprietário e contribuinte tem acesso de leitura / escrita diretamente para o serviço QnA Maker.

    ![QnA Maker IAM Adicionar função](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam-add-role.PNG)

5. Introduza o e-mail que pretende partilhar com e pressiono salvar.

    ![QnA Maker IAM adicionar e-mail](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam-add-email.PNG)

Agora quando a pessoa partilhou o serviço QnA Maker com, inicia sessão na [portal do QnA Maker](https://qnamaker.ai) podem ver todas as bases de dados de conhecimento desse serviço.

Lembre-se de que não é possível partilhar uma base de dados de conhecimento específica num serviço QnA Maker. Se pretender que o controlo de acesso mais granular, considere distribuir as suas bases de dados de conhecimento em todos os diferentes serviços do QnA Maker.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Testar uma base de dados de conhecimento](./test-knowledge-base.md)
