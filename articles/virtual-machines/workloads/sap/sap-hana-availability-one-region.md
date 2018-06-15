---
title: Disponibilidade de SAP HANA dentro de uma região do Azure | Microsoft Docs
description: Descreve as operações de SAP HANA em VMs do Azure de nativas numa região do Azure.
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: msjuergent
manager: patfilot
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/05/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 47dba73a6c22d11953485a69435000d3d2fe6f55
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
ms.locfileid: "32192803"
---
# <a name="sap-hana-availability-within-one-azure-region"></a>Disponibilidade de SAP HANA dentro de uma região do Azure
Este artigo descreve os vários cenários de disponibilidade dentro de uma região do Azure. O Azure tem várias regiões, propagar-se em todo o mundo. Para obter a lista de regiões do Azure, consulte [regiões do Azure](https://azure.microsoft.com/regions/). Para a implementação de SAP HANA em VMs dentro de uma região do Azure, a Microsoft oferece a implementação de uma única VM com uma instância HANA. Para uma maior disponibilidade, pode implementar duas VMs com duas instâncias HANA dentro de um [conjunto de disponibilidade do Azure](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets) que utiliza a replicação do sistema HANA de disponibilidade. 

Atualmente, é a oferta uma pré-visualização pública do Azure [zonas de disponibilidade do Azure (pré-visualização)](https://docs.microsoft.com/azure/availability-zones/az-overview). Neste artigo, vamos não descrevem zonas de disponibilidade em detalhe. No entanto, incluímos um debate geral sobre como utilizar os conjuntos de disponibilidade versus zonas de disponibilidade.

O que é a diferença entre um conjunto de disponibilidade e de uma zona de disponibilidade no Azure? Regiões do Azure, onde são oferecidas zonas de disponibilidade tem vários centros de dados. Os centros de dados são independentes no fornecimento de origem de energia, arrefecimento e rede. O motivo para a oferta diferentes zonas numa única região do Azure é por isso pode implementar aplicações em duas ou três horários de disponibilidade que são oferecidos. Partindo do princípio que problemas de rede ou de origem de energia iriam afetar apenas uma infraestrutura de zona de disponibilidade, a implementação da aplicação dentro de uma região do Azure ainda está totalmente funcional se utilizar as zonas de disponibilidade. Poderão ocorrer alguma capacidade reduzida. Por exemplo, VMs numa zona poderão perder-se, mas as VMs do outras duas zonas ainda seria configurado e em execução. 
 
Um conjunto de disponibilidade do Azure é uma capacidade de agrupamento lógico que ajuda a garantir que os recursos da VM que colocar no conjunto de disponibilidade estão falha isoladas entre si quando estão implementadas dentro de um datacenter do Azure. Azure garante que as VMs colocar dentro de um disponibilidade definida execução em vários servidores físicos, computação bastidores, unidades de armazenamento e comutadores de rede. Em alguma documentação do Azure, esta configuração é referida como possam ser posicionadas livremente em diferentes [domínios de atualização e falhas](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability). Normalmente, é estes possam ser posicionadas livremente dentro de um datacenter do Azure. Partindo do princípio que problemas de rede de origem e de energia iriam afetar o Centro de dados que estiver a implementar, seria afetada todas as suas capacidade uma região do Azure.

A colocação dos centros de dados que representam zonas de disponibilidade do Azure é um compromisso entre a distribuição de latência de rede entre serviços implementados em diferentes zonas que aceitável para a maioria das aplicações e uma distância específica entre centros de dados. Idealmente, catastrophes naturais não afetam a capacidade, o fornecimento de rede e a infraestrutura para todas as zonas de disponibilidade nesta região. No entanto, como monumental catastrophes naturais demonstraram, zonas de disponibilidade poderão não fornecer sempre a disponibilidade que pretende que sejam dentro de uma região. Consideração Maria furacão que ilha de Porto Rico de acessos no 20 de Setembro de 2017. O furacão causado basicamente quase 100 por cento de blackout na ilha de mile-toda a 90.

## <a name="single-vm-scenario"></a>Cenário de VM única

Cenário de uma única VM, crie uma VM do Azure para a instância de SAP HANA. Utilizar o Premium Storage do Azure para alojar o disco de sistema operativo e todos os discos de dados. O tempo de atividade do Azure SLA de 99,9% e os SLAs de outros componentes do Azure é suficiente para a cumprir a SLA de disponibilidade para os seus clientes. Neste cenário, é necessário não é necessário para tirar partido de um conjunto para VMs que executam a camada DBMS de disponibilidade do Azure. Neste cenário, dependem duas funcionalidades diferentes:

- Azure VM reinício automático (também referido como autorrecuperação do serviço do Azure)
- Reinício automático de SAP HANA

Reinício de automática VM do Azure ou a autorrecuperação do serviço, é uma funcionalidade do Azure que funciona em dois níveis:

- O anfitrião do servidor do Azure verifica o estado de funcionamento de uma VM que está alojado no anfitrião do servidor.
- O controlador de recursos de infraestrutura do Azure monitoriza o estado de funcionamento e a disponibilidade do anfitrião do servidor.

Uma funcionalidade de verificação do Estado de funcionamento monitoriza o estado de funcionamento de cada VM que está alojado num anfitrião do servidor do Azure. Se uma VM estiver num Estado nonhealthy, um reinício da VM pode ser iniciado pelo agente de anfitrião do Azure que verifica o estado de funcionamento da VM. O controlador de recursos de infraestrutura verifica o estado de funcionamento do anfitrião, verificando muitos diferentes parâmetros que possam indicar problemas com o hardware anfitrião. Verifica igualmente se existem na acessibilidade do anfitrião através da rede. Uma indicação de problemas com o anfitrião pode fazer com que os seguintes eventos:

- Se o anfitrião sinalizar um Estado de funcionamento incorreto, um reinício do anfitrião e reiniciar as VMs que estavam em execução no anfitrião.
- Se o anfitrião não está em bom estado de funcionamento após o reinício, um reinício do anfitrião e reiniciar as VMs que foram originalmente alojadas no anfitrião no anfitrião bom estado de funcionamento. Neste caso, o anfitrião está marcado como não bom estado de funcionamento. Não serão utilizada para implementações mais foi limpo ou substituída.
- Se o anfitrião em mau estado de funcionamento tem problemas durante o processo de reinício, um reinício imediato das VMs num anfitrião em bom estado. 

Com o anfitrião e VM monitorização fornecida pelo Azure, as VMs do Azure que ocorrer problemas do anfitrião são reiniciadas automaticamente num anfitrião bom estado de funcionamento do Azure. 

A funcionalidade segundo que dependem deste cenário é o facto de que reinicia o serviço HANA que é executado numa VM reiniciada inicia-se automaticamente após a VM. Pode configurar [-reinício automático do serviço HANA](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/cf10efba8bea4e81b1dc1907ecc652d3.html) através dos serviços watchdog de vários serviços HANA.

Poderá melhorar este cenário única VM ao adicionar um nó de ativação pós-falha amovíveis para uma configuração de SAP HANA. A documentação de SAP HANA, esta configuração é designada [auto-ativação pós-falha do anfitrião](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/ae60cab98173431c97e8724856641207.html). Esta configuração poderá fazer sentido uma situação de implementação no local onde o hardware do servidor é limitado e dedique um nó de servidor único, como o anfitrião de nó de ativação de pós-falha automática para um conjunto de anfitriões de produção. Mas no Azure, onde a infraestrutura subjacente do Azure fornece um servidor de destino em bom estado de um reinício VM com êxito, não fazer sentido para implementar automaticamente de anfitrião de SAP HANA-ativação pós-falha. Por este motivo, não temos de nenhum arquitetura de referência foresees um nó em modo de espera para HANA anfitrião auto-ativação pós-falha. Isto também se aplica a configurações de escalamento horizontal de SAP HANA.

## <a name="availability-scenarios-for-two-different-vms"></a>Cenários de disponibilidade para duas VMs diferentes

Se utilizar duas VMs do Azure dentro de um conjunto de disponibilidade do Azure, pode aumentar o tempo de atividade entre estas duas VMs, se estas são colocadas num conjunto dentro de uma região do Azure de disponibilidade do Azure. O programa de configuração base no Azure deverá ter o seguinte aspeto:

![Diagrama de duas VMs com todas as camadas](./media/sap-hana-availability-one-region/two_vm_all_shell.PNG)

Para ilustrar os cenários de disponibilidade diferente, algumas das camadas no diagrama estão omitidas. O diagrama mostra apenas as camadas que depict VMs, anfitriões, conjuntos de disponibilidade e regiões do Azure. Instâncias, grupos de recursos e subscrições de rede Virtual do Azure não desempenham um papel nos cenários descritos nesta secção.

### <a name="replicate-backups-to-a-second-virtual-machine"></a>Replicar as cópias de segurança para uma segunda máquina virtual

Um dos setups mais rudimentares é utilizar cópias de segurança. Em particular, poderá ter cópias de segurança do registo de transações enviadas de uma VM para outra VM do Azure. Pode escolher o tipo de armazenamento do Azure. Nesta configuração for o responsável pela scripting a cópia de cópias de segurança agendadas, que são efetuadas na VM primeiro para a VM segundo. Se precisar de utilizar as segundo instâncias VM, tem de restaurar o completo, incrementais/valor diferencial e cópias de segurança de registos de transação para o ponto de que precisa. 

A arquitetura tem o seguinte aspeto:

![Diagrama de duas VMs com a replicação de armazenamento](./media/sap-hana-availability-one-region/two_vm_storage_replication.PNG) 

Esta configuração não é adequada para alcançar esta excelente tempos de objetivo de ponto de recuperação (RPO) e o objetivo de tempo de recuperação (RTO). Vezes RTO especialmente seriam afetado negativamente devido à necessidade para restaurar completamente a base de dados completo com as cópias de segurança copiadas. No entanto, esta configuração é útil para recuperar a partir de eliminação de dados inesperados nas instâncias do principais. Com esta configuração, em qualquer altura, pode restaurar para um determinado ponto no tempo, extrair os dados e importar os dados eliminados para a instância principal. Por conseguinte, poderá ser aconselhável utilizar um método de cópia de segurança em combinação com outras funcionalidades de elevada disponibilidade. 

Enquanto as cópias de segurança estão a ser copiadas, poderá utilizar uma VM mais pequena do que a VM principal que a instância de SAP HANA está em execução. Tenha em atenção que pode anexar um número mais pequeno de VHDs para VMs mais pequenas. Para obter informações sobre os limites de tipos VM individuais, consulte [tamanhos de máquinas virtuais do Linux no Azure](https://docs.microsoft.com/azure/virtual-machines/linux/sizes).

### <a name="sap-hana-system-replication-without-automatic-failover"></a>Replicação do sistema de SAP HANA sem ativação pós-falha automática

Os cenários descritos nesta secção utilizam a replicação do sistema de SAP HANA. Para a documentação do SAP, consulte [replicação do sistema](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/b74e16a9e09541749a745f41246a065e.html). Duas VMs do Azure numa única região do Azure tenham configurações diferentes, pelo que existem algumas diferenças nas RTO. Em geral, cenários sem ativação pós-falha automática não podem ser aplicadas especificamente para VMs numa região do Azure. Isto acontece porque a maioria das falhas na infraestrutura do Azure, a autorrecuperação do serviço do Azure reinicia a VM principal noutro anfitrião. Existem alguns casos de limite, onde pode ajudar a esta configuração em termos de cenários de falha. Em alternativa, em alguns casos, um cliente pode regressam mais eficiência.

#### <a name="sap-hana-system-replication-without-auto-failover-and-without-data-preload"></a>Replicação do sistema de SAP HANA sem ativação de pós-falha automática e sem dados pré-carregamento

Neste cenário, pode utilizar replicação do sistema de SAP HANA para mover dados de forma síncrona para alcançar um RPO de 0. Por outro lado, tem um suficientemente longa RTO que não precisa de ativação pós-falha ou dados pré-carregamento para a cache de instância HANA. Neste caso, é possível alcançar mais economia na configuração, efetuando as seguintes ações:

- Execute outra instância de SAP HANA na segunda VM. A instância de SAP HANA na VM segundo tira o máximo da memória da máquina virtual. Normalmente, trata-se no caso de ocorrer uma ativação pós-falha para a VM segundo. Pode desligar a VM segundo para que os dados replicados podem ser carregados para a cache da instância de HANA visada na segunda VM.
- Utilize um tamanho VM mais pequeno da VM segundo. Se ocorrer uma ativação pós-falha, é necessário um passo adicional antes da ativação pós-falha manual. Neste passo, redimensione a VM para o tamanho da VM de origem. O cenário tem o seguinte aspeto:

![Diagrama de duas VMs com a replicação de armazenamento](./media/sap-hana-availability-one-region/two_vm_HSR_sync_nopreload.PNG)

> [!NOTE]
> Mesmo se não utilizar pré-carregamento de dados no destino de replicação do sistema HANA, precisa de, pelo menos, de 64 GB de memória. Também precisa de memória suficiente para além de 64 GB para manter os dados de rowstore na memória da instância de destino.

#### <a name="sap-hana-system-replication-without-auto-failover-and-with-data-preload"></a>Replicação do sistema de SAP HANA sem ativação de pós-falha automática e com o pré-carregamento de dados

Neste cenário, os dados são replicados para a instância HANA na VM segundo é pré-carregado. Isto elimina as duas vantagens da pré-carregamento não dados. Neste caso, não é possível executar outro sistema de SAP HANA na VM do segundo. Também é possível utilizar um tamanho VM mais pequeno. Por conseguinte, os clientes raramente implementam este cenário.

### <a name="sap-hana-system-replication-with-automatic-failover"></a>Replicação do sistema de SAP HANA com ativação pós-falha automática

No padrão e mais comuns de configuração de disponibilidade dentro de uma região do Azure, duas VMs do Azure com o SLES Linux ter um cluster de ativação pós-falha definido. O cluster do SLES Linux baseia-se no [Pacemaker](http://www.linux-ha.org/wiki/Pacemaker) framework, juntamente com um [STONITH](http://linux-ha.org/wiki/STONITH) dispositivo. 

A partir de uma perspetiva de SAP HANA, está sincronizado o modo de replicação que é utilizado e uma ativação pós-falha automática está configurada. Na segunda VM, a instância de SAP HANA atua como um nó em modo de espera frequente. O nó em modo de espera recebe uma transmissão em fluxo síncrono de registos de alteração da instância principal de SAP HANA. Como a aplicação no nó principal HANA empenhadas transações, o nó principal do HANA aguarda para confirmar a consolidação da aplicação, até que o nó secundário do SAP HANA confirma que recebeu o registo de consolidação. SAP HANA oferece dois modos de replicação síncrona. Para obter mais informações e para obter uma descrição das diferenças entre estes dois modos de replicação síncrona, consulte o artigo SAP [modos de replicação para a replicação do sistema de SAP HANA](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/c039a1a5b8824ecfa754b55e0caffc01.html).

A configuração global tem o seguinte aspeto:

![Diagrama de duas VMs com a replicação de armazenamento e de ativação pós-falha](./media/sap-hana-availability-one-region/two_vm_HSR_sync_auto_pre_preload.PNG)

Poderá escolher esta solução porque permite-lhe atingir um RPO = 0 e um RTO extremamente baixa. Configure a conectividade de cliente de SAP HANA para que os clientes de SAP HANA utilizam o endereço IP virtual para ligar à configuração de replicação do sistema HANA. Isto elimina a necessidade de reconfigurar a aplicação se ocorrer uma ativação pós-falha para o nó secundário. Neste cenário, os SKUs de VM do Azure para as VMs principais e secundários têm de ser iguais.

## <a name="next-steps"></a>Passos Seguintes

Para obter orientações passo a passo sobre como configurar estas configurações no Azure, consulte:

- [Configurar a replicação do sistema de SAP HANA em VMs do Azure](sap-hana-high-availability.md)
- [Elevada disponibilidade para SAP HANA utilizando a replicação do sistema](https://blogs.sap.com/2018/01/08/your-sap-on-azure-part-4-high-availability-for-sap-hana-using-system-replication/)

Para obter mais informações sobre a disponibilidade de SAP HANA em regiões do Azure, consulte:

- [Disponibilidade de SAP HANA em regiões do Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-across-regions) 

