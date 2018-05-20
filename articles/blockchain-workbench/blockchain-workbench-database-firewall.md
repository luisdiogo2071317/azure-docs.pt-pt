---
title: Configurar a firewall da BD SQL do Azure Blockchain Workbench
description: Saiba como configurar a firewall da BD SQL do Azure Blockchain Workbench.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 5/4/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: mmercuri
manager: femila
ms.openlocfilehash: afeea143f73fa4f7d3e373535007846a668616ab
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/12/2018
---
# <a name="configure-the-azure-blockchain-workbench-database-firewall"></a>Configurar a firewall da base de dados do Azure Blockchain Workbench

Este artigo mostra como configurar uma regra de firewall com o portal do Azure. As regras de firewall permitem às aplicações ou clientes externos ligar à base de dados do Azure Blockchain Workbench.

## <a name="connect-to-the-blockchain-workbench-database"></a>Ligar à base de dados do Blockchain Workbench

Para ligar à base de dados onde quer configurar uma regra:

1. Inicie sessão no Portal do Azure com uma conta que tenha permissões de **Proprietário** para os recursos do Azure Blockchain Workbench.
2. Na navegação à esquerda, selecione **Grupos de recursos**.
3. Escolha o nome do grupo de recursos para a sua implementação do Blockchain Workbench.
4. Selecione **Tipo** para ordenar a lista de recursos e, em seguida, selecione **SQL Server**.
5. O exemplo da lista de recursos na captura de ecrã seguinte mostra duas bases de dados: *mestra* e *lsgn-sdk*. Pode configurar a regra de firewall em *lsgn-sdk*.

![Listar os recursos do Blockchain Workbench](media/blockchain-workbench-database-firewall/list-database-resources.png)

## <a name="create-a-database-firewall-rule"></a>Criar uma regra de firewall de base de dados

Para criar uma regra de firewall:

1. Escolha a ligação à base de dados "lsgn-sdk".
2. Na barra de menus, selecione **Definir firewall do servidor**.

   ![Definir firewall do servidor](media/blockchain-workbench-database-firewall/configure-server-firewall.png)

3. Para criar uma regra para a sua organização:

   * Introduza um **NOME DA REGRA**
   * Introduza um endereço IP para o **IP INICIAL** do intervalo de endereços
   * Introduza um endereço IP para o **IP FINAL** do intervalo de endereços

   ![Criar regra de firewall](media/blockchain-workbench-database-firewall/create-firewall-rule.png)

    > [!NOTE]
    > Se apenas quiser adicionar o endereço IP do seu computador, selecione **+ Adicionar IP do cliente**.
        
1. Para guardar a configuração da firewall, selecione **Guardar**.
2. Teste o intervalo de endereços IP que configurou para a base de dados ao ligar a uma aplicação ou ferramenta. Por exemplo, SQL Server Management Studio.

## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"]
> [Vistas de base de dados no Azure Blockchain Workbench](blockchain-workbench-database-views.md)