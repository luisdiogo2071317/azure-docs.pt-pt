---
title: Encriptar as credenciais no Azure Data Factory | Microsoft Docs
description: Saiba como encriptar e armazenar as credenciais para os arquivos de dados no local num computador com o tempo de execução de integração personalizada alojada.
services: data-factory
documentationcenter: ''
author: nabhishek
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/15/2018
ms.author: abnarain
ms.openlocfilehash: b577c276627c3a187215cd0da551428fbb32791f
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37050911"
---
# <a name="encrypt-credentials-for-on-premises-data-stores-in-azure-data-factory"></a>Encriptar as credenciais para os arquivos de dados no local no Azure Data Factory
Pode encriptar e armazenar as credenciais para os arquivos de dados no local (serviços ligados com informações confidenciais) numa máquina com o tempo de execução de integração personalizada alojada. 

Transferir um ficheiro de definição JSON com as credenciais para o <br/>[**Novo AzureRmDataFactoryV2LinkedServiceEncryptedCredential** ](https://docs.microsoft.com/powershell/module/azurerm.datafactoryv2/New-AzureRmDataFactoryV2LinkedServiceEncryptedCredential?view=azurermps-4.4.0) cmdlet para produzir um ficheiro de definição de JSON de saída com as credenciais encriptadas. Em seguida, utilize a definição atualizada de JSON para criar os serviços ligados.

## <a name="author-sql-server-linked-service"></a>Serviço ligado do SQL Server de autor
Crie um ficheiro JSON com o nome **SqlServerLinkedService.json** em qualquer pasta com o seguinte conteúdo:  

Substitua `<servername>`, `<databasename>`, `<username>`, e `<password>` com os valores para o SQL Server antes de guardar o ficheiro. E, substitua `<integration runtime name>` com o nome do seu tempo de execução de integração. 

```json
{
    "properties": {
        "type": "SqlServer",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Server=<servername>;Database=<databasename>;User ID=<username>;Password=<password>;Timeout=60"
            }
        },
        "connectVia": {
            "type": "integrationRuntimeReference",
            "referenceName": "<integration runtime name>"
        },
        "name": "SqlServerLinkedService"
    }
}
```

## <a name="encrypt-credentials"></a>Encriptar credenciais
Para encriptar os dados confidenciais do payload JSON num tempo de execução de integração personalizada alojada no local, execute **New-AzureRmDataFactoryV2LinkedServiceEncryptedCredential**e transmitir o payload JSON. Este cmdlet assegura que as credenciais são encriptadas utilizando DPAPI e armazenado no nó de tempo de execução da integração personalizada alojada localmente. O payload de saída pode ser redirecionado para outro ficheiro JSON (neste caso, 'encryptedLinkedService.json'), que contém as credenciais encriptadas.

```powershell
New-AzureRmDataFactoryV2LinkedServiceEncryptedCredential -DataFactoryName $dataFactoryName -ResourceGroupName $ResourceGroupName -Name "SqlServerLinkedService" -DefinitionFile ".\SQLServerLinkedService.json" > encryptedSQLServerLinkedService.json
```

## <a name="use-the-json-with-encrypted-credentials"></a>Utilizar o JSON com credenciais encriptado
Agora, utilize o ficheiro de JSON de saída do comando anterior que contém a credencial encriptada para configurar o **SqlServerLinkedService**.

```powershell
Set-AzureRmDataFactoryV2LinkedService -DataFactoryName $dataFactoryName -ResourceGroupName $ResourceGroupName -Name "EncryptedSqlServerLinkedService" -DefinitionFile ".\encryptedSqlServerLinkedService.json" 
```

## <a name="next-steps"></a>Passos Seguintes
Para obter informações sobre considerações de segurança para o movimento de dados, consulte [considerações de segurança de movimento de dados](data-movement-security-considerations.md).

