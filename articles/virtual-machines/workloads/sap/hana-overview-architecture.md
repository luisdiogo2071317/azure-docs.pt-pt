---
title: Descrição geral e arquitetura de SAP HANA no Azure (instâncias de grande) | Microsoft Docs
description: Descrição geral da arquitetura de como implementar o SAP HANA no Azure (instâncias de grande).
services: virtual-machines-linux
documentationcenter: ''
author: RicksterCDN
manager: timlt
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 01/02/2018
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e3342f3057917202d81359a27accf47ba288b128
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/12/2018
---
# <a name="sap-hana-large-instances-overview-and-architecture-on-azure"></a>Descrição geral de SAP HANA (instâncias de grandes dimensões) e arquitetura no Azure

## <a name="what-is-sap-hana-on-azure-large-instances"></a>O que é o SAP HANA no Azure (instâncias de grande)?

SAP HANA no Azure (instâncias de grande) é uma solução exclusiva para o Azure. Além de fornecer as máquinas virtuais para implementar e executar SAP HANA, Azure oferece a possibilidade da executar e implementar SAP HANA nos servidores de bare-metal dedicadas para si. O SAP HANA na solução do Azure (instâncias de grande) baseia-se em hardware bare-metal não partilhado/servidor de anfitrião que está atribuída. O hardware do servidor está incorporado no carimbos de data / maiores que contêm/servidor de computação, rede e infraestrutura de armazenamento. Como uma combinação é HANA adaptado center integração de dados (TDI) certificada. SAP HANA no Azure (instâncias de grande) oferece SKUs de servidor diferente ou tamanhos. Unidades podem ter 72 CPUs e 768 GB de memória e avance cópias de segurança para unidades que tenham 960 CPUs e 20 TB de memória.

O isolamento de cliente no carimbo de infraestrutura é efetuado na inquilinos, que se pareça com:

- **Rede**: isolamento de clientes na pilha de infraestrutura através de redes virtuais por inquilino atribuído do cliente. Um inquilino é atribuído a um único cliente. Um cliente pode ter vários inquilinos. O isolamento de rede de inquilinos proíbe a comunicação de rede entre os inquilinos no nível de carimbo de data / da infraestrutura, mesmo que os inquilinos pertencerem ao mesmo cliente.
- **Componentes de armazenamento**: isolamento através da máquinas de virtuais de armazenamento que tenham volumes de armazenamento atribuídas. Volumes de armazenamento podem ser atribuídos a um armazenamento máquina apenas. Uma máquina virtual de armazenamento é atribuída exclusivamente a um inquilino único na pilha de infraestrutura de certificados de SAP HANA TDI. Como resultado, os volumes de armazenamento atribuídas a uma máquina virtual de armazenamento podem ser acedidas num específico e relacionado inquilino apenas. Não estão visíveis entre os diferentes inquilinos implementados.
- **Anfitrião ou servidor**: uma unidade de anfitrião do servidor ou não é partilhada entre os clientes ou inquilinos. Um servidor ou o anfitrião implementada para um cliente, é uma unidade de computação do bare-metal atómica que está atribuída a um único inquilino. *Não* é utilizada a criação de partições de hardware ou software de criação de partições que poderá resultar numa partilha de um anfitrião ou um servidor com o outro cliente. Armazenamento que estão atribuídas à máquina virtual de armazenamento do inquilino específico estão montados para esse servidor. Um inquilino pode ter um muitas unidades de servidor de SKUs diferentes atribuídos exclusivamente.
- Dentro de um SAP HANA no carimbo de infraestrutura do Azure (instâncias de grande), muitos inquilinos diferentes são implementados e isolados contra entre si através dos conceitos de inquilino na rede, armazenamento e computação nível. 


Estas unidades bare-metal server são suportadas para executar apenas o SAP HANA. O SAP aplicação camada ou carga de trabalho meio-ware maligno é executado em máquinas virtuais. Os carimbos de infraestrutura que executam o SAP HANA nas unidades do Azure (instâncias de grande) estão ligados as backbones de serviços de rede do Azure. Desta forma, a conectividade de latência baixa entre máquinas virtuais de SAP HANA nas unidades do Azure (instâncias de grandes dimensões) e é fornecida.

Este documento é um dos vários documentos que abrangem SAP HANA no Azure (instâncias de grande). Este documento apresenta a arquitetura básica, responsabilidades e serviços fornecidos pela solução. Capacidades de alto nível da solução também são debatidas. Para a maioria das outras áreas, tais como redes e a conectividade, quatro outros documentos abrangem detalhes e informações de desagregação. A documentação do SAP HANA no Azure (instâncias de grande) não abrange aspetos da instalação do SAP NetWeaver ou implementações de SAP NetWeaver nas VMs. SAP NetWeaver no Azure é abordada documentos separados encontrados no mesmo contentor de documentação do Azure. 


Os documentos diferentes de orientação de instância grande HANA incluem as seguintes áreas:

- [Descrição geral de SAP HANA (instâncias de grandes dimensões) e arquitetura no Azure](hana-overview-architecture.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Infraestrutura de SAP HANA (instâncias de grandes dimensões) e a conectividade no Azure](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Instalar e configurar o SAP HANA (instâncias de grande) no Azure](hana-installation.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [SAP HANA (instâncias de grande) elevada disponibilidade e recuperação após desastre no Azure](hana-overview-high-availability-disaster-recovery.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Resolução de problemas de SAP HANA (instâncias de grandes dimensões) e monitorização no Azure](troubleshooting-monitoring.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Elevada disponibilidade que configurar no SUSE utilizando o STONITH](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/ha-setup-with-stonith)
- [Cópia de segurança do SO e restauro para o tipo II SKUs](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/os-backup-type-ii-skus)

## <a name="definitions"></a>Definições

Várias definições comuns em grande escala são utilizadas na arquitetura e o guia de implementação técnica. Tenha em atenção os seguintes termos e os respetivos significados:

- **IaaS**: infraestrutura como serviço.
- **PaaS**: plataforma como serviço.
- **SaaS**: Software como um serviço.
- **Componente SAP**: uma aplicação de SAP individuais, tal como o componente Central ERP (ECC), do armazém de negócio (BW), Gestor de solução ou Enterprise Portal (EP). Componentes SAP podem ser baseados em tecnologias tradicionais ABAP ou Java ou uma aplicação não baseada em NetWeaver como objetos de negócio.
- **Ambiente de SAP**: um ou mais componentes do SAP logicamente agrupados para efetuar uma função de negócio, como o desenvolvimento, garantia da qualidade, formação, recuperação após desastre ou de produção.
- **SAP horizontal**: refere-se aos ativos SAP completos no seu horizontal IT. O horizontal SAP inclui todos os ambientes de não produção e produção.
- **Sistema SAP**: A combinação de camada DBMS e a camada de aplicação de, por exemplo, um sistema de desenvolvimento do SAP ERP, um sistema de teste de SAP BW e um sistema de produção do SAP CRM. Implementações do Azure não suportam dividindo estas duas camadas entre no local e o Azure. Um sistema SAP é implementado no local ou de implementado no Azure. Pode implementar os sistemas diferentes de um horizontal SAP para o Azure ou no local. Por exemplo, pode implementar o desenvolvimento de SAP CRM e sistemas de teste no Azure enquanto implementa o SAP CRM produção sistema no local. Para SAP HANA no Azure (instâncias de grande), destina-se que alojar a camada de aplicação de SAP dos sistemas SAP em VMs e a instância de SAP HANA relacionada numa unidade no SAP HANA no carimbo do Azure (instâncias de grande).
- **Carimbo de instância grande**: uma pilha de infraestrutura de hardware que está certificada de SAP HANA TDI e dedicados para executar instâncias de SAP HANA no Azure.
- **SAP HANA no Azure (instâncias de grande):** oficial nome para a oferta no Azure executar HANA instâncias no hardware de SAP HANA TDI certificado implementada no carimbos de data / instância grande em diferentes regiões do Azure. O termo relacionado *instância grande HANA* é curto para *SAP HANA no Azure (instâncias de grande)* e é amplamente utilizado neste guia de implementação técnica.
- **Em vários locais**: Descreve um cenário onde as VMs implementadas para uma subscrição do Azure com o site para site, multilocal ou Azure ExpressRoute conectividade entre centros de dados no local e o Azure. Documentação do Azure em comum, estes tipos de implementações também estão descritos como cenários de vários locais. O motivo para a ligação está a expandir domínios no local no local do Azure Active Directory/OpenLDAP, DNS e no local no Azure. O horizontal no local é expandido para os recursos do Azure das subscrições do Azure. Com esta extensão, as VMs podem ser parte do domínio no local. 

   Utilizadores de domínio do domínio no local podem aceder aos servidores e executar os serviços nessas VMs (por exemplo, serviços DBMS). Resolução do nome e a comunicação entre as VMs implementadas no local e as VMs implementadas no Azure, é possível. Este cenário é típico de forma no qual a maioria dos recursos de SAP são implementados. Para obter mais informações, consulte [planear e estruturar para Gateway de VPN do Azure](../../../vpn-gateway/vpn-gateway-plan-design.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) e [criar uma rede virtual com uma ligação site a site utilizando o portal do Azure](../../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
- **Inquilino**: um cliente implementado no stamp instância grande HANA obtém isolado para um *inquilino.* Um inquilino esteja isolado na rede, armazenamento e camada de computação de outros inquilinos. Unidades de armazenamento e computação atribuídas aos inquilinos diferentes não é possível ver entre si ou comunicam entre si no nível de carimbo de data / instância grande HANA. Um cliente pode optar por implementações em diferentes inquilinos. Mesmo assim, não há nenhuma comunicação entre inquilinos no nível de carimbo de data / instância grande HANA.
- **Categoria SKU**: para HANA grande instância, as seguintes duas categorias de SKUs-lhe oferecidas:
    - **Tipo de classe posso**: S72, S72m, S144, S144m, S192 e S192m
    - **Tipo de classe II**: S384, S384m, S384xm, S576m, S768m e S960m


Uma variedade de recursos adicionais estão disponíveis como implementar uma carga de trabalho do SAP na nuvem. Se pretender executar uma implementação de SAP HANA no Azure, tem de ser experiente com e em consideração os princípios de IaaS do Azure e a implementação de cargas de trabalho SAP no IaaS do Azure. Antes de continuar, consulte [soluções de utilização SAP em máquinas virtuais do Azure](get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) para obter mais informações. 

## <a name="certification"></a>Certificação

Para além de certificação NetWeaver, SAP requer uma certificação especial para SAP HANA suportar o SAP HANA em determinados infraestruturas, como o IaaS do Azure.

É o núcleo nota SAP NetWeaver e a certificação de SAP HANA um grau, [1928533 de n. º de nota SAP – aplicações SAP no Azure: suportados produtos e os tipos de VM do Azure](https://launchpad.support.sap.com/#/notes/1928533).

O [2316233 de n. º de nota SAP - SAP HANA no Microsoft Azure (instâncias de grande)](https://launchpad.support.sap.com/#/notes/2316233/E) também é significativa. Aborda a solução descrita neste guia. Além disso, são suportados para executar o SAP HANA no tipo GS5 VM do Azure. Informações para este cenário são publicadas no [o Web site SAP](http://global.sap.com/community/ebook/2014-09-02-hana-hardware/enEN/iaas.html).

O SAP HANA na solução do Azure (instâncias de grande) referida na SAP nota #2316233 fornece os clientes Microsoft e SAP a capacidade de implementar grandes SAP Business Suite, SAP BW, S/4 HANA, BW/4HANA ou outras cargas de trabalho de SAP HANA no Azure. A solução baseia-se no carimbo SAP HANA certificadas hardware dedicado ([SAP HANA adaptados a integração do Centro de dados – TDI](https://scn.sap.com/docs/DOC-63140)). Se executar uma solução configurada de SAP HANA TDI, todas as aplicações baseadas em SAP HANA (como SAP Business Suite no SAP HANA, SAP BW em SAP HANA, S4/HANA e BW4/HANA) funciona na infraestrutura de hardware.

Em comparação com o SAP HANA nas VMs, esta solução tem uma vantagem. Proporciona uma maior quantidade maiores volumes de memória. Para ativar esta solução, tem de compreender os seguintes aspetos fundamentais:

- As aplicações de camada e não SAP da aplicação de SAP executam em VMs que estão alojadas no carimbos habitual de hardware do Azure.
- Cliente infraestrutura no local, centros de dados, e as implementações de aplicações estão ligadas a plataforma de nuvem através do ExpressRoute (recomendado) ou uma rede privada virtual (VPN). O Active Directory e DNS também é expandido no Azure.
- A instância de base de dados SAP HANA para carga de trabalho HANA é executada em SAP HANA no Azure (instâncias de grande). O carimbo de instância grande está ligado em funcionamento em rede do Azure, para que o software em execução em VMs pode interagir com a instância HANA em execução numa instância grande HANA.
- Hardware de SAP HANA no Azure (instâncias de grande) é fornecido num IaaS com SUSE Linux Enterprise Server ou Red Hat Enterprise Linux pré-instalado de hardware dedicado. Tal como acontece com as máquinas virtuais, ainda mais as atualizações e manutenção para o sistema operativo é da responsabilidade do cliente.
- Instalação de HANA nem quaisquer componentes adicionais necessários para executar o SAP HANA em unidades de instância grande HANA é da responsabilidade do cliente. Todas as respetivas operações em curso e a administração de SAP HANA no Azure também são da responsabilidade do cliente.
- Para além das soluções descritas aqui, pode instalar outros componentes na sua subscrição do Azure que liga ao SAP HANA no Azure (instâncias de grande). Os exemplos são os componentes que permitem a comunicação com nem diretamente em SAP HANA da base de dados, tais como servidores de ir, servidores RDP, SAP HANA Studio, serviços de dados SAP para cenários de BI do SAP, ou soluções de monitorização de rede.
- Como no Azure, a instância grande HANA oferece suporte para elevada disponibilidade e a funcionalidade de recuperação de desastre.

## <a name="architecture"></a>Arquitetura

Um nível elevado, o SAP HANA na solução do Azure (instâncias de grande) tem a camada de aplicação de SAP residentes nas VMs. A camada de base de dados reside no hardware configurado para SAP TDI localizada no carimbo instância grande na mesma região do Azure que está ligada ao IaaS do Azure.

> [!NOTE]
> Implemente a camada de aplicação de SAP na mesma região do Azure como a camada de SAP DBMS. Esta regra está bem documento nas informações publicadas sobre cargas de trabalho do SAP no Azure. 

A arquitetura geral de SAP HANA no Azure (instâncias de grande) fornece uma configuração de hardware certificado SAP TDI, o que é um não virtualizados, bare metal, o servidor de elevado desempenho para a base de dados SAP HANA. Também fornece a capacidade e a flexibilidade do Azure para recursos de escala para a camada da aplicação SAP para satisfazer as suas necessidades.

![Descrição geral da arquitetura de SAP HANA no Azure (instâncias de grandes)](./media/hana-overview-architecture/image1-architecture.png)

A arquitetura mostrada está dividida nas três secções:

- **Direito**: mostra uma infraestrutura no local que executa aplicações diferentes nos dados centros de modo a que os utilizadores finais podem aceder a aplicações de LOB, como o SAP. Idealmente, isto no local infraestrutura está ligada, em seguida, para o Azure com [ExpressRoute](https://azure.microsoft.com/services/expressroute/).

- **Center**: mostra IaaS do Azure e, neste caso, a utilização de VMs para alojar o SAP ou outras aplicações que utilizam o SAP HANA como um sistema DBMS. As instâncias HANA mais pequenas que funcionarem com a memória que fornecem VMs são implementadas em VMs, juntamente com a respetiva camada de aplicação. Para obter mais informações sobre máquinas virtuais, consulte [máquinas virtuais](https://azure.microsoft.com/services/virtual-machines/).

   Serviços de rede do Azure são utilizados para agrupar sistemas SAP, juntamente com outras aplicações em redes virtuais. Estas redes virtuais ligam a sistemas no local, bem como SAP HANA no Azure (instâncias de grande).

   Para aplicações SAP NetWeaver e bases de dados que são suportados para executar no Azure, consulte [1928533 de n. º de nota de suporte de SAP – aplicações SAP no Azure: suportados produtos e os tipos de VM do Azure](https://launchpad.support.sap.com/#/notes/1928533). Para obter documentação sobre como implementar soluções SAP no Azure, consulte:

  -  [Utilizar o SAP em máquinas virtuais do Windows](../../virtual-machines-windows-sap-get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
  -  [Utilizar soluções SAP em máquinas virtuais do Azure](get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

- **À esquerda**: mostra o hardware de certificados de SAP HANA TDI o carimbo de instância de grande de Azure. As unidades de instância grande HANA estão ligadas para as redes virtuais da sua subscrição ao utilizar a mesma tecnologia que a conectividade no local no Azure.

O carimbo de instância de grande de Azure próprio combina os componentes seguintes:

- **Computação**: servidores baseados em Intel Xeon E7-8890v3 ou Intel Xeon E7-8890v4 processadores que fornecem a capacidade informática necessária e que são certificadas do SAP HANA.
- **Rede**: A unified recursos de infraestrutura de rede de alta velocidade interconnects computação, armazenamento e componentes de rede local.
- **Armazenamento**: uma infraestrutura de armazenamento que é acedida através de uma infraestrutura de rede unificada. A capacidade de armazenamento específico que é fornecida depende de SAP HANA específicas sobre a configuração do Azure (instâncias de grande) que é implementada. Mais capacidade de armazenamento está disponível um custo mensal adicionais.

Dentro da infraestrutura de multi-inquilino do carimbo instância grande, os clientes são implementados como inquilinos isolados. Na implementação do inquilino, nome de uma subscrição do Azure dentro da sua inscrição do Azure. Esta subscrição do Azure é que a instância de grande HANA é faturada contra. Estes inquilinos tem uma relação de 1:1 à subscrição do Azure. Para uma rede, é possível aceder a uma unidade de instância grande HANA implementada num inquilino numa região do Azure de várias redes virtuais que pertencem a diferentes subscrições do Azure. Essas subscrições do Azure têm de pertencer à mesma inscrição do Azure. 

Tal como acontece com as VMs, SAP HANA no Azure (instâncias de grande) é fornecido em várias regiões do Azure. Para oferecer funcionalidades de recuperação de desastre, pode optar por participar no. Carimbos de instância grande diferentes dentro de uma região georreplicação afiliações ligados entre si. Por exemplo, HANA grande instância carimbos dos EUA Oeste como nos EUA Leste estão ligados através de uma ligação de rede dedicada para replicação de recuperação após desastre. 

Tal como pode escolher entre diferentes tipos VM com máquinas virtuais do Azure, pode escolher entre diferentes SKUs de HANA grande Instância adaptados para tipos diferentes de carga de trabalho de SAP HANA. SAP aplica-se rácios de memória-para--socket de processador para diferentes cargas de trabalho com base no gerações de processadores Intel. A tabela seguinte mostra os tipos SKU oferecidos.

A partir de Julho de 2017, SAP HANA no Azure (instâncias de grande) está disponível em várias configurações em regiões do Azure dos EUA oeste e dos EUA leste, leste da Austrália, Sudeste da Austrália, Europa Ocidental e Europa do Norte.

| Solução SAP | CPU | Memória | Armazenamento | Disponibilidade |
| --- | --- | --- | --- | --- |
| Otimizado para OLAP: SAP BW, BW/4HANA<br /> ou SAP HANA para carga de trabalho OLAP genérica | SAP HANA no S72 do Azure<br /> – 2x processador Intel Xeon de®® E7 8890 v3<br /> 36 núcleos de CPU e 72 threads de CPU |  768 GB |  3 TB | Disponível |
| --- | SAP HANA no S144 do Azure<br /> – 4 x processador Intel Xeon de®® E7 8890 v3<br /> 72 núcleos de CPU e 144 threads de CPU |  1,5 TB |  6 TB | Não oferecido já |
| --- | SAP HANA no S192 do Azure<br /> – 4 x processador Intel Xeon de®® E7 8890 v4<br /> 96 núcleos de CPU e 192 threads de CPU |  2.0 TB |  8 TB | Disponível |
| --- | SAP HANA no S384 do Azure<br /> – 8 x processador Intel Xeon de®® E7 8890 v4<br /> 192 núcleos de CPU e 384 threads de CPU |  4.0 TB |  16 TB | Disponível |
| Otimizado para OLTP: SAP Business Suite<br /> SAP HANA ou S/4HANA (OLTP)<br /> OLTP genérico | SAP HANA no S72m do Azure<br /> – 2x processador Intel Xeon de®® E7 8890 v3<br /> 36 núcleos de CPU e 72 threads de CPU |  1,5 TB |  6 TB | Disponível |
|---| SAP HANA no S144m do Azure<br /> – 4 x processador Intel Xeon de®® E7 8890 v3<br /> 72 núcleos de CPU e 144 threads de CPU |  3.0 TB |  12 TB | Não oferecido já |
|---| SAP HANA no S192m do Azure<br /> – 4 x processador Intel Xeon de®® E7 8890 v4<br /> 96 núcleos de CPU e 192 threads de CPU  |  4.0 TB |  16 TB | Disponível |
|---| SAP HANA no S384m do Azure<br /> – 8 x processador Intel Xeon de®® E7 8890 v4<br /> 192 núcleos de CPU e 384 threads de CPU |  6.0 TB |  18 TB | Disponível |
|---| SAP HANA no S384xm do Azure<br /> – 8 x processador Intel Xeon de®® E7 8890 v4<br /> 192 núcleos de CPU e 384 threads de CPU |  8.0 TB |  22 TB |  Disponível |
|---| SAP HANA no S576m do Azure<br /> – 12 x processador Intel Xeon de®® E7 8890 v4<br /> 288 núcleos de CPU e 576 threads de CPU |  12.0 TB |  28 TB | Disponível |
|---| SAP HANA no S768m do Azure<br /> – 16 x processador Intel Xeon de®® E7 8890 v4<br /> 384 núcleos de CPU e 768 threads de CPU |  16.0 TB |  36 TB | Disponível |
|---| SAP HANA no S960m do Azure<br /> – 20 x processador Intel Xeon de®® E7 8890 v4<br /> 480 núcleos de CPU e 960 threads de CPU |  20.0 TB |  46 TB | Disponível |

- Núcleos de CPU = soma de núcleos de CPU não-com hyper-threading da soma de processadores da unidade do servidor.
- Threads de CPU = soma de threads de computação fornecidos pelo núcleos de CPU com hyper-threading da soma de processadores da unidade do servidor. Todas as unidades estão configuradas por predefinição para utilizar a tecnologia Hyper-Threading.


As configurações específicas escolhidas são dependentes de carga de trabalho, recursos de CPU e memória pretendida. É possível que a carga de trabalho OLTP a utilizar os SKUs que estão otimizados para a carga de trabalho do OLAP. 

O base para todas as ofertas de hardware são certificadas para SAP HANA TDI. Duas classes diferentes de hardware dividem os SKUs para:

- S72, S72m, S144, S144m, S192 e S192m, que são referidas como o "tipo de classe posso" de SKUs.
- S384, S384m, S384xm, S576m, S768m e S960m, que são referidas como o "Tipo de classe II" de SKUs.

Um carimbo de instância grande HANA concluído exclusivamente não está atribuído a um único cliente&#39;s utilização. Este facto aplica-se a bastidores dos recursos de computação e armazenamento ligados através de uma infraestrutura de rede implementada no Azure, bem como. Infraestrutura de instância grande HANA, como o Azure, implementa o cliente diferentes &quot;inquilinos&quot; que estão isoladas umas nas seguintes três níveis:

- **Rede**: isolamento através de redes virtuais no carimbo de instância grande HANA.
- **Armazenamento**: isolamento através da máquinas de virtuais de armazenamento que tenham volumes de armazenamento atribuídos e a isolar os volumes de armazenamento entre inquilinos.
- **Computação**: dedicado a atribuição de unidades de servidor para um único inquilino. Não rígido ou soft a criação de partições de unidades do servidor. Nenhuma partilha de uma única unidade de anfitrião do servidor ou entre inquilinos. 

As implementações de unidades de instância grande HANA entre diferentes inquilinos não estão visíveis para si. Unidades de instância grande HANA implementadas em diferentes inquilinos não consegue comunicar diretamente entre si no nível de carimbo de data / instância grande HANA. Apenas as unidades de instância grande HANA dentro de um inquilino podem comunicar entre si no nível de carimbo de data / instância grande HANA.

Um inquilino implementado no carimbo de instância grande é atribuído a uma subscrição do Azure para fins de faturação. Para uma rede, pode ser acedido a partir de redes virtuais das outras subscrições do Azure dentro da mesma inscrição do Azure. Se implementar com outra subscrição do Azure na mesma região do Azure, também pode optar por voltar a pedir um inquilino de instância grande HANA separado.

Existem diferenças significativas que executam o SAP HANA na instância grande HANA e SAP HANA em execução em VMs implementadas no Azure:

- Não há nenhuma camada de virtualização para SAP HANA no Azure (instâncias de grande). Obter o desempenho do hardware bare-metal subjacente.
- Ao contrário do Azure, o SAP HANA no servidor do Azure (instâncias de grande) está dedicado a um cliente específico. Não há nenhum possibilidade que uma unidade do servidor ou o anfitrião está rígido ou soft particionado. Como resultado, uma unidade de instância grande HANA é utilizada como como um todo para um inquilino e com que lhe atribuído. Um reinício ou encerramento do servidor não originou automaticamente o sistema operativo e a implementar noutro servidor do SAP HANA. (Para o tipo de classe posso SKUs, a única exceção é se um servidor detetar problemas e reimplementação tem de ser efetuada noutro servidor.)
- Ao contrário do Azure, onde os tipos de processadores do anfitrião estão selecionados para o rácio de preço/desempenho melhor, os tipos de processador escolhidos para SAP HANA no Azure (instâncias de grande) são efetuar mais elevado da linha de processador Intel E7v3 e E7v4.


### <a name="run-multiple-sap-hana-instances-on-one-hana-large-instance-unit"></a>Executar várias instâncias de SAP HANA numa unidade de instância grande HANA
É possível mais do que uma instância de SAP HANA ativa em unidades de instância grande HANA do anfitrião. Para fornecer capacidades de instantâneos de armazenamento e de recuperação de desastres, tal configuração de requer um volume definido por instância. Atualmente, as unidades de instância grande HANA podem subdivididas da seguinte forma:

- **S72, S72m, S144, S192**: em incrementos de 256 GB, com 256 GB a unidade de menor inicial. É possível combinar incrementos diferentes, tais como 256 GB e 512 GB para o número máximo de memória da unidade.
- **S144m e S192m**: em incrementos de 256 GB, com GB 512 a unidade mais pequena. Incrementos diferentes, tais como 512 GB e 768 GB podem ser combinados para o número máximo de memória da unidade.
- **Tipo de classe II**: em incrementos de 512 GB, com a unidade inicial menor do que 2 TB. É possível combinar incrementos diferentes, tais como 512 GB, 1 TB e 1,5 TB para o número máximo de memória da unidade.

Alguns exemplos de executar várias instâncias de SAP HANA poderão ter o seguinte aspeto.

| SKU | Tamanho da memória | Tamanho de armazenamento | Tamanhos com várias bases de dados |
| --- | --- | --- | --- |
| S72 | 768 GB | 3 TB | instância HANA 1 x 768 GB<br /> ou instância de 1 x 512 GB + 1 x 256 GB instância<br /> ou instâncias de 3 x 256 GB | 
| S72m | 1,5 TB | 6 TB | instâncias HANA 3x512GB<br />ou instância de 1 x 512 GB + instância 1 1 TB<br />ou instâncias de 6 x 256 GB<br />ou instância de 1x1.5 TB | 
| S192m | 4 TB | 16 TB | instâncias de 8 x 512 GB<br />ou instâncias de 4 1 TB<br />ou instâncias de 4 x 512 GB + instâncias de 2 1 TB<br />ou instâncias de 4 x 768 GB + instâncias de 2 x 512 GB<br />a instância de 1 4 TB ou |
| S384xm | 8 TB | 22 TB | instâncias de 4 x 2 TB<br />ou instâncias de 2 4 TB<br />ou instâncias de 2, 3 TB + instâncias 1 x 2 TB<br />ou instâncias de 2x2.5 TB + instâncias 1 x 3 TB<br />a instância de 1 x 8 TB ou |


Existem também outras variações. 

### <a name="use-sap-hana-data-tiering-and-extension-nodes"></a>Utilizar SAP HANA nós de criação de camadas e de extensão de dados
SAP suporta um modelo de camadas de dados para SAP BW de diferentes versões do SAP NetWeaver e SAP BW/4HANA. Para obter mais informações sobre o modelo de camadas de dados, consulte o documento SAP [SAP BW/4HANA e SAP BW no HANA connosco de extensão de SAP HANA](https://www.sap.com/documents/2017/05/ac051285-bc7c-0010-82c7-eda71af511fa.html#).
Com a instância de grande HANA, pode utilizar a configuração de opção-1 de nós de extensão de SAP HANA conforme explicado nos documentos de blogue FAQ e SAP. Opção 2 configurações podem ser configuradas com as seguintes instâncias SKUs HANA grande: S72m, S192, S192m, S384 e S384m. 

Quando observar a documentação, a vantagem poderá não estar visível imediatamente. Mas, quando observar as diretrizes de dimensionamento de SAP, pode ver uma vantagem utilizando a opção-1 e 2 opção nós de extensão de SAP HANA. Seguem-se exemplos:

- Diretrizes de dimensionamento de SAP HANA normalmente necessitam de duplo a quantidade de volume de dados como memória. Quando executar a instância de SAP HANA com os dados, tiver apenas 50 por cento ou menor de memória preenchida com dados. O resto da memória ideal é mantido para SAP HANA fazer o seu trabalho.
- Esse significa numa unidade de HANA grande instância S192 com 2 TB de memória, com uma base de dados SAP BW, só tem de 1 TB como volume de dados.
- Se utilizar um nó adicional para a extensão de SAP HANA de opção-1, também um S192 HANA grande instância SKU, proporciona uma capacidade de 2 TB adicional para o volume de dados. Na configuração de opção-2, que obtém um 4 TB adicional para o volume de dados transfira. Em comparação com o nó de acesso frequente, a capacidade da memória total do nó de extensão "transfira" pode ser utilizada para armazenar dados para a opção-1. Volte a memória pode ser utilizada para o volume de dados na configuração de nó da extensão de SAP HANA de opção-2.
- Acaba por ficar com uma capacidade de 3 TB de dados e um rácio de acesso frequente-para-transfira de 2:1 para a opção-1. Tem de 5 TB de dados e um rácio de 1:4 com a configuração de nó de extensão de opção-2.

Quanto maior for o volume de dados em comparação comparado a memória, quanto maior for as possibilidades são que os dados transfira que estão a pedir para são armazenados no armazenamento de disco.


## <a name="operations-model-and-responsibilities"></a>Modelo de operações e responsabilidades

O serviço fornecido com o SAP HANA no Azure (instâncias de grande) está alinhado com os serviços do IaaS do Azure. Obter uma instância de uma instância de grande HANA com um sistema operativo instalado que está otimizado para SAP HANA. Como com as VMs de IaaS do Azure, a maioria das tarefas de proteger o sistema operativo, instalar o software adicional, instalar HANA, o SO e HANA a funcionar e atualizar o SO e HANA é da responsabilidade do cliente. Microsoft não forçar atualizações do SO ou atualizações HANA no utilizador.

![Responsabilidades de SAP HANA no Azure (instâncias de grandes)](./media/hana-overview-architecture/image2-responsibilities.png)

Como é mostrado no diagrama, SAP HANA no Azure (instâncias de grande) é um multi-inquilino oferecem IaaS. A maioria das vezes, a divisão de responsabilidade é o limite de infraestrutura de SO. Microsoft é responsável por todos os aspetos do serviço abaixo da linha do sistema operativo. É responsável por todos os aspetos do serviço acima da linha. O SO é da responsabilidade do cliente. Pode continuar a utilizar mais atual no local métodos que poderá utilizar para a gestão de compatibilidade, segurança, gestão de aplicações, base e SO. Os sistemas de aparecem como se estão na sua rede no que diz respeito todos os.

Este serviço está otimizado para SAP HANA, pelo que existem áreas em que terá de trabalhar com a Microsoft a utilizar as capacidades de infraestrutura subjacente para obter os melhores resultados.

A lista seguinte fornece mais detalhes em cada uma das suas responsabilidades e as camadas:

**Rede**: todas as redes internas para o carimbo de grande instância em execução SAP HANA. Da responsabilidade do cliente inclui o acesso ao armazenamento, a conectividade entre o instâncias (para Escalamento horizontal e outras funções), a conectividade para o horizontal e a conectividade para o Azure onde está alojada a camada da aplicação de SAP nas VMs. Também inclui conectividade WAN entre centros de dados do Azure para replicação de objetivos de recuperação após desastre. Todas as redes são divididos em partições pelo inquilino e têm de qualidade de serviço aplicada.

**Armazenamento**: O virtualizado particionada armazenamento para todos os volumes necessários para os servidores de SAP HANA, bem como para instantâneos. 

**Servidores**: os servidores físicos dedicados para executar as bds de HANA SAP atribuído aos inquilinos. Os servidores do tipo posso classe de SKUs são abstracted de hardware. Com estes tipos de servidores, a configuração do servidor é recolhida e mantida em perfis, que podem ser movidas um hardware físico para outro hardware físico. Este tipo uma (manual) mudança de um perfil pelo operations pode ser comparado com um pouco autorrecuperação do serviço do Azure. Os servidores de SKUs de classe II de tipo não oferecem esse uma capacidade.

**SDDC**: O software de gestão que é utilizado para gerir dados centra-se como entidades definidas por software. -Permite à Microsoft para recursos de conjunto de dimensionamento, disponibilidade e motivos de desempenho.

**/ S**: O SO que escolher (SUSE Linux ou Red Hat Linux) que está em execução nos servidores. As imagens de SO que são fornecidos com foram fornecidas pelo fabricante do Linux individuais para a Microsoft para a execução de SAP HANA. Tem de ter uma subscrição com o fornecedor do Linux para a imagem específica com otimização de SAP HANA. É responsável por registar as imagens com o fornecedor do SO. 

A partir do ponto de handover pela Microsoft, é responsável por quaisquer ainda mais a aplicação de patches do sistema operativo Linux. Esta aplicação de patches inclui pacotes adicionais que poderá ser necessário para uma instalação de SAP HANA com êxito e que não foram incluídos pelo fabricante do Linux específico no respetivo SAP HANA com otimização de imagens do SO. (Para obter mais informações, consulte a do SAP HANA instalação documentação e notas SAP.) 

É responsável pela aplicação de patches de SO owing to avaria ou de otimização do SO e os controladores relativo para o hardware de servidor específico. Também é responsável pela segurança ou a aplicação de patches funcional do SO. 

Da responsabilidade do cliente também inclui a monitorização e planeamento da capacidade de:

- Consumo de recursos de CPU.
- Consumo de memória.
- Volumes de disco relacionados com espaço livre, o IOPS e latência.
- Tráfego de volume de rede entre a instância de grande HANA e a camada de aplicação SAP.

A infraestrutura subjacente da instância de grande HANA fornece a funcionalidade cópia de segurança e restauro do volume de SO. Utilizar esta funcionalidade também é da responsabilidade do cliente.

**Middleware**: instância de SAP HANA, principalmente. Administração, operações e monitorização são da responsabilidade do cliente. Pode utilizar a funcionalidade fornecida para utilizar instantâneos de armazenamento para fins de recuperação de cópia de segurança e restauro e após desastre. Estas funcionalidades são fornecidas pela infraestrutura. As suas responsabilidades também incluem a conceber a elevada disponibilidade ou recuperação de desastre com estas capacidades, monitorização e tirar partido dos mesmos para determinar se os instantâneos de armazenamento são executados com êxito.

**Dados**: os dados geridos pelo SAP HANA e outros dados, como cópias de segurança de partilhas de ficheiro ou ficheiros localizados em volumes. As suas responsabilidades incluem a monitorização de espaço livre em disco e gerir o conteúdo nos volumes. Também é responsável pela monitorização a execução bem-sucedida de cópias de segurança de volumes de disco e instantâneos de armazenamento. Execução da replicação de dados para sites de recuperação de desastre com êxito é da responsabilidade do Microsoft.

**Aplicações:** as instâncias de aplicações SAP ou, no caso de aplicações não SAP, a camada de aplicação dessas aplicações. As suas responsabilidades incluem a implementação, administração, operações e monitorização dessas aplicações. É responsável por planeamento da capacidade do consumo de recursos de CPU, o consumo de memória, o consumo de armazenamento do Azure e o consumo de largura de banda de rede nas redes virtuais. Também é responsável pela capacidade de planeamento para consumo de recursos de redes virtuais para SAP HANA no Azure (instâncias de grande).

**WANs**: ligações estabelecer no local para implementações do Azure para cargas de trabalho. Todos os clientes com instância grande HANA utilizam Azure ExpressRoute para conectividade. Esta ligação não faz parte do SAP HANA na solução do Azure (instâncias de grande). É responsável pela configuração desta ligação.

**Arquivo**: poderá preferir arquivar cópias dos dados utilizando os suas próprias métodos em contas de armazenamento. Arquivar necessita de operações, os custos, conformidade e gestão. É responsável pela geração cópias de arquivo e as cópias de segurança do Azure e armazene-os de forma em conformidade.

Consulte o [SLA para SAP HANA no Azure (instâncias de grande)](https://azure.microsoft.com/support/legal/sla/sap-hana-large/v1_0/).

## <a name="sizing"></a>Dimensionamento

Dimensionamento para a instância de grande HANA é não diferem Dimensionar para HANA em geral. Para existente e implementados sistemas que pretende mover de outro RDBMS para HANA, SAP fornece um conjunto de relatórios que são executadas em sistemas de SAP existentes. Se a base de dados é movido para HANA, estes relatórios Verifique os dados e calcular os requisitos de memória para a instância HANA. Para obter mais informações sobre como executar estes relatórios e obter os patches ou versões mais recentes, leia as notas de SAP seguintes:

- [A nota #1793345 - Dimensionar para SAP Suite no HANA](https://launchpad.support.sap.com/#/notes/1793345)
- [SAP nota #1872170 - Suite HANA e S/4 HANA relatório de dimensionamento](https://launchpad.support.sap.com/#/notes/1872170)
- [A nota #2121330 - FAQ: SAP BW no HANA relatório de dimensionamento](https://launchpad.support.sap.com/#/notes/2121330)
- [Tenha em atenção #1736976 - relatório de dimensionamento para BW no HANA de SAP](https://launchpad.support.sap.com/#/notes/1736976)
- [SAP nota #2296290 - Dimensionar novo relatório para BW no HANA](https://launchpad.support.sap.com/#/notes/2296290)

Para implementações de campo verde, Sizer rápida SAP está disponível para calcular os requisitos de memória da implementação do software SAP por cima HANA.

Requisitos de memória para HANA aumentam à medida que o crescimentos de volume de dados. Tenha em atenção o consumo de memória atual para o ajudar a prever que se passa a estar no futuro. Com base nos requisitos de memória, em seguida, pode mapear o pedido dos SKUs de instância grande HANA.

## <a name="requirements"></a>Requisitos

Esta lista monta requisitos para executar o SAP HANA no Azure (instâncias superior).

**Microsoft Azure**

- Uma subscrição do Azure que pode ser associada a SAP HANA no Azure (instâncias de grande).
- Contrato do suporte do Microsoft Premier. Para obter informações específicas relacionadas com o SAP no Azure, consulte [2015553 de n. º de nota de suporte de SAP – SAP no Microsoft Azure: suportar pré-requisitos](https://launchpad.support.sap.com/#/notes/2015553). Se utilizar unidades de instância grande HANA com 384 e mais CPUs, também tem de alargar a Premier contrato do suporte para incluir resposta rápida do Azure.
- Deteção dos SKUs de instância grande HANA necessário depois de efetuar um exercício de dimensionamento com o SAP.

**Conectividade de rede**

- ExpressRoute no local para o Azure: ligar o seu centro de dados no local ao Azure, certifique-se ordenar, pelo menos, uma ligação de 1 Gbps a partir do seu ISP. 

**Sistema operativo**

- Licenças do SUSE Linux Enterprise Server 12 para aplicações SAP.

   > [!NOTE] 
   > O sistema operativo disponibilizado pela Microsoft não está registado com SUSE. Não está ligado uma instância da ferramenta de gestão de subscrição.

- SUSE Linux subscrição ferramenta de gestão implementado no Azure numa VM. Esta ferramenta fornece a capacidade para SAP HANA no Azure (instâncias de grande) para ser registado e respetivamente atualizado pelo SUSE. (Sem acesso à internet no Centro de dados de instância grande HANA há.) 
- Licenças do Red Hat Enterprise Linux 6.7 ou 7.2 para SAP HANA.

   > [!NOTE]
   > O sistema operativo disponibilizado pela Microsoft não está registado com Red Hat. Não está ligado uma instância do Gestor de subscrição do Red Hat.

- Red Hat subscrição Manager implementado no Azure numa VM. O Gestor de subscrição do Red Hat fornece a capacidade para SAP HANA no Azure (instâncias de grande) para ser registado e respetivamente atualizado pelo Red Hat. (Não há nenhum acesso direto à internet de dentro do inquilino implementado no carimbo instância grande de Azure).
- SAP requer que tenha um suporte de contrato, bem como o seu fornecedor de Linux. Este requisito não é removido pela solução de instância grande HANA ou o facto de que executam o Linux no Azure. Ao contrário com algumas das imagens de galeria do Azure do Linux, a taxa de serviço é *não* incluídos a oferta de soluções de instância grande HANA. É da responsabilidade do cliente para satisfazer os requisitos de SAP relativos contratos de suporte com o distribuidor de Linux. 
   - Procurar para SUSE Linux, os requisitos de contratos de suporte no [1984787 de n. º de nota SAP - SUSE Linux Enterprise Server 12: notas instalação](https://launchpad.support.sap.com/#/notes/1984787) e [1056161 de n. º de nota SAP - suporte de prioridade SUSE para aplicações SAP](https://launchpad.support.sap.com/#/notes/1056161).
   - Para Red Hat Linux, tem de ter os níveis de subscrição correta que incluem suporte e atualizações para os sistemas operativos de instância grande HANA de serviço. Red Hat recomenda Red Hat Enterprise Linux [SAP soluções] (https://access.redhat.com/solutions/3082481 subscrição. 

Para a matriz de suporte de diferentes versões de SAP HANA com as diferentes versões de Linux, consulte [SAP nota #2235581](https://launchpad.support.sap.com/#/notes/2235581).

Para a matriz de compatibilidade do sistema operativo e versões de firmware/controlador HLI, consulte [atualização do SO para HLI](os-upgrade-hana-large-instance.md).


**Base de Dados**

- As licenças e componentes de instalação de software para SAP HANA (plataforma ou enterprise edition).

**Aplicações**

- As licenças e componentes de instalação de software para quaisquer aplicações SAP que se ligam ao SAP HANA e SAP relacionado suportam contratos.
- Licenças e componentes de instalação de software para quaisquer aplicações SAP não utilizada relativamente SAP HANA em ambientes do Azure (instâncias de grandes dimensões) e relacionadas com contratos de suporte.

**Competências**

- Experiência com e conhecimentos do IaaS do Azure e os respetivos componentes.
- Experiência com e conhecimentos sobre como implementar uma carga de trabalho do SAP no Azure.
- Instalação de SAP HANA certificadas técnico.
- SAP competências arquiteto de sistemas para estruturar a elevada disponibilidade e recuperação após desastre em torno de SAP HANA.

**SAP**

- Expectativas são de que está a um cliente SAP e ter um suporte de contrato com o SAP.
- Especialmente para implementações da classe II de tipo de instância SKUs de grande HANA, consulte com SAP em versões do SAP HANA e as configurações eventual no hardware de dimensionamento de grande dimensão.


## <a name="storage"></a>Armazenamento

O esquema de armazenamento para SAP HANA no Azure (instâncias de grande) está configurado por SAP HANA no modelo de implementação clássica através do SAP recomendado diretrizes. As diretrizes estão documentadas no [requisitos de armazenamento de SAP HANA](http://go.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html) documento técnico.

A instância de grande HANA do tipo de classe posso inclui quatro vezes o volume de memória como volume de armazenamento. Para a classe de II de tipo de unidades de instância grande HANA, o armazenamento não está a mais de quatro vezes. As unidades vêm com um volume que foi concebido para armazenar cópias de segurança de registo de transação de HANA. Para obter mais informações, consulte [instalar e configurar o SAP HANA (instâncias de grande) no Azure](hana-installation.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Consulte a tabela seguinte em termos de alocação de armazenamento. A tabela indica a capacidade aproximada para os volumes diferentes fornecido com as diferentes unidades de instância grande HANA.

| HANA grande instância SKU | Hana/dados | Hana/registo | Hana/partilhado | Hana / / cópia de segurança |
| --- | --- | --- | --- | --- |
| S72 | 1,280 GB | 512 GB | 768 GB | 512 GB |
| S72m | 3,328 GB | 768 GB |1,280 GB | 768 GB |
| S192 | 4,608 GB | 1.024 GB | 1,536 GB | 1.024 GB |
| S192m | 11,520 GB | 1,536 GB | 1,792 GB | 1,536 GB |
| S384 | 11,520 GB | 1,536 GB | 1,792 GB | 1,536 GB |
| S384m | 12.000 GB | 2,050 GB | 2,050 GB | 2,040 GB |
| S384xm | 16,000 GB | 2,050 GB | 2,050 GB | 2,040 GB |
| S576m | 20.000 GB | 3,100 GB | 2,050 GB | 3,100 GB |
| S768m | 28,000 GB | 3,100 GB | 2,050 GB | 3,100 GB |
| S960m | 36,000 GB | 4,100 GB | 2,050 GB | 4,100 GB |


Volumes implementados reais poderão variar com base na implementação e a ferramenta que é utilizada para mostrar os tamanhos de volume.

Se subdividir um SKU de instância grande HANA, alguns exemplos de peças de divisão possíveis poderão ter o seguinte aspeto:

| Partição de memória em GB | Hana/dados | Hana/registo | Hana/partilhado | Hana / / cópia de segurança |
| --- | --- | --- | --- | --- |
| 256 | 400 GB | 160 GB | 304 GB | 160 GB |
| 512 | 768 GB | 384 GB | 512 GB | 384 GB |
| 768 | 1,280 GB | 512 GB | 768 GB | 512 GB |
| 1,024 | 1,792 GB | 640 GB | 1.024 GB | 640 GB |
| 1,536 | 3,328 GB | 768 GB | 1,280 GB | 768 GB |


Estes são números de volume aproximada que podem diferir ligeiramente com base na implementação e as ferramentas utilizadas para observar os volumes. Existem também outros tamanhos de partição, tais como 2,5 TB. Estes tamanhos de armazenamento são calculados com uma fórmula semelhante às utilizadas para as partições anteriores. O termo "partições" não significa que o sistema operativo, memória ou recursos de CPU são de qualquer forma particionados. Indica as partições de armazenamento para as diferentes instâncias HANA que pode querer implementar numa única unidade de instância grande HANA. 

Poderá ser necessário mais armazenamento. Pode adicionar armazenamento, a aquisição de armazenamento adicional em unidades de 1 TB. Este tipo de armazenamento adicional pode ser adicionado como volume adicional. Também pode ser utilizado para expandir um ou mais dos volumes existentes. Não é possível diminuir os tamanhos dos volumes que originalmente implementadas e principalmente documentado tabelas anteriores. É também não é possível alterar os nomes dos volumes ou os nomes de montagem. Os volumes de armazenamento descritos anteriormente estão anexados às unidades instância grande HANA como NFS4 volumes.

Pode utilizar instantâneos de armazenamento para fins de recuperação de cópia de segurança e restauro e após desastre. Para obter mais informações, consulte [SAP HANA (instâncias de grande) elevada disponibilidade e recuperação após desastre no Azure](hana-overview-high-availability-disaster-recovery.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

### <a name="encryption-of-data-at-rest"></a>Encriptação de dados inativos
O armazenamento utilizado para a instância de grande HANA permite que a encriptação transparente de dados que são armazenadas nos discos. Quando uma unidade de instância grande HANA é implementada, pode ativar este tipo de encriptação. Também pode alterar para volumes encriptados após a implementação ocorre. A movimentação de não encriptados para volumes encriptados é transparente e não necessita de período de indisponibilidade. 

Com o tipo de classe posso de SKUs, o volume de arranque LUN é armazenado no é encriptado. Para a classe de tipo II de SKUs de HANA grande instância, terá de encriptar o arranque LUN com métodos de SO. Para obter mais informações, contacte a equipa de gestão de serviço da Microsoft.


## <a name="networking"></a>Redes

A arquitetura dos serviços de rede do Azure é um componente fundamental da implementação de aplicações de SAP HANA instância de grandes dimensões com êxito. Normalmente, SAP HANA em implementações do Azure (instâncias de grande) tem uma maior horizontal SAP com várias soluções SAP diferentes com diferentes tamanhos de bases de dados, o consumo de recursos de CPU e utilização da memória. É provável que nem todos os sistemas esses SAP baseiam-se em SAP HANA. O horizontal SAP provavelmente é uma versão híbrida que utiliza:

- Implementação de SAP sistemas no local. Devido ao respetivos tamanhos, estes sistemas atualmente não é possível alojados no Azure. Um exemplo é uma sistema de SAP ERP que é executada no SQL Server (como a base de dados) e necessita de mais recursos de CPU ou memória que podem fornecer as VMs de produção.
- Implementada com base em SAP HANA SAP sistemas no local.
- Sistemas SAP implementados nas VMs. Estes sistemas podem ser o desenvolvimento, teste, sandbox, ou instâncias de produção para qualquer uma das aplicações com base em SAP NetWeaver que podem implementar com êxito no Azure (em VMs), com base no pedido de memória e o consumo de recursos. Estes sistemas também podem ser com base nas bases de dados como o SQL Server. Para obter mais informações, consulte [1928533 de n. º de nota de suporte de SAP – aplicações SAP no Azure: suportados produtos e os tipos de VM do Azure](https://launchpad.support.sap.com/#/notes/1928533/E). E estes sistemas podem ser baseados em bases de dados, tais como o SAP HANA. Para obter mais informações, consulte [SAP HANA certificadas IaaS plataformas](http://global.sap.com/community/ebook/2014-09-02-hana-hardware/enEN/iaas.html).
- Implementar servidores de aplicações SAP no Azure (em VMs) que tiram partido de SAP HANA no Azure (instâncias de grande) no carimbos de data / instância grande de Azure.

Uma versão híbrida horizontal SAP com quatro ou mais diferentes cenários de implementação é normal. Também existem muitos cenários de cliente de landscapes SAP completas, que são executados no Azure. Como VMs fiquem mais poderosas, aumenta o número de clientes que mova todas as soluções de SAP no Azure.

Funcionamento em rede no contexto de sistemas SAP implementado no Azure do Azure não está mais complicado. Baseia-se nos seguintes princípios:

- Redes virtuais do Azure tem de estar ligados ao circuito ExpressRoute que se liga a uma rede no local.
- Um circuito de ExpressRoute que estabelece ligação no local para o Azure, normalmente, deve ter uma largura de banda de 1 Gbps ou superior. Esta largura de banda mínima permite largura de banda suficiente para a transferência de dados entre sistemas no local e que são executadas em VMs. Também permite largura de banda suficiente para a ligação ao Azure sistemas dos utilizadores no local.
- Todos os sistemas SAP no Azure tem de ser configurados em redes virtuais para comunicar entre si.
- Active Directory e DNS alojadas no local são expandidas no Azure através do ExpressRoute no local.


> [!NOTE] 
> Do ponto de vista da faturação, apenas uma subscrição do Azure pode ser ligada a apenas um inquilino num carimbo de instância grande numa região do Azure específico. Por outro lado, um único inquilino de carimbo de grande instância pode ser ligado a apenas uma subscrição do Azure. Este requisito é consistente com outros objetos sujeito a faturação no Azure.

Se o SAP HANA no Azure (instâncias de grande) for implementado em várias regiões do Azure diferentes, um inquilino individual é implementado no carimbo de instância de grandes dimensões. Pode executar ambas na mesma subscrição do Azure, desde que estas instâncias fazem parte do mesmo horizontal SAP. 

> [!IMPORTANT] 
> Apenas a implementação do Azure Resource Manager é suportada com SAP HANA no Azure (instâncias de grande).

 

### <a name="additional-virtual-network-information"></a>Informações de rede virtual adicionais

Para ligar uma rede virtual para o ExpressRoute, tem de ser criado um gateway do Azure. Para obter mais informações, consulte [sobre gateways de rede virtual para o ExpressRoute](../../../expressroute/expressroute-about-virtual-network-gateways.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). 

Um gateway do Azure pode ser utilizado com o ExpressRoute para uma infraestrutura fora do Azure ou para um carimbo de instância de grande de Azure. Um gateway do Azure também pode ser utilizado para estabelecer ligação entre redes virtuais. Para obter mais informações, consulte [configurar uma ligação de rede de rede para o Resource Manager utilizando o PowerShell](../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Pode ligar o gateway do Azure para um máximo de quatro ligações ExpressRoute diferentes, desde que essas ligações provenientes de diferentes routers de limite de enterprise da Microsoft. Para obter mais informações, consulte [infraestrutura de SAP HANA (instâncias de grandes dimensões) e a conectividade no Azure](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

> [!NOTE] 
> O débito que fornece um gateway do Azure é diferente para ambos os casos de utilização. Para obter mais informações, consulte [sobre o Gateway de VPN](../../../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). O débito máximo que pode realizar com um gateway de rede virtual é 10 Gbps, utilizando uma ligação ExpressRoute. Copiar ficheiros entre uma VM que reside numa rede virtual e um sistema local (como uma sequência de cópia única) não conseguir do débito total dos SKUs de gateway diferentes. Para tirar partido de largura de banda completa de gateway de rede virtual, utilize vários fluxos. Ou, tem de copiar ficheiros diferentes em fluxos paralelos de um único ficheiro.


### <a name="networking-architecture-for-hana-large-instance"></a>Arquitetura de rede para a instância de grande HANA
A arquitetura de rede para a instância de grande HANA pode ser separada em quatro partes distintas:

- No local de rede e a ligação do ExpressRoute para o Azure. Esta parte é o domínio do cliente e está ligada ao Azure através do ExpressRoute. Veja a parte inferior direita na figura seguinte.
- Serviços de rede do Azure, conforme discutidos anteriormente, com redes virtuais, novamente com gateways. Esta peça é uma área onde tem de localizar as estruturas adequadas para os seus requisitos de conformidade, os requisitos de aplicações e segurança. Se utilizar a instância de grande HANA é outro ponto da ter em consideração em termos do número de redes virtuais e SKUs de gateway do Azure à sua escolha. Consulte o canto superior direito na imagem.
- Conectividade da instância de grande HANA através de tecnologia de ExpressRoute para o Azure. Esta peça é implementada e processada pela Microsoft. Tudo o que precisa de fazer é fornecer alguns intervalos de endereços IP após a implementação dos seus ativos na instância grande HANA ligarem o circuito de ExpressRoute para as redes virtuais. Para obter mais informações, consulte [infraestrutura de SAP HANA (instâncias de grandes dimensões) e a conectividade no Azure](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 
- Funcionamento em rede na instância grande HANA, que é principalmente transparente para si.

![Rede virtual ligada ao SAP HANA no Azure (instâncias de grandes dimensões) e no local](./media/hana-overview-architecture/image3-on-premises-infrastructure.png)

O requisito de que os recursos no local tem de estabelecer ligação através do ExpressRoute para o Azure não irá alterar porque utilizar instância grande HANA. O requisito de ter uma ou várias redes virtuais que executam as VMs que alojam a camada da aplicação que liga as instâncias HANA alojadas em unidades de instância grande HANA, também não irá alterar. 

As diferenças das implementações de SAP no Azure são:

- As unidades de instância grande HANA do seu inquilino do cliente estão ligadas através de outro circuito do ExpressRoute para as redes virtuais. Para separar as condições de carga, o local para ligações do ExpressRoute de redes virtuais e as ligações entre redes virtuais e a instância de grande HANA não partilhe os mesmos routers.
- O perfil de carga de trabalho entre a camada de aplicação SAP e a instância de grande HANA é de natureza diferentes, com o número de pedidos pequeno e bursts, tal como os dados são transferidos (conjuntos de resultados) de SAP HANA na camada da aplicação.
- A arquitetura de aplicações SAP é mais sensível à latência de rede do que cenários típicos onde os dados são trocados entre no local e o Azure.
- O gateway de rede virtual tem, pelo menos, duas ligações do ExpressRoute. Ambas as ligações partilham a largura de banda máxima para dados recebidos do gateway de rede virtual.

A latência de rede teve entre VMs e instâncias de grande HANA unidades podem ser superiores a uma típica latência reportadas round-trip de rede de VM para VM. Depende a região do Azure, os valores de medida podem exceder a latência de reportadas round-trip 0,7 ms classificada como abaixo média [1100926 de n. º de nota SAP - FAQ: desempenho de rede](https://launchpad.support.sap.com/#/notes/1100926/E). Contudo, os clientes implementar aplicações de SAP de produção com base em SAP HANA com êxito na instância de grandes dimensões do SAP HANA. Os clientes que implementado melhoramentos excelente relatório ao executar as aplicações SAP no SAP HANA através da utilização de unidades de instância grande HANA. Certifique-se de que testa os processos empresariais exaustivamente Azure HANA grande instância.
 
Para fornecer a latência de rede determinista entre VMs e instâncias de grande HANA, a escolha do gateway de rede virtual SKU é essencial. Ao contrário de padrões de tráfego entre VMs e no local, o padrão de tráfego entre VMs e instâncias de grande HANA pode desenvolver bursts pequenos mas elevados de volumes de dados e pedidos a ser transmitidos. Para processar esses bursts bem, recomendamos vivamente a utilização do UltraPerformance gateway SKU. Para a classe de II de tipo de instância SKUs de grande HANA, a utilização do SKU de gateway UltraPerformance como um gateway de rede virtual é obrigatória.

> [!IMPORTANT] 
> Tendo em conta o tráfego de rede global entre camadas de base de dados de aplicação de SAP e, apenas o HighPerformance ou UltraPerformance SKUs de gateway para redes virtuais são suportados para ligar ao SAP HANA no Azure (instâncias de grande). Para HANA grande instância tipo II SKUs, apenas o UltraPerformance SKU de gateway é suportado como um gateway de rede virtual.



### <a name="single-sap-system"></a>Sistema SAP única

A infraestrutura no local, apresentada anteriormente está ligada através do ExpressRoute para o Azure. O circuito de ExpressRoute liga-se para um router de limite de enterprise. Para obter mais informações, consulte [descrição geral técnica do ExpressRoute](../../../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Depois da rota é estabelecida, para estabelecer a ligação para a estrutura principal do Azure e todas as regiões do Azure estão acessíveis.

> [!NOTE] 
> Para executar SAP landscapes no Azure, ligue para o router de limite de enterprise mais próximo da região do Azure na horizontal SAP. Carimbos de grande instância do Azure estão ligados através de dispositivos de router de limite de enterprise dedicado para minimizar a latência de rede entre as VMs do IaaS do Azure e a instância grande carimbos de data /.

O gateway de rede virtual para as VMs que alojem instâncias de aplicações SAP está ligado ao circuito ExpressRoute. A mesma rede virtual está ligada a um router de limite de enterprise separado dedicado para ligar a carimbos de data / instância grandes.

Este sistema é um exemplo simples de um único sistema SAP. Camada da aplicação de SAP está alojada no Azure. A base de dados SAP HANA é executada em SAP HANA no Azure (instâncias de grande). Pressuposto é que a largura de banda de gateway de rede virtual de débito de 2 Gbps ou de 10 Gbps não representa um estrangulamento.

### <a name="multiple-sap-systems-or-large-sap-systems"></a>Vários sistemas SAP ou grandes sistemas SAP

Se vários SAP sistemas ou grandes SAP sistemas são implementados para ligar ao SAP HANA no Azure (instâncias de grande), o débito do gateway de rede virtual pode tornar-se um engarrafamento. Nesse caso, divida as camadas da aplicação em várias redes virtuais. Também pode criar uma rede virtual especial que estabelece ligação à instância grande HANA para casos, tal como:

- Efetuar cópias de segurança diretamente a partir de instâncias HANA na instância grande HANA para uma VM no Azure que aloja partilhas NFS.
- Copiar grande cópias de segurança ou outros ficheiros de unidades de instância grande HANA para espaço de disco gerido no Azure.

Utilize uma rede virtual separada para as VMs que gerir o armazenamento do anfitrião. Esta disposição evita que os efeitos de ficheiros grandes ou transferência de dados de instância grande HANA no Azure no gateway de rede virtual que serve as VMs que executam a camada da aplicação de SAP. 

Para uma arquitetura de rede mais escalável:

- Tire partido de várias redes virtuais para uma camada de aplicação de SAP única e superior.
- Implemente uma rede virtual separada para cada sistema SAP implementado, comparada com a combinar estes sistemas SAP sub-redes separadas sob a mesma rede virtual.

 Uma arquitetura de rede mais dimensionável para SAP HANA no Azure (instâncias de grande):

![Implemente a camada de aplicação de SAP através de várias redes virtuais](./media/hana-overview-architecture/image4-networking-architecture.png)

A ilustração mostra a camada de aplicação do SAP ou componentes, implementados ao longo de várias redes virtuais. Esta configuração introduzida overhead de latência de obrigatório que ocorreram durante a comunicação entre as aplicações alojadas nessas redes virtuais. Por predefinição, o tráfego de rede entre as VMs localizadas na rota de redes virtuais diferentes através os routers de limite de enterprise nesta configuração. Desde Setembro de 2016, este encaminhamento pode ser otimizado. 

Peering redes virtuais na mesma região é a forma de otimizar e diminuir a latência de comunicação entre duas redes virtuais. Este método funciona mesmo que essas redes virtuais estão em subscrições diferentes. Com o peering de rede virtual, a comunicação entre VMs em duas redes virtuais diferentes pode utilizar o principal de rede do Azure para comunicar diretamente entre si. Latência mostra como se as VMs estão na mesma rede virtual. O tráfego que aborda os intervalos de endereços IP que estão ligados através do gateway de rede virtual do Azure é encaminhado através do gateway de rede virtual individual da rede virtual. 

Para obter mais informações sobre peering de rede virtual, consulte [peering de rede Virtual](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview).


### <a name="routing-in-azure"></a>Encaminhamento no Azure

Três considerações de encaminhamento de rede são importantes para SAP HANA no Azure (instâncias de grande):

* SAP HANA no Azure (instâncias de grande) podem ser acedido apenas através de VMs e a ligação do ExpressRoute dedicada, não diretamente a partir do local. Acesso direto no local para as unidades de instância grande HANA, conforme fornecidos pela Microsoft, não é possível imediatamente. As restrições de encaminhamento transitivas são devido à arquitetura de rede do Azure atual utilizada para a instância de grandes dimensões do SAP HANA. Alguns clientes de administração e todas as aplicações que precisam de direcionar o acesso, tais como o SAP solução Manager em execução no local, não é possível ligar à base de dados SAP HANA.

* Se tiver de unidades de instância grande HANA implementadas em duas regiões do Azure diferentes para a recuperação após desastre, aplicam-se as restrições de encaminhamento transitórias. Por outras palavras, os endereços IP de uma unidade de instância grande HANA uma região (por exemplo, EUA oeste) não são encaminhados para uma unidade de instância grande HANA implementada noutra região (por exemplo, EUA Leste). Esta restrição é independente de utilização de rede do Azure peering entre regiões ou em vários locais-ligar os circuitos ExpressRoute que se ligam unidades de instância grande HANA às redes virtuais. Para uma representação de gráfico, consulte a figura na secção "Utilização HANA grande instância unidades por várias regiões." Esta restrição, que é fornecido com a arquitetura de implementada, proíbe a utilização imediata de replicação do sistema HANA como a funcionalidade de recuperação após desastre.

* SAP HANA nas unidades do Azure (instâncias de grande) tem um endereço IP atribuído partir servidor conjunto intervalo de endereços IP que é submetido. Para obter mais informações, consulte [infraestrutura de SAP HANA (instâncias de grandes dimensões) e a conectividade no Azure](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Este endereço IP é acessível através de subscrições do Azure e ExpressRoute que liga as redes virtuais para HANA no Azure (instâncias de grande). O endereço IP atribuído fora de que o intervalo de endereços do conjunto IP do servidor diretamente é atribuído à unidade de hardware. Tem *não* atribuído através de NAT, tal como foi as maiúsculas e minúsculas nas implementações primeiro desta solução. 

> [!NOTE] 
> Para ultrapassar a restrição no encaminhamento transitório conforme explicado nos itens de lista de dois primeiro, utilize os componentes adicionais para o encaminhamento. Os componentes que podem ser utilizados para ultrapassar a restrição podem ser:

> * Um proxy inverso para encaminhar os dados, de e para. Por exemplo, F5 BIG-IP, NGINX com o Gestor de tráfego seja implementado no Azure como uma solução de encaminhamento de tráfego/firewall virtual.
> * Utilizar [IPTables regras](http://www.linuxhomenetworking.com/wiki/index.php/Quick_HOWTO_%3a_Ch14_%3a_Linux_Firewalls_Using_iptables#.Wkv6tI3rtaQ) numa VM com Linux para ativar o encaminhamento entre localizações no local e unidades de instância grande HANA ou entre unidades de instância grande HANA em regiões diferentes.

> Lembre-se de que a implementação e suporte para soluções personalizadas que envolvem terceiros aparelhos de rede ou IPTables não foi fornecido pela Microsoft. Suporte tem de ser fornecido pelo fabricante do componente utilizado ou a integrador de. 

### <a name="internet-connectivity-of-hana-large-instance"></a>Acesso à Internet da instância de grande HANA
Instância de grande HANA *não* tem conectividade internet direta. Por exemplo, esta limitação pode restringir a capacidade de registar a imagem do SO diretamente com o fornecedor do SO. Poderá ter de trabalhar com o servidor de ferramenta de gestão de subscrição do SUSE Linux Enterprise Server local ou com o Gestor de subscrição do Red Hat Enterprise Linux.

### <a name="data-encryption-between-vms-and-hana-large-instance"></a>Encriptação de dados entre as VMs e instâncias de grande HANA
Dados transferidos entre VMs e instâncias de grande HANA não estão encriptados. No entanto, puramente para a troca entre o lado HANA DBMS e as aplicações baseadas em JDBC/ODBC, pode ativar a encriptação de tráfego. Para obter mais informações, consulte [esta documentação por SAP](http://help-legacy.sap.com/saphelp_hanaplatform/helpdata/en/db/d3d887bb571014bf05ca887f897b99/content.htm?frameset=/en/dd/a2ae94bb571014a48fc3b22f8e919e/frameset.htm&current_toc=/en/de/ec02ebbb57101483bdf3194c301d2e/plain.htm&node_id=20&show_children=false).

### <a name="use-hana-large-instance-units-in-multiple-regions"></a>Utilizar unidades de instância grande HANA em várias regiões

Poderá ter razões para implementar o SAP HANA no Azure (instâncias de grande) em várias regiões do Azure diferente para a recuperação de desastre. Talvez pretende aceder à instância de grande HANA de cada uma das VMs implementadas em redes virtuais diferentes nas regiões. Os endereços IP atribuídos às unidades HANA instância de grandes dimensões diferentes não serão propagados para além das redes virtuais que estão ligadas diretamente através do seu gateway as instâncias. Como resultado, uma alteração ligeiras é apresentada para o design de rede virtual. Um gateway de rede virtual pode processar quatro circuitos do ExpressRoute diferentes fora routers de limite de enterprise diferentes. Cada rede virtual que está ligada a um dos carimbos de instância de grandes dimensões pode ser ligada ao carimbo instância grande noutra região do Azure.

![Rede virtual ligado a carimbos de instância de grande de Azure em diferentes regiões do Azure](./media/hana-overview-architecture/image8-multiple-regions.png)

A ilustração mostra como as redes virtuais diferentes em ambas as regiões estão ligadas à dois circuitos do ExpressRoute diferentes que são utilizados para ligar ao SAP HANA no Azure (instâncias de grande) em ambas as regiões do Azure. As ligações recentemente introduzidas são as linhas vermelhos retangular. Com estas ligações, sem as redes virtuais, as VMs em execução dessas redes virtuais podem aceder a cada uma das unidades de instância grande HANA diferentes implementadas em duas regiões. Como mostra a figura, pressupõe que tem duas ligações ExpressRoute no local para as duas regiões do Azure. Esta disposição é recomendada por motivos de recuperação após desastre.

> [!IMPORTANT] 
> Se utilizar vários circuitos ExpressRoute, prefixação como caminho e as definições de BGP de preferência Local devem ser utilizadas para garantir o correto de encaminhamento de tráfego.


