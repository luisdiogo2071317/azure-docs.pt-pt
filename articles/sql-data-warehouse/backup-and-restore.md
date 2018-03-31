---
title: Cópia de segurança do armazém de dados SQL do Azure e de restauro - instantâneos, georredundante | Microsoft Docs
description: Saiba como funciona a cópia de segurança e restauro no Azure SQL Data Warehouse. Utilize cópias de segurança de armazém de dados para restaurar o seu armazém de dados para um ponto de restauro na região primária, ou as cópias de segurança georredundante restaurar para uma região geográfica diferentes.
services: sql-data-warehouse
author: ronortloff
manager: jhubbard
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: design
ms.date: 03/28/2018
ms.author: rortloff
ms.reviewer: igorstan
ms.openlocfilehash: 7f540bca0d2eb2c9009a386bd14a5beda2912014
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/30/2018
---
# <a name="backup-and-restore-in-azure-sql-data-warehouse"></a>Cópia de segurança e restauro no Azure SQL Data Warehouse
Saiba como funciona a cópia de segurança e restauro no Azure SQL Data Warehouse. Utilize cópias de segurança de armazém de dados para restaurar o seu armazém de dados para um ponto de restauro na região primária, ou as cópias de segurança georredundante restaurar para uma região geográfica diferentes. 

## <a name="what-is-backup-and-restore"></a>O que é a cópia de segurança e restauro?
A *cópia de segurança de armazém de dados* for a cópia da base de dados que pode utilizar para restaurar um armazém de dados.  Uma vez que o SQL Data Warehouse é um sistema distribuído, uma cópia de segurança do armazém de dados consiste em muitos ficheiros que estão localizados no armazenamento do Azure. Uma cópia de segurança do armazém de dados inclui instantâneos da base de dados local e georreplicação-as cópias de segurança de todas as bases de dados e ficheiros que estão associados um armazém de dados. 

A *restauro do armazém de dados* é um novo armazém de dados que é criado a partir de uma cópia de segurança existente ou do armazém de dados eliminada. O armazém de dados restaurada recria o armazém de dados de cópia de segurança num momento específico. Restaurar o seu armazém de dados é uma parte essencial de qualquer estratégia de recuperação de continuidade e desastre negócio porque cria novamente os dados depois de danos acidentais ou eliminação.

Restauros locais e geográficas fazem parte das capacidades de recuperação de desastre do SQL Data Warehouse. 

## <a name="local-snapshot-backups"></a>Cópias de segurança do instantâneo local
Cópias de segurança do instantâneo local são uma funcionalidade incorporada do serviço.  Não é necessário ativá-los. 

SQL Data Warehouse assume instantâneos do seu armazém de dados ao longo do dia. Os instantâneos são disponíveis durante sete dias. Armazém de dados do SQL Server suporta um objetivo de ponto de recuperação (RPO) oito horas. Pode restaurar o seu armazém de dados na região primária em qualquer um dos instantâneos efetuadas nos últimos sete dias.

Para ver quando o instantâneo último iniciada, execute esta consulta no online SQL Data Warehouse. 

```sql
select   top 1 *
from     sys.pdw_loader_backup_runs 
order by run_id desc
;
```

### <a name="snapshot-retention-when-a-data-warehouse-is-paused"></a>Retenção instantâneo quando um armazém de dados está em pausa
SQL Data Warehouse não cria instantâneos e não expira instantâneos enquanto um armazém de dados está em pausa. A antiguidade do instantâneo não altera enquanto o armazém de dados está em pausa. Retenção de instantâneo é baseada no número de dias que no armazém de dados está online, não os dias de calendário.

Por exemplo, se um instantâneo é iniciado 1 de Outubro a 4 pm e o armazém de dados está em pausa 3 de Outubro, 4 pm, os instantâneos são até dois dias de antiguidade. Quando o armazém de dados fica online novamente o instantâneo é dois dias de antiguidade. Se o armazém de dados ficar online 5 de Outubro, 4 pm, o instantâneo é dois dias de antiguidade e continua durante cinco dias mais.

Quando o armazém de dados volta a ficar online, o SQL Data Warehouse retoma dos novos instantâneos e expira instantâneos quando têm mais de sete dias de dados.

### <a name="snapshot-retention-when-a-data-warehouse-is-dropped"></a>Retenção instantâneo quando um armazém de dados foi removido
Quando remover um armazém de dados, o SQL Data Warehouse cria um instantâneo final e guarda-o durante sete dias. Pode restaurar o armazém de dados para o ponto de restauro final criado em eliminação. 

> [!IMPORTANT]
> Se eliminar uma instância do SQL server lógica, todas as bases de dados que pertencem à instância também são eliminados e não podem ser recuperados. Não é possível restaurar um servidor eliminado.
> 

## <a name="geo-backups"></a>Cópias de segurança a Georreplicação
O SQL Data Warehouse efetua uma georreplicação-cópia de segurança uma vez por dia para um [Centro de dados emparelhado](../best-practices-availability-paired-regions.md). O RPO para um georrestauro é de 24 horas. Pode restaurar a georreplicação-cópia de segurança para o servidor na região georreplicação-emparelhado. Uma georreplicação-cópia de segurança garante que pode restaurar o armazém de dados no caso de não é possível aceder os instantâneos na sua região primária.

As cópias de segurança a Georreplicação estão por predefinição. Se o seu armazém de dados está otimizado para elasticidade, pode [ativamente](/powershell/module/azurerm.sql/set-azurermsqldatabasegeobackuppolicy) se desejar. Não é possível ativamente georreplicação-cópias de segurança com o otimizado para a camada de desempenho de computação.

## <a name="backup-costs"></a>Custos de cópia de segurança
Vai notar que a fatura do Azure tem um item de linha para o Premium Storage do Azure e um item de linha para o armazenamento georredundante. A taxa de Premium Storage é o custo total para armazenar os dados na região primária, que inclui os instantâneos.  A taxa georredundante abrange o custo de armazenar cópias de segurança georreplicação.  

O total de custos para o seu armazém de dados primária e sete dias de instantâneos do Blob do Azure é arredondado para o TB mais próximo. Por exemplo, se o armazém de dados é 1,5 TB e os instantâneos utilizam 100 GB, é-lhe faturado para 2 TB de dados às taxas de Premium Storage do Azure. 

> [!NOTE]
> Cada instantâneo está inicialmente vazio e crescimentos de como efetuar alterações no armazém de dados principal. Todos os instantâneos aumentam de tamanho como as alterações de armazém de dados. Por conseguinte, os custos de armazenamento de instantâneos de crescimento, de acordo com a taxa de alteração.
> 
> 

Se estiver a utilizar o armazenamento georredundante, receberá um custo de armazenamento separada. O armazenamento georredundante é faturado a taxa de acesso de leitura geograficamente armazenamento redundantes (RA-GRS) padrão.

Para obter mais informações sobre preços do SQL Data Warehouse, consulte [preços do SQL Data Warehouse](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).

## <a name="restoring-from-restore-points"></a>Restaurar a partir de pontos de restauro
Cada instantâneo tem um ponto de restauro que representa a hora de início do instantâneo. Para restaurar um armazém de dados, escolha um ponto de restauro e emitir um comando de restauro.  

O SQL Data Warehouse restaura sempre a cópia de segurança para um novo armazém de dados. Pode manter o armazém de dados restaurada e aquele atual ou elimine um deles. Se pretende substituir o armazém de dados atual com o armazém de dados restaurada, pode alterá-la utilizando [ALTER DATABASE (Azure SQL Data Warehouse)](/sql/t-sql/statements/alter-database-azure-sql-data-warehouse) com a opção de nome modificar. 

Para restaurar um armazém de dados, consulte [restaurar um armazém de dados no portal do Azure](sql-data-warehouse-restore-database-portal.md), [restaurar um armazém de dados com o PowerShell](sql-data-warehouse-restore-database-powershell.md), ou [restaurar um data warehouse com T-SQL](sql-data-warehouse-restore-database-rest-api.md) .

Para restaurar um armazém de dados eliminada ou em pausa, pode [criar um pedido de suporte](sql-data-warehouse-get-started-create-support-ticket.md). 


## <a name="geo-redundant-restore"></a>Georredundante restauro
Pode restaurar o seu armazém de dados para qualquer região que suportam o Azure SQL Data Warehouse ao seu nível de desempenho que escolheu. 

> [!NOTE]
> Para efetuar um restauro com redundância geográfica tem não optou por fora esta funcionalidade.
> 
> 

## <a name="restore-timeline"></a>Restaurar a linha cronológica
Pode restaurar uma base de dados para qualquer ponto de restauro disponíveis nos últimos sete dias. Instantâneos de iniciar a cada quatro para oito horas e estão disponíveis para sete dias. Quando um instantâneo com mais de sete dias, expirar e o respetivo ponto de restauro já não está disponível. 

As cópias de segurança não acontecer contra um armazém de dados em pausa. Se o seu armazém de dados está em pausa durante mais de sete dias, não terá quaisquer pontos de restauro. 

## <a name="restore-costs"></a>Restaurar os custos
Os encargos de armazenamento para o armazém de dados restaurada é faturado a taxa de Premium Storage do Azure. 

Se colocar em pausa de um armazém de dados restaurada, são-lhe cobrados para armazenamento a taxa de Premium Storage do Azure. A vantagem de colocar em pausa não é que lhe serem cobrados os recursos de computação.

Para obter mais informações sobre preços do SQL Data Warehouse, consulte [preços do SQL Data Warehouse](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).

## <a name="restore-use-cases"></a>Casos de utilização de restauro
A utilização principal para o restauro de armazém de dados é recuperar dados após a perda acidental de dados ou danos. Também pode utilizar o restauro de armazém de dados para manter uma cópia de segurança mais de sete dias. Depois da cópia de segurança é restaurada, que tem o armazém de dados online e pode colocar em pausa indefinidamente para reduzir os custos de computação. A base de dados em pausa implica os encargos de armazenamento, a taxa de Premium Storage do Azure. 

## <a name="next-steps"></a>Passos Seguintes
Para obter mais informações sobre o planeamento de desastre, consulte [descrição geral da continuidade de negócio](../sql-database/sql-database-business-continuity.md)
