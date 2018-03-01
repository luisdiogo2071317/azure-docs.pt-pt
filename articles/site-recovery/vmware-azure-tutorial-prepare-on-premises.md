---
title: "Preparar servidores VMware no local para a recuperação após desastre de VMs de VMware para o Azure | Microsoft Docs"
description: "Saiba como preparar servidores VMware no local para a recuperação após desastre para o Azure com o serviço Azure Site Recovery."
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 02/27/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 6b5c16e5b4136acc06c4ace4b16e8eb0e3522d24
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/28/2018
---
# <a name="prepare-on-premises-vmware-servers-for-disaster-recovery-to-azure"></a>Preparar servidores VMware no local para a recuperação após desastre para o Azure

Este tutorial mostra como preparar a sua infraestrutura de VMware no local quando quer replicar VMs de VMware para o Azure. Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Preparar uma conta no servidor vCenter ou no anfitrião vSphere ESXi para automatizar a deteção de VMs
> * Preparar uma conta para a instalação automática do serviço de Mobilidade em VMs de VMware
> * Analisar os requisitos de servidor VMware
> * Analisar os requisitos de VM de VMware

Nesta série de tutoriais, mostramos como fazer a cópia de segurança de uma única VM com o Azure Site Recovery. Se tenciona proteger várias VMs de VMware, deve transferir a [ferramenta Planeador de Implementações](https://aka.ms/asr-deployment-planner) para a replicação de VMware. Esta ferramenta permite recolher informações sobre a compatibilidade das VMs, os discos por VM e as alterações a dados por disco. A ferramenta também abarca os requisitos de largura de banda da rede e a infraestrutura do Azure necessários para uma replicação e ativação pós-falha de teste bem-sucedidas. [Saiba mais](site-recovery-deployment-planner.md) sobre como executar a ferramenta.

Este é o segundo tutorial da série. Certifique-se de que [configurou os componentes do Azure](tutorial-prepare-azure.md) conforme descrito no tutorial anterior.

## <a name="prepare-an-account-for-automatic-discovery"></a>Preparar uma conta para a deteção automática

O Site Recovery precisa de ter acesso a servidores VMware para:

- Detetar automaticamente as VMs. É necessária, pelo menos, uma conta só de leitura.
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

O serviço de Mobilidade tem de ser instalado na VM que quer replicar. O Site Recovery instala este serviço automaticamente quando a replicação para a VM é ativada. Para a instalação automática, tem de preparar uma conta que o Site Recovery irá utilizar para aceder à VM. Irá especificar esta conta quando configurar a recuperação após desastre na consola do Azure.

1. Prepare um domínio ou uma conta local com permissões para instalar na VM.
2. Para instalar em VMs do Windows, se não estiver a utilizar uma conta de domínio, desative o Controlo de Acesso de Utilizador Remoto no computador local.
   - No registo, em **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System**, adicione a entrada DWORD **LocalAccountTokenFilterPolicy** com um valor igual a 1.
3. Para instalar em VMs do Linux, prepare uma conta raiz no servidor Linux de origem.


## <a name="check-vmware-server-requirements"></a>Verificar os requisitos de servidor VMware

Certifique-se de que os servidores VMware cumprem os seguintes requisitos.

**Componente** | **Requisito**
--- | ---
**Servidor vCenter** | vCenter 6.5, 6.0 ou 5.5
**Anfitrião vSphere** | vSphere 6.5, 6.0, 5.5

## <a name="check-vmware-vm-requirements"></a>Verificar os requisitos de VM de VMware

Certifique-se de que a VM cumpre os requisitos do Azure resumidos na tabela seguinte.

**Requisito de VM** | **Detalhes**
--- | ---
**Tamanho do disco do sistema operativo** | Até 2048 GB.
**Contagem de discos do sistema operativo** | 1
**Contagem de discos de dados** | 64 ou menos
**Tamanho do VHD do disco de dados** | Até 4095 GB
**Adaptadores de rede** | São suportados vários adaptadores
**VHD partilhado** | Não suportado
**Disco FC** | Não suportado
**Formato de disco rígido** | VHD ou VHDX.<br/><br/> Apesar de o formato VHDX não ser suportado atualmente no Azure, o Site Recovery converte automaticamente VHDX em VHD quando executa a ativação pós-falha para o Azure. Quando executa a reativação pós-falha para o plano local, as VMs continuam a utilizar o formato VHDX.
**BitLocker** | Não suportado. Desative antes de ativar a replicação para uma VM.
**Nome da VM** | Entre 1 e 63 carateres.<br/><br/> Limitado a letras, números e hífenes. O nome da VM tem de começar e terminar com uma letra ou um número.
**Tipo de VM** | Geração 1 - Linux ou Windows<br/><br/>Geração 2 - Apenas Windows

A VM tem também de estar a executar um sistema operativo suportado. Consulte a [Matriz de suporte do Site Recovery](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions) para obter uma lista completa das versões suportadas.

## <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Preparar a ligação para VMs do Azure após a ativação pós-falha

Durante um cenário de ativação pós-falha, pode querer ligar às suas VMs replicadas no Azure a partir da sua rede local.

Para ligar às VMs do Windows via RDP após a ativação pós-falha, faça o seguinte:

1. Para aceder através da Internet, ative o protocolo RDP na VM local antes da ativação pós-falha. Certifique-se de que são adicionadas regras de TCP e UDP para o perfil **Público** e que o protocolo RDP é permitido em **Firewall do Windows** > **Aplicações Permitidas** para todos os perfis.
2. Para aceder através da rede de VPNs, ative o protocolo RDP na máquina local. O protocolo RDP deve estar permitido em **Firewall do Windows** -> **Aplicações e funcionalidades permitidas** para redes de **Domínio e Privadas**.
   Verifique se a política de SAN do sistema operativo está definida como **OnlineAll**. [Saiba mais](https://support.microsoft.com/kb/3031135). Não devem haver atualizações do Windows pendentes na VM quando aciona uma ativação pós-falha. Se houverem, só poderá iniciar sessão na máquina virtual depois de a atualização estar concluída.
3. Na VM do Azure do Windows após a ativação pós-falha, consulte o **Diagnóstico de arranque**  para ver uma captura de ecrã da VM. Se não conseguir ligar-se, verifique se a VM está em execução e reveja estas [sugestões de resolução de problemas](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).

Para ligar às VMs do Linux via SSH após a ativação pós-falha, faça o seguinte:

1. Na máquina local antes da ativação pós-falha, verifique se o serviço Secure Shell está definido para iniciar automaticamente no arranque do sistema. Verifique se as regras de firewall permitem uma ligação SSH.

2. Na VM do Azure após a ativação pós-falha, permita ligações de entrada na porta SSH para as regras de grupo de segurança de rede na VM com ativação pós-falha e para a sub-rede do Azure à qual está ligada.
   [Adicione um endereço IP público](site-recovery-monitoring-and-troubleshooting.md) para a VM. Pode consultar o **Diagnóstico de arranque** para ver uma captura de ecrã da VM.

## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"]
> [Configurar a recuperação após desastre para o Azure para VMs de VMware](vmware-azure-tutorial.md)
