---
title: VMware para a arquitetura da replicação do Azure no Azure Site Recovery | Microsoft Docs
description: Este artigo fornece uma descrição geral da arquitetura de utilizada quando replicar VMs de VMware no local para o Azure com o Azure Site Recovery e componentes
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: raynew
ms.openlocfilehash: 61c283c178936c98a9a18509c1b46035e48f8f24
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36285275"
---
# <a name="vmware-to-azure-replication-architecture"></a>VMware para a arquitetura da replicação do Azure

Este artigo descreve a arquitetura e processos utilizados quando replicar, efetuar a ativação pós-falha e recuperar máquinas virtuais VMware (VMs) entre um site de VMware no local e o Azure utilizando [do Azure Site Recovery](site-recovery-overview.md).


## <a name="architectural-components"></a>Componentes da arquitetura

A seguinte tabela e o gráfico fornecem uma vista detalhada dos componentes utilizados para replicação de VMware no Azure.

**Componente** | **Requisito** | **Detalhes**
--- | --- | ---
**Azure** | Uma subscrição do Azure, uma conta de armazenamento do Azure e uma rede do Azure. | Os dados replicados de VMs no local são armazenados na conta de armazenamento. As VMs do Azure são criadas com os dados replicados quando executar uma ativação pós-falha no local para o Azure. As VMs do Azure ligam-se à rede virtual do Azure quando são criadas.
**Máquina do servidor de configuração** | Um único computador local. Recomendamos que execute-la como uma VM de VMware que podem ser implementadas a partir de um modelo transferido do OVF.<br/><br/> A máquina executa todos os componentes da recuperação de Site no local, que incluem o servidor de configuração, o servidor de processos e o servidor de destino principal. | **Servidor de configuração**: coordena as comunicações entre no local e o Azure e gere a replicação de dados.<br/><br/> **Servidor de processos**: instalado por predefinição no servidor de configuração. Recebe dados de replicação otimiza-as com colocação em cache, compressão e encriptação; e envia-a para o Storage do Azure. O servidor de processos também instala o serviço de mobilidade do Azure Site Recovery em VMs que pretende replicar, e efetua a descoberta automática de máquinas no local. À medida que cresça a implementação, pode adicionar servidores de processos adicionais, em separado para processar maiores volumes de tráfego de replicação.<br/><br/> **Servidor de destino mestre**: instalado por predefinição no servidor de configuração. -Processa dados de replicação durante a reativação pós-falha a partir do Azure. Para grandes implementações, pode adicionar um servidor de destino mestre adicionais, em separado para reativação pós-falha.
**Servidores de VMware** | VMs de VMware alojadas em servidores de ESXi do vSphere no local. Recomendamos que um servidor vCenter para gerir os anfitriões. | Durante a implementação da recuperação de sites, adicione servidores VMware para o Cofre dos serviços de recuperação.
**Máquinas replicadas** | Serviço de mobilidade está instalado em cada VM de VMware, replicar. | Recomendamos que permitem a instalação automática do servidor de processos. Em alternativa, pode instalar manualmente o serviço ou utilize um método de implementação automática, tais como o System Center Configuration Manager.

**Arquitetura de VMware para o Azure**

![Componentes](./media/vmware-azure-architecture/arch-enhanced.png)

## <a name="configuration-steps"></a>Passos de configuração

Os passos abrangentes para configurar o VMware para recuperação após desastre do Azure ou migração são os seguintes:

1. **Configurar componentes do Azure**. Precisa de uma conta do Azure com as permissões corretas, uma conta de armazenamento do Azure, uma rede virtual do Azure e um cofre dos serviços de recuperação. [Saiba mais](tutorial-prepare-azure.md).
2. **Configurar no local**. Estes incluem como configurar uma conta no servidor do VMware para que a recuperação de sites pode detetar automaticamente as VMs para replicar, configurar uma conta que pode ser utilizada para instalar o componente de serviço de mobilidade em VMs que pretende replicar e a verificar que servidores do VMware e as VMs estão em conformidade com os pré-requisitos. Também, opcionalmente, pode preparar para ligar a estes VMs do Azure após a ativação pós-falha. Recuperação de site replica os dados VM para uma conta de armazenamento do Azure e cria as VMs do Azure com os dados ao executar uma ativação pós-falha para o Azure. [Saiba mais](vmware-azure-tutorial-prepare-on-premises.md).
3. **Configurar a replicação**. Selecione onde pretende replicar para. Configurar o ambiente de replicação de origem, configurar uma VM (o servidor de configuração) que executa todos os locais componentes da recuperação de Site que precisa de VMware único no local. Após a configuração, registar a máquina do servidor de configuração no cofre dos serviços de recuperação. Em seguida, selecione as definições de destino. [Saiba mais](vmware-azure-tutorial.md).
4. **Criar uma política de replicação**. Criar uma política de replicação que especifica a forma como a replicação deve ocorrer. 
    - **Limiar RPO**: esta definição de monitorização Estados que se a replicação não ocorrer no tempo especificado, um alerta (e opcionalmente um e-mail) são emitidos. Por exemplo, se definir o limiar do RPO para 30 minutos e impede a um problema de replicação aconteça durante 30 minutos, é gerado um evento. Esta definição não afeta a replicação. A replicação está contínua e pontos de recuperação são criados em poucos minutos
    - **Retenção**: ponto de recuperação retenção Especifica o período de tempo pontos de recuperação deve ser mantido no Azure. Pode especificar um valor entre 0 e 24 horas para o premium storage, ou até 72 horas para armazenamento standard. Pode efetuar a ativação pós-falha para o ponto de recuperação mais recente ou para um ponto de armazenado se definir o valor superior a zero. Depois da janela de retenção, pontos de recuperação são removidos.
    - **Instantâneos consistentes com falhas**: por predefinição, a recuperação de Site demora instantâneos consistentes com falhas e cria pontos de recuperação com os mesmos, cada alguns minutos. Um ponto de recuperação com se todos os componentes dados interrelated são consistente escrita ordem falhas, tal como estavam na instantâneas o ponto de recuperação foi criado. Para compreender melhor, imagine o estado dos dados no disco rígido PC após uma falha de energia ou evento semelhante. Um ponto de recuperação consistentes com falhas é normalmente suficiente se a aplicação foi concebida para recuperar a partir de uma falha sem quaisquer inconsistências de dados.
    - **Instantâneos consistentes com aplicação**: se este valor não for zero, o serviço de mobilidade em execução na VM tenta gerar instantâneos consistentes com o sistema de ficheiros e de pontos de recuperação. O primeiro instantâneo é colocado depois de concluída a replicação inicial. Em seguida, são tirados instantâneos com a frequência que especificar. Um ponto de recuperação se consistentes com aplicações se, além de ser escrita ordem consistentes e em execução, aplicações concluir todas as respetivas operações e descarregar as memórias intermédias para disco (quiescing de aplicação). Pontos de recuperação consistentes da aplicação são recomendados para aplicações de base de dados, tais como o SQL Server, Oracle e o Exchange. Se um instantâneo consistente de falhas é suficiente, este valor pode ser definido como 0.  
    - **A consistência multi VM**: Opcionalmente, pode criar um grupo de replicação. Em seguida, ao ativar a replicação, pode recolher as VMs para esse grupo. As VMs de uma replicação agrupar replicar e tem partilhado pontos de recuperação consistentes com falhas e consistentes da aplicação quando a ativação pós-falha. Deve utilizar esta opção atentamente, uma vez que pode afetar o desempenho da carga de trabalho como instantâneos necessárias para ser reunidas em várias máquinas. Apenas fazê-lo se VMs executam a mesma carga de trabalho e precisam de ser consistente e as VMs têm churns semelhantes. Pode adicionar até 8 VMs a um grupo. 
5. **Ativar a replicação de VM**. Por fim, ativar a replicação para as VMs de VMware no local. Se criou uma conta para instalar o serviço de mobilidade e especificado que a recuperação de Site deve efetuar uma instalação de push, será instalado o serviço de mobilidade em cada VM para o qual pode ativa a replicação. [Saiba mais](vmware-azure-tutorial.md#enable-replication). Se tiver criado um grupo de replicação para a consistência multi VM, pode adicionar VMs a esse grupo.
6. **Ativação pós-falha de teste**. Depois de tudo está configurado, pode efetuar uma ativação pós-falha de teste para verificar que as VMs efetuar a ativação pós-falha para o Azure conforme esperado. [Saiba mais](tutorial-dr-drill-azure.md).
7. **Ativação pós-falha**. Se estiver a migrar apenas as VMs no Azure - executar uma ativação pós-falha completa para o fazer. Se estiver a configurar a recuperação após desastre, pode executar uma ativação pós-falha completa que precisar para. Para recuperação após desastre completa após a ativação pós-falha para o Azure, pode falhar novamente para o site no local como e quando estiver disponível. [Saiba mais](vmware-azure-tutorial-failover-failback.md).

## <a name="replication-process"></a>Processo de replicação

1. Ao ativar a replicação para uma VM, começa a replicar em conformidade com a política de replicação. 
2. O tráfego é replicado para o armazenamento do Azure pontos finais públicos através da internet. Em alternativa, pode utilizar o Azure ExpressRoute com [peering público](../expressroute/expressroute-circuit-peerings.md#azure-public-peering). Replicar o tráfego através de uma rede privada virtual (VPN) do site para site a partir de um site no local para o Azure não é suportada.
3. Uma cópia inicial dos dados VM é replicada para o armazenamento do Azure.
4. Após a conclusão da replicação inicial, começa a replicação das alterações de delta para o Azure. As alterações registadas relativas a uma máquina são guardadas num ficheiro .hrl.
5. Comunicação ocorrerá o seguinte:

    - VMs comunicam com o servidor de configuração no local na porta HTTPS 443 entrada para a gestão de replicação.
    - O servidor de configuração orquestra a replicação com o Azure através da porta HTTPS 443 de saída.
    - VMs enviam dados de replicação para o servidor de processos (em execução na máquina do servidor de configuração) na porta 9443 de HTTPS de entrada. Esta porta pode ser modificada.
    - O servidor de processos recebe dados de replicação, otimiza e encripta- e envia-a para o armazenamento do Azure através da porta 443 saída.


**VMware para o processo de replicação do Azure**

![Processo de replicação](./media/vmware-azure-architecture/v2a-architecture-henry.png)

## <a name="failover-and-failback-process"></a>Processo de ativação pós-falha e de reativação pós-falha

Depois de configurar a replicação e executar um exercício de recuperação após desastre (ativação pós-falha de teste) para verificar que tudo está a funcionar conforme esperado, pode executar a ativação pós-falha e reativação pós-falha conforme necessário para.

1. Executar falhar para um único computador, ou criar uma recuperação planos para efetuar a ativação pós-falha de várias VMs ao mesmo tempo. A vantagem de um plano de recuperação em vez de ativação pós-falha da máquina único incluem:
    - Pode modelo dependências de aplicações, incluindo todas as VMs em toda a aplicação num plano de recuperação simples.
    - Pode adicionar scripts, os runbooks do Azure e colocar em pausa para ações manuais.
2. Após a acionar a ativação pós-falha inicial, a consolidação-o para iniciar a aceder a carga de trabalho a partir da VM do Azure.
3. Quando o site no local primário esteja novamente disponível, pode preparar para voltar a falhar. Para voltar a falhar, terá de configurar uma infraestrutura de reativação pós-falha, incluindo:

    * **Servidor de processo temporário no Azure**: falhar novamente a partir do Azure, configurar uma VM do Azure para atuar como um servidor de processos para lidar com a replicação a partir do Azure. É possível eliminar esta VM após a conclusão da reativação pós-falha.
    * **Ligação VPN**: para falhar novamente, precisa de uma ligação VPN (ou ExpressRoute) da rede do Azure para o site no local.
    * **Servidor de destino principal separado**: por predefinição, o servidor de destino principal foi instalado com o servidor de configuração de VM de VMware no local processa a reativação pós-falha. Se precisar de falhar novamente grandes volumes de tráfego, configurar um servidor de destino principal independente no local para esta finalidade.
    * **Política de reativação pós-falha**: para replicar de novo para o site no local, precisa de uma política de reativação pós-falha. Esta política foi criada automaticamente quando criou a política de replicação no local para o Azure.
4. Depois dos componentes estão em vigor, a reativação pós-falha ocorre nas três ações:

    - Fase 1: Voltar a proteger as VMs do Azure para que o se replicar a partir do Azure para as VMs de VMware no local.
    -  Fase 2: Executar uma ativação pós-falha para o site no local.
    - Fase 3: Depois de cargas de trabalho falharam novamente, reativar a replicação para as VMs no local.
    
 
**Reativação pós-falha da VMware a partir do Azure**

![Reativação pós-falha](./media/vmware-azure-architecture/enhanced-failback.png)


## <a name="next-steps"></a>Passos Seguintes

Siga [neste tutorial](vmware-azure-tutorial.md) para ativar o VMware para a replicação do Azure.
