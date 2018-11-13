---
title: Perguntas comuns - VMware para recuperação após desastre do Azure com o Azure Site Recovery | Documentos da Microsoft
description: Este artigo resume as perguntas mais comuns ao configurar a recuperação após desastre de VMs de VMware no local para o Azure com o Azure Site Recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.date: 10/29/2018
ms.topic: conceptual
ms.author: raynew
ms.openlocfilehash: 2436a4e75045200a8d2f48586e31ebfa0c03705a
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/12/2018
ms.locfileid: "51566266"
---
# <a name="common-questions---vmware-to-azure-replication"></a>Perguntas comuns - VMware para replicação do Azure

Este artigo fornece respostas a perguntas comuns que vemos ao implementar a recuperação após desastre de VMs de VMware no local para o Azure. Se tiver questões depois de ler este artigo, publique o [fórum de serviços de recuperação do Azure](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr).


## <a name="general"></a>Geral
### <a name="how-is-site-recovery-priced"></a>Como é que o Site Recovery é cobrado?
Revisão [preços do Azure Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/) detalhes.

### <a name="how-do-i-pay-for-azure-vms"></a>Como pago para VMs do Azure?
Durante a replicação, os dados são replicados para o armazenamento do Azure e não será necessário prestar quaisquer alterações VM. Quando executar uma ativação pós-falha para o Azure, o Site Recovery cria automaticamente máquinas de virtuais de IaaS do Azure. Depois disso é-lhe cobrada para recursos de computação que consumir no Azure.

### <a name="what-can-i-do-with-vmware-to-azure-replication"></a>O que posso fazer com o VMware para replicação do Azure?
- **Recuperação após desastre**: pode configurar a recuperação após desastre. Neste cenário, é possível replicar VMs de VMware no local para o armazenamento do Azure. Em seguida, se a sua infraestrutura no local não estiver disponível, pode efetuar a ativação pós-falha para o Azure. Quando efetuar a ativação pós-falha, as VMs do Azure são criadas com os dados replicados. Pode aceder a aplicações e cargas de trabalho em VMs do Azure, até que o seu datacenter no local estiver novamente disponível. Em seguida, pode reativação pós-falha do Azure para o seu site no local.
- **Migração**: pode utilizar o Site Recovery para migrar VMs de VMware no local para o Azure. Neste cenário, replicar VMs de VMware no local para o armazenamento do Azure. Em seguida, efetuar a ativação pós-falha do local para o Azure. Após a ativação pós-falha, as suas aplicações e cargas de trabalho estão disponíveis e em execução em VMs do Azure.



## <a name="azure"></a>Azure
### <a name="what-do-i-need-in-azure"></a>O que é necessário no Azure?
Terá de uma subscrição do Azure, um cofre dos serviços de recuperação, uma conta de armazenamento e uma rede virtual. O cofre, a conta de armazenamento e a rede tem de ser na mesma região.

### <a name="what-azure-storage-account-do-i-need"></a>Que conta de armazenamento do Azure é necessário?
Precisa de uma conta de armazenamento LRS ou GRS. Recomendamos GRS para que os dados sejam resilientes se ocorrer uma falha regional ou se a região primária não pode ser recuperada. Armazenamento Premium é suportado.

### <a name="does-my-azure-account-need-permissions-to-create-vms"></a>Minha conta do Azure precisa de permissões para criar VMs?
Se for um administrador de subscrição, tem as permissões de replicação que precisa. Se não tiver, terá permissões para criar uma VM do Azure no grupo de recursos e rede virtual que especificou quando configurar a recuperação de sites e permissões para escrever na conta de armazenamento selecionada. [Saiba mais](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines).



## <a name="on-premises"></a>Local

### <a name="what-do-i-need-on-premises"></a>O que fazer, preciso no local?
No local terá de componentes do Site Recovery, instalados numa única VM de VMware. Terá também de uma infraestrutura do VMware, com, pelo menos, um anfitrião ESXi e recomendamos um servidor vCenter. Além disso, terá de uma ou mais VMs de VMware para replicar. [Saiba mais](vmware-azure-architecture.md) sobre VMware para a arquitetura do Azure.

O servidor de configuração no local pode ser implementado de uma das duas formas seguintes

1. Implantá-la usando um modelo VM com o servidor de configuração pré-instalado. [Saiba mais aqui](vmware-azure-tutorial.md#download-the-vm-template).
2. Implantá-la usando a configuração num computador Windows Server 2016 à sua escolha. [Saiba mais aqui](physical-azure-disaster-recovery.md#set-up-the-source-environment).

Para saber os passos para implementar o servidor de configuração nos seus próprios computadores Windows Server, o objetivo de proteção de ativação da proteção, de introdução escolha **para o Azure > não virtualizado/outro**.

### <a name="where-do-on-premises-vms-replicate-to"></a>Onde replicar VMs no local para o?
Dados são replicados para o armazenamento do Azure. Quando executa uma ativação pós-falha, o Site Recovery cria automaticamente as VMs do Azure da conta de armazenamento.

## <a name="replication"></a>Replicação

### <a name="what-apps-can-i-replicate"></a>Que aplicações posso replicar?
Pode replicar qualquer aplicação ou carga de trabalho em execução numa VM de VMware que está em conformidade com [requisitos de replicação](vmware-physical-azure-support-matrix.md##replicated-machines). Recuperação de sites fornece suporte para a replicação com suporte a aplicativos, para que aplicações podem ser realizadas a ativação pós-falha e falhou para um Estado inteligente. Site Recovery integra-se com aplicações da Microsoft, como SharePoint, Exchange, Dynamics, SQL Server e Active Directory e trabalha diretamente com fornecedores importantes, incluindo a Oracle, SAP, IBM e Red Hat. [Saiba mais](site-recovery-workload.md) sobre proteção de carga de trabalho.

### <a name="can-i-replicate-to-azure-with-a-site-to-site-vpn"></a>Pode replicar para o Azure com uma VPN de site a site?
O site Recovery replica dados no local para o armazenamento do Azure através de um ponto final público ou com peering público do ExpressRoute. Não é suportada a replicação através de uma rede VPN de site a site.

### <a name="can-i-replicate-to-azure-with-expressroute"></a>Pode replicar para o Azure com o ExpressRoute?
Sim, o ExpressRoute pode ser utilizado para replicar VMs no Azure. O site Recovery replica dados para uma conta de armazenamento do Azure através de um ponto final público e tem de configurar [peering público](../expressroute/expressroute-circuit-peerings.md#azure-public-peering) para replicação do Site Recovery. Depois de VMs com a ativação pós-falha para uma rede virtual do Azure, pode acessá-los usando [peering privado](../expressroute/expressroute-circuit-peerings.md#azure-private-peering).


### <a name="why-cant-i-replicate-over-vpn"></a>Por que não é possível replicar através de VPN?

Ao replicar para o Azure, o tráfego de replicação atinge os pontos finais públicos de uma conta de armazenamento do Azure, assim, apenas pode replicar através da internet pública com o ExpressRoute (peering público) e VPN não funciona.


### <a name="what-are-the-replicated-vm-requirements"></a>Quais são os requisitos de VM replicados?

Para a replicação, uma VM do VMware tem de executar um sistema operativo suportado. Além disso, a VM tem de cumprir os requisitos para VMs do Azure. [Saiba mais](vmware-physical-azure-support-matrix.md##replicated-machines) na matriz de suporte.

### <a name="how-often-can-i-replicate-to-azure"></a>Com que frequência posso replicar para o Azure?
A replicação é contínua ao replicar VMs de VMware para Azure.

### <a name="can-i-extend-replication"></a>Posso expandir a replicação?
Não é suportada a replicação expandida ou em cadeia. Pedir esta funcionalidade no [fórum de comentários](http://feedback.azure.com/forums/256299-site-recovery/suggestions/6097959-support-for-exisiting-extended-replication).

### <a name="can-i-do-an-offline-initial-replication"></a>Posso fazer uma replicação inicial offline?
Tal não é suportado. Pedir esta funcionalidade no [fórum de comentários](http://feedback.azure.com/forums/256299-site-recovery/suggestions/6227386-support-for-offline-replication-data-transfer-from).

### <a name="can-i-exclude-disks"></a>Pode excluir discos?
Sim, pode excluir discos da replicação.

### <a name="can-i-replicate-vms-with-dynamic-disks"></a>Pode replicar VMs com discos dinâmicos?
Discos dinâmicos podem ser replicados. O disco do sistema operativo tem de ser um disco básico.

### <a name="if-i-use-replication-groups-for-multi-vm-consistency-can-i-add-a-new-vm-to-an-existing-replication-group"></a>Se utilizar grupos de replicação consistência multi-VM, posso adicionar uma nova VM a um grupo de replicação existente?
Sim, pode adicionar novas VMs a um grupo de replicação existente quando ativa a replicação para eles. Não é possível adicionar uma VM a um grupo de replicação existente, depois de replicação é iniciada e não é possível criar um grupo de replicação de VMs existentes.

### <a name="can-i-modify-vms-that-are-replicating-by-adding-or-resizing-disks"></a>Pode modificar as VMs que estão a replicar com a adição ou redimensionar discos?

Para a replicação de VMware para o Azure pode modificar o tamanho do disco. Se pretender adicionar novos discos, que terá de adicionar o disco e reativar a proteção da VM.

## <a name="configuration-server"></a>Servidor de configuração

### <a name="what-does-the-configuration-server-do"></a>O que faz o servidor de configuração?
O servidor de configuração é executado no local para componentes do Site Recovery, incluindo:
- O servidor de configuração que coordena as comunicações entre no local e o Azure e gere a replicação de dados.
- O servidor de processos atua como um gateway de replicação. Recebe dados de replicação. otimiza-os com colocação em cache, compressão e encriptação; e envia-o para o armazenamento do Azure., o servidor de processos também instala o serviço de mobilidade nas VMs que pretende replicar e efetua a deteção automática de VMs de VMware no local.
- O servidor de destino principal processa dados de replicação durante a reativação pós-falha do Azure.

[Saiba mais](vmware-azure-architecture.md) sobre os componentes de servidor de configuração e os processos.

### <a name="where-do-i-set-up-the-configuration-server"></a>Onde posso configurar o servidor de configuração?
Precisa de uma VM do VMware único elevada disponibilidade no local para o servidor de configuração.

### <a name="what-are-the-requirements-for-the-configuration-server"></a>Quais são os requisitos para o servidor de configuração?

Reveja os [pré-requisitos](vmware-azure-deploy-configuration-server.md#prerequisites).

### <a name="can-i-manually-set-up-the-configuration-server-instead-of-using-a-template"></a>Posso manualmente configurar o servidor de configuração em vez de utilizar um modelo?
Recomendamos que utilize a versão mais recente do modelo OVF para [criar a VM do servidor de configuração](vmware-azure-deploy-configuration-server.md). Se por algum motivo não pode, por exemplo não tem acesso ao servidor VMware, pode [transfira o ficheiro de configuração unificada](physical-azure-set-up-source.md) no portal, e executá-la numa VM.

### <a name="can-a-configuration-server-replicate-to-more-than-one-region"></a>Um servidor de configuração pode replicar para o mais do que uma região?
Não. Para tal, terá de configurar um servidor de configuração em cada região.

### <a name="can-i-host-a-configuration-server-in-azure"></a>Pode alojar um servidor de configuração no Azure?
Ainda que possível, a VM do Azure com o servidor de configuração têm de comunicar com a sua infraestrutura de VMware no local e VMs. Isso pode adicionar as latências e afetar os replicação em curso.

### <a name="how-do-i-update-the-configuration-server"></a>Como posso atualizar o servidor de configuração?
[Saiba mais sobre](vmware-azure-manage-configuration-server.md#upgrade-the-configuration-server) a atualizar o servidor de configuração. Pode encontrar as informações de atualização mais recente na [página de atualizações do Azure](https://azure.microsoft.com/updates/?product=site-recovery). Pode transferir a versão mais recente do servidor de configuração de também diretamente [Microsoft Download Center](https://aka.ms/asrconfigurationserver).

### <a name="should-i-backup-the-deployed-configuration-server"></a>Deve posso criar cópias de segurança do servidor de configuração implementada?
Recomendamos que faça cópias de segurança agendadas regulares do servidor de configuração. Para a reativação pós-falha concluída com êxito, a máquina virtual que está a ser realizarão a reativação pós-falha tem de existir na base de dados de servidor de configuração e o servidor de configuração tem de estar em execução e num estado ligado. Pode saber mais sobre tarefas de gestão de servidor de configuração comuns [aqui](vmware-azure-manage-configuration-server.md).

### <a name="when-im-setting-up-the-configuration-server-can-i-download-and-install-mysql-manually"></a>Quando eu estou configurar o servidor de configuração, posso transferir e instalar manualmente o MySQL?
Sim. Transferir o MySQL e colocá-la a **C:\Temp\ASRSetup** pasta. Em seguida, instale manualmente. Quando configurar a VM do servidor de configuração e aceitar os termos, MySQL será listada como **já instalado** na **transferir e instalar**.

### <a name="can-i-avoid-downloading-mysql-but-let-site-recovery-install-it"></a>Pode evitar o download de MySQL mas permitir que o Site Recovery instalá-lo?
Sim. Transferir o instalador do MySQL e colocá-la a **C:\Temp\ASRSetup** pasta.  Quando configurou a VM do servidor de configuração, aceite os termos e clique em **transferir e instalar**, o portal irá utilizar o instalador que adicionou ao instalar o MySQL.
 
### <a name="canl-i-use-the-configuration-server-vm-for-anything-else"></a>CanL posso utilizar o servidor de configuração de VM para qualquer outra coisa?
Não, só deve utilizar a VM para o servidor de configuração. 

### <a name="can-i-change-the-vault-registered-in-the-configuration-server"></a>Pode alterar o Cofre registado no servidor de configuração?
Não. Depois de um cofre é registrado no servidor de configuração, não pode ser alterado.

### <a name="can-i-use-the-same-configuration-server-for-disaster-recovery-of-both-vmware-vms-and-physical-servers"></a>Pode utilizar o mesmo servidor de configuração para recuperação após desastre de VMs de VMware e servidores físicos
Sim, mas tenha em atenção que a máquina física pode apenas ser falha novamente para uma VM do VMware.

### <a name="where-can-i-download-the-passphrase-for-the-configuration-server"></a>Onde posso transferir a frase de acesso para o servidor de configuração?
[Rever este artigo](vmware-azure-manage-configuration-server.md#generate-configuration-server-passphrase) para saber mais sobre a frase de acesso a transferir.

### <a name="where-can-i-download-vault-registration-keys"></a>Onde posso transferir as chaves de registo do Cofre?

Na **Cofre de serviços de recuperação**, **gerir** > **infraestrutura do Site Recovery** > **deservidoresdeconfiguração**. Na **servidores**, selecione **chave de registo do Download** para transferir o ficheiro de credenciais do cofre.



## <a name="mobility-service"></a>Serviço de Mobilidade

### <a name="where-can-i-find-the-mobility-service-installers"></a>Onde posso encontrar os programas de instalação do serviço de mobilidade?
Os programas de instalação são guardados na **%ProgramData%\ASR\home\svsystems\pushinstallsvc\repository** pasta no servidor de configuração.

## <a name="how-do-i-install-the-mobility-service"></a>Como posso instalar o serviço de mobilidade?
Instalar em cada VM que pretende replicar, com um [instalação push](vmware-azure-install-mobility-service.md), ou [instalação manual](vmware-physical-mobility-service-install-manual.md) partir da IU ou do Powershell. Em alternativa, pode implementar utilizando uma ferramenta de implantação, como [System Center Configuration Manager](vmware-azure-mobility-install-configuration-mgr.md).



## <a name="security"></a>Segurança

### <a name="what-access-does-site-recovery-need-to-vmware-servers"></a>Que acesso o Site Recovery precisa aos servidores VMware?
O Site Recovery precisa de ter acesso a servidores VMware para:

- Configure uma VM de VMware que executam o servidor de configuração e outros componentes do Site Recovery no local. [Saiba mais](vmware-azure-deploy-configuration-server.md)
- Detete automaticamente as VMs para a replicação. Saiba mais sobre como preparar uma conta para a deteção automática. [Saiba mais](vmware-azure-tutorial-prepare-on-premises.md#prepare-an-account-for-automatic-discovery)


### <a name="what-access-does-site-recovery-need-to-vmware-vms"></a>Que acesso o Site Recovery precisa de VMs do VMware?

- Para replicar, uma VM de VMware tem de ter o serviço de mobilidade de recuperação de sites instalada e em execução. Pode implementar a ferramenta manualmente, ou especificar que a recuperação de Site deve fazer uma instalação push do serviço, quando ativa a replicação para uma VM. Para a instalação de push, recuperação de sites precisa de uma conta que pode utilizar para instalar o componente de serviço. [Saiba mais](vmware-azure-tutorial-prepare-on-premises.md#prepare-an-account-for-mobility-service-installation). O servidor de processos (em execução por predefinição no servidor de configuração) executa esta instalação. [Saiba mais](vmware-azure-install-mobility-service.md) sobre a instalação do serviço de mobilidade.
- Durante a replicação, VMs comunicam com o Site Recovery da seguinte forma:
    - As VMs a comunicar com o servidor de configuração na porta HTTPS 443 para a gestão de replicação.
    - VMs enviam dados de replicação para o servidor de processos na porta HTTPS 9443 (pode ser modificada).
    - Se ativar a consistência multi-VM, as VMs comunicam entre si pela porta 20004.


### <a name="is-replication-data-sent-to-site-recovery"></a>Dados de replicação são enviados para o Site Recovery?
Não, a recuperação de sites não interceta os dados replicados e não tem quaisquer informações sobre o que está em execução nas suas VMs. Os dados de replicação são trocados entre os hipervisores de VMware e de armazenamento do Azure. A Recuperação de Sites não tem capacidade para intercetar esses dados. Apenas os metadados necessários para orquestrar a replicação e a ativação pós-falha são enviados para o serviço de Recuperação de Sites.  

Site Recovery é ISO 27001:2013, 27018, HIPAA, DPA certified e está no processo de SOC2 e FedRAMP JAB.

### <a name="can-we-keep-on-premises-metadata-within-a-geographic-regions"></a>Vamos guardar os metadados de locais dentro de um regiões geográficas?
Sim. Quando cria um cofre numa região, podemos assegurar que todos os metadados usados pelo Site Recovery permanece dentro do limite de geográfico dessa região.

### <a name="does-site-recovery-encrypt-replication"></a>A Recuperação de Sites faz encriptação de replicação?
Sim, ambas as encriptação em trânsito e [encriptação no Azure](https://docs.microsoft.com/azure/storage/storage-service-encryption) são suportados.


## <a name="failover-and-failback"></a>Ativação pós-falha e reativação pós-falha
### <a name="how-far-back-can-i-recover"></a>Qual a amplitude posso recuperar?
Para VMware para o Azure, o ponto de recuperação mais antigo, que pode usar é 72 horas.

### <a name="how-do-i-access-azure-vms-after-failover"></a>Como posso acessar as VMs do Azure após a ativação pós-falha?
Após a ativação pós-falha, pode acessar as VMs do Azure através de uma ligação de Internet segura, através de uma VPN de site a site ou através do Azure ExpressRoute. Terá de preparar uma série de coisas para se ligar. [Saiba mais](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover)

### <a name="is-failed-over-data-resilient"></a>É efetuada a ativação pós-falha dados resilientes?
O Azure foi concebido para ser resiliente. Recuperação de site foi desenvolvida para ativação pós-falha para um datacenter secundário do Azure, de acordo com o SLA do Azure. Quando ocorre a ativação pós-falha, devemos verificar se os metadados e cofres permanecem na mesma região geográfica que escolheu para o cofre.

### <a name="is-failover-automatic"></a>A ativação pós-falha é automática?
[Ativação pós-falha](site-recovery-failover.md) não é automática. Inicia as ativações pós-falha com um clique único no portal ou pode utilizar [ PowerShell](/powershell/module/azurerm.siterecovery) para acionar uma ativação pós-falha.

### <a name="can-i-fail-back-to-a-different-location"></a>Pode falhar novamente para uma localização diferente?
Sim, se a ativação pós-falha para o Azure, pode reativação pós-falha para uma localização diferente se original não estiver disponível. [Saiba mais](concepts-types-of-failback.md#alternate-location-recovery-alr).

### <a name="why-do-i-need-a-vpn-or-expressroute-to-fail-back"></a>Por que eu preciso uma VPN ou ExpressRoute para a reativação pós-falha?

Quando realizar a ativação pós-falha do Azure, dados do Azure são copiados para a VM no local e o acesso privado é necessário.



## <a name="automation-and-scripting"></a>Automatização e criação de scripts

### <a name="can-i-set-up-replication-with-scripting"></a>Pode definir a replicação com scripts?
Sim. Pode automatizar fluxos de trabalho de recuperação de Site utilizando a API Rest, PowerShell ou o SDK do Azure. [Saiba mais](vmware-azure-disaster-recovery-powershell.md).

## <a name="performance-and-capacity"></a>Desempenho e capacidade
### <a name="can-i-throttle-replication-bandwidth"></a>Posso limitar a largura de banda de replicação?
Sim. [Saiba mais](site-recovery-plan-capacity-vmware.md).


## <a name="next-steps"></a>Passos Seguintes
* [Revisão](vmware-physical-azure-support-matrix.md) dar suporte aos requisitos.
* [Configurar](vmware-azure-tutorial.md) VMware para replicação do Azure.
