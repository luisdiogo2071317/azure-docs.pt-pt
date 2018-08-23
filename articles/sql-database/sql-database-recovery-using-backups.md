---
title: Restaurar uma base de dados SQL do Azure a partir de uma cópia de segurança | Documentos da Microsoft
description: Saiba mais sobre o restauro de ponto no tempo, o que permite-lhe reverter uma base de dados do SQL do Azure para um ponto anterior no tempo (até 35 dias).
services: sql-database
author: anosov1960
manager: craigg
ms.service: sql-database
ms.custom: business continuity
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: sashan
ms.reviewer: carlrab
ms.openlocfilehash: 75805cad43f015f1741193ec5a1ead1fa7603f41
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/20/2018
ms.locfileid: "42057025"
---
# <a name="recover-an-azure-sql-database-using-automated-database-backups"></a>Recuperar uma base de dados SQL do Azure com cópias de segurança da base de dados automatizada
Base de dados SQL fornece estas opções de recuperação de base de dados com [cópias de segurança da base de dados automatizadas](sql-database-automated-backups.md) e [cópias de segurança em retenção de longo prazo](sql-database-long-term-retention.md). Pode restaurar a partir de uma cópia de segurança da base de dados para:

* Uma nova base de dados no mesmo servidor lógico recuperado para um determinado ponto no tempo dentro do período de retenção. 
* Uma base de dados no mesmo servidor lógico recuperado para o tempo de eliminação de uma base de dados eliminada.
* Uma nova base de dados em qualquer servidor lógico em qualquer região recuperado para o ponto das cópias de segurança diárias mais recentes no armazenamento de BLOBs georreplicado (RA-GRS).

> [!IMPORTANT]
> Não é possível substituir a base de dados existente durante o restauro.
>

Uma base de dados restaurada implica um custo de armazenamento extra nas seguintes condições: 
- Restauro de P11-P15 para S4-S12 ou P1 – P6 se o tamanho máximo da base de dados for superior a 500 GB.
- Restauro de P1 – P6 para S4-S12 se o tamanho máximo da base de dados for superior a 250 GB.

O extra custo é porque o tamanho máximo da base de dados restaurada é maior do que a quantidade de armazenamento incluída para o nível de desempenho, e qualquer armazenamento extra aprovisionado acima da quantidade incluída é cobrado extra.  Para detalhes de armazenamento extra de preços, consulte a [base de dados SQL página de preços](https://azure.microsoft.com/pricing/details/sql-database/).  Se a quantidade real de espaço utilizada é inferior à quantidade de armazenamento incluída, em seguida, isso encargos extra pode ser evitado ao reduzir o tamanho máximo da base de dados para o montante incluído.  

> [!NOTE]
> [Cópias de segurança da base de dados automatizadas](sql-database-automated-backups.md) são utilizadas quando cria uma [cópia da base de dados](sql-database-copy.md). 
>


## <a name="recovery-time"></a>Tempo de recuperação
O tempo de recuperação para restaurar uma base de dados com cópias de segurança da base de dados automatizada é afetado por diversos fatores: 

* O tamanho da base de dados
* O nível de desempenho da base de dados
* O número de registos de transações envolvidos
* A quantidade de atividade que tem de ser reproduzido para recuperar para o ponto de restauro
* A largura de banda de rede se o restauro estiver numa região diferente 
* O número de pedidos de restauro em simultâneo a ser processado na região de destino. 
  
  Para uma base de dados muito grandes e/ou Active Directory, o restauro pode demorar várias horas. Se houver um interrupção prolongada numa região, é possível que há um grande número de pedidos de restauro geográfico que está a ser processados pelo noutras regiões. Quando existem muitos pedidos, pode aumentar o tempo de recuperação para bases de dados nessa região. A maioria das bases de dados restaura concluída no prazo de 12 horas.

Para uma única subscrição, existem algumas limitações no número de pedidos de restauro em simultâneo (incluindo o ponto de restauro para um tempo, o restauro geográfico e o restauro da cópia de segurança de retenção longo prazo) que está a ser submetidas e prosseguiram:
|  | **N. º máximo de pedidos simultâneos a ser processado** | **N. º máximo de pedidos simultâneos a ser submetida** |
| :--- | --: | --: |
|Base de dados (por subscrição)|10|60|
|Conjunto elástico (por conjunto)|4|200|
||||

Não é uma funcionalidade incorporada em massa de restauro. O [SQL Database do Azure: recuperação de servidor completo](https://gallery.technet.microsoft.com/Azure-SQL-Database-Full-82941666) script é um exemplo de uma maneira de realizar essa tarefa.

> [!IMPORTANT]
> Para recuperar ao utilizar cópias de segurança automáticas, tem de ser um membro da função de contribuinte do SQL Server na subscrição ou ser o proprietário da subscrição - veja [RBAC: funções incorporadas](../role-based-access-control/built-in-roles.md). Pode recuperar através do portal do Azure, do PowerShell ou da API REST. Não é possível utilizar Transact-SQL. 
> 

## <a name="point-in-time-restore"></a>Restauro para um ponto anterior no tempo

Pode restaurar uma base de dados existente para um ponto anterior no tempo como uma nova base de dados no mesmo servidor lógico no portal do Azure, [PowerShell](https://docs.microsoft.com/powershell/module/azurerm.sql/restore-azurermsqldatabase), ou o [REST API](https://msdn.microsoft.com/library/azure/mt163685.aspx). 

> [!TIP]
> Para um script do PowerShell de exemplo que mostra como executar um restauro de ponto no tempo de uma base de dados, consulte [restaurar uma base de dados SQL com o PowerShell](scripts/sql-database-restore-database-powershell.md).
>

Pode ser restaurada a base de dados para qualquer nível de desempenho ou de camada de serviço e como uma base de dados ou para um conjunto elástico. Certifique-se de que tem recursos suficientes no servidor lógico ou do conjunto elástico para o qual está a restaurar a base de dados. Depois de concluído, a base de dados restaurada é uma base de dados online, normal e totalmente acessível. A base de dados restaurada é cobrada às tarifas normais com base em seu nível de desempenho e a camada de serviço. Que não incorra em custos até que o restauro de base de dados esteja concluído.

Geralmente, restaurar uma base de dados para um ponto anterior para fins de recuperação. Ao fazê-lo, pode tratar a base de dados restaurada como um substituto para a base de dados original ou utilizá-lo a obter dados a partir e, em seguida, atualizar a base de dados original. 

* ***Substituição de base de dados:*** se a base de dados restaurada destina-se como um substituto para a base de dados original, deve verificar o nível de desempenho e/ou camada de serviços, conforme adequada e dimensionar a base de dados, se necessário. Pode mudar o nome da base de dados original e, em seguida, dar a base de dados restaurada ao nome original utilizando o [ALTER DATABASE](/sql/t-sql/statements/alter-database-azure-sql-database) comando em T-SQL. 
* ***Recuperação de dados:*** se pretender recuperar dados de base de dados restaurada para recuperar a partir de um erro de utilizador ou aplicação, terá de escrever e executar os scripts de recuperação de dados necessários para extrair dados do banco de dados restaurado para a base de dados original. Embora a operação de restauro pode demorar muito tempo a concluir, a base de dados de restauro está visível na lista de base de dados em todo o processo de restauro. Se eliminar a base de dados durante o restauro, a operação de restauro foi cancelada e não lhe são cobradas da base de dados que não foi possível concluir o restauro. 

### <a name="azure-portal"></a>Portal do Azure

Para recuperar para um ponto no tempo, com o portal do Azure, abra a página da base de dados e clique em **restaurar** na barra de ferramentas.

![ponto de restauro anterior no tempo](./media/sql-database-recovery-using-backups/point-in-time-recovery.png)

## <a name="deleted-database-restore"></a>Restauro de base de dados eliminada
Pode restaurar uma base de dados eliminada para a hora de eliminação de uma base de dados eliminada no mesmo servidor lógico no portal do Azure, [PowerShell](https://docs.microsoft.com/powershell/module/azurerm.sql/restore-azurermsqldatabase), ou o [REST (createMode = restaurar)](https://msdn.microsoft.com/library/azure/mt163685.aspx). Pode restaurar uma base de dados eliminada para um ponto anterior no tempo durante a utilização de retenção [PowerShell](https://docs.microsoft.com/powershell/module/azurerm.sql/restore-azurermsqldatabase).

> [!TIP]
> Para um script do PowerShell de exemplo que mostra como restaurar uma base de dados eliminada, consulte [restaurar uma base de dados SQL com o PowerShell](scripts/sql-database-restore-database-powershell.md).
>

> [!IMPORTANT]
> Se eliminar uma instância de servidor de base de dados do Azure SQL, todas as suas bases de dados também são eliminados e não podem ser recuperados. Atualmente não há suporte para restaurar um servidor foi eliminado.
> 

### <a name="azure-portal"></a>Portal do Azure

Para recuperar uma base de dados eliminada durante sua [período de retenção do modelo baseado em DTU](sql-database-service-tiers-dtu.md) ou [período de retenção do modelo baseado em vCore](sql-database-service-tiers-vcore.md) com o portal do Azure, abra a página do seu servidor e na área de operações, clique em **Bases de dados eliminadas**.

![deleted-database-restore-1](./media/sql-database-recovery-using-backups/deleted-database-restore-1.png)


![deleted-database-restore-2](./media/sql-database-recovery-using-backups/deleted-database-restore-2.png)

## <a name="geo-restore"></a>Georrestauro
Pode restaurar uma base de dados SQL em qualquer servidor em qualquer região do Azure a partir as mais recentes georreplicado completas e diferenciais cópias de segurança. O restauro geográfico utiliza uma cópia de segurança georredundante como origem e pode ser usado para recuperar uma base de dados, mesmo que a base de dados ou o Centro de dados não está acessível devido a uma falha. 

Georrestauro encontra-se a opção de recuperação predefinida quando a sua base de dados está indisponível devido a um incidente na região onde está hospedado o banco de dados. Se um incidente em grande escala nos resultados da região na indisponibilidade da sua aplicação de base de dados, pode restaurar uma base de dados das cópias de segurança georreplicado para um servidor em qualquer outra região. Existe um atraso entre quando é feita uma cópia de segurança diferencial e quando é georreplicado a Azure blob numa região diferente. Este atraso pode demorar até uma hora, por isso, se ocorrer um desastre, pode haver até à perda de dados de uma hora. A ilustração seguinte mostra o restauro da base de dados da cópia de segurança disponível última noutra região.

![Restauro geográfico](./media/sql-database-geo-restore/geo-restore-2.png)

> [!TIP]
> Para um script do PowerShell de exemplo que mostra como efetuar um georrestauro, consulte [restaurar uma base de dados SQL com o PowerShell](scripts/sql-database-restore-database-powershell.md).
> 

Restauro de ponto no tempo na geo-secundária não é atualmente suportado. Restauro de ponto no tempo pode ser feito apenas numa base de dados primária. Para obter informações detalhadas sobre como utilizar o restauro geográfico para recuperar a partir de uma falha, consulte [recuperar a partir de um período de indisponibilidade](sql-database-disaster-recovery.md).

> [!IMPORTANT]
> Recuperação a partir de cópias de segurança é a mais básica das soluções de recuperação após desastre disponíveis no banco de dados SQL com o objetivo de ponto de recuperação mais longo (RPO) e o tempo de recuperação de estimativa (ERT). Para soluções que utilizar bases de dados de tamanho pequeno (camada de serviços por exemplo, Basic ou conjuntos elásticos de bases de dados de inquilinos de tamanho pequeno), o restauro geográfico com frequência é uma solução de DR razoável com um ERT de 12 horas. Para soluções com grandes bancos de dados e necessitam de recuperação menores vezes, deve considerar o uso [ativação pós-falha de grupos e a georreplicação ativa](sql-database-geo-replication-overview.md). Replicação geográfica activa oferece um RPO de muito mais baixo e ERT uma vez que ele apenas requer que inicie uma ativação pós-falha para uma secundária replicada de forma contínua. Para obter mais informações sobre as opções de continuidade do negócio, veja [descrição geral da continuidade do negócio](sql-database-business-continuity.md).
> 

### <a name="azure-portal"></a>Portal do Azure

Para uma base de dados durante o restauro geográfico seus [período de retenção do modelo baseado em DTU](sql-database-service-tiers-dtu.md) ou [período de retenção do modelo baseado em vCore](sql-database-service-tiers-vcore.md) com o portal do Azure, abra a página de bases de dados SQL e, em seguida, clique em **adicionar** . Na **selecionar origem** caixa de texto, selecione **cópia de segurança**. Especifique a cópia de segurança a partir da qual pretende efetuar a recuperação, na região e no servidor da sua preferência. 

## <a name="programmatically-performing-recovery-using-automated-backups"></a>A execução através de programação de recuperação com cópias de segurança automáticas
Como foi discutido anteriormente, além de portal do Azure, a recuperação de base de dados pode ser executada por meio de programação com o Azure PowerShell ou a API REST. As tabelas seguintes descrevem o conjunto de comandos disponíveis.

### <a name="powershell"></a>PowerShell
| Cmdlet | Descrição |
| --- | --- |
| [Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase) |Obtém uma ou mais bases de dados. |
| [Get-AzureRMSqlDeletedDatabaseBackup](/powershell/module/azurerm.sql/get-azurermsqldeleteddatabasebackup) | Obtém uma base de dados eliminada que pode restaurar. |
| [Get-AzureRmSqlDatabaseGeoBackup](/powershell/module/azurerm.sql/get-azurermsqldatabasegeobackup) |Obtém uma cópia de segurança georredundante de uma base de dados. |
| [Restore-AzureRmSqlDatabase](/powershell/module/azurerm.sql/restore-azurermsqldatabase) |Restaura uma base de dados SQL. |
|  | |

### <a name="rest-api"></a>API REST
| API | Descrição |
| --- | --- |
| [REST (createMode = recuperação)](https://msdn.microsoft.com/library/azure/mt163685.aspx) |Restaura uma base de dados |
| [Começar a criar ou atualizar o estado da base de dados](https://msdn.microsoft.com/library/azure/mt643934.aspx) |Devolve o estado durante uma operação de restauro |
|  | |

## <a name="summary"></a>Resumo
Cópias de segurança automáticas proteger as bases de dados de utilizador e erros de aplicações, a eliminação acidental de base de dados e interrupções prolongadas. Esta capacidade incorporada está disponível para todos os escalões de serviço e níveis de desempenho. 

## <a name="next-steps"></a>Passos Seguintes
* Para uma visão geral de continuidade de negócio e cenários, consulte [descrição geral da continuidade de negócio](sql-database-business-continuity.md).
* Para saber mais sobre SQL do Azure, base de dados automatizada de cópias de segurança, consulte [cópias de segurança automatizadas de base de dados SQL](sql-database-automated-backups.md).
* Para saber mais sobre a retenção de longa duração, veja [retenção a longo prazo](sql-database-long-term-retention.md).
* Para saber mais sobre as opções de recuperação mais rápidas, veja [ativação pós-falha de grupos e a georreplicação ativa](sql-database-geo-replication-overview.md).  
