---
title: A chave de gestão - serviços cognitivos Microsoft | Microsoft Docs
titleSuffix: Azure
description: Como gerir as chaves de QnA Maker
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 05/07/2018
ms.author: saneppal
ms.openlocfilehash: b402187f4949dac34fa476648c81b980ba3efc96
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354079"
---
# <a name="key-management"></a>Gestão de Chaves

O serviço de QnA Maker lida com dois tipos de chaves, **chaves de subscrição** e **chaves de ponto final**.

![gestão de chaves](../media/qnamaker-how-to-key-management/key-management.png)

1. **Chaves de subscrição**: estas chaves são utilizadas para aceder a [APIs do serviço de gestão de QnA Maker](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff). Estas APIs permitem-lhe desempenhar várias operações CRUD na sua base de dados de conhecimento.  

2. **Chaves de ponto final**: estas chaves são utilizadas para aceder ao ponto final de base de dados de conhecimento para obter uma resposta para uma pergunta de utilizador. Normalmente, utilizaria este ponto final no seu código de bot/aplicação de chat que consome o serviço de QnA Maker.
 
## <a name="subscription-keys"></a>Chaves de subscrição
Pode ver e repor as chaves de subscrição do portal do Azure onde criou o recurso de QnA Maker. 
1. Ir para o recurso de QnA Maker no portal do Azure.

    ![Lista de recursos de QnA Maker](../media/qnamaker-how-to-key-management/qnamaker-resource-list.png)

2. Aceda a **chaves**.

    ![chave de subscrição](../media/qnamaker-how-to-key-management/subscription-key.PNG)

## <a name="endpoint-keys"></a>Chaves de ponto final

Chaves de ponto final podem ser geridas a partir de [portal de QnA Maker](https://qnamaker.ai).

1. Inicie sessão no [portal de QnA Maker](https://qnamaker.ai)e aceda a **gerir chaves**.

    ![chave de ponto final](../media/qnamaker-how-to-key-management/Endpoint-keys.png)

2. Ver ou repor as suas chaves.

    ![Gestor de chave de ponto final](../media/qnamaker-how-to-key-management/Endpoint-keys1.png)

    >[!NOTE]
    >Atualize as suas chaves se sentir que possam tem sido comprometidas. Esta operação pode requerer alterações correspondentes ao código de aplicação/Bot.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Criar uma base de dados de conhecimento num idioma diferente](./language-knowledge-base.md)
