---
title: Disponibilizar bases de dados SQL para os utilizadores do Azure Stack | Documentos da Microsoft
description: Tutorial para instalar o fornecedor de recursos do SQL Server e criar oferece que permitem que os utilizadores do Azure Stack criar bases de dados SQL.
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
ms.date: 11/05/2018
ms.author: jeffgilb
ms.reviewer: quying
ms.lastreviewed: 11/05/2018
ms.custom: mvc
ms.openlocfilehash: 983e8b279261d3ff8e5d24c8e3a6f61c5a787e5b
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/30/2019
ms.locfileid: "55240816"
---
# <a name="tutorial-make-sql-databases-available-to-your-azure-stack-users"></a>Tutorial: disponibilizar bases de dados SQL para os utilizadores do Azure Stack

Como um administrador de nuvem do Azure Stack, pode criar ofertas que permita que os utilizadores (inquilinos) criar bases de dados do SQL que eles podem usar com as suas cargas de trabalho, Web sites e aplicações nativas da cloud. Ao fornecer esses bancos de dados personalizados, sob demanda, com base na cloud aos seus utilizadores, pode guardá-los tempo e recursos. Para configurar isso, irá:

> [!div class="checklist"]
> * Implementar o fornecedor de recursos do SQL Server
> * Criar uma oferta
> * Testar a oferta

## <a name="deploy-the-sql-server-resource-provider"></a>Implementar o fornecedor de recursos do SQL Server

O processo de implantação é descrito detalhadamente no [bases de dados do SQL de utilização no artigo do Azure Stack](azure-stack-sql-resource-provider-deploy.md)e é composta pelos seguintes passos principais:

1. [Implementar o fornecedor de recursos do SQL](azure-stack-sql-resource-provider-deploy.md).
2. [Verificar a implementação](azure-stack-sql-resource-provider-deploy.md#verify-the-deployment-using-the-azure-stack-portal).
3. Fornece capacidade ao ligar a um SQL server de alojamento. Para obter mais informações, consulte [adicionar servidores de alojamento](azure-stack-sql-resource-provider-hosting-servers.md)

## <a name="create-an-offer"></a>Criar uma oferta

1.  [Define uma quota](azure-stack-setting-quotas.md) e o nomeio *SQLServerQuota*. Selecione **Microsoft.SQLAdapter** para o **espaço de nomes** campo.
2.  [Criar um plano](azure-stack-create-plan.md). Atribua o nome *TestSQLServerPlan*, selecione a **Microsoft.SQLAdapter** serviço, e **SQLServerQuota** quota.

    > [!NOTE]
    > Para permitir aos utilizadores criar outras aplicações, os outros serviços poderão ser necessárias no plano. Por exemplo, as funções do Azure requer o **Microsoft. Storage** serviço no plano, embora exija Wordpress **Microsoft.MySQLAdapter**.

3.  [Criar uma oferta](azure-stack-create-offer.md), nomeie- **TestSQLServerOffer** e selecione o **TestSQLServerPlan** plano.

## <a name="test-the-offer"></a>Testar a oferta

Agora que implementou o fornecedor de recursos do SQL Server e criou uma oferta, pode iniciar sessão como um utilizador, subscreva a oferta e criar uma base de dados.

### <a name="subscribe-to-the-offer"></a>Subscreva a oferta

1. Inicie sessão no portal do Azure Stack (https://portal.local.azurestack.external) como um inquilino.
2. Selecione **obter uma subscrição** e, em seguida, introduza **TestSQLServerSubscription** sob **nome a apresentar**.
3. Selecione **selecionar uma oferta** > **TestSQLServerOffer** > **criar**.
4. Selecione **todos os serviços** > **subscrições** > **TestSQLServerSubscription** > **recursos fornecedores**.
5. Selecione **registar** junto a **Microsoft.SQLAdapter** fornecedor.

### <a name="create-a-sql-database"></a>Criar uma base de dados SQL

1. Selecione **+**  >  **dados + armazenamento** > **base de dados SQL**.
2. Mantenha os valores predefinidos ou utilizar estes exemplos para os seguintes campos:
    - **Nome de base de dados**: SQLdb
    - **Tamanho máximo em MB**: 100
    - **Subscrição**: TestSQLOffer
    - **Grupo de recursos**: SQL-RG
3. Selecione **definições de início de sessão**, introduza as credenciais para a base de dados e, em seguida, selecione **OK**.
4. Selecione **SKU** > selecione o SKU de SQL que criou para o servidor de alojamento de SQL > e, em seguida, selecione **OK**.
5. Selecione **Criar**.

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Implementar o fornecedor de recursos do SQL Server
> * Criar uma oferta
> * Testar a oferta

Avance para o próximo tutorial para saber como:

> [!div class="nextstepaction"]
> [Disponibilizar aplicações de API, web e móveis aos seus utilizadores]( azure-stack-tutorial-app-service.md)