---
title: Descrição geral e arquitetura do SAP HANA no Azure (instâncias grandes) | Documentos da Microsoft
description: Descrição geral da arquitetura de como implementar o SAP HANA no Azure (instâncias grandes).
services: virtual-machines-linux
documentationcenter: ''
author: RicksterCDN
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 08/27/2018
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9d28b6ea5612a3db539c51d2603c3f12282ca519
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/27/2018
ms.locfileid: "43090421"
---
# <a name="sap-hana-large-instances-overview-and-architecture-on-azure"></a>Descrição geral do SAP HANA (instâncias grandes) e a arquitetura no Azure

## <a name="what-is-sap-hana-on-azure-large-instances"></a>O que é o SAP HANA no Azure (instâncias grandes)?

SAP HANA no Azure (instâncias grandes) é uma solução única para o Azure. Além de fornecer máquinas virtuais para implementar e executar o SAP HANA, Azure oferece-lhe a possibilidade de executar e implementar o SAP HANA em servidores bare-metal dedicadas para. O SAP HANA na solução do Azure (instâncias grandes) baseia-se em hardware de bare-metal não partilhado/servidor de anfitrião que está atribuída a. O hardware de servidor é incorporado nos carimbos de data / maiores que contêm/servidor de computação, rede e infraestrutura de armazenamento. Como uma combinação é HANA adaptada center integração de dados (TDI) com certificação. SAP HANA no Azure (instâncias grandes) oferece os SKUs de servidor diferente ou tamanhos. Unidades podem ter 36 núcleos de Intel CPU e 768 GB de memória e vá até as unidades que tenham até 480 núcleos de Intel CPU e até 24 TB de memória.

O isolamento de cliente dentro de carimbo de data / infraestrutura é executado em inquilinos, que se parece com:

- **Funcionamento em rede**: isolamento de clientes na pilha de infraestrutura através de redes virtuais por inquilino cliente atribuído. Um inquilino é atribuído a um único cliente. Um cliente pode ter vários inquilinos. O isolamento de rede de inquilinos proíbe a comunicação de rede entre inquilinos no nível de carimbo de data / infraestrutura, mesmo que os inquilinos de pertencer ao mesmo cliente.
- **Componentes de armazenamento**: isolamento através de máquinas de virtuais de armazenamento que tenham volumes de armazenamento atribuídas. Volumes de armazenamento podem ser atribuídos a máquina de virtual de um armazenamento apenas. Uma máquina de virtual de armazenamento é atribuída exclusivamente para um único inquilino na pilha de infraestrutura de certificados de TDI do SAP HANA. Como resultado, os volumes de armazenamento atribuídos a uma máquina virtual de armazenamento podem ser acessados num específico e relacionado inquilino apenas. Não são visíveis entre os diferentes inquilinos implementados.
- **Servidor ou anfitrião**: uma unidade de anfitrião ou servidor não é partilhada entre inquilinos ou clientes. Um servidor ou o anfitrião implementada para um cliente, é uma unidade de computação do bare-metal atômicas que é atribuída a um único inquilino. *Não* é utilizada o particionamento de hardware ou software de criação de partições que poderá resultar numa partilha de um anfitrião ou um servidor com outro cliente. Volumes de armazenamento que estão atribuídas à máquina virtual de armazenamento do inquilino específico são instalados em tal servidor. Um inquilino pode ter um para muitos unidades de servidores de SKUs diferentes atribuídas exclusivamente.
- Dentro de um SAP HANA no carimbo de infraestrutura do Azure (instâncias grandes), muitos inquilinos diferentes são implementados e isolados entre si por meio dos conceitos de inquilino na rede, armazenamento e nível de computação. 


Estas unidades de bare-metal server são suportadas para executar apenas o SAP HANA. A camada de aplicação SAP ou a camada intermediária ware de carga de trabalho é executado em máquinas virtuais. Os carimbos de infraestrutura que executam o SAP HANA nas unidades do Azure (instâncias grandes) estão ligados as backbones de serviços de rede do Azure. Dessa forma, a conectividade de baixa latência entre máquinas virtuais e SAP HANA nas unidades do Azure (instâncias grandes) é fornecida.

Este documento é um dos vários documentos que abrangem o SAP HANA no Azure (instâncias grandes). Este documento apresenta a arquitetura básica, responsabilidades e serviços fornecidos pela solução. Capacidades de alto nível da solução também são discutidas. Para a maioria das outras áreas, tais como o sistema de rede e conectividade, quatro outros documentos sobre detalhes e informações de desagregação. A documentação do SAP HANA no Azure (instâncias grandes) não abrange aspetos da instalação do SAP NetWeaver ou implementações de SAP NetWeaver em VMs. SAP NetWeaver no Azure é descrito no documento separado, foi encontrado no mesmo contentor de documentação do Azure. 


Os documentos diferentes de orientação de instância grande do HANA abrangem as seguintes áreas:

- [Descrição geral do SAP HANA (instâncias grandes) e a arquitetura no Azure](hana-overview-architecture.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Infraestrutura de SAP HANA (instâncias grandes) e a conectividade no Azure](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Instalar e configurar o SAP HANA (instâncias grandes) no Azure](hana-installation.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [SAP HANA (instâncias grandes) elevada disponibilidade e recuperação após desastre no Azure](hana-overview-high-availability-disaster-recovery.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Resolução de problemas do SAP HANA (instâncias grandes) e monitorização no Azure](troubleshooting-monitoring.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Elevada disponibilidade, configure no SUSE, utilizando o STONITH](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/ha-setup-with-stonith)
- [Cópia de segurança do sistema operacional e de restauro para SKUs do tipo II](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/os-backup-type-ii-skus)

## <a name="definitions"></a>Definições

Várias definições de comuns são amplamente utilizadas na arquitetura e guia de implementação técnica. Tenha em atenção os seguintes termos e seus significados:

- **IaaS**: infraestrutura como serviço.
- **PaaS**: plataforma como serviço.
- **SaaS**: Software como serviço.
- **Componente SAP**: uma aplicação individual do SAP, como o componente Central de ERP (ECC), o Business Warehouse (BW), o Gestor de solução ou o Enterprise Portal (EP). Componentes SAP podem basear-se em tecnologias tradicionais de ABAP ou Java ou um aplicativo de não com base em NetWeaver, como objetos comerciais.
- **Ambiente de SAP**: um ou mais componentes SAP logicamente agrupados para executar uma função de negócio, como o desenvolvimento, controle de qualidade, treinamento, recuperação após desastre ou de produção.
- **Ambiente SAP**: refere-se para os ativos SAP inteiros em seu ambiente IT. A paisagem SAP inclui todos os ambientes de não produção e produção.
- **Sistema SAP**: A combinação de camada do DBMS e a camada de aplicativo de, por exemplo, um sistema de desenvolvimento SAP ERP, um sistema de teste de SAP BW e um sistema de produção do SAP CRM. Implementações do Azure não suportam dividindo estas duas camadas entre no local e o Azure. Um sistema SAP é implementado no local ou é implementado no Azure. Pode implantar os sistemas diferentes de um ambiente SAP para o Azure ou no local. Por exemplo, pode implementar o desenvolvimento de CRM do SAP e testar sistemas no Azure, enquanto implementa o SAP CRM produção sistema no local. Para o SAP HANA no Azure (instâncias grandes), seu objetivo é que hospede a camada de aplicativo SAP de sistemas SAP em VMs e a instância do SAP HANA relacionada numa unidade do SAP HANA no carimbo de data / do Azure (instâncias grandes).
- **Carimbo de instância grande**: uma pilha de infraestrutura de hardware que é dedicado para executar as instâncias do SAP HANA no Azure e com certificação TDI do SAP HANA.
- **SAP HANA no Azure (instâncias grandes):** nome oficial para a oferta no Azure executar o HANA instâncias em hardware com certificação TDI do SAP HANA, o que é implementada nos carimbos de data / instância grande em diferentes regiões do Azure. O termo relacionado *instância grande do HANA* é a abreviação de *SAP HANA no Azure (instâncias grandes)* e é amplamente utilizada neste guia de implementação técnica.
- **Em vários locais**: Descreve um cenário em que as VMs são implementadas para uma subscrição do Azure que tem conectividade do ExpressRoute do Azure entre centros de dados no local e o Azure, múltiplos sites ou site a site. Documentação do Azure em comum, esses tipos de implementações também são descritos como cenários de vários locais. O motivo para a ligação é para expandir os domínios no local, no local do Azure Active Directory/OpenLDAP e DNS no local para o Azure. O Panorama de no local é expandido para recursos do Azure das subscrições do Azure. Com essa extensão, as VMs podem ser parte do domínio no local. 

   Os utilizadores de domínio do domínio no local podem aceder aos servidores e executam serviços nessas VMS (como serviços do DBMS). Resolução de nomes e de comunicação entre VMs implementadas no local e VMs implementadas no Azure, é possível. Este cenário é típico de forma em que a maioria dos ativos SAP são implementados. Para obter mais informações, consulte [planear e conceber para o Gateway de VPN do Azure](../../../vpn-gateway/vpn-gateway-plan-design.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) e [criar uma rede virtual com uma ligação site a site com o portal do Azure](../../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
- **Inquilino**: um cliente implementado num carimbo de data / instância grande do HANA obtém isolado num *inquilino.* Um inquilino seja isolado no funcionamento em rede, armazenamento e a camada de computação de outros inquilinos. Unidades de armazenamento e computação atribuídas a diferentes inquilinos não é possível ver uns aos outros ou comunicar entre si no nível de carimbo de data / instância grande do HANA. Um cliente pode optar por implementações em inquilinos diferentes. Mesmo assim, não existe nenhuma comunicação entre inquilinos no nível de carimbo de data / instância grande do HANA.
- **Categoria SKU**: para a instância grande do HANA, as seguintes duas categorias de SKUs são oferecidas:
    - **Tipo de classe eu**: S72, S72m, S144, S144m, S192, S192m e S192xm
    - **Tipo de classe II**: S384, S384m, S384xm, S384xxm, S576m, S576xm, S768m, S768xm e S960m


Uma variedade de recursos adicionais estão disponíveis sobre como implementar uma carga de trabalho SAP na cloud. Se pretender executar uma implementação do SAP HANA no Azure, terá de ser experientes com e em consideração os princípios de IaaS do Azure e a implementação de cargas de trabalho SAP no Azure IaaS. Antes de continuar, consulte [soluções SAP de utilização em máquinas virtuais do Azure](get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) para obter mais informações. 

## <a name="certification"></a>Certificação

Além da certificação NetWeaver, o SAP requer uma certificação especial para o SAP HANA suportar o SAP HANA em determinados infraestruturas, como o IaaS do Azure.

É o núcleo sobre o NetWeaver e uma certificação do SAP HANA de grau, a nota SAP [1928533 no SAP Note # – aplicações SAP no Azure: produtos suportados e tipos de VM do Azure](https://launchpad.support.sap.com/#/notes/1928533).

Os registos de certificação para o SAP HANA nas unidades do Azure (instâncias grandes) podem ser encontrados no [plataformas IaaS com certificação SAP HANA](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure) site. 

O SAP HANA em tipos de Azure (instâncias grandes), referidos no SAP HANA certificadas site de plataformas de IaaS, fornece à Microsoft e SAP os clientes a capacidade de implantar grandes do SAP Business Suite, SAP BW, S/4 HANA, BW/4HANA ou outras cargas de trabalho do SAP HANA no Azure. A solução baseia-se o carimbo de hardware dedicado de certificado do SAP HANA ([SAP HANA adaptadas a integração do Centro de dados – TDI](https://scn.sap.com/docs/DOC-63140)). Se executar uma solução configurada TDI do SAP HANA, todos os aplicativos baseados em SAP HANA (como o SAP Business Suite no SAP HANA, SAP BW em SAP HANA, S4/HANA e BW4/HANA) funciona na infraestrutura de hardware.

Em comparação com a execução do SAP HANA nas VMs, essa solução tem um benefício. Ele fornece para volumes muito maiores de memória. Para ativar esta solução, precisa entender os seguintes aspetos fundamentais:

- Executam a aplicações de camada como não SAP de aplicações SAP em VMs que estão alojadas numa carimbos usual de hardware do Azure.
- Cliente infraestrutura no local, centros de dados, e implementações de aplicações estão ligadas para a plataforma na cloud através do ExpressRoute (recomendado) ou uma rede privada virtual (VPN). O Active Directory e DNS também é expandido para o Azure.
- A instância de base de dados do SAP HANA para carga de trabalho do HANA é executado no SAP HANA no Azure (instâncias grandes). O carimbo de data / instância grande está ligada em rede do Azure, para que o software em execução em VMs pode interagir com a instância HANA em execução numa instância grande do HANA.
- Hardware de SAP HANA no Azure (instâncias grandes) é fornecido num de IaaS com SUSE Linux Enterprise Server ou do Red Hat Enterprise Linux pré-instalado de hardware dedicado. Tal como acontece com as máquinas virtuais, ainda mais as atualizações e manutenção para o sistema operativo é sua responsabilidade.
- Instalação do HANA ou quaisquer componentes adicionais necessários para executar o SAP HANA em unidades de instância grande do HANA é sua responsabilidade. Todas as respectivas operações em curso e administração de SAP HANA no Azure também são da responsabilidade do cliente.
- Além das soluções descritas aqui, pode instalar os outros componentes na sua subscrição do Azure que se conecta ao SAP HANA no Azure (instâncias grandes). Os exemplos são componentes que permitem a comunicação com ou diretamente para o SAP HANA de base de dados, tais como servidores de atalhos, servidores RDP e SAP HANA Studio, serviços de dados do SAP para cenários de BI do SAP, ou soluções de monitorização de rede.
- Como no Azure, a instância grande do HANA oferece suporte a alta disponibilidade e a funcionalidade de recuperação após desastre.

## <a name="architecture"></a>Arquitetura

Num alto nível, o SAP HANA na solução do Azure (instâncias grandes) tem a camada de aplicativo SAP que residem nas VMs. A camada de base de dados reside no hardware configurada TDI do SAP localizado numa instância de gestão grande na mesma região do Azure que está ligada ao Azure IaaS.

> [!NOTE]
> Implemente a camada de aplicação SAP na mesma região do Azure como a camada do SAP DBMS. Esta regra é bem documentada na informações publicadas sobre cargas de trabalho SAP no Azure. 

A arquitetura geral do SAP HANA no Azure (instâncias grandes) fornece uma configuração de hardware com certificação TDI do SAP, o que é uma não virtualizados, bare-metal, o servidor de alto desempenho para a base de dados do SAP HANA. Ele também fornece a capacidade e a flexibilidade do Azure para dimensionar os recursos para a camada de aplicação SAP atender às suas necessidades.

![Descrição geral da arquitetura do SAP HANA no Azure (instâncias grandes)](./media/hana-overview-architecture/image1-architecture.png)

A arquitetura mostrada é dividida em três seções:

- **Direita**: mostra uma infraestrutura no local que executa aplicações de diferentes nos dados centra-se para que os utilizadores finais podem aceder a aplicações de LOB, como o SAP. O ideal é que isso no local infraestrutura, em seguida, está ligada ao Azure com o [ExpressRoute](https://azure.microsoft.com/services/expressroute/).

- **Centro de**: mostra IaaS do Azure e, neste caso, usar de VMs para alojar SAP ou outras aplicações que utilizam SAP HANA como um sistema do DBMS. Menor HANA nas instâncias que funcionam com a memória que fornecem VMs é implementada em VMs em conjunto com a sua camada de aplicativo. Para obter mais informações sobre as máquinas virtuais, consulte [máquinas virtuais](https://azure.microsoft.com/services/virtual-machines/).

   Serviços de rede do Azure são utilizados para agrupar sistemas SAP junto com outros aplicativos em redes virtuais. Nestas redes virtuais liguem a sistemas no local, bem como para o SAP HANA no Azure (instâncias grandes).

   Para aplicações SAP NetWeaver e bases de dados que são suportados para executar no Azure, consulte [1928533 de n. º de nota de suporte de SAP – aplicações SAP no Azure: produtos suportados e tipos de VM do Azure](https://launchpad.support.sap.com/#/notes/1928533). Para obter documentação sobre como implementar soluções SAP no Azure, consulte:

  -  [Utilizar o SAP em máquinas de virtuais do Windows](../../virtual-machines-windows-sap-get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
  -  [Utilize soluções SAP em máquinas virtuais do Azure](get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

- **Esquerda**: mostra o hardware com certificação TDI do SAP HANA no carimbo de data / instâncias grandes do Azure. As unidades de instância grande do HANA são ligadas às redes virtuais da sua subscrição ao utilizar a mesma tecnologia como a conectividade no local para o Azure.

O carimbo de instâncias grandes do Azure em si combina os seguintes componentes:

- **Computação**: servidores que são baseadas em processadores Intel Xeon E7-8890v3 ou Intel Xeon E7-8890v4 que fornecem a capacidade de computação necessária e são a certificação do SAP HANA.
- **Rede**: A unificação dos recursos de infraestrutura de rede de alta velocidade que interliga a computação, armazenamento e componentes de rede local.
- **Armazenamento**: uma infraestrutura de armazenamento que é acessada através de um recurso de infraestrutura de rede unificada. A capacidade de armazenamento específico que é fornecida depende do SAP HANA específicas sobre a configuração do Azure (instâncias grandes) que é implementada. Maior capacidade de armazenamento está disponível a um custo mensal adicional.

Dentro da infra-estrutura de multi-inquilino de carimbo de data / instância grande, os clientes são implementados como inquilinos isolados. Na implementação do inquilino, nome de uma subscrição do Azure dentro de sua inscrição do Azure. Esta subscrição do Azure é a que a instância grande do HANA é faturada relativamente. Estes inquilinos têm uma relação de 1:1 para a subscrição do Azure. Para uma rede, é possível aceder a uma unidade de instância grande do HANA implementada num inquilino numa única região do Azure a partir de diferentes redes virtuais que pertencem a diferentes subscrições do Azure. As subscrições do Azure têm de pertencer à mesma inscrição do Azure. 

Tal como acontece com VMs, o SAP HANA no Azure (instâncias grandes) é disponibilizada em várias regiões do Azure. Para oferecer capacidades de recuperação de desastre, pode optar por participar no. Carimbos de instância grande diferentes numa região geográfica político estão ligados entre si. Por exemplo, o HANA carimbos de instância grande no E.u.a. oeste e EUA Leste estão ligados através de uma ligação de rede dedicada para a replicação de recuperação após desastre. 

Assim como pode escolher entre diferentes tipos VM com máquinas virtuais do Azure, pode escolher entre diferentes SKUs de instância grande do HANA adaptadas para tipos diferentes de carga de trabalho do SAP HANA. SAP aplica-se em proporções de memória-para--socket de processador para diferentes cargas de trabalho com base nas gerações de processador Intel. A tabela seguinte mostra os tipos SKU oferecidos.

SAP HANA no serviço do Azure (instâncias grandes) está disponível em várias configurações nas regiões do Azure dos EUA oeste e EUA leste, leste da Austrália, Sudeste da Austrália, Europa Ocidental, Europa do Norte, leste do Japão e oeste do Japão.

[SKUs de HANA nas instâncias grandes com certificação SAP HANA](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure) lista, como:

| Solução SAP | CPU | Memória | Armazenamento | Disponibilidade |
| --- | --- | --- | --- | --- |
| Otimizado para OLAP: SAP BW, BW/4HANA<br /> ou SAP HANA para carga de trabalho OLAP genérica | SAP HANA no Azure S72<br /> – 2 x Intel® Xeon® processador E7 8890 v3<br /> 36 núcleos de CPU e 72 threads de CPU |  768 GB |  3 TB | Disponível |
| --- | SAP HANA no Azure S144<br /> – 4 x Intel® Xeon® processador E7 8890 v3<br /> 72 núcleos de CPU e 144 threads de CPU |  1,5 TB |  6 TB | Não é fornecido já |
| --- | SAP HANA em Azure S192<br /> – 4 x Intel® Xeon® processador E7 8890 v4<br /> 96 núcleos de CPU e de 192 threads de CPU |  2.0 TB DE |  8 TB | Disponível |
| --- | SAP HANA no Azure S384<br /> e 8 x processadores Intel® Xeon® E7 8890 v4<br /> 192 núcleos de CPU e 384 threads de CPU |  4.0 TB |  16 TB | Disponível |
| Otimizado para OLTP: SAP Business Suite<br /> no SAP HANA ou S/4HANA (OLTP),<br /> OLTP genérico | SAP HANA no Azure S72m<br /> – 2 x Intel® Xeon® processador E7 8890 v3<br /> 36 núcleos de CPU e 72 threads de CPU |  1,5 TB |  6 TB | Disponível |
|---| SAP HANA no Azure S144m<br /> – 4 x Intel® Xeon® processador E7 8890 v3<br /> 72 núcleos de CPU e 144 threads de CPU |  3.0 TB |  12 TB | Não é fornecido já |
|---| SAP HANA em Azure S192m<br /> – 4 x Intel® Xeon® processador E7 8890 v4<br /> 96 núcleos de CPU e de 192 threads de CPU  |  4.0 TB |  16 TB | Disponível |
|---| SAP HANA no Azure S384m<br /> e 8 x processadores Intel® Xeon® E7 8890 v4<br /> 192 núcleos de CPU e 384 threads de CPU |  6.0 TB |  18 TB | Disponível |
|---| SAP HANA no Azure S384xm<br /> e 8 x processadores Intel® Xeon® E7 8890 v4<br /> 192 núcleos de CPU e 384 threads de CPU |  8.0 TB |  22 TB |  Disponível |
|---| SAP HANA no Azure S576m<br /> – 12 x processadores Intel® Xeon® E7 8890 v4<br /> 288 núcleos de CPU e 576 threads de CPU |  12.0 TB |  28 DE TB | Disponível |
|---| SAP HANA no Azure S768m<br /> – 16 x processadores Intel® Xeon® E7 8890 v4<br /> 384 núcleos de CPU e 768 threads de CPU |  16.0 TB |  36 TB | Disponível |
|---| SAP HANA no Azure S960m<br /> a 20 x processadores Intel® Xeon® E7 8890 v4<br /> 480 núcleos de CPU e 960 threads de CPU |  20.0 TB |  46 TB | Disponível |


Em TDIv5 do SAP HANA, SAP permite o dimensionamento de específicas do cliente e projetos de específicas do cliente que podem levar a configurações de servidor que não estão listadas como a certificação na:

- [Aplicações com certificação do SAP HANA](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/appliances.html)
- [Plataformas de IaaS com certificação SAP HANA](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure)

Em muitos casos, estas configurações de servidor específicas do cliente realizar mais memória do que as unidades de servidor com certificação SAP. Ao trabalhar com o SAP, os clientes têm a possibilidade de obter suporte do SAP e certificar-se para as respetivas configurações de servidor específicas do cliente de tamanho. No Azure, os seguintes SKU standard de instância grande do HANA estão disponíveis e no Microsoft preço de lista para tais projetos de dimensionamento de específicas do cliente TDIv5.


| SKU do original que pode ser <br /> expandido na memória | CPU | Memória | Armazenamento | Disponibilidade |
| --- | --- | --- | --- | --- |
| S192m podem ser estendidos para | SAP HANA no Azure S192xm<br /> – 4 x Intel® Xeon® processador E7 8890 v4<br /> 96 núcleos de CPU e de 192 threads de CPU |  6.0 TB |  16 TB | Disponível |
| S384xm podem ser estendidos para | SAP HANA no Azure S384xxm<br /> e 8 x processadores Intel® Xeon® E7 8890 v4<br /> 192 núcleos de CPU e 384 threads de CPU |  12.0 TB |  28 DE TB | Disponível |
| S576m podem ser estendidos para | SAP HANA no Azure S576xm<br /> – 12 x processadores Intel® Xeon® E7 8890 v4<br /> 288 núcleos de CPU e 576 threads de CPU |  VERSÕES 18.0 TB |  41 TB | Disponível |
| S768m podem ser estendidos para | SAP HANA no Azure S768xm<br /> – 16 x processadores Intel® Xeon® E7 8890 v4<br /> 384 núcleos de CPU e 768 threads de CPU |  24.0 TB |  56 TB | Disponível |

- Núcleos de CPU = sum não-hyper-thread de núcleos de CPU da soma de processadores da unidade do servidor.
- Threads de CPU = sum de threads de computação fornecidos pelo hyper-thread núcleos de CPU da soma de processadores da unidade do servidor. A maioria das unidades são configuradas por predefinição para utilizar a tecnologia Hyper-Threading.
- Com base nas recomendações do fornecedor S768m, S768xm e S960m não estão configuradas para utilizar o Hyper-Threading para execução do SAP HANA.


As configurações específicas escolhidas são dependentes de carga de trabalho, recursos da CPU e memória pretendida. É possível que a carga de trabalho OLTP utilizar os SKUs que estão otimizados para a carga de trabalho OLAP. 

O hardware base para as ofertas, exceto unidades para projetos de dimensionamento de específicas do cliente, são com certificação TDI do SAP HANA. Duas classes diferentes de hardware dividem os SKUs em:

- S72, S72m, S144, S144m, S192, S192m e S192xm, que são referidas como "Tipo de classe," de SKUs.
- S384, S384m, S384xm, S384xxm, S576m, S576xm S768m, S768xm e S960m, que são conhecidos como o "Tipo de classe II" dos SKUs.

Um carimbo de data / instância grande do HANA completo não está alocada exclusivamente para um único cliente&#39;s de utilização. Esse fato aplica-se para os bastidores de recursos de computação e armazenamento ligados através de um recurso de infraestrutura de rede implementado no Azure, também. Infraestrutura de instância grande do HANA, como o Azure, implementa o outro cliente &quot;inquilinos&quot; que são isolados uns dos outros nos seguintes três níveis:

- **Rede**: isolamento através de redes virtuais dentro do carimbo de data / instância grande do HANA.
- **Armazenamento**: isolamento através de máquinas de virtuais de armazenamento que tenham volumes de armazenamento atribuídos e isolar os volumes de armazenamento entre inquilinos.
- **Computação**: dedicado a atribuição das unidades de servidor para um único inquilino. Não muito ou por software de criação de partições de unidades de servidor. Sem partilha de uma única unidade de anfitrião ou servidor entre inquilinos. 

As implementações de unidades de instância grande do HANA entre diferentes inquilinos não são visíveis para si. Unidades de instância grande do HANA implementadas em diferentes inquilinos não podem comunicar diretamente entre si no nível de carimbo de data / instância grande do HANA. Apenas as unidades de instância grande do HANA dentro de um inquilino podem comunicar entre si no nível de carimbo de data / instância grande do HANA.

Um inquilino implementado no carimbo de data / instância grande é atribuído a uma subscrição do Azure para efeitos de faturação. Para uma rede, pode ser acedido a partir de redes virtuais de outras subscrições do Azure dentro da mesmo inscrição do Azure. Se implementar com outra subscrição do Azure na mesma região do Azure, também pode optar por solicitar um inquilino de instância grande do HANA separado.

Existem diferenças significativas entre executar o SAP HANA nas instâncias grandes do HANA e SAP HANA em execução em VMs implementadas no Azure:

- Não há nenhuma camada de virtualização para o SAP HANA no Azure (instâncias grandes). Obtém o desempenho do hardware bare-metal subjacente.
- Ao contrário do Azure, o SAP HANA no servidor do Azure (instâncias grandes) é dedicado a um cliente específico. Não existe nenhuma possibilidade que uma unidade de servidor ou o anfitrião está duro ou por software particionado. Como resultado, uma unidade de instância grande do HANA é utilizada como atribuído como um todo para um inquilino e com isso para. Um reinício ou encerramento do servidor não gerar automaticamente o sistema operativo e a ser implementado em outro servidor do SAP HANA. (Para o tipo de classe eu SKUs, a única exceção é se um servidor encontra problemas e a nova implementação tem de ser efetuada noutro servidor.)
- Ao contrário do Azure, onde os tipos de processador do anfitrião estão selecionados para a proporção de relação preço/desempenho melhor, os tipos de processador escolhidos para SAP HANA no Azure (instâncias grandes) são a realização de mais alto da linha de processador Intel E7v3 e E7v4.


### <a name="run-multiple-sap-hana-instances-on-one-hana-large-instance-unit"></a>Executar várias instâncias do SAP HANA numa unidade de instância grande do HANA
É possível hospedar mais de uma instância ativa do SAP HANA nas unidades de instância grande do HANA. Para fornecer os recursos de instantâneos de armazenamento e recuperação após desastre, esta configuração requer que um volume definido por instância. Atualmente, as unidades de instância grande do HANA puderem ser subdivididas da seguinte forma:

- **S72, S72m, S144, S192**: em incrementos de 256 GB, com 256 GB a unidade de partida mais pequeno. Incrementos diferentes, como 256 GB e 512 GB podem ser combinados para o máximo da memória da unidade.
- **S144m e S192m**: em incrementos de 256 GB, com a menor unidade de 512 GB. Incrementos diferentes, como 512 GB e 768 GB podem ser combinados para o máximo da memória da unidade.
- **Tipo de classe II**: em incrementos de 512 GB, com a unidade de partida mais pequeno de 2 TB. Incrementos diferentes como 512 GB, 1 TB e 1,5 TB podem ser combinados para o máximo da memória da unidade.

Alguns exemplos de várias instâncias do SAP HANA em execução podem ser semelhante ao seguinte:

| SKU | Tamanho da memória | Tamanho de armazenamento | Tamanhos com várias bases de dados |
| --- | --- | --- | --- |
| S72 | 768 GB | 3 TB | instância HANA 1 x 768 GB<br /> ou instância de 1 x 512 GB + 1 x 256 GB de instância<br /> ou instâncias de 3 x 256 GB | 
| S72m | 1,5 TB | 6 TB | 3x512GB HANA nas instâncias<br />ou a instância de 1 x 512 GB + a instância de 1 x 1 TB<br />ou instâncias de 6 x 256 GB<br />ou instância de 1x1.5 TB | 
| S192m | 4 TB | 16 TB | instâncias de 8 x 512 GB<br />ou instâncias de 4 x 1 TB<br />ou instâncias de 4 x 512 GB + instâncias de 2 x 1 TB<br />ou instâncias de 4 x 768 GB + 2 x 512 GB instâncias<br />ou instância de 1 x 4 TB |
| S384xm | 8 TB | 22 TB | instâncias de 4 x 2 TB<br />ou instâncias de 2 4 TB<br />ou instâncias de 2 x 3 TB + instâncias de 1 x 2 TB<br />ou instâncias de 2x2.5 TB + instâncias de 1 x 3 TB<br />ou instância de 1 x 8 TB |


Existem também outras variações. 

### <a name="use-sap-hana-data-tiering-and-extension-nodes"></a>Utilizar nós de extensão e de criação de camadas de dados do SAP HANA
SAP suporta um modelo de camadas de dados para SAP BW das diferentes versões do SAP NetWeaver e SAP BW/4HANA. Para obter mais informações sobre o modelo de camadas de dados, consulte o documento SAP [SAP BW/4HANA e do SAP BW em HANA connosco de extensão do SAP HANA](https://www.sap.com/documents/2017/05/ac051285-bc7c-0010-82c7-eda71af511fa.html#).
Com a instância grande do HANA, pode utilizar a opção 1 configuração de nós de extensão do SAP HANA conforme explicado nos documentos de blog de perguntas frequentes e SAP. Opção 2 configurações podem ser configuradas com SKUs seguintes instância grande de HANA: S72m, S192, S192m, S384 e S384m. 

Quando examinar a documentação, a vantagem pode não estar visível imediatamente. Mas quando examinar as diretrizes de dimensionamento do SAP, verá uma vantagem, utilizando a opção-1 e a opção 2 nós de extensão do SAP HANA. Seguem-se exemplos:

- Diretrizes de dimensionamento do SAP HANA geralmente exigem o dobro da quantidade do volume de dados como memória. Quando executar a sua instância do SAP HANA com os dados de acesso frequente, tem apenas 50 por cento ou menor de memória preenchido com dados. O restante da memória ideal é que está a ser retido para o SAP HANA fazendo seu trabalho.
- Isso significa que numa unidade de S192 instância grande do HANA com 2 TB de memória, execução de uma base de dados do SAP BW, só tem 1 TB como volume de dados.
- Se utilizar um nó adicional para a extensão de SAP HANA de opção-1, também um S192 HANA grandes instância SKU, ele fornece uma capacidade adicional de 2 TB para o volume de dados. Na configuração da opção-2, receberá um adicional 4 TB para volumes de dados muito ou pouco. Em comparação com o nó de acesso frequente, a capacidade de memória cheia do nó de extensão "quente" pode ser utilizada para o armazenamento de dados para a opção-1. Duplicar a memória pode ser utilizada para o volume de dados na configuração de nó de extensão de SAP HANA de opção-2.
- Acaba tendo uma capacidade de 3 TB para os seus dados e uma proporção de acesso frequente para quente de 1:2 para a opção-1. Tem 5 TB de dados e uma proporção de 1:4 com a configuração de nó de extensão de opção-2.

Quanto maior for o volume de dados em comparação comparado a memória, maior será o provável que os dados de acesso pouco frequente que estão fazendo para são armazenados no armazenamento de disco.


## <a name="operations-model-and-responsibilities"></a>Modelo de operações e responsabilidades

O serviço fornecido com o SAP HANA no Azure (instâncias grandes) é alinhado com os serviços de IaaS do Azure. Obtém uma instância de uma instância grande do HANA com um sistema operativo instalado que está otimizado para o SAP HANA. Como com as VMs de IaaS do Azure, a maioria das tarefas de sistema de proteção do sistema operacional, instalar software adicional, instalar o HANA, operar o sistema operacional e HANA e a atualizar o sistema operacional e HANA é sua responsabilidade. Microsoft não força a atualizações do SO ou atualizações do HANA com.

![Responsabilidades do SAP HANA no Azure (instâncias grandes)](./media/hana-overview-architecture/image2-responsibilities.png)

Conforme mostrado no diagrama, o SAP HANA no Azure (instâncias grandes) é um multi-inquilino que oferecem IaaS. Na maior parte, a divisão de responsabilidade é no limite a infraestrutura de sistema operacional. A Microsoft é responsável por todos os aspectos do serviço abaixo da linha do sistema operativo. É responsável por todos os aspectos do serviço acima da linha. O sistema operacional é sua responsabilidade. Pode continuar a utilizar mais atual no local métodos que pode ser empregadas para conformidade, segurança, gestão de aplicações, base e gerenciamento de sistema operacional. Os sistemas aparecem como se eles estão em sua rede no que respeita todas.

Este serviço está otimizado para o SAP HANA, pelo que existem áreas em que precisa trabalhar com a Microsoft para usar os recursos de infraestrutura subjacente para obter melhores resultados.

A lista seguinte fornece mais detalhes sobre cada uma das camadas e as suas responsabilidades:

**Funcionamento em rede**: todas as redes internas para o carimbo de instância grande execução do SAP HANA. Sua responsabilidade inclui acesso a armazenamento, conectividade entre o instâncias (para Escalamento horizontal e outras funções), a conectividade para o cenário e a conectividade para o Azure onde está alojada a camada de aplicação SAP em VMs. Ele também inclui a conectividade WAN entre centros de dados do Azure para a replicação de objetivos de recuperação após desastre. Todas as redes estão particionadas por inquilino e tem a qualidade de serviço aplicado.

**Armazenamento**: O virtualizado particionada para todos os volumes necessários para os servidores de SAP HANA, bem como para instantâneos de armazenamento. 

**Servidores**: os servidores físicos dedicados para executar o bds SAP HANA atribuído aos inquilinos. Os servidores do tipo a classe de SKUs são abstraído do hardware. Com esses tipos de servidores, a configuração do servidor é recolhida e mantida em perfis, que podem ser movidos um hardware físico para outro hardware físico. Tal (manual) mover de um perfil por operações pode ser em comparação com um pouco para recuperação do serviço do Azure. Os servidores dos SKUs de classe de tipo II não oferecem esse recurso.

**SDDC**: O software de gestão que é utilizado para gerir dados centra-se como entidades definidas por software. Ele permite que a Microsoft para recursos de conjunto de dimensionamento, disponibilidade e motivos de desempenho.

**Navegadores**: O sistema operacional é escolher (SUSE Linux ou Red Hat Linux) que está em execução nos servidores. As imagens de sistema operacional que são fornecidos com foram fornecidas pelo fornecedor do Linux individual para a Microsoft para a execução do SAP HANA. Tem de ter uma subscrição com o fornecedor do Linux para a imagem específica otimizadas para SAP HANA. É responsável por registar as imagens com o fornecedor do sistema operacional. 

Do ponto de entregar direito de permanência pela Microsoft, é responsável por qualquer ainda mais patches do sistema operativo Linux. Esta aplicação de patches inclui pacotes adicionais que poderá ser necessário para uma instalação de SAP HANA com êxito e que não estavam incluídas pelo fornecedor do Linux específico no seu SAP HANA com otimização de imagens do sistema operacional. (Para obter mais informações, consulte a documentação de instalação do HANA e anotações de SAP do SAP.) 

É responsável por aplicação de patches de SO devido avarias ou a otimização do sistema operacional e seus drivers em relação ao hardware de servidor específico. É também responsável por segurança ou a aplicação de patches funcional do sistema operacional. 

Sua responsabilidade também inclui a monitorização e planeamento da capacidade do:

- Consumo de recursos de CPU.
- Consumo de memória.
- Volumes de disco relacionados com espaço livre, IOPS e latência.
- Tráfego de volume de rede entre a instância grande do HANA e a camada de aplicação SAP.

A infraestrutura subjacente da instância grande do HANA fornece funcionalidade de cópia de segurança e restauro do volume de sistema operacional. Usar essa funcionalidade também é sua responsabilidade.

**Middleware**: O SAP HANA de instância, principalmente. Administração, as operações e monitorização são da responsabilidade do cliente. Pode utilizar a funcionalidade fornecida para utilizar instantâneos de armazenamento para fins de recuperação e cópia de segurança e restauro. Estas capacidades são fornecidas pela infraestrutura. As suas responsabilidades também incluem o design de elevada disponibilidade ou recuperação após desastre com esses recursos, tirando partido-los e a monitorização para determinar se os instantâneos de armazenamento é executado com êxito.

**Dados**: os dados geridos pelo SAP HANA e outros dados, como cópias de segurança de partilham de ficheiros localizados em volumes ou ficheiros. As suas responsabilidades incluem a monitorizar o espaço livre em disco e a gerir o conteúdo nos volumes. É também responsável por monitorizar a execução bem-sucedida de cópias de segurança de volumes de discos e instantâneos de armazenamento. Execução bem-sucedida de replicação de dados para sites de recuperação após desastre é da responsabilidade da Microsoft.

**Aplicações:** as instâncias de aplicações SAP ou, no caso de aplicações não pertencentes à SAP, a camada de aplicativo desses aplicativos. As suas responsabilidades incluem a implementação, administração, operações e monitorização desses aplicativos. É responsável por planeamento da capacidade do consumo de recursos de CPU, o consumo de memória, o consumo de armazenamento do Azure e o consumo de largura de banda de rede nas redes virtuais. É também responsável por planeamento da capacidade do consumo de recursos de redes virtuais para o SAP HANA no Azure (instâncias grandes).

**WANs**: as ligações estabelecer no local para implementações do Azure para cargas de trabalho. Todos os clientes com a instância grande do HANA utilizam o Azure ExpressRoute para conectividade. Esta ligação não faz parte do SAP HANA na solução do Azure (instâncias grandes). É responsável pela configuração desta ligação.

**Arquivo**: poderá preferir arquivar as cópias dos dados ao utilizar os seus próprios métodos nas contas de armazenamento. Arquivamento necessita de operações, os custos, conformidade e gestão. É responsável por gerar cópias do arquivo e as cópias de segurança no Azure e armazená-los de uma forma em conformidade.

Consulte a [SLA para o SAP HANA no Azure (instâncias grandes)](https://azure.microsoft.com/support/legal/sla/sap-hana-large/v1_0/).

## <a name="sizing"></a>Dimensionamento

Dimensionamento para a instância grande do HANA não é diferente de dimensionamento para o HANA em geral. Para existente e sistemas que pretende mover de outros RDBMS ao HANA, SAP fornece vários relatórios que são executadas em seus sistemas SAP existentes. Se a base de dados for movido para o HANA, estes relatórios Verifique os dados e calcular os requisitos de memória para a instância do HANA. Para obter mais informações sobre como executar esses relatórios e obtenha os mais recentes patches ou versões, leia as notas de SAP seguintes:

- [A nota SAP #1793345 - Dimensionar para SAP Suite em HANA](https://launchpad.support.sap.com/#/notes/1793345)
- [SAP Note #1872170 - Suite em HANA e S/4 HANA relatório de dimensionamento](https://launchpad.support.sap.com/#/notes/1872170)
- [A nota SAP #2121330 - FAQ: SAP BW em HANA relatório de dimensionamento](https://launchpad.support.sap.com/#/notes/2121330)
- [SAP Note #1736976 - relatório de dimensionamento para BW em HANA](https://launchpad.support.sap.com/#/notes/1736976)
- [SAP Note #2296290 - Dimensionar novo relatório para BW em HANA](https://launchpad.support.sap.com/#/notes/2296290)

Para implementações de campo verde, Sizer rápido do SAP está disponível para calcular os requisitos de memória da implementação de software SAP em cima do HANA.

Requisitos de memória para HANA aumentam à medida que aumenta o volume de dados. Tenha em atenção o consumo de memória atual para o ajudar a prever o que ele vai ser no futuro. Com base nos requisitos de memória, em seguida, pode mapear o seu pedido em um dos SKUs de instância grande do HANA.

## <a name="requirements"></a>Requisitos

Esta lista monta requisitos para executar o SAP HANA no Azure (instâncias grandes).

**Microsoft Azure**

- Uma subscrição do Azure que pode ser ligada ao SAP HANA no Azure (instâncias grandes).
- Contrato de suporte Premier da Microsoft. Para obter informações específicas relacionadas com a executar o SAP no Azure, consulte [2015553 de n. º de nota de suporte de SAP – SAP no Microsoft Azure: pré-requisitos do suporte](https://launchpad.support.sap.com/#/notes/2015553). Se utilizar unidades de instância grande do HANA com 384 e mais CPUs, terá também de estender o Premier do contrato de suporte para incluir a resposta rápida do Azure.
- Reconhecimento dos SKUs de instância grande HANA é necessário depois de realizar um exercício de dimensionamento com o SAP.

**Conectividade de rede**

- ExpressRoute entre no local para o Azure: ligar o seu centro de dados no local ao Azure, certifique-se solicitar, pelo menos, uma ligação de 1 Gbps a partir do seu ISP. 

**Sistema operativo**

- Licenças para o SUSE Linux Enterprise Server 12 para aplicações SAP.

   > [!NOTE] 
   > O sistema de operativo fornecido pela Microsoft não se encontra registado SUSE. Ele não está ligado a uma instância de ferramenta de gestão de subscrição.

- SUSE Linux subscrição ferramenta de gestão implementados no Azure numa VM. Essa ferramenta fornece a capacidade para o SAP HANA no Azure (instâncias grandes) para ser registrado e respectivamente atualizado pelo SUSE. (Não há nenhum acesso à internet no Centro de dados HANA de instância grande). 
- Licenças para Red Hat Enterprise Linux 6.7 ou 7.x para o SAP HANA.

   > [!NOTE]
   > O sistema de operativo fornecido pela Microsoft não está registado com a Red Hat. Ele não está ligado a uma instância do Gestor de subscrições do Red Hat.

- Red Hat subscrição Manager implementados no Azure numa VM. O Gestor de subscrições do Red Hat fornece a capacidade para o SAP HANA no Azure (instâncias grandes) a ser registado e atualizado respectivamente da Red Hat. (Não há nenhum acesso direto à internet do inquilino implementadas no carimbo de instâncias grandes do Azure).
- O SAP requer que tenha suporte a um contrato com o seu fornecedor do Linux também. Este requisito não é removido pela solução de instância grande do HANA ou o fato de que executam o Linux no Azure. Ao contrário com algumas das imagens da galeria do Linux do Azure, a tarifa de serviço é *não* incluída na oferta de solução da instância grande do HANA. É da responsabilidade do cliente para satisfazer os requisitos de SAP sobre contratos de suporte com o distribuidor do Linux. 
   - Para o SUSE Linux, procure os requisitos de contratos de suporte [1984787 no SAP Note # - SUSE Linux Enterprise Server 12: observações de instalação](https://launchpad.support.sap.com/#/notes/1984787) e [1056161 no SAP Note # - suporte de prioridade do SUSE para aplicações SAP](https://launchpad.support.sap.com/#/notes/1056161).
   - Para Red Hat Linux, tem de ter os níveis de assinatura corretos que incluem o suporte e serviço de atualizações para os sistemas operativos de instância grande do HANA. Red Hat recomenda o Red Hat Enterprise Linux para [soluções SAP] (https://access.redhat.com/solutions/3082481 subscrição. 

Para a matriz de suporte de diferentes versões do SAP HANA com diferentes versões do Linux, consulte [SAP Note #2235581](https://launchpad.support.sap.com/#/notes/2235581).

Para a matriz de compatibilidade do sistema operativo e versões de firmware/controlador HLI, consulte [atualização do SO para HLI](os-upgrade-hana-large-instance.md).


> [!IMPORTANT] 
> Unidades de tipo II apenas o SLES 12 SP2 sistema operacional de versão é suportada neste momento. 


**Base de Dados**

- As licenças e componentes de instalação de software para o SAP HANA (plataforma ou enterprise edition).

**Aplicações**

- Contratos do suporte de licenças e componentes de instalação de software para todas as aplicações SAP que liguem ao SAP HANA e SAP relacionado.
- Licenças e componentes de instalação de software para todos os aplicativos não pertencentes à SAP utilizado em relação ao SAP HANA em ambientes do Azure (instâncias grandes) e relacionadas com contratos de suporte.

**Habilidades**

- Experiência com e conhecimento do IaaS do Azure e os respetivos componentes.
- Experiência com e o conhecimento de como implementar uma carga de trabalho SAP no Azure.
- Instalação do SAP HANA certificadas pessoal.
- SAP habilidades arquiteto projetar elevada disponibilidade e recuperação após desastre em torno do SAP HANA.

**SAP**

- Expectativa é que for um cliente do SAP e têm suporte para um contrato com o SAP.
- Especialmente para implementações da classe de tipo II de SKUs de instância grande do HANA, consulte o SAP em versões do SAP HANA e as configurações eventual no hardware de ampliação de grande dimensão.


## <a name="storage"></a>Armazenamento

O esquema de armazenamento para o SAP HANA no Azure (instâncias grandes) é configurado pelo SAP HANA no modelo de implementação clássica através do SAP diretrizes recomendada. As diretrizes estão documentadas no [requisitos de armazenamento do SAP HANA](http://go.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html) white paper.

Instância grande do HANA, do tipo de classe que é fornecido com quatro vezes o volume de memória como volume de armazenamento. Para a classe de tipo II de unidades de instância grande do HANA, o armazenamento não mais de quatro vezes. As unidades são fornecidos com um volume que pretende armazenar os backups de log de transação do HANA. Para obter mais informações, consulte [instalar e configurar o SAP HANA (instâncias grandes) no Azure](hana-installation.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Consulte a tabela seguinte em termos de alocação de armazenamento. A tabela lista a capacidade estimada para os volumes diferentes fornecido com as diferentes unidades de instância grande do HANA.

| Instância grande do HANA SKU | dados do Hana | Hana/do registo | Hana/partilhado | Hana/logbackups |
| --- | --- | --- | --- | --- |
| S72 | 1,280 GB | 512 GB | 768 GB | 512 GB |
| S72m | 3,328 GB | 768 GB |1,280 GB | 768 GB |
| S192 | 4,608 GB | 1024 GB | 1,536 GB | 1024 GB |
| S192m | 11,520 GB | 1,536 GB | 1,792 GB | 1,536 GB |
| S192xm |  11,520 GB |  1,536 GB |  1,792 GB |  1,536 GB |
| S384 | 11,520 GB | 1,536 GB | 1,792 GB | 1,536 GB |
| S384m | 12 000 GB | 2,050 GB | 2,050 GB | 2040 GB |
| S384xm | GB DE 16.000 | 2,050 GB | 2,050 GB | 2040 GB |
| S384xxm |  20.000 GB | 3100 GB | 2,050 GB | 3100 GB |
| S576m | 20.000 GB | 3100 GB | 2,050 GB | 3100 GB |
| S576xm | 31,744 GB | 4,096 GB | 2.048 GB | 4,096 GB |
| S768m | 28,000 GB | 3100 GB | 2,050 GB | 3100 GB |
| S768xm | 40,960 GB | 6,144 GB | 4,096 GB | 6,144 GB |
| S960m | 36,000 GB | 4,100 GB | 2,050 GB | 4,100 GB |


Volumes implementados reais podem variar com base na implementação e a ferramenta que é utilizada para mostrar os tamanhos de volume.

Se subdividir um SKU de instância grande do HANA, alguns exemplos das partes de divisão possível podem ter o seguinte aspeto:

| Partição de memória em GB | dados do Hana | Hana/do registo | Hana/partilhado | Hana/registo/cópia de segurança |
| --- | --- | --- | --- | --- |
| 256 | 400 GB | 160 GB | 304 GB | 160 GB |
| 512 | 768 GB | 384 GB | 512 GB | 384 GB |
| 768 | 1,280 GB | 512 GB | 768 GB | 512 GB |
| 1,024 | 1,792 GB | 640 GB | 1024 GB | 640 GB |
| 1,536 | 3,328 GB | 768 GB | 1,280 GB | 768 GB |


Estes tamanhos são números de volume aproximado que podem variar ligeiramente com base na implementação e as ferramentas usadas para examinar os volumes. Também há outros tamanhos de partição, como 2,5 TB. Estes tamanhos de armazenamento são calculados com uma fórmula semelhante ao utilizado para as partições anteriores. O termo "partições" não significa que o sistema operativo, memória ou recursos da CPU são de qualquer forma particionados. Ele indica as partições de armazenamento para as diferentes instâncias HANA, que pode querer implementar numa única unidade de instância grande do HANA. 

Poderá precisar de mais armazenamento. Pode adicionar armazenamento ao comprar armazenamento adicional em unidades de 1 TB. Este armazenamento adicional pode ser adicionado como volume adicional. Ele também pode ser usado para estender um ou mais dos volumes existentes. Não é possível diminuir os tamanhos dos volumes como originalmente implementado e quase totalmente documentados por tabelas anteriores. Também não é possível alterar os nomes dos volumes ou nomes de montagem. Os volumes de armazenamento descritos anteriormente, são anexados às unidades instância grande do HANA como NFS4 volumes.

Pode utilizar instantâneos de armazenamento para fins de recuperação e cópia de segurança e restauro. Para obter mais informações, consulte [SAP HANA (instâncias grandes) elevada disponibilidade e recuperação após desastre no Azure](hana-overview-high-availability-disaster-recovery.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Consultar [cenários suportados de HLI](hana-supported-scenario.md) para obter detalhes de esquema de armazenamento para o seu cenário.

### <a name="encryption-of-data-at-rest"></a>Encriptação de dados inativos
O armazenamento utilizado para a instância grande do HANA permite a criptografia transparente de dados quando ele é armazenado nos discos. Quando uma unidade de instância grande do HANA é implementada, pode ativar este tipo de criptografia. Também pode alterar para volumes criptografados após a implantação se efetivar. A mudança de não encriptadas para volumes criptografados é transparente e não exige período de indisponibilidade. 

Com o tipo de a classe de SKUs, o volume de arranque LUN é armazenado no é encriptado. Para a classe de tipo II de SKUs de instância grande do HANA, tem de encriptar o arranque LUN com métodos de SO. Para obter mais informações, contacte a equipa de gestão de serviço da Microsoft.


## <a name="networking"></a>Redes

A arquitetura dos serviços de rede do Azure é um componente fundamental da implementação de aplicações SAP numa instância grande do HANA com êxito. Normalmente, o SAP HANA nas implementações do Azure (instâncias grandes) tem um ambiente SAP maior com várias soluções diferentes de SAP com diversos tamanhos de bases de dados, o consumo de recursos de CPU e utilização da memória. É provável que nem todos os esses sistemas SAP baseiam-se no SAP HANA. A paisagem SAP é provavelmente uma híbrida que utiliza:

- Implementar SAP sistemas no local. Devido a seus tamanhos, esses sistemas atualmente não é possível alojados no Azure. Um exemplo é um sistema de SAP ERP, que é executado no SQL Server (como a base de dados) e requer mais recursos de CPU ou memória que podem fornecer as VMs de produção.
- Implementado com base em SAP HANA SAP sistemas no local.
- Sistemas implantados de SAP em VMs. Estes sistemas podem ser o desenvolvimento, teste, área de segurança, ou instâncias de produção para qualquer um dos aplicativos baseados em SAP NetWeaver, que podem implementar com êxito no Azure (em VMs), com base na procura de memória e de consumo de recursos. Estes sistemas também podem ser baseados nas bases de dados como o SQL Server. Para obter mais informações, consulte [1928533 de n. º de nota de suporte de SAP – aplicações SAP no Azure: produtos suportados e tipos de VM do Azure](https://launchpad.support.sap.com/#/notes/1928533/E). E esses sistemas podem basear-se nas bases de dados, como o SAP HANA. Para obter mais informações, consulte [plataformas IaaS com certificação SAP HANA](http://global.sap.com/community/ebook/2014-09-02-hana-hardware/enEN/iaas.html).
- Implementar servidores de aplicações SAP no Azure (em VMs) que tiram partido do SAP HANA no Azure (instâncias grandes) no carimbos de instâncias grandes do Azure.

Um ambiente SAP com quatro ou mais cenários de implementação diferentes de híbrido é normal. Também há muitos casos de clientes de cenários SAP completos, que são executadas no Azure. À medida que as VMs se tornar mais poderosas, aumenta o número de clientes que mover todas as suas soluções SAP no Azure.

Redes no contexto de sistemas SAP implementadas no Azure do Azure não é complicado. Baseia-se os seguintes princípios:

- Redes virtuais do Azure tem de estar ligados ao circuito do ExpressRoute que se liga a uma rede no local.
- Um circuito do ExpressRoute que se liga no local para o Azure, normalmente, deve ter uma largura de banda de 1 Gbps ou superior. Esta largura de banda mínima permite a largura de banda suficiente para a transferência de dados entre sistemas no local e que são executadas em VMs. Também permite que a largura de banda suficiente para a ligação a sistemas do Azure dos utilizadores no local.
- Todos os sistemas SAP no Azure tem de ser configurados nas redes virtuais para comunicar entre si.
- Active Directory e DNS alojadas no local são expandidos para o Azure através do ExpressRoute no local.


> [!NOTE] 
> Do ponto de vista faturação, apenas uma subscrição do Azure pode ser associada ao inquilino apenas um num carimbo de data / instância grande numa região do Azure específica. Por outro lado, um único inquilino de carimbo de data / instância grande pode ser ligado a apenas uma subscrição do Azure. Este requisito é consistente com outros objetos cobrar no Azure.

Se o SAP HANA no Azure (instâncias grandes) é implementado em várias regiões do Azure diferentes, um inquilino separado é implementado no carimbo de data / instância grande. Pode executar ambos na mesma subscrição do Azure, desde que estas instâncias são parte do mesmo ambiente SAP. 

> [!IMPORTANT] 
> Apenas a implementação do Azure Resource Manager é suportada com o SAP HANA no Azure (instâncias grandes).

 

### <a name="additional-virtual-network-information"></a>Informações de rede virtual adicionais

Para ligar uma rede virtual para o ExpressRoute, tem de criar um gateway do Azure. Para obter mais informações, consulte [sobre os gateways de rede virtual para o ExpressRoute](../../../expressroute/expressroute-about-virtual-network-gateways.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). 

Um gateway do Azure pode ser utilizado com o ExpressRoute para uma infra-estrutura de fora do Azure ou para um carimbo de instâncias grandes do Azure. Um gateway do Azure também pode ser utilizado para estabelecer ligação entre redes virtuais. Para obter mais informações, consulte [configurar uma ligação de rede de rede para o Resource Manager com o PowerShell](../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Pode ligar o gateway do Azure para um máximo de quatro diferentes conexões do ExpressRoute, desde que essas conexões provêm de diferentes routers de limite de enterprise da Microsoft. Para obter mais informações, consulte [infraestrutura de SAP HANA (instâncias grandes) e a conectividade no Azure](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

> [!NOTE] 
> A taxa de transferência que fornece um gateway do Azure é diferente para os dois casos de utilização. Para obter mais informações, consulte [sobre o Gateway de VPN](../../../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). O débito máximo que pode conseguir com um gateway de rede virtual é de 10 Gbps ao utilizar uma ligação do ExpressRoute. Copiar ficheiros entre uma VM que residem numa rede virtual e um sistema no local (como um fluxo de cópia única) não alcançar o débito total do gateway diferente SKUs. Para aproveitar a largura de banda completa do gateway de rede virtual, utilize vários fluxos. Ou, tem de copiar ficheiros diferentes em fluxos paralelos de um único ficheiro.


### <a name="networking-architecture-for-hana-large-instance"></a>Arquitetura de rede para a instância grande do HANA
A arquitetura de rede para a instância grande do HANA pode ser separada em quatro partes distintas:

- Local de rede e a ligação do ExpressRoute para o Azure. Esta parte é o domínio do cliente e está ligada ao Azure através do ExpressRoute. Consulte a parte inferior direita na figura a seguir.
- Serviços de rede do Azure, conforme discutidos anteriormente, com redes virtuais, que novamente tem gateways. Esta parte é uma área em que precisa localizar os designs apropriados para os seus requisitos de aplicações, segurança e os requisitos de conformidade. Se utilizar a instância grande do HANA é outro ponto a considerar em termos de número de redes virtuais e os SKUs de gateway do Azure à sua escolha. Veja o canto superior direito da figura.
- Conectividade de instância grande do HANA através de tecnologia de ExpressRoute para o Azure. Esta parte é implementada e processada pela Microsoft. Tudo que precisa fazer é fornecer alguns intervalos de endereços IP após a implementação de seus ativos na instância grande do HANA ligarem o circuito do ExpressRoute para as redes virtuais. Para obter mais informações, consulte [infraestrutura de SAP HANA (instâncias grandes) e a conectividade no Azure](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 
- Funcionamento em rede na instância grande do HANA, que é principalmente transparente para.

![Rede virtual ligada ao SAP HANA no Azure (instâncias grandes) e no local](./media/hana-overview-architecture/image3-on-premises-infrastructure.png)

O requisito de que seus ativos no local tem de se ligar através do ExpressRoute para o Azure não muda o fato de usar a instância grande do HANA. O requisito de ter uma ou várias redes virtuais que executam as VMs que alojam a camada da aplicação que liga para o HANA nas instâncias alojada em unidades de instância grande do HANA, também não é alterado. 

As diferenças para implementações de SAP no Azure são:

- As unidades de instância grande do HANA do seu inquilino do cliente estão ligadas através de outro circuito do ExpressRoute em suas redes virtuais. Para separar as condições de carga, o local para ligações do ExpressRoute de redes virtuais e as ligações entre redes virtuais e a instância grande do HANA não partilha os routers da mesmos.
- O perfil de carga de trabalho entre a camada de aplicação SAP e a instância grande do HANA é de natureza diferente, com muitas solicitações pequenas e o aumento, como transferências de dados (conjuntos de resultados) da SAP HANA para a camada de aplicativo.
- A arquitetura de aplicação SAP é mais sensível à latência de rede que cenários típicos onde os dados são trocados entre no local e o Azure.
- O gateway de rede virtual tem, pelo menos, duas ligações do ExpressRoute. Ambas as ligações partilham a largura de banda máxima para os dados recebidos do gateway de rede virtual.

A latência de rede teve entre VMs e instâncias grandes do HANA unidades podem ser mais do que um típica latência ida e volta de rede de VM para VM. Depende da região do Azure, os valores de medida podem exceder a latência de ida e volta de 0,7 ms classificada como abaixo da média na [1100926 no SAP Note # - FAQ: desempenho de rede](https://launchpad.support.sap.com/#/notes/1100926/E). No entanto, os clientes implementar aplicações de SAP de produção com base em SAP HANA com êxito na instância grande do SAP HANA. Os clientes que implementaram melhorias relatório ao executar seus aplicativos de SAP no SAP HANA através da utilização de unidades de instância grande do HANA. Certifique-se de que testar os processos empresariais minuciosamente em instâncias grandes do HANA do Azure.
 
Para fornecer a latência de rede determinística entre VMs e instâncias grandes do HANA, a escolha do gateway de rede virtual SKU é essencial. Ao contrário dos padrões de tráfego entre VMs e no local, o padrão de tráfego entre VMs e instâncias grandes do HANA pode desenvolver pequenas mas elevadas rajadas de volumes de pedidos e dados a serem transmitidos. Para lidar com essas extrapolações bem, recomendamos vivamente a utilização do SKU de gateway de UltraPerformance. Para a classe de tipo II de SKUs de instância grande do HANA, a utilização do SKU de gateway de UltraPerformance como um gateway de rede virtual é obrigatória.

> [!IMPORTANT] 
> Tendo em conta o tráfego de rede geral entre o aplicativo SAP e camadas de base de dados, apenas o HighPerformance ou SKUs de gateway de UltraPerformance para redes virtuais são suportados para ligar ao SAP HANA no Azure (instâncias grandes). Para HANA grandes instância de tipo II SKUs, apenas o SKU do gateway UltraPerformance é suportado como um gateway de rede virtual.



### <a name="single-sap-system"></a>Único sistema SAP

A infraestrutura no local mostrada anteriormente está ligada através do ExpressRoute para o Azure. O circuito do ExpressRoute liga-se num router de periferia de empresa. Para obter mais informações, consulte [visão geral técnica do ExpressRoute](../../../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Depois da rota é estabelecida, liga-se para o backbone do Azure, e todas as regiões do Azure estão acessíveis.

> [!NOTE] 
> Para executar cenários SAP no Azure, ligue o router de periferia de empresa mais próxima da região do Azure no ambiente SAP. Carimbos de instâncias grandes do Azure estão ligados através de dispositivos de router de limite de enterprise dedicado para minimizar a latência de rede entre VMs na carimbos de IaaS do Azure e a instância grande.

O gateway de rede virtual para as VMs que alojam as instâncias da aplicação SAP está ligado ao circuito do ExpressRoute. Da mesma rede virtual está ligada a um router de limite de enterprise separado dedicado à ligação a carimbos de data / instância grande.

Este sistema é um exemplo simples de um único sistema SAP. Camada de aplicação SAP está alojada no Azure. A base de dados do SAP HANA é executado no SAP HANA no Azure (instâncias grandes). A pressuposição é de que a largura de banda de gateway de rede virtual de 2 Gbps ou 10 Gbps débito não representa um afunilamento.

### <a name="multiple-sap-systems-or-large-sap-systems"></a>Vários sistemas SAP ou sistemas grandes de SAP

Se vários sistemas SAP ou sistemas de grande dimensão SAP forem implementados para ligar ao SAP HANA no Azure (instâncias grandes), o débito do gateway de rede virtual pode se tornar um afunilamento. Nesse caso, divida as camadas do aplicativo em várias redes virtuais. Também pode criar uma rede virtual especial que se liga a instância grande do HANA para casos como:

- Efetuar cópias de segurança diretamente a partir do HANA nas instâncias na instância grande do HANA para uma VM no Azure que aloja partilhas NFS.
- Copiar as cópias de segurança grandes ou outros ficheiros de unidades de instância grande do HANA para espaço de disco gerido no Azure.

Utilize uma rede virtual separada para as VMs que gerir o armazenamento do anfitrião. Essa organização evita os efeitos de arquivo grande ou transferência de dados de instância grande do HANA para o Azure no gateway de rede virtual que serve de VMs que executam a camada de aplicação SAP. 

Para uma arquitetura de rede mais escalonável:

- Tire partido de várias redes virtuais numa camada de aplicação SAP única e maior.
- Implemente uma rede virtual separada para cada sistema SAP implementado, em comparação com a combinação desses sistemas SAP em sub-redes separadas na mesma rede virtual.

 Uma arquitetura de rede mais escalonável para o SAP HANA no Azure (instâncias grandes):

![Implementar a camada de aplicação SAP através de várias redes virtuais](./media/hana-overview-architecture/image4-networking-architecture.png)

A figura mostra a camada de aplicação SAP ou a componentes, implantados através de várias redes virtuais. Esta configuração introduziu a sobrecarga de latência inevitável que ocorreram durante a comunicação entre as aplicações alojadas nessas redes virtuais. Por predefinição, o tráfego de rede entre VMs localizadas na rota de redes virtuais diferentes por meio dos routers de limite de enterprise nesta configuração. É a forma de otimizar e reduzir a latência de comunicação entre duas redes virtuais através do peering de redes virtual na mesma região. Este método funciona mesmo que essas redes virtuais estejam em subscrições diferentes. Com o peering de rede virtual, a comunicação entre VMs em duas redes virtuais diferentes pode utilizar o backbone de rede do Azure para comunicar diretamente entre si. Latência mostra como se as VMs estão na mesma rede virtual. O tráfego que reflete os intervalos de endereços IP que estão ligados através do gateway de rede virtual do Azure é encaminhado através do gateway de rede virtual individual da rede virtual. 

Para obter mais informações sobre o peering de rede virtual, consulte [peering de rede Virtual](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview).


### <a name="routing-in-azure"></a>Encaminhamento do Azure

Três considerações de encaminhamento de rede são importantes para o SAP HANA no Azure (instâncias grandes):

* SAP HANA no Azure (instâncias grandes) podem ser acedido apenas por meio de VMs e a ligação dedicada do ExpressRoute, não diretamente a partir de locais. Acesso direto no local para as unidades de instância grande do HANA, conforme fornecido pela Microsoft, não é possível imediatamente. As restrições de encaminhamento transitivas são devido à arquitetura de rede do Azure atual utilizada para a instância grande do SAP HANA. Alguns clientes da administração e todas as aplicações que precisam direcionar o acesso, como o SAP solução Manager em execução no local, não é possível ligar à base de dados SAP HANA.

* Se tiver implementadas em duas regiões do Azure diferentes para recuperação após desastre de unidades de instância grande do HANA, as restrições de encaminhamento transitórias se aplicam. Em outras palavras, os endereços IP de uma unidade de instância grande do HANA numa única região (por exemplo, e.u.a. Centro-Oeste) não são encaminhados para uma unidade de instância grande do HANA implementada noutra região (por exemplo, E.u.a. Leste). Esta restrição é independente do uso de peering entre regiões ou ligar os circuitos do ExpressRoute que se ligam a unidades de instância grande do HANA para redes virtuais entre a rede do Azure. Para uma representação gráfica, consulte a figura na secção "Instância grande do HANA de utilização de unidades em várias regiões." Esta restrição, que vem com a arquitetura de implementada, proíbe o uso imediato do HANA System Replication como funcionalidade de recuperação após desastre.

* SAP HANA nas unidades do Azure (instâncias grandes) tem um endereço IP atribuído o intervalo de endereços de conjunto IP de servidor enviada. Para obter mais informações, consulte [infraestrutura de SAP HANA (instâncias grandes) e a conectividade no Azure](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Este endereço IP está acessível através de subscrições do Azure e ExpressRoute que se liga a redes virtuais para HANA no Azure (instâncias grandes). O endereço IP atribuído de que o intervalo de endereços do conjunto IP do servidor é diretamente atribuído para a unidade de hardware. Ele possui *não* atribuída através de NAT, como era o caso nas Implantações primeiro desta solução. 

> [!NOTE] 
> Para superar a restrição no encaminhamento transitório, conforme explicado nos primeiros itens de lista de dois, use componentes adicionais para o encaminhamento. Componentes que podem ser usados para superar a restrição podem ser:

> * Um proxy inverso para encaminhar os dados, de e para. Por exemplo, F5 BIG-IP, o NGINX com o Gestor de tráfego seja implementado no Azure como uma solução de encaminhamento de tráfego/firewall virtual.
> * Usando [regras de IPTables](http://www.linuxhomenetworking.com/wiki/index.php/Quick_HOWTO_%3a_Ch14_%3a_Linux_Firewalls_Using_iptables#.Wkv6tI3rtaQ) numa VM do Linux para ativar o encaminhamento entre localizações no local e unidades de instância grande do HANA ou entre unidades de instância grande do HANA em regiões diferentes.

> Lembre-se de que o suporte para soluções personalizadas que envolvem de terceiros e de implementação de aplicações de rede ou IPTables não foi fornecido pela Microsoft. Suporte deve ser fornecido pelo fornecedor do componente utilizado ou o integrador de. 

### <a name="internet-connectivity-of-hana-large-instance"></a>Conectividade de Internet da instância grande do HANA
Instância grande do HANA faz *não* tem conectividade internet direta. Por exemplo, esta limitação pode restringir a capacidade de registar a imagem do SO diretamente com o fornecedor do sistema operacional. Talvez precise trabalhar com o seu servidor local da ferramenta de gestão de subscrição do SUSE Linux Enterprise Server ou o Gestor de subscrições do Red Hat Enterprise Linux.

### <a name="data-encryption-between-vms-and-hana-large-instance"></a>Encriptação de dados entre VMs e instâncias grandes do HANA
Dados transferidos entre a instância grande do HANA e VMs não estão encriptados. No entanto, apenas para a troca entre o lado do HANA DBMS e as aplicações baseadas em JDBC/ODBC, pode ativar a encriptação de tráfego. Para obter mais informações, consulte [esta documentação pela SAP](http://help-legacy.sap.com/saphelp_hanaplatform/helpdata/en/db/d3d887bb571014bf05ca887f897b99/content.htm?frameset=/en/dd/a2ae94bb571014a48fc3b22f8e919e/frameset.htm&current_toc=/en/de/ec02ebbb57101483bdf3194c301d2e/plain.htm&node_id=20&show_children=false).

### <a name="use-hana-large-instance-units-in-multiple-regions"></a>Utilizar unidades de instância grande do HANA em várias regiões

Talvez tenha motivos para implantar o SAP HANA no Azure (instâncias grandes) em várias regiões do Azure diferente para recuperação após desastre. Talvez deseja acessar a instância grande do HANA de cada uma das VMs implementadas em redes virtuais diferentes nas regiões. Os endereços IP atribuídos a diferentes unidades de instância grande do HANA não serão propagados para além de redes virtuais que estejam diretamente ligadas através do seu gateway para as instâncias. Como resultado, uma pequena alteração é introduzida para o design de rede virtual. Um gateway de rede virtual pode lidar com quatro diferentes circuitos do ExpressRoute fora de routers de periferia de empresa diferente. Cada rede virtual que está ligada a um dos carimbos de data / instância grande pode ser ligada para o carimbo de data / instância grande noutra região do Azure.

![Rede virtual ligado a carimbos de instâncias grandes do Azure em diferentes regiões do Azure](./media/hana-overview-architecture/image8-multiple-regions.png)

A figura mostra como as redes virtuais diferentes em ambas as regiões estão ligadas a dois circuitos do ExpressRoute diferentes que são utilizados para ligar ao SAP HANA no Azure (instâncias grandes) em ambas as regiões do Azure. As recém-lançado as ligações são as linhas vermelhas retangulares. Com estas ligações, de redes virtuais, as VMs em execução em uma dessas redes virtuais podem aceder a cada um dos diferentes unidades de instância grande do HANA implementadas em duas regiões. Conforme mostrado na figura, presume que tem duas ligações do ExpressRoute no local para as duas regiões do Azure. Essa disposição é recomendada por motivos de recuperação após desastre.

> [!IMPORTANT] 
> Se utilizou vários circuitos do ExpressRoute, prefixação como caminho e as definições de BGP de preferência Local devem ser utilizadas para garantir que o encaminhamento adequado de tráfego.


