---
title: Instalar o serviço de mobilidade (VMware ou físico para o Azure) | Documentos da Microsoft
description: Saiba como instalar o agente do serviço de mobilidade para proteger as suas VMs de VMware no local e servidores físicos com o Azure Site Recovery.
author: Rajeswari-Mamilla
ms.service: site-recovery
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: ramamill
ms.openlocfilehash: e98e67c453e909662c509598acc4cf65c5a0a6fa
ms.sourcegitcommit: 11321f26df5fb047dac5d15e0435fce6c4fde663
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/06/2018
ms.locfileid: "37887662"
---
# <a name="install-the-mobility-service"></a>Instalar o serviço de Mobilidade 

Serviço de mobilidade da recuperação de sites do Azure está instalado em VMs de VMware e servidores físicos que pretende replicar para o Azure. O serviço obtém dados, escreve num computador e, em seguida, encaminha-os para o servidor de processos. Implemente o serviço de mobilidade para todos os computadores (VMware VM ou servidor físico) que pretende replicar para o Azure. Pode implementar o serviço de mobilidade nos servidores e VMs de VMware que pretende proteger através dos seguintes métodos:


* [Instalar com ferramentas de implantação de software, como o System Center Configuration Manager](vmware-azure-mobility-install-configuration-mgr.md)
* [Instalar com a automatização do Azure e Desired State Configuration (DSC de automatização)](vmware-azure-mobility-deploy-automation-dsc.md)
* [Instalar manualmente a partir da interface do Usuário](vmware-azure-install-mobility-service.md#install-mobility-service-manually-by-using-the-gui)
* [Instalar manualmente a partir de um prompt de comando](vmware-azure-install-mobility-service.md#install-mobility-service-manually-at-a-command-prompt)
* [Instalar utilizando a instalação push do Site Recovery](vmware-azure-install-mobility-service.md#install-mobility-service-by-push-installation-from-azure-site-recovery)


>[!IMPORTANT]
> A partir da versão 9.7.0.0, em VMs do Windows, o serviço de mobilidade instalador também instala o mais recente disponível [agente de VM do Azure](../virtual-machines/extensions/features-windows.md#azure-vm-agent). Quando um computador efetua a ativação pós-falha para o Azure, o computador cumpre a pré-requisitos para a utilização de qualquer extensão de VM a instalação do agente.

## <a name="prerequisites"></a>Pré-requisitos
Conclua estes passos de pré-requisitos antes de instalar manualmente o serviço de mobilidade no seu servidor:
1. Inicie sessão no seu servidor de configuração e, em seguida, abra uma janela de linha de comandos como administrador.
2. Altere o diretório para a pasta bin e, em seguida, crie um ficheiro da frase de acesso.

    ```
    cd %ProgramData%\ASR\home\svsystems\bin
    genpassphrase.exe -v > MobSvc.passphrase
    ```
3. Store o ficheiro da frase de acesso numa localização segura. Utilize o ficheiro durante a instalação do serviço de mobilidade.
4. Programas de instalação de serviço de mobilidade para todos os sistemas operativos suportados estão na pasta %ProgramData%\ASR\home\svsystems\pushinstallsvc\repository.

### <a name="mobility-service-installer-to-operating-system-mapping"></a>Mapeamento sistema de instalador operacional do serviço de mobilidade

| Nome do modelo de ficheiro de instalador| Sistema operativo |
|---|--|
|Microsoft-ASR\_UA\*Windows\*release.exe | Windows Server 2008 R2 SP1 (64-bit) </br> Windows Server 2012 (64-bit) </br> Windows Server 2012 R2 (64-bit) </br> Windows Server 2016 (64-bit) |
|Microsoft-ASR\_UA\*RHEL6-64\*release.tar.gz| Red Hat Enterprise Linux (RHEL) 6.4, 6.5, 6.6, 6.7, 6.8, 6.9 (apenas 64 bits) </br> CentOS 6.4, 6.5, 6.6, 6.7, 6.8, 6.9 (apenas 64 bits) |
|Microsoft-ASR\_UA\*RHEL7-64\*release.tar.gz | Red Hat Enterprise Linux (RHEL) 7.1, 7.2, 7.3 (apenas 64 bits) </br> CentOS 7.0, 7.1, 7.2, 7.3 (apenas 64 bits) |
|Microsoft-ASR\_UA\*SLES11-SP3-64\*release.tar.gz| SUSE Linux Enterprise Server 11 SP3 (apenas 64 bits)|
|Microsoft-ASR\_UA\*SLES11-SP4-64\*release.tar.gz| SUSE Linux Enterprise Server 11 SP4 (apenas 64 bits)|
|Microsoft-ASR\_UA\*OL6-64\*release.tar.gz | Oracle Enterprise Linux 6.4, 6.5 (apenas 64 bits)|
|Microsoft-ASR\_UA\*UBUNTU-14.04-64\*release.tar.gz | Ubuntu Linux 14.04 (apenas 64 bits)|
|Microsoft-ASR\_UA\*UBUNTU-16.04-64\*release.tar.gz | Servidor de 16.04 LTS do Ubuntu Linux (apenas 64 bits)|
|Microsoft-ASR_UA\*DEBIAN7-64\*release.tar.gz | Debian 7 (apenas 64 bits)|
|Microsoft-ASR_UA\*DEBIAN8-64\*release.tar.gz | Debian 8 (apenas 64 bits)|


## <a name="install-mobility-service-manually-by-using-the-gui"></a>Instalar o serviço de mobilidade manualmente utilizando o GUI

>[!IMPORTANT]
> Se utilizar um servidor de configuração para replicar máquinas de virtuais de IaaS do Azure de uma subscrição do Azure/região para outro, use o método de instalação baseada na linha de comandos.

[!INCLUDE [site-recovery-install-mob-svc-gui](../../includes/site-recovery-install-mob-svc-gui.md)]

## <a name="install-mobility-service-manually-at-a-command-prompt"></a>Instalar o serviço de mobilidade manualmente num prompt de comando

### <a name="command-line-installation-on-a-windows-computer"></a>Instalação da linha de comandos num computador Windows
[!INCLUDE [site-recovery-install-mob-svc-win-cmd](../../includes/site-recovery-install-mob-svc-win-cmd.md)]

### <a name="command-line-installation-on-a-linux-computer"></a>Instalação da linha de comandos num computador Linux
[!INCLUDE [site-recovery-install-mob-svc-lin-cmd](../../includes/site-recovery-install-mob-svc-lin-cmd.md)]


## <a name="install-mobility-service-by-push-installation-from-azure-site-recovery"></a>Instalar o serviço de mobilidade através da instalação push do Azure Site Recovery
Pode fazer uma instalação push do serviço de mobilidade com o Site Recovery. Todos os computadores de destino tem de cumprir os seguintes pré-requisitos.

[!INCLUDE [site-recovery-prepare-push-install-mob-svc-win](../../includes/site-recovery-prepare-push-install-mob-svc-win.md)]

[!INCLUDE [site-recovery-prepare-push-install-mob-svc-lin](../../includes/site-recovery-prepare-push-install-mob-svc-lin.md)]


> [!NOTE]
Depois do serviço de mobilidade está instalado, no portal do Azure, selecione **+ replicar** para começar a proteger estas VMs.

## <a name="update-mobility-service"></a>Atualização do serviço de mobilidade

> [!WARNING]
> Certifique-se de que o servidor de configuração, servidores de processos de escalamento horizontal e quaisquer servidores de destino mestre que fazem parte da implementação são atualizados antes de começar a atualizar o serviço de mobilidade nos servidores protegidos.

1. No portal do Azure, navegue para o *nome do seu Cofre* > **itens replicados** vista.
2. Se o servidor de configuração já foi atualizado para a versão mais recente, verá uma notificação que lê a "atualização do agente de replicação de recuperação do novo Site está disponível. Clique para instalar."

     ![Janela de itens replicados](.\media\vmware-azure-install-mobility-service\replicated-item-notif.png)
3. Selecione a notificação para abrir a página de seleção de máquina virtual.
4. Selecione as máquinas virtuais que pretende atualizar o serviço de mobilidade no e selecione **OK**.

     ![Lista de VMS de itens de replicados](.\media\vmware-azure-install-mobility-service\update-okpng.png)

A tarefa de serviço de mobilidade de atualização é iniciada para cada uma das máquinas virtuais selecionadas.

> [!NOTE]
> [Leia mais](vmware-azure-manage-configuration-server.md) sobre como atualizar a palavra-passe para a conta utilizada para instalar o serviço de mobilidade.

## <a name="uninstall-mobility-service-on-a-windows-server-computer"></a>Desinstale o serviço de mobilidade num computador Windows Server
Utilize um dos seguintes métodos para desinstalar o serviço de mobilidade num computador Windows Server.

### <a name="uninstall-by-using-the-gui"></a>Desinstalar utilizando o GUI
1. No painel de controlo, selecione **programas**.
2. Selecione **servidor do Microsoft Azure Site Recovery Mobility Service/destino principal**e, em seguida, selecione **desinstalação**.

### <a name="uninstall-at-a-command-prompt"></a>Desinstalar uma linha de comandos
1. Abra uma janela de linha de comandos como administrador.
2. Para desinstalar o serviço de mobilidade, execute o seguinte comando:

    ```
    MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1} /L+*V "C:\ProgramData\ASRSetupLogs\UnifiedAgentMSIUninstall.log"
    ```

## <a name="uninstall-mobility-service-on-a-linux-computer"></a>Desinstale o serviço de mobilidade num computador Linux
1. No seu servidor do Linux, inicie sessão como um **raiz** utilizador.
2. Num terminal, vá para /user/local/ASR.
3. Para desinstalar o serviço de mobilidade, execute o seguinte comando:

    ```
    uninstall.sh -Y
    ```
