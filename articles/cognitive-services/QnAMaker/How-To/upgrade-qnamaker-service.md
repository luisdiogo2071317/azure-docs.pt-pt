---
title: Atualizar o serviço QnA Maker - QnA Maker
titleSuffix: Azure Cognitive Services
description: Pode optar por atualizar componentes individuais da pilha do QnA Maker após a criação inicial.
services: cognitive-services
author: nstulasi
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: saneppal
ms.openlocfilehash: 6d8940389e72ee3924c036e8d4833a20464d628e
ms.sourcegitcommit: f983187566d165bc8540fdec5650edcc51a6350a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/13/2018
ms.locfileid: "45543280"
---
# <a name="upgrade-your-qna-maker-service"></a>Atualizar o serviço Criador de FAQ
Pode optar por atualizar componentes individuais da pilha do QnA Maker após a criação inicial. Ver os detalhes dos componentes dependentes e seleção de SKU [aqui](https://aka.ms/qnamaker-docs-capacity).

## <a name="upgrade-qna-maker-management-sku"></a>Atualizar a ferramenta QnA Maker gestão SKU
Para atualizar o SKU de gerenciamento do QnA Maker:
1. Aceda ao seu recurso do QnA Maker no portal do Azure e selecione **escalão de preço**.

    ![Recursos do QnA Maker](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-resource.png)

2. Selecione o SKU de apropriados e prima **selecione**.

    ![Preços do QnA Maker](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-pricing-page.png)

## <a name="upgrade-app-service"></a>Atualizar serviço de aplicações
Pode [aumentar verticalmente](https://docs.microsoft.com/azure/app-service/web-sites-scale) ou reduzir verticalmente o serviço de aplicações.

1. Vá para o recurso de serviço de aplicações no portal do Azure e selecione **aumentar verticalmente** ou **reduzir verticalmente** opções conforme necessário.

    ![Dimensionamento do serviço de aplicações de QnA Maker](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-scale.png)

## <a name="upgrade-azure-search-service"></a>Atualizar o serviço Azure Search
Atualmente não é possível executar um ponto de partida no SKU de pesquisa de atualização do Azure. No entanto, pode criar um novo recurso de pesquisa do Azure com o SKU desejado, restaurar os dados para o novo recurso e, em seguida, ligá-lo para a pilha do QnA Maker.

1. Criar um novo recurso de pesquisa do Azure no portal do Azure e selecione o SKU pretendido.

    ![Recurso de pesquisa do Azure do QnA Maker](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-azuresearch-new.png)

2. Restaure os índices do seu recurso de pesquisa do Azure original para a nova. Consulte o código de exemplo de restauro de cópia de segurança [aqui](https://github.com/pchoudhari/QnAMakerBackupRestore).

3. Assim que os dados são restaurados, aceda ao seu recurso de pesquisa do Azure nova, selecione **chaves**e tome nota da **nome** e o **chave Admin**.

    ![Chaves de pesquisa do Azure do QnA Maker](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-azuresearch-keys.png)

4. Para ligar o novo recurso de pesquisa do Azure para a pilha do QnA Maker, vá para o serviço de aplicações do QnA Maker.

    ![Serviço de aplicações do QnA Maker](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-resource-list-appservice.png)

5. Selecione **as configurações do aplicativo** e substitua o **AzureSearchName** e **AzureSearchAdminKey** campos do passo 3.

    ![Definição de serviço de aplicações do QnA Maker](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-settings.png)

6. Reinicie o serviço de aplicações.

    ![Reiniciar o serviço de aplicações do QnA Maker](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-restart.png)

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Utilizar a API do QnA Maker](../Quickstarts/csharp.md)
