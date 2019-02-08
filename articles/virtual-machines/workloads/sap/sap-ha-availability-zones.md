---
title: Configurações de carga de trabalho SAP com zonas de disponibilidade do Azure | Documentos da Microsoft
description: Arquitetura de elevada disponibilidade e cenários para utilizar zonas de disponibilidade do Azure do SAP NetWeaver
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: msjuergent
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
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e319fb5bc1e0ed3354a09b685405656c7f3131bc
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/07/2019
ms.locfileid: "55858810"
---
# <a name="sap-workload-configurations-with-azure-availability-zones"></a>Configurações de carga de trabalho SAP com zonas de disponibilidade do Azure

Um dos recursos de alta disponibilidade é o Azure oferece [zonas de disponibilidade do Azure](https://docs.microsoft.com/azure/availability-zones/az-overview). Utilizar zonas de disponibilidade melhora a disponibilidade global da carga de trabalho SAP no Azure. As zonas de disponibilidade estão disponíveis em vários [regiões do Azure](https://azure.microsoft.com/global-infrastructure/regions/) já. Irão seguir a mais regiões. 

A arquitetura básica de elevada disponibilidade do SAP pode ser descrita, tal como apresentado neste gráfico:

![Configuração de HA padrão](./media/sap-ha-availability-zones/standard-ha-config.png)

Camada de aplicação SAP é implementada numa do Azure [conjunto de disponibilidade](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability). Para elevada disponibilidade do SAP Central Services, vai implementar duas VMs num conjunto de disponibilidade separados. Utilize serviços de Cluster de ativação pós-falha do Windows ou Pacemaker (Linux) como uma estrutura de elevada disponibilidade com a ativação pós-falha automática em caso de problema de infraestrutura ou software. Documentos com detalhes sobre implementações deste tipo estão listados aqui:

- [Uma instância do SAP ASCS/SCS de cluster num cluster de ativação pós-falha do Windows utilizando um disco partilhado de cluster](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-shared-disk)
- [Uma instância do SAP ASCS/SCS de cluster num cluster de ativação pós-falha do Windows ao utilizar a partilha de ficheiros](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-file-share)
- [Elevada disponibilidade para SAP NetWeaver em VMs do Azure no SUSE Linux Enterprise Server para aplicações de SAP](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse)
- [Azure máquinas virtuais elevada disponibilidade para SAP NetWeaver em Red Hat Enterprise Linux](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel)

Uma arquitetura semelhante aplica-se para a camada do DBMS da SAP NetWeaver, S/4HANA ou Hybris sistemas. Implemente a camada do DBMS em modo ativo/passivo com uma solução de cluster de ativação pós-falha para proteção contra a falha de uma infraestrutura ou software. A solução de cluster de ativação pós-falha pode ser qualquer um dos framework de ativação pós-falha específico DBMS, serviços de Cluster de ativação pós-falha do Windows ou Pacemaker. 

Para poder implementar a mesma arquitetura utilizar zonas de disponibilidade do Azure, são necessárias algumas alterações para a arquitetura descrita acima. Estas alterações são descritas neste documento.

## <a name="considerations-deploying-across-availability-zones"></a>Considerações de implantação em zonas de disponibilidade

Ao utilizar as zonas de disponibilidade, existem alguns aspetos a considerar:

- As zonas de disponibilidade do Azure não garantem a determinados distância entre as zonas diferentes dentro de uma região do Azure.
- As zonas de disponibilidade do Azure não são uma solução de DR ideal. Desastres naturais podem causar ampla distribuídos danos em algumas regiões do mundo, incluindo danos pesado à infra-estrutura de energia. A distância entre zonas diferentes pode não ser suficientemente grande para ser qualificado como uma solução adequada de DR.
- A latência de rede em zonas de disponibilidade varia em diferentes regiões do Azure. Há casos, onde pode implementar e executar a camada de aplicação SAP em zonas diferentes, como a latência de rede de uma zona para a VM DBMS Active Directory é aceitável de um impacto de processo comercial. Mas, em algumas regiões do Azure, a latência entre a VM DBMS Active Directory e a instância da aplicação SAP, implementadas em diferentes zonas, pode ser muito intrusivo e aceitável para os processos de negócios SAP. Como resultado, a arquitetura de implementação tem de ser diferente com uma arquitetura de ativo/ativo para a aplicação ou a arquitetura de ativo/passivo em que a latência de zona cruzada é demasiado elevada.
- Decida que configuração poderia usar zonas de disponibilidade do Azure, com base na latência de rede que medir entre as zonas diferentes. A latência de rede desempenha um papel importante em duas áreas:
    - Latência entre as duas instâncias do DBMS que tem de ter uma replicação síncrona estabelecida. Quanto maior for a latência de rede, maior será o potencial de escalabilidade de impacto da sua carga de trabalho
    - A diferença na latência de rede entre uma VM em execução uma SAP diálogo instância na zona com a instância ativa do DBMS e uma VM semelhante noutra zona. Maior esta diferença, maior será o impacto no tempo de execução de processos de negócios e tarefas de lote, dependentes independentemente de serem executadas numa zona com o DBMS ou numa zona diferente.


Existem algumas restrições, quando implementar VMs do Azure em zonas de disponibilidade e o estabelecimento de soluções de ativação pós-falha na mesma região do Azure. Estas restrições são abordadas na lista seguinte:

- Usando [discos geridos do Azure](https://azure.microsoft.com/services/managed-disks/) é obrigatório para a implantação em zonas de disponibilidade do Azure 
- O mapeamento de enumerações de zona para zonas físicas é corrigido numa base de subscrição do Azure. Se estiver a utilizar para implementar os seus sistemas SAP subscrições diferentes, terá de definir as zonas ideais para cada subscrição.
- Não é possível implementar conjuntos de disponibilidade do Azure dentro de uma zona de disponibilidade do Azure. Escolha uma zona de disponibilidade do Azure ou um conjunto como uma estrutura de implantação para máquinas virtuais de disponibilidade do Azure.
- Não é possível utilizar um [Balanceador de carga básico do Azure](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview#skus) para criar soluções de cluster com base nos serviços de Cluster de ativação pós-falha do Windows ou Linux Pacemaker de ativação pós-falha. Em vez disso, tem de utilizar o [SKU de Balanceador de carga Standard do Azure](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-availability-zones)



## <a name="ideal-availability-zones-combination"></a>Combinação de zonas de disponibilidade ideal
Antes de decidir como tirar partido das zonas de disponibilidade, terá de efetuar uma investigação que dá-lhe:

- A latência de rede entre as três zonas diferentes de uma região do Azure. Então, que pode escolher as zonas com a latência de rede, pelo menos, no cruzada tráfego de rede de zona
- A diferença entre a latência de VM para VM dentro de uma das zonas escolhidas por si e a latência de rede entre duas zonas que escolheu
- Uma instrução se a VM tipos que precisa implantar estão disponíveis nas duas zonas da sua preferência. Especialmente com máquinas de virtuais de série M, vai encontrar situações em que os diferentes SKUs de VM de série M vai estar disponível apenas em dois dos três zonas

### <a name="network-latency-between-zones-and-within-zone"></a>Latência de rede entre zonas e dentro de zona
Para saber o que é a latência entre as zonas diferentes, terá de:

- Implemente o SKU de VM que pretende utilizar para a sua instância do DBMS em todas as três zonas. Certifique-se de que [Azure Accelerated Networking](https://azure.microsoft.com/blog/maximize-your-vm-s-performance-with-accelerated-networking-now-generally-available-for-both-windows-and-linux/) é ativada quando efetuar esta medida.
- Como encontrar duas zonas com a menor latência de rede, implemente outro três VMs do SKU de VM que pretende utilizar como a camada de aplicativo VM entre as três zonas de disponibilidade. Medir a latência de rede em relação a "DBMS VMs' duas em duas zonas 'DBMS' diferentes da sua preferência. 
- Como uma ferramenta para medir, utilize **niping**. Uma ferramenta do SAP, que funciona conforme descrito nas notas de suporte do SAP [#500235](https://launchpad.support.sap.com/#/notes/500235) e [#1100926](https://launchpad.support.sap.com/#/notes/1100926/E). Concentre-se sobre os comandos SAP documentada para medidas de latência. Usando **ping** não é recomendável, uma vez que **ping** não funcionará com o Azure acelerado de caminhos de código de rede.

Com base em medidas e a disponibilidade das suas SKUs de VM em zonas de disponibilidade, terá de tomar as decisões:

- Definir as zonas ideais para a camada DBMS
- Responder a pergunta sobre se pretender distribuir sua camada de aplicativo do Active Directory SAP através de um, dois ou todos os três regiões, com base nas diferenças de latência em-zona de rede vs. em zonas
- Responder à pergunta, se pretender implementar um ativo/passivo ou uma configuração de ativo/ativo de um ponto de vista do aplicativo (ver mais tarde)

Essas decisões, lembre-se também as recomendações de latência de rede do SAP conforme documentado na nota SAP [#1100926](https://launchpad.support.sap.com/#/notes/1100926/E).

### <a name="be-aware-of"></a>Tenha em atenção

> [!IMPORTANT]
> As medidas e as decisões que tomar, são válidos para a subscrição do Azure que utilizou tornar essas medidas. Se utilizar outra subscrição do Azure, terá de repetir as medidas, uma vez que o mapeamento de enumerados zonas pode ser diferentes para outra subscrição do Azure.


> [!IMPORTANT]
> Espera-se que as medidas como efetuada acima mostram resultados diferentes em cada região do Azure que suporta [zonas de disponibilidade](https://docs.microsoft.com/azure/availability-zones/az-overview). Mesmo que os seus requisitos de latência de rede são os mesmos, poderá ter de adotar diferentes estratégias de implantação em diferentes regiões do Azure, uma vez que a latência de rede entre zonas pode ser diferente. É esperado que em algumas regiões do Azure, a latência de rede entre as três zonas diferentes pode ser bastante diferente. Enquanto em outras regiões, a latência de rede entre as três zonas diferentes é mais uniforme. A afirmação de facto **sempre** é uma latência de rede em zonas de 1 milissegundo para 2 milissegundos é **errado**. A latência de rede em zonas de disponibilidade em regiões do Azure não pode ser generalizada.


## <a name="activeactive-deployment"></a>Implementação de ativo/ativo
Esta arquitetura de implementação é chamada ativo/ativo, uma vez que implementar suas instâncias de caixa de diálogo SAP ativas em duas ou três zonas. O SAP Central Services utilizando a replicação de colocar em fila vai ser implementado entre duas zonas. O mesmo é válido para a camada do DBMS, o que vai ser implantado em zonas da mesmo que o serviço de Central de SAP.

Para contemplar esta configuração, terá de encontrar duas zonas de disponibilidade na sua região que oferecem a latência de rede de zona cruzado que é aceitável para a sua carga de trabalho e para a replicação síncrona do DBMS. Além disso pretende que o delta entre a latência de rede dentro de zonas que selecionou e a entre a latência de rede de zona não era demasiado grande. Razão para a última opção, é que não desejar demasiado grandes variações em tempos de execução de seus processos de negócios ou tarefas, dependentes do batch, se uma tarefa é executada na zona com o servidor do DBMS ou através de zona. Algumas variações são aceitáveis, mas não os fatores de diferença.

Um esquema simplificado de uma implementação de ativo/ativo entre duas zonas poderia ter o seguinte aspeto:

![active_active_xone_deployment](./media/sap-ha-availability-zones/active_active_zones_deployment.png)

As seguintes considerações aplicam-se para esta configuração:

- Tratar as zonas de disponibilidade do Azure como domínios de falha e atualização de todas as VMs, uma vez que os conjuntos de disponibilidade não podem ser implementados em zonas de disponibilidade do Azure
- Os balanceadores de carga do Azure que utiliza para os clusters de ativação pós-falha do SAP Central Services e a camada do DBMS, tem de ser o [Balanceador de carga de Standard SKU](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-availability-zones). O Balanceador de carga básico não funcionará em zonas
- A rede virtual do Azure e respetivas sub-redes implementou para alojar o sistema SAP são transferidas entre zonas. **Não é necessário** separar as redes virtuais para cada zona
- Para todas as máquinas virtuais a implementar, utilize [discos geridos do Azure](https://azure.microsoft.com/services/managed-disks/). Discos não geridos não são suportados para implementações zonais
- Armazenamento Premium do Azure ou [armazenamento Ultra SSD](https://docs.microsoft.com/azure/virtual-machines/windows/disks-ultra-ssd) não estiver sustentando qualquer tipo de replicação de armazenamento em zonas. Como resultado é responsabilidade do aplicativo (DBMS ou SAP Central Services) para replicar dados importantes
- Mesmo é válido para o diretório partilhado /sapmnt, que é um disco partilhado (Windows), uma partilha CIFS (Windows), ou partilhar de NFS (Linux). Tem de utilizar uma tecnologia, que replica o disco partilhado ou partilha entre as zonas. Neste momento, são suportadas as seguintes tecnologias:
    - Para Windows, uma solução de cluster que utiliza o SIOS Datakeeper conforme documentado [uma instância do SAP ASCS/SCS de Cluster num cluster de ativação pós-falha do Windows ao utilizar um disco partilhado de cluster no Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-shared-disk) é suportado nas zonas
    - Para o SUSE Linux, uma de partilha NFS que baseia-se conforme documentado [elevada disponibilidade para NFS nas VMs do Azure no SUSE Linux Enterprise Server](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs) é suportada
    - Atualmente, a solução usando serviços de ficheiros de escalamento horizontal do Windows (SOFS), conforme documentado [infraestrutura de preparar o Azure para elevada disponibilidade SAP através de uma partilha de ficheiros e de cluster de ativação pós-falha do Windows para as instâncias do SAP ASCS/SCS](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-infrastructure-wsfc-file-share) **não é suportado nas zonas**
- A terceiro zona é utilizada para alojar o dispositivo SBD no caso de criar um [cluster de pacemaker do SUSE Linux](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker#create-azure-fence-agent-stonith-device) ou instâncias de aplicações adicionais
- Para obter consistência de tempo de execução dos processos empresariais críticos, pode experimentar direcionar determinadas tarefas de lote e os utilizadores para instâncias de aplicações que estão na zona com a instância ativa do DBMS utilizando grupos de servidores de Batch do SAP, grupos de Logon ou grupos de RFC. No entanto, em caso de uma ativação pós-falha zonal terá de mover manualmente estes grupos para instâncias em execução em VMs na zona com a VM de DB Active Directory.  
- Decida se pretende implementar algumas instâncias de caixa de diálogo inativo em cada uma das zonas para poder imediatamente obter a capacidade do recurso antigo, se uma zona utilizada por parte das suas instâncias do aplicativo está fora do serviço


## <a name="activepassive-deployment"></a>Implementação de ativo/passivo
Se não conseguir encontrar um intervalo aceitável entre a latência de rede dentro de um tráfego de rede de zona de zona e para várias, a arquitetura que pode implementar tem um caráter de modo ativo/passivo do ponto de vista do SAP aplicação camada. Definir uma zona 'active', que é a zona em que implementa a camada de aplicativo completo e onde está tentando executar ambos os o DBMS e SAP Central Services instância ativa. Com esta configuração, certifique-se de que não têm variações de tempo de execução extreme em transações empresariais e tarefas de lote, dependentes se uma tarefa for executada na zona com a instância ativa do DBMS ou não.

O layout básico de tal uma arquitetura é semelhante a:

![active_active_xone_deployment](./media/sap-ha-availability-zones/active_active_zones_deployment.png)

As seguintes considerações aplicam-se para esta configuração:

- Conjuntos de disponibilidade não podem ser implementados em zonas de disponibilidade do Azure. Portanto, neste caso, tem um domínio de atualização e com falha para a camada de aplicação. Motivo é que apenas é implementado numa zona. Esta configuração é uma ligeira infeliz, em comparação comparada a arquitetura de referência que foresees que implemente a camada de aplicativo num conjunto de disponibilidade do Azure.
- Operando uma arquitetura desse tipo, precisa monitorizar de perto e tente manter as instâncias de serviços DBMS e SAP Central ativas na mesma zona de camada de aplicação implementado. Em caso de uma ativação pós-falha do serviço de Central de SAP ou a instância do DBMS, pretende certificar-se de que pode manualmente falhar novamente para a zona com a camada de aplicação SAP implementada mais cedo possível
- Os balanceadores de carga do Azure que utiliza para os clusters de ativação pós-falha do SAP Central Services e a camada do DBMS, tem de ser o [Balanceador de carga de Standard SKU](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-availability-zones). O Balanceador de carga básico não funcionará em zonas
- A rede virtual do Azure e respetivas sub-redes implementou para alojar o sistema SAP são transferidas entre zonas. **Não é necessário** separar as redes virtuais para cada zona
- Todas as máquinas virtuais a implementar, precisa usar [discos geridos do Azure](https://azure.microsoft.com/services/managed-disks/). Discos não geridos não são suportados para implementações zonais
- Armazenamento Premium do Azure ou [armazenamento Ultra SSD](https://docs.microsoft.com/azure/virtual-machines/windows/disks-ultra-ssd) não estiver sustentando qualquer tipo de replicação de armazenamento em zonas. Como resultado é responsabilidade do aplicativo (DBMS ou SAP Central Services) para replicar dados importantes
- Mesmo é válido para o diretório partilhado /sapmnt, que é um disco partilhado (Windows), uma partilha CIFS (Windows), ou partilhar de NFS (Linux). Tem de utilizar uma tecnologia, que replica o disco partilhado ou partilha entre as zonas. Neste momento, são suportadas as seguintes tecnologias:
    - Para Windows, uma solução de cluster que utiliza o SIOS Datakeeper conforme documentado [uma instância do SAP ASCS/SCS de Cluster num cluster de ativação pós-falha do Windows ao utilizar um disco partilhado de cluster no Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-shared-disk) é suportado nas zonas
    - Para o SUSE Linux, uma de partilha NFS que baseia-se conforme documentado [elevada disponibilidade para NFS nas VMs do Azure no SUSE Linux Enterprise Server](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs) é suportada
    - Atualmente, a solução usando serviços de ficheiros de escalamento horizontal do Windows (SOFS), conforme documentado [infraestrutura de preparar o Azure para elevada disponibilidade SAP através de uma partilha de ficheiros e de cluster de ativação pós-falha do Windows para as instâncias do SAP ASCS/SCS](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-infrastructure-wsfc-file-share) **não é suportado nas zonas**
- A terceiro zona é utilizada para alojar o dispositivo SBD no caso de criar um [cluster de pacemaker do SUSE Linux](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker#create-azure-fence-agent-stonith-device) ou instâncias de aplicações adicionais
- Implementar em VMs Inativas na zona passiva (do ponto de vista do DBMS) para poder iniciar os recursos de aplicação em caso de falha de zona
    - Não é possível usar [do Azure Site Recovery](https://azure.microsoft.com/services/site-recovery/) para replicar VMs do Active Directory para VMs Inativas entre zonas. No momento, não é capaz de atender a uma função do Azure Site Recovery
- Investir para a automatização permite-lhe, em caso de falha de zona, para iniciar automaticamente a camada de aplicação SAP na segunda zona

## <a name="combined-high-availability-and-disaster-recovery-configuration"></a>Combinados de elevada disponibilidade e a configuração da recuperação após desastre
Microsoft não partilha todas as informações à distância geográfica entre os recursos desse anfitrião diferentes do Azure zonas de disponibilidade na região do Azure. Apesar desse fato, alguns clientes estão a tirar partido zonas para uma configuração de HA e DR combinada que promete um **R**ecovery **P**oint **s**bjective (RPO) igual a zero. O que significa, não deverá perder quaisquer transações consolidadas de base de dados, mesmo no caso de recuperação após desastre. 

> [!NOTE]
> Uma configuração como isso é recomendada para apenas as circunstâncias específicas. Por exemplo, casos em que os dados não podem deixar a região do Azure devido a motivos de segurança e conformidade. 

Um exemplo de como essa configuração pode ter um aspeto como é demonstrado neste gráfico:

![combined_ha_dr_in_zones](./media/sap-ha-availability-zones/combined_ha_dr_in_zones.png)

As seguintes considerações aplicam-se para esta configuração:

- É supondo que existe uma distância significativa entre os recursos que aloja uma zona de disponibilidade. Ou é forçado a manter-se com uma determinada região do Azure. Conjuntos de disponibilidade não podem ser implementados em zonas de disponibilidade do Azure. Portanto, neste caso, é terminar com um domínio de atualização e com falha para a camada de aplicação. Motivo é que apenas é implementado numa zona. Este é um infeliz em comparação comparada a arquitetura de referência que foresees que implemente a camada de aplicativo num conjunto de disponibilidade do Azure.
- Operando uma arquitetura desse tipo, precisa monitorizar de perto e tente manter as instâncias de serviços DBMS e SAP Central ativas na mesma zona de camada de aplicação implementado. Em caso de uma ativação pós-falha do serviço de Central de SAP ou a instância do DBMS, pretende certificar-se de que pode manualmente falhar novamente para a zona com a camada de aplicação SAP implementada mais cedo possível
- Tem instâncias de aplicações de produção pré-instaladas nas VMs que executam as Active Directory instâncias da aplicação de controle de qualidade.
- Em caso de falha zonal, encerrar as instâncias do aplicativo de controle de qualidade e iniciar as instâncias de produção em vez disso. Lembre-se de que precisa para trabalhar com nomes virtual para instâncias da aplicação fazer isso funcionar
- Os balanceadores de carga do Azure que utiliza para os clusters de ativação pós-falha do SAP Central Services e a camada do DBMS, tem de ser o [Balanceador de carga de Standard SKU](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-availability-zones). O Balanceador de carga básico não funcionará em zonas
- A rede virtual do Azure e respetivas sub-redes implementou para alojar o sistema SAP são transferidas entre zonas. **Não é necessário** separar as redes virtuais para cada zona
- Todas as máquinas virtuais a implementar, precisa usar [discos geridos do Azure](https://azure.microsoft.com/services/managed-disks/). Discos não geridos não são suportados para implementações zonais
- Armazenamento Premium do Azure ou [armazenamento Ultra SSD](https://docs.microsoft.com/azure/virtual-machines/windows/disks-ultra-ssd) não estiver sustentando qualquer tipo de replicação de armazenamento em zonas. Como resultado é responsabilidade do aplicativo (DBMS ou SAP Central Services) para replicar dados importantes
- Mesmo é válido para o diretório partilhado /sapmnt, que é um disco partilhado (Windows), uma partilha CIFS (Windows), ou partilhar de NFS (Linux). Tem de utilizar uma tecnologia, que replica o disco partilhado ou partilha entre as zonas. Neste momento, são suportadas as seguintes tecnologias:
    - Para Windows, uma solução de cluster que utiliza o SIOS Datakeeper conforme documentado [uma instância do SAP ASCS/SCS de Cluster num cluster de ativação pós-falha do Windows ao utilizar um disco partilhado de cluster no Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-shared-disk) é suportado nas zonas
    - Para o SUSE Linux, uma de partilha NFS que baseia-se conforme documentado [elevada disponibilidade para NFS nas VMs do Azure no SUSE Linux Enterprise Server](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs) é suportada
    - No momento, a solução usando serviços de ficheiros de escalamento horizontal do Windows (SOFS), conforme documentado [infraestrutura de preparar o Azure para elevada disponibilidade SAP através de uma partilha de ficheiros e de cluster de ativação pós-falha do Windows para as instâncias do SAP ASCS/SCS](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-infrastructure-wsfc-file-share) **não são suportadas nas zonas**
- A terceiro zona é utilizada para alojar o dispositivo SBD no caso de criar um [cluster de pacemaker do SUSE Linux](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker#create-azure-fence-agent-stonith-device) ou instâncias de aplicações adicionais





## <a name="next-steps"></a>Próximos Passos
Verifique os passos seguintes para implementar em zonas de disponibilidade do Azure:

- [Uma instância do SAP ASCS/SCS de cluster num cluster de ativação pós-falha do Windows ao utilizar um disco partilhado de cluster no Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-shared-disk)
- [Preparar a infraestrutura do Azure para elevada disponibilidade SAP através de uma partilha de ficheiros e de cluster de ativação pós-falha do Windows para as instâncias do SAP ASCS/SCS](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-infrastructure-wsfc-file-share)






