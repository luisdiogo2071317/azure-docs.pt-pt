---
title: Perguntas comuns - VMware para a replicação do Azure com o Azure Site Recovery | Microsoft Docs
description: Este artigo resume perguntas comuns quando replicar VMs de VMware no local para o Azure utilizando o Azure Site Recovery
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 03/15/2018
ms.author: raynew
ms.openlocfilehash: 345b73db423c6e12b56bb3308f7700917a372dda
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="common-questions---vmware-to-azure-replication"></a>Perguntas comuns - VMware para a replicação do Azure

Este artigo fornece respostas a questões recorrentes, vemos quando replicar VMs de VMware no local para o Azure. Se tiver questões depois de ler este artigo, publique-los no [fórum de serviços de recuperação do Azure](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr).


## <a name="general"></a>Geral
### <a name="how-is-site-recovery-priced"></a>Como a recuperação de sites tem um preço?
Reveja [preços do Azure Site Recovery](https://azure.microsoft.com/en-in/pricing/details/site-recovery/) detalhes.

### <a name="how-do-i-pay-for-azure-vms"></a>Como posso pagar para VMs do Azure?
Durante a replicação, os dados são replicados para o armazenamento do Azure e que não paga quaisquer alterações VM. Quando executar uma ativação pós-falha para o Azure, a recuperação de sites cria automaticamente máquinas virtuais do IaaS do Azure. Depois de que está a cobrados os recursos de computação que consumir no Azure.

### <a name="what-can-i-do-with-vmware-to-azure-replication"></a>O que posso fazer com o VMware para a replicação do Azure?
- **Recuperação após desastre**: pode configurar a recuperação após desastre completa. Neste cenário, replicar VMs de VMware no local para o armazenamento do Azure. Em seguida, se a sua infraestrutura no local não estiver disponível, pode efetuar a ativação pós-falha para o Azure. Quando efetuar a ativação pós-falha, as VMs do Azure são criadas utilizando os dados replicados. Pode aceder a aplicações e cargas de trabalho em VMs do Azure, até que o seu centro de dados no local está novamente disponível. Em seguida, pode efetuar a reativação a partir do Azure para o site no local.
- **Migração**: pode utilizar a recuperação de sites para migrar as VMs de VMware no local para o Azure. Neste cenário replicar VMs de VMware no local para o armazenamento do Azure. Em seguida, pode efetua a ativação pós-falha no local para o Azure. Após a ativação pós-falha, a aplicações e cargas de trabalho estão disponíveis e em execução em VMs do Azure.



## <a name="azure"></a>Azure
### <a name="what-do-i-need-in-azure"></a>O que é necessário no Azure?
Precisa de uma subscrição do Azure, um cofre dos serviços de recuperação, uma conta do storage e uma rede virtual. O cofre, a conta de armazenamento e a rede tem de ser na mesma região.

### <a name="what-azure-storage-account-do-i-need"></a>Que conta de armazenamento do Azure que preciso?
Necessita de uma conta de armazenamento LRS ou GRS. Recomendamos GRS para que os dados sejam resilientes se ocorrer uma falha regional ou se a região primária não pode ser recuperada. Armazenamento Premium é suportado.

### <a name="does-my-azure-account-need-permissions-to-create-vms"></a>A minha conta do Azure necessita de permissões para criar VMs?
Se a um administrador de subscrição, tem as permissões de replicação que é necessário. Se não estiver, precisa de permissões para criar uma VM do Azure no grupo de recursos e especificar quando configurar Reocvery de Site de rede virtual e permissões para escrever o accout de armazenamento selecionada. [Saiba mais](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines).



## <a name="on-premises"></a>Local 

### <a name="what-do-i-need-on-premises"></a>O que fazer necessário no local?
No local tem de componentes do Site Recovery, instalados numa única VM de VMware. Também precisa de uma infraestrutura de VMware, pelo menos um anfitrião ESXi e, recomendamos um vCenter server. Além disso, terá de uma ou mais VMs de VMware para replicar. [Saiba mais](vmware-azure-architecture.md) sobre VMware à arquitetura do Azure.

O servidor de configuração no local pode ser implementado das seguintes duas formas

1. Implementá-la através de um modelo VM que tenha o servidor de configuração pré-instaladas. [Leia mais aqui](vmware-azure-tutorial.md#download-the-vm-template).
2. Implementá-lo utilizando a configuração num computador com Windows Server 2016 à sua escolha. [Leia mais aqui](physical-azure-disaster-recovery.md#set-up-the-source-environment).

Para detetar a introdução aos passos para implementar o servidor de configuração no seus próprio máquinas Windows Server, o objetivo de proteção de ativar a proteção, escolha **para o Azure > não virtualizados/outras**.

### <a name="where-do-on-premises-vms-replicate-to"></a>Onde replicar VMs no local para?
Replicam os dados para armazenamento do Azure. Quando executar uma ativação pós-falha, a recuperação de sites cria automaticamente as VMs do Azure da conta do storage.

### <a name="what-apps-can-i-replicate"></a>Que aplicações posso replicar?
Pode replicar qualquer aplicação ou a carga de trabalho em execução numa VM VMware que está em conformidade com [requisitos de replicação](vmware-physical-azure-support-matrix.md##replicated-machines). Recuperação de sites fornece suporte para aplicações com suporte para replicação, para que as aplicações possam ser efetuadas a ativação pós-falha e falhou para um Estado inteligente. Recuperação de sites integra-se com aplicações da Microsoft, tais como o SharePoint, Exchange, Dynamics, SQL Server e Active Directory e funciona diretamente com os principais fornecedores, incluindo Oracle, SAP, IBM e Red Hat. [Saiba mais](site-recovery-workload.md) sobre proteção de carga de trabalho.

### <a name="can-i-replicate-to-azure-with-a-site-to-site-vpn"></a>Pode replicar para o Azure com uma VPN de site a site?
Recuperação de site replica os dados no local para o armazenamento do Azure através de um ponto final público ou utilizando o peering público do ExpressRoute. Não é suportada a replicação através de uma rede VPN de site para site.

### <a name="can-i-replicate-to-azure-with-expressroute"></a>Pode replicar para o Azure com o ExpressRoute?
Sim, ExpressRoute pode ser utilizado para replicar as VMs no Azure. Recuperação de site replica dados para uma conta de armazenamento do Azure através de um ponto final público, e terá de configurar [peering público](../expressroute/expressroute-circuit-peerings.md#azure-public-peering) para replicação de recuperação de sites. Depois de VMs com a ativação pós-falha para uma rede virtual do Azure, pode aceder aos mesmos utilizando [peering privado](../expressroute/expressroute-circuit-peerings.md#azure-private-peering).


### <a name="why-cant-i-replicate-over-vpn"></a>Por que motivo não é possível replicar através de VPN?

Quando replicar para o Azure, o tráfego de replicação atinge os pontos finais públicos de uma conta de armazenamento do Azure, deste modo, apenas pode replicar através da internet pública com o ExpressRoute (peering público) e VPN não funciona. 



## <a name="what-are-the-replicated-vm-requirements"></a>Quais são os requisitos de VM replicados?

Para replicação, uma VM de VMware tem de executar um sistema operativo suportado. Além disso, a VM tem de cumprir os requisitos para as VMs do Azure. [Saiba mais](vmware-physical-azure-support-matrix.md##replicated-machines) na matriz de suporte.

## <a name="how-often-can-i-replicate-to-azure"></a>Com que frequência posso replicar para o Azure?
A replicação está contínua quando replicar VMs de VMware para o Azure.

## <a name="can-i-extend-replication"></a>Pode expandir a replicação?
Não é suportada a replicação expandida ou em cadeia. Pedir esta funcionalidade no [fórum de comentários](http://feedback.azure.com/forums/256299-site-recovery/suggestions/6097959-support-for-exisiting-extended-replication).

## <a name="can-i-do-an-offline-initial-replication"></a>Pode fazer uma replicação inicial offline?
Tal não é suportado. Esta funcionalidade do pedido de [fórum de comentários](http://feedback.azure.com/forums/256299-site-recovery/suggestions/6227386-support-for-offline-replication-data-transfer-from).

### <a name="can-i-exclude-disks"></a>Pode excluir discos?
Sim, pode excluir discos de replicação. 

### <a name="can-i-replicate-vms-with-dynamic-disks"></a>Pode replicar as VMs com discos dinâmicos?
É possível replicar os discos dinâmicos. O disco do sistema operativo tem de ser um disco básico.

### <a name="can-i-add-a-new-vm-to-an-existing-replication-group"></a>Pode adicionar uma nova VM a um grupo de replicação existente?
Sim.

## <a name="configuration-server"></a>Servidor de configuração

### <a name="what-does-the-configuration-server-do"></a>O que o servidor de configuração?
O servidor de configuração é executado no local componentes da recuperação de Site, incluindo: 
- O servidor de configuração que coordena as comunicações entre no local e o Azure e gere a replicação de dados.
- O servidor de processos atua como um gateway de replicação. Recebe dados de replicação otimiza-as com colocação em cache, compressão e encriptação; e envia para o Azure armazenamento., o servidor de processos também instala o serviço de mobilidade em VMs que pretende replicar e efetua a descoberta automática de VMs de VMware no local.
- O servidor de destino principal que processa dados de replicação durante a reativação pós-falha a partir do Azure.

### <a name="where-do-i-set-up-the-configuration-server"></a>Onde posso configurar o servidor de configuração?
Precisa de uma VM de VMware único altamente disponível no local para o servidor de configuração.

### <a name="what-are-the-requirements-for-the-configuration-server"></a>Quais são os requisitos para o servidor de configuração?

Reveja o [pré-requisitos](vmware-azure-deploy-configuration-server.md#prerequisites).

### <a name="can-i-manually-set-up-the-configuration-server-instead-of-using-a-template"></a>Pode posso configurar manualmente o servidor de configuração em vez de utilizar um modelo?
Recomendamos que utilize a versão mais recente do modelo OVF para [criar o servidor de configuração VM](vmware-azure-deploy-configuration-server.md). Se por alguma razão não é possível, por exemplo não tiver acesso ao servidor do VMware, pode [transfira o ficheiro de configuração do Unified](physical-azure-set-up-source.md) no portal, e execute-a numa VM. 

### <a name="can-a-configuration-server-replicate-to-more-than-one-region"></a>Um servidor de configuração pode replicar a mais do que uma região?
Não. Para tal, terá de configurar um servidor de configuração em cada região.

### <a name="can-i-host-a-configuration-server-in-azure"></a>Pode alojar um servidor de configuração no Azure?
Ainda que possível, seria necessária a VM do Azure com o servidor de configuração comunicar com a infraestrutura do VMware no local e as VMs. A sobrecarga provavelmente não é viável.


### <a name="where-can-i-get-the-latest-version-of-the-configuration-server-template"></a>Onde posso obter a versão mais recente do modelo de servidor de configuração?
Transferir a versão mais recente do [Microsoft Download Center](https://aka.ms/asrconfigurationserver).

### <a name="how-do-i-update-the-configuration-server"></a>Como posso atualizar o servidor de configuração?
Instale o update rollup. Pode encontrar as informações de atualização mais recentes no [wiki atualizações página](https://social.technet.microsoft.com/wiki/contents/articles/38544.azure-site-recovery-service-updates.aspx).

## <a name="mobility-service"></a>Serviço de Mobilidade

### <a name="where-can-i-find-the-mobility-service-installers"></a>Onde posso encontrar os programas de instalação do serviço de mobilidade?
Os programas de instalação são guardados no **%ProgramData%\ASR\home\svsystems\pushinstallsvc\repository** pasta no servidor de configuração.

## <a name="how-do-i-install-the-mobility-service"></a>Como instalar o serviço de mobilidade?
Instalar em cada VM que pretende replicar, utilizando um [instalação push](vmware-azure-install-mobility-service.md#install-mobility-service-by-push-installation-from-azure-site-recovery), ou uma instalação manual do [a IU](vmware-azure-install-mobility-service.md#install-mobility-service-manually-by-using-the-gui), ou [através do PowerShell](vmware-azure-install-mobility-service.md#install-mobility-service-manually-at-a-command-prompt). Em alternativa, pode implementar utilizando uma ferramenta de implementação, tais como [System Center Configuration Manager](vmware-azure-mobility-install-configuration-mgr.md), ou [automatização do Azure e DSC](vmware-azure-mobility-deploy-automation-dsc.md).



## <a name="security"></a>Segurança

### <a name="what-access-does-site-recovery-need-to-vmware-servers"></a>O acesso a recuperação de sites necessita de servidores do VMware?
O Site Recovery precisa de ter acesso a servidores VMware para:

- Configure uma VM de VMware com o servidor de configuração e outros componentes de recuperação de sites no local. [Saiba mais](vmware-azure-deploy-configuration-server.md)
- Detete automaticamente as VMs para replicação. Saiba mais sobre como preparar uma conta de deteção automática. [Saiba mais](vmware-azure-tutorial-prepare-on-premises.md#prepare-an-account-for-automatic-discovery)


### <a name="what-access-does-site-recovery-need-to-vmware-vms"></a>O acesso a recuperação de sites precisa de VMs de VMware?

- Para replicar, uma VM de VMware tem de ter o serviço de mobilidade de recuperação de Site instalado e em execução. Pode implementar a ferramenta manualmente ou especificar que a recuperação de Site deve efetuar uma instalação push do serviço ao ativar a replicação para uma VM. Para a instalação de push, a recuperação de sites necessita de uma conta que pode utilizar para instalar o componente de serviço. [Saiba mais](vmware-azure-tutorial-prepare-on-premises.md#prepare-an-account-for-mobility-service-installation). O servidor de processos (em execução por predefinição no servidor de configuração) executa esta instalação. [Saiba mais](vmware-azure-install-mobility-service.md) sobre a instalação do serviço de mobilidade.
- Durante a replicação, VMs comunicam com a recuperação de sites da seguinte forma:
    - VMs a comunicar com o servidor de configuração na porta 443 de HTTPS para a gestão de replicação.
    - VMs enviarem dados de replicação para o servidor de processos na porta HTTPS 9443 (podem ser modificadas).
    - Se ativar a consistência multi VM, VMs comunicam entre si através da porta 20004.


### <a name="is-replication-data-sent-to-site-recovery"></a>Dados de replicação são enviados para a recuperação de sites?
Não, a recuperação de sites não interceta os dados replicados e não tem quaisquer informações sobre o que está em execução nas suas VMs. Dados de replicação são trocados entre os hipervisores VMware e de armazenamento do Azure. A Recuperação de Sites não tem capacidade para intercetar esses dados. Apenas os metadados necessários para orquestrar a replicação e a ativação pós-falha são enviados para o serviço de Recuperação de Sites.  

Recuperação de sites é ISO 27001: 2013, 27018, HIPAA, DPA e está no processo SOC2 e FedRAMP JAB.

### <a name="can-we-keep-on-premises-metadata-within-a-geographic-regions"></a>Vamos manter metadados no local dentro de um regiões geográficas
Sim. Quando criar um cofre numa região, podemos assegurar que todos os metadados utilizada pela recuperação de Site permanecem dentro do limite geográfico dessa região.

### <a name="does-site-recovery-encrypt-replication"></a>A Recuperação de Sites faz encriptação de replicação?
Sim, ambos os encriptação em trânsito e [encriptação no Azure](https://docs.microsoft.com/azure/storage/storage-service-encryption) são suportados.


## <a name="failover-and-failback"></a>Ativação pós-falha e reativação pós-falha
### <a name="how-far-back-can-i-recover"></a>Até que ponto novamente posso recuperar?
Para VMware para o Azure, o ponto de recuperação mais antigo, que pode utilizar é 72 horas.

### <a name="how-do-i-access-azure-vms-after-failover"></a>Como aceder a VMs do Azure após a ativação pós-falha
Após a ativação pós-falha, pode aceder a VMs do Azure através de uma ligação de Internet segura, através de uma VPN de site para site ou através do ExpressRoute do Azure. Terá de preparar um número de elementos para se ligar. [Saiba mais](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover)

### <a name="is-failed-over-data-resilient"></a>É efetuada a ativação pós-falha dados resilientes?
O Azure foi concebido para ser resiliente. Recuperação de sites é foi desenvolvida para ativação pós-falha para um datacenter secundário do Azure, de acordo com o SLA do Azure. Quando ocorre a ativação pós-falha, mas certifique-se os seus metadados e cofres permanecem na mesma região geográfica que escolheu para o cofre.

### <a name="is-failover-automatic"></a>A ativação pós-falha é automática?
[Ativação pós-falha](site-recovery-failover.md) não é automática. Iniciar as ativações pós-falha com único clique no portal ou pode utilizar [ PowerShell](/powershell/module/azurerm.siterecovery) para acionar uma ativação pós-falha.

### <a name="can-i-fail-back-to-a-different-location"></a>Pode falhar novamente para uma localização diferente?
Sim, se executar a ativação pós-falha para o Azure, pode efetuar para uma localização diferente se a um original não está disponível. [Saiba mais](concepts-types-of-failback.md#alternate-location-recovery-alr).

### <a name="why-do-i-need-a-vpn-or-expressroute-to-fail-back"></a>Por que motivo é necessário uma VPN ou ExpressRoute para falhar novamente?

Ao efetuar a reativação a partir do Azure, dados do Azure são copiados para a VM no local e acesso privado é necessário. 



## <a name="automation-and-scripting"></a>Automatização e processamento de scripts

### <a name="can-i-set-up-replication-with-scripting"></a>Pode definir a replicação com scripting?
Sim. Pode automatizar fluxos de trabalho de recuperação de sites utilizando a API Rest, PowerShell ou o SDK do Azure. [Mais](vmware-azure-disaster-recovery-powershell.md).

## <a name="performance-and-capacity"></a>Desempenho e capacidade
### <a name="can-i-throttle-replication-bandwidth"></a>Pode limitar a largura de banda de replicação?
Sim. [Saiba mais](site-recovery-plan-capacity-vmware.md).


## <a name="next-steps"></a>Passos Seguintes
* [Reveja](vmware-physical-azure-support-matrix.md) suportar requisitos.
* [Configurar](vmware-azure-tutorial.md) VMware para a replicação do Azure.
