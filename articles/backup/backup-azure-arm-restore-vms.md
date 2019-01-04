---
title: 'Cópia de segurança do Azure: Restaurar máquinas virtuais com o portal do Azure'
description: Restaurar uma máquina virtual do Azure a partir de um ponto de recuperação com o portal do Azure
services: backup
author: geethalakshmig
manager: vijayts
keywords: restaurar cópia de segurança; como restaurar; ponto de recuperação;
ms.service: backup
ms.topic: conceptual
ms.date: 09/04/2017
ms.author: geg
ms.openlocfilehash: ac0c727e41ad9b361ea9f558a97f16446c12ef0e
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/27/2018
ms.locfileid: "53793381"
---
# <a name="use-the-azure-portal-to-restore-virtual-machines"></a>Utilizar o portal do Azure para restaurar máquinas virtuais
Proteger os seus dados através de instantâneos dos seus dados em intervalos definidos. Estes instantâneos são conhecidos como pontos de recuperação e estão armazenados nos cofres dos serviços de recuperação. Se for necessário reparar ou recriar uma máquina virtual (VM), pode restaurar a VM a partir de qualquer um dos pontos de recuperação guardado. Quando restaurar um ponto de recuperação, pode:

* Crie uma nova VM: De um ponto de restauro para ponto anterior no tempo da sua VM de cópia de segurança.
* Restaure discos: Utilizar o modelo que é fornecido com o processo para personalizar a VM restaurada ou fazer uma recuperação de arquivos individuais.

Este artigo explica como restaurar uma VM para uma nova VM ou restaurar todos os discos de cópia de segurança. Para a recuperação de arquivos individuais, consulte [recuperar ficheiros a partir de uma cópia de segurança de VM do Azure](backup-azure-restore-files-from-vm.md).

![Três formas de restaurar a partir de cópia de segurança VM](./media/backup-azure-arm-restore-vms/azure-vm-backup-restore.png)


Restaurar uma VM ou todos os discos da VM backup envolve dois passos:

* Selecione um ponto de restauro para restauro.
* Selecione o tipo de restauro
    - Opção 1: Criar uma nova VM
    - Opção 2: Restaure discos.

## <a name="select-a-restore-point-for-restore"></a>Selecione um ponto de restauro para o restauro
1. Inicie sessão no [portal do Azure](http://portal.azure.com/).

2. No menu do Azure, selecione **todos os serviços**. Na lista de serviços, escreva **serviços de recuperação** ou aceda ao **armazenamento** em que o **cofres do serviço de recuperação dos** é listada, selecione.

    ![Cofre dos Serviços de Recuperação](./media/backup-azure-arm-restore-vms/open-recovery-services-vault1.png)

3. É apresentada a lista de cofres na subscrição.

    ![Cofres de lista de serviços de recuperação](./media/backup-azure-arm-restore-vms/list-of-rs-vaults1.png)

4. Na lista de cofres de serviços de recuperação, selecione o Cofre associado à VM que pretende restaurar. Quando selecionar o cofre, é aberto o dashboard do mesmo.

    ![Selecionar cofre dos serviços de recuperação](./media/backup-azure-arm-restore-vms/select-vault-open-vault-blade1.png)

5. No dashboard do cofre, sobre o **itens de cópia de segurança** mosaico, selecione **Máquina Virtual do Azure**.

    ![Dashboard do Cofre](./media/backup-azure-arm-restore-vms/vault-dashboard1.png)

6. O **itens de cópia de segurança** é aberto o painel com a lista de VMs do Azure.

    ![Lista de VMs no Cofre](./media/backup-azure-arm-restore-vms/list-of-vms-in-vault1.png)

7. Na lista, selecione uma VM para abrir o dashboard. Dashboard de VM abre-se para a área de monitorização, que contém o **pontos de recuperação**. Como todas as operações de nível de VM **cópia de segurança agora**, **recuperação de ficheiros**, **parar cópia de segurança** podem ser executadas a partir deste painel.
Restauro pode ser executado em muitos aspetos a partir deste painel. Tenha em atenção que este painel lista apenas os últimos 30 dias pontos de recuperação.

    - Para restaurar com um ponto de restauro dos últimos 30 dias, com o botão direito a VM > **restaurar a VM**.
    - Para restaurar com um ponto de restauro mais antigo do que 30 dias, clique na ligação na **pontos de restauro**.
    - Para listar e filtrar VMs com datas personalizadas, clique em **restaurar a VM** no menu. Utilize o filtro para modificar o intervalo de tempo para os pontos de restauro apresentada. Também pode filtrar sobre diferentes tipos de consistência dos dados.
8. Reveja as definições do ponto de restauro:
    - Mostra de consistência de dados do [tipo de consistência](backup-azure-vms-introduction.md#consistency-types) no ponto de recuperação.
    - O **tipo de recuperação** mostra onde o ponto é armazenado (numa conta de armazenamento, o cofre, ou ambos. [Saiba mais](https://azure.microsoft.com/blog/large-disk-support/) sobre pontos de recuperação instantânea.

  ![Pontos de restauro](./media/backup-azure-arm-restore-vms/vm-blade1.png)
9. Selecione um ponto de restauro.

10. Selecione **restauro da configuração**. O **restauro da configuração** é aberto o painel.

## <a name="choose-a-vm-restore-configuration"></a>Escolha uma configuração de restauro de VM
Depois de selecionar o ponto de restauro, escolha uma configuração de restauro de VM. Para configurar a cm restaurada, pode utilizar o portal do Azure ou o PowerShell.

### <a name="restore-options"></a>Opções de restauro

**Opção** | **Detalhes**
--- | ---
**Criar novo-criar VM** | Equivalente a criação rápida de uma VM. Obtém uma VM básica em execução de um ponto de restauro.<br/><br/> As definições de VM podem ser modificadas como parte do processo de restauro.
**Criar o disco novo restauro** | Cria uma VM que pode personalizar como parte do processo de restauro.<br/><br/> Esta opção copia o VHD para a conta de armazenamento especificada.<br/><br/> -A conta de armazenamento deve estar na mesma localização que o cofre.<br/><br/> Se não tiver uma conta de armazenamento adequado que tem de criar um.<br/><br/> O tipo de replicação de conta de armazenamento é mostrado em parênteses. Armazenamento com redundância de zona (ZRS) não é suportado.<br/><br/> Da conta de armazenamento pode anexar os discos restaurados para uma VM existente ou crie uma nova VM a partir de discos restaurados, com o PowerShell.
**Substituir a existente** | Com esta opção não terá de criar uma VM.<br/><br/> A VM atual tem de existir. Se é eliminado esta opção não pode ser utilizada.<br/><br/> O Azure Backup tira um instantâneo da VM existente e armazena-a no local temporário especificado. Os discos existentes ligados à VM, em seguida, são substituídos pelo ponto de restauro selecionado. O instantâneo criado anteriormente é copiado para o Cofre e armazenado como um ponto de recuperação de acordo com a política de retenção de cópia de segurança.<br/><br/> Substituir existente é suportada para VMs geridas não encriptadas. Não é suportado para discos não geridos, [generalizada VMs](https://docs.microsoft.com/azure/virtual-machines/windows/capture-image-resource), ou para VMs [criado usando imagens personalizadas](https://azure.microsoft.com/resources/videos/create-a-custom-virtual-machine-image-in-azure-resource-manager-with-powershell/).<br/><br/> Se o ponto de restauro mais ou menos tem discos que a VM atual, o número de discos no ponto de restauro apenas irá refletir a VM.

> [!NOTE]
> Se quiser restaurar uma VM com definições de rede, por exemplo, se eles sejam geridos por um balanceador de carga interno ou externo ou tem várias NICs ou vários endereços IP reservados, avançadas restaure com o PowerShell. [Saiba mais](#restore-vms-with-special-network-configurations).
> Se uma VM do Windows utiliza [licenciamento do HUB](../virtual-machines/windows/hybrid-use-benefit-licensing.md), utilize o **criar nova restauro disco** opção e, em seguida, utilizar o PowerShell ou o modelo de restauro para criar a VM com **tipo de licença** definido como **Windows_Server**. Também pode ser aplicada esta definição depois de criar a VM.


Especifique o restauro de definição da seguinte forma:

1. Na **restaurar**, selecione um [ponto de restauro](#select-a-restore-point-for-restore) > **configuração do restauro**.
2. Na **restauro da configuração**, selecione como pretende restaurar a VM em conformidade com as definições resumidas na tabela acima.

    ![Restaurar o Assistente de configuração](./media/backup-azure-arm-restore-vms/restore-configuration-create-new1.png)


## <a name="create-new-create-a-vm"></a>Criar novo-criar uma VM

1. Na **configuração do restauro** > **criar novo** > **restaurar tipo**, selecione **criar uma máquina virtual** .
2. Na **nome da Máquina Virtual**, especifique uma VM que não existe na subscrição.
3. Na **grupo de recursos**, selecione o grupo de recursos existente para a nova VM, ou criar um novo com um nome globalmente exclusivo. Se atribuir um nome que já existentes, o Azure atribui o grupo do mesmo nome que a VM.
4. Na **rede Virtual**, selecione a VNet na qual a VM será realizada. São apresentadas todas as VNets associadas à subscrição. Selecione a sub-rede. A primeira sub-rede está selecionada por predefinição.
5. Na **localização de armazenamento**, especifique o tipo de armazenamento utilizado para a VM.

    ![Restaurar o Assistente de configuração](./media/backup-azure-arm-restore-vms/recovery-configuration-wizard1.png)

6. Na **configuração do restauro**, selecione **OK**. Na **restaurar**, clique em **restaurar** para acionar a operação de restauro.



## <a name="create-new-restore-disks"></a>Criar novo restaurar discos

1. Na **configuração do restauro** > **criar novo** > **restaurar tipo**, selecione **restaurar discos**.
2. Na **grupo de recursos**, selecione um grupo de recursos existente para os discos restaurados ou criar um novo com um nome globalmente exclusivo.
3. Na **conta de armazenamento**, especifique a conta ao qual pretende copiar os VHDs. Certifique-se que a conta está na mesma região que o cofre.

    ![Configuração de recuperação concluída](./media/backup-azure-arm-restore-vms/trigger-restore-operation1.png)

4. Na **configuração do restauro**, selecione **OK**. Na **restaurar**, clique em **restaurar** para acionar a operação de restauro.
5. Depois de restaurar o disco, pode fazer qualquer um dos seguintes passos para concluir a operação de restauro de VM.

    - Utilize o modelo que foi gerado como parte da operação de restauro para personalizar as definições e acionar a implementação da VM. Editar as definições de modelo padrão e submeta o modelo de implementação da VM.
    - Pode [ligue os discos restaurados](https://docs.microsoft.com/azure/virtual-machines/windows/attach-disk-ps) a uma VM existente.
    - Pode [criar uma nova VM](backup-azure-vms-automation.md#restore-an-azure-vm) dos discos restaurados com o PowerShell.


## <a name="replace-existing-disks"></a>Substituir discos existentes

Utilize esta opção para substituir discos existentes na VM atual com o ponto de restauro selecionado.

1. Na **configuração do restauro**, clique em **substituir a existente**.
2. Na **restaurar tipo**, selecione **substitua/s de disco** (o ponto de restauro irá substituir o disco da VM ou discos existentes).
3. Na **localização de transição**, especifique onde os instantâneos dos discos geridos atuais deverá ser guardados.

   ![Restaurar o Assistente de configuração substituir existente](./media/backup-azure-arm-restore-vms/restore-configuration-replace-existing.png)


## <a name="track-the-restore-operation"></a>Controlar a operação de restauro
Depois de acionar a operação de restauro, o serviço de cópia de segurança cria uma tarefa para controlar a operação de restauro. O serviço de cópia de segurança também cria e temporariamente apresenta a notificação no **notificações** área do portal. Se não vir a notificação, selecione o **notificações** símbolo para ver as notificações.

![Restauro acionado](./media/backup-azure-arm-restore-vms/restore-notification1.png)

Clicar no hiperlink de notificações para aceder à **BackupJobs** lista. Todos os detalhes das operações para uma determinada tarefa está disponível na **BackupJobs** apresenta uma lista. Pode aceder à **BackupJobs** do dashboard do cofre clicando as tarefas de cópia de segurança do mosaico, selecione **máquina virtual do Azure** para apresentar os trabalhos associados no cofre.

O **tarefas de cópia de segurança** painel abre e apresenta a lista de tarefas.

![Lista de VMs num cofre](./media/backup-azure-arm-restore-vms/restore-job-in-progress1.png)

**Barra de progresso** está agora disponível na **restaurar detalhes** painel. O **restaurar detalhes** pode ser aberto o painel ao clicar em qualquer tarefa de restauro que tem estado **em curso**. O **barra de progresso** está disponível em todas as variantes de restauros como **criar nova**, **restaurar disco** e **substituir a existente**. Os detalhes transportados pela barra de progresso restaurar são **estimado momento do restauro**, **percentagem de restauro** e **número de bytes transferidos**.

Restaure barra de progresso, detalhes são indicados abaixo:

- **Tempo de restauro estimado** inicialmente fornece o tempo necessário para concluir a operação de restauro. À medida que a operação avança, o tempo que reduz e atinge 0 uma vez concluída a operação de restauro.
- **Percentagem de restauro** fornece os dados que percentagem de operação de restauro está concluída.
- **Número de bytes transferidos** está disponível na tarefa sub quando o restauro ocorre por meio de criar uma nova VM. Esta opção fornece que os detalhes de quantos números de bytes foram transferidos com o número total de bytes a serem transferidos.


## <a name="use-templates-to-customize-a-restored-vm"></a>Utilize modelos para personalizar uma VM restaurada
Depois do [conclusão da operação de discos de restauro](#Track-the-restore-operation), utilize o modelo que foi gerado como parte da operação de restauro para criar uma nova VM com uma configuração diferente da configuração de cópia de segurança. Também pode utilizá-lo para personalizar os nomes dos recursos que foram criados durante o processo de criar uma nova VM a partir de um ponto de restauro.

![Restaurar a desagregação da tarefa](./media/backup-azure-arm-restore-vms/restore-job-drill-down1.png)

Para obter o modelo que foi gerado como parte da opção de discos de restauro:

1. Vá para o **detalhes da tarefa de restauro** correspondente para a tarefa.

2. Sobre o **detalhes da tarefa de restauro** ecrã, selecione **para implementar o modelo** para iniciar a implementação do modelo.

3. Na **implementar modelo** painel implementação personalizada, para a implementação do modelo de utilização [editar e implementar o modelo](../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template) ou anexar personalizações mais por [criar um modelo de](../azure-resource-manager/resource-group-authoring-templates.md) antes de implementar.

  ![Implementação do modelo de carga](./media/backup-azure-arm-restore-vms/edit-template1.png)

4. Depois de introduzir os valores necessários, aceite o **termos e condições** e selecione **Compra**.

  ![Submeter a implementação de modelo](./media/backup-azure-arm-restore-vms/submitting-template1.png)

## <a name="post-restore-steps"></a>Passos de pós-restauro
* Se utilizar uma distribuição do Linux com base na cloud-init, como o Ubuntu, por motivos de segurança, a palavra-passe é bloqueada publicar o restauro. Utilizar a extensão VMAccess à VM restaurada para [repor a palavra-passe](../virtual-machines/linux/reset-password.md). Recomendamos que utilize chaves SSH nessas distribuições para evitar a repor o restauro de postagem de palavra-passe.
* As extensões estão presentes durante a configuração de cópia de segurança são instaladas, mas eles não estar ativados. Se vir um problema, reinstale as extensões.
* Se a VM de cópia de segurança tem o restauro de postagem IP estático, a VM restaurada tem um IP dinâmico para evitar conflitos ao criar uma VM restaurada. Saiba mais sobre como pode [adicionar um IP estático a uma VM restaurada](../virtual-network/virtual-networks-reserved-private-ip.md#how-to-add-a-static-internal-ip-to-an-existing-vm).
* Uma VM restaurada não tem um conjunto de valores de disponibilidade. Recomendamos que utilize a opção de discos de restauro para [adicionar um conjunto de disponibilidade](../virtual-machines/windows/tutorial-availability-sets.md) quando cria uma VM do PowerShell ou modelos utilizando restaurar discos.


## <a name="backup-for-restored-vms"></a>Cópia de segurança para VMs restauradas
Se restaurar uma VM para o mesmo grupo de recursos com o mesmo nome da VM originalmente uma cópia de segurança, cópia de segurança continua o restauro de postagem VM. Se tiver restaurado a VM a um grupo de recursos diferente ou que especificou um nome diferente para a VM restaurada, a VM é tratada como se fosse uma nova VM. Terá de configurar a cópia de segurança para a VM restaurada.

## <a name="restore-a-vm-during-an-azure-datacenter-disaster"></a>Restaurar uma VM durante um desastre de datacenter do Azure
O Azure Backup permite restaurar uma cópia de segurança de VMs para o datacenter emparelhado, no caso de um desastre de ocorrer no datacenter principal, onde as VMs são executadas e configurou o Cofre de cópia de segurança para ser georredundante. Durante a tais cenários, selecione uma conta de armazenamento, o que está presente num datacenter emparelhado. O restante do processo de restauro permanece o mesmo. Cópia de segurança utiliza o serviço de computação da geo emparelhado para criar a VM restaurada. Para obter mais informações, consulte [resiliência de datacenter do Azure](../resiliency/resiliency-technical-guidance-recovery-loss-azure-region.md).

## <a name="restore-domain-controller-vms"></a>Restaurar VMs do controlador de domínio
Cópia de segurança do controlador de domínio (DC) de VMs é um cenário suportado com a cópia de segurança. No entanto, deve ter cuidado durante o processo de restauração. O processo de restauração correto depende da estrutura do domínio. No caso mais simples, tem um único controlador de domínio num único domínio. Mais comumente para cargas de produção, tem um único domínio com vários controladores de domínio, talvez com alguns controladores de domínio no local. Por fim, pode ter uma floresta com vários domínios.

Da perspectiva do Active Directory, a VM do Azure é como qualquer outra VM num hipervisor suportado modernos. A principal diferença com hipervisores no local é que não existe nenhuma consola da VM disponíveis no Azure. Uma consola é necessária para determinados cenários, tais como de recuperação por meio de uma recuperação bare-metal (BMR)-cópia de segurança de tipo. No entanto, o restauro de VMS a partir do Cofre de cópia de segurança é uma substituição completa para a BMR. Modo de restauro serviços de diretório (DSRM) também está disponível, portanto, todos os cenários de recuperação do Active Directory são viáveis. Para obter mais informações, consulte [considerações sobre a cópia de segurança e restauro para controladores de domínio virtualizado](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv(v=ws.10).aspx#backup_and_restore_considerations_for_virtualized_domain_controllers) e [planear a recuperação de floresta do Active Directory](https://technet.microsoft.com/library/planning-active-directory-forest-recovery(v=ws.10).aspx).

### <a name="single-dc-in-a-single-domain"></a>Controlador de domínio único num único domínio
A VM pode ser restaurada (como qualquer outra VM) do portal do Azure ou através do PowerShell.

### <a name="multiple-dcs-in-a-single-domain"></a>Vários controladores de domínio num único domínio
Quando os demais DCs do mesmo domínio podem ser contatados através da rede, o controlador de domínio pode ser restaurada, como qualquer VM. Se for o último controlador de domínio restante no domínio ou uma recuperação numa rede isolada é executada, um procedimento de recuperação de floresta tem de ser seguido.

### <a name="multiple-domains-in-one-forest"></a>Vários domínios numa floresta
Quando os demais DCs do mesmo domínio podem ser contatados através da rede, o controlador de domínio pode ser restaurada, como qualquer VM. Em todos os outros casos, recomendamos uma recuperação de floresta.

## <a name="restore-vms-with-special-network-configurations"></a>Restaurar VMs com configurações de rede especiais
É possível criar cópias de segurança e restaurar VMs com as seguintes configurações de rede especiais. No entanto, estas configurações requerem tidos em consideração especial ao percorrer o processo de restauro:

* VMs em balanceadores de carga (internos e externos)
* VMs com vários IPs reservados
* VMs com vários NICs

> [!IMPORTANT]
> Quando cria a configuração de rede especiais para as VMs, tem de utilizar o PowerShell para criar VMs a partir de discos restaurados.
>
>

Totalmente voltar a criar as VMs depois de restaurar para o disco, siga estes passos:

1. Restaurar os discos a partir dos serviços de recuperação cofre utilizando [PowerShell](backup-azure-vms-automation.md#restore-an-azure-vm).

1. Criar a configuração de VM necessária para o Balanceador de carga / vários NIC/vários reservado IP utilizando os cmdlets do PowerShell. Utilize-o para criar a VM com a configuração que pretende:

   a. Criar uma VM no serviço cloud com uma [Balanceador de carga interno](https://azure.microsoft.com/documentation/articles/load-balancer-internal-getstarted/).

   b. Criar uma VM para ligar a uma [Balanceador de carga com acesso à internet](https://azure.microsoft.com/documentation/articles/load-balancer-internet-getstarted/).

   c. Crie uma VM com [vários NICs](../virtual-machines/windows/multiple-nics.md).

   d. Crie uma VM com [reservados de vários IPs](../virtual-network/virtual-network-multiple-ip-addresses-powershell.md).

## <a name="next-steps"></a>Passos Seguintes
Agora que pode restaurar as suas VMs, consulte o artigo de resolução de problemas para obter informações sobre erros comuns com VMs. Além disso, consulte o artigo sobre o gerenciamento de tarefas com as suas VMs.

* [Resolução de problemas de erros](backup-azure-vms-troubleshoot.md#restore)
* [Gerir máquinas virtuais](backup-azure-manage-vms.md)
