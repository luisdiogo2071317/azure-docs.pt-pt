---
title: Configurar a recuperação após desastre para VMs de Hyper-V entre sites no local com o Azure Site Recovery | Documentos da Microsoft
description: Saiba como configurar a recuperação após desastre das VMs de Hyper-V entre os sites no local com o Azure Site Recovery.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 07/06/2018
ms.author: raynew
ms.openlocfilehash: 4cc4da130d9253bf40e5d02806088a95b2195e7c
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/09/2018
ms.locfileid: "37915913"
---
# <a name="set-up-disaster-recovery-for-hyper-v-vms-to-a-secondary-on-premises-site"></a>Configurar a recuperação após desastre para VMs de Hyper-V para um site secundário no local

O serviço [Azure Site Recovery](site-recovery-overview.md) contribui para a estratégia de recuperação após desastre, através da gestão e orquestração de replicação, ativação pós-falha e reativação pós-falha de máquinas no local e máquinas virtuais (VMs) do Azure.

Este artigo mostra-lhe como configurar a recuperação após desastre para um site secundário, para VMs de Hyper-V no local geridas em clouds do System Center Virtual Machine Manager (VMM). Neste artigo, vai aprender a:

> [!div class="checklist"]
> * Preparar servidores VMM no local e anfitriões Hyper-V
> * Criar um cofre dos serviços de recuperação para o Site Recovery 
> * Configurar a origem e ambientes de replicação de destino. 
> * Configurar o mapeamento de rede 
> * Criar uma política de replicação
> * Ativar replicação para uma VM

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este cenário:

- Reveja os [arquitetura do cenário e componentes](hyper-v-vmm-architecture.md).
- Certifique-se de que servidores VMM e anfitriões Hyper-V estão em conformidade com [suportar requisitos](hyper-v-vmm-secondary-support-matrix.md).
- Verifique se as VMs que pretende replicar cumprem [replicados suporte de máquina](hyper-v-vmm-secondary-support-matrix.md#replicated-vm-support).
- Prepare servidores VMM para o mapeamento da rede.

### <a name="prepare-for-network-mapping"></a>Preparar o mapeamento da rede

[Mapeamento da rede](hyper-v-vmm-network-mapping.md) mapeia entre as redes no local VM do VMM em nuvens de origem e de destino. Mapeamento faz o seguinte:

- Liga-se as VMs para redes VM de destino apropriado após a ativação pós-falha. 
- Idealmente, coloca as VMs da réplica nos servidores de anfitrião de Hyper-V de destino. 
- Se não configurar o mapeamento da rede, as VMs de réplica não será ligada a uma rede VM após a ativação pós-falha.

Prepare o VMM da seguinte forma:

1. Certifique-se de que tem [redes lógicas do VMM](https://docs.microsoft.com/system-center/vmm/network-logical) nos servidores do VMM de origem e de destino.
    - A rede lógica no servidor de origem deve ser associada com a cloud de origem em que estão localizados os anfitriões de Hyper-V.
    - A rede lógica no servidor de destino deve ser associada com a nuvem de destino.
1. Certifique-se de que tem [redes VM](https://docs.microsoft.com/system-center/vmm/network-virtual) nos servidores do VMM de origem e de destino. Redes VM devem ser ligadas à rede lógica em cada local.
2. Ligar as VMs nos anfitriões de Hyper-V de origem à rede VM de origem. 


## <a name="create-a-recovery-services-vault"></a>Criar um cofre dos Serviços de Recuperação 

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]


## <a name="choose-a-protection-goal"></a>Escolha um objetivo de proteção

Selecione o que pretende replicar e para onde pretende que seja replicado.

1. Clique em **Site Recovery** > **passo 1: preparar a infraestrutura** > **objetivo de proteção**.
2. Selecione **para o site de recuperação**e selecione **Sim, com o Hyper-V**.
3. Selecione **Sim** para indicar que está a utilizar o VMM para gerir os anfitriões de Hyper-V.
4. Selecione **Sim** se tiver um servidor VMM secundário. Se estiver a implementar a replicação entre nuvens num único servidor VMM, clique em **não**. Em seguida, clique em **OK**.


## <a name="set-up-the-source-environment"></a>Configurar o ambiente de origem

Instalar o fornecedor do Azure Site Recovery nos servidores do VMM e detetar e registar os servidores no cofre.

1. Clique em **Preparar a Infraestrutura** > **Origem**.
2. Em **Preparar a origem**, clique em **+ VMM** para adicionar um servidor VMM.
3. Na **Adicionar servidor**, verifique se **servidor do System Center VMM** aparece no **tipo de servidor**.
4. Transfira o ficheiro de instalação do Fornecedor do Azure Site Recovery.
5. Transfira a chave de registo. Precisará disto quando instala o fornecedor. A chave é válida durante cinco dias depois de gerá-la.

    ![Configurar a origem](./media/hyper-v-vmm-disaster-recovery/source-settings.png)

6. Instale o fornecedor em cada servidor VMM. Não precisa de instalar nada explicitamente em anfitriões Hyper-V.

### <a name="install-the-azure-site-recovery-provider"></a>Instalar o fornecedor do Azure Site Recovery

1. Execute o fornecedor de ficheiro de configuração em cada servidor VMM. Se o VMM for implementado num cluster, instale pela primeira vez da seguinte forma:
    -  Instalar o fornecedor num nó ativo e conclua a instalação para registar o servidor VMM no cofre.
    - Em seguida, instale o fornecedor nos outros nós. Nós de cluster devem executar a mesma versão do fornecedor.
2. A configuração executa algumas verificações de pré-requisitos e pede permissão para parar o serviço do VMM. O serviço VMM será reiniciado automaticamente após a conclusão do programa de configuração. Se instalar num VMM cluster, lhe for pedido para parar a função de Cluster.
3. Na **Microsoft Update**, pode optar por especificar que as atualizações do fornecedor são instaladas de acordo com a política do Microsoft Update.
4. Na **instalação**, aceite ou modifique a localização de instalação predefinida e clique em **instalar**.
5. Após a instalação estiver concluída, clique em **registar** para registar o servidor no cofre.

    ![Localização de instalação](./media/hyper-v-vmm-disaster-recovery/provider-register.png)
6. Em **Nome do cofre**, verifique o nome do cofre no qual o servidor será registado. Clique em **Seguinte**.
7. Na **ligação Proxy**, especifique a forma como o fornecedor em execução no servidor VMM estabelece ligação para o Azure.
   - Pode especificar que o fornecedor deve se conectar diretamente à internet ou através de um proxy. Especifique as definições de proxy, conforme necessário.
   - Se utilizar um proxy, uma conta RunAs do VMM (DRAProxyAccount) é criada automaticamente, com as credenciais de proxy especificado. Configure o servidor proxy para que esta conta possa autenticar-se com êxito. As definições da conta RunAs podem ser modificadas na consola do VMM > **configurações** > **segurança** > **contas Run as**.
   - Reinicie o serviço do VMM para atualizar as alterações.
8. Na **chave de registo**, selecione a chave que transferiu e copiados para o servidor do VMM.
9. A definição de encriptação não é relevante neste cenário. 
10. Em **Nome do servidor**, especifique um nome amigável para identificar o servidor VMM no cofre. Num cluster, especifique o nome de função de cluster do VMM.
11. Na **sincronizar metadados da nuvem**, selecione se pretende sincronizar os metadados de todas as nuvens no servidor do VMM. Esta ação só deverá ocorrer uma vez em cada servidor. Se não quiser sincronizar todas as nuvens, deixe esta definição desmarcada. Pode sincronizar cada nuvem individualmente, nas propriedades da nuvem na consola do VMM.
12. Clique em **Seguinte** para concluir o processo. Após o registo, o Site Recovery obtém os metadados do servidor do VMM. O servidor é apresentado em **servidores** > **servidores VMM** no cofre.
13. Depois do servidor é apresentado no cofre, no **origem** > **preparar origem** selecione o servidor VMM e selecione a nuvem em que o anfitrião de Hyper-V está localizado. Em seguida, clique em **OK**.


## <a name="set-up-the-target-environment"></a>Configurar o ambiente de destino

Selecione o servidor de destino do VMM e a cloud:

1. Clique em **preparar infraestrutura** > **destino**e selecione o servidor do VMM de destino.
2. Nuvens do VMM são sincronizadas com a recuperação de Site são apresentados. Selecione a nuvem de destino.

   ![Destino](./media/hyper-v-vmm-disaster-recovery/target-vmm.png)


## <a name="set-up-a-replication-policy"></a>Configurar uma política de replicação

Antes de começar, certifique-se de que todos os anfitriões através da política de ter o mesmo sistema operativo. Se os anfitriões estão a executar versões diferentes do Windows Server, tem várias políticas de replicação.

1. Para criar uma nova política de replicação, clique em **Preparar a infraestrutura** > **Definições de Replicação** > **+Criar e associar**.
2. Em **Criar e associar política**, especifique um nome de política. O tipo de origem e de destino deve ser **Hyper-V**.
3. Na **versão de anfitrião do Hyper-V**, selecione o sistema operativo está em execução no anfitrião.
4. Na **tipo de autenticação** e **porta de autenticação**, especifique a forma como o tráfego é autenticado entre os servidores de anfitrião de Hyper-V principal e de recuperação.
    - Selecione **certificado** a menos que tenha um ambiente de Kerberos em funcionamento. O Azure Site Recovery irá configurar automaticamente certificados para autenticação HTTPS. Não precisa de fazer qualquer coisa manualmente.
    - Por predefinição, a porta 8083 e 8084 (para certificados) será aberta na Firewall do Windows nos servidores de anfitrião de Hyper-V.
    - Se selecionar **Kerberos**, será utilizado um tíquete Kerberos para autenticação mútua, os servidores de anfitrião. Kerberos só é relevante para os servidores de anfitrião de Hyper-V em execução no Windows Server 2012 R2 ou posterior.
1. Em **Frequência de cópia**, especifique a frequência com que pretende replicar dados delta após a replicação inicial (a cada 30 segundos, 5 ou 15 minutos).
2. Na **retenção do ponto de recuperação**, especifique \how há muito tempo (em horas) da janela de retenção será de cada ponto de recuperação. Máquinas replicadas podem ser recuperadas para qualquer ponto nessa janela.
3. Na **frequência de instantâneos consistentes com a aplicação**, especifique a frequência (1-12 horas) de pontos de recuperação que contêm instantâneos consistentes com a aplicação são criados. Hyper-V utiliza dois tipos de instantâneos:
    - **Instantâneo padrão**: Fornece um instantâneo incremental de toda a máquina virtual.
    - **Instantâneos consistentes com a aplicação**: tira um instantâneo de ponto no tempo dos dados de aplicação no interior da VM. Serviço de cópia de sombra de volumes (VSS) garante que as aplicações estão num estado consistente quando o instantâneo. Ativar instantâneos consistentes com aplicações, afeta o desempenho de aplicações em VMs de origem. Defina um valor que é inferior ao número de pontos de recuperação adicionais que configurar.
4. Na **compressão da transferência de dados**, especifique se os dados de replicação transferidos devem ser comprimidos.
5. Selecione **Eliminar réplica VM**para especificar que a máquina virtual de réplica deve ser eliminada se desativar a proteção da VM de origem. Se ativar esta definição, quando desativar a proteção para a VM é removido da consola de recuperação de Site de origem, definições de recuperação de Site para o VMM serão removidas da consola do VMM e a réplica é eliminada.
6. Na **inicial do método de replicação**, se estiver a replicar através da rede, especifique se pretende iniciar a replicação inicial ou agendá-la. Para poupar largura de banda de rede, poderá querer agendá-la fora do horário de trabalho. Em seguida, clique em **OK**.

     ![Política de replicação](./media/hyper-v-vmm-disaster-recovery/replication-policy.png)
     
7. A nova política é associada automaticamente a nuvem do VMM. Na **política de replicação**, clique em **OK**. 


## <a name="enable-replication"></a>Ativar a replicação

1. Clique em **Replicar aplicação** > **Origem**. 
2. Na **origem**, selecione o servidor do VMM e a nuvem em que estão localizados os anfitriões de Hyper-V que pretende replicar. Em seguida, clique em **OK**.
3. Na **destino**, certifique-se de que o servidor VMM secundário e a cloud.
4. Na **máquinas virtuais**, selecione as VMs que pretende proteger na lista.


Pode controlar o progresso da ação **Ativar Proteção** em **Tarefas** > **Tarefas do Site Recovery**. Depois do **finalizar proteção** tarefa estiver concluída, a replicação inicial está concluída e a VM está preparada para ativação pós-falha.

## <a name="next-steps"></a>Passos Seguintes

[Executar um teste de recuperação após desastre](hyper-v-vmm-test-failover.md)
