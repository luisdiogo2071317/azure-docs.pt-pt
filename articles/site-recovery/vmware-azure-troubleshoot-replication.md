---
title: Resolver problemas de replicação para a recuperação após desastre de VMs de VMware e servidores físicos para o Azure com o Azure Site Recovery | Documentos da Microsoft
description: Este artigo fornece informações de resolução de problemas de replicação comuns durante a recuperação após desastre de VMs de VMware e servidores físicos para o Azure com o Azure Site Recovery.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 12/17/2018
ms.author: ramamill
ms.openlocfilehash: 1c37b764b47856d3a369228d3f224f2a464029bb
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/27/2018
ms.locfileid: "53790661"
---
# <a name="troubleshoot-replication-issues-for-vmware-vms-and-physical-servers"></a>Resolver problemas de replicação de VMs de VMware e servidores físicos

Poderá receber uma mensagem de erro específico ao proteger as suas máquinas de virtuais de VMware ou servidores físicos com o Azure Site Recovery. Este artigo descreve alguns problemas comuns que poderá encontrar ao replicar no local VMs de VMware e servidores físicos para o Azure com [do Azure Site Recovery](site-recovery-overview.md).


## <a name="initial-replication-issues"></a>Problemas da replicação inicial

Em muitos casos, as falhas de replicação inicial que podemos encontrar no suporte são devido a problemas de conectividade entre o servidor de processo do servidor de origem ou processo server para o Azure. Na maioria dos casos, pode solucionar esses problemas, seguindo os passos listados abaixo.

### <a name="verify-the-source-machine"></a>Certifique-se a máquina de origem
* Na linha de comando de máquina do servidor de origem, use o Telnet para executar ping no servidor de processo com a porta de https (predefinição 9443), conforme mostrado abaixo para ver se existem problemas de conectividade de rede ou problemas de bloqueio de porta de firewall.

    `telnet <PS IP address> <port>`
> [!NOTE]
    > Utilizar o Telnet, não utilize o PING para testar a conectividade.  Se o Telnet não estiver instalado, siga a lista de passos [aqui](https://technet.microsoft.com/library/cc771275(v=WS.10).aspx)

Se não conseguir ligar, permitir a entrada de porta 9443 no servidor de processos e verifique se o problema ainda é encerrado. Há alguns casos em que o servidor de processos estava por trás da rede de Perímetro, o que estava causando esse problema.

* Verificar o estado do serviço `InMage Scout VX Agent – Sentinel/OutpostStart` se não estiver em execução e verifique se o problema ainda existe.   

### <a name="verify-the-process-server"></a>Verifique se o servidor de processos

* **Verifique se o servidor de processos ativamente é enviar dados para o Azure**

A partir da máquina do servidor de processos, abra o Gestor de tarefas (prima Ctrl-Shift-Esc). Vá até a guia de desempenho e clique em ligação de "Open Monitor de recursos". Do Resource Manager, aceda ao separador de rede. Verifique se cbengine.exe em "Processos com a atividade de rede" ativamente é o envio de grande volume (em MB) de dados.

![Ativar a replicação](./media/vmware-azure-troubleshoot-replication/cbengine.png)

Caso contrário, siga os passos listados abaixo:

* **Verifique se o servidor de processos é capaz de ligar os BLOBs do Azure**: Selecione e verifique cbengine.exe para ver as "conexões TCP" para ver se existe conectividade entre o servidor de processos e o URL do blob de armazenamento do Azure.

![Ativar a replicação](./media/vmware-azure-troubleshoot-replication/rmonitor.png)

Se não, em seguida, aceda ao painel de controlo > serviços, verifique se os seguintes serviços estão em execução:

     * cxprocessserver
     * InMage Scout VX Agent – Sentinel/Outpost
     * Microsoft Azure Recovery Services Agent
     * Microsoft Azure Site Recovery Service
     * tmansvc
     *
(Re) Comece a qualquer serviço, que não está em execução e verifique se o problema ainda existe.

* **Verifique se o servidor de processos é capaz de se ligar ao endereço IP público do Azure através da porta 443**

Abra o CBEngineCurr.errlog mais recente do `%programfiles%\Microsoft Azure Recovery Services Agent\Temp` , procure por: 443 e ligação tentativa falhada.

![Ativar a replicação](./media/vmware-azure-troubleshoot-replication/logdetails1.png)

Se existirem problemas, em seguida, na linha de comando do servidor de processos, utilizam o telnet para enviar um ping de seu endereço de IP público do Azure (mascarado no acima imagem) encontrado na CBEngineCurr.currLog através da porta 443.

      telnet <your Azure Public IP address as seen in CBEngineCurr.errlog>  443
Se não for possível estabelecer ligação, em seguida, verifique se o problema de acesso é devido a firewall ou Proxy, conforme descrito no passo seguinte.


* **Verifique se firewall de baseadas no endereço IP no servidor de processos não está a bloquear o acesso**: Se estiver a utilizar um regras de firewall baseadas no endereço IP no servidor, em seguida, transfira a lista completa de Microsoft Azure intervalos IP do Datacenter do [aqui](https://www.microsoft.com/download/details.aspx?id=41653) e adicioná-los à sua configuração de firewall para se certificar de que permitem a comunicação para O Azure (e a porta HTTPS (443)).  Permita os intervalos de endereços IP da região do Azure da sua subscrição e para a região E.U.A. Oeste (utilizada para Controlo de Acesso e Gestão de Identidades).

* **Verifique se firewall baseado em URL no servidor de processos não está a bloquear o acesso**:  Se estiver a utilizar regras de firewall baseado em URL no servidor, certifique-se de que os seguintes URLs são adicionados à configuração da firewall.

[!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]  

* **Verifique se as definições de Proxy no servidor de processos não estão a bloquear o acesso**.  Se estiver a utilizar um servidor Proxy, certifique-se de que o nome do servidor proxy está a resolver pelo servidor DNS.
Para verificar o que fornecidos no momento da configuração do servidor de configuração. Vá para a chave de registo

    `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Azure Site Recovery\ProxySettings`

Agora Certifique-se de que as mesmas definições estão a ser utilizadas pelo agente de recuperação de sites do Azure para enviar dados.
Pesquisa do Microsoft Azure Backup

Abra-o e clique em ação > alterar propriedades. No separador de configuração de Proxy, deverá ver o endereço de proxy, que deve ser a mesmo, conforme mostrado pelas definições de registo. Caso contrário,. Altere-o para o mesmo endereço.


* **Verifique se a limitação de largura de banda não está restrito no servidor de processos**:  Aumentar a largura de banda e verificar se o problema ainda existe.

## <a name="source-machine-to-be-protected-through-site-recovery-is-not-listed-on-azure-portal"></a>Máquina de origem a ser protegido por meio do Site Recovery não está listada no portal do Azure

Ao tentar escolher a máquina de origem para ativar a replicação através do Azure Site Recovery, a máquina pode não está disponível para que continue pelos seguintes motivos

* Se existirem duas máquinas virtuais sob o vCenter com a mesma instância UUID, em seguida, a primeira máquina virtual detetada pelo servidor de configuração são apresentados no portal. Para resolver, certifique-se de que não existem duas máquinas virtuais têm a mesma instância UUID.
* Certifique-se de que adicionou as credenciais do vCenter correto durante o conjunto de cópia de segurança da configuração através do modelo OVF ou conjunto unificado de cópia de segurança. Para verificar as credenciais de adicionadas, consulte as diretrizes partilhadas [aqui](vmware-azure-manage-configuration-server.md#modify-credentials-for-automatic-discovery).
* Se as permissões fornecidas para aceder ao vCenter não tem privilégios suficientes, pode levar a falhas em detetar as máquinas virtuais. Certifique-se as permissões fornecidas [aqui](vmware-azure-tutorial-prepare-on-premises.md#prepare-an-account-for-automatic-discovery) são adicionados para a conta de utilizador do vCenter.
* Se a máquina virtual já está protegida por meio do Site Recovery, em seguida, ele não estarão disponível para proteção. Certifique-se de que a máquina virtual que está procurando no portal não já está protegida por qualquer outro utilizador ou em outras subscrições.

## <a name="protected-virtual-machines-are-greyed-out-in-the-portal"></a>Máquinas virtuais protegidas são cinzento out no portal

Máquinas virtuais que são replicadas na recuperação de sites estão esbatidas se existem entradas duplicadas no sistema. Consulte as diretrizes dadas [aqui](https://social.technet.microsoft.com/wiki/contents/articles/32026.asr-vmware-to-azure-how-to-cleanup-duplicatestale-entries.aspx) para eliminar entradas obsoletas e resolver o problema.

## <a name="next-steps"></a>Passos Seguintes
Se precisar de mais ajuda, em seguida, publique a sua consulta [fórum do Azure Site Recovery](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr). Temos uma Comunidade ativa e um dos nossos engenheiros será capaz de ajudá-lo.
