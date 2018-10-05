---
title: Contentores | Documentos da Microsoft
description: Passos para a publicação de uma imagem de contentor.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pbutlerm
ms.openlocfilehash: 5653e4b2ec9aa5e21a107611b9d4142168630d4a
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/05/2018
ms.locfileid: "48810600"
---
<a name="publishing-a-container-image-in-the-cloud-partner-portal"></a>Publicação de uma imagem de contentor no Portal de parceiros de nuvem
========================================================

Este artigo descreve como publicar uma nova imagem de contentor no Portal de parceiros de nuvem.

Utilize os seguintes passos para publicar uma nova imagem de contentor.

1. Selecione **nova oferta** e, em seguida, selecione **contentores**

    ![Selecione a opção de contentores para a nova oferta](media/cpp-containers-guide/azure-container-offer.png)

2. No separador de definições da oferta, sob a identidade de oferecer, introduza o **ID da oferta**, **ID de publicador**, e **nome**.

    ![Identidade de oferta](media/cpp-containers-guide/containers-offer-settings.png)

3. No separador de SKUs, selecione **novo SKU**.
    >[!NOTE]
    >Pode adicionar mais do que um SKU.

    ![Nova linha de SKU](media/cpp-containers-guide/containers-sku-settings.png)

4. Fornece informações de SKU e o contentor. Cada SKU corresponde a uma imagem de contentor. Existem duas partes para um SKU:

    -   Metadados SKU
    -   Metadados do contentor

    Os metadados SKU contém as informações de exibição para as imagens de contentor.

    ![Metadados SKU](media/cpp-containers-guide/containers-sku-details.png)

    Os metadados do contentor tem informações de referência dos detalhes do seu repositório de imagem dentro do Azure container registry (ACR). O Azure Marketplace, em seguida, copia esta imagem para o registo de marketplace público e é disponibilizado para clientes após a certificação. Todos os pedidos do utilizador do Azure para consumir uma imagem de contentor são servidos a partir do registo de contentor do Marketplace.

   ![Metadados do contentor](media/cpp-containers-guide/containers-image-repository.png)

    Os detalhes do repositório de imagem na captura de ecrã anterior contém os seguintes campos:

    -   **ID de subscrição** -id de subscrição do Azure onde o registo do ACR está presente.
    -   **Nome do grupo de recursos** -o nome do grupo de recursos do registo ACR.
    -   **Nome do registo** -nome do registo do ACR.
    -   **Nome do repositório** -o nome do repositório. Uma vez definido, este valor não é possível alterar mais tarde. Isso deve ser fornecido um nome exclusivo para que nenhuma outra oferta na sua conta tem o mesmo nome.
    -   **Nome de utilizador** -o nome de utilizador associada com o ACR (nome de utilizador do administrador).
    -   **Palavra-passe** -a palavra-passe associada com o ACR.

    >[!NOTE]
    >O nome de utilizador e palavra-passe são necessários para assegurar que os parceiros têm acesso para o ACR mencionado no processo de publicação.

    Ao publicar uma imagem de contentor, também pode fornecer uma ou mais etiquetas de imagem. Além de uma tag de imagem, parceiros, também podem fornecer resumos de SHA. Não se esqueça de adicionar uma **teste de marcas** à sua imagem para que possa identificar a imagem durante o teste.

5. No separador de Marketplace, adicione o seu conteúdo de marketing específico.

    ![Informação do Marketplace](media/cpp-containers-guide/containers-marketplace-tab.png)

6. No separador de suporte, introduza o seu contacto de engenharia e informações de suporte do cliente.

   ![Informações de suporte](media/cpp-containers-guide/containers-support-tab.png)

7. Selecione **publicar** para publicar a oferta. Depois de selecionar publicar, verá uma linha do tempo que mostra os passos envolvidos para publicar a sua imagem de contentor.
