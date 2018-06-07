---
title: Implementar um contentor de ASP.NET Docker para registo de contentor do Azure (ACR) | Microsoft Docs
description: Saiba como utilizar o Visual Studio Tools para Docker para implementar uma aplicação web do ASP.NET Core um registo de contentor
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
ms.openlocfilehash: 4442c1d763f4ed21a5efeedbe957727254e2a0b8
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34658476"
---
# <a name="deploy-an-aspnet-container-to-a-container-registry-using-visual-studio"></a>Implementar um contentor do ASP.NET para um registo de contentor com o Visual Studio
## <a name="overview"></a>Descrição geral
Docker é um motor de contentor simples, semelhante em alguns aspetos a uma máquina virtual, que pode utilizar para o anfitrião de aplicações e serviços.
Este tutorial explica-lhe utilizar o Visual Studio para publicar a aplicação de para um [registo de contentor do Azure](https://azure.microsoft.com/en-us/services/container-registry).

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/dotnet/?utm_source=acr-publish-doc&utm_medium=docs&utm_campaign=docs) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos
Para concluir este tutorial:

* Instale a versão mais recente do [Visual Studio 2017](https://azure.microsoft.com/en-us/downloads/) com a carga de trabalho "desenvolvimento ASP.NET e web"
* Instalar [Docker para Windows](https://docs.docker.com/docker-for-windows/install/)

## <a name="1-create-an-aspnet-core-web-app"></a>1. Criar uma aplicação Web ASP.NET Core
Os seguintes passos guiá-lo através da criação de uma aplicação ASP.NET Core básica que vai ser utilizada neste tutorial.

[!INCLUDE [create-aspnet5-app](../includes/create-aspnet5-app.md)]

## <a name="2-publish-your-container-to-azure-container-registry"></a>2. Publicar o contentor do registo de contentor do Azure
1. Clique com o botão direito no projeto **Explorador de soluções** e escolha **publicar**.
2. Na caixa de diálogo do destino de publicar, selecione o **contentor registo** separador.
3. Escolha **novo registo de contentor do Azure** e clique em **publicar**.
4. Preencha os valores pretendidos no **criar um novo registo de contentor do Azure**.

    | Definição      | Valor sugerido  | Descrição                                |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Prefixo DNS** | Nome globalmente exclusivo | Nome que identifica exclusivamente o seu registo de contentor. |
    | **Subscrição** | Escolher a sua subscrição | A subscrição do Azure que deve utilizar. |
    | **[Grupo de Recursos](../articles/azure-resource-manager/resource-group-overview.md)** | myResourceGroup |  Nome do grupo de recursos no qual pretende criar o registo de contentor. Escolha **novo** para criar um novo grupo de recursos.|
    | **[SKU](https://docs.microsoft.com/en-us/azure/container-registry/container-registry-skus)** | Standard | Camada de serviço de registo do contentor  |
    | **Localização do registo** | Uma localização perto de si | Escolha uma localização de um [região](https://azure.microsoft.com/regions/) perto de si ou junto de outros serviços que irão utilizar o registo de contentor. |
    ![Visual Studio criar caixa de diálogo de registo de contentor do Azure][0]
5. Clique em **Criar**.

Pode agora solicitar o contentor do registo para qualquer anfitrião capaz de executar imagens de Docker, por exemplo [instâncias de contentor do Azure](./container-instances/container-instances-tutorial-deploy-app.md).

[0]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/vs-acr-provisioning-dialog.png
