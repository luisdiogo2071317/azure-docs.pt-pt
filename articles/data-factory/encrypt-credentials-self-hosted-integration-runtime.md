---
title: Encriptar as credenciais no Azure Data Factory | Documentos da Microsoft
description: Saiba como criptografar e armazenar as credenciais para os arquivos de dados no local num computador com o integration runtime autoalojado.
services: data-factory
documentationcenter: ''
author: nabhishek
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/15/2018
ms.author: abnarain
ms.openlocfilehash: 8e8a4cabd948783278981c61fa718e51b679ad72
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/04/2019
ms.locfileid: "54014170"
---
# <a name="encrypt-credentials-for-on-premises-data-stores-in-azure-data-factory"></a>Encriptar as credenciais para arquivos de dados no local no Azure Data Factory
Pode criptografar e armazenar credenciais de seus arquivos de dados no local (serviços ligados com informações confidenciais) numa máquina com o integration runtime autoalojado. 

Passa um ficheiro de definição de JSON com as credenciais para o <br/>[**Novo-AzureRmDataFactoryV2LinkedServiceEncryptedCredential** ](https://docs.microsoft.com/powershell/module/azurerm.datafactoryv2/New-AzureRmDataFactoryV2LinkedServiceEncryptedCredential?view=azurermps-4.4.0) cmdlet para produzir um ficheiro de definição de JSON de saída com as credenciais encriptadas. Em seguida, utilize a definição de JSON atualizada para criar serviços ligados.

## <a name="author-sql-server-linked-service"></a>Serviço ligado do SQL Server de autor
Crie um ficheiro JSON com o nome **Sqlserverlinkedservice** em qualquer pasta com o seguinte conteúdo:  

Substitua `<servername>`, `<databasename>`, `<username>`, e `<password>` com valores para o servidor de SQL antes de guardar o ficheiro. E, substitua `<integration runtime name>` com o nome do seu integration runtime. 

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
Para encriptar os dados confidenciais do payload JSON num runtime de integração autoalojado no local, execute **New-AzureRmDataFactoryV2LinkedServiceEncryptedCredential**e transmitir o payload JSON. Este cmdlet garante que as credenciais são encriptadas com a interface DPAPI e armazenadas no nó do runtime de integração autoalojado localmente. O payload de saída pode ser redirecionado para outro ficheiro JSON (neste caso, "Encryptedlinkedservice"), que contém credenciais encriptadas.

```powershell
New-AzureRmDataFactoryV2LinkedServiceEncryptedCredential -DataFactoryName $dataFactoryName -ResourceGroupName $ResourceGroupName -Name "SqlServerLinkedService" -DefinitionFile ".\SQLServerLinkedService.json" > encryptedSQLServerLinkedService.json
```

## <a name="use-the-json-with-encrypted-credentials"></a>Utilizar o JSON com as credenciais encriptadas
Agora, utilize o ficheiro JSON de saída do comando anterior, que contém a credencial encriptada para configurar o **SqlServerLinkedService**.

```powershell
Set-AzureRmDataFactoryV2LinkedService -DataFactoryName $dataFactoryName -ResourceGroupName $ResourceGroupName -Name "EncryptedSqlServerLinkedService" -DefinitionFile ".\encryptedSqlServerLinkedService.json" 
```

## <a name="next-steps"></a>Passos Seguintes
Para obter informações sobre considerações de segurança para movimento de dados, consulte [considerações de segurança de movimento de dados](data-movement-security-considerations.md).

