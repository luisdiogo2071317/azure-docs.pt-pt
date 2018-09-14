---
title: Recursos e gestão de chaves - QnA Maker
titleSuffix: Azure Cognitive Services
description: O serviço QnA Maker lida com dois tipos de chaves, chaves de subscrição e chaves de ponto final.
services: cognitive-services
author: nstulasi
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: saneppal
ms.openlocfilehash: 9134819fc4610daadb617e123d861673e8cbfd32
ms.sourcegitcommit: f983187566d165bc8540fdec5650edcc51a6350a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/13/2018
ms.locfileid: "45543448"
---
# <a name="key-management"></a>Gestão de Chaves

O serviço QnA Maker lida com dois tipos de chaves, **chaves de subscrição** e **chaves de ponto final**.

![Gestão de chaves](../media/qnamaker-how-to-key-management/key-management.png)

1. **Chaves de subscrição**: estas chaves são utilizadas para acesso a [APIs de serviço de gestão do QnA Maker](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff). Essas APIs permitem-lhe executar várias operações de CRUD em sua base de dados de conhecimento.  

2. **Chaves de ponto final**: estas chaves são utilizadas para aceder ao ponto final de base de dados de conhecimento para obter uma resposta para uma pergunta do utilizador. Normalmente usaria este ponto final no seu código de aplicação/bot de bate-papo que consome o serviço QnA Maker.
 
## <a name="subscription-keys"></a>Chaves de subscrição
Pode ver e repor as chaves de subscrição do portal do Azure onde criou o recurso do QnA Maker. 
1. Vá para o recurso do QnA Maker no portal do Azure.

    ![Lista de recursos do QnA Maker](../media/qnamaker-how-to-key-management/qnamaker-resource-list.png)

2. Aceda a **chaves**.

    ![Chave de subscrição](../media/qnamaker-how-to-key-management/subscription-key.PNG)

## <a name="endpoint-keys"></a>Chaves de ponto final

Chaves de ponto de extremidade podem ser geridas a partir da [portal do QnA Maker](https://qnamaker.ai).

1. Iniciar sessão para o [portal do QnA Maker](https://qnamaker.ai)e aceda a **gerir chaves**.

    ![Chave de ponto final](../media/qnamaker-how-to-key-management/Endpoint-keys.png)

2. Ver ou repor as suas chaves.

    ![Gestor de chave de ponto final](../media/qnamaker-how-to-key-management/Endpoint-keys1.png)

    >[!NOTE]
    >Atualize as suas chaves se achar que as mesmas foram comprometidas. Isso pode exigir alterações correspondentes ao seu código de aplicação/Bot.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Criar uma base de dados de conhecimento num idioma diferente](./language-knowledge-base.md)
