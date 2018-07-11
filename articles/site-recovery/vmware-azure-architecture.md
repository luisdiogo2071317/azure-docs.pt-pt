---
title: VMware para a arquitetura da replicação do Azure no Azure Site Recovery | Documentos da Microsoft
description: Este artigo fornece uma visão geral dos componentes e da arquitetura utilizada ao replicar VMs de VMware no local para o Azure com o Azure Site Recovery
author: rayne-wiselman
ms.service: site-recovery
ms.date: 07/06/2018
ms.author: raynew
ms.openlocfilehash: 48adf61dc0f1796b820e1e14ca509d4618c6256b
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/09/2018
ms.locfileid: "37920572"
---
# <a name="vmware-to-azure-replication-architecture"></a>VMware para a arquitetura da replicação do Azure

Este artigo descreve a arquitetura e os processos utilizados ao replicar, efetuar a ativação pós-falha e recuperar máquinas virtuais de VMware (VMs) entre um site de VMware no local e o Azure, utilizando [do Azure Site Recovery](site-recovery-overview.md).


## <a name="architectural-components"></a>Componentes da arquitetura

A tabela seguinte e o gráfico fornecem uma visão geral dos componentes utilizados para a replicação de VMware para o Azure.

**Componente** | **Requisito** | **Detalhes**
--- | --- | ---
**Azure** | Uma subscrição do Azure, uma conta de armazenamento do Azure e uma rede do Azure. | Dados replicados de VMs no local são armazenados na conta de armazenamento. VMs do Azure são criadas com os dados replicados quando executar uma ativação pós-falha do local para o Azure. As VMs do Azure ligam-se à rede virtual do Azure quando são criadas.
**Máquina do servidor de configuração** | Uma única máquina no local. Recomendamos que executá-la como uma VM de VMware que podem ser implementadas a partir de um modelo do OVF transferido.<br/><br/> A máquina é executada todos os componentes do Site Recovery no local, que incluem o servidor de configuração, o servidor de processos e o servidor de destino mestre. | **Servidor de configuração**: coordena as comunicações entre no local e o Azure e gere a replicação de dados.<br/><br/> **Servidor de processos**: instalado por predefinição no servidor de configuração. Recebe dados de replicação. otimiza-os com colocação em cache, compressão e encriptação; e envia-os para o armazenamento do Azure. O servidor de processos também instala o serviço de mobilidade de recuperação de sites do Azure nas VMs que pretende replicar, e efetua a deteção automática de máquinas no local. À medida que cresça a implementação, pode adicionar servidores de processo adicionais, em separado para processar maiores volumes de tráfego de replicação.<br/><br/> **Servidor de destino mestre**: instalado por predefinição no servidor de configuração. Ele processa dados de replicação durante a reativação pós-falha do Azure. Para implementações maiores, pode adicionar um servidor de destino mestre adicionais, em separado para a reativação pós-falha.
**Servidores de VMware** | VMware VMs são alojadas em servidores do ESXi do vSphere no local. Recomendamos um servidor vCenter para gerir os anfitriões. | Durante a implementação da recuperação de Site, pode adicionar servidores VMware para o Cofre dos serviços de recuperação.
**Máquinas replicadas** | O serviço de mobilidade está instalado em cada VM de VMware, replicar. | Recomendamos que permite que a instalação automática do servidor de processos. Em alternativa, pode instalar o serviço manualmente ou utilizar um método de implementação automatizada, como o System Center Configuration Manager.

**Arquitetura de VMware para o Azure**

![Componentes](./media/vmware-azure-architecture/arch-enhanced.png)

## <a name="configuration-steps"></a>Passos de configuração

As etapas amplo para configuração de VMware para migração ou de recuperação após desastre do Azure são da seguinte forma:

1. **Configurar componentes do Azure**. Terá de uma conta do Azure com as permissões corretas, uma conta de armazenamento do Azure, uma rede virtual do Azure e um cofre dos serviços de recuperação. [Saiba mais](tutorial-prepare-azure.md).
2. **Configurar no local**. Estes incluem como configurar uma conta no servidor do VMware, para que o Site Recovery pode detetar automaticamente as VMs para replicar, configurar uma conta que pode ser utilizada para instalar o componente de serviço de mobilidade em VMs que pretende replicar e para verificar que servidores de VMware e as VMs estão em conformidade com os pré-requisitos. Opcionalmente, também pode preparar para ligar a estas VMs do Azure após a ativação pós-falha. Site Recovery replica os dados da VM para uma conta de armazenamento do Azure e cria VMs do Azure com os dados ao executar uma ativação pós-falha para o Azure. [Saiba mais](vmware-azure-tutorial-prepare-on-premises.md).
3. **Configurar a replicação**. Pode escolher onde pretende replicar para o. Configurar o ambiente de replicação de origem ao definir um único VMware no local VM (o servidor de configuração), que executa todos os locais de componentes do Site Recovery que terá de. Após a configuração registar-se a máquina do servidor de configuração no cofre dos serviços de recuperação. Em seguida, selecione as definições de destino. [Saiba mais](vmware-azure-tutorial.md).
4. **Criar uma política de replicação**. Criar uma política de replicação que especifica a forma como os replicação deve acontecer. 
    - **Limiar RPO**: esta definição de monitorização dos Estados que se a replicação não ocorrer no tempo especificado, um alerta (e, opcionalmente, uma mensagem de e-mail) são emitidos. Por exemplo, se definir o limiar RPO como 30 minutos e um problema impede que os replicação aconteça durante 30 minutos, é gerado um evento. Esta definição não afeta os replicação. A replicação é contínua e pontos de recuperação são criados os intervalos de poucos minutos
    - **Retenção**: retenção Especifica o tempo em que pontos de recuperação de ponto de recuperação deve ser mantido no Azure. Pode especificar um valor entre 0 e 24 horas para o armazenamento premium ou, até 72 horas para o armazenamento standard. Pode efetuar a ativação pós-falha para o ponto de recuperação mais recente ou para um ponto de armazenado se definir o valor superior a zero. Depois da janela de retenção, pontos de recuperação são removidos.
    - **Instantâneos consistentes com falhas**: por predefinição, o Site Recovery tira instantâneos consistentes com falhas e cria pontos de recuperação com os mesmos intervalos de poucos minutos. Um ponto de recuperação é consistente com se todos os componentes de dados inter-relacionados são escrita ordem consistente com a falha, como estavam no instante o ponto de recuperação foi criado. Para compreender melhor, imagine o estado dos dados no seu disco rígido do computador após uma falha de energia ou um evento semelhante. Um ponto de recuperação consistentes com falhas é geralmente suficiente se seu aplicativo foi projetado para recuperar a partir de uma falha sem quaisquer inconsistências de dados.
    - **Instantâneos consistentes com a aplicação**: se este valor não for zero, o serviço de mobilidade em execução na VM tenta gerar instantâneos consistentes com o sistema de ficheiros e de pontos de recuperação. O primeiro instantâneo ser tirado após a conclusão da replicação inicial. Em seguida, são tirados instantâneos com a frequência que especificar. Um ponto de recuperação é consistente com a aplicação se, além da ordem de escrita consistentes e em execução de aplicações concluir todas as operações e libera os buffers para o disco (quiescing de aplicativo). Pontos de recuperação consistente com a aplicação são recomendados para aplicativos de banco de dados, como SQL, Oracle e no Exchange. Se um instantâneo consistente de falhas é suficiente, este valor pode ser definido como 0.  
    - **Consistência de multi-VMS**: Opcionalmente, pode criar um grupo de replicação. Em seguida, quando ativa a replicação, pode recolher as VMs para esse grupo. As VMs numa replicação agrupar replicar e tem partilhado pontos de recuperação consistentes com falhas e consistente com a aplicação quando a ativação pós-falha. Deve utilizar esta opção com cuidado, uma vez que ele pode afetar o desempenho de carga de trabalho como instantâneos precisava ser reunidas em várias máquinas. Fazer isso apenas se as VMs são executadas a mesma carga de trabalho e têm de ser consistente e as VMs têm churns semelhante. Pode adicionar até 8 VMs a um grupo. 
5. **Ativar a replicação de VM**. Por fim, ativar a replicação para as VMs de VMware no local. Se criou uma conta para instalar o serviço de mobilidade e especificado que a recuperação de Site deve fazer uma instalação de push, o serviço de mobilidade será instalado em cada VM para o qual pode ativar a replicação. [Saiba mais](vmware-azure-tutorial.md#enable-replication). Se tiver criado um grupo de replicação para a consistência multi-VM, pode adicionar VMs a esse grupo.
6. **Ativação pós-falha de teste**. Depois que tudo está configurado, pode fazer uma ativação pós-falha de teste para verificar que as VMs efetuar a ativação pós-falha para o Azure conforme esperado. [Saiba mais](tutorial-dr-drill-azure.md).
7. **Ativação pós-falha**. Se estiver a migrar apenas as VMs para o Azure - executar uma ativação pós-falha completa para isso. Se estiver a configurar a recuperação após desastre, pode executar uma ativação pós-falha completa, precisa. Para a recuperação após desastre, após a ativação pós-falha para o Azure, pode efetuar a ativação para seu site no local como e quando estiver disponível. [Saiba mais](vmware-azure-tutorial-failover-failback.md).

## <a name="replication-process"></a>Processo de replicação

1. Quando ativa a replicação para uma VM, este começa a replicar em conformidade com a política de replicação. 
2. Tráfego replica para o armazenamento do Azure pontos finais públicos na Internet. Em alternativa, pode utilizar o ExpressRoute do Azure com [peering público](../expressroute/expressroute-circuit-peerings.md#azure-public-peering). Replicar o tráfego através de uma rede privada virtual (VPN) do site a site a partir de um site no local para o Azure não é suportada.
3. Uma cópia inicial dos dados VM é replicada para o armazenamento do Azure.
4. Depois de concluída a replicação inicial, começa a replicação das alterações delta para o Azure. As alterações registadas relativas a uma máquina são guardadas num ficheiro .hrl.
5. Comunicação acontece da seguinte forma:

    - As VMs comunicar com o servidor de configuração no local na porta HTTPS 443 entrada, para a gestão da replicação.
    - O servidor de configuração orquestra a replicação com o Azure através da porta HTTPS 443 de saída.
    - VMs enviam dados de replicação para o servidor de processos (em execução na máquina do servidor de configuração) na porta HTTPS 9443 de entrada. Esta porta pode ser modificada.
    - O servidor de processos recebe dados de replicação, otimiza e criptografa- e envia-os para o armazenamento do Azure através da porta 443 saída.


**VMware para o processo de replicação do Azure**

![Processo de replicação](./media/vmware-azure-architecture/v2a-architecture-henry.png)

## <a name="failover-and-failback-process"></a>Processo de ativação pós-falha e de reativação pós-falha

Após a replicação está configurada e executar um teste de recuperação após desastre (ativação pós-falha de teste) para verificar se tudo o que está a funcionar conforme esperado, pode executar a ativação pós-falha e reativação pós-falha conforme pretender.

1. Executar ativação para uma única máquina, ou criar uma recuperação planos para efetuar a ativação pós-falha de várias VMs ao mesmo tempo. A vantagem de um plano de recuperação em vez de ativação pós-falha da máquina única incluem:
    - Pode modelar dependências de aplicações, incluindo todas as VMs em toda a aplicação no plano de recuperação simples.
    - Pode adicionar scripts, os runbooks do Azure e colocar em pausa para ações manuais.
2. Depois de acionar a ativação pós-falha inicial, confirmá-lo a começar a aceder à carga de trabalho da VM do Azure.
3. Quando o seu site no local primário estiver novamente disponível, pode se preparar para reativação pós-falha. Para efetuar a reativação pós-falha, terá de configurar uma infraestrutura de reativação pós-falha, incluindo:

    * **Servidor de processo temporário no Azure**: para reativação pós-falha do Azure, configurar uma VM do Azure para atuar como um servidor de processos para processar a replicação do Azure. É possível eliminar esta VM após a conclusão da reativação pós-falha.
    * **Ligação VPN**: para efetuar a reativação pós-falha, terá de uma ligação de VPN (ou ExpressRoute) da rede do Azure para o site no local.
    * **Servidor de destino principal separado**: por predefinição, o servidor de destino mestre foi instalado com o servidor de configuração do VM de VMware no local processa a reativação pós-falha. Se precisar de efetuar a ativação pós-falha grandes volumes de tráfego, configure um servidor de destino principal independente no local para esta finalidade.
    * **Política de reativação pós-falha**: para replicar de novo para o site no local, precisa de uma política de reativação pós-falha. Esta política foi criada automaticamente quando criou a política de replicação no local para o Azure.
4. Depois dos componentes estão em vigor, a reativação pós-falha ocorre em três ações:

    - Fase 1: Voltar a proteger as VMs do Azure para que eles replicar a partir do Azure para as VMs do VMware no local.
    -  Fase 2: Executar uma ativação pós-falha para o site no local.
    - Fase 3: Depois de tem realizarão a reativação pós-falha cargas de trabalho, reativar a replicação para as VMs no local.
    
 
**Reativação pós-falha de VMware do Azure**

![Reativação pós-falha](./media/vmware-azure-architecture/enhanced-failback.png)


## <a name="next-steps"></a>Passos Seguintes

Siga [deste tutorial](vmware-azure-tutorial.md) para ativar a VMware para replicação do Azure.
