---
title: Backups do Azure de automática, georredundante de base de dados SQL | Documentos da Microsoft
description: Base de dados SQL cria uma cópia de segurança da base de dados local intervalos de poucos minutos e utiliza o armazenamento georredundante de acesso de leitura do Azure para redundância geográfica automaticamente.
services: sql-database
author: anosov1960
manager: craigg
ms.service: sql-database
ms.custom: business continuity
ms.topic: conceptual
ms.workload: Active
ms.date: 07/25/2018
ms.author: sashan
ms.reviewer: carlrab
ms.openlocfilehash: 8798d0f17918ecce473afe8dc21b3f60bf0fa4b1
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/08/2018
ms.locfileid: "39620135"
---
# <a name="learn-about-automatic-sql-database-backups"></a>Saiba mais sobre cópias de segurança automáticas da base de dados SQL

Base de dados SQL cria cópias de segurança da base de dados e utiliza o armazenamento com redundância geográfica do Azure do acesso de leitura (RA-GRS) para fornecer redundância geográfica automaticamente. Estas cópias de segurança são criadas automaticamente e sem encargos adicionais. Não precisa de fazer nada para que aconteçam. Cópias de segurança da base de dados são uma parte essencial de qualquer estratégia de recuperação de desastre e continuidade comercial, porque eles protegem os dados de danos acidentais ou eliminação. Se as regras de segurança exigem que as cópias de segurança estão disponíveis por um longo período de tempo, pode configurar uma política de retenção de cópia de segurança de longo prazo. Para obter mais informações, veja [Retenção de longa duração](sql-database-long-term-retention.md).

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="what-is-a-sql-database-backup"></a>O que é uma cópia de segurança da base de dados SQL?

Base de dados SQL utiliza a tecnologia do SQL Server para criar [completo](https://msdn.microsoft.com/library/ms186289.aspx), [diferencial](https://docs.microsoft.com/sql/relational-databases/backup-restore/differential-backups-sql-server), e [log de transação](https://msdn.microsoft.com/library/ms191429.aspx) (PITR) de restauro de cópias de segurança para os objetivos de ponto no tempo. Os backups de log de transação ocorram, geralmente, a cada 5 a 10 minutos e cópias de segurança diferenciais ocorram, geralmente, a cada 12 horas, com a frequência com a base do nível de desempenho e a quantidade de atividade da base de dados. Backups de log de transação, com cópias de segurança completas e diferenciais, permitem-lhe restaurar uma base de dados para um específico ponto anterior no tempo para o mesmo servidor que aloja a base de dados. Ao restaurar uma base de dados, o serviço descobre qual log completas, diferenciais e transação cópias de segurança tem de ser restaurada.


Pode utilizar estas cópias de segurança para:

* Restaure uma base de dados para um ponto anterior no tempo dentro do período de retenção. Esta operação irá criar uma nova base de dados no mesmo servidor que a base de dados original.
* Restaure uma base de dados eliminada para o tempo que tiver sido eliminado ou a qualquer altura dentro do período de retenção. Só pode ser restaurado a base de dados eliminada no mesmo servidor onde a base de dados original foi criado.
* Restaure uma base de dados para outra região geográfica. Isto permite-lhe recuperar a partir de um desastre geográfico ao não é possível aceder ao seu servidor e base de dados. Ele cria uma nova base de dados em qualquer servidor existente em qualquer lugar no mundo. 
* Restaure uma base de dados a partir de uma cópia de segurança de longo prazo específica se a base de dados tiver sido configurado com uma política de retenção de longo prazo (LTR). Isto permite-lhe restaurar uma versão antiga da base de dados para satisfazer uma solicitação de conformidade ou para executar uma versão antiga da aplicação. Ver [retenção a longo prazo](sql-database-long-term-retention.md).
* Para efetuar um restauro, consulte [restaurar a base de dados de cópias de segurança](sql-database-recovery-using-backups.md).

> [!NOTE]
> No armazenamento do Azure, o termo *replicação* refere-se para copiar os arquivos de uma localização para outra. Do SQL *replicação de base de dados* refere-se ao manter várias bases de dados secundários sincronizados com uma base de dados primária. 
> 

## <a name="how-long-are-backups-kept"></a>Quanto tempo as cópias de segurança permanecem?
Cada cópia de segurança da base de dados SQL tem um período de retenção predefinido que baseia-se no escalão de serviço da base de dados e é diferente entre o [modelo de compra baseado em DTU](sql-database-service-tiers-dtu.md) e o [modelo de compra baseado em vCore](sql-database-service-tiers-vcore.md). Pode atualizar o período de retenção de cópia de segurança para uma base de dados. Ver [período de retenção de cópia de segurança de alteração](#how-to-change-backup-retention-period) para obter mais detalhes.

Se eliminar uma base de dados, base de dados SQL manterá as cópias de segurança da mesma forma que faria para uma base de dados online. Por exemplo, se eliminar uma base de dados básico que tem um período de retenção de sete dias, é guardada uma cópia de segurança que é de quatro dias de antiguidade por três dias mais.

Se precisar de manter as cópias de segurança por mais tempo do que o período de retenção máximo PITR, pode modificar as propriedades de cópia de segurança para adicionar um ou mais períodos de retenção longo prazo para a base de dados. Ver [retenção de cópia de segurança de longo prazo](sql-database-long-term-retention.md) para obter mais detalhes.

> [!IMPORTANT]
> Se eliminar o servidor de SQL do Azure que aloja as bases de dados SQL, tudo de dados elásticas e bases de dados que pertencem ao servidor também são eliminados e não podem ser recuperados. Não é possível restaurar um servidor foi eliminado. Mas se tiver configurado a retenção de longa duração, as cópias de segurança das bases de dados com LTR não serão eliminadas e esses bancos de dados podem ser restaurados.

### <a name="pitr-retention-for-dtu-based-service-tiers"></a>Retenção de PITR para os escalões de serviço baseado em DTU
O período de retenção predefinido para uma base de dados criada com o modelo de compra baseado em DTU depende do escalão de serviço:

* O escalão de serviço básico é 1 semana.
* Camada de serviço Standard é cinco semanas.
* Escalão de serviço Premium é cinco semanas.

Se reduzir o período de retenção PITR atual, todas as cópias de segurança existentes mais antigas do que o novo período de retenção já não estará disponíveis. 

Se aumentar o período de retenção PITR atual, base de dados SQL manterá as cópias de segurança existentes até que o período de retenção mais longo for atingido.

## <a name="how-often-do-backups-happen"></a>Com que frequência as cópias de segurança acontecem?
### <a name="backups-for-point-in-time-restore"></a>Cópias de segurança para restauro para ponto no tempo
Base de dados SQL suporta o Self-Service para o restauro de ponto no tempo (PITR) ao criar automaticamente a cópia de segurança completa, backups diferenciais e backups de log de transação. Cópias de segurança completa da base de dados são criadas semanalmente e cópias de segurança da base de dados diferenciais são geralmente criadas a cada 12 horas para backups de log de transação em geral, são criados a cada 5-10 minutos, com a frequência com a base do nível de desempenho e a quantidade de atividade da base de dados. A primeira cópia de segurança completa está agendada imediatamente depois de criar uma base de dados. Ela normalmente fica concluída no prazo de 30 minutos, mas pode demorar mais tempo quando a base de dados é um tamanho significativo. Por exemplo, a cópia de segurança inicial pode demorar mais tempo numa base de dados restaurada ou copiar uma base de dados. Após a primeira cópia de segurança completa, todas as cópias de segurança adicionais são agendadas automaticamente e gerenciadas automaticamente em segundo plano. O tempo certo de todas as cópias de segurança da base de dados é determinado pelo serviço de base de dados SQL, como ele faz o balanceamento de carga de trabalho geral do sistema.

As cópias de segurança PITR são georredundante e protegido por [replicação entre regiões de armazenamento do Azure](../storage/common/storage-redundancy-grs.md#read-access-geo-redundant-storage)

Para obter mais informações, consulte [restaurodepontonotempoda](sql-database-recovery-using-backups.md#point-in-time-restore)

### <a name="backups-for-long-term-retention"></a>Cópias de segurança de retenção de longa duração
Base de dados SQL oferece a opção de configurar a retenção de longo prazo (LTR) de cópias de segurança completas para até 10 anos no armazenamento de Blobs do Azure. Se a política LTR estiver ativada, as cópias de segurança completas semanais são copiadas automaticamente para um contentor de armazenamento RA-GRS diferente. Para cumprir o requisito de conformidade diferentes, pode selecionar os períodos de retenção diferentes para cópias de segurança semanais, mensais e/ou anuais. O consumo de armazenamento depende na frequência de cópias de segurança e a retenção period(s) selecionada. Pode utilizar o [Calculadora de preços de LTR](https://azure.microsoft.com/pricing/calculator/?service=sql-database) para calcular o custo de armazenamento LTR. 

Como PITR, as cópias de segurança LTR são georredundante e estão protegidas pela [replicação do armazenamento do Azure entre regiões](../storage/common/storage-redundancy-grs.md#read-access-geo-redundant-storage).

Para obter mais informações, consulte [retenção de cópia de segurança de longo prazo](sql-database-long-term-retention.md).

## <a name="are-backups-encrypted"></a>As cópias de segurança são encriptadas?

Se a sua base de dados é encriptado com TDE, as cópias de segurança são encriptadas automaticamente em repouso, incluindo cópias de segurança LTR. Quando a TDE está ativada para uma base de dados SQL do Azure, cópias de segurança também são encriptadas. Todas as novas bases de dados do SQL do Azure são configuradas com o TDE ativado por predefinição. Para obter mais informações sobre a TDE, consulte [encriptação de dados transparente com a base de dados do Azure SQL](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql).

## <a name="how-do-automated-backups-impact-my-compliance"></a>Como cópias de segurança automáticas afetar minha conformidade?

Quando migra a base de dados de um escalão de serviço baseado em DTU com o período de retenção PITR padrão de 35 dias, para um escalão de serviço baseado em vCore, o período de retenção PITR é preservado para se certificar de que a política de recuperação de dados do seu aplicativo não é comprometida. Se o período de retenção predefinida não cumprir os requisitos de conformidade, pode alterar o período de retenção PITR com o PowerShell ou a REST API. Ver [período de retenção de cópia de segurança de alteração](#how-to-change-backup-retention-period) para obter mais detalhes.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="how-to-change-backup-retention-period"></a>Como alterar o período de retenção de cópia de segurança
Pode alterar o período de retenção predefinido com REST API ou o PowerShell. Os valores suportados são: 7, 14, 21, 28 ou 35 dias. Os exemplos seguintes mostram como alterar a retenção PITR 28 dias. 

> [!NOTE]
> APIs de Thes só terá impacto sobre o período de retenção PITR. Se tiver configurado LTR da base de dados, não será afetada. Ver [retenção de cópia de segurança de longo prazo](sql-database-long-term-retention.md) para obter detalhes sobre como alterar o period(s) de retenção LTR.

### <a name="change-pitr-backup-retention-period-using-powershell"></a>Alterar o período de retenção de cópia de segurança de PITR com o PowerShell
```powershell
Set-AzureRmSqlDatabaseBackupShortTermRetentionPolicy -ResourceGroupName resourceGroup -ServerName testserver -DatabaseName testDatabase -RetentionDays 28
```
> [!IMPORTANT]
> Esta API está incluída no módulo do PowerShell azurerm. SQL a partir da versão [4.7.0-preview](https://www.powershellgallery.com/packages/AzureRM.Sql/4.7.0-preview). 

### <a name="change-pitr-retention-period-using-rest-api"></a>Alterar o período de retenção PITR com a REST API
**Pedido de exemplo**
```http
PUT https://management.azure.com/subscriptions/00000000-1111-2222-3333-444444444444/resourceGroups/resourceGroup/providers/Microsoft.Sql/servers/testserver/databases/testDatabase/backupShortTermRetentionPolicies/default?api-version=2017-10-01-preview
```
**Corpo do pedido**
```json
{
  "properties":{  
      "retentionDays":28
   }
}
```
**Resposta de exemplo**

Código de estado: 200
```json
{
  "id": "/subscriptions/00000000-1111-2222-3333-444444444444/providers/Microsoft.Sql/resourceGroups/resourceGroup/servers/testserver/databases/testDatabase/backupShortTermRetentionPolicies/default",
  "name": "default",
  "type": "Microsoft.Sql/resourceGroups/servers/databases/backupShortTermRetentionPolicies",
  "properties": {
    "retentionDays": 28
  }
}
```
Ver [API de REST de retenção de cópia de segurança](https://docs.microsoft.com/rest/api/sql/backupshorttermretentionpolicies) para obter mais detalhes.

## <a name="next-steps"></a>Passos Seguintes

- Cópias de segurança da base de dados são uma parte essencial de qualquer estratégia de recuperação de desastre e continuidade comercial, porque eles protegem os dados de danos acidentais ou eliminação. Para saber mais sobre as outras base de dados do Azure SQL soluções de continuidade empresarial, consulte [descrição geral da continuidade de negócio](sql-database-business-continuity.md).
- Para restaurar para um ponto no tempo, com o portal do Azure, consulte [restaurar base de dados para um ponto no tempo com o portal do Azure](sql-database-recovery-using-backups.md).
- Para restaurar para um ponto no tempo com o PowerShell, consulte [restaurar base de dados para um ponto no tempo com o PowerShell](scripts/sql-database-restore-database-powershell.md).
- Para configurar, gerir e restaurar a partir de retenção de longa duração de cópias de segurança no armazenamento de Blobs do Azure com o portal do Azure automatizadas, consulte [gerir a retenção de cópia de segurança de longa duração com o portal do Azure](sql-database-long-term-backup-retention-configure.md).
- Para configurar, gerir e restaurar a partir de retenção de longa duração de cópias de segurança no armazenamento do blogue do Azure com o PowerShell automatizadas, consulte [gerir a retenção de cópia de segurança de longa duração com o PowerShell](sql-database-long-term-backup-retention-configure.md).
