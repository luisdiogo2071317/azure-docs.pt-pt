---
title: Colaborar na base de dados de conhecimento - Qna Maker
titleSuffix: Azure Cognitive Services
description: A ferramenta QnA Maker permite que várias pessoas colaborar numa base de dados de conhecimento. Esta funcionalidade é fornecida com o controlo de acesso.
services: cognitive-services
author: nstulasi
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: saneppal
ms.openlocfilehash: 52cac0d73dd75f400c9777b9cc9c0b3ca101df58
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/14/2018
ms.locfileid: "45579807"
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

4. Selecione o **proprietário** ou o **contribuinte** função.

    ![QnA Maker IAM Adicionar função](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam-add-role.PNG)

5. Introduza o e-mail que pretende partilhar com e pressiono salvar.

    ![QnA Maker IAM adicionar e-mail](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam-add-email.PNG)

Agora quando a pessoa partilhou o serviço QnA Maker com, inicia sessão na [portal do QnA Maker](https://qnamaker.ai) podem ver todas as bases de dados de conhecimento desse serviço.

Lembre-se de que não é possível partilhar uma base de dados de conhecimento específica num serviço QnA Maker. Se pretender que o controlo de acesso mais granular, considere distribuir as suas bases de dados de conhecimento em todos os diferentes serviços do QnA Maker.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Testar uma base de dados de conhecimento](./test-knowledge-base.md)
