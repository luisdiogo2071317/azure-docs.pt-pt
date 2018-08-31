---
title: Colocar em pausa, retomar, dimensionar com REST no Azure SQL Data Warehouse | Documentos da Microsoft
description: Gerir o poder de computação no SQL Data Warehouse através de REST APIs.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/17/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 8db4d5cb69b65e60cd77d85d743798168bc6d813
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/30/2018
ms.locfileid: "43300838"
---
# <a name="rest-apis-for-azure-sql-data-warehouse"></a>APIs REST para o armazém de dados SQL do Azure
APIs REST para gerir a computação do Azure SQL Data Warehouse.

## <a name="scale-compute"></a>Dimensionar computação
Para alterar as unidades de armazém de dados, utilize o [criar ou atualizar banco de dados](/rest/api/sql/databases/createorupdate) REST API. O exemplo seguinte define as unidades de armazém de dados para DW1000 MySQLDW, que está alojado no servidor MyServer da base de dados. O servidor está num grupo de recursos do Azure com o nome ResourceGroup1.

```
PATCH https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}?api-version=2014-04-01-preview HTTP/1.1
Content-Type: application/json; charset=UTF-8

{
    "properties": {
        "requestedServiceObjectiveName": DW1000
    }
}
```

## <a name="pause-compute"></a>Computação em pausa

Para colocar em pausa uma base de dados, utilize o [colocar em pausa a base de dados](/rest/api/sql/databases/pause) REST API. O exemplo seguinte coloca em pausa uma base de dados com o nome Database02 alojada num servidor com o nome servidor01. O servidor está num grupo de recursos do Azure com o nome ResourceGroup1.

```
POST https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}/pause?api-version=2014-04-01-preview HTTP/1.1
```

## <a name="resume-compute"></a>Computação de retomar

Para iniciar uma base de dados, utilize o [base de dados retomar](/rest/api/sql/databases/resume) REST API. O exemplo seguinte inicia um banco de dados com o nome Database02 alojada num servidor com o nome servidor01. O servidor está num grupo de recursos do Azure com o nome ResourceGroup1. 

```
POST https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}/resume?api-version=2014-04-01-preview HTTP/1.1
```

## <a name="check-database-state"></a>Verificar o estado da base de dados

```
GET https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}?api-version=2014-04-01 HTTP/1.1
```


## <a name="next-steps"></a>Passos Seguintes
Para obter mais informações, consulte [gerir a computação](sql-data-warehouse-manage-compute-overview.md).

