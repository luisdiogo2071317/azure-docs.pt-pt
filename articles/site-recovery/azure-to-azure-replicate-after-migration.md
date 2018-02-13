---
title: "Configurar a recuperação após desastre para as VMs do Azure após a migração para o Azure com o Azure Site Recovery | Microsoft Docs"
description: "Este artigo descreve como preparar máquinas para configurar a recuperação após desastre entre regiões do Azure após a migração para o Azure utilizando o Azure Site Recovery."
services: site-recovery
author: ponatara
ms.service: site-recovery
ms.topic: article
ms.date: 01/07/2018
ms.author: ponatara
ms.openlocfilehash: c06af21cd6e273b98c004e8bd0e6eac61ba7d644
ms.sourcegitcommit: 828cd4b47fbd7d7d620fbb93a592559256f9d234
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/18/2018
---
# <a name="set-up-disaster-recovery-for-azure-vms-after-migration-to-azure"></a>Configurar a recuperação após desastre para as VMs do Azure após a migração para o Azure 

>[!NOTE]
> Recuperação após desastre para as VMs do Azure utilizando o Azure Site Recovery está atualmente em pré-visualização.

Utilize este artigo depois de já [migrado máquinas no local para as VMs do Azure](tutorial-migrate-on-premises-to-azure.md) utilizando o [recuperação de Site](site-recovery-overview.md) serviço. Este artigo ajuda-o a preparar as VMs do Azure para configurar a recuperação de desastres para uma região secundária do Azure, utilizando a recuperação de sites.



## <a name="before-you-start"></a>Antes de começar

Antes de configurar a recuperação após desastre, certifique-se de que a migração foi concluída conforme esperado. Para concluir uma migração com êxito, após a ativação pós-falha, deve selecionar o **concluir a migração** opção, para cada máquina que pretende migrar. 



## <a name="install-the-azure-vm-agent"></a>Instalar o agente da VM do Azure

O Azure [agente da VM](../virtual-machines/windows/agent-user-guide.md) tem de estar instalado na VM, para que a recuperação de sites pode replicá-lo.


1. Para instalar o agente da VM em VMs do Windows em execução, transfira e execute o [instalador do agente](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Necessita de privilégios de administrador na VM para concluir a instalação.
2. Para instalar o agente da VM em VMs do Linux em execução, instale a versão mais recente [agente Linux](../virtual-machines/linux/agent-user-guide.md). Necessita de privilégios de administrador para concluir a instalação. Recomendamos que instale a partir do repositório de distribuição. Recomendamos que não instale o agente de VM com Linux diretamente a partir do GitHub. 


## <a name="validate-the-installation-on-windows-vms"></a>Validar a instalação em VMs do Windows

1. Na VM do Azure, na pasta C:\WindowsAzure\Packages, deverá ver o ficheiro WaAppAgent.exe.
2. O ficheiro com o botão direito e na **propriedades**, selecione o **detalhes** separador.
3. Certifique-se de que o **versão do produto** campo mostra 2.6.1198.718 ou superior.



## <a name="migration-from-vmware-vms-or-physical-servers"></a>Migração a partir de VMs de VMware ou servidores físicos

Se migrar VMs de VMware no local (ou servidores físicos) para o Azure, tenha em atenção que:

- Só precisa de instalar o agente da VM do Azure, se o serviço de mobilidade instalado na máquina migrada v9.6 ou anterior.
- Em VMs do Windows com a versão 9.7.0.0 do serviço de mobilidade em diante, o instalador do serviço instala o agente de VM do Azure mais recente disponível. Quando migra, estas VMs já cumprem a instalação do agente pré-requisitos para qualquer extensão VM, incluindo a extensão de recuperação de sites.
- Terá de desinstalar manualmente o serviço de mobilidade de VM do Azure, utilizando um dos seguintes métodos. Reinicie a VM antes de configurar a replicação.
    - Para o Windows, no painel de controlo > **Adicionar/remover programas**, desinstalar **servidor de destino mestre do serviço de mobilidade recuperação Microsoft Azure Site**. Numa linha de comandos elevada, execute:
        ```
        MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1} /L+*V "C:\ProgramData\ASRSetupLogs\UnifiedAgentMSIUninstall.log"
        ```
    - Para Linux, iniciar sessão um utilizador raiz. Um terminal, aceda a **/user/local/ASR**, e execute o seguinte comando:
        ```
        uninstall.sh -Y
        ```


## <a name="next-steps"></a>Passos Seguintes

[Replicar rapidamente](azure-to-azure-quickstart.md) uma VM do Azure para uma região secundária.
