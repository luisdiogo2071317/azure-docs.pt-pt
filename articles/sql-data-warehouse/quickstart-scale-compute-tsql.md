---
title: "Início rápido: Aumentar horizontalmente computação no Azure SQL Data Warehouse - T-SQL | Microsoft Docs"
description: "Comandos de T-SQL para dimensionar recursos de computação ao ajustar as DWUs."
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: jhubbard
editor: 
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 01/31/2018
ms.author: elbutter;barbkess
ms.openlocfilehash: 9d6ecd53fc034fd7014b17c98f1c5a99088723fe
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/03/2018
---
# <a name="quickstart-scale-compute-in-azure-sql-data-warehouse-using-t-sql"></a>Início rápido: Dimensionar a computação no Azure SQL Data Warehouse com T-SQL

Dimensionar a computação no Azure SQL Data Warehouse com T-SQL e SQL Server Management Studio (SSMS). Aumentar horizontalmente computação para um melhor desempenho ou a escala de computação novamente para reduzir os custos. 

Se não tiver uma subscrição do Azure, crie uma conta [gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="before-you-begin"></a>Antes de começar

Transfira e instale a versão mais recente do [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms.md) (SSMS).

Isto pressupõe que concluiu [início rápido: criar e ligar - portal](create-data-warehouse-portal.md). Depois de concluir o guia de introdução de criar e ligar sabe como ligar à: criar um armazém de dados com o nome **mySampleDataWarehouse**, criou uma regra de firewall que permite que os nossos clientes aceder ao servidor, instalado.
 
## <a name="create-a-data-warehouse"></a>Criar um armazém de dados

Utilize [início rápido: criar e ligar - portal](create-data-warehouse-portal.md) para criar um armazém de dados com o nome **mySampleDataWarehouse**. Conclua o guia de introdução para se certificar de que tem uma regra de firewall e pode ligar ao seu armazém de dados a partir do SQL Server Management Studio.

## <a name="connect-to-the-server-as-server-admin"></a>Ligar ao servidor como administrador do servidor

Esta secção utiliza o [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms.md) (SSMS) para estabelecer uma ligação ao servidor SQL do Azure.

1. Abra o SQL Server Management Studio.

2. Na caixa de dialogo **Ligar ao Servidor**, introduza as seguintes informações:

   | Definição       | Valor sugerido | Descrição | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | Tipo de servidor | Motor de base de dados | Este valor é obrigatório |
   | Nome do servidor | O nome de servidor completamente qualificado | Segue-se um exemplo: **mynewserver-20171113.database.windows.net**. |
   | Autenticação | Autenticação do SQL Server | A Autenticação do SQL é o único tipo de autenticação configurado neste tutorial. |
   | Iniciar sessão | A conta de administrador do servidor | Esta é a conta que especificou quando criou o servidor. |
   | Palavra-passe | A palavra-passe da sua conta de administrador do servidor | Esta é a palavra-passe que especificou quando criou o servidor. |

    ![ligar ao servidor](media/load-data-from-azure-blob-storage-using-polybase/connect-to-server.png)

4. Clique em **Ligar**. A janela do Object Explorer é aberta no SSMS. 

5. No Object Explorer, expanda **Databases**. Em seguida, expanda **mySampleDatabase** para ver os objetos na nova base de dados.

    ![objetos da base de dados](media/create-data-warehouse-portal/connected.png) 

## <a name="view-service-objective"></a>Objetivo de serviço de vista
A definição de objetivo de serviço contém o número de unidades de armazém de dados para o armazém de dados. 

Para ver as unidades de armazém de dados atual para o seu armazém de dados:

1. Sob a ligação ao **mynewserver 20171113.database.windows.net**, expanda **bases de dados do sistema**.
2. Clique com botão direito **mestre** e selecione **nova consulta**. É aberta uma nova janela de consulta.
3. Execute a seguinte consulta para selecionar a vista de gestão dinâmica sys.database_service_objectives. 

    ```sql
    SELECT
        db.name [Database]
    ,   ds.edition [Edition]
    ,   ds.service_objective [Service Objective]
    FROM
        sys.database_service_objectives ds
    JOIN
        sys.databases db ON ds.database_id = db.database_id
    WHERE 
        db.name = 'mySampleDataWarehouse'
    ```

4. O seguinte resulta mostrar **mySampleDataWarehouse** tem um objetivo do serviço de DW400. 

    ![DWUs atuais de vista](media/quickstart-scale-compute-tsql/view-current-dwu.png)


## <a name="scale-compute"></a>Dimensionar a computação
No SQL Data Warehouse, pode aumentar ou diminuir os recursos de computação ao ajustar unidades do data warehouse. O [criar e ligar - portal](create-data-warehouse-portal.md) criado **mySampleDataWarehouse** e inicializado-lo com 400 DWUs. Os seguintes passos ajustar as DWUs para **mySampleDataWarehouse**.

Para alterar as unidades de armazém de dados:

1. Clique com botão direito **mestre** e selecione **nova consulta**.
2. Utilize o [ALTER DATABASE](/sql/t-sql/statements/alter-database-azure-sql-database) instrução de T-SQL para modificar o objetivo de serviço. Execute a consulta seguinte para alterar o objetivo de serviço para DW300. 

```Sql
ALTER DATABASE mySampleDataWarehouse
MODIFY (SERVICE_OBJECTIVE = 'DW300')
;
```

## <a name="check-database-state"></a>Verificar o estado da base de dados

Para verificar o estado da base de dados, execute a seguinte consulta contra o **mestre** base de dados.

```sql
SELECT name AS "Database Name", state_desc AS "Status" 
FROM sys.databases db
WHERE db.name = 'mySampleDataWarehouse'
;
```

Quando executar este comando, deverá receber um valor de estado de Online, Pausing, a retomar, dimensionamento ou em pausa.

## <a name="check-operation-status"></a>Verifique o estado da operação

Para devolver informações sobre várias operações de gestão no seu armazém de dados do SQL Server, execute a seguinte consulta [operation_status](/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database) DMV. Por exemplo, devolve a operação e o estado da operação, o que irá ser IN_PROGRESS ou foi concluída.

```sql
SELECT *
FROM
    sys.dm_operation_status
WHERE
    resource_type_desc = 'Database'
AND 
    major_resource_id = 'MySQLDW'
```


## <a name="next-steps"></a>Passos Seguintes
Agora que tem aprendeu como dimensionar a computação para o seu armazém de dados. Para saber mais sobre o Azure SQL Data Warehouse, avance para o tutorial para carregar dados.

> [!div class="nextstepaction"]
>[Carregar dados para o SQL Data Warehouse](load-data-from-azure-blob-storage-using-polybase.md)
