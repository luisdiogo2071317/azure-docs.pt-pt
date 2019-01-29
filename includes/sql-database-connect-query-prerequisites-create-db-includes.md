---
author: MightyPen
ms.service: sql-database
ms.topic: include
ms.date: 01/28/2019
ms.author: genemi
ms.openlocfilehash: edeaa932abe4d1882f957d0ed26aaddd97dabe3f
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55198066"
---
<!-- sql-database-connect-query-prerequisites-create-db-includes.md -->

- Uma base de dados do Azure SQL colocado no [servidor lógico](https://docs.microsoft.com/azure/sql-database/sql-database-single-index) ou [instância gerida](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-index). Pode utilizar uma destas técnicas para criar uma base de dados:

| Servidor lógico | Instância Gerida |
| --- | --- |
| [Portal](../articles/sql-database/sql-database-get-started-portal.md) | [Portal](../articles/sql-database/sql-database-managed-instance-get-started.md) |
| [CLI](../articles/sql-database/sql-database-get-started-cli.md) | |
| [PowerShell](../articles/sql-database/sql-database-get-started-powershell.md) | |

- **Apenas servidor lógico** -uma regra de firewall de ao nível do servidor configurada que permite-lhe ligar ao seu servidor lógico. Para obter mais informações, consulte [criar regra de firewall ao nível do servidor](../articles/sql-database/sql-database-get-started-portal-firewall.md).
- **Instância gerida depois de apenas** -configurado a ligação ao computador que está a aceder a instância gerida. Pode utilizar as seguintes opções:
  - [Máquina Virtual do Azure](../articles/sql-database/sql-database-managed-instance-configure-vm.md) na mesma VNet do Azure como a instância gerida, que pode aceder à instância.
  - [Ligação ponto a site](../articles/sql-database/sql-database-managed-instance-configure-p2s.md) no seu computador que irá permitir-lhe associar o seu computador para a VNet onde a instância gerida é colocada e utilizar a instância gerida de qualquer outro SQL Server na sua rede.
