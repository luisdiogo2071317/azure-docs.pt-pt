---
title: Atualize o serviço de QnA Maker - serviços cognitivos Microsoft | Microsoft Docs
titleSuffix: Azure
description: Como atualizar o seu serviço de QnA Maker
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 05/07/2018
ms.author: saneppal
ms.openlocfilehash: e8d3713d6729c4e30da9a64a382e9d5a647dfefd
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354044"
---
# <a name="upgrade-your-qna-maker-service"></a>Atualize o serviço de QnA Maker
Pode optar por atualizar componentes individuais da pilha de QnA Maker após a criação inicial. Consulte os detalhes dos componentes dependentes e seleção de SKU [aqui](https://aka.ms/qnamaker-docs-capacity).

## <a name="upgrade-qna-maker-management-sku"></a>Atualizar a gestão de QnA Maker SKU
Para atualizar a gestão de QnA Maker SKU:
1. Aceda ao seu recurso de QnA Maker no portal do Azure e selecione **escalão de preço**.

    ![Recurso de QnA Maker](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-resource.png)

2. Escolha o SKU e prima adequado **selecione**.

    ![Preços de QnA Maker](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-pricing-page.png)

## <a name="upgrade-app-service"></a>Atualização do serviço de aplicações
Pode [aumentar verticalmente](https://docs.microsoft.com/azure/app-service/web-sites-scale) ou reduzir verticalmente o serviço de aplicações.

1. Ir para o recurso de serviço de aplicação no portal do Azure e selecione **aumentar verticalmente** ou **reduzir verticalmente** opções conforme necessário.

    ![Escala de serviço de aplicação de QnA Maker](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-scale.png)

## <a name="upgrade-azure-search-service"></a>Atualizar o serviço da Azure Search
Atualmente não é possível efetuar um local no SKU de pesquisa de atualização do Azure. No entanto, pode criar um novo recurso de pesquisa do Azure com o SKU pretendido, restaurar os dados para o novo recurso e, em seguida, ligue-o para a pilha de QnA Maker.

1. Criar um novo recurso de pesquisa do Azure no portal do Azure e escolha o SKU pretendido.

    ![Recurso de pesquisa de QnA Maker Azure](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-azuresearch-new.png)

2. Restaure os índices do seu recurso original de pesquisa do Azure para a nova. Consulte o código de exemplo de restauro de cópia de segurança [aqui](https://github.com/pchoudhari/QnAMakerBackupRestore).

3. Depois dos dados são restaurados, aceda ao seu recurso de pesquisa do Azure nova, selecione **chaves**e tome nota do **nome** e o **chave Admin**.

    ![Chaves de pesquisa de QnA Maker Azure](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-azuresearch-keys.png)

4. Para ligar o novo recurso de pesquisa do Azure para a pilha de QnA Maker, vá para o serviço de QnA Maker aplicação.

    ![Serviço de aplicações de QnA Maker](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-resource-list-appservice.png)

5. Selecione **definições da aplicação** e substitua o **AzureSearchName** e **AzureSearchAdminKey** campos do passo 3.

    ![Definição de serviço aplicacional Maker de QnA](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-settings.png)

6. Reinicie o serviço de aplicações.

    ![Reiniciar o serviço de aplicações Maker de QnA](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-restart.png)

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Utilizar Maker de QnA API](../Quickstarts/csharp.md)
