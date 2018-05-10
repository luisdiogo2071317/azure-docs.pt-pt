---
title: Obter os detalhes da base de dados do Azure Blockchain Workbench
description: Saiba como obter a base de dados do Azure Blockchain Workbench e informações do servidor de base de dados.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 5/4/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: mmercuri
manager: femila
ms.openlocfilehash: bf7cc85e823e6630dbd3278bc91fba85f404059f
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/08/2018
---
# <a name="get-information-about-your-azure-blockchain-workbench-database"></a>Obter informações sobre a base de dados do Azure Blockchain Workbench

Este artigo mostra como obter informações detalhadas sobre a base de dados do Azure Blockchain Workbench.

## <a name="overview"></a>Descrição geral

São fornecidas informações sobre as aplicações, os fluxos de trabalho e execução de contrato inteligente utilizar as vistas de base de dados na base de dados de SQL do Blockchain Workbench. Os programadores podem utilizar estas informações quando utilizando ferramentas como o Microsoft Excel, PowerBI, Visual Studio e SQL Server Management Studio.

Antes de um programador pode ligar à base de dados, que precisam:

* Acesso de cliente externo permitido na firewall de base de dados. Este artigo sobre como configurar um artigo de firewall de base de dados explica como permitir o acesso.
* O nome do servidor de base de dados e o nome de base de dados.

## <a name="connect-to-the-blockchain-workbench-database"></a>Ligar à base de dados Blockchain Workbench

Para ligar à base de dados:

1. Inicie sessão no Portal do Azure com uma conta que tenha **proprietário** permissões para os recursos do Azure Blockchain Workbench.
2. No painel de navegação esquerdo, escolha **grupos de recursos**.
3. Escolha o nome do grupo de recursos para a sua implementação Blockchain Workbench.
4. Selecione **tipo** para ordenar a lista de recursos e, em seguida, escolha o **do SQL Server**. A lista ordenada na captura de ecrã seguinte mostra as duas bases de dados do SQL Server, "original" e que utiliza "lhgn" como o **prefixo recursos**.

   ![Lista de recursos do Blockchain Workbench ordenada](media/blockchain-workbench-getdb-details/sorted-workbench-resource-list.png)

5. Para ver informações detalhadas sobre a base de dados do Blockchain Workbench, selecione a ligação para a base de dados com o **prefixo recursos** fornecido para a implementação Blockchain Workbench.

   ![Detalhes da base de dados](media/blockchain-workbench-getdb-details/workbench-db-details.png)

O nome do servidor de base de dados e o nome de base de dados permitem-lhe estabelecer ligação à base de dados Blockchain Workbench utilizando o seu desenvolvimento ou a ferramenta de relatórios.

## <a name="next-steps"></a>Passos Seguintes

* [Vistas de base de dados no Azure Blockchain Workbench](blockchain-workbench-database-views.md)