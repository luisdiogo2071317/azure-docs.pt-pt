---
title: Configurações de carga de trabalho SAP com zonas de disponibilidade do Azure | Documentos da Microsoft
description: Arquitetura de elevada disponibilidade e cenários para utilizar zonas de disponibilidade do Azure do SAP NetWeaver
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: juergent
manager: patfilot
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: 887caaec-02ba-4711-bd4d-204a7d16b32b
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 02/03/2019
ms.author: msjuergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 56c1ffd314a9a8e9440832b9fd92a51cdaf9f228
ms.sourcegitcommit: 3aa0fbfdde618656d66edf7e469e543c2aa29a57
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/05/2019
ms.locfileid: "55735656"
---
# <a name="sap-workload-configurations-with-azure-availability-zones"></a>Configurações de carga de trabalho SAP com zonas de disponibilidade do Azure

Uma das funcionalidades de elevada disponibilidade para melhorar a disponibilidade geral da carga de trabalho SAP no Azure, o Azure oferece são [zonas de disponibilidade do Azure](https://docs.microsoft.com/azure/availability-zones/az-overview). As zonas de disponibilidade estão disponíveis em diferentes [regiões do Azure](https://azure.microsoft.com/global-infrastructure/regions/) já. Irão seguir a mais regiões. 

A arquitetura básica de elevada disponibilidade do SAP pode ser descrita, tal como apresentado neste gráfico:

![Configuração de HA padrão](./media/sap-ha-availability-zones/standard-ha-config.png)

Camada de aplicação SAP é implementada numa do Azure [conjunto de disponibilidade](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability). Para elevada disponibilidade do SAP Central Services, implementar duas VMs num conjunto de disponibilidade separados e utilizar serviços de Cluster de ativação pós-falha do Windows ou Pacemaker (Linux) para implementar uma arquitetura de elevada disponibilidade que permite a ativação pós-falha automática em caso de uma infraestrutura ou problema de software. Documentação com detalhes sobre essa lista de implementações, como:

- [Uma instância do SAP ASCS/SCS de cluster num cluster de ativação pós-falha do Windows utilizando um disco partilhado de cluster](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-shared-disk)
- [Uma instância do SAP ASCS/SCS de cluster num cluster de ativação pós-falha do Windows ao utilizar a partilha de ficheiros](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-file-share)
- [Elevada disponibilidade para SAP NetWeaver em VMs do Azure no SUSE Linux Enterprise Server para aplicações de SAP](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse)
- [Azure máquinas virtuais elevada disponibilidade para SAP NetWeaver em Red Hat Enterprise Linux](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel)

Uma arquitetura semelhante aplica-se para a camada do DBMS da SAP NetWeaver, S/4HANA ou Hybris sistemas. Implementar a camada do DBMS em modo ativo/passivo com uma solução de cluster de ativação pós-falha, que usa o DBMS estruturas de ativação pós-falha específico, serviços de Cluster de ativação pós-falha do Windows ou Pacemaker para iniciar as atividades de ativação pós-falha em caso de uma infraestrutura ou software Falha. 

Para poder implementar a mesma arquitetura utilizar zonas de disponibilidade do Azure, algumas alterações para a arquitetura descrita precisam ser feitas. Estas alterações são descritas as diferentes partes deste documento.

## <a name="considerations-deploying-across-availability-zones"></a>Considerações de implantação em zonas de disponibilidade

Utilizar zonas de disponibilidade, existem alguns aspetos a considerar. A lista de considerações como:

- As zonas de disponibilidade do Azure não dá nenhuma garantia de determinados distância entre as zonas diferentes dentro de uma região do Azure
- As zonas de disponibilidade do Azure não são uma solução de DR ideal. Em tempos em que desastres naturais enormes estão a causar ampla spread danos em algumas regiões do mundo, incluindo danos pesado à infra-estrutura de energia, a distância entre zonas diferentes podem não ser suficientemente grande para ser qualificado como uma solução de DR adequada
- A latência de rede em zonas de disponibilidade do Azure diferentes em diferentes regiões do Azure é diferente da região para outra região do Azure. Há casos, onde é possível executar a camada de aplicação SAP implementada em diferentes zonas, uma vez que a latência de rede de uma zona para o ativo DBMS VM é ainda aceitável de um impacto de processo comercial. Mas existem cenários em algumas regiões do Azure em que a latência entre a VM DBMS Active Directory numa zona e uma instância de aplicação SAP noutra zona pode ser demasiado INVASIVO e não é aceitável para os processos de negócios SAP. Como resultado, a arquitetura de implementação tem de ser diferente com uma arquitetura de ativo/ativo para a aplicação ou a arquitetura de ativo/passivo em que a latência de zona cruzada é demasiado elevada.
- Tome uma decisão numa configuração de que poderia usar as zonas de disponibilidade do Azure para. Com base na latência de rede, que está medindo entre as zonas diferentes. A latência de rede desempenha um papel importante em duas áreas diferentes:
    - Latência entre as duas instâncias do DBMS que tem de ter uma replicação síncrona estabelecida. Quanto maior for a latência de rede, maior será o potencial de escalabilidade de impacto da sua carga de trabalho
    - A diferença na latência de rede entre uma VM que executa uma instância de caixa de diálogo SAP na mesma zona de como a instância ativa do DBMS e uma VM semelhante noutra zona. Maior essa diferença, o maior o impacto sobre o tempo de processos de negócios e tarefas de lote, dependentes independentemente de serem executadas na mesma zona com o DBMS ou numa zona diferente de execução


A partir do uso de recursos do Azure, existem algumas restrições de funcionalidade que podem ser utilizados quando implementar VMs do Azure entre zonas e o estabelecimento de soluções de ativação pós-falha em zonas de disponibilidade diferente na mesma região do Azure. Estas listas de restrições, como:

- Usando [discos geridos do Azure](https://azure.microsoft.com/services/managed-disks/) é obrigatório para a implantação em zonas de disponibilidade do Azure 
- O mapeamento de enumerações de zona para zonas físicas é corrigido numa base de subscrição do Azure. Se estiver a utilizar para implementar os seus sistemas SAP subscrições diferentes, é necessário definir as zonas ideais para cada subscrição separadamente
- Não é possível implementar conjuntos de disponibilidade do Azure dentro de uma zona de disponibilidade do Azure. Escolha uma zona de disponibilidade do Azure ou um conjunto como uma estrutura de implantação para máquinas virtuais de disponibilidade do Azure.
- Não é possível utilizar um [Balanceador de carga básico do Azure](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview#skus) para criar soluções de cluster com base nos serviços de Cluster de ativação pós-falha do Windows ou Linux Pacemaker de ativação pós-falha. Em vez disso, tem de utilizar o [SKU de Balanceador de carga Standard do Azure](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-availability-zones)



## <a name="ideal-zone-combination"></a>Combinação ideal de zona
Para poder decidir em como pode aproveitar as zonas de disponibilidade, terá de efetuar uma investigação que dá-lhe:

- A latência de rede entre as três zonas diferentes de uma região do Azure. Então, que pode escolher as zonas que têm a menor latência de rede no cruzada tráfego de rede de zona
- A diferença entre a latência de VM para VM dentro de uma das zonas escolhidas por si e a latência de rede entre duas zonas que escolheu
- Uma instrução se a VM tipos que tem de ter implementado em zonas de disponibilidade de preferência estão disponíveis nas duas zonas da sua preferência. Especialmente com máquinas de virtuais de série M, vai encontrar situações em que os diferentes SKUs de VM de série M vai estar disponível em duas as três zonas

### <a name="network-latency-between-zones-and-within-zone"></a>Latência de rede entre zonas e dentro de zona
Para saber o que é a latência entre as zonas diferentes, terá de:

- Implemente o SKU de VM que pretende utilizar para a sua instância do DBMS em todas as três zonas. Certifique-se de que que [Azure Accelerated Networking](https://azure.microsoft.com/blog/maximize-your-vm-s-performance-with-accelerated-networking-now-generally-available-for-both-windows-and-linux/) é ativada quando efetuar esta medida
- Como encontrar duas zonas com a menor latência de rede, implemente outro três VMs do SKU de VM que pretende utilizar como a camada de aplicativo VM entre as três zonas de disponibilidade. Medir a latência de rede em relação a "DBMS VMs' duas em duas zonas 'DBMS' diferentes da sua preferência. 
- Como uma ferramenta para medir, utilize **niping**. Uma ferramenta do SAP, que funciona conforme descrito nas notas de suporte do SAP [#500235](https://launchpad.support.sap.com/#/notes/500235) e [#1100926](https://launchpad.support.sap.com/#/notes/1100926/E). Concentre-se sobre os comandos SAP documentada para medidas de latência. Usando **ping** não é uma ferramenta recomendada desde **ping** não funcionará com o Azure acelerado de caminhos de código de rede.

Com base em medidas e a disponibilidade das suas SKUs de VM em diferentes horários, terá de tomar as decisões:

- Definir as zonas ideais para a camada DBMS
- Responder à pergunta se, com base nas diferenças de latência de rede dentro de uma zona de ou entre zonas, pode distribuir sua camada de aplicação SAP Active Directory num, dois ou todos os três zonas diferentes
- Responder à pergunta se pretende implementar um ativo/passivo ou uma configuração de ativo/ativo de um ponto de vista do aplicativo (ver mais tarde)

Essas decisões, lembre-se também as recomendações de latência de rede do SAP conforme documentado na nota SAP [#1100926](https://launchpad.support.sap.com/#/notes/1100926/E).

### <a name="be-aware-of"></a>Tenha em atenção

> [!IMPORTANT]
> As medidas e as decisões que tomar, são válidos para a subscrição do Azure que utilizou tornar essas medidas. Utilizar outra subscrição do Azure, que precisa repetir as medidas, uma vez que o mapeamento da sua enumeração dependentes de subscrição de zonas pode alterar com uma subscrição diferente


> [!IMPORTANT]
> Espera-se que as medidas como efetuada acima mostram resultados diferentes em cada região do Azure que suporta [zonas de disponibilidade](https://docs.microsoft.com/azure/availability-zones/az-overview). Mesmo com os requisitos de latência de rede não será alterado, poderá ter de adaptar as estratégias de implementação diferentes em diferentes regiões do Azure, uma vez que a latência de rede entre zonas pode ser diferente. É esperado que em algumas regiões do Azure, latência de rede entre as três zonas diferentes pode ser bastante diferente. Enquanto em outras regiões, a latência de rede entre as três zonas diferentes são mais uniforme. A afirmação de facto **sempre** é uma latência de rede em zonas de 1 milissegundo para 2 milissegundos é **errado**. A latência de rede em zonas de disponibilidade em regiões do Azure não pode ser generalizada.


## <a name="activeactive-deployment"></a>Implementação de ativo/ativo
Esta arquitetura de implementação é chamada ativo/ativo, uma vez que implementar suas instâncias de caixa de diálogo SAP ativas em duas ou três zonas. O SAP Central Services utilizando a replicação de colocar em fila vai ser implementado entre duas zonas. O mesmo é válido para a camada do DBMS, o que vai ser implantado em zonas da mesmo que o serviço de Central de SAP.

Para contemplar esta configuração, terá de encontrar duas zonas de disponibilidade na sua região que oferecem a latência de rede de zona cruzado que é aceitável para a sua carga de trabalho e para a replicação síncrona do DBMS. Além disso pretende que o delta entre a latência de rede dentro de zonas que selecionou e a entre a latência de rede de zona não era demasiado grande. Razão para a última opção, é que não quer demasiado grandes variações nos tempos de execução de seus processos de negócios ou de processos do batch dependentes no se uma tarefa é executada na zona com o servidor do DBMS ou através de zona. Algumas variações são aceitáveis, mas não os fatores de diferença.

Um esquema simplificado de uma implementação de ativo/ativo entre duas zonas poderia ter o seguinte aspeto:

![active_active_xone_deployment](./media/sap-ha-availability-zones/active_active_zones_deployment.png)

As seguintes considerações aplicam-se para esta configuração:

- Tratar as zonas de disponibilidade do Azure como domínios de falha e atualização de todas as VMs, uma vez que os conjuntos de disponibilidade não podem ser implementados em zonas de disponibilidade do Azure
- Os balanceadores de carga do Azure que utiliza para os clusters de ativação pós-falha do SAP Central Services, bem como de camada do DBMS, tem de ser o [Balanceador de carga de Standard SKU](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-availability-zones). O Balanceador de carga básico não funcionará em zonas
- A rede virtual do Azure e respetivas sub-redes implementou para alojar o sistema SAP são transferidas entre zonas. **Não é necessário** separar as redes virtuais para cada zona
- Para todas as máquinas virtuais a implementar, utilize [discos geridos do Azure](https://azure.microsoft.com/services/managed-disks/). Discos não geridos não são suportados para implementações zonais
- Armazenamento Premium do Azure ou [armazenamento Ultra SSD](https://docs.microsoft.com/azure/virtual-machines/windows/disks-ultra-ssd) não estiver sustentando qualquer tipo de replicação de armazenamento em zonas. Como resultado é responsabilidade do aplicativo (DBMS ou SAP Central Services) para replicar dados importantes
- Mesmo é válido para o diretório partilhado /sapmnt, que é um disco partilhado (Windows), uma partilha CIFS (Windows), ou partilhar de NFS (Linux). Tem de utilizar uma tecnologia, que replica o disco partilhado ou partilha entre as zonas. Neste momento, são suportadas as seguintes tecnologias:
    - Para Windows, uma solução de cluster que utiliza o SIOS Datakeeper conforme documentado [uma instância do SAP ASCS/SCS de Cluster num cluster de ativação pós-falha do Windows ao utilizar um disco partilhado de cluster no Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-shared-disk) é suportado nas zonas
    - Para o SUSE Linux, uma partilha NFS criado conforme documentado [elevada disponibilidade para NFS nas VMs do Azure no SUSE Linux Enterprise Server](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs) é suportada
    - No momento, a solução usando serviços de ficheiros de escalamento horizontal do Windows (SOFS), conforme documentado [infraestrutura de preparar o Azure para elevada disponibilidade SAP através de uma partilha de ficheiros e de cluster de ativação pós-falha do Windows para as instâncias do SAP ASCS/SCS](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-infrastructure-wsfc-file-share) **não são suportadas nas zonas**
- A terceiro zona é utilizada para alojar o dispositivo SBD no caso de criar um [cluster de pacemaker do SUSE Linux](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker#create-azure-fence-agent-stonith-device) ou instâncias de aplicações adicionais
- Para obter consistência de tempo de execução de algumas transações comerciais críticos e/ou tarefas. Pode experimentar direcionar a determinados utilizadores diretamente para as instâncias de aplicação específica que estão numa zona com a instância ativa do DBMS utilizando grupos de servidores de Batch do SAP, grupos de Logon ou grupos de RFC e tarefas de lote. No entanto, em caso de uma ativação pós-falha zonal, terá de mover manualmente estes grupos para instâncias de caixa de diálogo, que estão as zona ou zonas restantes 
- Decidir se pretende implementar algumas instâncias de caixa de diálogo inativo em cada uma das zonas de ser capaz de obter imediatamente para a capacidade de recursos anteriores, no caso de uma zona implementou parte das suas instâncias de aplicação, está fora do serviço


## <a name="activepassive-deployment"></a>Implementação de ativo/passivo
Em casos em que não estão encontrando uma aceitável delta entre a latência de rede dentro de um tráfego de rede de zona de zona e para várias, a arquitetura que pode implementar tem um caráter de modo ativo/passivo do ponto de vista do SAP aplicação camada. É possível definir uma zona 'active', que é a zona implementa a camada de aplicativo completo em e onde está tentando executar a instância ativa do DBMS e a instância ativa do SAP Central Services. Com esta configuração, certifique-se de que não tem de diferença de tempo de execução extreme nas transações comerciais e tarefas de lote, dependentes se com que uma tarefa for executada na mesma zona com a instância ativa do DBMS ou não.

O layout básico de tal uma arquitetura é semelhante a:

![active_active_xone_deployment](./media/sap-ha-availability-zones/active_active_zones_deployment.png)

As seguintes considerações aplicam-se para esta configuração:

- Conjuntos de disponibilidade não podem ser implementados em zonas de disponibilidade do Azure. Portanto, neste caso, é terminar com um domínio de atualização e com falha para a camada de aplicação. Motivo é que apenas é implementado numa zona. Esta configuração é um infeliz pequena em comparação comparada a arquitetura de referência que foresees que implemente a camada de aplicativo num conjunto de disponibilidade do Azure.
- Operando uma arquitetura desse tipo, precisa monitorizar de perto e tente manter as instâncias de serviços DBMS e SAP Central ativas na mesma zona de camada de aplicação implementado. Em caso de uma ativação pós-falha do serviço de Central de SAP ou a instância do DBMS, pretende certificar-se de que pode manualmente falhar novamente para a zona com a camada de aplicação SAP implementada mais cedo possível
- Os balanceadores de carga do Azure que utiliza para os clusters de ativação pós-falha do SAP Central Services, bem como de camada do DBMS, tem de ser o [Balanceador de carga de Standard SKU](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-availability-zones). O Balanceador de carga básico não funcionará em zonas
- A rede virtual do Azure e respetivas sub-redes implementou para alojar o sistema SAP são transferidas entre zonas. **Não é necessário** separar as redes virtuais para cada zona
- Todas as máquinas virtuais a implementar, precisa usar [discos geridos do Azure](https://azure.microsoft.com/services/managed-disks/). Discos não geridos não são suportados para implementações zonais
- Armazenamento Premium do Azure ou [armazenamento Ultra SSD](https://docs.microsoft.com/azure/virtual-machines/windows/disks-ultra-ssd) não estiver sustentando qualquer tipo de replicação de armazenamento em zonas. Como resultado é responsabilidade do aplicativo (DBMS ou SAP Central Services) para replicar dados importantes
- Mesmo é válido para o diretório partilhado /sapmnt, que é um disco partilhado (Windows), uma partilha CIFS (Windows), ou partilhar de NFS (Linux). Tem de utilizar uma tecnologia, que replica o disco partilhado ou partilha entre as zonas. Neste momento, são suportadas as seguintes tecnologias:
    - Para Windows, uma solução de cluster que utiliza o SIOS Datakeeper conforme documentado [uma instância do SAP ASCS/SCS de Cluster num cluster de ativação pós-falha do Windows ao utilizar um disco partilhado de cluster no Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-shared-disk) é suportado nas zonas
    - Para o SUSE Linux, uma partilha NFS criado conforme documentado [elevada disponibilidade para NFS nas VMs do Azure no SUSE Linux Enterprise Server](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs) é suportada
    - No momento, a solução usando serviços de ficheiros de escalamento horizontal do Windows (SOFS), conforme documentado [infraestrutura de preparar o Azure para elevada disponibilidade SAP através de uma partilha de ficheiros e de cluster de ativação pós-falha do Windows para as instâncias do SAP ASCS/SCS](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-infrastructure-wsfc-file-share) **não são suportadas nas zonas**
- A terceiro zona é utilizada para alojar o dispositivo SBD no caso de criar um [cluster de pacemaker do SUSE Linux](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker#create-azure-fence-agent-stonith-device) ou instâncias de aplicações adicionais
- Implementar em VMs Inativas na zona passiva (do ponto de vista do DBMS) para poder iniciar os recursos de aplicação em caso de falha de zona
    - Não é possível usar [do Azure Site Recovery](https://azure.microsoft.com/services/site-recovery/) para replicar VMs do Active Directory para VMs Inativas entre zonas. No momento, não é capaz de atender a uma função do Azure Site Recovery
- Investir para a automatização permite-lhe, em caso de falha de zona, para iniciar automaticamente a camada de aplicação SAP na segunda zona

## <a name="combined-high-availability-and-disaster-recovery-configuration"></a>Combinados de elevada disponibilidade e a configuração da recuperação após desastre
Apesar do fato de que a Microsoft não está concedendo todas as informações à distância geográfica entre os recursos que alojar zonas de disponibilidade do Azure diferente numa determinada região do Azure, alguns clientes estão a tirar partido zonas para uma configuração de HA e DR combinada que promete um **R**ecovery **P**oint **s**bjective (RPO) igual a zero. O que significa, não deverá perder quaisquer transações consolidadas de base de dados, mesmo no caso de recuperação após desastre. 

> [!NOTE]
> Uma configuração como isso é recomendada para apenas as circunstâncias específicas. Por exemplo, casos em que os dados não podem deixar a região do Azure devido a motivos de segurança e conformidade. 

Um exemplo de como essa configuração pode ter um aspeto como é demonstrado neste gráfico:

![combined_ha_dr_in_zones](./media/sap-ha-availability-zones/combined_ha_dr_in_zones.png)

As seguintes considerações aplicam-se para esta configuração:

- É supondo que existe uma distância significativa entre os recursos que aloja uma zona de disponibilidade. Ou é forçado a manter-se com uma determinada região do Azure. Conjuntos de disponibilidade não podem ser implementados em zonas de disponibilidade do Azure. Portanto, neste caso, é terminar com um domínio de atualização e com falha para a camada de aplicação. Motivo é que apenas é implementado numa zona. Este é um infeliz em comparação comparada a arquitetura de referência que foresees que implemente a camada de aplicativo num conjunto de disponibilidade do Azure.
- Operando uma arquitetura desse tipo, precisa monitorizar de perto e tente manter as instâncias de serviços DBMS e SAP Central ativas na mesma zona de camada de aplicação implementado. Em caso de uma ativação pós-falha do serviço de Central de SAP ou a instância do DBMS, pretende certificar-se de que pode manualmente falhar novamente para a zona com a camada de aplicação SAP implementada mais cedo possível
- Tem instâncias de aplicações de produção pré-instaladas nas VMs que executam as Active Directory instâncias da aplicação de controle de qualidade.
- Em caso de falha zonal, encerrar as instâncias do aplicativo de controle de qualidade e iniciar as instâncias de produção em vez disso. Lembre-se de que precisa para trabalhar com nomes virtual para instâncias da aplicação fazer isso funcionar
- Os balanceadores de carga do Azure que utiliza para os clusters de ativação pós-falha do SAP Central Services, bem como de camada do DBMS, tem de ser o [Balanceador de carga de Standard SKU](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-availability-zones). O Balanceador de carga básico não funcionará em zonas
- A rede virtual do Azure e respetivas sub-redes implementou para alojar o sistema SAP são transferidas entre zonas. **Não é necessário** separar as redes virtuais para cada zona
- Todas as máquinas virtuais a implementar, precisa usar [discos geridos do Azure](https://azure.microsoft.com/services/managed-disks/). Discos não geridos não são suportados para implementações zonais
- Armazenamento Premium do Azure ou [armazenamento Ultra SSD](https://docs.microsoft.com/azure/virtual-machines/windows/disks-ultra-ssd) não estiver sustentando qualquer tipo de replicação de armazenamento em zonas. Como resultado é responsabilidade do aplicativo (DBMS ou SAP Central Services) para replicar dados importantes
- Mesmo é válido para o diretório partilhado /sapmnt, que é um disco partilhado (Windows), uma partilha CIFS (Windows), ou partilhar de NFS (Linux). Tem de utilizar uma tecnologia, que replica o disco partilhado ou partilha entre as zonas. Neste momento, são suportadas as seguintes tecnologias:
    - Para Windows, uma solução de cluster que utiliza o SIOS Datakeeper conforme documentado [uma instância do SAP ASCS/SCS de Cluster num cluster de ativação pós-falha do Windows ao utilizar um disco partilhado de cluster no Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-shared-disk) é suportado nas zonas
    - Para o SUSE Linux, uma partilha NFS criado conforme documentado [elevada disponibilidade para NFS nas VMs do Azure no SUSE Linux Enterprise Server](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs) é suportada
    - No momento, a solução usando serviços de ficheiros de escalamento horizontal do Windows (SOFS), conforme documentado [infraestrutura de preparar o Azure para elevada disponibilidade SAP através de uma partilha de ficheiros e de cluster de ativação pós-falha do Windows para as instâncias do SAP ASCS/SCS](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-infrastructure-wsfc-file-share) **não são suportadas nas zonas**
- A terceiro zona é utilizada para alojar o dispositivo SBD no caso de criar um [cluster de pacemaker do SUSE Linux](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker#create-azure-fence-agent-stonith-device) ou instâncias de aplicações adicionais





## <a name="next-steps"></a>Próximos Passos
Verifique os passos seguintes para implementar em zonas de disponibilidade do Azure:

- [Uma instância do SAP ASCS/SCS de cluster num cluster de ativação pós-falha do Windows ao utilizar um disco partilhado de cluster no Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-shared-disk)
- [Preparar a infraestrutura do Azure para elevada disponibilidade SAP através de uma partilha de ficheiros e de cluster de ativação pós-falha do Windows para as instâncias do SAP ASCS/SCS](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-infrastructure-wsfc-file-share)






