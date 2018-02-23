---
title: "Executar uma sequência de simulação de VMs de Hyper-V para um site secundário utilizando o Azure Site Recovery | Microsoft Docs"
description: "Saiba como executar uma simulação de recuperação para as VMs de Hyper-V em nuvens VMM para um datacenter secundário utilizando o Azure Site Recovery."
services: site-recovery
author: ponatara
manager: abhemraj
ms.service: site-recovery
ms.topic: article
ms.date: 02/12/2018
ms.author: ponatara
ms.openlocfilehash: a586eac3be39a4d3fb35dff7a4b1cc40f32f2720
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/21/2018
---
# <a name="run-a-dr-drill-for-hyper-v-vms-to-a-secondary-site"></a>Executar uma simulação de recuperação para as VMs de Hyper-V para um site secundário


Este artigo descreve como fazê-lo um exercício de recuperação (DR) após desastre para as VMs de Hyper-V que são geridos em nuvens do System Center Virtual Machine Manager V(MM), a um site secundário no local, utilizar [do Azure Site Recovery](site-recovery-overview.md).

Pode executar uma ativação pós-falha de teste para validar a sua estratégia de replicação e efetuar uma pormenorização DR sem qualquer perda de dados ou o período de indisponibilidade. Uma ativação pós-falha de teste não tem qualquer impacto sobre a replicação em curso ou no seu ambiente de produção. 

## <a name="how-do-test-failovers-work"></a>Como testar as ativações pós-falha trabalho?

Execute uma ativação pós-falha de teste a partir de principal para o site secundário. Se quiser simplesmente verificar que uma VM falha, pode executar uma ativação pós-falha de teste sem configurar nada no site secundário. Se pretender verificar funciona de ativação pós-falha de aplicação conforme esperado, terá de configurar a infraestrutura de rede e na localização secundária.
- Pode executar uma ativação pós-falha de teste a uma única VM ou com um [plano de recuperação](site-recovery-create-recovery-plans.md).
- Pode executar uma ativação pós-falha de teste sem uma rede, uma rede existente ou com uma rede criada automaticamente. Obter mais detalhes sobre estas opções são fornecidos na tabela abaixo.
    - Pode executar uma ativação pós-falha de teste sem uma rede. Esta opção é útil se pretender simplesmente verificar que uma VM conseguiu efetuar a ativação pós-falha, mas não poderá verificar qualquer configuração de rede.
    - Execute a ativação pós-falha com uma rede existente. Recomendamos que não utilize uma rede de produção.
    - Executar a ativação pós-falha e permita que a recuperação de sites, criar automaticamente uma rede de teste. Neste caso, a recuperação de sites irá criar automaticamente a rede e limpar de quando a ativação pós-falha de teste está concluída.
- Tem de selecionar um ponto de recuperação para ativação pós-falha de teste: 
    - **Mais recentes processados**: esta opção a ativação pós-falha uma VM para o ponto de recuperação mais recente, processado pelo Site Recovery. Esta opção proporciona um RTO (Objetivo de Tempo de Recuperação) baixo, porque não é despendido tempo ao processar os dados não processados.
    - **Mais recente consistentes da aplicação**: falhar esta opção através de uma VM para o ponto mais recente recuperação consistentes com aplicações processados pela recuperação de sites. 
    - **Mais recente**: esta opção primeiro processa todos os dados que foi enviados para o serviço de recuperação de sites, criar um ponto de recuperação para cada VM antes de falhar ao longo ao mesmo. Esta opção fornece o RPO mais baixo (objetivo de ponto de recuperação), uma vez que a VM criadas após ativação pós-falha terá de todos os dados replicados no Site Recovery quando a ativação pós-falha foi acionada.
    - **Mais recente várias VMS processados**: disponíveis para os planos de recuperação que incluem uma ou mais VMs que têm a consistência multi VM ativada. As VMs com a definição ativada com a ativação pós-falha do ponto de recuperação consistente de várias VMS mais recente. Outras VMs com a ativação pós-falha do ponto de recuperação de processamento mais recente.
    - **Mais recente várias VMS consistentes da aplicação**: esta opção está disponível para planos de recuperação com uma ou mais VMs que têm a consistência multi VM ativada. As VMs que fazem parte de um grupo de replicação de ativação pós-falha para o ponto de recuperação consistentes com aplicações de várias VMS mais recente. Outras VMs com a ativação pós-falha para o respetivo ponto de recuperação consistentes com aplicações mais recente.
    - **Personalizada**: Utilize esta opção para efetuar a ativação pós-falha de uma VM específica para um ponto de recuperação específico.



## <a name="prepare-networking"></a>Preparar o funcionamento em rede

Quando executar uma ativação pós-falha de teste, está a pedido para selecionar as definições de rede para máquinas de réplica de teste, conforme resumido na tabela.

**Opção** | **Detalhes** 
--- | --- 
Nenhum | VM de teste é criada no anfitrião no qual se encontra a VM de réplica. Não é adicionado à nuvem e não está ligado a nenhuma rede.<br/><br/> Pode ligar a máquina para uma rede VM, depois de terem sido criadas.
**Utilizar existentes** | VM de teste é criada no anfitrião no qual se encontra a VM de réplica. Não é adicionada a para a nuvem.<br/><br/>Crie uma rede VM que está isolada da sua rede de produção.<br/><br/>Se estiver a utilizar uma rede baseada em VLAN, recomendamos que crie uma rede lógica separada (não utilizada em produção) no VMM para esta finalidade. Esta rede lógica é utilizada para criar redes VM para as ativações pós-falha de teste.<br/><br/>A rede lógica deve estar associada a, pelo menos, um dos adaptadores de rede de todos os servidores de Hyper-V que alojam máquinas virtuais.<br/><br/>Para redes lógicas VLAN, os sites de rede que adicionar à rede lógica devem ser isolados.<br/><br/>Se estiver a utilizar uma rede lógica baseada em Virtualização de rede do Windows, o Azure Site Recovery cria automaticamente redes isoladas de VM. 
**Criar uma rede** | Uma rede de teste temporário é criada automaticamente com base nas definições que especificou nas **rede lógica** e os respetivos sites de rede relacionadas.<br/><br/> Ativação pós-falha verifica se as VMs são criadas. |Deve utilizar esta opção se utilizar mais do que uma rede VM a um plano de recuperação.<br/><br/> Se estiver a utilizar redes de Virtualização de rede do Windows, esta opção pode criar automaticamente redes VM com as mesmas definições (sub-redes e conjuntos de endereços IP) na rede da máquina virtual de réplica. Estas redes VM são automaticamente limpa após a conclusão da ativação pós-falha de teste.<br/><br/> O teste é criada a VM no anfitrião no qual existe a máquina virtual de réplica. Não é adicionada a para a nuvem.

### <a name="best-practices"></a>Melhores práticas

- Uma rede de produção de teste faz com que o período de indisponibilidade para cargas de trabalho de produção. Peça aos seus utilizadores para não utilizar aplicações relacionadas quando o exercício de recuperação após desastre está em curso.
- A rede de teste não necessita fazer corresponder o tipo de rede lógica de VMM utilizado para ativação pós-falha de teste. No entanto, algumas combinações não funcionam: - se a réplica utiliza DHCP e do isolamento baseado em VLAN, a rede VM para a réplica não necessita de um conjunto de endereços IP estáticos. Ao utilizar a virtualização de rede do Windows para a ativação pós-falha de teste poderá não funciona porque não existem conjuntos de endereços estão disponíveis. 
        -Ativação pós-falha de teste não funcionará se a rede de réplica utiliza sem isolamento, e a rede de teste utiliza Virtualização de rede do Windows. Isto acontece porque a rede sem isolamento não tem sub-redes necessárias para criar uma rede de Virtualização de rede do Windows.
- Recomendamos que não utiliza a rede que selecionou mapeamento da rede, para ativação pós-falha de teste.
- Como máquinas virtuais de réplica estão ligadas a redes VM mapeadas após a ativação pós-falha depende de como a rede VM está configurada na consola do VMM.

### <a name="vm-network-configured-with-no-isolation-or-vlan-isolation"></a>Rede VM configurada sem isolamento nem isolação de VLAN

Se uma rede VM está configurada no VMM, sem qualquer isolamento ou isolação de VLAN, tenha em atenção o seguinte:

- Se o DHCP é definido para a rede VM, a máquina virtual de réplica está ligada para o ID de VLAN através de definições que são especificadas para o site de rede na rede lógica associada. A máquina virtual recebe o respetivo endereço IP do servidor DHCP disponível.
- Não precisa de definir um conjunto de endereços IP estáticos para a rede VM de destino. Se for utilizado um conjunto de endereços IP estáticos para a rede VM, a máquina virtual de réplica está ligada para o ID de VLAN através de definições que são especificadas para o site de rede na rede lógica associada.
- A máquina virtual recebe o respetivo endereço IP do conjunto que está definido para a rede VM. Se não está definido um conjunto de endereços IP estático na rede VM de destino, a atribuição de endereços IP irá falhar. Crie conjunto de endereços IP em servidores do VMM de origem e de destino que irá utilizar para proteção e recuperação.

### <a name="vm-network-with-windows-network-virtualization"></a>Rede VM com Virtualização de rede do Windows

Se estiver configurada uma rede VM no VMM com a virtualização de rede do Windows, tenha em atenção o seguinte:

- Deve definir um conjunto estático para a rede VM de destino, independentemente se a rede VM de origem está configurada para utilizar DHCP ou um conjunto de endereços IP estáticos. 
- Se definir o DHCP, o servidor do VMM de destino age como um servidor DHCP e fornece um endereço IP do conjunto que está definido para a rede VM de destino.
- Se a utilização de um conjunto de endereços IP estáticos é definida para o servidor de origem, o VMM de destino atribui um endereço IP do conjunto. Em ambos os casos, a atribuição de endereços IP irá falhar se não for definido um conjunto de endereços IP estáticos.



## <a name="prepare-the-infrastructure"></a>Preparar a infraestrutura

Se quiser simplesmente verificar que uma VM pode falhar, pode executar uma ativação pós-falha de teste sem uma infraestrutura. Se pretender fazer uma simulação completa para testar a ativação pós-falha de aplicação, terá de preparar a infraestrutura no site secundário:

- Se executar uma ativação pós-falha de teste utilizando uma rede existente, prepare o DNS, DHCP e do Active Directory nessa rede.
- Se executar uma ativação pós-falha de teste com a opção para criar automaticamente uma rede VM, terá de adicionar os recursos de infraestrutura à rede criada automaticamente, antes de executar a ativação pós-falha de teste. Num plano de recuperação, pode facilitar este adicionando um passo manual antes de 1 de grupo no plano de recuperação que vai utilizar a ativação pós-falha de teste. Em seguida, adicione os recursos de infraestrutura para a rede automaticamente criada antes de executar a ativação pós-falha de teste.


### <a name="prepare-dhcp"></a>Preparar o DHCP
Se as máquinas virtuais envolvido na ativação pós-falha de teste utilizar DHCP, crie um servidor DHCP de teste dentro da rede para efeitos de ativação pós-falha de teste isolado.


### <a name="prepare-active-directory"></a>Preparar do Active Directory
Para executar uma ativação pós-falha de teste para a aplicação de teste, terá de uma cópia do ambiente de produção do Active Directory no seu ambiente de teste. Para obter mais informações, consulte o [considerações de ativação pós-falha do Active Directory](site-recovery-active-directory.md#test-failover-considerations).

### <a name="prepare-dns"></a>Preparar o DNS
Prepare um servidor DNS para a ativação pós-falha de teste da seguinte forma:

* **DHCP**: se as máquinas virtuais utilizar DHCP, o endereço IP do teste DNS deve ser atualizado no servidor DHCP de teste. Se estiver a utilizar um tipo de rede de Virtualização de rede do Windows, o servidor VMM atua como servidor DHCP. Por conseguinte, o endereço IP do DNS deve ser atualizado na rede de ativação pós-falha de teste. Neste caso, as máquinas virtuais registar-se ao servidor DNS relevante.
* **Endereço estático**: se as máquinas virtuais utilizar um endereço IP estático, o endereço IP do servidor DNS de teste deve ser atualizado na rede de ativação pós-falha de teste. Poderá ter de atualizar o DNS com o endereço IP de máquinas de virtuais de teste. Pode utilizar o seguinte script de exemplo para esta finalidade:

        Param(
        [string]$Zone,
        [string]$name,
        [string]$IP
        )
        $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
        $newrecord = $record.clone()
        $newrecord.RecordData[0].IPv4Address  =  $IP
        Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord



## <a name="run-a-test-failover"></a>Executar uma ativação pós-falha de teste

Este procedimento descreve como executar uma ativação pós-falha de teste para um plano de recuperação. Em alternativa, pode executar a ativação pós-falha para uma única máquina virtual no **máquinas virtuais** separador.

1. Selecione **planos de recuperação** > *recoveryplan_name*. Clique em **ativação pós-falha** > **ativação pós-falha de teste**.
2. No **ativação pós-falha de teste** painel, especifique a forma como réplica VMs deve estar ligada a redes após a ativação pós-falha de teste.
3. Controlar o progresso de ativação pós-falha no **tarefas** separador.
4. Após a conclusão da ativação pós-falha, certifique-se de que as VMs iniciam com êxito.
5. Quando tiver terminado, clique em **ativação pós-falha de teste de limpeza** no plano de recuperação. Em **Notas**, registe e guarde todas as observações associadas à ativação pós-falha de teste. Este passo elimina quaisquer VMs e as redes que foram criados pela recuperação de sites durante a ativação pós-falha de teste. 

![Ativação pós-falha de teste](./media/hyper-v-vmm-test-failover/TestFailover.png)
 


> [!TIP]
> O endereço IP especificado para uma máquina virtual durante a ativação pós-falha de teste é o mesmo endereço IP que receberia a máquina virtual para uma ativação de pós-falha planeada ou imprevista (presuming que o endereço IP está disponível na rede de ativação pós-falha de teste). Se o mesmo endereço IP não está disponível na rede de ativação pós-falha de teste, a máquina virtual recebe outro endereço IP que está disponível na rede de ativação pós-falha de teste.



### <a name="run-a-test-failover-to-a-production-network"></a>Executar uma ativação pós-falha de teste para uma rede de produção

Recomendamos que não execute uma ativação pós-falha de teste para a sua rede de site de recuperação de produção que especificou durante o mapeamento da rede. Mas se for necessário validar a conetividade de rede ponto a ponto numa VM efetuada a ativação pós-falha, tenha em atenção os seguintes pontos:

* Certifique-se de que a VM principal é encerrada quando estiver a fazer a ativação pós-falha de teste. Se não o fizer, duas máquinas virtuais com a mesma identidade será executado na mesma rede ao mesmo tempo. Nessa situação pode levar a consequências indesejadas.
* As alterações que efetuar a ativação pós-falha de teste VMs serão perdidas quando limpar as máquinas de virtuais de ativação pós-falha de teste. Estas alterações não são replicadas para as VMs primárias.
* Teste como esta situação origina o período de indisponibilidade para a sua aplicação de produção. Peça aos utilizadores da aplicação para não utilizar a aplicação quando a simulação de recuperação está em curso.  


## <a name="next-steps"></a>Passos Seguintes
Depois de executar uma simulação de recuperação com êxito, pode [executar uma ativação pós-falha completa](site-recovery-failover.md).



