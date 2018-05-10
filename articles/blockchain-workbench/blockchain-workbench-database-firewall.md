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
ms.openlocfilehash: dc22f212c014ab1d6622eff3491d669b21ca6f47
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/08/2018
---
# <a name="configure-the-azure-blockchain-workbench-database-firewall"></a>Configurar a firewall de base de dados do Azure Blockchain Workbench

Este artigo mostra como configurar uma regra de firewall com o portal do Azure. Regras de firewall permitem clientes externos ou aplicações de ligação à base de dados do Azure Blockchain Workbench.

## <a name="connect-to-the-blockchain-workbench-database"></a>Ligar à base de dados Blockchain Workbench

Para ligar à base de dados onde pretende configurar uma regra:

1. Inicie sessão no Portal do Azure com uma conta que tenha **proprietário** permissões para os recursos do Azure Blockchain Workbench.
2. No painel de navegação esquerdo, escolha **grupos de recursos**.
3. Escolha o nome do grupo de recursos para a sua implementação Blockchain Workbench.
4. Selecione **tipo** para ordenar a lista de recursos e, em seguida, escolha o **do SQL Server**.
5. O exemplo de lista de recursos na captura de ecrã seguinte mostra duas bases de dados: *mestre* e *lsgn sdk*. Configurar a regra de firewall no *lsgn sdk*.

![Lista de recursos do Blockchain Workbench](media/blockchain-workbench-database-firewall/list-database-resources.png)

## <a name="create-a-database-firewall-rule"></a>Criar uma regra de firewall de base de dados

Para criar uma regra de firewall:

1. Escolha a ligação à base de dados "lsgn-sdk".
2. Na barra de menus, selecione **definir a firewall do servidor**.

   ![Definir firewall do servidor](media/blockchain-workbench-database-firewall/configure-server-firewall.png)

3. Para criar uma regra para a sua organização:

   * Introduza um **nome da regra**
   * Introduza um endereço IP para o **IP inicial** do intervalo de endereços
   * Introduza um endereço IP para o **IP final** do intervalo de endereços

   ![Criar regra de firewall](media/blockchain-workbench-database-firewall/create-firewall-rule.png)

    > [!NOTE]
    > Se apenas pretender adicionar o endereço IP do seu computador, escolha **+ Adicionar IP do cliente**.
        
1. Para guardar a configuração de firewall, selecione **guardar**.
2. O intervalo de endereços IP que configurou para a base de dados através da ligação de uma aplicação ou a ferramenta de teste. Por exemplo, SQL Server Management Studio.

## <a name="next-steps"></a>Passos Seguintes

* [Vistas de base de dados no Azure Blockchain Workbench](blockchain-workbench-database-views.md)