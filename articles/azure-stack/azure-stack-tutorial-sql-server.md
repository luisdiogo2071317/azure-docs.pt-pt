---
title: Disponibilizar bases de dados SQL para os utilizadores de pilha do Azure | Microsoft Docs
description: Tutorial para instalar o fornecedor de recursos do SQL Server e criar oferece que permitem que os utilizadores de pilha do Azure criar bases de dados do SQL Server.
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
ms.date: 05/01/2018
ms.author: jeffgilb
ms.reviewer: ''
ms.custom: mvc
ms.openlocfilehash: 0e1eed2601946ddff6fa15f1a1f82398706c920d
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/03/2018
---
# <a name="make-sql-databases-available-to-your-azure-stack-users"></a>Disponibilizar bases de dados SQL para os utilizadores de pilha do Azure
Como um administrador da nuvem de pilha do Azure, pode criar ofertas informar os utilizadores (inquilinos) criar bases de dados do SQL Server que podem utilizar com as respetivas aplicações de nuvem nativo, Web sites e cargas de trabalho. Ao fornecer estas bases de dados personalizados a pedido, baseado na nuvem aos seus utilizadores, pode guardá-los tempo e recursos. Para configurar esta opção, irá:

> [!div class="checklist"]
> * Implementar o fornecedor de recursos do SQL Server
> * Criar uma oferta
> * A oferta de teste

## <a name="deploy-the-sql-server-resource-provider"></a>Implementar o fornecedor de recursos do SQL Server

O processo de implementação é descrito em detalhe no [bases de dados do SQL de utilização do artigo de pilha do Azure](azure-stack-sql-resource-provider-deploy.md)e é composto pelos seguintes passos principais:

1. [Implementar o fornecedor de recursos do SQL Server](azure-stack-sql-resource-provider-deploy.md).
2. [Verificar a implementação](azure-stack-sql-resource-provider-deploy.md#verify-the-deployment-using-the-azure-stack-portal).
3. Fornecem capacidade ao ligar a um servidor SQL de alojamento.

## <a name="create-an-offer"></a>Criar uma oferta

1.  [Define uma quota de](azure-stack-setting-quotas.md) e dê-lhe nome *SQLServerQuota*. Selecione **Microsoft.SQLAdapter** para o **espaço de nomes** campo.
2.  [Criar um plano](azure-stack-create-plan.md). Nome *TestSQLServerPlan*, selecione o **Microsoft.SQLAdapter** serviço, e **SQLServerQuota** quota.

    > [!NOTE]
    > Para permitir aos utilizadores criar outras aplicações, os outros serviços poderão ser necessário no plano. Por exemplo, as funções do Azure requer que o plano inclui o **Microsoft** service, enquanto o Wordpress requer **Microsoft.MySQLAdapter**.
    > 
    >

3.  [Criar uma oferta](azure-stack-create-offer.md), nome **TestSQLServerOffer** e selecione o **TestSQLServerPlan** plano.

## <a name="test-the-offer"></a>A oferta de teste

Agora que implementou o fornecedor de recursos do SQL Server e criar uma oferta, pode iniciar sessão como um utilizador, subscrever a oferta e criar uma base de dados.

### <a name="subscribe-to-the-offer"></a>Subscrever a oferta
1. Inicie sessão no portal do Azure pilha (https://portal.local.azurestack.external) como um inquilino.
2. Clique em **obter uma subscrição** e, em seguida, escreva **TestSQLServerSubscription** em **nome a apresentar**.
3. Clique em **selecionar uma oferta** > **TestSQLServerOffer** > **criar**.
4. Clique em **mais serviços** > **subscrições** > **TestSQLServerSubscription** > **recursos fornecedores**.
5. Clique em **registar** junto a **Microsoft.SQLAdapter** fornecedor.

### <a name="create-a-sql-database"></a>Criar uma base de dados SQL

1. Clique em **+**  >  **dados + armazenamento** > **base de dados SQL**.
2. Deixe as predefinições para os campos ou pode utilizar estes exemplos:
    - **Nome de base de dados**: SQLdb
    - **Tamanho máximo em MB**: 100
    - **Subscrição**: TestSQLOffer
    - **Grupo de recursos**: RG do SQL Server
3. Clique em **definições de início de sessão**, introduza as credenciais para a base de dados e, em seguida, clique em **OK**.
4. Clique em **SKU** > selecione o SKU de SQL que criou para o servidor de alojamento do SQL Server > **OK**.
5. Clique em **Criar**.

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Implementar o fornecedor de recursos do SQL Server
> * Criar uma oferta
> * A oferta de teste

Avançadas para o próximo tutorial para saber como:

> [!div class="nextstepaction"]
> [Disponibilizar web, móveis e API apps aos seus utilizadores]( azure-stack-tutorial-app-service.md)

