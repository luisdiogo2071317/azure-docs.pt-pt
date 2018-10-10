---
title: Parametrizar os serviços ligados no Azure Data Factory | Documentos da Microsoft
description: Saiba como parametrizar serviços ligados no Azure Data Factory e passar valores dinâmicos em tempo de execução.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/09/2018
ms.author: douglasl
ms.openlocfilehash: 287dcdedede5cab575aa0b9a73ec3e122556dc93
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/10/2018
ms.locfileid: "48900728"
---
# <a name="parameterize-linked-services-in-azure-data-factory"></a>Parametrizar os serviços ligados no Azure Data Factory

Agora pode parametrizar um serviço ligado e passar valores dinâmicos em tempo de execução. Por exemplo, se pretender ligar a bases de dados diferentes no mesmo servidor de base de dados do Azure SQL, agora é possível parametrizar o nome de base de dados na definição do serviço ligado. Isso evita a necessidade de criar um serviço ligado para cada base de dados no servidor de base de dados SQL do Azure. É possível parametrizar outras propriedades na definição do serviço ligado, também - por exemplo, *nome de utilizador.*

Pode utilizar a IU do Data Factory no Portal do Azure ou uma interface de programação parametrizar serviços ligados.

> [!TIP]
> Recomendamos que não para parametrizar senhas ou segredos. Store em vez disso, todas as cadeias de ligação no Azure Key Vault e parametrizar os *nome do segredo*.

## <a name="supported-data-stores"></a>Armazena os dados suportados

Neste momento, a parametrização de serviço ligado é suportada na IU do Data Factory no portal do Azure para os seguintes arquivos de dados. Para todos os outros arquivos de dados, é possível parametrizar o serviço ligado ao selecionar o **código** ícone no separador do pipeline e usando o editor de JSON.
- Base de Dados SQL do Azure
- Azure SQL Data Warehouse
- SQL Server
- Oracle
- BD do Cosmos
- Amazon Redshift
- MySQL
- Base de Dados do Azure para MySQL

## <a name="data-factory-ui"></a>IU do Data Factory

![Adicionar conteúdo dinâmico para a definição de serviço ligado](media/parameterize-linked-services/parameterize-linked-services-image1.png)

![Criar um novo parâmetro](media/parameterize-linked-services/parameterize-linked-services-image2.png)

## <a name="json"></a>JSON

```json
{
    "name": "AzureSqlDatabase",
    "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
            "connectionString": {
                "value": "Server=tcp:myserver.database.windows.net,1433;Database=@{linkedService().DBName};User ID=user;Password=fake;Trusted_Connection=False;Encrypt=True;Connection Timeout=30",
                "type": "SecureString"
            }
        },
        "connectVia": null,
        "parameters": {
            "DBName": {
                "type": "String"
            }
        }
    }
}
```
