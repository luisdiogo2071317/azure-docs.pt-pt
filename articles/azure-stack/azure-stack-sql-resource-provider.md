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
ms.date: 06/21/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: 55d0e51606e8768a01c0b5a7766dbafe24d97a0d
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/21/2018
ms.locfileid: "36307830"
---
# <a name="use-sql-databases-on-microsoft-azure-stack"></a>Utilizar bases de dados do SQL Server na pilha do Microsoft Azure

Utilizar a placa de fornecedor de recursos do SQL Server API para expor bases de dados do SQL Server como um serviço de [Azure pilha](azure-stack-poc.md). Depois de instalar o fornecedor de recursos e ligá-lo para um ou mais instâncias do SQL Server, podem criar e os seus utilizadores:

- Bases de dados para aplicações em nuvem nativo.
- Web sites que utilizam o SQL Server.
- Cargas de trabalho que utilizam o SQL Server.

O fornecedor de recursos não fornece todas as a base de dados capacidades de gestão de [SQL Database do Azure](https://azure.microsoft.com/services/sql-database/). Por exemplo, os conjuntos elásticos que atribuem automaticamente recursos não são suportados. No entanto, o suporta de fornecedor de recursos semelhante cria, ler, atualiza e eliminar as operações (CRUD) numa base de dados do SQL Server. Para mais informações sobre a API do fornecedor de recursos, consulte [Windows Azure Pack SQL Server recursos fornecedor referência da API REST](https://msdn.microsoft.com/library/dn528529.aspx).

>[!NOTE]
O fornecedor de recursos do SQL Server API não é compatível com a SQL Database do Azure.

## <a name="sql-resource-provider-adapter-architecture"></a>Arquitetura de adaptador do fornecedor de recursos do SQL Server

O fornecedor de recursos é composta pelos seguintes componentes:

- **SQL Server recursos fornecedor adaptador máquina virtual (VM)**, que é uma VM do Windows Server que executa os serviços do fornecedor.
- **O fornecedor de recursos**, que processa os pedidos de acessos base de dados e recursos.
- **Servidores que alojam o SQL Server**, que fornecem a capacidade para bases de dados denominados servidores de alojamento.

Tem de criar pelo menos uma instância do SQL Server ou conceder acesso a instâncias externas do SQL Server.

> [!NOTE]
> Alojamento de servidores que estejam instalados na pilha do Azure integrados sistemas tem de ser criados de uma subscrição de inquilino. Estes não podem ser criados da subscrição de fornecedor predefinido. Deve ser criadas no portal inquilino ou utilizando o PowerShell com o adequado início de sessão. Todos os servidores de alojamento são facturável máquinas virtuais e tem de ter as licenças. O administrador de serviço pode ser o proprietário da subscrição de inquilino.

## <a name="next-steps"></a>Passos Seguintes

[Implementar o fornecedor de recursos do SQL Server](azure-stack-sql-resource-provider-deploy.md)
