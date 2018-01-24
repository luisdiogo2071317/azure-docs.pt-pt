---
title: Encriptar as credenciais no Azure Data Factory | Microsoft Docs
description: "Saiba como encriptar e armazenar as credenciais para os arquivos de dados no local num computador com o tempo de execução de integração personalizada alojada."
services: data-factory
documentationcenter: 
author: nabhishek
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: abnarain
ms.openlocfilehash: 0f42d971fcf21d0f719468a8c10ff637f2e523f9
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/23/2018
---
# <a name="encrypt-credentials-for-on-premises-data-stores-in-azure-data-factory"></a>Encriptar as credenciais para os arquivos de dados no local no Azure Data Factory
Pode encriptar e armazenar as credenciais para os arquivos de dados no local (serviços ligados com informações confidenciais) numa máquina com o tempo de execução de integração personalizada alojada. 

Transferir um ficheiro de definição JSON com as credenciais para o <br/>[**Novo AzureRmDataFactoryV2LinkedServiceEncryptedCredential** ](https://docs.microsoft.com/powershell/module/azurerm.datafactoryv2/New-AzureRmDataFactoryV2LinkedServiceEncryptedCredential?view=azurermps-4.4.0) cmdlet para produzir um ficheiro de definição de JSON de saída com as credenciais encriptadas. Em seguida, utilize a definição atualizada de JSON para criar os serviços ligados.

> [!NOTE]
> Este artigo aplica-se à versão 2 do Data Factory, que está atualmente em pré-visualização. Se estiver a utilizar a versão 1 do serviço Data Factory, que está disponível em geral (GA), veja [Data Factory version 1 documentation](v1/data-factory-introduction.md) (Documentação da versão 1 do Data Factory).

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

