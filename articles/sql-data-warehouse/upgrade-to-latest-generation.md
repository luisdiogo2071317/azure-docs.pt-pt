---
title: Atualizar para a geração mais recente do Azure SQL Data Warehouse | Documentos da Microsoft
description: Atualize o Azure SQL Data Warehouse para a última geração da arquitetura de hardware e o armazenamento do Azure.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 08/22/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: fe1f2e026aaa4260d34b9b1cb96064053af1c3c7
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/12/2018
ms.locfileid: "51568017"
---
# <a name="optimize-performance-by-upgrading-sql-data-warehouse"></a>Otimizar o desempenho ao atualizar o SQL Data Warehouse
Atualize o Azure SQL Data Warehouse para a última geração da arquitetura de hardware e o armazenamento do Azure.

## <a name="why-upgrade"></a>Por que atualizar?
Agora pode facilmente atualizar para o escalão de SQL Data Warehouse de computação otimizada geração 2 no portal do Azure. Se tiver um armazém de dados de escalão de computação otimizada geração 1, a atualização é recomendada. Com a atualização, pode utilizar a última geração de hardware do Azure e aprimorada arquitetura de armazenamento. Pode tirar partido de um desempenho mais rápido, escalabilidade e armazenamento em colunas ilimitado. 

> [!VIDEO https://www.youtube.com/embed/9B2F0gLoyss]

## <a name="applies-to"></a>Aplica-se a
Esta atualização aplica-se para armazéns de dados de escalão de computação otimizada geração 1.

## <a name="sign-in-to-the-azure-portal"></a>Iniciar sessão no portal do Azure

Inicie sessão no [portal do Azure](https://portal.azure.com/).

## <a name="before-you-begin"></a>Antes de começar
> [!NOTE]
> Se o seu armazém de dados de escalão de computação otimizada geração 1 existente não está a ser uma região onde está disponível o escalão de computação otimizada geração 2, pode [georrestauro](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-restore-database-powershell#restore-from-an-azure-geographical-region) através do PowerShell para uma região suportada.
> 
>

1. Se o armazém de dados de escalão de computação otimizada geração 1 a ser atualizado está em pausa, [retomar o armazém de dados](pause-and-resume-compute-portal.md).
2. Esteja preparado para alguns minutos de tempo de inatividade. 



## <a name="start-the-upgrade"></a>Iniciar a atualização

1. Vá para a computação otimizada geração 1 no portal do Azure do armazém de dados de escalão e clique nas **atualizar para ger2** cartão no separador tarefas: ![Upgrade_1](./media/sql-data-warehouse-upgrade-to-latest-generation/Upgrade_to_Gen2_1.png)
    
> [!NOTE]
> Se não vir a **atualizar para ger2** cartão no separador tarefas, o tipo de subscrição é limitado na região atual. [Submeter um pedido de suporte](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-get-started-create-support-ticket) para obter a sua subscrição na lista de permissões.

2. Por predefinição, **selecione o nível de desempenho sugeridos** para o armazém de dados com base no seu nível de desempenho atual na camada de computação otimizada geração 1 com o mapeamento abaixo:
    
   | Computação otimizada geração 1 camada | Computação otimizada geração 2 escalão |
   | :----------------------: | :-------------------: |
   |      DW100 – DW600       |        DW500c         |
   |          DW1000          |        DW1000c        |
   |          DW1200          |        DW1500c        |
   |          DW1500          |        DW1500c        |
   |          DW2000          |        DW2000c        |
   |          DW3000          |        DW3000c        |
   |          DW6000          |        DW6000c        |

3. Certifique-se de que sua carga de trabalho foi concluída em execução e terem silenciado as antes de atualizar. Terá um período de indisponibilidade durante alguns minutos antes do armazém de dados estiver novamente online como um armazém de dados de escalão de computação otimizada geração 2. **Clique em Atualizar**. O preço do escalão de desempenho do escalão de computação otimizada geração 2 está atualmente metade-desativada durante o período de pré-visualização:
    
   ![Upgrade_2](./media/sql-data-warehouse-upgrade-to-latest-generation/Upgrade_to_Gen2_2.png)

4. **Monitorizar a sua atualização** ao verificar o estado no portal do Azure:

   ![Upgrade3](./media/sql-data-warehouse-upgrade-to-latest-generation/Upgrade_to_Gen2_3.png)
   
   A primeira etapa do processo de atualização atravessa a operação de dimensionamento ("atualizando - Offline") em que serão eliminadas todas as sessões e ligações serão ignoradas. 
   
   A segunda etapa do processo de atualização é a migração de dados ("atualizando - Online"). Migração de dados é um processo de plano de fundo online trickle, que se move lentamente para armazenamento de dados da arquitetura do armazenamento antigo para a nova arquitetura de armazenamento tirar partido de uma cache SSD local. Durante este período, o seu armazém de dados estarão online para consultar e carregar. Todos os seus dados vão estar disponíveis para consultar, independentemente de se ter sido migrada ou não. A migração de dados acontece a uma taxa variada, dependendo do tamanho dos dados, o nível de desempenho e o número de segmentos de columnstore. 

5. **Recomendação opcional:** para agilizar o processo de segundo plano de migração de dados, pode forçar imediatamente movimento de dados através da execução [recompilação de índice Alter](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-index) em todas as tabelas de columnstore primário poderia consultar num SLO maior e a classe de recursos. Esta operação é **offline** em comparação com o processo em segundo plano trickle que pode demorar horas a concluir consoante o número e os tamanhos de suas tabelas; no entanto, migração de dados será muito mais rápido em que o que, em seguida, pode tirar total partido da nova arquitetura de armazenamento avançado depois de concluído com grupos de linhas de alta qualidade. 

A seguinte consulta gera os comandos necessários Alter Index Rebuild para agilizar o processo de migração de dados:

```sql
SELECT 'ALTER INDEX [' + idx.NAME + '] ON [' 
       + Schema_name(tbl.schema_id) + '].[' 
       + Object_name(idx.object_id) + '] REBUILD ' + ( CASE 
                                                         WHEN ( 
                                                     (SELECT Count(*) 
                                                      FROM   sys.partitions 
                                                             part2 
                                                      WHERE  part2.index_id 
                                                             = idx.index_id 
                                                             AND 
                                                     idx.object_id = 
                                                     part2.object_id) 
                                                     > 1 ) THEN 
              ' PARTITION = ' 
              + Cast(part.partition_number AS NVARCHAR(256)) 
              ELSE '' 
                                                       END ) + '; SELECT ''[' + 
              idx.NAME + '] ON [' + Schema_name(tbl.schema_id) + '].[' + 
              Object_name(idx.object_id) + '] ' + ( 
              CASE 
                WHEN ( (SELECT Count(*) 
                        FROM   sys.partitions 
                               part2 
                        WHERE 
                     part2.index_id = 
                     idx.index_id 
                     AND idx.object_id 
                         = part2.object_id) > 1 ) THEN 
              ' PARTITION = ' 
              + Cast(part.partition_number AS NVARCHAR(256)) 
              + ' completed'';' 
              ELSE ' completed'';' 
                                                    END ) 
FROM   sys.indexes idx 
       INNER JOIN sys.tables tbl 
               ON idx.object_id = tbl.object_id 
       LEFT OUTER JOIN sys.partitions part 
                    ON idx.index_id = part.index_id 
                       AND idx.object_id = part.object_id 
WHERE  idx.type_desc = 'CLUSTERED COLUMNSTORE'; 
```



## <a name="next-steps"></a>Passos Seguintes
O armazém de dados atualizada está online. Para tirar partido da arquitetura avançada, consulte [classes de recursos para a gestão da carga de trabalho](resource-classes-for-workload-management.md).
 
