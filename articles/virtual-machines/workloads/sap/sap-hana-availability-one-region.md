---
title: "SAP HANA disponibilidade dentro de uma região do Azure | Microsoft Docs"
description: "Operações de SAP HANA em VMs nativas do Azure"
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: 
author: msjuergent
manager: patfilot
editor: 
tags: azure-resource-manager
keywords: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/5/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 28fe9bc7c8cb1d59df404b7dd7429def54648a18
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/08/2018
---
# <a name="sap-hana-availability-within-one-azure-region"></a>SAP HANA disponibilidade dentro de uma região do Azure
Nesta secção são apresentados vários cenários, que descrevem os cenários de disponibilidade dentro de uma região do Azure. O Azure tem várias regiões, que são distribuídas todas as através do mundo. Para obter a lista de regiões do Azure, consulte o [regiões do Azure](https://azure.microsoft.com/regions/) artigo. Implementação de SAP HANA em VMs dentro de uma região do Azure, a Microsoft oferece a implementação de uma única VM com uma instância HANA. Ou para uma maior disponibilidade implementar duas VMs com duas instâncias HANA dentro de um [do conjunto de disponibilidade do Azure](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets) que estiver a utilizar replicação do sistema HANA para fins de disponibilidade. O Azure tem uma versão de pré-visualização pública do [zonas de disponibilidade do Azure](https://docs.microsoft.com/azure/availability-zones/az-overview). Estes zonas de disponibilidade não vai ser abordada em detalhe ainda. Exceto algumas ideias em torno da utilização de conjuntos de disponibilidade versus zonas de disponibilidade gerais.

O que é a diferença entre conjuntos de disponibilidade do Azure e zonas de disponibilidade? Para regiões do Azure onde vai ser oferecido zonas de disponibilidade, as regiões têm múltiplos centros de dados, que são independentes no fornecimento de origem de energia, arrefecimento e rede. Razão para a oferta diferentes zonas numa única região do Azure é que lhe permite implementar aplicações em tow ou três zonas de disponibilidade oferecidos. Partindo do princípio que problemas na fontes de alimentação e/ou de rede iriam afetar apenas uma infraestrutura de zona de disponibilidade, a implementação da aplicação dentro de uma região do Azure ainda está totalmente funcional. Eventualmente, com algumas capacidade reduzida desde algumas VMs numa zona poderão perder-se. Mas VMs nas outras duas zonas estão ainda em execução. 
 
Enquanto, um conjunto de disponibilidade do Azure é uma capacidade de agrupamento lógico que pode utilizar no Azure para garantir que os recursos da VM é colocar na são falha isolada entre si quando estão implementadas dentro de um datacenter do Azure. O Azure garante que as VMs que colocar num Conjunto de Disponibilidade são executadas em vários servidores físicos, suportes de computação, unidades de armazenamento e comutadores de rede. Ou como em algumas outra documentação do Azure, é referido como possam ser posicionadas livremente em diferentes [atualização e domínios de falhas](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability). Normalmente, é estes possam ser posicionadas livremente dentro de um datacenter do Azure. Partindo do princípio que problemas na fontes de alimentação e/ou de rede iriam afetar o Centro de dados que é implementadas, seria afetada todas as suas capacidade uma região do Azure.

A colocação dos centros de dados que representam zonas de disponibilidade do Azure é um compromisso entre a latência de rede entre serviços implementados em diferentes zonas que são aceitáveis para a maioria das aplicações e de uma determinada distância entre os datacenters de entrega. Para que catastrophes naturais ideal seriam não afetar a capacidade e o fornecimento de rede e a infraestrutura para todas as zonas de disponibilidade nesta região. No entanto, como monumental catastrophes naturais mostrados, zonas de disponibilidade poderá nem sempre ser capazes de fornecer a disponibilidade dentro de uma região conforme pretendido. Consideração furacão Maria que ilha de Porto Rico de acessos no 20/08/2017 e basicamente causou um breve negra-limite de 100% na ilha de wide quilómetros 90.   
  


## <a name="single-vm-scenario"></a>Cenário VM único
Neste cenário, tem de criar uma Máquina Virtual do Azure para a instância de SAP HANA. Utilizar o Premium Storage do Azure para alojar o disco do sistema operativo e todos os discos de dados. O SLA de tempo de 99,9% pelo Azure e os SLAs de outros componentes do Azure são suficientes para a cumprir a SLA de disponibilidade para os seus clientes. Neste cenário, é necessário não é necessário para tirar partido de um conjunto de disponibilidade do Azure para as VMs que executam a camada DBMS. Neste cenário, baseiam-se em duas funcionalidades diferentes:

- Azure VM automática reiniciar (também referenciado como Autorrecuperação do serviço do Azure)
- SAP HANA Auto-reinício

Reinicie o automática VM do Azure ou 'autorrecuperação do serviço' é uma funcionalidade do Azure que funciona em dois níveis:

- A verificar o estado de funcionamento de uma VM alojada no anfitrião do servidor de anfitrião do servidor do Azure
- Controlador de recursos do Azure, monitorização do Estado de funcionamento e a disponibilidade do anfitrião do servidor

Para cada VM alojada num anfitrião do servidor do Azure, uma funcionalidade de verificação do Estado de funcionamento está a monitorizar o estado de funcionamento da VM alojada. No caso de VMs enquadram-se em estado de mau, um reinício da VM pode ser iniciado pelo agente de anfitrião do Azure que verifica o estado de funcionamento da VM. O controlador de recursos de infraestrutura do Azure está a verificar o estado de funcionamento do anfitrião, verificando muitos diferentes parâmetros que indicam problemas com o hardware anfitrião, mas também verifica na acessibilidade do anfitrião através da rede. Uma indicação de problemas com o anfitrião pode levar a ações como:

- Reinício do anfitrião e reinicie das VMs que estavam em execução no anfitrião se o anfitrião sinalizar um Estado de funcionamento incorreto
- Reinício do anfitrião e do reinício da VM que foram originalmente alojadas no anfitrião no anfitrião bom estado de funcionamento no caso do anfitrião não está em bom estado de funcionamento após o reinício. Neste caso, o anfitrião encontra-se que ser marcada como não bom estado de funcionamento e não utilizado para implementações mais limpo ou substituída.
- Reinicie o imediata das VMs num anfitrião bom estado de funcionamento nos casos em que o anfitrião em mau estado de funcionamento tem problemas no processo de reinício. 

Com o anfitrião e VM monitorização fornecida pelo Azure, as VMs do Azure que sofrem de problemas do anfitrião são reiniciadas automaticamente num anfitrião bom estado de funcionamento do Azure 

A funcionalidade segundo que dependem neste cenário é o facto de que o serviço HANA que é executado dentro de uma VM reiniciar esse está a ser iniciado automaticamente após o reinício da VM. O [-reinício automático do serviço de HANA](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/cf10efba8bea4e81b1dc1907ecc652d3.html) pode ser configurado através dos serviços watchdog dos diferentes serviços HANA.

Neste cenário VM único foi possível obter melhorado através da adição de um nó de ativação pós-falha amovíveis para uma configuração de SAP HANA. Ou como é realçada na documentação do SAP HANA como [anfitrião Auto-ativação pós-falha](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/ae60cab98173431c97e8724856641207.html). Esta configuração pode fazer sentido em situações de implementação no local onde o hardware de servidor e dedique um nó de servidor como nó de anfitrião Auto-ativação pós-falha de um conjunto de anfitriões de produção. No entanto para situações, como o Azure em que a infraestrutura subjacente do Azure está a fornecer um servidor de destino em bom estado de um reinício de uma VM com êxito, o cenário de SAP HANA anfitrião Auto-ativação pós-falha não fazer sentido para implementar. 

Como resultado, temos não arquitetura de referência foresees um nó de modo de espera para HANA anfitrião Auto-ativação pós-falha. Isto também se aplica a configurações de escalamento horizontal de SAP HANA.


## <a name="availability-scenarios-involving-two-different-vms"></a>Cenários de disponibilidade que envolvem duas VMs diferentes
Utilizar duas VMs do Azure dentro de conjuntos de disponibilidade do Azure permitem-lhe aumentar o tempo de cópia de segurança entre estas duas VMs, se essas VMS são colocadas num conjunto de disponibilidade do Azure dentro de uma região do Azure. O programa de configuração base no Azure teria aspeto gráficos apresentados aqui: ![duas VMs com todas as camadas](./media/sap-hana-availability-one-region/two_vm_all_shell.PNG)

Para ilustrar os cenários de disponibilidade diferente, algumas das camadas acima são as operações de corte e gráficos limitados para as camadas de VMs, anfitriões, conjuntos de disponibilidade e regiões do Azure. As VNets do Azure, grupos de recursos e subscrições não desempenham uma função para os cenários descritos.

### <a name="replicating-backups-to-second-virtual-machine"></a>Replicar as cópias de segurança para a segunda máquina virtual
Um dos setups mais rudimentares é ter cópias de segurança, especialmente transação registo cópias de segurança enviadas de uma VM para outra máquina Virtual do Azure. Tem a opção de qualquer tipo de armazenamento do Azure. Será responsável por scripting a cópia de cópias de segurança agendadas realizados em primeira VM para a VM segundo. Em caso de utilizar a necessidade de instâncias da VM segundo, terá de restaurar o completo, incremental/valor diferencial e cópias de segurança de registos de transação para o ponto de que precisa. A arquitetura deverá ter o seguinte aspeto: ![duas VMs com a replicação de armazenamento](./media/sap-hana-availability-one-region/two_vm_storage_replication.PNG) 

Esta configuração não é demasiado adequada para alcançar excelente vezes RPO e RTO. Tempos de RTO especialmente seriam afetado devido à necessidade de totalmente restaurar a base de dados completo com as cópias de segurança copiadas. No entanto, esta configuração é utilizada para recuperar de eliminação de dados inesperados nas instâncias do principais. com este tipo de configuração é capaz de em qualquer altura para restaurar para um determinado ponto no tempo, a extrair os dados e a importar os dados eliminados para a instância principal. Por conseguinte, pode tornar sentido para utilizar esse método de cópia de segurança em combinação com outras funcionalidades de elevada disponibilidade. Durante o período de tempo quando apenas as cópias de segurança são copiadas, poderá obter juntamente com uma VM mais pequena do que o SAP HANA principais instâncias em execução no. Mas tenha em atenção que as VMs mais pequenas tem número inferior de VHDs que podem ser anexados. Verifique [tamanhos de máquinas virtuais do Linux no Azure](https://docs.microsoft.com/azure/virtual-machines/linux/sizes) para limites de tipos VM individuais.

### <a name="using-sap-hana-system-replication-without-automatic-failover"></a>Utilizando a replicação do sistema de SAP HANA sem ativação pós-falha automática
Para os seguintes cenários, está a utilizar replicação do sistema do SAP HANA. Pode encontrar SAP emitido documentação começando com o artigo [replicação do sistema](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/b74e16a9e09541749a745f41246a065e.html). Entre duas VMs do Azure numa única região do Azure, existem duas configurações diferentes com algumas diferenças no objetivo de tempo de recuperação. Em geral, os cenários com não ter a ativação pós-falha automática não podem ser demasiado relevantes para cenários de dentro de uma região do Azure. Motivo que é que na maioria das situações de falha na infraestrutura do Azure, a autorrecuperação do serviço de Azure vai reiniciar a VM principal noutro anfitrião. Existem alguns casos do limite em que pode ajudar a tipo de configuração em termos de cenários de falha. Em alternativa alguns casos, como um cliente regressam, especialmente em torno da eficiência.

#### <a name="using-hana-system-replication-without-auto-failover-and-without-data-pre-load"></a>Utilizando a replicação do sistema de HANA sem ativação de pós-falha automática e sem pré-carregar de dados 
Este é um cenário em que utilize replicação do sistema de SAP HANA para fins de mover os dados de forma síncrona para atingir um objetivo de ponto de recuperação (RPO) 0. No outro lado, tem um suficientemente longa recuperação tempo objetivo (RTO), por isso, que não precisa de ativação pós-falha ou pré-carregar dos dados para a cache de instância HANA. Nesse caso, terá das possibilidades para implementarem mais economics para a configuração por:

- Pode executar outra instância de SAP HANA na segunda VM que executa a maioria da memória da máquina virtual. Normalmente, uma instância deste tipo seria numa instância em caso de uma ativação pós-falha para a segunda VM foi encerrada. Por isso, que os dados replicados podem ser carregados para a cache da instância de HANA visada na segunda VM.
- Pode utilizar um tamanho VM mais pequeno da VM segundo. Em caso de uma ativação pós-falha, terá de um passo antes da ativação pós-falha manual onde seria redimensione a VM para o tamanho da VM de origem. O cenário tem o seguinte:

![Duas VMs com a replicação de armazenamento](./media/sap-hana-availability-one-region/two_vm_HSR_sync_nopreload.PNG)

> [!NOTE]
> Mesmo sem dados pré-carregar no destino de replicação do sistema HANA, precisa de, pelo menos, 64 GB de memória e esse suficiente memória para manter os dados de rowstore na memória da instância de destino.

#### <a name="using-hana-system-replication-without-auto-failover-and-with-data-pre-load"></a>Utilizando a replicação do sistema de HANA sem ativação de pós-falha automática e com pré-carregar de dados
A diferença para o cenário introduzida antes é que os dados, que obtém replicados para a instância de HANA na VM segundo sido previamente carregados. Isto seria eliminar as duas vantagens que pode ter com o cenário não previamente de carregamento dos dados. Neste caso, não é possível executar outro sistema de SAP HANA na VM do segundo. Nem foi possível utilizar um tamanho VM mais pequeno. Por conseguinte, este é um cenário hardly implementado com os clientes


### <a name="using-sap-hana-system-replication-with-automatic-failover"></a>Utilizando a replicação do sistema de SAP HANA com ativação pós-falha automática

A configuração de disponibilidade padrão dentro de uma região do Azure, a maioria dos clientes estiver a implementar o SAP HANA, é uma configuração em que as duas máquinas virtuais do Azure com o SLES Linux tem um cluster de ativação pós-falha definido. O cluster Linux SLES baseia-se no [Pacemaker](http://www.linux-ha.org/wiki/Pacemaker) framework em conjunto com um [STONITH](http://linux-ha.org/wiki/STONITH) dispositivo. Uma perspetiva de SAP HANA, o modo de replicação utilizado está sincronizado e uma ativação pós-falha automática está configurada. Na segunda VM, a instância de SAP HANA atua como um nó em modo de espera frequente, recebe uma transmissão em fluxo síncrono de registos de alteração da instância principal de SAP HANA. Como obterem consolidadas transações pela aplicação no nó principal HANA, o nó principal do HANA aguarda para confirmar a consolidação da aplicação, até que o nó secundário do SAP HANA confirmada ter recebido o registo de consolidação. Modos de replicação síncrona diferentes de SAP HANA dois. Para obter detalhes e diferenças nestes dois modos de replicação síncrona, leia o artigo [modos de replicação para a replicação do sistema de SAP HANA](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/c039a1a5b8824ecfa754b55e0caffc01.html)

A configuração global aspeto

![Duas VMs com a replicação de armazenamento e de ativação pós-falha](./media/sap-hana-availability-one-region/two_vm_HSR_sync_auto_pre_preload.PNG)

Esta solução é escolhida porque permite-lhe atingir um RPO = 0 e um RTO baixa Alpine vezes. Configure a conectividade de cliente de SAP HANA de forma a que os clientes de SAP HANA utilizam o endereço IP virtual para ligar à configuração de replicação do sistema HANA. Isto elimina a necessidade de reconfigurar a aplicação em caso de uma ativação pós-falha para o nó secundário. Nesta solução, os SKUs de VM do Azure para o site primário ou secundário tem de ser o mesmo.  


## <a name="next-steps"></a>Próximos Passos
Se necessitar de orientação passo a passo sobre como configurar este tipo de uma configuração no Azure, leia os artigos:

- [Configurar a replicação do sistema de SAP HANA em VMs do Azure](sap-hana-high-availability.md)
- [O SAP no Azure – parte 4 – elevada disponibilidade para SAP HANA utilizando a replicação do sistema](https://blogs.sap.com/2018/01/08/your-sap-on-azure-part-4-high-availability-for-sap-hana-using-system-replication/)

Se precisar de mais informações sobre a disponibilidade de SAP HANA em regiões do Azure, leia:

- [SAP HANA disponibilidade em regiões do Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/workloads/sap/sap-hana-availability-across-regions) 

