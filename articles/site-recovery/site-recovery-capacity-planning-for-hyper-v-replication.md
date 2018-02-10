---
title: Capacidade de Hyper-V planear a ferramenta do Azure Site Recovery | Microsoft Docs
description: Este artigo descreve como executar a ferramenta de planeamento de capacidade Hyper-V para o Azure Site Recovery
services: site-recovery
documentationcenter: na
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: 2bc3832f-4d6e-458d-bf0c-f00567200ca0
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/28/2017
ms.author: nisoneji
ms.openlocfilehash: 80c3dcb65bd74bcfa3acc5f12dd5c45cd1c06455
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/09/2018
---
# <a name="hyper-v-capacity-planning-tool-for-site-recovery"></a>Planear a ferramenta de recuperação de sites de capacidade de Hyper-V

Uma nova versão avançada do [Planeador de implementação do Azure Site Recovery para o Hyper-V para implementação do Azure](site-recovery-hyper-v-deployment-planner.md) está agora disponível. Substitui a ferramenta antiga. Utilize a ferramenta de novo para o planeamento da implementação. A ferramenta fornece as seguintes diretrizes:

* Avaliação de elegibilidade da VM, com base no número de discos, tamanho do disco, IOPS, volume de alterações e alguns características VM
* Tem de largura de banda de rede versus avaliação RPO
* Requisitos de infraestrutura do Azure
* Requisitos de infraestrutura no local
* Replicação inicial orientações de criação de batches
* Estimar o custo de recuperação de desastre total para o Azure

Como parte da implementação do Azure Site Recovery, tem de descobrir os seus requisitos de largura de banda e de replicação. A capacidade de Hyper-V planear a ferramenta de recuperação de sites ajuda a determinar os requisitos seguintes para replicação de VM de Hyper-V.

Este artigo descreve como executar a ferramenta de planeamento de capacidade Hyper-V. Utilize esta ferramenta, juntamente com as informações no [planeamento de capacidade de recuperação de Site](site-recovery-capacity-planner.md).

## <a name="before-you-start"></a>Antes de começar
A ferramenta é executada num nó servidor ou cluster Hyper-V no seu site primário. Para executar a ferramenta, os servidores de anfitrião do Hyper-V tem de:

* Sistema operativo: Windows Server 2012 ou 2012 R2
* Memória: 20 MB (no mínimo)
* Da CPU: a sobrecarga de 5 porcento (no mínimo)
* Espaço em disco: 5 MB (no mínimo)

Antes de executar a ferramenta, terá de preparar o site primário. Se replicar entre dois sites no local e pretende verificar a largura de banda, terá de preparar o servidor de réplica.

## <a name="step-1-prepare-the-primary-site"></a>Passo 1: Preparar o site primário

1. No site primário, se uma lista de todas as VMs de Hyper-V que pretende replicar. Lista de anfitriões Hyper-V ou clusters onde está localizados. A ferramenta pode ser executado para vários anfitriões autónomos ou para um único cluster, mas não ambos em conjunto. Também necessita de ser executado em separado para cada sistema operativo. Recolha as seguintes informações sobre servidores de Hyper-V:

   * Servidores autónomos do Windows Server 2012
   * Clusters do Windows Server 2012
   * Servidores autónomos do Windows Server 2012 R2
   * Clusters do Windows Server 2012 R2

2. Ative o acesso remoto ao Windows Management Instrumentation em todos os anfitriões Hyper-V e clusters. Execute este comando em cada servidor ou certifique-se de regras de firewall do cluster e as permissões de utilizador estão definidas:

        netsh firewall set service RemoteAdmin enable
3. Ative monitorização do desempenho nos servidores e clusters da seguinte forma:

   a. Abrir a Firewall do Windows com o **segurança avançada** snap-in. 
   
   b. Selecione a regra de entrada **COM + acesso à rede (DCOM-IN)**. Selecione todas as regras de **a gestão remota do registo de eventos** grupo.

## <a name="step-2-prepare-a-replica-server-on-premises-to-on-premises-replication"></a>Passo 2: Preparar um servidor de réplica (no local para replicação no local)
Se replicar para o Azure, não precisa de efetuar este passo.

Recomendamos que configure um único anfitrião Hyper-V como um servidor de recuperação para que uma VM fictício pode ser replicada para a mesma para verificar a largura de banda. Pode ignorar este passo, mas não é possível medir a largura de banda, a menos que pode fazê-lo.

1. Se pretender utilizar um nó de cluster como a réplica, configure o Mediador de réplicas do Hyper-V:

   a. No **Gestor de servidor**, abra **Gestor de clusters de ativação pós-falha**.

   b. Ligar ao cluster e, realce o nome do cluster. Selecione **ações** > **configurar função** para abrir o Assistente de elevada disponibilidade.

   c. No **selecionar função**, selecione **Mediador de réplicas do Hyper-V**. No assistente, introduza um nome para **nome NetBIOS**. Introduza um endereço para **endereço IP** a ser utilizado como ponto de ligação ao cluster (chamado um ponto de acesso de cliente). O **Mediador de réplicas do Hyper-V** estiver configurado, que resulta num nome de ponto de acesso de cliente que deve ter em consideração.

   d. Certifique-se de que a função de Mediador de réplicas do Hyper-V fica online com sucesso e pode efetuar a ativação pós-falha entre todos os nós do cluster. A função com o botão direito e selecione **mover** > **selecionar nó**. Selecione um nó e, em seguida, selecione **OK**.

   e. Se utilizar a autenticação baseada em certificados, certifique-se de que cada nó do cluster e o cliente do ponto de acesso têm o certificado instalado.

2. Para ativar um servidor de réplica, siga estes passos:

   a. Para um cluster, abra **Gestor de clusters de falha** e ligar ao cluster. Selecione **funções**e, em seguida, selecione uma função. Selecione **as definições de replicação** > **ativar este cluster como um servidor de réplica**. Se utilizar um cluster como a réplica, terá de ter a função de Mediador de réplicas do Hyper-V existe no cluster no site primário.

   b. Para um servidor autónomo, abra **Gestor de Hyper-V**. No **ações** painel, selecione **definições de Hyper-V** para o servidor que pretende ativar. No **configuração de replicação**, selecione **ativar este computador como um servidor de réplica**.

3. Para configurar a autenticação, siga estes passos:

   a. Em **autenticação e portas**, selecione a forma como autenticar o servidor primário e a autenticação portas. Se utilizar um certificado, selecione **selecionar certificado** para selecionar uma. Utilize o Kerberos se os anfitriões de Hyper-V principal e de recuperação estão no mesmo domínio ou em domínios fidedignos. Utilize certificados para domínios diferentes ou para uma implementação de grupo de trabalho.

   b. Em **autorização e armazenamento**, permitir **qualquer** autenticado (principal) servidor para enviar dados de replicação para este servidor de réplica.

   ![Configuração de replicação](./media/site-recovery-capacity-planning-for-hyper-v-replication/image1.png)

   c. Para verificar se o serviço de escuta está em execução para o protocolo ou a porta que especificou, execute **netsh http show servicestate**. 

4. Configure firewalls. Durante a instalação do Hyper-V, são criadas as regras de firewall para permitir tráfego no predefinido portas (HTTPS no 443) e de Kerberos em 80. Ative estas regras da seguinte forma:

    - Autenticação de certificado no cluster (443):``Get-ClusterNode | ForEach-Object {Invoke-command -computername \$\_.name -scriptblock {Enable-Netfirewallrule -displayname "Hyper-V Replica HTTPS Listener (TCP-In)"}}``
    - Autenticação Kerberos no cluster (80):``Get-ClusterNode | ForEach-Object {Invoke-command -computername \$\_.name -scriptblock {Enable-Netfirewallrule -displayname "Hyper-V Replica HTTP Listener (TCP-In)"}}``
    - Autenticação de certificado no servidor autónomo:``Enable-Netfirewallrule -displayname "Hyper-V Replica HTTPS Listener (TCP-In)"``
    - Autenticação Kerberos num servidor autónomo:``Enable-Netfirewallrule -displayname "Hyper-V Replica HTTP Listener (TCP-In)"``

## <a name="step-3-run-the-capacity-planning-tool"></a>Passo 3: Executar a ferramenta de planeamento de capacidade
Depois de preparar o seu site primário e configurar um servidor de recuperação, pode executar a ferramenta.

1. [Transferir](https://www.microsoft.com/download/details.aspx?id=39057) a ferramenta do Microsoft Download Center.

2. Execute a ferramenta a partir de um dos servidores primários ou um de nós do cluster primário. O ficheiro .exe com o botão direito e, em seguida, escolha **executar como administrador**.

3. No **antes de começar**, especifique durante quanto pretende recolher dados. Recomendamos que execute a ferramenta durante o horário de produção para assegurar que os dados são representante. Se pretende validar apenas a conectividade de rede, pode recolher para apenas um minuto.

    ![Antes de começar](./media/site-recovery-capacity-planning-for-hyper-v-replication/image2.png)

4. No **detalhes de Site primário**, especifique o nome do servidor ou o FQDN para um anfitrião autónomo. Para um cluster, especifique o FQDN do ponto de acesso de cliente, o nome de cluster ou qualquer nó no cluster. Selecione **Seguinte**. A ferramenta Deteta automaticamente o nome do servidor que é executada. A ferramenta escolherá VMs que podem ser monitorizadas para os servidores especificados.

    ![Detalhes de Site primário](./media/site-recovery-capacity-planning-for-hyper-v-replication/image3.png)

5. No **detalhes de Site de réplica**, se replicar para o Azure ou se replicar para um datacenter secundário e ainda não configurar um servidor de réplica, selecione **ignorar testes que envolve o site réplica**. Se replicar para um datacenter secundário e configurar um tipo de réplica, introduza o FQDN do servidor autónomo ou o ponto de acesso de cliente para o cluster no **Server name (ou) extremidade de Mediador de réplicas do Hyper-V**.

    ![Detalhes de Site de réplica](./media/site-recovery-capacity-planning-for-hyper-v-replication/image4.png)

6. No **detalhes de réplica expandido**, selecione **ignorar os testes que envolve o site de réplica expandida**. Estes testes não são suportadas pela recuperação de sites.

7. No **escolha VMs replicar**, a ferramenta estabelece ligação ao servidor ou cluster. Apresenta as VMs e discos em execução no servidor primário, com base nas definições que especificou no **detalhes de Site primário** página. As VMs que já estão a ser ativada para replicação ou que não estão em execução não serão apresentadas. Selecione as VMs para o qual pretende recolher a métrica. Selecionar os VHDs automaticamente, recolhe dados para as VMs, demasiado.

8. Se configurou o servidor de réplica ou num cluster, no **informações de rede**, especificar a largura de banda WAN aproximada a ser utilizada entre os sites primário e réplica. Se tiver configurado a autenticação de certificados, selecione os certificados.

    ![Informações de rede](./media/site-recovery-capacity-planning-for-hyper-v-replication/image5.png)

9. No **resumo**, verifique as definições. Selecione **seguinte** para começar a recolher métricas. Estado e o progresso da ferramenta são apresentados no **calcular a capacidade** página. Quando a ferramenta de conclusão da execução, selecione **Ver relatório** para ver o resultado. Por predefinição, os registos e relatórios são armazenados em **%systemdrive%\Users\Public\Documents\Capacity Planner**.

   ![Calcular a capacidade](./media/site-recovery-capacity-planning-for-hyper-v-replication/image6.png)

## <a name="step-4-interpret-the-results"></a>Passo 4: Interpretar os resultados

Seguem-se as métricas importantes. Pode ignorar as métricas que não estão listadas aqui. Não está relevantes para a recuperação de Site.

### <a name="on-premises-to-on-premises-replication"></a>No local para replicação no local

* Impacto da replicação de computação e memória do anfitrião principal
* Impacto da replicação no anfitrião primário e o anfitrião de recuperação espaço em disco de armazenamento e IOPS
* A largura de banda total necessária para replicação de diferenças (Mbps)
* Largura de banda de rede observado entre o anfitrião primário e o anfitrião de recuperação (Mbps)
* Sugestão para o número ideal de Active Directory transferências paralelas entre dois anfitriões ou clusters

### <a name="on-premises-to-azure-replication"></a>No local para a replicação do Azure

* Impacto da replicação de computação e memória do anfitrião principal
* Impacto da replicação no espaço de disco de armazenamento e IOPS o anfitrião principal
* A largura de banda total necessária para replicação de diferenças (Mbps)

## <a name="more-resources"></a>Mais recursos

* Para obter mais informações sobre a ferramenta, ler o documento que acompanha a transferência da ferramenta.
* Assista a obter instruções sobre a ferramenta no de Keith Mayer [blogue do TechNet](http://blogs.technet.com/b/keithmayer/archive/2014/02/27/guided-hands-on-lab-capacity-planner-for-windows-server-2012-hyper-v-replica.aspx).
* [Obtenha os resultados](site-recovery-performance-and-scaling-testing-on-premises-to-on-premises.md) de testes de desempenho para no local para a replicação de Hyper-V no local.

## <a name="next-steps"></a>Passos Seguintes

Depois de concluir o planeamento de capacidade, pode implementar a recuperação de Site:
* [Replicar VMs de Hyper-V em nuvens VMM para o Azure](site-recovery-vmm-to-azure.md)
* [Replicar VMs de Hyper-V (sem VMM) para o Azure](site-recovery-hyper-v-site-to-azure.md)
* [Replicar VMs Hyper-V entre sites do VMM](site-recovery-vmm-to-vmm.md)
