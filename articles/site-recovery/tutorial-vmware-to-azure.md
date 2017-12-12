---
title: "Configure a recuperação de desastre para o Azure para as VMs de VMware no local com o Azure Site Recovery | Microsoft Docs"
description: "Saiba como configurar a recuperação após desastre para o Azure para as VMs de VMware no local com o serviço do Azure Site Recovery."
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/11/2017
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 5810ff908d48fc4ff742d734e7c2457fdfe8cb03
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/11/2017
---
# <a name="set-up-disaster-recovery-to-azure-for-on-premises-vmware-vms"></a>Configure a recuperação de desastre para o Azure para as VMs de VMware no local

Este tutorial mostra como configurar a recuperação após desastre para o Azure para as VMs de VMware no local com o Windows. Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Especifique a origem de replicação e de destino.
> * Configure o ambiente de replicação de origem, incluindo os componentes da recuperação de Site no local e o ambiente de replicação de destino.
> * Criar uma política de replicação
> * Ativar a replicação para uma VM

Este é o terceiro tutorial uma série. Este tutorial parte do princípio de que já concluiu as tarefas nos tutoriais anteriores:

1. [Preparar o Azure](tutorial-prepare-azure.md)
2. [Preparar o VMware no local](tutorial-prepare-on-premises-vmware.md)

Antes de começar, é útil [rever a arquitetura](concepts-vmware-to-azure-architecture.md) para cenário de recuperação após desastre.


## <a name="select-a-replication-goal"></a>Selecione um objetivo de replicação

1. No **cofres dos serviços de recuperação**, clique no nome do cofre, **ContosoVMVault**.
2. No **introdução**, clique em recuperação de sites. Em seguida, clique em **preparar infraestrutura**.
3. No **objetivo de proteção** > **onde estão as máquinas localizadas**, selecione **no local**.
4. No * * onde pretende replicar as máquinas, selecione **para o Azure**.
5. No **são as máquinas virtualizadas**, selecione **Sim, com o VMware vSphere hipervisor**. Em seguida, clique em **OK**.

## <a name="set-up-the-source-environment"></a>Configurar o ambiente de origem

Para configurar o ambiente de origem, transfira o ficheiro de configuração do Unified Site Recovery. Execute a configuração para instalar os componentes da recuperação de Site no local, registar os servidores do VMware no cofre e detetar VMs no local.

### <a name="verify-on-premises-site-recovery-requirements"></a>Certifique-se requisitos de recuperação de sites no local

Precisa de uma VM de VMware único, elevada disponibilidade, no local para componentes de recuperação de sites no local do anfitrião. Componentes incluem o servidor de configuração, o servidor de processos e o servidor de destino principal.

- O servidor de configuração coordena as comunicações entre o local e o Azure, e gere a replicação de dados.
- O servidor de processos atua como um gateway de replicação. Recebe dados de replicação, otimiza-os com a colocação em cache, compressão e encriptação, e envia-os para o armazenamento do Azure. O servidor de processos também instala o serviço de mobilidade em VMs que pretende replicar, e efetua a descoberta automática de VMs de VMware no local.
- O servidor de destino mestre processa dados de replicação durante a reativação pós-falha a partir do Azure.

A VM deve cumprir os seguintes requisitos.

| **Requisito** | **Detalhes** |
|-----------------|-------------|
| Número de núcleos de CPU| 8 |
| RAM | 12 GB |
| Número de discos | 3 - disco do SO, disco de cache do servidor de processos, unidade de retenção (para reativação pós-falha) |
| Espaço livre no disco (cache do servidor de processos) | 600 GB |
| Espaço livre no disco (disco de retenção) | 600 GB |
| Versão do sistema operativo | Windows Server 2012 R2 |
| Região do sistema operativo | Inglês (en-us) |
| Versão do VMware vSphere PowerCLI | [PowerCLI 6.0](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1 "PowerCLI 6.0") |
| Funções do Windows Server | Não ative estas funções: serviços de domínio do Active Directory, serviços de informação de Internet, Hyper-V |
| Tipo NIC | VMXNET3 |
| Tipo de endereço IP | Estático |
| Portas | 443 (Canal de controlo e orquestração)<br/>9443 (Transporte de dados)|

Além disso: 
- Certifique-se de que o relógio do sistema na VM está sincronizado com um servidor de tempo. Tempo têm de ser sincronizado para em 15 minutos. Se for superior a configuração falha.
a configuração falha.
- Certifique-se de que o servidor de configuração VM pode aceder a estes URLs:

    [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]
    
- Certifique-se de que as regras de firewall baseadas no endereço IP permitem a comunicação com o Azure.
    - Permitir o [intervalos IP do datacenter do Azure](https://www.microsoft.com/download/confirmation.aspx?id=41653), a porta 443 (HTTPS) e porta 9443 (replicação de dados).
    - Permita intervalos de endereços IP para a região do Azure da sua subscrição e para EUA oeste (utilizado para gestão de identidade e controlo de acesso).


### <a name="download-the-site-recovery-unified-setup-file"></a>Transfira o ficheiro de configuração do Unified Site Recovery

1. No cofre > **preparar infraestrutura**, clique em **origem**.
1. No **preparar a origem**, clique em **+ o servidor de configuração**.
2. No **Adicionar servidor**, verifique se **servidor de configuração** aparece no **tipo de servidor**.
3. Transfira o ficheiro de instalação de configuração do Unified Site Recovery.
4. Transferir a chave de registo do cofre. Isto tem quando executar a configuração do Unified. A chave é válida durante cinco dias depois de gerá-la.

   ![Configurar a origem](./media/tutorial-vmware-to-azure/source-settings.png)

### <a name="set-up-the-configuration-server"></a>Configurar o servidor de configuração

1. Execute o ficheiro de instalação Configuração Unificada.
2. No **antes de começar**, selecione **instalar o servidor de configuração e o servidor de processos** , em seguida, clique em **seguinte**.

3. No **licença de Software de terceiros**, clique em **aceito** para transferir e instalar o MySQL, em seguida, clique em **seguinte**.

4. Em **Registo**, selecione a chave de registo que transferiu a partir do cofre.

5. Em **Definições da Internet**, especifique a forma como o Fornecedor em execução no servidor estabelece ligação ao Azure Site Recovery através da Internet.

   - Se pretende estabelecer ligação com o proxy que está atualmente configurado no computador, selecione **ligar ao Azure Site Recovery com um servidor proxy**.
   - Se pretender que o fornecedor se ligue diretamente, selecione **ligar diretamente ao Azure Site Recovery sem um servidor proxy**.
   - Se o proxy existente requer autenticação ou se pretender utilizar um proxy personalizado para a ligação de fornecedor, selecione **ligar com definições de proxy personalizado**e especifique o endereço, porta e credenciais.

   ![Firewall](./media/tutorial-vmware-to-azure/combined-wiz4.png)

6. Em **Verificação de Pré-requisitos**, a Configuração executa uma verificação para se certificar de que a instalação pode ser executada. Se for apresentado um aviso sobre a **Verificação de sincronização de hora global**, certifique-se de que a hora no relógio do sistema (definições de **Data e Hora**) é a mesma que o fuso horário.

   ![Pré-requisitos](./media/tutorial-vmware-to-azure/combined-wiz5.png)

7. Em **Configuração do MySQL**, crie as credenciais para iniciar sessão na instância de servidor do MySQL que está instalada.

8. No **ambiente detalhes**, selecione **Sim** para proteger as VMs de VMware. Programa de configuração verifica se o PowerCLI 6.0 está instalada.

9. Em **Localização de Instalação**, selecione onde pretende instalar os binários e armazenar a cache. A unidade que selecionar tem de ter, pelo menos, 5 GB de espaço disponível no disco, mas recomenda-se uma unidade de cache com, pelo menos, 600 GB de espaço livre.

10. Em **Seleção de Rede**, especifique o serviço de escuta (placa de rede e porta SSL) no qual o servidor de configuração envia e recebe dados de replicação. A porta 9443 é a porta predefinida utilizada para envio e receção de tráfego de replicação, mas pode modificar este número de porta para melhor corresponda às necessidades do seu ambiente. Podemos também abrir a porta 443, que é utilizada para orquestrar as operações de replicação. Não utilize a porta 443 para enviar ou receber tráfego de replicação.

11. Em **Resumo**, consulte as informações e clique em **Instalar**. A configuração instala o servidor de configuração e regista-o serviço do Azure Site Recovery.

    ![Resumo](./media/tutorial-vmware-to-azure/combined-wiz10.png)

    Quando a instalação estiver concluída, é gerada uma frase de acesso. Irá precisar dela, quando ativar a replicação, por isso, copie-a e mantenha-a numa localização segura. O servidor é apresentado no **definições** > **servidores** painel no cofre.

### <a name="configure-automatic-discovery"></a>Configurar a deteção automática

Detetar VMs, o servidor de configuração tem de ligar aos servidores do VMware no local. Para efeitos deste tutorial, adicione o servidor vCenter ou anfitriões vSphere, utilizando uma conta que tenha privilégios de administrador no servidor. Criou nesta conta na [tutorial anterior](tutorial-prepare-on-premises-vmware.md). 

Para adicionar a conta:

1. O servidor de configuração de VM, iniciar **CSPSConfigtool.exe**. Está disponível como um atalho no ambiente de trabalho e encontra-se na pasta *localização de instalação*\home\svsystems\bin.

2. Clique em **Gerir Contas** > **Adicionar Conta**.

   ![Adicionar conta](./media/tutorial-vmware-to-azure/credentials1.png)

3. Em **Detalhes da Conta**, adicione a conta que será utilizada para a deteção automática.

   ![Detalhes](./media/tutorial-vmware-to-azure/credentials2.png)

Para adicionar o servidor VMware:

1. Abra o [portal do Azure](https://portal.azure.com) e clique em **todos os recursos**.
2. Clique no cofre do serviço de recuperação com o nome **ContosoVMVault**.
3. Clique em **recuperação de sites** > **preparar infraestrutura** > **origem**
4. Selecione **+ vCenter**, para ligar ao vCenter server ou vSphere ESXi anfitrião.
5. No **adicionar vCenter**, especifique um nome amigável para o servidor. Em seguida, especifique o endereço IP ou FQDN.
6. Deixe a porta definida como 443, a menos que os servidores do VMware escutam os pedidos numa porta diferente.
7. Selecione a conta a utilizar para ligar ao servidor. Clique em **OK**.

Recuperação de sites liga-se aos servidores do VMware com as definições especificadas e Deteta as VMs.

> [!NOTE]
> Pode demorar 15 minutos ou mais para o nome da conta aparecer no portal. Para atualizar imediatamente, clique em **Servidores de Configuração** > ***nome do servidor*** > **Atualizar Servidor**.

## <a name="set-up-the-target-environment"></a>Configurar o ambiente de destino

Selecione e certifique-se de recursos de destino.

1. Clique em **Preparar a infraestrutura** > **Destino** e selecione a subscrição do Azure que pretende utilizar.
2. Especifique se o seu modelo de implementação de destino é baseado no Resource Manager ou clássico.
3. A Recuperação de Sites verifica que tem uma ou mais contas de armazenamento e redes do Azure compatíveis.

   ![Alvo](./media/tutorial-vmware-to-azure/storage-network.png)

## <a name="create-a-replication-policy"></a>Criar uma política de replicação

1. Abra o [portal do Azure](https://portal.azure.com) e clique em **todos os recursos**.
2. Clique no cofre do serviço de recuperação com o nome **ContosoVMVault**.
3. Para criar uma política de replicação, clique em **infraestrutura de recuperação de Site** > **políticas de replicação** > **+ política de replicação**.
4. No **criar política de replicação**, especifique um nome de política **VMwareRepPolicy**.
5. No **limiar RPO**, utilizar a predefinição de 60 minutos. Este valor define com que frequência são criados pontos de recuperação. É gerado um alerta se a replicação contínua excede este limite.
6. No **retenção do ponto de recuperação**, utilizar a predefinição de 24 horas para o período de retenção da janela de retenção para cada ponto de recuperação. Para este tutorial, iremos selecionar 72 horas. VMs replicadas podem ser recuperadas para qualquer ponto numa janela.
7. No **frequência de instantâneos consistentes com aplicação**, utilizar a predefinição de 60 minutos para a frequência de que os instantâneos consistentes com aplicações foram criados. Clique em **OK** para criar uma política.

   ![Política](./media/tutorial-vmware-to-azure/replication-policy.png)

A política é automaticamente associada ao servidor de configuração. Por predefinição, uma política correspondente é criada automaticamente para reativação pós-falha. Por exemplo, se a política de replicação é **rep política** a política de reativação pós-falha será **rep-política-reativação pós-falha**. Esta política não é utilizada depois de iniciar uma reativação pós-falha a partir do Azure.

## <a name="enable-replication"></a>Ativar a replicação

Recuperação de site instala o serviço de mobilidade quando a replicação está ativada para uma VM. Pode demorar 15 minutos ou já para as alterações que tenha efeito e aparecer no portal.

Ative a replicação da seguinte forma:

1. Clique em **replicar aplicação** > **origem**.
2. No **origem**, selecione o servidor de configuração.
3. No **máquina tipo**, selecione **máquinas virtuais**.
4. No **vCenter/vSphere hipervisor**, selecione o servidor vCenter que gere o anfitrião do Vcenter ou selecione o anfitrião.
5. Selecione o servidor de processos (servidor de configuração). Clique IThen **OK**.
6. No **destino**, selecione a subscrição e o grupo de recursos no qual pretende criar a ativação pós-falha das VMs. Escolha o modelo de implementação que pretende utilizar no Azure (clássica ou recurso management), para a ativação pós-falha das VMs com falhas.
7. Selecione a conta de armazenamento do Azure que pretende utilizar para replicar os dados.
8. Selecione a rede e a sub-rede do Azure às quais as VMs do Azure se vão ligar quando forem criadas após a ativação pós-falha.
9. Selecione **Configurar agora para as máquinas selecionadas** para aplicar a definição de rede a todas as máquinas selecionadas para proteção. Selecione **Configurar mais tarde** para selecionar a rede do Azure por máquina.
10. Em **Máquinas Virtuais** > **Selecionar máquinas virtuais**, clique e selecione cada máquina que pretende replicar. Só pode selecionar máquinas para as quais a replicação pode ser ativada. Em seguida, clique em **OK**.
11. No **propriedades** > **configurar propriedades**, selecione a conta que será utilizada pelo servidor de processos para instalar automaticamente o serviço de mobilidade na máquina.
12. No **as definições de replicação** > **configurar as definições de replicação**, certifique-se de que a política de replicação correto está selecionada.
13. Clique em **ativar a replicação**.

Pode controlar o progresso do **ativar proteção** da tarefa no **definições** > **tarefas** > **tarefas de recuperação de Site**. Depois da tarefa **Finalizar Proteção** ser executada, a máquina está preparada para ativação pós-falha.

Para monitorizar as VMs que adiciona, pode verificar a última vez detetada para VMs no **servidores de configuração**
> **Último contacto em**. Para adicionar VMs sem aguardar que a deteção agendada, realce o servidor de configuração (não clique nele) e clique em **atualizar**.

## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"]
> [Executar um teste de recuperação após desastre](site-recovery-test-failover-to-azure.md)
