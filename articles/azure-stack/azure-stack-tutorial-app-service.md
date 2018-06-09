---
title: Certifique-web e API apps disponíveis para os utilizadores de pilha do Azure | Microsoft Docs
description: Tutorial para instalar o fornecedor de recursos do serviço de aplicações e criar oferece que conceder aos seus utilizadores de pilha do Azure a capacidade de criar web e API apps.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 06/05/2018
ms.author: jeffgilb
ms.reviewer: ''
ms.custom: mvc
ms.openlocfilehash: 0171dba639e480a04cdd1c7f23d546d01121fb42
ms.sourcegitcommit: 50f82f7682447245bebb229494591eb822a62038
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/08/2018
ms.locfileid: "35247403"
---
# <a name="tutorial-make-web-and-api-apps-available-to-your-azure-stack-users"></a>Tutorial: disponibilizar web e API apps aos seus utilizadores de pilha do Azure

Como um administrador da nuvem de pilha do Azure, pode criar ofertas informar os utilizadores (inquilinos) criam aplicações das funções do Azure e web e API. Ao fornecer aos utilizadores acesso a estas aplicações a pedido, baseado na nuvem, pode guardá-los tempo e recursos.

Para configurar esta opção, irá:

> [!div class="checklist"]
> * Implementar o fornecedor de recursos do serviço de aplicações
> * Criar uma oferta
> * A oferta de teste

## <a name="deploy-the-app-service-resource-provider"></a>Implementar o fornecedor de recursos do serviço de aplicações

1. [Preparar o anfitrião do Kit de desenvolvimento de pilha do Azure](azure-stack-app-service-before-you-get-started.md). Isto inclui a implementar o fornecedor de recursos do SQL Server, que é necessário para a criação de algumas aplicações.
2. [Transferir os instalador e do programa auxiliar de scripts](azure-stack-app-service-deploy.md).
3. [Execute o script de programa auxiliar para criar os certificados necessários](azure-stack-app-service-deploy.md).
4. [Instalar o fornecedor de recursos do serviço de aplicações](azure-stack-app-service-deploy.md) (irá demorar algumas horas para instalar e para todas as funções de trabalho a aparecer.)
5. [Validar a instalação](azure-stack-app-service-deploy.md#validate-the-app-service-on-azure-stack-installation).

## <a name="create-an-offer"></a>Criar uma oferta

Por exemplo, pode criar uma oferta, que permite aos utilizadores criar sistemas de gestão de conteúdos do DNN web. Requer o serviço do SQL Server que já ativou ao instalar o fornecedor de recursos do SQL Server.

1.  [Define uma quota de](azure-stack-setting-quotas.md) e dê-lhe nome *AppServiceQuota*. Selecione **Microsoft** para o **espaço de nomes** campo.
2.  [Criar um plano](azure-stack-create-plan.md). Nome *TestAppServicePlan*, selecione o **Microsoft.SQL** serviço e o **AppService Quota** quota.

    > [!NOTE]
    > Para permitir aos utilizadores criar outras aplicações, os outros serviços poderão ser necessário no plano. Por exemplo, as funções do Azure requer o **Microsoft** serviço no plano de enquanto Wordpress requer **Microsoft.MySQL**.

3.  [Criar uma oferta](azure-stack-create-offer.md), nome **TestAppServiceOffer** e selecione o **TestAppServicePlan** plano.

## <a name="test-the-offer"></a>A oferta de teste

Agora que implementou o fornecedor de recursos do serviço de aplicações e criar uma oferta, pode iniciar sessão como um utilizador, subscrever a oferta e criar uma aplicação.

Neste exemplo, vamos criar um sistema de gestão de conteúdos DNN plataforma. Em primeiro lugar, crie uma base de dados do SQL Server e, em seguida, a aplicação de web DNN.

### <a name="subscribe-to-the-offer"></a>Subscrever a oferta

1. Inicie sessão no portal do Azure pilha (https://portal.local.azurestack.external) como um inquilino.
2. Selecione **obter uma subscrição** >, introduza **TestAppServiceSubscription** em **nome a apresentar** > **selecionar uma oferta**  >  **TestAppServiceOffer** > **criar**.

### <a name="create-a-sql-database"></a>Criar uma base de dados SQL

1. Selecione **+**  >  **dados + armazenamento** > **base de dados SQL**.
2. Mantenha os valores predefinidos, exceto para os seguintes campos:

    - **Nome de base de dados**: DNNdb
    - **Tamanho máximo em MB**: 100
    - **Subscrição**: TestAppServiceOffer
    - **Grupo de recursos**: DNN RG

3. Selecione **definições de início de sessão**, introduza as credenciais para a base de dados e, em seguida, selecione **OK**. Irá utilizar estas credenciais mais tarde no tutorial.
4. Em **SKU** > selecione o SKU de SQL que criou para o servidor de alojamento do SQL Server > e, em seguida, selecione **OK**.
5. Selecione **Criar**.

### <a name="create-a-dnn-app"></a>Criar uma aplicação DNN

1. Selecione **+**  >  **ver todos os** > **pré-visualização de plataforma DNN** > **criar** .
2. Introduza *DNNapp* em **nome da aplicação** e selecione **TestAppServiceOffer** em **subscrição**.
3. Selecione **configurar definições necessárias** > **criar novo** > introduza um **plano do App Service** nome.
4. Selecione **escalão de preço** > **F1 gratuito** > **selecione** > **OK**.
5. Selecione **base de dados** e introduza as credenciais para a base de dados do SQL Server que criou anteriormente.
6. Selecione **Criar**.

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Implementar o fornecedor de recursos do serviço de aplicações
> * Criar uma oferta
> * A oferta de teste

Avançadas para o próximo tutorial para saber como:

> [!div class="nextstepaction"]
> [Implementar aplicações do Azure e Azure pilha](user/azure-stack-solution-pipeline.md)
