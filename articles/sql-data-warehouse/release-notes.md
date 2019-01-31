---
title: Notas de versão do armazém de dados SQL do Azure | Documentos da Microsoft
description: Notas de versão do Azure SQL Data Warehouse.
services: sql-data-warehouse
author: twounder
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 11/11/2018
ms.author: twounder
ms.reviewer: twounder
ms.openlocfilehash: 7a131429d8f70255e47efaac0c4b5fa99a6df7d6
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/31/2019
ms.locfileid: "55460816"
---
# <a name="azure-sql-data-warehouse-release-notes"></a>Notas de versão do armazém de dados SQL do Azure

O Azure SQL Data Warehouse (SQL DW) é um com base na cloud Enterprise Data Warehouse que tira partido Massivamente paralela de processamento (MPP) para executar rapidamente consultas complexas em petabytes de dados. Utilize o SQL Data Warehouse como um componente fundamental de uma solução de macrodados. Importe macrodados para o SQL Data Warehouse com consultas simples de PolyBase T-SQL e, em seguida, utilize o poder do MPP para executar análises de elevado desempenho. À medida que integra e analisa, o armazém de dados tornar-se-á a única versão real com que a sua empresa pode contar para obter informações.

Clique nos links abaixo para obter mais informações sobre os novos recursos e aprimoramentos que pode esperar na versão mais recente do Azure SQL Data Warehouse. Pode esperar receber estas atualizações de serviço durante a sua agenda de manutenção identificados.

- [Versão do SQL DW 10.0.10106.0](./release-notes-10-0-10106-0.md)
- [Dezembro de 2018](./release-notes-december-2018.md)
- [Outubro de 2018](./release-notes-october-2018.md)
- [Setembro de 2018](./release-notes-september-2018.md)
- [Agosto de 2018](./release-notes-august-2018.md)
- [Julho de 2018](./release-notes-july-2018.md)
- [Junho de 2018](./release-notes-june-2018.md)
- [Maio de 2018](./release-notes-may-2018.md)

## <a name="checking-the-code-version-that-has-been-applied-to-your-data-warehouse"></a>A verificar a versão de código que foi aplicada ao seu armazém de dados

Para confirmar que versão foi como foi aplicada ao seu armazém de dados. Ligar ao seu armazém de dados através do SSMS e execute a seguinte sintaxe para devolver a versão atual do SQL Data Warehouse.

```sql
SELECT @@VERSION AS 'SQL Data Warehouse';
```

Exemplo de saída: ![Versão do SQL Data Warehouse](./media/release-notes/dw-version.png)

Utilize a versão identificada para confirmar que versão foi aplicada ao seu armazém de dados SQL do Azure. 


## <a name="next-steps"></a>Passos Seguintes
- [Saiba mais](https://docs.microsoft.com/azure/sql-data-warehouse/viewing-maintenance-schedule) sobre a visualização de um agendamento de manutenção. 
- [Saiba mais](https://docs.microsoft.com/azure/sql-data-warehouse/changing-maintenance-schedule) sobre como alterar uma agenda de manutenção.
- [Saiba mais](https://docs.microsoft.com/azure/monitoring-and-diagnostics/alert-metric) sobre como criar, visualizar e gerir alertas com o Azure Monitor.
- [Saiba mais](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-log-webhook) sobre as ações de webhook para regras de alerta de registo.
- [Saiba mais](https://docs.microsoft.com/azure/service-health/service-health-overview) sobre o Azure Service Health
