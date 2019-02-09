---
title: Cópia da base de dados com a tabela de controle com o Azure Data Factory em massa | Documentos da Microsoft
description: Saiba como utilizar um modelo de solução totalmente copiar dados em massa de uma base de dados com uma tabela de controlo externo para armazenar a lista de partições de tabelas de origem com o Azure Data Factory.
services: data-factory
documentationcenter: ''
author: dearandyxu
ms.author: yexu
ms.reviewer: douglasl
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 12/14/2018
ms.openlocfilehash: b267da18f2537e462ecda0ac265eac07a069c293
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/08/2019
ms.locfileid: "55967562"
---
# <a name="bulk-copy-from-database-with-control-table"></a>Cópia em massa da base de dados com a tabela de controle

Quando deseja copiar dados do seu armazém de dados como servidor Oracle, Netezza server, servidor de Teradata ou SQL Server para o Azure, terá de carregar a enorme quantidade de dados de várias tabelas numa origens de dados. Na maioria dos casos, os dados devem ser ainda mais particionados em cada tabela para que pode carregar linhas com vários threads em paralelo da tabela única. O modelo presente foi concebido para esse caso. 

Se pretender copiar dados de um pequeno número de tabelas com o pequeno tamanho dos dados, é mais eficiente para que aceda a "ferramenta copiar dados" ter um único copiar atividade ou atividade foreach + atividade de cópia no seu pipeline. Este modelo é mais do que precisa para este simples caso de utilização.

## <a name="about-this-solution-template"></a>Sobre este modelo de solução

Este modelo obtém a lista de partições da base de dados de origem a partir de uma tabela de controlo externo que precisa ser copiadas para o arquivo de destino, e, em seguida, itera em cada partição na base de dados de origem e executa a operação de cópia de dados.

O modelo contém três atividades:
-   R **Lookup** atividade para obter a lista de partições da base de dados de origem a partir de uma tabela de controlo externo.
-   R **ForEach** atividade para obter a lista de partição de atividade de pesquisa e, em seguida, itera cada um deles para a atividade de cópia.
-   R **cópia** atividade para copiar cada partição de armazenamento de banco de dados de origem para o arquivo de destino.

O modelo define cinco parâmetros:
-   O parâmetro *Control_Table_Name* é o nome da tabela para a sua tabela de controlo externo. A tabela de controle é utilizada para armazenar a lista de partição da base de dados de origem.
-   O parâmetro *Control_Table_Schema_PartitionID* é o nome de coluna na tabela controlo externo para armazenar cada ID de partição. Certifique-se de que o ID de partição é exclusivo para cada partição na base de dados de origem.
-   O parâmetro *Control_Table_Schema_SourceTableName* é o nome de coluna na tabela controlo externo para armazenar cada nome de tabela da base de dados de origem.
-   O parâmetro *Control_Table_Schema_FilterQuery* é o nome de coluna na tabela controlo externo para armazenar a consulta de filtro para obter os dados de cada partição na base de dados de origem. Por exemplo, se particionada os dados por cada ano, a consulta armazenada em cada linha pode ser semelhante a como "selecionar * da origem de dados onde LastModifytime > = ' 2015-01-01 00:00:00 ' e LastModifytime < = ' 2015-12-31 23:59:59.999 ' '
-   O parâmetro *Data_Destination_Folder_Path* é o caminho da pasta onde os dados são copiados para o arquivo de destino.  Este parâmetro só é visível quando o destino que escolher é um arquivo de armazenamento baseada em ficheiros.  Se escolher o SQL Data Warehouse como arquivo de destino, não existe nenhum parâmetro deve ser introduzido aqui. Mas os nomes de tabela e o esquema no SQL data warehouse tem de ser o mesmo que as existentes na base de dados de origem.

## <a name="how-to-use-this-solution-template"></a>Como utilizar este modelo de solução

1. Crie uma tabela de controle num SQL server ou do SQL Azure para armazenar a lista de partição da base de dados de origem para cópia em massa.  O exemplo abaixo, pode ver quantos forem os cinco partições na sua base de dados de origem, onde três partições são para uma tabela:*datasource_table* e duas partições são para outra tabela:*project_table*. A coluna *LastModifytime* é utilizado para particionar os dados na tabela *datasource_table* da base de dados de origem. A consulta usada para ler a primeira partição é "selecionar * de datasource_table onde LastModifytime > = ' 2015-01-01 00:00:00 ' e LastModifytime < = ' 2015-12-31 23:59:59.999 ' '.  Também pode ver a consulta semelhante para ler os dados de outras partições. 

     ```sql
            Create table ControlTableForTemplate
            (
            PartitionID int,
            SourceTableName  varchar(255),
            FilterQuery varchar(255)
            );

            INSERT INTO ControlTableForTemplate
            (PartitionID, SourceTableName, FilterQuery)
            VALUES
            (1, 'datasource_table','select * from datasource_table where LastModifytime >= ''2015-01-01 00:00:00'' and LastModifytime <= ''2015-12-31 23:59:59.999'''),
            (2, 'datasource_table','select * from datasource_table where LastModifytime >= ''2016-01-01 00:00:00'' and LastModifytime <= ''2016-12-31 23:59:59.999'''),
            (3, 'datasource_table','select * from datasource_table where LastModifytime >= ''2017-01-01 00:00:00'' and LastModifytime <= ''2017-12-31 23:59:59.999'''),
            (4, 'project_table','select * from project_table where ID >= 0 and ID < 1000'),
            (5, 'project_table','select * from project_table where ID >= 1000 and ID < 2000');
    ```

2. Aceda ao modelo **cópia em massa da base de dados**e criar um **nova ligação** à sua tabela de controlo externo.  Esta ligação é a ligação à base de dados onde criou a tabela de controle no passo #1.

    ![Criar uma nova ligação para a tabela de controle](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable2.png)

3. Criar uma **nova ligação** sua base de dados de origem onde os dados se copiar do.

     ![Criar uma nova ligação para a base de dados de origem](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable3.png)
    
4. Criar uma **nova ligação** ao seu arquivo de dados de destino onde os dados se copiar para.

    ![Criar uma nova ligação para o arquivo de destino](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable4.png)

5. Clique em **Utilize este modelo**.

    ![Utilizar este modelo](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable5.png)
    
6. Verá o pipeline disponível no painel, conforme mostrado no exemplo a seguir:

    ![Reveja o pipeline](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable6.png)

7. Clique em **depurar**, os parâmetros de entrada e, em seguida, clique em **concluir**

    ![Clique em Debug](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable7.png)

8. Veja o resultado disponível no painel, conforme mostrado no exemplo a seguir:

    ![O resultado da revisão](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable8.png)

9. (Opcional) Se selecionar o SQL Data Warehouse como destino de dados, terá também de entrada a ligação de um armazenamento de Blobs do Azure como um teste, o que é exigida pelo Polybase do SQL Data Warehouse.  Certifique-se de que o contentor no armazenamento de BLOBs já criado.  
    
    ![Definição de Polybase](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable9.png)
       
## <a name="next-steps"></a>Passos Seguintes

- [Introdução ao Azure Data Factory](introduction.md)