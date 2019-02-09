---
title: Cópia do delta da base de dados com a tabela de controle com o Azure Data Factory | Documentos da Microsoft
description: Saiba como utilizar um modelo de solução para copiar de forma incremental linhas novas ou atualizadas apenas a partir de uma base de dados com o Azure Data Factory.
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
ms.date: 12/24/2018
ms.openlocfilehash: 23e1255013cd5e52166fe0e59a8931dd9ecd81a0
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/08/2019
ms.locfileid: "55967496"
---
# <a name="delta-copy-from-database-with-control-table"></a>Cópia do delta da base de dados com a tabela de controle

Quando quiser carregar de forma incremental as alterações (linhas novas ou atualizadas) apenas a partir de uma tabela numa base de dados para o Azure com uma tabela de controlo externo armazenar o valor de limite superior de tamanho.  O modelo presente foi concebido para esse caso. 

Este modelo requer que o esquema da base de dados de origem tem que contêm timestamp coluna ou incrementando chave para identificar as linhas novas ou atualizadas.

Se tem a coluna timestamp na sua base de dados de origem para identificar as linhas novas ou atualizadas, mas não pretender criar uma tabela de controlo externo para ativar a cópia do delta, pode utilizar a ferramenta copiar dados para obter um pipeline que utiliza um período de tempo de Acionador-agendado como uma variável para ler as novas linhas apenas a partir da base de dados de origem.

## <a name="about-this-solution-template"></a>Sobre este modelo de solução

Este modelo sempre obtém o valor de marca d'água antigo primeiro e, em seguida, compara-o com o valor de limite de tamanho atual. Depois disso, ele apenas copia as alterações da base de dados de origem com base na comparação entre 2 valores de marca d'água.  Quando terminar, ele armazena o novo valor de limite superior de tamanho para uma tabela de controlo externo para o próximo tempo de carregamento de dados delta.

O modelo contém quatro atividades:
-   R **Lookup** atividade para obter o valor de limite superior de tamanho antigo armazenado numa tabela do controlo externo.
-   R **Lookup** atividade para obter o valor de limite superior de tamanho atual da base de dados de origem.
-   R **cópia** atividade para copiar as alterações apenas a partir da base de dados de origem para o arquivo de destino. A consulta usada para identificar as alterações da base de dados de origem na atividade de cópia é semelhante a como "SELECIONAR * de Data_Source_Table onde TIMESTAMP_Column >"último limite superior de tamanho"e TIMESTAMP_Column < ="atual limite superior de tamanho"'.
-   R **SqlServerStoredProcedure** atividade escrever o valor de limite superior de tamanho atual para uma tabela de controlo externo para as cópias delta próxima vez.

O modelo define cinco parâmetros:
-   O parâmetro *Data_Source_Table_Name* é o nome da tabela da base de dados de origem para carregar dados a partir de onde pretende.
-   O parâmetro *Data_Source_WaterMarkColumn* é o nome de coluna na tabela de origem que pode ser utilizado para identificar as linhas novas ou atualizadas. Normalmente, o tipo desta coluna pode ser datetime ou INT etc.
-   O parâmetro *Data_Destination_Folder_Path* ou *Data_Destination_Table_Name* é o local onde os dados são copiados para o arquivo de destino.
-   O parâmetro *Control_Table_Table_Name* é o nome da tabela de controlo externo para armazenar o valor de limite superior de tamanho.
-   O parâmetro *Control_Table_Column_Name* é o nome da coluna da tabela de controlo externo para armazenar o valor de limite superior de tamanho.

## <a name="how-to-use-this-solution-template"></a>Como utilizar este modelo de solução

1. Explore a tabela de origem que pretende carregar e definir a coluna de limite superior de tamanho que pode ser utilizada para dividir as linhas novas ou atualizadas. Normalmente, o tipo desta coluna pode ser datetime ou etc. INT e os respetivos dados mantendo aumentar quando novas linhas que está a ser adicionado.  Da tabela de origem de exemplo (nome da tabela: data_source_table) abaixo, pode utilizar a coluna *LastModifytime* como a coluna de limite superior de tamanho.

    ```sql
            PersonID    Name    LastModifytime
            1   aaaa    2017-09-01 00:56:00.000
            2   bbbb    2017-09-02 05:23:00.000
            3   cccc    2017-09-03 02:36:00.000
            4   dddd    2017-09-04 03:21:00.000
            5   eeee    2017-09-05 08:06:00.000
            6   fffffff 2017-09-06 02:23:00.000
            7   gggg    2017-09-07 09:01:00.000
            8   hhhh    2017-09-08 09:01:00.000
            9   iiiiiiiii   2017-09-09 09:01:00.000
    ```
    
2. Crie uma tabela de controle num SQL server ou do SQL Azure para armazenar o valor de limite superior de tamanho de carregamento de dados delta. O exemplo a seguir, pode ver é o nome da tabela de controle *watermarktable*. Dentro da mesma, é o nome da coluna para armazenar o valor de limite superior de tamanho *WatermarkValue* e seu tipo é *datetime*.

    ```sql
            create table watermarktable
            (
            WatermarkValue datetime,
            );
            INSERT INTO watermarktable
            VALUES ('1/1/2010 12:00:00 AM')
    ```
    
3. Criar um procedimento armazenado no mesmo SQL server ou do SQL Azure utilizada para criar a tabela de controle. O procedimento armazenado é utilizado para escrever o novo valor de limite superior de tamanho para a tabela de controlo externo para o próximo tempo de carregamento de dados delta.

    ```sql
            CREATE PROCEDURE update_watermark @LastModifiedtime datetime
            AS

            BEGIN

                UPDATE watermarktable
                SET [WatermarkValue] = @LastModifiedtime 

            END
    ```
    
4. Aceda ao modelo **cópias Delta da base de dados**e criar um **nova ligação** sua base de dados de origem onde os dados se copiar do.

    ![Criar uma nova ligação para a tabela de origem](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable4.png)

5. Criar uma **nova ligação** ao seu arquivo de dados de destino onde os dados se copiar para.

    ![Criar uma nova ligação para a tabela de destino](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable5.png)

6. Criar uma **nova ligação** à sua tabela de controlo externo e um procedimento armazenado.  Está a ligar à base de dados em que tinha criou a tabela de controle e procedimento armazenado no passo #2 e 3 de #.

    ![Criar uma nova ligação para o arquivo de dados de tabela de controle](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable6.png)

7. Clique em **Utilize este modelo**.

     ![Utilizar este modelo](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable7.png)
    
8. Verá o pipeline disponível no painel, conforme mostrado no exemplo a seguir:

     ![Pipeline de revisão](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable8.png)

9. Clique em atividade de procedimento armazenado, selecione **nome do procedimento armazenado**, clique em **importar parâmetro** e clique em **adicionar conteúdo dinâmico**.  

     ![Atividade de procedimento armazenado de conjunto](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable9.png) 

10. Escrever o conteúdo **@{activity('LookupCurrentWaterMark').output.firstRow.NewWatermarkValue}** e clique em **concluir**.  

     ![Escrever o conteúdo para o parâmetro para o procedimento armazenado](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable10.png)      
     
11. Clique em **depurar**, os parâmetros de entrada e clique em **concluir**.

    ![Clique em Debug](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable11.png)

12. Veja o resultado disponível no painel, conforme mostrado no exemplo a seguir:

    ![O resultado da revisão](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable12.png)

13. Pode criar novas linhas na tabela de origem.  O sql de exemplo para criar novas linhas pode ser seguinte:

    ```sql
            INSERT INTO data_source_table
            VALUES (10, 'newdata','9/10/2017 2:23:00 AM')

            INSERT INTO data_source_table
            VALUES (11, 'newdata','9/11/2017 9:01:00 AM')
    ```
13. Execute o pipeline novamente, clicando em **depurar**, os parâmetros de entrada e clique em **concluir**.

    ![Clique em Debug](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable11.png)

14. Verá apenas novas linhas foram copiadas para o destino.

15. (Opcional) Se selecionar o SQL Data Warehouse como destino de dados, terá também de entrada a ligação de um armazenamento de Blobs do Azure como um teste, o que é exigida pelo Polybase do SQL Data Warehouse.  Certifique-se de que o contentor no armazenamento de BLOBs já criado.  
    
    ![Configurar o Polybase](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable15.png)
    
## <a name="next-steps"></a>Passos Seguintes

- [Introdução ao Azure Data Factory](introduction.md)