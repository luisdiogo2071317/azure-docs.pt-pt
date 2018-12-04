---
title: Configurar a recuperação após desastre para VMs do Azure após a migração para o Azure com o Azure Site Recovery | Documentos da Microsoft
description: Este artigo descreve como preparar as máquinas para configurar a recuperação após desastre entre regiões do Azure após a migração para o Azure com o Azure Site Recovery.
services: site-recovery
author: ponatara
ms.service: site-recovery
ms.topic: article
ms.date: 11/27/2018
ms.author: ponatara
ms.openlocfilehash: 274a69c6a2c23caf391a636ce53a9bb3897c0aa2
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52836071"
---
# <a name="set-up-disaster-recovery-for-azure-vms-after-migration-to-azure"></a>Configurar a recuperação após desastre de VMs do Azure após a migração para o Azure 


Utilize este artigo depois [migrar máquinas no local para VMs do Azure](tutorial-migrate-on-premises-to-azure.md) utilizando o [Site Recovery](site-recovery-overview.md) serviço. Este artigo ajuda-o a preparar as VMs do Azure para configurar a recuperação após desastre para uma região secundária do Azure, com o Site Recovery.



## <a name="before-you-start"></a>Antes de começar

Antes de configurar a recuperação após desastre, certifique-se de que a migração foi concluída conforme esperado. Para concluir uma migração com êxito, após a ativação pós-falha, deve selecionar o **concluir a migração** opção, para cada máquina que pretende migrar. 



## <a name="install-the-azure-vm-agent"></a>Instalar o agente de VM do Azure

O Azure [agente da VM](../virtual-machines/extensions/agent-windows.md) tem de estar instalado na VM, para que o Site Recovery pode replicá-la.


1. Para instalar o agente da VM em VMs que executam o Windows, transfira e execute o [instalador do agente](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). São necessários privilégios de administrador na VM para concluir a instalação.
2. Para instalar o agente da VM em VMs que executem o Linux, instale a versão mais recente [agente Linux](../virtual-machines/extensions/agent-linux.md). São necessários privilégios de administrador para concluir a instalação. É recomendável que instalar a partir do seu repositório de distribuição. Não é recomendada a instalação do agente de VM com Linux diretamente a partir do GitHub. 


## <a name="validate-the-installation-on-windows-vms"></a>Validar a instalação em VMs do Windows

1. Na VM do Azure, na pasta C:\WindowsAzure\Packages, deverá ver o ficheiro de WaAppAgent.exe.
2. O arquivo, com o botão direito e, em **propriedades**, selecione a **detalhes** separador.
3. Certifique-se de que o **versão do produto** campo mostra 2.6.1198.718 ou superior.



## <a name="migration-from-vmware-vms-or-physical-servers"></a>Migração de VMs de VMware ou servidores físicos

Se migrar VMs de VMware no local (ou servidores físicos) para o Azure, tenha em atenção que:

- Só tem de instalar o agente de VM do Azure se o serviço de mobilidade instalado na máquina migrada é v9.6 ou anterior.
- Em VMs do Windows com a versão de 9.7.0.0 do serviço de mobilidade e posteriores, o instalador do serviço instala o agente de VM do Azure mais recente disponível. Quando migra, estas VMs já cumprem a pré-requisitos para qualquer extensão VM, incluindo a extensão de recuperação de sites a instalação do agente.
- Terá de desinstalar manualmente o serviço de mobilidade da VM do Azure, através de um dos seguintes métodos. Reinicie a VM antes de configurar a replicação.
    - Para Windows, no painel de controlo > **Adicionar/remover programas**, desinstalar **servidor do Microsoft Azure Site Recovery Mobility Service/destino principal**. Na linha de comandos elevada, execute:
        ```
        MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1} /L+*V "C:\ProgramData\ASRSetupLogs\UnifiedAgentMSIUninstall.log"
        ```
    - Para o Linux, inicie sessão como utilizador raiz. Num terminal, aceda a **/user/local/ASR**, e execute o seguinte comando:
        ```
        uninstall.sh -Y
        ```


## <a name="next-steps"></a>Passos Seguintes

[Replicar rapidamente](azure-to-azure-quickstart.md) uma VM do Azure para uma região secundária.
