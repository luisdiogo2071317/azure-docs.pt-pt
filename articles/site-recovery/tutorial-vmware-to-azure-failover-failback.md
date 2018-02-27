---
title: "Fazer a ativação pós-falha e a reativação pós-falha de VMs do VMware e de servidores físicos replicados no Azure com o Site Recovery | Microsoft Docs"
description: "Saiba como fazer a ativação pós-falha de VMs do VMware e de servidores físicos para o Azure e a reativação pós-falha para o site no local com o Azure Site Recovery"
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 02/07/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: f074312ecee39d4b3022df64b51aadd2bb8f968c
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/09/2018
---
# <a name="fail-over-and-fail-back-vmware-vms-and-physical-servers-replicated-to-azure"></a>Fazer a ativação pós-falha e a reativação pós-falha de VMs do VMware e de servidores físicos replicados no Azure

Este tutorial descreve como fazer a ativação pós-falha de uma VM do VMware para o Azure. Depois de feita a ativação pós-falha, vai fazer a reativação pós-falha para o seu site no local quando estiver disponível. Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Verificar as propriedades da VM do VMware para ver a conformidade com os requisitos do Azure
> * Executar uma ativação pós-falha para o Azure
> * Criar um servidor de processos e um servidor de destino mestre para a reativação pós-falha
> * Voltar a proteger as VMs do Azure para o site no local
> * Fazer a ativação pós-falha do Azure o local
> * Voltar a proteger VMs no local, para iniciar a replicação para o Azure novamente

Este é o quinto tutorial de uma série. Este tutorial parte do princípio de que já concluiu as tarefas nos tutoriais anteriores.

1. [Preparar o Azure](tutorial-prepare-azure.md)
2. [Preparar o VMware no local](tutorial-prepare-on-premises-vmware.md)
3. [Configurar a recuperação após desastre](tutorial-vmware-to-azure.md)
4. [Executar um teste de recuperação após desastre](tutorial-dr-drill-azure.md)

## <a name="preparing-for-failover-and-failback"></a>Preparar a ativação pós-falha e a reativação pós-falha

Confirme que não existem instantâneos na VM. A VM no local é desativada durante a nova proteção.
Desta forma, garante-se a consistência dos dados durante a replicação. Não ative a VM após a conclusão da nova proteção. Utilize o mesmo servidor de destino mestre para voltar a proteger um grupo de replicação. Se utilizar um servidor de destino mestre diferente para voltar a proteger um grupo de replicação, o servidor não pode fornecer um ponto no tempo comum.

A ativação pós-falha e a reativação pós-falha têm quatro fases:

1. **Fazer a ativação pós-falha para o Azure**: fazer a ativação pós-falha de máquinas do site no local para o Azure.
2. **Voltar a proteger as VMs do Azure**: voltar a proteger as VMs do Azure, para que comecem a ser replicadas novamente para as VMs do VMware no local.
3. **Fazer a ativação pós-falha no local**: executar uma ativação pós-falha para fazer a reativação pós-falha do Azure.
4. **Voltar a proteger VMs no local**: após a reativação pós-falha dos dados, voltar a proteger as VMs no local das quais foi feita a reativação pós-falha, para que comecem a ser replicadas para o Azure.

## <a name="verify-vm-properties"></a>Verificar as propriedades da VM

Verifique as propriedades da VM e certifique-se de que a VM está em conformidade com [requisitos do Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).

1. Em **Itens Protegidos**, clique em **Itens Replicados** > VM.

2. No painel **Item replicado**, existe um resumo das informações, do estado de funcionamento e dos pontos de recuperação mais recentes disponíveis da VM. Clique em **Propriedades** para ver mais detalhes.

3. Em **Computação e Rede**, pode modificar o nome do Azure, o grupo de recursos, o tamanho de destino, o [conjunto de disponibilidade](../virtual-machines/windows/tutorial-availability-sets.md) e as [definições do disco gerido](#managed-disk-considerations)

4. Pode ver e modificar as definições de rede, incluindo a rede/sub-rede na qual a VM do Azure será localizada após a ativação pós-falha e o endereço IP que será atribuído à mesma.

5. Em **Discos**, pode ver informações sobre o sistema operativo e os discos de dados na VM.

## <a name="run-a-failover-to-azure"></a>Executar uma ativação pós-falha para o Azure

1. Em **Definições** > **Itens replicados** clique na VM > **Ativação Pós-falha**.

2. Em **Ativação pós-falha**, selecione um **Ponto de Recuperação** para o qual fazer a ativação pós-falha. Pode utilizar uma das opções seguintes:
   - **Mais recente** (predefinição): esta opção processa primeiro todos os dados enviados para o Site Recovery. Disponibiliza o último RPO (Objetivo de Ponto de Recuperação), porque a VM do Azure criada após a ativação pós-falha tem todos os dados que foram replicados para o Site Recovery quando a ativação pós-falha foi acionada.
   - **Processado mais recentemente**: esta opção faz a ativação pós-falha da VM para o ponto de recuperação mais recente processado pelo Site Recovery. Esta opção proporciona um RTO (Objetivo de Tempo de Recuperação) baixo, porque não é despendido tempo ao processar os dados não processados.
   - **Consistente com a aplicação mais recente**: esta opção faz a ativação pós-falha da VM para o último ponto de recuperação consistente com a aplicação processado pelo Site Recovery.
   - **Personalizado**: especifique um ponto de recuperação.

3. Selecione **Encerrar a máquina antes de iniciar a ativação pós-falha** para tentar encerrar as máquinas virtuais de origem antes de acionar a ativação pós-falha. A ativação pós-falha continua, mesmo que o encerramento falhe. Pode seguir o progresso da ativação pós-falha na página **Trabalhos**.

4. Se preparou para ligar à VM do Azure, ligue para validá-la após a ativação pós-falha.

5. Depois de verificar, **consolide** a ativação pós-falha. São eliminados todos os pontos de recuperação disponíveis.

> [!WARNING]
> **Não cancelar uma ativação pós-falha em curso**: antes do início da ativação pós-falha, a replicação da VM é parada.
> Se cancelar uma ativação pós-falha que esteja em curso, a mesma para, mas a VM não será replicada outra vez.

Em alguns cenários, a ativação pós-falha requer processamento adicional, que demora cerca de oito a dez minutos a concluir. Poderá reparar em tempos de ativação pós-falha superiores Para servidores físicos, máquinas do Linux do VMware, VMs do VMware que não têm o serviço DHCP ativado e VMs do VMware que não têm os controladores de arranque storvsc, vmbus, storflt, intelide e atapi.

## <a name="create-a-process-server-in-azure"></a>Criar um servidor de processos no Azure

O servidor de processos recebe dados da VM do Azure e envia-os para o site no local. É necessária uma rede de latência baixa entre o servidor de processos e a VM protegida.

- Para fins de teste, se tiver uma ligação do Azure ExpressRoute, pode utilizar o servidor de processos no local que é instalado automaticamente no servidor de configuração.
- Se tiver uma ligação VPN ou se estiver a executar a reativação pós-falha num ambiente de produção, tem de configurar uma VM do Azure como um servidor de processos baseado no Azure para reativação pós-falha.
- Para configurar um servidor de processos no Azure, siga as instruções [neste artigo](site-recovery-vmware-setup-azure-ps-resource-manager.md).

## <a name="configure-the-master-target-server"></a>Configurar o servidor de destino mestre

Por predefinição, o servidor de destino mestre é executado no servidor de configuração no local. Para efeitos deste tutorial, vamos utilizar o servidor mestre principal. O servidor de destino mestre recebe dados da reativação pós-falha.

Se a VM num anfitrião ESXi que é gerido por um vCenter Server, o servidor de destino mestre tem de ter acesso ao arquivo de dados da VM (VMDK), para escrever os dados replicados para os discos da VM. Certifique-se de que o arquivo de dados da VM está montado no anfitrião do servidor de destino mestre, com acesso de leitura/escrita.

Se a VM estiver num ESXi que não seja gerido por um vCenter Server, o serviço Site Recovery cria uma VM nova durante a nova proteção. A VM é criada no anfitrião ESX no qual criou o servidor de destino principal.
O disco rígido da VM tem de estar num arquivo de dados que seja acessível pelo anfitrião no qual o servidor de destino mestre está em execução.

Se a VM não utilizar o vCenter, deve concluir a deteção do anfitrião no qual o servidor de destino mestre está em execução, antes de poder voltar a proteger a máquina. Isto também se aplica à reativação pós-falha de servidores físicos. Se a VM no local existir, outra opção é eliminá-la antes de fazer uma reativação pós-falha. A reativação pós-falha cria, em seguida, uma VM nova no mesmo anfitrião do anfitrião ESX do servidor de destino mestre. Quando fizer a reativação pós-falha para uma localização alternativa, os dados são recuperados para o mesmo arquivo de dados e para o mesmo anfitrião ESX que foi utilizado pelo servidor de destino mestre no local.

Não pode utilizar o Storage vMotion no servidor de destino mestre. Se o fizer, a reativação pós-falha não funcionará, porque os discos não estão disponíveis para a mesma. Exclua os servidores de destino mestre da sua lista do vMotion.

## <a name="reprotect-azure-vms"></a>Voltar a proteger VMs do Azure

Este procedimento parte do princípio de que a VM no local não está disponível e que está a proteger novamente para uma localização alternativa.

1. Em **Definições** > **Itens replicados**, clique com o botão direito do rato na VM da qual foi feita a ativação pós-falha > **Voltar a proteger**.
2. Em **Voltar a proteger**, confirme que está selecionado **Do Azure para o local**.
3. Especifique o servidor de destino mestre no local e o servidor de processos.

4. Em **Arquivo de dados**, selecione o arquivo de dados de destino mestre para o qual pretende recuperar os discos no local. Utilize esta opção se a VM no local tiver sido eliminada e for preciso criar discos novos. Esta definição é ignorada se os discos já existirem, mas tem de especificar um valor.
5. Selecione a unidade de retenção de destino mestre. A política de reativação pós-falha é selecionada automaticamente.
6. Clique em **OK** para começar a nova proteção. Um trabalho começa a replicar a máquina virtual do Azure para o site no local. Pode acompanhar o progresso no separador **Trabalhos**.

> [!NOTE]
> Se quiser recuperar a VM do Azure para uma VM no local já existente, monte o arquivo de dados da máquina virtual no local com acesso de leitura/escrita no anfitrião ESXi do servidor de destino mestre.


## <a name="run-a-failover-from-azure-to-on-premises"></a>Executar uma ativação pós-falha do Azure para o local

Para replicar novamente para o local, é utilizada uma política de reativação pós-falha. Esta política é criada automaticamente quando criou uma política de replicação para replicação para o Azure:

- A política é associada automaticamente ao servidor de configuração.
- A política não pode ser modificada.
- Os valores da política são:
    - Limiar de RPO = 15 minutos
    - Retenção de ponto de recuperação = 24 horas
    - Frequência de instantâneos consistentes com a aplicação = 60 minutos

Execute a ativação pós-falha da seguinte forma:

1. Na página **Itens Replicados**, clique com o botão direito do rato na máquina > **Ativação pós-falha não planeada**.
2. Em **Confirmar Ativação Pós-falha**, confirme que a direção da ativação pós-falha provém do Azure.

3. Selecione o ponto de recuperação que pretende utilizar para a ativação pós-falha. Um ponto de recuperação consistente com a aplicação ocorre antes do ponto no tempo mais recente, o que provoca alguma perda de dados. Quando a ativação pós-falha é executada, o Site Recovery encerra as VMs do Azure e arranca a VM no local. Haverá algum tempo de inatividade, pelo que deve escolher uma hora adequada.
4. Clique com o botão direito do rato na máquina e clique em **Consolidar**. É acionado um trabalho que remove as VMs do Azure.
5. Confirme que as VMs do Azure foram encerradas, conforme esperado.


## <a name="reprotect-on-premises-machines-to-azure"></a>Voltar a proteger máquinas no local para o Azure

Os dados devem estar agora outra vez no site no local, mas não estão a ser replicados para o Azure. Pode iniciar a replicação para o Azure novamente da seguinte forma:

1. Em cofre > **Definições** >**Itens Replicados**, selecione as VMs das quais foi feita reativação pós-falha e clique em **Voltar a Proteger**.
2. Selecione o servidor de processos que é utilizado para enviar os dados replicados para o Azure e clique em **OK**.

Após a conclusão da nova proteção, a VM é replicada outra vez para o Azure e pode executar uma ativação pós-falha, conforme necessário.
