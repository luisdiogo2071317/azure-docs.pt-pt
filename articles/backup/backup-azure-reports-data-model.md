---
title: Modelo de dados para cópia de segurança do Azure
description: Este artigo fala sobre detalhes de modelo de dados do Power BI para relatórios de cópia de segurança do Azure.
services: backup
author: adiganmsft
manager: shivamg
ms.service: backup
ms.topic: conceptual
ms.date: 06/26/2017
ms.author: adigan
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 573b7e9c5c44c7162b4020f1ef54b8986003c0b5
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52877138"
---
# <a name="data-model-for-azure-backup-reports"></a>Modelo de dados para relatórios do Azure Backup
Este artigo descreve o modelo de dados do Power BI utilizado para a criação de relatórios do Azure Backup. Usando esse modelo de dados, pode filtrar relatórios existentes com base nos campos relevantes e mais importante, criar seus próprios relatórios com tabelas e campos no modelo. 

## <a name="creating-new-reports-in-power-bi"></a>Criar novos relatórios no Power BI
O Power BI fornece recursos de personalização com o que pode [criar relatórios usando o modelo de dados](https://powerbi.microsoft.com/documentation/powerbi-service-create-a-new-report/).

## <a name="using-azure-backup-data-model"></a>Usando o modelo de dados de cópia de segurança do Azure
Pode utilizar os seguintes campos fornecidos como parte do modelo de dados para criar relatórios e personalizar relatórios existentes.

### <a name="alert"></a>Alerta
Esta tabela fornece campos básicos e agregações ao longo de vários campos relacionados alerta.

| Campo | Tipo de Dados | Descrição |
| --- | --- | --- |
| #AlertsCreatedInPeriod |Número inteiro |Número de alertas criados no período de tempo selecionado |
| % ActiveAlertsCreatedInPeriod |Percentagem |Percentagem de alertas ativos no período de tempo selecionado |
| % CriticalAlertsCreatedInPeriod |Percentagem |Percentagem de alertas críticos no período de tempo selecionado |
| AlertOccurrenceDate |Date |Data de quando o alerta foi criado |
| AlertSeverity |Texto |Gravidade do alerta, por exemplo, crítico |
| AlertStatus |Texto |Estado do alerta, por exemplo, Active Directory |
| AlertType |Texto |Tipo de alerta gerado por exemplo, a cópia de segurança |
| AlertUniqueId |Texto |Id exclusivo do alerta gerado |
| AsOnDateTime |Data/Hora |Hora de atualização mais recente para a linha selecionada |
| AvgResolutionTimeInMinsForAlertsCreatedInPeriod |Número decimal |Tempo médio (em minutos) para resolver o alerta para o período de tempo selecionado |
| EntityState |Texto |Estado atual do objeto alerta, por exemplo, Active Directory, eliminado |

### <a name="backup-item"></a>Item de cópia de segurança
Esta tabela fornece campos básicos e agregações ao longo de vários campos de cópia de segurança relacionados com o item.

| Campo | Tipo de Dados | Descrição |
| --- | --- | --- |
| #BackupItems |Número inteiro |Número de itens de cópia de segurança |
| #UnprotectedBackupItems |Número inteiro |Número de itens de cópia de segurança parado para proteção ou configurados para cópias de segurança, mas as cópias de segurança não foi iniciadas|
| AsOnDateTime |Data/Hora |Hora de atualização mais recente para a linha selecionada |
| BackupItemFriendlyName |Texto |Nome amigável de item de cópia de segurança |
| BackupItemId |Texto |ID do item de cópia de segurança |
| BackupItemName |Texto |Nome do item de cópia de segurança |
| BackupItemType |Texto |Tipo de item de cópia de segurança, por exemplo, a VM, FileFolder |
| EntityState |Texto |Estado atual do objeto de item de cópia de segurança, por exemplo, Active Directory, eliminado |
| LastBackupDateTime |Data/Hora |Hora da última cópia de segurança para o item de cópia de segurança selecionado |
| LastBackupState |Texto |Estado da última cópia de segurança para o item de cópia de segurança selecionado por exemplo, com êxito, com falhas |
| LastSuccessfulBackupDateTime |Data/Hora |Hora da última cópia de segurança com êxito para o item de cópia de segurança selecionado |
| ProtectionState |Texto |Estado de proteção atual do item de cópia de segurança por exemplo, protegido, ProtectionStopped |

### <a name="calendar"></a>Calendário
Esta tabela fornece detalhes sobre os campos relacionados com o calendário.

| Campo | Tipo de Dados | Descrição |
| --- | --- | --- |
| Date |Date |Data selecionada para filtrar dados |
| DateKey |Texto |Chave exclusiva para cada item de dados |
| DayDiff |Número decimal |Diferença em dias para filtrar dados, por exemplo, 0 indica a data do dia atual, -1 indica dados do dia anterior um, 0 e -1 indicam dados para o dia atual e anterior  |
| Mês |Texto |Mês do ano selecionado para filtrar os dados, o mês começa no primeiro dia e termina a 31 dias |
| MonthDate | Date |Data no mês quando termina o mês, selecionada para filtrar os dados |
| MonthDiff |Número decimal |Diferença no mês para filtrar dados, por exemplo, 0 indica os dados do mês atual, -1 indica os dados do mês anterior, 0 e -1 indicam dados para o mês atual e anterior |
| Week (Semana) |Texto |Semana selecionado para filtrar dados semana começa no Domingo e termina no Sábado |
| WeekDate |Date |Data da semana, quando a semana termina, selecionada para filtrar os dados |
| WeekDiff |Número decimal |Diferença na semana para filtrar dados, por exemplo, 0 indica os dados da semana atual, -1 indica os dados da semana anterior, 0 e -1 indicam dados para a semana atual e anterior |
| Ano |Texto |Ano de calendário selecionado para filtrar os dados |
| YearDate |Date |Datas no ano quando termina o ano, selecionada para filtrar os dados |

### <a name="job"></a>Tarefa
Esta tabela fornece campos básicos e agregações ao longo de vários campos relacionados com tarefas.

| Campo | Tipo de Dados | Descrição |
| --- | --- | --- |
| #JobsCreatedInPeriod |Número inteiro |Número de tarefas criadas no período de tempo selecionado |
| % FailuresForJobsCreatedInPeriod |Percentagem |Percentagem de falhas em tarefas gerais no período de tempo selecionado |
| 80thPercentileDataTransferredInMBForBackupJobsCreatedInPeriod |Número decimal |valor de percentil 80th dos dados transferidos em MB para **cópia de segurança** tarefas criadas no período de tempo selecionado |
| AsOnDateTime |Data/Hora |Hora de atualização mais recente para a linha selecionada |
| AvgBackupDurationInMinsForJobsCreatedInPeriod |Número decimal |Tempo médio em minutos para que **cópia de segurança concluída** tarefas criadas no período de tempo selecionado |
| AvgRestoreDurationInMinsForJobsCreatedInPeriod |Número decimal |Tempo médio em minutos para que **concluir o restauro** tarefas criadas no período de tempo selecionado |
| BackupStorageDestination |Texto |Destino de cópias de segurança, por exemplo, a nuvem, disco  |
| EntityState |Texto |Estado atual do objeto de tarefa, por exemplo, Active Directory, eliminado |
| JobFailureCode |Texto |Cadeia de caracteres do código de falha devido a que ocorreu a falha da tarefa |
| JobOperation |Texto |Operação para o qual tarefa é executada por exemplo, cópia de segurança, restauro, configurar a cópia de segurança |
| JobStartDate |Date |Data de quando iniciar a tarefa em execução |
| JobStartTime |Hora |Tempo quando iniciar a tarefa em execução |
| Estado da Tarefa |Texto |Estado do trabalho concluído, por exemplo, concluído, com falhas |
| JobUniqueId |Texto |Id exclusivo para identificar a tarefa |

### <a name="policy"></a>Política
Esta tabela fornece campos básicos e agregações ao longo de vários campos relacionados com a política.

| Campo | Tipo de Dados | Descrição |
| --- | --- | --- |
| #Policies |Número inteiro |Número de políticas de cópia de segurança que existe no sistema |
| #PoliciesInUse |Número inteiro |Número de políticas atualmente a ser utilizado para configurar cópias de segurança |
| AsOnDateTime |Data/Hora |Hora de atualização mais recente para a linha selecionada |
| BackupDaysOfTheWeek |Texto |Dias da semana quando tiverem sido agendadas as cópias de segurança |
| BackupFrequency |Texto |Frequência com que as cópias de segurança são executadas, por exemplo, diariamente, semanalmente |
| BackupTimes |Texto |Data e hora quando estão agendadas cópias de segurança |
| DailyRetentionDuration |Número inteiro |Duração total de retenção em dias para cópias de segurança configurados |
| DailyRetentionTimes |Texto |Data e hora quando a retenção diária foi configurada |
| EntityState |Texto |Estado atual do objeto de política, por exemplo, Active Directory, eliminado |
| MonthlyRetentionDaysOfTheMonth |Texto |Datas do mês selecionado para a retenção mensal |
| MonthlyRetentionDaysOfTheWeek |Texto |Dias da semana selecionada para a retenção mensal |
| MonthlyRetentionDuration |Número decimal |Duração da retenção total nos meses para cópias de segurança configuradas |
| MonthlyRetentionFormat |Texto |Tipo de configuração de retenção mensal por exemplo, diariamente por dia, com base, semanalmente por semana com base |
| MonthlyRetentionTimes |Texto |Data e hora quando estiver configurada retenção mensal |
| MonthlyRetentionWeeksOfTheMonth |Texto |Semanas do mês quando o período de retenção mensal é configurado, por exemplo, primeiro, último etc. |
| PolicyName |Texto |Nome da política definida |
| PolicyUniqueId |Texto |Id exclusivo para identificar a política |
| RetentionType |Texto |Tipo de política de retenção por exemplo, diárias, semanais, mensais, anuais |
| WeeklyRetentionDaysOfTheWeek |Texto |Dias da semana selecionada para a retenção semanal |
| WeeklyRetentionDuration |Número decimal |Duração da retenção semanal total em semanas para cópias de segurança configuradas |
| WeeklyRetentionTimes |Texto |Data e hora quando estiver configurada retenção semanal |
| YearlyRetentionDaysOfTheMonth |Texto |Datas do mês selecionado para a retenção anual |
| YearlyRetentionDaysOfTheWeek |Texto |Dias da semana selecionada para a retenção anual |
| YearlyRetentionDuration |Número decimal |Duração da retenção total nos anos para cópias de segurança configurados |
| YearlyRetentionFormat |Texto |Tipo de configuração de retenção anual por exemplo, diariamente por dia, com base, semanalmente por semana com base |
| YearlyRetentionMonthsOfTheYear |Texto |Meses do ano selecionado para a retenção anual |
| YearlyRetentionTimes |Texto |Data e hora quando estiver configurada retenção anual |
| YearlyRetentionWeeksOfTheMonth |Texto |Semanas do mês quando o período de retenção anual é configurado, por exemplo, primeiro, último etc. |

### <a name="protected-server"></a>Servidor protegido
Esta tabela fornece campos básicos e agregações ao longo de vários campos protegidos relacionados a servidores.

| Campo | Tipo de Dados | Descrição |
| --- | --- | --- |
| #ProtectedServers |Número inteiro |Número de servidores protegidos |
| AsOnDateTime |Data/Hora |Hora de atualização mais recente para a linha selecionada |
| AzureBackupAgentOSType |Texto |Tipo de SO do agente de cópia de segurança do Azure |
| AzureBackupAgentOSVersion |Texto |Versão do SO do agente de cópia de segurança do Azure |
| AzureBackupAgentUpdateDate |Texto |Data de quando o agente de cópia de segurança do agente foi atualizado |
| AzureBackupAgentVersion |Texto |Número de versão de versão do agente de cópia de segurança |
| BackupManagementType |Texto |Tipo de fornecedor para a execução de cópia de segurança, por exemplo, IaaSVM, FileFolder |
| EntityState |Texto |Estado atual do objeto de servidor protegido, por exemplo, Active Directory, eliminado |
| ProtectedServerFriendlyName |Texto |Nome amigável do servidor protegido |
| ProtectedServerName |Texto |Nome do servidor protegido |
| ProtectedServerType |Texto |Tipo de servidor protegido backup IaaSVMContainer por exemplo, |
| ProtectedServerName |Texto |Nome do servidor protegido para o qual o item de cópia de segurança pertence |
| RegisteredContainerId |Texto |O ID de contentor registado para cópia de segurança |

### <a name="storage"></a>Armazenamento
Esta tabela fornece campos básicos e agregações ao longo de vários campos relacionados com o armazenamento.

| Campo | Tipo de Dados | Descrição |
| --- | --- | --- |
| #ProtectedInstances |Número decimal |Número de instâncias protegidas usada para calcular o armazenamento de front-end na faturação, calculada com base no valor mais recente no tempo selecionado |
| AsOnDateTime |Data/Hora |Hora de atualização mais recente para a linha selecionada |
| CloudStorageInMB |Número decimal |O armazenamento de cópia de segurança na cloud utilizados pelas cópias de segurança, calculadas com base no valor mais recente no tempo selecionado |
| EntityState |Texto |Estado atual do objeto, por exemplo, Active Directory, eliminado |
| LastUpdatedDate |Date |Data de quando linha selecionada foi atualizado pela última vez |

### <a name="time"></a>Hora
Esta tabela fornece detalhes sobre os campos relacionados com o tempo.

| Campo | Tipo de Dados | Descrição |
| --- | --- | --- |
| Hora |Hora |Hora do dia, por exemplo, 1: das 12:00:00 |
| HourNumber |Número decimal |Número de hora do dia, por exemplo, 13.00 |
| Minuto |Número decimal |Minuto da hora |
| PeriodOfTheDay |Texto |Ranhura de período de tempo no dia, por exemplo, 3 de 12 AM |
| Hora |Hora |Hora do dia, por exemplo, 12:00: 01 AM |
| TimeKey |Texto |Valor da chave para representar o tempo |

### <a name="vault"></a>Cofre
Esta tabela fornece campos básicos e agregações ao longo de vários campos relacionados com o cofre.

| Campo | Tipo de Dados | Descrição |
| --- | --- | --- |
| #Vaults |Número inteiro |Número de cofres |
| AsOnDateTime |Data/Hora |Hora de atualização mais recente para a linha selecionada |
| AzureDataCenter |Texto |Onde está localizado o Cofre de centro de dados |
| EntityState |Texto |Estado atual do objeto de cofre, por exemplo, Active Directory, eliminado |
| StorageReplicationType |Texto |Tipo de replicação de armazenamento para o cofre, por exemplo, GeoRedundant |
| SubscriptionId |Texto |Id de subscrição do cliente selecionado para a geração de relatórios |
| VaultName |Texto |Nome do Cofre |
| VaultTags |Texto |Marcas associadas ao Cofre |

## <a name="next-steps"></a>Passos Seguintes
Depois de rever o modelo de dados para a criação de relatórios do Azure Backup, consulte os artigos seguintes para obter mais detalhes sobre como criar e visualizar relatórios no Power BI.

* [Criar relatórios no Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-create-a-new-report/)
* [Filtragem de relatórios no Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-about-filters-and-highlighting-in-reports/)
