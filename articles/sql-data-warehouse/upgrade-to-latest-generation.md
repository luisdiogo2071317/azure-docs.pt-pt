---
title: Atualizar para a geração de mais recente do Azure SQL Data Warehouse | Microsoft Docs
description: Atualize o Azure SQL Data Warehouse para geração mais recente da arquitetura de hardware e de armazenamento do Azure.
services: sql-data-warehouse
author: kevinvngo
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 04/17/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 8ca8156d98932e0e7313375cadc1ace2a3088881
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="optimize-performance-by-upgrading-sql-data-warehouse"></a>Otimizar o desempenho ao atualizar o SQL Data Warehouse
Atualize o Azure SQL Data Warehouse para geração mais recente da arquitetura de hardware e de armazenamento do Azure.

## <a name="why-upgrade"></a>Por que motivo atualizar?
Pode agora totalmente integrada atualizar para o SQL Data Warehouse Gen2 no portal do Azure. Se tiver um armazém de dados Gen1, a atualização é recomendada. Ao atualizar, pode utilizar a geração de mais recente de hardware do Azure e arquitetura de armazenamento melhorada. Pode tirar partido de um desempenho mais rápido, escalabilidade e armazenamento columnar ilimitado. 

## <a name="applies-to"></a>Aplica-se a
Esta atualização aplica-se Gen1 para armazéns de dados.

## <a name="sign-in-to-the-azure-portal"></a>Iniciar sessão no portal do Azure

Inicie sessão no [portal do Azure](https://portal.azure.com/).

## <a name="before-you-begin"></a>Antes de começar
> [!NOTE]
> Se o seu armazém de dados Gen1 existente não está numa região onde Gen2 está disponível, pode [georrestauro para Gen2](https://docs.microsoft.com/en-us/azure/sql-data-warehouse/sql-data-warehouse-restore-database-powershell#restore-from-an-azure-geographical-region) através do PowerShell para uma região suportada.
> 
>

1. Se o armazém de dados Gen1 a ser atualizado está em pausa, [retomar o armazém de dados](pause-and-resume-compute-portal.md).
2. Estar preparadas para alguns minutos de indisponibilidade. 



## <a name="start-the-upgrade"></a>Iniciar a atualização

1. Aceda ao seu Gen1 dados armazém no portal do Azure e clique em **atualizar para Gen2**: ![Upgrade_1](./media/sql-data-warehouse-upgrade-to-latest-generation/Upgrade_to_Gen2_1.png)

2. Por predefinição, **selecione o nível de desempenho sugeridos** para o armazém de dados com base no seu nível de desempenho atual no Gen1 com o mapeamento abaixo:
    
| Gen1 | Gen2 |
| :----------------------: | :-------------------: |
|      DW100 – DW1000      |        DW1000c        |
|          DW1200          |        DW1500c        |
|          DW1500          |        DW1500c        |
|          DW2000          |        DW2000c        |
|          DW3000          |        DW3000c        |
|          DW6000          |        DW6000c        |


3. Certifique-se de que a carga de trabalho foi concluída em execução e de terem silenciado antes de atualizar. Irá ocorrer um período de indisponibilidade para alguns minutos antes do armazém de dados estiver novamente online como um armazém de dados Gen2. **Clique em Atualizar**. O preço de camada de desempenho Gen2 está atualmente half-desligado durante o período de pré-visualização:
    
    ![Upgrade_2](./media/sql-data-warehouse-upgrade-to-latest-generation/Upgrade_to_Gen2_2.png)

4. **Monitorizar a sua atualização** verificando o estado no portal do Azure:

   ![Upgrade3](./media/sql-data-warehouse-upgrade-to-latest-generation/Upgrade_to_Gen2_3.png)
   
   O primeiro passo do processo de atualização atravessa a operação de dimensionamento ("Atualizar - Offline") em que serão eliminadas todas as sessões e ligações serão ignoradas. 
   
   O segundo passo do processo de atualização é a migração de dados ("Atualizar - Online"). Migração de dados é um processo de em segundo plano online trickle, que lentamente move os dados columnar de arquitetura de armazenamento antigo para a nova arquitetura de armazenamento tirar partido de uma cache SSD local. Durante este período, o armazém de dados será online para consultar e carregar. Todos os seus dados serão disponíveis para consulta independentemente se esta tiver sido migrada ou não. A migração de dados ocorre uma taxa variando consoante o tamanho dos dados, o nível de desempenho e o número de segmentos sua columnstore. 

5. **Recomendação opcional:** a fim de acelerar o processo de em segundo plano de migração de dados, é recomendado para forçar imediatamente o movimento de dados através da execução [reconstrução Alter Index](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-index) em todas as tabelas columnstore numa maior SLO e recursos classe. Esta operação está offline em comparação com o processo de fundo trickle; No entanto, a migração de dados será muito mais rápida onde, em seguida, pode tirar partido da nova arquitetura do armazenamento avançado uma vez concluída com rowgroups de alta qualidade. 

Esta consulta seguinte gera os comandos necessários Alter Index Rebuild a fim de acelerar o processo de migração de dados:

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
O armazém de dados atualizada está online. Para tirar partido da arquitetura do avançada, consulte [classes de recursos para a gestão de carga de trabalho](resource-classes-for-workload-management.md).
 
