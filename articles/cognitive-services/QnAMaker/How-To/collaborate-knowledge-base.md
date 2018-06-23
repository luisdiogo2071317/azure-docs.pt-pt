---
title: Colaborar na sua base de dados de conhecimento - serviços cognitivos Microsoft | Microsoft Docs
titleSuffix: Azure
description: Como podem colaborar na sua base de dados de conhecimento de QnA Maker
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 05/07/2018
ms.author: saneppal
ms.openlocfilehash: e18d656236276595fc5186a6656349bf28974ead
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353953"
---
# <a name="collaborate-on-your-knowledge-base"></a>Colaborar na sua base de dados de conhecimento

Maker de QnA permite que várias pessoas para colaborar numa base de dados de conhecimento. Esta funcionalidade é fornecida com o Azure [controlo de acesso baseado em funções](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-control-configure). 

Execute os seguintes passos para partilhar o seu serviço de QnA Maker com alguém:

1. Inicie sessão no portal do Azure e aceda ao seu recurso de QnA Maker.

    ![Lista de recursos de QnA Maker](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-resource-list.PNG)

2. Vá para o **controlo de acesso (IAM)** separador.

    ![Maker de QnA IAM](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam.PNG)

3. Selecione **Adicionar**.

    ![Adicionar de QnA Maker IAM](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam-add.PNG)

4. Selecione o **proprietário** ou **contribuinte** função.

    ![IAM Maker de QnA Adicionar função](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam-add-role.PNG)

5. Introduza o e-mail que pretende partilhar e prima guardar.

    ![Adicionar de QnA Maker IAM e-mail](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam-add-email.PNG)

Agora quando a pessoa que partilhou o serviço de QnA Maker com, iniciar sessão a [portal de QnA Maker](https://qnamaker.ai) podem ver todas as bases de dados de conhecimento que o serviço.

Lembre-se de que não é possível partilhar uma base de dados de conhecimento específico num serviço Maker de QnA. Se pretender que o controlo de acesso mais granular, considere distribui as bases de dados de conhecimento em diferentes serviços de QnA Maker.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Testar uma base de dados de conhecimento](./test-knowledge-base.md)
