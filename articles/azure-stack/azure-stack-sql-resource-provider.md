---
title: Bases de dados SQL a utilizar na pilha do Azure | Microsoft Docs
description: Saiba como pode implementar bases de dados do SQL Server como um serviço na pilha do Azure e os passos rápidos para implementar o adaptador de fornecedor de recursos do SQL Server.
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
ms.date: 05/01/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: b1cc1fad6b0831bcf0bab5ba4f37b753c3cf33ca
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/04/2018
---
# <a name="use-sql-databases-on-microsoft-azure-stack"></a>Utilizar bases de dados do SQL Server na pilha do Microsoft Azure
Utilizar a placa de fornecedor de recursos do SQL Server para expor bases de dados do SQL Server como um serviço de [Azure pilha](azure-stack-poc.md). Depois de instalar o fornecedor de recursos e ligá-lo para um ou mais instâncias do SQL Server, podem criar e os seus utilizadores:
- Bases de dados para aplicações em nuvem nativo.
- Web sites baseados em SQL.
- Cargas de trabalho que se baseiam em SQL.
Não tem de aprovisionar uma máquina virtual (VM) que aloja o servidor de SQL cada vez.

O fornecedor de recursos não suporta todas as capacidades de gestão de base de dados do [SQL Database do Azure](https://azure.microsoft.com/services/sql-database/). Por exemplo, conjuntos de base de dados elástica e a capacidade de aumentar e reduzir verticalmente o desempenho de base de dados automaticamente não estão disponíveis. No entanto, o suporte de does de fornecedor de recursos semelhante criar, ler, atualizar e eliminar as operações (CRUD). A API não é compatível com a base de dados SQL.

## <a name="sql-resource-provider-adapter-architecture"></a>Arquitetura de adaptador do fornecedor de recursos do SQL Server
O fornecedor de recursos é composto por três componentes:

- **O adaptador de fornecedor de recursos do SQL Server VM**, que é uma máquina virtual do Windows que executa os serviços do fornecedor.
- **O fornecedor de recursos próprio**, que processa os pedidos de aprovisionamento e expõe recursos de base de dados.
- **Servidores que alojam o SQL Server**, que fornecem a capacidade para bases de dados denominados servidores de alojamento.

Deve criar uma (ou mais) instâncias do SQL Server e/ou fornecer acesso a instâncias externas do SQL Server.

> [!NOTE]
> Alojamento de servidores que estejam instalados na pilha do Azure integrados sistemas tem de ser criados de uma subscrição de inquilino. Estes não podem ser criados da subscrição de fornecedor predefinido. Deve ser criadas no portal de inquilinos ou de uma sessão do PowerShell com um adequado início de sessão. Todos os servidores de alojamento são cobráveis VMs e tem de ter licenças adequadas. O administrador de serviço pode ser o proprietário da subscrição de inquilino.


## <a name="next-steps"></a>Passos Seguintes

[Implementar o fornecedor de recursos do SQL Server](azure-stack-sql-resource-provider-deploy.md)
