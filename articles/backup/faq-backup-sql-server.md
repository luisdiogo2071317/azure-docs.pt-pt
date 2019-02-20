---
title: Perguntas mais frequentes sobre o backup de bancos de dados do SQL Server em VMs do Azure com o Azure Backup
description: Fornece respostas a perguntas comuns sobre o backup de bancos de dados do SQL Server em VMs do Azure com o Azure Backup.
services: backup
author: sogup
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 8/16/2018
ms.author: sogup
ms.openlocfilehash: a14406733ff60d53d4bf7792ff0c9a015c57d9b3
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/20/2019
ms.locfileid: "56430781"
---
# <a name="faq-on-sql-server-running-on-azure-vm-backup"></a>FAQ sobre o SQL Server em execução na cópia de segurança de VM do Azure

Este artigo responde a perguntas comuns sobre o backup de bancos de dados do SQL Server em execução em VMs do Azure com o [Azure Backup](backup-overview.md) serviço.

> [!NOTE]
> Esta funcionalidade está atualmente em pré-visualização pública.



## <a name="can-i-throttle-the-backup-speed"></a>Pode limitar a velocidade de cópia de segurança?

Sim. Pode limitar a taxa em que a política de cópia de segurança é executado para minimizar o impacto numa instância do SQL Server. Para alterar a definição:
1. Na instância do SQL Server, na *pasta C:\Program c:\programas\azure carga de trabalho Backup\bin*, criar o **ExtensionSettingsOverrides.json** ficheiro.
2. Na **ExtensionSettingsOverrides.json** de ficheiros, alterar a **DefaultBackupTasksThreshold** na definição de um valor inferior (por exemplo, 5) <br>
  ` {"DefaultBackupTasksThreshold": 5}`

3. Guarde as alterações. Feche o ficheiro.
4. Na instância do SQL Server, abra **Gerenciador de tarefas**. Reinicie o **AzureWLBackupCoordinatorSvc** serviço.

## <a name="can-i-run-a-full-backup-from-a-secondary-replica"></a>Pode executar um backup completo de uma réplica secundária?
Não. Esta funcionalidade não é suportada.

## <a name="do-successful-backup-jobs-create-alerts"></a>Tarefas de cópia de segurança bem-sucedida criar alertas?

Não. Tarefas de cópia de segurança com êxito não geram alertas. Os alertas são enviados apenas para tarefas de cópia de segurança que não obedeçam.

## <a name="can-i-see-scheduled-backup-jobs-in-the-jobs-menu"></a>Pode ver tarefas de cópia de segurança agendadas no menu tarefas?

Não. O **tarefas de cópia de segurança** menu mostra detalhes da tarefa a pedido, mas não agendadas tarefas de cópia de segurança. Se as tarefas de cópia de segurança agendadas falharem, os detalhes estão disponíveis nos alertas de tarefa falhada. Para monitorizar todos os agendados e tarefas de cópia de segurança ad hoc, usar [SQL Server Management Studio](manage-monitor-sql-database-backup.md).

## <a name="are-future-databases-automatically-added-for-backup"></a>Bases de dados futuros são adicionados automaticamente para cópia de segurança?

Não. Ao configurar a proteção para uma instância do SQL Server, se selecionar a opção de nível de servidor, são adicionados a todas as bases de dados. Se adicionar bases de dados para uma instância do SQL Server depois de configurar a proteção, tem de adicionar manualmente as novas bases de dados para protegê-los. As bases de dados não são incluídos automaticamente na proteção configurada.

##  <a name="how-do-i-restart-protection-after-changing-recovery-type"></a>Como posso reiniciar a proteção depois de alterar o tipo de recuperação?

Acione uma cópia de segurança completa. Backups de log começarem conforme esperado.

## <a name="can-i-protect-availability-groups-on-premises"></a>Pode proteger grupos de disponibilidade no local?

Não. O Azure Backup protege os SQL Servers em execução no Azure. Se um grupo de disponibilidade (AG) é distribuído por entre as máquinas do Azure e no local, o AG pode ser protegido apenas se a réplica primária está em execução no Azure. Além disso, o Azure Backup protege apenas os nós em execução na mesma região do Azure que o Cofre dos serviços de recuperação.

## <a name="can-i-protect-availability-groups-across-regions"></a>Pode proteger grupos de disponibilidade em regiões?

Cofre de serviços de recuperação de cópia de segurança do Azure pode detetar e proteger todos os nós que estão na mesma região que o Cofre dos serviços de recuperação. Se tiver um SQL sempre no grupo de disponibilidade em diversas regiões do Azure, terá de configurar a cópia de segurança da região que tem o nó principal. Cópia de segurança do Azure será capaz de detetar e proteger todas as bases de dados no grupo de disponibilidade de acordo com a preferência de cópia de segurança. Se a preferência de cópia de segurança não for cumprida, as cópias de segurança irão falhar e receberá o alerta de falha.

## <a name="can-i-exclude-databases-with-auto-protection-enabled"></a>Pode excluir bases de dados com a proteção automática ativada?

Não, [proteção automática](backup-azure-sql-database.md#enable-auto-protection) aplica-se a toda a instância. Seletivamente não consegue proteger bases de dados uma instância com a proteção automática.

## <a name="can-i-have-different-policies-in-an-auto-protected-instance"></a>Pode ter políticas diferentes numa instância de proteção automática?

Se já tiver algumas bases de dados protegidos numa instância, irão continuar a ser protegidos com as respetivas políticas, mesmo após transformar **ON** a [proteção automática](backup-azure-sql-database.md#enable-auto-protection) opção. No entanto, todos os bancos de dados não protegidos, juntamente com os que adicionaria no futuro terão apenas uma única política que definir sob **configurar a cópia de segurança** depois das bases de dados estão selecionados. Na verdade, ao contrário de outras bases de dados protegidos, é possível até mesmo alterar a política para um banco de dados numa instância de proteção automática.
Se quiser fazer isso, é a única maneira de desativar a proteção automática na instância por enquanto e, em seguida, alterar a política para essa base de dados. Pode agora voltar a ativar a proteção automática desta instância.

## <a name="if-i-delete-a-database-from-auto-protection-will-backups-stop"></a>Se eu eliminar uma base de dados da proteção automática irão parar de cópias de segurança?

Não, se uma base de dados é removida de uma instância automaticamente protegida, ainda são tentadas as cópias de segurança nessa base de dados. Isso implica que a base de dados eliminada começa a aparecer como mau estado de funcionamento sob **itens de cópia de segurança** e ainda é tratado como protegido.

A única forma de parar de proteger esta base de dados é desabilitar o [proteção automática](backup-azure-sql-database.md#enable-auto-protection) na instância por enquanto e, em seguida, escolha a **parar cópia de segurança** opção em **itens de cópia de segurança**para essa base de dados. Pode agora voltar a ativar a proteção automática desta instância.

##  <a name="why-cant-i-see-an-added-database-for-an-auto-protected-instance"></a>Por que motivo não vejo uma base de dados foi adicionado para uma instância de proteção automática?

Poderá não ver uma base de dados adicionado recentemente uma [automaticamente protegida](backup-azure-sql-database.md#enable-auto-protection) itens protegidos de instância protegida instantaneamente em. Isto acontece porque a deteção, normalmente, é executada a cada 8 horas. No entanto, o utilizador pode executar uma deteção manual ao utilizar **bds recuperar** opção para detetar e proteger novos bancos de dados imediatamente, como mostra a abaixo da imagem:

  ![Exibir banco de dados adicionados recentemente](./media/backup-azure-sql-database/view-newly-added-database.png)

## <a name="next-steps"></a>Passos Seguintes

[Saiba como](backup-azure-sql-database.md) cópia de segurança de uma base de dados do SQL Server em execução numa VM do Azure.
