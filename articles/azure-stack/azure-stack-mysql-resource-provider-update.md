---
title: Utilizar bases de dados fornecidas pelo RP de adaptador MySQL no AzureStack | Microsoft Docs
description: Como criar e gerir bases de dados MySQL aprovisionados utilizando o fornecedor de recursos do adaptador de MySQL
services: azure-stack
documentationCenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/26/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: 0a900d75315fd0015633c036877faef84c48d65b
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37031849"
---
# <a name="create-mysql-databases"></a>Criar bases de dados MySQL

Pode criar e gerir bases de dados personalizados no portal de utilizador. Um utilizador de pilha do Azure tem uma subscrição com uma oferta, que inclui o serviço de base de dados MySQL.

## <a name="test-your-deployment-by-creating-a-mysql-database"></a>Testar a implementação através da criação de uma base de dados MySQL

1. Inicie sessão no portal de utilizador de pilha do Azure.
2. Selecione **+ nova** > **dados + armazenamento** > **base de dados MySQL** > **adicionar**.
3. Em **criar base de dados MySQL**, introduza o nome de base de dados e configurar outras definições conforme necessário para o seu ambiente.

    ![Criar um teste de base de dados MySQL](./media/azure-stack-mysql-rp-deploy/mysql-create-db.png)

4. Em **Create Database**, selecione **SKU**. Em **selecionar um SKU de MySQL**, escolha o SKU da base de dados.

    ![Selecione um SKU de MySQL](./media/azure-stack-mysql-rp-deploy/mysql-select-a-sku.png)

    >[!Note]
    >Servidores de alojamento são adicionadas à pilha do Azure, foram atribuídos um SKU. Bases de dados são criados no agrupamento de servidores de um SKU de alojamento.

5. Em **início de sessão**, selecione ***configurar definições necessárias***.
6. Em **selecionar um início de sessão**, pode escolher um início de sessão existente ou selecione **+ criar um novo início de sessão** para configurar um novo início de sessão.  Introduza um **início de sessão da base de dados** nome e **palavra-passe**e, em seguida, selecione **OK**.

    ![Criar um novo início de sessão de base de dados](./media/azure-stack-mysql-rp-deploy/create-new-login.png)

    >[!NOTE]
    >O comprimento do nome de início de sessão da base de dados não pode exceder 32 carateres no MySQL 5.7. Nas edições anteriores, este não pode exceder os 16 carateres.

7. Selecione **criar** para concluir a configuração da base de dados.

Depois da base de dados é implementado, tome nota do **cadeia de ligação** em **Essentials**. Pode utilizar esta cadeia de qualquer aplicação que necessita de aceder à base de dados MySQL.

![Obter a cadeia de ligação para a base de dados MySQL](./media/azure-stack-mysql-rp-deploy/mysql-db-created.png)

## <a name="update-the-administrative-password"></a>Atualizar a palavra-passe administrativa

Pode modificar a palavra-passe por alterá-lo na instância de servidor MySQL.

1. Selecione **recursos administrativos** > **MySQL servidores de alojamento**. Selecione o servidor de alojamento.
2. Em **definições**, selecione **palavra-passe**.
3. Em **palavra-passe**, introduza a palavra-passe nova e, em seguida, selecione **guardar**.

![Atualizar a palavra-passe de administrador](./media/azure-stack-mysql-rp-deploy/mysql-update-password.png)

## <a name="next-steps"></a>Passos Seguintes

[Manter o fornecedor de recursos de MySQL](azure-stack-mysql-resource-provider-maintain.md)