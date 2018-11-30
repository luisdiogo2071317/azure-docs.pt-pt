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
ms.openlocfilehash: 0d78ae294cea383fbe59a1f7968d8bf18b1942d1
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/27/2018
ms.locfileid: "52422961"
---
# <a name="use-the-azure-portal-to-restore-virtual-machines"></a>Utilizar o portal do Azure para restaurar máquinas virtuais
Proteger os seus dados através de instantâneos dos seus dados em intervalos definidos. Estes instantâneos são conhecidos como pontos de recuperação e estão armazenados nos cofres dos serviços de recuperação. Se for necessário reparar ou recriar uma máquina virtual (VM), pode restaurar a VM a partir de qualquer um dos pontos de recuperação guardado. Quando restaurar um ponto de recuperação, pode:

* Crie uma nova VM, o que é uma representação de ponto no tempo da sua VM de cópia de segurança.
* Restaurar discos e utilize o modelo que é fornecido com o processo para personalizar a VM restaurada ou fazer uma recuperação de arquivos individuais.

Este artigo explica como restaurar uma VM para uma nova VM ou restaurar todos os discos de cópia de segurança. Para a recuperação de arquivos individuais, consulte [recuperar ficheiros a partir de uma cópia de segurança de VM do Azure](backup-azure-restore-files-from-vm.md).

![Três formas de restaurar a partir de cópia de segurança VM](./media/backup-azure-arm-restore-vms/azure-vm-backup-restore.png)

> [!NOTE]
> O Azure tem dois modelos de implementação para criar e trabalhar com recursos: [do Azure Resource Manager e clássica](../azure-resource-manager/resource-manager-deployment-model.md). Este artigo fornece as informações e procedimentos usados para restaurar implementado VMs utilizando o modelo do Resource Manager.
>
>

Restaurar uma VM ou todos os discos da VM backup envolve dois passos:

* Selecione um ponto de restauro para restauro.
* Selecione o tipo de restauro, crie uma nova VM ou restaurar discos e especificar os parâmetros necessários.

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

    a) com o botão direito clique no ponto de recuperação neste painel (menos de 30 dias) e inicie **restaurar a VM**.

    b) para restaurar a recuperação pontos superior a 30 dias, clique aqui fornecida no painel pode ser utilizado.

    c) **restaurar a VM** no menu de cabeçalho fornece uma opção para listar e filtrar as VMs nas datas personalizadas como preferencial.

    Utilize o filtro para alterar o intervalo de tempo dos pontos de restauro apresentada. Por predefinição, são apresentados os pontos de restauração de consistência de todas as. Modifique o filtro de pontos de restauro todos para selecionar uma consistência de ponto de restauro específico. Para obter mais informações sobre cada tipo de ponto de restauro, veja [consistência dos dados](backup-azure-vms-introduction.md#data-consistency).

    Opções de consistência do ponto de restauro:
    - Pontos de restauro consistentes de falhas
    - Pontos de restauro consistentes da aplicação
    - Pontos de restauro consistentes do sistema de ficheiros
    - Todos os pontos de restauro

  ![Pontos de restauro](./media/backup-azure-arm-restore-vms/vm-blade1.png)

    >  [!NOTE]
    > Tipo de recuperação representam se estiver na conta de armazenamento do cliente, no cofre ou ambos. Saiba mais sobre [ponto de recuperação instantânea](https://azure.microsoft.com/blog/large-disk-support/).

8. Sobre o **restaurar** painel, selecione **ponto de restauro**.

    ![Selecione o ponto de restauro](./media/backup-azure-arm-restore-vms/select-recovery-point1.png)

    O **restaurar** painel mostra que o ponto de restauro está definido ao clicar em **OK**.
9. Se ainda não estiver lá, vá para o **restaurar** painel. Certifique-se de que um [estiver selecionado o ponto de restauro](#select-a-restore-point-for-restore)e selecione **configuração de restauro**. O **restauro da configuração** é aberto o painel.

## <a name="choose-a-vm-restore-configuration"></a>Escolha uma configuração de restauro de VM
Depois de selecionar o ponto de restauro, escolha uma configuração de restauro de VM. Para configurar a cm restaurada, pode utilizar o portal do Azure ou o PowerShell.

1. Se ainda não estiver lá, vá para o **restaurar** painel. Certifique-se de que um [estiver selecionado o ponto de restauro](#select-a-restore-point-for-restore)e selecione **configuração de restauro**. O **restauro da configuração** é aberto o painel.
2. Este painel atualmente tem duas opções uma sendo **criar novo** que é padrão e o outro é **substituir a existente** que é o restauro de in-loco para substituir o disco (s) apenas manter as configurações existentes e extensões.

> [!NOTE]
> Estamos a trabalhar na substituição de toda a VM com disco (s), as definições de rede, configurações e extensões nos próximos meses.
>
>

 Na **criar novo** opção que restaura os dados para a nova VM ou disco nova (s), tem duas opções:

 ![Restaurar o Assistente de configuração](./media/backup-azure-arm-restore-vms/restore-configuration-create-new.png)

 - **Criar máquina virtual**
 - **Restaurar discos**

![Restaurar o Assistente de configuração](./media/backup-azure-arm-restore-vms/restore-configuration-create-new1.png)

O portal fornece um **criação rápida** opção para uma VM restaurada. Para personalizar a configuração da VM ou os nomes dos recursos criados como parte da criação de uma nova opção VM, utilize o PowerShell ou o portal para restaurar discos de cópia de segurança. Utilize comandos do PowerShell para anexe-os à sua opção de configuração da VM. Em alternativa, pode utilizar o modelo que vem com discos restaurados para personalizar a VM restaurada. Para obter informações sobre como restaurar uma VM que tem várias NICs ou está sob um balanceador de carga, veja [restaurar uma VM com configurações de rede especiais](#restore-vms-with-special-network-configurations). Se a sua VM do Windows utiliza [licenciamento de HUB](../virtual-machines/windows/hybrid-use-benefit-licensing.md), restaurar discos e utilize o PowerShell/modelo conforme especificado neste artigo para criar a VM. Certifique-se de que especifica o **tipo de licença** como "Windows_Server" enquanto a criar a VM para aproveitar os benefícios HUB à VM restaurada. Tenha em atenção de que isso pode ser feito mais tarde após a criação da VM também.

## <a name="create-a-new-vm-from-a-restore-point"></a>Criar uma nova VM a partir de um ponto de restauro
1. Sobre o **restauro da configuração** painel mencionado no antes de seção, introduza ou selecione os valores para cada um dos seguintes campos:

    a. **Restaurar tipo**. Cria uma máquina virtual.

    b. **Nome da máquina virtual**. Introduza o nome da VM que não existe na subscrição.

    c. **Grupo de recursos**. Utilizar um grupo de recursos existente ou criar um novo. Se estiver a restaurar uma VM clássica, utilize este campo para especificar o nome de um novo serviço cloud. Se estiver a criar um novo serviço de cloud/grupo de recursos, o nome tem de ser globalmente exclusivo. Normalmente, o nome do serviço em nuvem está associado uma URL pública: por exemplo, [cloudservice]. cloudapp.net. Se tentar usar um nome para o serviço de cloud/grupo de recursos de nuvem já em utilização, o Azure atribui o serviço de cloud/grupo de recursos o mesmo nome que a VM. Azure apresenta recursos grupos/serviços cloud e VMs não associadas a quaisquer grupos de afinidade. Para obter mais informações, consulte [como migrar a partir de grupos de afinidades para uma rede virtual regional](../virtual-network/virtual-networks-migrate-to-regional-vnet.md).

    d. **Rede virtual**. Selecione a rede virtual ao criar a VM. O campo fornece todas as redes virtuais associadas à subscrição. O grupo de recursos da VM é apresentado em parênteses.

    e. **Sub-rede**. Se a rede virtual tem sub-redes, a primeira sub-rede está selecionada por predefinição. Se existirem sub-redes adicionais, selecione a sub-rede que pretende.

    f. **Localização de armazenamento**. Contas de armazenamento são a localização de transição. Esse menu lista as contas de armazenamento na mesma localização que o Cofre de serviços de recuperação. Contas de armazenamento que estão a redundância de zona não são suportadas. Se não existirem não existem contas de armazenamento com a mesma localização que o Cofre dos serviços de recuperação, tem de criar uma antes de iniciar a operação de restauro. Tipo de replicação da conta de armazenamento é apresentado em parênteses.

    ![Restaurar o Assistente de configuração](./media/backup-azure-arm-restore-vms/recovery-configuration-wizard1.png)

    > [!NOTE]
    > * Uma rede virtual é obrigatório para a VM implementadas no Resource Manager e opcional para uma VM clássica.

    > * Tipo de armazenamento fornecida no armazenamento de conta (premium ou standard) na localização de transição decide o tipo de armazenamento de disco de restauro. Atualmente não suportamos um modo misto de discos durante a restauração.
    >
    >

2. Sobre o **configuração de restauro** painel, selecione **OK** para finalizar a configuração de restauro. Sobre o **restaurar** painel, selecione **restaurar** para acionar a operação de restauro.

## <a name="restore-backed-up-disks"></a>Restaurar discos de cópia de segurança
Restaurar o valor do tipo **discos de restauro** na **configuração de restauro** painel permite criar uma VM com configurações personalizadas. Ao restaurar discos, a conta de armazenamento para ser selecionado deve estar na mesma localização que cofre dos serviços de recuperação. É obrigatório para criar uma conta de armazenamento, se existirem não existem contas de armazenamento com a mesma localização que o Cofre dos serviços de recuperação. Contas de armazenamento ZRS não são suportadas. Tipo de replicação da conta de armazenamento é apresentado em parênteses.

Após a operação de restauro, utilize abaixo:
* [Utilizar o modelo para personalizar a VM restaurada](#use-templates-to-customize-restore-vm)
* [Utilize os discos restaurados para anexar a uma VM existente](../virtual-machines/windows/attach-managed-disk-portal.md)
* [Criar uma nova VM com o PowerShell a partir de discos restaurados](./backup-azure-vms-automation.md#restore-an-azure-vm)

Sobre o **configuração de restauro** painel, selecione **OK** para finalizar a configuração de restauro. Sobre o **restaurar** painel, selecione **restaurar** para acionar a operação de restauro.

![Configuração de recuperação concluída](./media/backup-azure-arm-restore-vms/trigger-restore-operation1.png)

Na **restaurar lugar** está a ser feito por meio da guia **substituir existente**.

## <a name="replace-existing-disks-from-a-restore-point"></a>Substituir discos existentes de um ponto de restauro
**Substituir a existente** ajuda a opção para substituir discos existentes na VM atual com o ponto de restauro selecionado. Pode efetuar esta operação apenas se a VM atual existe. Se tiver sido eliminado devido a qualquer motivos, não é possível efetuar esta operação; em alternativa, recomendamos que faça **criar novo** operações de restauro de VM ou discos para continuar. Durante esta operação, como uma medida de precaução, podemos criar cópias de segurança os dados antes de iniciar a substituição de discos operações. Esta ação cria um instantâneo e também um ponto de recuperação no cofre com o período de retenção conforme agendado na política de cópia de segurança configurada. Se o ponto de restauro tem discos que mais/menos do que a VM atual, em seguida, o número de discos no ponto de restauro apenas se refletirá na VM. **Substituir a existente** opção não é atualmente suportada para discos não geridos e VMs encriptadas. Ele também não é suportado para [generalizada VMs](https://docs.microsoft.com/azure/virtual-machines/windows/capture-image-resource) e para as VMs criadas com [imagens personalizadas](https://azure.microsoft.com/resources/videos/create-a-custom-virtual-machine-image-in-azure-resource-manager-with-powershell/).  

 Sobre o **restaurar configuração** painel, a única entrada que tem de ser selecionada é **localização de transição**.

   ![Restaurar o Assistente de configuração substituir existente](./media/backup-azure-arm-restore-vms/restore-configuration-replace-existing.png)

 a. **Restaurar tipo**. Substitua disco (s) que representa que o ponto de restauro selecionado substituirá dos discos na VM existente.

 b. **Localização de transição**. Contas de armazenamento são a localização de transição para os discos geridos. Esse menu lista as contas de armazenamento na mesma localização que o Cofre de serviços de recuperação. Contas de armazenamento que estão a redundância de zona não são suportadas. Se não existirem não existem contas de armazenamento com a mesma localização que o Cofre dos serviços de recuperação, tem de criar uma antes de iniciar a operação de restauro. Tipo de replicação da conta de armazenamento é apresentado em parênteses.

## <a name="track-the-restore-operation"></a>Controlar a operação de restauro
Depois de acionar a operação de restauro, o serviço de cópia de segurança cria uma tarefa para controlar a operação de restauro. O serviço de cópia de segurança também cria e temporariamente apresenta a notificação no **notificações** área do portal. Se não vir a notificação, selecione o **notificações** símbolo para ver as notificações.

![Restauro acionado](./media/backup-azure-arm-restore-vms/restore-notification1.png)

Clicar no hiperlink de notificações para aceder à **BackupJobs** lista. Todos os detalhes das operações para uma determinada tarefa está disponível na **BackupJobs** apresenta uma lista. Pode aceder à **BackupJobs** do dashboard do cofre clicando as tarefas de cópia de segurança do mosaico, selecione **máquina virtual do Azure** para apresentar os trabalhos associados no cofre.

O **tarefas de cópia de segurança** painel abre e apresenta a lista de tarefas.

![Lista de VMs num cofre](./media/backup-azure-arm-restore-vms/restore-job-in-progress1.png)

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
