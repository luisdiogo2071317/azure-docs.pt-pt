---
title: "Criar uma instância do Azure Database Migration Service com o portal do Azure | Microsoft Docs"
description: "Utilizar o portal do Azure para criar uma instância do Azure Database Migration Service"
services: database-migration
author: edmacauley
ms.author: edmaca
manager: craigg
ms.reviewer: 
ms.service: database-migration
ms.workload: data-services
ms.custom: mvc
ms.topic: quickstart
ms.date: 12/13/2017
ms.openlocfilehash: 9dea80b0a6848bd69541aa9f7e0a0fe111fa0a28
ms.sourcegitcommit: d247d29b70bdb3044bff6a78443f275c4a943b11
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/13/2017
---
# <a name="create-an-instance-of-the-azure-database-migration-service-by-using-the-azure-portal"></a>Criar uma instância do Azure Database Migration Service com o portal do Azure
Neste Início Rápido, utiliza o portal do Azure para criar uma instância do Azure Database Migration Service.  Depois de criar o serviço, pode utilizá-lo para migrar dados do SQL Server no local para uma base de dados SQL do Azure.

Se não tiver uma subscrição do Azure, crie uma conta [gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="log-in-to-the-azure-portal"></a>Iniciar sessão no portal do Azure
Abra o browser, navegue para o [portal do Microsoft Azure](https://portal.azure.com/) e, em seguida, introduza as suas credenciais para iniciar sessão no portal.

A vista predefinida é o dashboard de serviço.

## <a name="register-the-resource-provider"></a>Registar o fornecedor de recursos
Registe o fornecedor de recursos Microsoft.DataMigration antes de criar a primeira instância do Database Migration Service.

1. No portal do Azure, selecione **Todos os serviços** e, em seguida, selecione **Subscrições**.

2. Selecione a subscrição na qual pretende criar a instância do Database Migration Service do Azure e, em seguida, selecione **Fornecedores de recursos**.

3. Procure por migração e, em seguida, à direita de Microsoft.DataMigration, selecione **Registar**.

![Registar o fornecedor de recursos](media/quickstart-create-data-migration-service-portal/dms-register-provider.png)

## <a name="create-an-instance-of-the-service"></a>Criar uma instância do serviço
1. Clique em **+ Criar um recurso** para criar uma instância do Azure Database Migration Service, que está atualmente em pré-visualização.

2. Procure no marketplace por "migração", selecione **Azure Database Migration Service** e, em seguida, no ecrã **Azure Database Migration Service (pré-visualização)** e clique em **Criar**.

3. No ecrã **Azure Database Migration Service**: 

    - Escolha um **Nome de serviço** que seja recordável e exclusivo para identificar a sua instância do Azure Database Migration Service.
    - Selecione a sua **Subscrição** do Azure, na qual pretende criar a instância.
    - Crie uma nova aplicação **Rede** com um nome exclusivo.
    - Escolha a **Localização** que esteja mais próxima do seu servidor de origem ou de destino.
    - Selecione Básico: 1 vCore para o **Escalão de preço**.

    ![Criar serviço de migração](media/quickstart-create-data-migration-service-portal/dms-create-service.png)
4. Selecione **Criar**.

Após alguns momentos, a sua instância do serviço Azure Database Migration está criado e pronto para utilizar. O Database Migration Service aparece conforme mostrado na seguinte imagem:

![O serviço de migração foi criado](media/quickstart-create-data-migration-service-portal/dms-service-created.png)

## <a name="clean-up-resources"></a>Limpar recursos
Pode limpar os recursos criados neste Início Rápido ao eliminar o [grupo de recursos do Azure](../azure-resource-manager/resource-group-overview.md).  Para eliminar o grupo de recursos, navegue para a instância do Azure Database Migration Service que criou. Selecione o nome do **Grupo de recursos** e selecione **Eliminar grupo de recursos**.  Esta ação elimina todos os recursos no grupo de recursos, bem como o próprio grupo.

## <a name="next-steps"></a>Passos seguintes
> [!div class="nextstepaction"]
> [Migrar o SQL Server local para a Base de Dados SQL do Azure](tutorial-sql-server-to-azure-sql.md)