---
title: Utilizar bases de dados SQL no Azure Stack | Documentos da Microsoft
description: Saiba como pode implementar bases de dados SQL como um serviço no Azure Stack e os passos rápidos para implementar o adaptador de fornecedor de recursos do SQL Server.
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
ms.date: 02/12/2019
ms.author: jeffgilb
ms.reviewer: quying
ms.lastreviewed: 10/25/2018
ms.openlocfilehash: eccbd502e1fcd50524f7627abf0c7cb675672a98
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2019
ms.locfileid: "56182277"
---
# <a name="use-sql-databases-on-microsoft-azure-stack"></a>Utilizar bases de dados SQL no Microsoft Azure Stack

Utilizar o fornecedor de recursos do SQL Server para oferecer bases de dados SQL como um serviço de [do Azure Stack](azure-stack-poc.md). Depois de instalar o fornecedor de recursos e ligá-la a uma ou mais instâncias do SQL Server, e seus usuários podem criar:

- Bases de dados para aplicações nativas da cloud.
- Web sites que utilizam o SQL.
- Cargas de trabalho que usam SQL.

O fornecedor de recursos não fornece todas as bases de dados das capacidades de gestão do [base de dados do Azure SQL](https://azure.microsoft.com/services/sql-database/). Por exemplo, os conjuntos elásticos que atribuem automaticamente os recursos não são suportados. No entanto, o suporte de fornecedor de recursos semelhantes cria, ler, atualizar e eliminar operações de (CRUD) num banco de dados do SQL Server. 

## <a name="sql-resource-provider-adapter-architecture"></a>Arquitetura de adaptador do fornecedor de recursos do SQL

O fornecedor de recursos é composta pelos seguintes componentes:

- **A máquina no SQL recursos do fornecedor adaptador virtual (VM)**, que é uma VM do Windows Server que executa os serviços do fornecedor.
- **O fornecedor de recursos**, que processa os pedidos e recursos da base de dados de acessos.
- **Servidores que hospedam o SQL Server**, que proporcionam a capacidade para bases de dados chamado servidores de alojamento.

Tem de criar pelo menos uma instância do SQL Server ou fornecer acesso a instâncias externas do SQL Server.

> [!NOTE]
> Alojar servidores que estão instalados no Azure Stack sistemas integrados tem de ser criados de uma subscrição de inquilino. Eles não não possível criar a partir da subscrição do fornecedor predefinido. Eles devem ser criados no portal de inquilinos do ou com o PowerShell com o início de sessão-in adequado. Todos os servidores de alojamento são a cobrar as máquinas de virtuais e tem de ter licenças. O administrador de serviços pode ser o proprietário da subscrição do inquilino.

## <a name="next-steps"></a>Passos Seguintes

[Deploy the SQL Server resource provider](azure-stack-sql-resource-provider-deploy.md) (Implementar o fornecedor de recursos do SQL Server)
