---
title: Monitorizar alertas de cópias de segurança para máquinas virtuais do Azure
description: Monitorizar eventos e alertas das tarefas de cópia de segurança de máquina virtual do Azure. Envie correio eletrónico com base em alertas.
services: backup
author: markgalioto
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 02/23/2018
ms.author: markgal
ms.openlocfilehash: 3783014738ec4e8f185531773b1259dc63e7f49f
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34606312"
---
# <a name="monitor-alerts-for-azure-virtual-machine-backups"></a>Monitorizar alertas de cópias de segurança de máquinas virtuais do Azure
Os alertas são as respostas do serviço do que um limiar de evento foi alcançado ou ultrapassado. Saber quando o início de problemas pode ser fundamental para manter os custos de negócio para baixo. Normalmente, não ocorrerem os alertas com base numa agenda e por isso, é útil logo que possível saber depois de ocorrerem os alertas. Por exemplo, quando uma tarefa de cópia de segurança ou restauro falhar, um alerta ocorre dentro de cinco minutos da falha. No dashboard do cofre, o mosaico alertas de cópia de segurança apresenta os eventos críticos e de nível de aviso. Nas definições de alertas de cópia de segurança, pode ver todos os eventos. Mas, o que pode fazer se um alerta ocorre quando estiver a trabalhar num problema separado? Se não souber quando o alerta acontece, pode ser uma inconveniência secundária ou comprometer dados. Para garantir que as pessoas corretas sabem de um alerta - quando ocorrer, configure o serviço para enviar notificações de alerta por e-mail. Para obter mais informações sobre como configurar notificações por e-mail, consulte [configurar notificações](backup-azure-monitor-vms.md#configure-notifications).

## <a name="how-do-i-find-information-about-the-alerts"></a>Como posso encontrar informações sobre os alertas?
Para ver informações sobre o evento que acionou um alerta, tem de abrir a secção alertas de cópia de segurança. Existem duas formas para abrir a secção alertas de cópia de segurança: os alertas de cópia de segurança a partir de mosaico no dashboard do cofre ou a partir da secção de eventos e alertas.

Para abrir o painel Alertas de cópia de segurança a partir do mosaico alertas de cópia de segurança:

* No **alertas de cópia de segurança** mosaico no dashboard do cofre, clique em **crítico** ou **aviso** para ver os eventos operacionais para esse nível de gravidade.

    ![Mosaico alertas de cópias de segurança](./media/backup-azure-monitor-vms/backup-alerts-tile.png)

Para abrir o painel Alertas de cópia de segurança da secção de eventos e alertas:

1. No dashboard do cofre, clique em **todas as definições**. ![Botão de todas as definições](./media/backup-azure-monitor-vms/all-settings-button.png)
2. No **definições** painel, clique em **eventos e alertas**. ![Botão de eventos e alertas](./media/backup-azure-monitor-vms/alerts-and-events-button.png)
3. No **eventos e alertas** painel, clique em **alertas de cópia de segurança**. ![Botão de alertas de cópia de segurança](./media/backup-azure-monitor-vms/backup-alerts.png)

    O **alertas de cópia de segurança** secção abre e apresenta os alertas filtrados.

    ![Mosaico alertas de cópias de segurança](./media/backup-azure-monitor-vms/backup-alerts-critical.png)
4. Para ver informações detalhadas sobre um alerta específico, da lista de eventos, clique no alerta para abrir o respetivo **detalhes** secção.

    ![Detalhe do Evento](./media/backup-azure-monitor-vms/audit-logs-event-detail.png)

    Para personalizar os atributos apresentados na lista, consulte [ver atributos eventos adicionais](backup-azure-monitor-vms.md#view-additional-event-attributes)

## <a name="configure-notifications"></a>Configurar notificações
 Pode configurar o serviço para enviar notificações por e-mail para os alertas que ocorreram durante a hora anterior, ou quando ocorrem tipos específicos de eventos.

Para configurar notificações por e-mail para alertas

1. No menu de alertas de cópia de segurança, clique em **configurar notificações**

    ![Menu de alertas de cópia de segurança](./media/backup-azure-monitor-vms/backup-alerts-menu.png)

    Abre a secção de notificações de configuração.

    ![Configurar o painel de notificações](./media/backup-azure-monitor-vms/configure-notifications.png)
2. Na secção de notificações de configuração, para notificações por E-Mail, clique em **no**.

    Os destinatários e a gravidade caixas de diálogo tem uma estrela junto-lhes porque essas informações são necessárias. Fornecer, pelo menos, um endereço de e-mail e selecione, pelo menos, uma gravidade.
3. No **destinatários (E-Mail)** caixa de diálogo, escreva os endereços de e-mail para que receber as notificações. Utilize o formato: username@domainname.com. Separe vários endereços de e-mail com um ponto e vírgula (;).
4. No **notificar** área, escolha **por alerta** enviar a notificação quando ocorre o alerta especificado, ou **resumida horária** para enviar um resumo para a hora anterior.
5. No **gravidade** caixa de diálogo, escolha um ou mais níveis que quiser acionar a notificação por correio eletrónico.
6. Clique em **Guardar**.

   ### <a name="what-alert-types-are-available-for-azure-iaas-vm-backup"></a>Que tipos de alertas estão disponíveis para cópia de segurança de VM do IaaS do Azure?
   | Nível de alerta | Alertas enviados |
   | --- | --- |
   | Crítica | Falha de cópia de segurança, falha de recuperação |
   | Aviso | para as tarefas de cópia de segurança efetuada com êxito com avisos (por exemplo: alguns escritores falharam ao criar um instantâneo) |
   | Informativo | Atualmente, não existem alertas informativos estão disponíveis para cópia de segurança de VM do Azure |

### <a name="are-there-situations-where-email-isnt-sent-even-if-notifications-are-configured"></a>Existem situações em que um e-mail não é enviado, mesmo que as notificações estejam configuradas?
Existem situações em que não é enviado um alerta, apesar das notificações foi configuradas corretamente. No e-mail de situações seguintes não são enviadas notificações para evitar o ruído de alertas:

* Se as notificações estão configuradas para texto implícita por hora e um alerta é gerado e resolvido dentro da hora.
* A tarefa foi cancelada.
* Uma tarefa de cópia de segurança é acionada e, em seguida, falha e está em curso outra tarefa de cópia de segurança.
* Inicia uma tarefa de cópia de segurança agendada para uma VM preparados no Gestor de recursos, mas já não existe a VM.

## <a name="using-activity-logs-to-get-notifications-for-successful-backups"></a>Utilizar registos de atividade para obter notificações para cópias de segurança com êxito

Se pretender ser notificado quando as cópias de segurança foram bem sucedidas, pode utilizar alertas criadas a [registos de atividade](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-audit) do cofre.

### <a name="login-into-azure-portal"></a>Início de sessão no portal do Azure
Início de sessão no portal do Azure e avance para o Cofre dos serviços de recuperação do Azure relevante e clique na secção "Registo de atividade" nas propriedades.

### <a name="identify-appropriate-log"></a>Identificar o registo adequado

Aplica os filtros mostrados na imagem seguinte para verificar se estão a receber os registos de atividade para cópias de segurança com êxito. Altere o período de tempo em conformidade para ver registos.

![Registos de Atividade](./media/backup-azure-monitor-vms/activity-logs-identify.png)

Pode clicar o segmento de "JSON" para obter mais detalhes e visualizá-lo ao copiar-colá-lo no editor de texto. Se deve apresentar os detalhes do cofre e o item que acionou o registo de atividade ou seja, o item de cópia de segurança.

Em seguida, clique em "Adicionar o alerta de registo de atividade" para gerar alertas para todos os registos deste tipo.

### <a name="add-activity-log-alert"></a>Adicionar o alerta de registo de atividade

Ao clicar em "Adicionar alerta de registo de atividade" irá mostrar-lhe um ecrã conforme mostrado abaixo

![Alerta do registo de atividade](./media/backup-azure-monitor-vms/activity-logs-alerts-successful.png)
    
A subscrição e o grupo de recursos são utilizados para armazenar o alerta. Os critérios serão preenchidas previamente. Certifique-se de que todos os valores são relevantes para o requisito.

Para cópias de segurança com êxito, o nível está assinalado como estado como "Com êxito" e "Informativa".

Se selecionar um "recurso" acima, o alerta será gerado quando os registos de atividade são registados para esse recurso ou o cofre. Se pretender que a regra ser aplicáveis a todos os cofres, deixe "recursos" para estar em branco.

### <a name="define-action-on-alert-firing"></a>Definir ação no alerta acionadas

Utilize o "grupo de ação" para definir a ação depois de gerar um alerta. Pode clicar em "Tipo de ação" para saber mais sobre as ações disponíveis esse e-mail/SMS/integração com ITSM etc.

![Grupo de ação de registo de atividade](./media/backup-azure-monitor-vms/activity-logs-alerts-action-group.png)


Assim que clicar em OK, será gerado um alerta de registo de atividade e os registos de atividade subsequente registados para cópias de segurança com êxito serão acionados a ação, tal como definido no grupo de ação.

### <a name="limitations-on-alerts"></a>Limitações de alertas
Alertas com base em eventos estão sujeitos às seguintes limitações:

1. Alertas são acionados em todas as máquinas virtuais no cofre dos serviços de recuperação. Não é possível personalizar o alerta para um subconjunto de máquinas virtuais num cofre dos serviços de recuperação.
2. Os alertas são enviados a partir de "alerts-noreply@mail.windowsazure.com". Atualmente não é possível modificar o remetente do e-mail.

## <a name="next-steps"></a>Passos Seguintes
Para obter informações sobre como voltar a criar uma máquina virtual a partir de um ponto de recuperação, consulte [restaurar VMs do Azure](backup-azure-arm-restore-vms.md).

Se precisar de informações sobre a proteção de máquinas virtuais, consulte o artigo [primeiras impressões: cópia de segurança VMs para um cofre dos serviços de recuperação](backup-azure-vms-first-look-arm.md). 

Saiba mais sobre as tarefas de gestão para cópias de segurança VM no artigo, [cópias de segurança do Azure de gerir máquinas virtuais](backup-azure-manage-vms.md).
