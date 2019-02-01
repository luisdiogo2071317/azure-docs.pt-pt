---
title: Gerir cópias de segurança de máquinas de virtuais implementadas no Resource Manager
description: Saiba como gerir e monitorizar cópias de segurança de máquinas de virtuais implementadas no Resource Manager
services: backup
author: sogup
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 11/28/2016
ms.author: sogup
ms.openlocfilehash: b51cf5611bf00c133378a7d90d1693e50b27f7b0
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/31/2019
ms.locfileid: "55491584"
---
# <a name="manage-azure-virtual-machine-backups"></a>Gerir cópias de segurança de máquinas virtuais do Azure

Este artigo fornece orientações sobre como gerir cópias de segurança VM e explica as informações de alertas de cópia de segurança disponíveis no dashboard do portal. As orientações neste artigo aplica-se a utilização de VMs com os cofres dos serviços de recuperação. Este artigo não abrange a criação de máquinas virtuais, nem explica como proteger máquinas virtuais. Para obter um manual sobre a proteção de VMs implementadas no Azure Resource Manager no Azure com um cofre dos serviços de recuperação, consulte [primeira impressão: Cópia de segurança de VMs para um cofre dos serviços de recuperação](backup-azure-vms-first-look-arm.md).

## <a name="manage-vaults-and-protected-virtual-machines"></a>Gerir cofres e máquinas virtuais protegidas
No portal do Azure, o dashboard do Cofre de serviços de recuperação fornece acesso a informações sobre o cofre, incluindo:

* o instantâneo de cópia de segurança mais recente, que também é o ponto de restauro mais recente
* a política de cópia de segurança
* tamanho total de todos os instantâneos de cópia de segurança
* número de máquinas virtuais que estão protegidos com o Cofre

Muitas tarefas de gestão com uma cópia de segurança de máquina virtual para começar abrindo o Cofre no dashboard. No entanto, uma vez que cofres podem ser utilizados para proteger vários itens (ou várias VMs), para ver detalhes sobre uma VM específica, abra o dashboard de item do cofre. O procedimento seguinte mostra como abrir o *dashboard do cofre* e, em seguida, avance para o *dashboard do cofre item*. Existem "sugestões" em ambos os procedimentos que indicam como adicionar o Cofre e ao utilizar o Pin para o comando de dashboard do cofre item ao dashboard do Azure. Afixar ao dashboard é uma forma de criar um atalho para o cofre ou o item. Também pode executar comandos comuns a partir do atalho.

> [!TIP]
> Se tiver vários dashboards e são abertos os painéis, utilize o controlo de deslize de azul escuro na parte inferior da janela para o dashboard do Azure para a frente e para trás, faça deslizar.
>
>

![Vista completa com controlo de deslize](./media/backup-azure-manage-vms/bottom-slider.png)

### <a name="open-a-recovery-services-vault-in-the-dashboard"></a>Abra um cofre dos serviços de recuperação no dashboard:
1. Inicie sessão no [portal do Azure](https://portal.azure.com/).
2. No menu Hub, clique em **Procurar** e na lista de recursos, escreva **Serviços de Recuperação**. À medida que começa a escrever, a lista filtra com base na sua entrada. Clique em **Cofre dos Serviços de Recuperação**.

    ![Passo 1 da Criação de um Cofre dos Serviços de Recuperação](./media/backup-azure-manage-vms/browse-to-rs-vaults.png)

    É apresentada a lista dos cofres dos Serviços de Recuperação.

    ![Cofres de lista de serviços de recuperação ](./media/backup-azure-manage-vms/list-o-vaults.png)

   > [!TIP]
   > Se afixar um cofre para o Dashboard do Azure, esse cofre é imediatamente acessível ao abrir o portal do Azure. Para afixar um cofre para o dashboard, na lista de cofre, clique com o botão direito do cofre e selecione **afixar ao dashboard**.
   >
   >
3. Na lista de cofres, selecione o cofre para abrir o dashboard. Quando seleciona o cofre, o dashboard do cofre e o **definições** painel aberto. Na imagem seguinte, o **Contoso-cofre** dashboard é realçado.

    ![Abra o dashboard do cofre e o painel de definições](./media/backup-azure-manage-vms/full-view-rs-vault.png)

### <a name="open-a-vault-item-dashboard"></a>Abra um dashboard do item de cofre
No procedimento anterior tiver aberto o dashboard do cofre. Para abrir o dashboard de item de cofre:

1. No dashboard do cofre, sobre o **itens de cópia de segurança** mosaico, clique em **máquinas virtuais do Azure**.

    ![Mosaico de abrir itens de cópia de segurança](./media/backup-azure-manage-vms/contoso-vault-1606.png)

    O **itens de cópia de segurança** painel lista a última tarefa de cópia de segurança para cada item. Neste exemplo, existe uma máquina virtual, demovm-markgal, protegida por este cofre.  

    ![Mosaico de itens de cópia de segurança](./media/backup-azure-manage-vms/backup-items-blade.png)

   > [!TIP]
   > Para facilitar o acesso, pode afixar um item de cofre para o Dashboard do Azure. Para afixar um item de cofre, na lista de itens de cofre, o item com o botão direito e selecione **afixar ao dashboard**.
   >
   >
2. Na **itens de cópia de segurança** painel, clique no item para abrir o dashboard de item do cofre.

    ![Mosaico de itens de cópia de segurança](./media/backup-azure-manage-vms/backup-items-blade-select-item.png)

    Dashboard de item do cofre e a respetiva **definições** painel aberto.

    ![Dashboard de itens de cópia de segurança com o painel de definições](./media/backup-azure-manage-vms/item-dashboard-settings.png)

    Do dashboard de item do cofre, pode fazer muitas das tarefas principais, tais como:

   * alterar as políticas ou crie uma nova política de cópia de segurança
   * ver pontos de restauro e ver o estado de consistência
   * cópia de segurança a pedido de uma máquina virtual
   * parar a proteção de máquinas virtuais
   * retomar a proteção de uma máquina virtual
   * eliminar um dados de cópia de segurança (ou um ponto de recuperação)
   * [restaurar discos de cópia de segurança](backup-azure-arm-restore-vms.md#create-new-restore-disks)

Para obter os procedimentos seguintes, o ponto de partida é o dashboard de item de cofre.

## <a name="manage-backup-policies"></a>Gerir políticas de cópia de segurança
1. Na [dashboard de item do cofre](backup-azure-manage-vms.md#open-a-vault-item-dashboard), clique em **todas as definições** para abrir o **definições** painel.

    ![Painel de política de cópia de segurança](./media/backup-azure-manage-vms/all-settings-button.png)
2. Sobre o **configurações** painel, clique em **política de cópia de segurança** para abrir o painel.

    No painel, são apresentados os detalhes de intervalo de frequência e a retenção de cópia de segurança.

    ![Painel de política de cópia de segurança](./media/backup-azure-manage-vms/backup-policy-blade.png)
3. Partir do **escolher política de cópia de segurança** menu:

   * Para alterar as políticas, selecione uma política diferente e clique em **guardar**. A nova política é imediatamente aplicada ao cofre.
   * Para criar uma política, selecione **criar novo**.

     ![Cópia de segurança da máquina virtual](./media/backup-azure-manage-vms/backup-policy-create-new.png)

     Para obter instruções sobre como criar uma política de cópia de segurança, consulte [definir uma política de cópia de segurança](backup-azure-manage-vms.md#defining-a-backup-policy).

[!INCLUDE [backup-create-backup-policy-for-vm](../../includes/backup-create-backup-policy-for-vm.md)]

> [!NOTE]
> Durante a gestão de políticas de cópia de segurança, certifique-se seguir a [melhores práticas](backup-azure-vms-introduction.md#best-practices) para o melhor desempenho de cópia de segurança
>
>

## <a name="on-demand-backup-of-a-virtual-machine"></a>cópia de segurança a pedido de uma máquina virtual
Pode demorar uma demanda cópia de segurança de uma máquina virtual, uma vez que é configurado para proteção. Se a cópia de segurança inicial estiver pendente, a cópia de segurança a pedido cria uma cópia completa da máquina virtual no cofre dos serviços de recuperação. Se a cópia de segurança inicial estiver concluída, uma cópia de segurança a pedido irá enviar apenas alterações de instantâneo anterior, para o Cofre dos serviços de recuperação. Ou seja, cópias de segurança subsequentes são sempre incrementais.

> [!NOTE]
> O período de retenção para uma cópia de segurança a pedido é o valor de retenção especificado para o ponto de cópia de segurança diário na política. Não se for selecionado não existe nenhum ponto de cópia de segurança diário, em seguida, é utilizado o ponto de cópia de segurança semanal.
>
>

Para acionar uma cópia de segurança a pedido de uma máquina virtual:

* Sobre o [dashboard de item do cofre](backup-azure-manage-vms.md#open-a-vault-item-dashboard), clique em **agora a cópia de segurança**.

    ![Cópia de segurança agora botão](./media/backup-azure-manage-vms/backup-now-button.png)

    O portal torna-se de que deseja iniciar uma tarefa de cópia de segurança a pedido. Clique em **Sim** para iniciar a tarefa de cópia de segurança.

    ![Cópia de segurança agora botão](./media/backup-azure-manage-vms/backup-now-check.png)

    O trabalho de cópia de segurança cria um ponto de recuperação. O período de retenção do ponto de recuperação é o mesmo que o intervalo de retenção especificado na política de associados à máquina virtual. Para acompanhar o progresso para a tarefa, no dashboard do cofre, clique nas **tarefas de cópia de segurança** mosaico.  

## <a name="stop-protecting-virtual-machines"></a>parar a proteção de máquinas virtuais
Se optar por parar a proteção de uma máquina virtual, é-lhe perguntado se pretende manter os pontos de recuperação. Existem duas formas de parar a proteção de máquinas virtuais:

* parar todas as tarefas de cópia de segurança futuras e eliminar todos os pontos de recuperação, ou
* parar todas as tarefas de cópia de segurança futuras, mas deixar os pontos de recuperação

Existe um custo associado a deixar os pontos de recuperação no armazenamento. No entanto, a vantagem de deixar os pontos de recuperação é que pode restaurar a máquina virtual mais tarde, se assim o desejar. Para obter informações sobre o custo de deixar os pontos de recuperação, consulte a [os detalhes dos preços](https://azure.microsoft.com/pricing/details/backup/). Se optar por eliminar todos os pontos de recuperação, não é possível restaurar a máquina virtual.

Para parar a proteção para uma máquina virtual:

1. Sobre o [dashboard de item do cofre](backup-azure-manage-vms.md#open-a-vault-item-dashboard), clique em **parar cópia de segurança**.

    ![Parar o botão de cópia de segurança](./media/backup-azure-manage-vms/stop-backup-button.png)

    É aberto o painel de parar cópia de segurança.

    ![Parar o painel cópia de segurança](./media/backup-azure-manage-vms/stop-backup-blade.png)
2. Sobre o **parar cópia de segurança** painel, escolha se pretende manter ou eliminar os dados de cópia de segurança. A caixa de informações fornece detalhes sobre a sua escolha.

    ![Parar proteção](./media/backup-azure-manage-vms/retain-or-delete-option.png)
3. Se optar por manter os dados de cópia de segurança, avance para o passo 4. Se optar por eliminar os dados de cópia de segurança, confirme que pretende parar as tarefas de cópia de segurança e elimine os pontos de recuperação - escreva o nome do item.

    ![Parar de verificação](./media/backup-azure-manage-vms/item-verification-box.png)

    Se não tiver a certeza do nome do item, Paire o rato sobre o ponto de exclamação para ver o nome. Além disso, o nome do item está sob **parar cópia de segurança** na parte superior do painel.
4. Opcionalmente, indique um **motivo** ou **comentário**.
5. Para parar a tarefa de cópia de segurança para o item atual, clique em ![botão Parar de cópia de segurança](./media/backup-azure-manage-vms/stop-backup-button-blue.png)

    Uma mensagem de notificação permite-lhe saber que as tarefas de cópia de segurança foram paradas.

    ![Confirmar pare a proteção](./media/backup-azure-manage-vms/stop-message.png)

## <a name="resume-protection-of-a-virtual-machine"></a>retomar a proteção de uma máquina virtual
Se o **reter dados de cópia de segurança** opção foi escolhida quando a proteção da máquina virtual foi parada, em seguida, é possível retomar a proteção. Se o **eliminar dados de cópia de segurança** foi escolhida a opção, em seguida, não é possível retomar a proteção da máquina virtual.

Para retomar a proteção da máquina virtual

1. Sobre o [dashboard de item do cofre](backup-azure-manage-vms.md#open-a-vault-item-dashboard), clique em **retomar cópia de segurança**.

    ![Retomar a proteção](./media/backup-azure-manage-vms/resume-backup-button.png)

    É aberto o painel de política de cópia de segurança.

   > [!NOTE]
   > Quando voltar a proteger a máquina virtual, pode escolher uma política diferente que a política com a qual máquina virtual foi protegida inicialmente.
   >
   >
2. Siga os passos em [gerir políticas de cópia de segurança](backup-azure-manage-vms.md#manage-backup-policies) para atribuir a política para a máquina virtual.

    Assim que é aplicada a política de cópia de segurança para a máquina virtual, verá a seguinte mensagem.

    ![VM protegida com êxito](./media/backup-azure-manage-vms/success-message.png)

## <a name="delete-backup-data"></a>Eliminar Dados de cópia de segurança
Pode eliminar os dados de cópia de segurança associados a uma máquina virtual durante a **parar cópia de segurança** tarefa, ou em qualquer altura após a cópia de segurança tarefa for concluída. Até pode ser vantajoso aguardar dias ou semanas antes de eliminar os pontos de recuperação. Ao contrário de restaurar pontos de recuperação, ao eliminar dados de cópia de segurança, não pode escolher pontos de recuperação específicos para eliminar. Se optar por eliminar os dados de cópia de segurança, tem de eliminar todos os pontos de recuperação associados ao item.

O procedimento seguinte assume que a tarefa de cópia de segurança para a máquina virtual foi parada ou desabilitada. Assim que a tarefa de cópia de segurança estiver desativada, o **retomar cópia de segurança** e **Eliminar cópia de segurança** opções estão disponíveis no dashboard do item do cofre.

![Botões de retomar e delete](./media/backup-azure-manage-vms/resume-delete-buttons.png)

Para eliminar dados de cópia de segurança numa máquina virtual com o *desativado de cópia de segurança*:

1. Sobre o [dashboard de item do cofre](backup-azure-manage-vms.md#open-a-vault-item-dashboard), clique em **Eliminar cópia de segurança**.

    ![Tipo de VM](./media/backup-azure-manage-vms/delete-backup-buttom.png)

    O **eliminar dados de cópia de segurança** é aberto o painel.

    ![Tipo de VM](./media/backup-azure-manage-vms/delete-backup-blade.png)
2. Escreva o nome do item para confirmar que pretende eliminar os pontos de recuperação.

    ![Parar de verificação](./media/backup-azure-manage-vms/item-verification-box.png)

    Se não tiver a certeza do nome do item, Paire o rato sobre o ponto de exclamação para ver o nome. Além disso, o nome do item está sob **eliminar dados de cópia de segurança** na parte superior do painel.
3. Opcionalmente, indique um **motivo** ou **comentário**.
4. Para eliminar os dados de cópia de segurança para o item atual, clique em ![botão Parar de cópia de segurança](./media/backup-azure-manage-vms/delete-button.png)

    Uma mensagem de notificação permite-lhe saber que os dados de cópia de segurança foi eliminados.

## <a name="next-steps"></a>Passos Seguintes
Para informações sobre como voltar a criar uma máquina virtual a partir de um ponto de recuperação, verifique [restaurar VMs do Azure](backup-azure-arm-restore-vms.md). Se precisar de informações sobre como proteger suas máquinas virtuais, consulte o artigo [primeira impressão: Cópia de segurança de VMs para um cofre dos serviços de recuperação](backup-azure-vms-first-look-arm.md). Para obter informações sobre eventos de monitorização, consulte [monitorizar alertas para cópias de segurança da máquina virtual do Azure](backup-azure-monitor-vms.md).
