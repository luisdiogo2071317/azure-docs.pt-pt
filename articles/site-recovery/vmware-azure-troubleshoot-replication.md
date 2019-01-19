---
title: Resolver problemas de replicação para a recuperação após desastre de VMs de VMware e servidores físicos para o Azure com o Azure Site Recovery | Documentos da Microsoft
description: Este artigo fornece informações de resolução de problemas de replicação comuns durante a recuperação após desastre de VMs de VMware e servidores físicos para o Azure com o Azure Site Recovery.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 01/18/2019
ms.author: ramamill
ms.openlocfilehash: 5c2d33b39614ded95ac38e07c844b0a8cafa7cd2
ms.sourcegitcommit: 82cdc26615829df3c57ee230d99eecfa1c4ba459
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/19/2019
ms.locfileid: "54411480"
---
# <a name="troubleshoot-replication-issues-for-vmware-vms-and-physical-servers"></a>Resolver problemas de replicação de VMs de VMware e servidores físicos

Poderá ver uma mensagem de erro específico quando protege as máquinas virtuais VMware ou servidores físicos com o Azure Site Recovery. Este artigo descreve alguns problemas comuns que poderá encontrar ao replicar VMs de VMware no local e servidores físicos para o Azure, utilizando [recuperação de Site](site-recovery-overview.md).

## <a name="initial-replication-issues"></a>Problemas da replicação inicial

Falhas de replicação inicial, muitas vezes, são causadas por problemas de conectividade entre o servidor de origem e o servidor de processos ou entre o servidor de processos e o Azure. Na maioria dos casos, pode solucionar esses problemas, concluindo os passos nas secções seguintes.

### <a name="check-the-source-machine"></a>Verificação de máquina de origem

As seguintes lista mostra formas como pode verificar a máquina de origem:

*  Na linha de comandos no servidor de origem, use o Telnet para executar ping no servidor de processo através da porta HTTPS (a porta HTTPS predefinida é 9443) ao executar o comando seguinte. O comando verifica para problemas de conectividade de rede e para problemas que bloquear a porta de firewall.


   `telnet <process server IP address> <port>`


   > [!NOTE]
   > Utilize o Telnet para testar a conectividade. Não utilize `ping`. Se o Telnet não estiver instalado, conclua os passos listados em [instalar o cliente Telnet](https://technet.microsoft.com/library/cc771275(v=WS.10).aspx).

   Se não conseguir ligar ao servidor de processos, permita a entrada de porta 9443 no servidor de processos. Por exemplo, poderá ter de permitir a entrada de porta 9443 no servidor de processos, se a rede tiver uma rede de perímetro ou sub-rede filtrada. Em seguida, verifique se o problema ainda ocorre.

*  Verificar o estado do **InMage Scout VX Agent – Sentinel/OutpostStart** serviço. Se o serviço não está em execução, inicie o serviço e, em seguida, verifique se o problema ainda ocorre.   

### <a name="check-the-process-server"></a>Verificar o servidor de processos

As seguintes lista mostra formas como pode verificar o servidor de processos:

*  **Verifique se o servidor de processos está ativamente enviar dados por push para o Azure**.

   1. No servidor de processos, abra o Gestor de tarefas (prima Ctrl + Shift + Esc).
   2. Selecione o **desempenho** separador e, em seguida, selecione a **abra o Monitor de recursos** ligação. 
   3. Sobre o **Monitor de recursos** página, selecione a **rede** separador. Sob **processos com a atividade de rede**, verifique se **cbengine.exe** ativamente está enviando um grande volume de dados.

        ![Captura de ecrã que mostra os volumes em processos com a atividade de rede](./media/vmware-azure-troubleshoot-replication/cbengine.png)

   Se cbengine.exe não está a enviar um grande volume de dados, conclua os passos nas secções seguintes.

*  **Verifique se o servidor de processos pode ligar ao armazenamento de Blobs do Azure**.

   Selecione **cbengine.exe**. Sob **conexões TCP**, verifique se existe conectividade entre o servidor de processos e o URL de armazenamento do blogue do Azure.

   ![Captura de ecrã que mostra a conectividade entre cbengine.exe e o URL de armazenamento de Blobs do Azure](./media/vmware-azure-troubleshoot-replication/rmonitor.png)

   Se nenhuma conectividade do servidor de processos para o URL de armazenamento do blogue do Azure, no painel de controlo, selecione **serviços**. Verifique se os seguintes serviços estão em execução:

   *  cxprocessserver
   *  InMage Scout VX Agent – Sentinel/Outpost
   *  Agente dos Serviços de Recuperação do Microsoft Azure
   *  Serviço Microsoft Azure Site Recovery
   *  tmansvc

   Iniciar ou reiniciar qualquer serviço que não está em execução. Verifique se o problema ainda ocorre.

*  **Verifique se o servidor de processos pode ligar-se para o endereço IP público do Azure através da porta 443**.

   Em %programfiles%\Microsoft Agent\Temp de serviços de recuperação do Azure, abra o ficheiro de CBEngineCurr.errlog mais recente. No ficheiro, procure **443** ou para a cadeia de caracteres **tentativa de ligação falhada**.

   ![Captura de ecrã que mostra o erro registos na pasta Temp](./media/vmware-azure-troubleshoot-replication/logdetails1.png)

   Se são apresentados os problemas, na linha de comandos no servidor de processos, utilize o Telnet para enviar um ping de seu endereço IP público do Azure (o endereço IP está oculto na imagem anterior). Pode encontrar o endereço IP público do Azure no ficheiro CBEngineCurr.currLog através da porta 443:

   `telnet <your Azure Public IP address as seen in CBEngineCurr.errlog>  443`

   Se não conseguir ligar, verifique se o problema de acesso é devido a definições de firewall ou proxy, tal como descrito no passo seguinte.

*  **Verifique se o firewall baseadas no endereço IP no servidor de processos bloqueia o acesso**.

   Se utilizar regras de firewall baseadas no endereço IP no servidor, transferir a lista completa de [intervalos IP de datacenter do Microsoft Azure](https://www.microsoft.com/download/details.aspx?id=41653). Adicione os intervalos de endereços IP à sua configuração de firewall para se certificar de que a firewall permite a comunicação para o Azure (e a porta HTTPS predefinida, 443). Permita intervalos de endereços IP para a região do Azure da sua subscrição e para a região do Azure-oeste (utilizada para gestão de identidades e de controlo de acesso).

*  **Verificar se um firewall baseado em URL no servidor de processos bloqueia o acesso**.

   Se utilizar uma regra de firewall baseado em URL no servidor, adicione os URLs indicados na tabela a seguir para a configuração de firewall:

[!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]  

*  **Verifique se as definições de proxy no servidor de processos bloquear o acesso**.

   Se utilizar um servidor proxy, certifique-se de que o nome do servidor proxy é resolvido pelo servidor DNS. Para verificar o valor que forneceu quando configurou o servidor de configuração, vá para a chave de registo **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Azure Site Recovery\ProxySettings**.

   Em seguida, certifique-se de que as mesmas definições são utilizadas pelo agente de recuperação de sites do Azure para enviar dados: 
      
   1. Procure **do Microsoft Azure Backup**. 
   2. Open **Microsoft Azure Backup**e, em seguida, selecione **ação** > **alterar propriedades**. 
   3. Sobre o **configuração do Proxy** separador, deverá ver o endereço de proxy. O endereço de proxy deve ser a mesmo que o endereço de proxy que é mostrado nas definições de registo. Caso contrário, alterá-lo para o mesmo endereço.

*  **Verifique se a largura de banda de limitação é restrita no servidor de processos**.

   Aumentar a largura de banda e, em seguida, verifique se o problema ainda ocorre.

## <a name="source-machine-isnt-listed-in-the-azure-portal"></a>Máquina de origem não estiver listada no portal do Azure

Quando tenta selecionar a máquina de origem para ativar a replicação utilizando a recuperação de Site, a máquina pode não estar disponível para um dos seguintes motivos:

* **Duas máquinas virtuais com o mesmo UUID da instância**: Se duas máquinas virtuais sob o vCenter tiver a mesma instância UUID, a primeira máquina virtual detetada pelo servidor de configuração é apresentada no portal do Azure. Para resolver este problema, certifique-se de que não existem duas máquinas virtuais têm a mesma instância UUID. Este cenário é comum visto em casos em que uma VM de cópia de segurança é ativada e está registada na nossa registos de deteção. Consulte [do Azure Site Recovery VMware para o Azure: Como limpar entradas duplicadas ou obsoletas](https://social.technet.microsoft.com/wiki/contents/articles/32026.asr-vmware-to-azure-how-to-cleanup-duplicatestale-entries.aspx) para resolver.
* **As credenciais de utilizador do vCenter incorreto**: Certifique-se de que adicionou as credenciais do vCenter correto quando configurou o servidor de configuração com o modelo OVF ou a configuração unificada. Para verificar as credenciais que foi adicionado durante a configuração, consulte [modificar as credenciais para a deteção automática](vmware-azure-manage-configuration-server.md#modify-credentials-for-automatic-discovery).
* **privilégios insuficientes do vCenter**: Se as permissões fornecidas para aceder ao vCenter não tem as permissões necessárias, pode ocorrer Falha ao detetar máquinas virtuais. Certifique-se de que as permissões descritas [preparar uma conta para a deteção automática](vmware-azure-tutorial-prepare-on-premises.md#prepare-an-account-for-automatic-discovery) são adicionados para a conta de utilizador do vCenter.
* **Servidores de gestão do Azure Site Recovery**: Se a máquina virtual for utilizada como servidor de gestão numa ou mais das seguintes funções - configuração /scale-out de servidor servidor de processos / principal do servidor de destino, em seguida, não poderá escolher a máquina virtual a partir do portal. Servidores de gestão não podem ser replicadas.
* **Já protegidos/ativação pós-falha através dos serviços do Azure Site Recovery**: Se a máquina virtual já está protegida ou a ativação pós-falha através do Site Recovery, a máquina virtual não está disponível para selecionar para a proteção no portal. Certifique-se de que a máquina virtual que está procurando no portal do já não está protegida por qualquer outro utilizador ou numa subscrição diferente.
* **vCenter não ligado**: Verifique se o vCenter está no estado ligado. Para verificar, aceda ao Cofre dos Recovery Services > infraestrutura do Site Recovery > servidores de configuração > clique no servidor de configuração respetivos > é aberto um painel de diretamente com os detalhes de servidores associados. Verifique se o vCenter está ligado. Se no estado "Não ligado", resolver o problema e, em seguida [atualizar o servidor de configuração](vmware-azure-manage-configuration-server.md#refresh-configuration-server) no portal. Depois disso, máquina virtual será listada no portal.
* **ESXi está desligada**: Se o anfitrião ESXi em que reside a máquina virtual está num Estado, desligado, em seguida, máquina virtual não será apresentada ou não podem ser selecionada no portal do Azure. Ligar o anfitrião ESXi, [atualizar o servidor de configuração](vmware-azure-manage-configuration-server.md#refresh-configuration-server) no portal. Depois disso, máquina virtual será listada no portal.
* **Reinício pendente**: Se existir um reinício pendente na máquina virtual, em seguida, não será possível selecionar a máquina no portal do Azure. Certifique-se para concluir as atividades de reinício pendente [atualizar o servidor de configuração](vmware-azure-manage-configuration-server.md#refresh-configuration-server). Depois disso, máquina virtual será listada no portal.
* **IP não foi encontrado**: Se a máquina virtual não tiver um endereço IP válido associado ao mesmo, em seguida, não será possível selecionar a máquina no portal do Azure. Certifique-se de atribuir um endereço IP válido para a máquina virtual, [atualizar o servidor de configuração](vmware-azure-manage-configuration-server.md#refresh-configuration-server). Depois disso, máquina virtual será listada no portal.

## <a name="protected-virtual-machines-are-greyed-out-in-the-portal"></a>Máquinas virtuais protegidas são cinzento out no portal

Máquinas virtuais que são replicadas na recuperação de sites não estão disponíveis no portal do Azure se existirem entradas duplicadas no sistema. Para saber como eliminar entradas obsoletas e resolver o problema, consulte [Azure Site Recovery VMware para o Azure: Como limpar entradas duplicadas ou obsoletas](https://social.technet.microsoft.com/wiki/contents/articles/32026.asr-vmware-to-azure-how-to-cleanup-duplicatestale-entries.aspx).

## <a name="next-steps"></a>Passos Seguintes

Se precisar de mais ajuda, publique sua pergunta no [fórum do Azure Site Recovery](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr). Temos uma Comunidade ativa e um dos nossos engenheiros pode a ajudá-lo.
