---
title: Preparar servidores VMware no local para a recuperação após desastre de VMs de VMware para o Azure | Microsoft Docs
description: Saiba como preparar servidores VMware no local para a recuperação após desastre para o Azure com o serviço Azure Site Recovery.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 07/06/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: facf8895770f890bfbbef946a32cc681f685e998
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/09/2018
ms.locfileid: "37915207"
---
# <a name="prepare-on-premises-vmware-servers-for-disaster-recovery-to-azure"></a>Preparar servidores VMware no local para a recuperação após desastre para o Azure

O [Azure Site Recovery](site-recovery-overview.md) contribui para a sua estratégia de continuidade comercial e recuperação após desastre (BCDR) ao manter as suas aplicações empresariais em funcionamento durante falhas planeadas e não planeadas. O Site Recovery gere e orquestra a recuperação após desastre de computadores no local e máquinas virtuais (VMs) do Azure, incluindo replicação, ativação pós-falha e recuperação.

- Este artigo é o segundo tutorial de uma série que lhe mostra como configurar a recuperação após desastre no Azure para VMs do VMware no local. No primeiro tutorial, [configurámos os componentes do Azure](tutorial-prepare-azure.md) necessários para a recuperação após desastre do VMware.
- Os tutoriais são concebidos para mostrar o caminho de implementação mais simples num cenário. Utilizam opções predefinidas sempre que possível e não mostram todas as definições e caminhos possíveis. 

Neste artigo, vamos mostrar-lhe como preparar o ambiente do VMware no local quando pretende replicar VMs do VMware para o Azure com o Azure Site Recovery. Saiba como:

> [!div class="checklist"]
> * Preparar uma conta no servidor vCenter ou no anfitrião vSphere ESXi para automatizar a deteção de VMs
> * Preparar uma conta para a instalação automática do serviço de Mobilidade em VMs de VMware
> * Analisar os requisitos de servidor VMware e de VM
> * Preparar a ligação para VMs do Azure após a ativação pós-falha



## <a name="prepare-an-account-for-automatic-discovery"></a>Preparar uma conta para a deteção automática

O Site Recovery precisa de ter acesso a servidores VMware para:

- Detetar automaticamente VMs. É necessária, pelo menos, uma conta só de leitura.
- Orquestrar a replicação, a ativação pós-falha e a reativação pós-falha. Precisa de uma conta com capacidade para executar operações como criar e remover discos, e ligar VMs.

Crie a conta da seguinte forma:

1. Para utilizar uma conta dedicada, crie uma função ao nível do vCenter. Dê um nome à função, como **Azure_Site_Recovery**.
2. Atribua à função as permissões resumidas na tabela abaixo.
3. Crie um utilizador no servidor vCenter ou no anfitrião vSphere. Atribua a função ao utilizador.

### <a name="vmware-account-permissions"></a>Permissões de conta de VMware

**Tarefa** | **Função/Permissões** | **Detalhes**
--- | --- | ---
**Deteção de VMs** | Pelo menos um utilizador só de leitura<br/><br/> Objeto Data Center –> Propagar ao Objeto Subordinado, função=Só de Leitura | Utilizador atribuído ao nível do datacenter, com acesso a todos os objetos no datacenter.<br/><br/> Para restringir o acesso, atribua a função **Sem acesso** com **Propagar ao objeto subordinado** aos objetos subordinados (anfitriões vSphere, arquivos de dados, VMs e redes).
**Replicação, ativação pós-falha e reativação pós-falha completas** |  Criar uma função (Azure_Site_Recovery) com as permissões necessárias e, em seguida, atribuir a função a um grupo ou utilizador de VMware<br/><br/> Objeto Data Center –> Propagar ao Objeto Subordinado, função=Azure_Site_Recovery<br/><br/> Arquivo de Dados -> Alocar espaço, navegar no arquivo de dados, operações de ficheiro de baixo nível, remover ficheiros, atualizar ficheiros de máquinas virtuais<br/><br/> Rede -> Atribuição de rede<br/><br/> Recursos -> Atribuir VM a agrupamento de recursos, migrar VMs desligadas, migrar VMs ligadas<br/><br/> Tarefas -> Criar tarefa, atualizar tarefa<br/><br/> Máquina virtual -> Configuração<br/><br/> Máquina virtual -> Interagir -> responder a perguntas, ligação de dispositivos, configurar suportes de dados em CD, configurar suportes de dados em disquete, desligar, ligar, instalação de ferramentas de VMware<br/><br/> Máquina virtual -> Inventário -> Criar, registar, anular o registo<br/><br/> Máquina virtual -> Aprovisionamento -> Permitir transferência de máquinas virtuais, permitir carregamento de ficheiros de máquinas virtuais<br/><br/> Máquina virtual -> Instantâneos -> Remover instantâneos | Utilizador atribuído ao nível do datacenter, com acesso a todos os objetos no datacenter.<br/><br/> Para restringir o acesso, atribua a função **Sem acesso** com **Propagar ao objeto subordinado** aos objetos subordinados (anfitriões vSphere, arquivos de dados, VMs e redes).

## <a name="prepare-an-account-for-mobility-service-installation"></a>Preparar uma conta para a instalação do serviço de Mobilidade

O Serviço de mobilidade tem de ser instalado em cada computador que queira replicar. O Site Recovery pode efetuar uma instalação push deste serviço quando ativar a replicação para um computador, pode instalá-lo manualmente ou através de ferramentas de instalação.

- Neste tutorial, vamos instalar o Serviço de mobilidade com a instalação push.
- Para a instalação push, tem de preparar uma conta que o Site Recovery pode utilizar para aceder à VM. Esta conta é especificada quando configurar a recuperação após desastre na consola do Azure.

Prepare a conta da seguinte forma:

Prepare um domínio ou uma conta local com permissões para instalar na VM.

- **VMs do Windows**: para instalar em VMs do Windows, se não estiver a utilizar uma conta de domínio, desative o Controlo de Acesso de Utilizador Remoto no computador local. Para fazer isto, no registo > **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System**, adicione a entrada DWORD **LocalAccountTokenFilterPolicy**, com um valor de 1.
- **VMs do Linux**: para instalar em VMs do Linux, prepare uma conta raiz no servidor Linux de origem.


## <a name="check-vmware-requirements"></a>Verificar os requisitos de VMware

Certifique-se de que os servidores do VMware e as VM cumprem os requisitos.

1. [Verifique](vmware-physical-azure-support-matrix.md#on-premises-virtualization-servers) os requisitos do servidor do VMware.
2. Para as VMs do Linux, [verifique](vmware-physical-azure-support-matrix.md#linux-file-systemsguest-storage) o sistema de ficheiros e os requisitos de armazenamento. 
3. Verifique a [rede](vmware-physical-azure-support-matrix.md#network) local e o suporte de [armazenamento](vmware-physical-azure-support-matrix.md#storage). 
4. Verifique o que é suportado para [redes do Azure](vmware-physical-azure-support-matrix.md#azure-vm-network-after-failover), [armazenamento](vmware-physical-azure-support-matrix.md#azure-storage), e [computação](vmware-physical-azure-support-matrix.md#azure-compute), após a ativação pós-falha.
5. As suas VM no local que replica para o Azure devem estar em conformidade com os [requisitos de VM do Azure](vmware-physical-azure-support-matrix.md#azure-vm-requirements).


## <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Preparar a ligação para VMs do Azure após a ativação pós-falha

Após a ativação pós-falha, pode querer ligar às VMs do Azure a partir da sua rede no local.

Para ligar às VMs do Windows via RDP após a ativação pós-falha, faça o seguinte:

- **Acesso à Internet**. Antes da ativação pós-falha, ative o protocolo RDP na VM local. Confirme que são adicionadas regras de TCP e UDP ao perfil **Público** e que o protocolo RDP é permitido em **Firewall do Windows** > **Aplicações Permitidas** para todos os perfis.
- **Acesso de rede de VPNs**:
    - Antes da ativação pós-falha, ative o protocolo RDP no computador local.
    - O protocolo RDP deve estar permitido em **Firewall do Windows** -> **Aplicações e funcionalidades permitidas** para redes de **Domínio e Privadas**.
    - Verifique se a política de SAN do sistema operativo está definida como **OnlineAll**. [Saiba mais](https://support.microsoft.com/kb/3031135).
- Não devem haver atualizações do Windows pendentes na VM quando aciona uma ativação pós-falha. Se houverem, só poderá iniciar sessão na máquina virtual depois de a atualização estar concluída.
- Na VM do Azure do Windows após a ativação pós-falha, consulte o **Diagnóstico de arranque**  para ver uma captura de ecrã da VM. Se não conseguir ligar-se, verifique se a VM está em execução e reveja estas [sugestões de resolução de problemas](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).

Para ligar às VMs do Linux via SSH após a ativação pós-falha, faça o seguinte:

- No computador local, antes da ativação pós-falha, verifique se o serviço Secure Shell está definido para iniciar automaticamente no arranque do sistema.
- Verifique se as regras de firewall permitem uma ligação SSH.
- Na VM do Azure após a ativação pós-falha, permita ligações de entrada na porta SSH para as regras de grupo de segurança de rede na VM com ativação pós-falha e para a sub-rede do Azure à qual está ligada.
- [Adicione um endereço IP público](site-recovery-monitoring-and-troubleshooting.md) para a VM.
- Pode consultar o **Diagnóstico de arranque** para ver uma captura de ecrã da VM.

## <a name="useful-links"></a>Ligações úteis

Se estiver a replicar várias VMs, deverá planear a capacidade e a implementação antes de começar. [Saiba mais](site-recovery-deployment-planner.md).



## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"]
> [Configurar a recuperação após desastre para o Azure para VMs de VMware](vmware-azure-tutorial.md)
