---
title: Implementar um contentor de Docker do ASP.NET para o Azure Container Registry (ACR) | Documentos da Microsoft
description: Saiba como utilizar o Visual Studio Tools for Docker para implementar uma aplicação web ASP.NET Core para um registo de contentor
services: azure-container-service
documentationcenter: .net
author: mlearned
manager: douge
editor: ''
ms.assetid: e5e81c5e-dd18-4d5a-a24d-a932036e78b9
ms.service: azure-container-service
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/21/2018
ms.author: mlearned
ms.openlocfilehash: 58df17b17de1d93683875b68dd7c6c087bc6d16d
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/12/2018
ms.locfileid: "38972313"
---
# <a name="deploy-an-aspnet-container-to-a-container-registry-using-visual-studio"></a>Implementar um contentor ASP.NET para um registo de contentor com o Visual Studio
## <a name="overview"></a>Descrição geral
O docker é um mecanismo de contentor leve, semelhante em vários aspetos a uma máquina virtual, que pode utilizar para alojar aplicações e serviços.
Este tutorial explica-lhe com o Visual Studio para publicar a aplicação contentorizada numa [Azure Container Registry](https://azure.microsoft.com/services/container-registry).

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/dotnet/?utm_source=acr-publish-doc&utm_medium=docs&utm_campaign=docs) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos
Para concluir este tutorial:

* Instale a versão mais recente do [Visual Studio 2017](https://azure.microsoft.com/downloads/) com a carga de trabalho de "desenvolvimento do ASP.NET e web"
* Instalar [Docker para Windows](https://docs.docker.com/docker-for-windows/install/)

## <a name="1-create-an-aspnet-core-web-app"></a>1. Criar uma aplicação Web ASP.NET Core
Os seguintes passos guiá-lo através da criação de uma aplicação ASP.NET Core básica, que será utilizada neste tutorial.

[!INCLUDE [create-aspnet5-app](../includes/create-aspnet5-app.md)]

## <a name="2-publish-your-container-to-azure-container-registry"></a>2. Publicar o seu contentor para o Azure Container Registry
1. Com o botão direito do rato no **Explorador de soluções** e escolha **Publish**.
2. Na caixa de diálogo do destino de publicar, selecione o **Container Registry** separador.
3. Escolher **novo Azure Container Registry** e clique em **Publish**.
4. Preencha os valores pretendidos na **criar um novo registo de contentor do Azure**.

    | Definição      | Valor sugerido  | Descrição                                |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Prefixo DNS** | Nome globalmente exclusivo | Nome que identifica exclusivamente o seu registo de contentor. |
    | **Subscrição** | Escolher a sua subscrição | A subscrição do Azure que deve utilizar. |
    | **[Grupo de Recursos](../articles/azure-resource-manager/resource-group-overview.md)** | myResourceGroup |  Nome do grupo de recursos no qual pretende criar o seu registo de contentor. Escolha **Novo** para criar um grupo de recursos novo.|
    | **[SKU](https://docs.microsoft.com/azure/container-registry/container-registry-skus)** | Standard | Escalão de serviço de registo de contentor  |
    | **Localização do registo** | Uma localização perto de si | Escolha uma localização de um [região](https://azure.microsoft.com/regions/) perto de ou outros serviços que irá utilizar o seu registo de contentor. |
    ![Visual Studio criar caixa de diálogo do Azure Container Registry][0]
5. Clique em **Criar**.

Pode agora solicitar o contentor do registo em qualquer anfitrião com capacidade para executar imagens do Docker, por exemplo [Azure Container Instances](./container-instances/container-instances-tutorial-deploy-app.md).

[0]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/vs-acr-provisioning-dialog.png
