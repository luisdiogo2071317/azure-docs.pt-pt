---
title: Considerações para a implementação de DBMS de máquinas virtuais do Azure para a carga de trabalho do SAP | Documentos da Microsoft
description: Considerações para a implementação de DBMS de máquinas virtuais do Azure para a carga de trabalho do SAP
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
ms.date: 09/06/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3948c226f13f0ff358f9ca467f19cf0e48795911
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/19/2018
ms.locfileid: "49429893"
---
# <a name="considerations-for-azure-virtual-machines-dbms-deployment-for-sap-workload"></a>Considerações para a implementação de DBMS de máquinas virtuais do Azure para a carga de trabalho do SAP
[1114181]:https://launchpad.support.sap.com/#/notes/1114181
[1409604]:https://launchpad.support.sap.com/#/notes/1409604
[1597355]:https://launchpad.support.sap.com/#/notes/1597355
[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[1984787]:https://launchpad.support.sap.com/#/notes/1984787
[1999351]:https://launchpad.support.sap.com/#/notes/1999351
[2002167]:https://launchpad.support.sap.com/#/notes/2002167
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2039619]:https://launchpad.support.sap.com/#/notes/2039619
[2069760]:https://launchpad.support.sap.com/#/notes/2069760
[2171857]:https://launchpad.support.sap.com/#/notes/2171857
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2191498]:https://launchpad.support.sap.com/#/notes/2191498
[2233094]:https://launchpad.support.sap.com/#/notes/2233094
[2243692]:https://launchpad.support.sap.com/#/notes/2243692
[deployment-guide]:deployment-guide.md 
[deployment-guide-3]:deployment-guide.md#b3253ee3-d63b-4d74-a49b-185e76c4088e 
[planning-guide]:planning-guide.md 

[Logo_Linux]:media/virtual-machines-shared-sap-shared/Linux.png
[Logo_Windows]:media/virtual-machines-shared-sap-shared/Windows.png


[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

Este guia faz parte da documentação sobre como implementar e implantar o software SAP no Microsoft Azure. Antes de ler este guia, leia os [guia de implementação e planeamento][planning-guide]. Este documento aborda os aspectos de genérico de implementação de sistemas de relacionados com o SAP DBMS em máquinas de virtuais de Azure do Microsoft (VMs) com a infraestrutura do Azure como um capacidades do serviço (IaaS).

O documento complementa a documentação de instalação de SAP e SAP Notes, que representam os recursos principais para instalações e implementações de SAP software em determinadas plataformas.

Neste documento, são introduzidas considerações da execução de sistemas relacionados com o SAP DBMS em VMs do Azure. Existem algumas referências para sistemas específicos do DBMS neste capítulo. Em vez disso, os sistemas DBMS específicos são tratados dentro deste documento, depois deste documento.

## <a name="definitions-upfront"></a>Definições de início
Em todo o documento, são utilizados os seguintes termos:

* IaaS: Infraestrutura como serviço.
* PaaS: Plataforma como um serviço.
* SaaS: Software como serviço.
* Componente SAP: Um SAP aplicativos individuais como ECC, BW, Gestor de solução ou EP.  Componentes SAP podem basear-se em tecnologias tradicionais de ABAP ou Java ou um aplicativo de não com base em NetWeaver, como objetos comerciais.
* Ambiente de SAP: um ou mais componentes SAP logicamente agrupados para executar uma função de negócio, como o desenvolvimento, QAS, treinamento, DR ou de produção.
* Ambiente SAP: Esse termo refere-se para os ativos SAP inteiros num cliente paisagem IT. A paisagem SAP inclui todos os ambientes de não produção e produção.
* Sistema SAP: A combinação de camada do DBMS e camada de aplicativo de, por exemplo, um sistema de desenvolvimento SAP ERP, sistema de teste de SAP BW, sistema de produção do SAP CRM, etc. Nas implementações do Azure, não é suportada para dividir estas duas camadas entre no local e o Azure. Isso significa que um sistema SAP é implementado no local ou está implementado no Azure. No entanto, pode implantar os sistemas diferentes de um ambiente SAP no Azure ou no local. Por exemplo, pode implementar o desenvolvimento de CRM do SAP e testar sistemas no Azure, mas a SAP CRM produção sistema no local.
* Em vários locais: Descreve um cenário em que as VMs são implementadas para uma subscrição do Azure que tem conectividade do ExpressRoute entre o datacenter(s) no local e o Azure, múltiplos sites ou site a site. Documentação do Azure em comum, esses tipos de implementações também são descritos como cenários de vários locais. O motivo para a ligação é para expandir os domínios no local, Active Directory no local e o DNS no local para o Azure. O Panorama de no local é expandido para recursos do Azure da subscrição. Com esta extensão, as VMs podem ser parte do domínio no local. Os utilizadores de domínio do domínio no local podem aceder aos servidores e podem executar serviços nessas VMS (como o DBMS serviços). Resolução de nomes e de comunicação entre VMs implementadas no local e as VMs implementadas no Azure, é possível. Este cenário é o cenário mais comum para a implementação de ativos SAP no Azure. Para obter mais informações, consulte [planear e conceber para o gateway VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-plan-design).

> [!NOTE]
> Implementações em vários locais dos sistemas SAP em máquinas virtuais do Azure com sistemas SAP são membros de um domínio no local são suportadas para sistemas SAP de produção. Configurações em vários locais são suportadas para a implementação de partes ou concluir cenários SAP no Azure. Até mesmo executar o ambiente completo do SAP no Azure requer ter essas VMs a fazer parte do domínio no local e o AD/LDAP. Nas versões anteriores da documentação, cenários de TI híbrida foram mencionados, em que o termo *híbrida* está enraizada no fato de que existe uma conectividade em vários locais entre no local e o Azure. Neste caso *híbrida* também significa que as VMs no Azure fazem parte do Active Directory no local.
> 
> 

Alguma documentação de Microsoft descreve os cenários de em vários locais de forma um pouco diferente, especialmente para configurações de HA do DBMS. No caso dos documentos relacionados com o SAP, o cenário em vários locais se resume a ter um site a site ou privadas [ExpressRoute](https://azure.microsoft.com/services/expressroute/) conectividade e o fato de que o ambiente SAP é distribuído entre no local e o Azure.

## <a name="resources"></a>Recursos
O são lançadas a vários artigos na carga de trabalho SAP no Azure.  Recomenda-se iniciar em [carga de trabalho SAP no Azure - introdução ao](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/get-started) e, em seguida, escolha a área de interesse

As seguintes notas de SAP estão relacionadas com o SAP no Azure em relação à área de abrangidas neste documento:

| Número de nota | Cargo |
| --- | --- |
| [1928533] |Aplicações SAP no Azure: produtos suportados e a VM do Azure tipos |
| [2015553] |SAP no Microsoft Azure: pré-requisitos do suporte |
| [1999351] |Resolução de problemas avançada a monitorização do Azure para SAP |
| [2178632] |Chave de métricas de monitorização para o SAP no Microsoft Azure |
| [1409604] |Virtualização no Windows: avançada de monitorização |
| [2191498] |SAP no Linux com o Azure: avançada de monitorização |
| [2039619] |Aplicações SAP no Microsoft Azure utilizando a base de dados do Oracle: produtos suportados e versões |
| [2233094] |DB6: Aplicações de SAP no Azure com o IBM DB2 para Linux, UNIX e Windows - informações adicionais |
| [2243692] |Linux no Microsoft Azure (IaaS) VM: problemas de licença do SAP |
| [1984787] |SUSE LINUX Enterprise Server 12: Observações de instalação |
| [2002167] |Red Hat Enterprise Linux 7.x: instalação e atualização |
| [2069760] |Instalação de SAP do Oracle Linux 7.x e atualização |
| [1597355] |Recomendação de espaço de comutação para Linux |
| [2171857] |Base de dados Oracle 12C - suporte de sistema de ficheiros no Linux |
| [1114181] |Oracle Database 11g - suporte de sistema de ficheiros no Linux |


Leia também a [SCN Wiki](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) que contém todas as anotações do SAP para o Linux.

Deve ter um conhecimento prático sobre a arquitetura do Microsoft Azure e como as máquinas virtuais do Microsoft Azure são implementados e em funcionamento. Pode encontrar mais informações em [documentação do Azure](https://docs.microsoft.com/azure/).

Embora discutir IaaS, em geral a instalação do Windows, Linux e DBMS e configuração são, essencialmente, igual a qualquer máquina virtual ou máquina bare-metal, terá de instalar no local. No entanto, existem alguns gestão de arquitetura e o sistema suas decisões de implementação, que são diferentes quando a utilização de IaaS do Azure. O objetivo deste documento é explicar as diferenças de gestão do sistema que precisa estar preparado para quando utilizar IaaS do Azure e específicos de arquitetura.


## <a name="65fa79d6-a85f-47ee-890b-22e794f51a64"></a>Estrutura de armazenamento de uma VM para implementações de RDBMS
Para seguir este capítulo, é necessário compreender o que foi apresentado no [isso] [ deployment-guide-3] capítulo o [guia de implementação][deployment-guide]. Dados de conhecimento sobre a série de VM diferente e suas diferenças e as diferenças do Azure Standard e [o armazenamento Premium](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage) devem ser compreendidos e conhecido antes de ler este capítulo.

Em termos de armazenamento do Azure para VMs do Azure, deve estar familiarizado com os artigos:

- [Sobre o armazenamento de discos para VMs do Windows do Azure](https://docs.microsoft.com/azure/virtual-machines/windows/about-disks-and-vhds)
- [Sobre o armazenamento de discos para VMs Linux do Azure](https://docs.microsoft.com/azure/virtual-machines/linux/about-disks-and-vhds) 

Numa configuração básica, normalmente, recomendamos uma estrutura de implementação em que o sistema operativo, o DBMS e a eventual binários SAP estão separados dos arquivos de banco de dados. Por conseguinte, recomendamos que os sistemas SAP em execução em máquinas virtuais do Azure para que o VHD base (ou disco) instalados com o sistema operativo, executáveis de sistema de gestão de base de dados e executáveis SAP. Os ficheiros de dados e de registo do DBMS são armazenados no armazenamento do Azure (armazenamento Standard ou Premium) em discos separados e anexados como discos lógicos para a imagem de sistema de operativo Azure VM original. Especialmente em Implantações de Linux, pode haver diferentes recomendações documentadas. Sobretudo em torno de SAP HANA.  

Ao planejar seu layout de disco, tem de encontrar o melhor equilíbrio entre os seguintes itens:

* O número de ficheiros de dados.
* O número de discos que contêm os ficheiros.
* As quotas IOPS de um único disco.
* A taxa de transferência de dados por disco.
* O número de discos de dados adicionais possíveis por tamanho da VM.
* O débito global do armazenamento uma VM pode fornecer.
* A latência podem fornecer diferentes tipos de armazenamento do Azure.
* SLAs VM

Azure impõe uma quota IOPS por disco de dados. Estas quotas são diferentes para discos alojados no armazenamento padrão do Azure e o armazenamento Premium. Latência de e/s também é diferente entre os dois tipos de armazenamento.  Com o armazenamento Premium a distribuição de fatores melhor latência de e/s. Cada um dos diferentes tipos VM tem um número limitado de discos de dados que pode anexar. Outra restrição é que apenas determinados tipos VM podem tirar partido do armazenamento Premium do Azure. Como resultado, a decisão para um determinado tipo VM pode não apenas ser orientada pelos requisitos de CPU e memória, mas também por IOPS, latência e disco requisitos de débito que normalmente são dimensionados com o número de discos ou o tipo de discos de armazenamento Premium. Especialmente com o armazenamento Premium o tamanho de um disco pode de ser ditado também pelo número de IOPS e débito de que tem de ser possível através de cada disco.

> [!NOTE]
> Para implementações do DBMS, é altamente recomendada a utilização do armazenamento Premium para quaisquer dados, registo de transações ou arquivos de Refazer. Deste modo, não importa se deseja implantar sistemas de não produção ou de produção.

> [!NOTE]
> Para se beneficiar do Azure do exclusivo [único SLA de VM](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_8/) todos os discos ligados têm de ser do tipo para o armazenamento Premium do Azure, incluindo o VHD base.
>

A colocação de ficheiros de base de dados e ficheiros de registo/Refazer e o tipo de armazenamento do Azure utilizado, devem ser definidos pelos requisitos de IOPS, débito e latência. Para ter suficiente IOPS, poderá ser forçado a tirar partido de vários discos ou utilizar um disco de armazenamento Premium maior. Em caso de a utilizar vários discos, criaria uma faixa de software em todos os discos que contêm os ficheiros de dados ou ficheiros de registo/Refazer. Nesses casos, o IOPS e o débito de disco SLAs dos discos subjacentes do armazenamento Premium ou os discos de IOPS dos armazenamento Standard do Azure alcançáveis máximos são acumulativos para o conjunto resultante do stripe. 

Como já declarado, se seu requisito de IOPS excede o que pode fornecer um único VHD, tem de equilibrar o número de IOPS necessário para os ficheiros de base de dados num número de VHDs. A forma mais fácil para distribuir a carga IOPS entre discos é criar uma faixa de software sobre os discos diferentes. Em seguida, coloca um número de ficheiros de dados do SAP DBMS em LUNS criados fora do stripe de software. o número de discos no stripe é orientado por demanda, as necessidades de débito de disco e volume IOPs demandas. 


- - -
> ![Windows][Logo_Windows] Windows
> 
> Recomendamos que utilize espaços de armazenamento do Windows para criar os conjuntos de faixa entre vários VHDs do Azure. É recomendado que utilize, pelo menos, Windows Server 2012 R2 ou Windows Server 2016.
> 
> ![Linux][Logo_Linux] Linux
> 
> Apenas MDADM LVM (Gestor de volumes lógicos) são suportados e para criar um RAID de software no Linux. Para obter mais informações, leia os artigos seguintes:
> 
> - [Configurar o RAID de Software no Linux](https://docs.microsoft.com/azure/virtual-machines/linux/configure-raid) usando MDADM
> - [Configurar LVM numa VM do Linux no Azure](https://docs.microsoft.com/azure/virtual-machines/linux/configure-lvm) usando LVM
> 
> 

- - -
 
> [!NOTE]
> Uma vez que o armazenamento do Azure é manter três imagens dos VHDs, ele não faz sentido para configurar uma redundância quando repartição de. É apenas necessária para configurar a repartição, deste modo, que o e/s são introdução distribuídas sobre os VHDs de diferentes.
>

### <a name="managed-or-non-managed-disks"></a>Discos geridos ou não geridos
Uma conta de armazenamento do Azure é não apenas uma construção administrativa, mas também um sujeito de limitações. As limitações são diferentes entre Accounst de armazenamento Standard do Azure e contas de armazenamento Premium do Azure. As capacidades exatas e limitações são listadas no artigo [metas de desempenho e escalabilidade do armazenamento do Azure](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets)

Armazenamento do Azure Standard, é importante, lembre-se de que existe um limite de IOPS por conta de armazenamento (linha que contém **taxa Total de pedidos** no artigo [escalabilidade do armazenamento do Azure e metas de desempenho](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets)). Além disso, existe um limite inicial do número de contas de armazenamento por subscrição do Azure. Portanto, precisa balancear VHDs para maior paisagem SAP em contas de armazenamento diferentes para evitar atingir os limites de uma dessas contas de armazenamento. Um trabalho tedioso quando estamos a falar sobre algumas das máquinas virtuais centenas com mais de mil VHDs. 

Uma vez que não é recomendado a utilizar o armazenamento padrão do Azure para implementações de DBMS em conjunto com cargas de trabalho do SAP, referências e recomendações para o armazenamento do Azure Standard estão limitadas a neste breve [artigo](https://blogs.msdn.com/b/mast/archive/2014/10/14/configuring-azure-virtual-machines-for-optimal-storage-performance.aspx)

Para evitar o trabalho administrativo de planeamento e implementação de VHDs em diferentes contas de armazenamento do Azure, a Microsoft introduziu o que é chamado [Managed Disks](https://azure.microsoft.com/services/managed-disks/) em 2017. Discos geridos estão disponíveis para o armazenamento padrão do Azure, bem como o armazenamento Premium do Azure. Principais vantagens dos Managed Disks em comparação com a lista de discos não geridos, como:

- Para os Managed Disks, o Azure distribui os VHDs de diferentes contas de armazenamento diferentes automaticamente no momento da implementação e, assim, evita a atingir os limites de uma conta de armazenamento do Azure em termos de IOPS, débito de e/s e volume de dados.
- Utilizar Managed Disks, o armazenamento do Azure está a respeitar os conceitos de conjuntos de disponibilidade do Azure e com que implementa o base VHD e disco anexado de uma VM em diferentes domínios de falha e de atualização se a VM faz parte de um conjunto de disponibilidade do Azure.


> [!IMPORTANT]
> Tendo em conta as vantagens do Managed Disks do Azure, é altamente recomendado para utilizar Managed Disks do Azure para as suas implementações do DBMS e implementações de SAP em geral.
>

Para converter de discos não para os discos geridos, consulte os artigos:

- [Converter uma máquina virtual do Windows de discos não geridos para managed disks](https://docs.microsoft.com/azure/virtual-machines/windows/convert-unmanaged-to-managed-disks)
- [Converter uma máquina virtual Linux de discos não geridos para managed disks](https://docs.microsoft.com/azure/virtual-machines/linux/convert-unmanaged-to-managed-disks)


### <a name="c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f"></a>Colocação em cache para VMs e os discos de dados
Ao montar discos para VMs, pode escolher se o tráfego de e/s entre a VM e esses discos localizados no armazenamento do Azure são armazenados em cache. O armazenamento Premium e Standard do Azure utilizam duas tecnologias diferentes para este tipo de cache. 

As recomendações abaixo são supondo que essas características de e/s para DBMS padrão:

- Serão lidos principalmente a carga de trabalho em relação a arquivos de dados de uma base de dados. Essas leituras são desempenho crítico para o sistema DBMS
- Escrever contra os ficheiros de dados se verificarem em picos com base em pontos de verificação ou um fluxo constante. No entanto, apresentou uma média ao longo do dia, as gravações são mais pequeno do que as leituras. Em diferentes dos padrões convencionais leituras de ficheiros de dados, essas escritas são assíncronas e não estão obstruindo de quaisquer transações de utilizador.
- Existem mínimo de leituras da transação arquivos de log ou refazer. As exceções são grandes e/ss quando efetuar cópias de segurança de registo de transações. 
- Principal de carga em relação a arquivos de log de transação ou refazer é escrita. Depende da natureza da carga de trabalho, pode ter e/ss tão pequena como 4 KB ou em outros casos de tamanhos de e/s de 1 MB ou mais.
- Todas as gravações precisam ser mantidos no disco de forma fiável

Armazenamento do Azure Standard, os tipos de cache de possíveis são:

* Nenhuma
* Leitura
* Leitura/Escrita

Para obter um desempenho consistente e determinista, deve definir a colocação em cache no armazenamento padrão do Azure para todos os discos que contêm **relacionados com o DBMS arquivos de dados, ficheiros de registo/Refazer e tabela espaço para 'NONE'**. A colocação em cache da base de VHD pode permanecer com o padrão.

Armazenamento do Azure Premium, existem as seguintes opções de colocação em cache:

* Nenhuma
* Leitura 
* Leitura/escrita 
* Nenhum + acelerador de escrita (apenas para VMs de série M do Azure)
* Leitura + acelerador de escrita (apenas para VMs de série M do Azure)

Recomendação para o armazenamento Premium do Azure consiste em tirar partido **colocação em cache para ficheiros de dados de leitura** da base de dados do SAP e escolha **sem colocação em cache para os discos dos ficheiros de registo**.

Para implementações de série M, recomendamos utilizar o acelerador de escrita do Azure para a sua implementação do DBMS. Para obter detalhes, restrições e a implantação do acelerador de escrita do Azure, consulte o documento [acelerador de escrita](https://docs.microsoft.com/azure/virtual-machines/windows/how-to-enable-write-accelerator). 


### <a name="azure-non-persistent-disks"></a>Discos não persistentes do Azure
VMs do Azure oferecem discos não persistentes depois de uma VM é implementada. Em caso de um reinício VM, todo o conteúdo nessas unidades vai ser eliminado. Por conseguinte, é uma vez que os ficheiros de dados e ficheiros de registo/Refazer de bases de dados devem sob nenhuma circunstância estar localizados nessas unidades não persistente. Pode haver exceções para algumas das bases de dados, onde estas unidades não persistente podem ser adequadas para tempdb e temp tablespaces. No entanto, evite utilizar essas unidades para VMs de série, uma vez que essas unidades não persistente são limitadas em débito com essa família VM. Para obter mais detalhes, leia o artigo [Noções básicas sobre a unidade temporária em VMs do Windows no Azure](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)

- - -
> ![Windows][Logo_Windows] Windows
> 
> Unidade D:\ numa VM do Azure é uma unidade não persistente, que é suportada por alguns discos locais no nó de computação do Azure. Como é não persistente, isso significa que todas as alterações feitas para o conteúdo na unidade D:\ é perdido quando a VM é reiniciada. Por "todas as alterações", como arquivos armazenados, diretórios criados, aplicativos instalados, etc.
> 
> ![Linux][Logo_Linux] Linux
> 
> As VMs do Linux do Azure automaticamente montar uma unidade durante /mnt/resource que é uma unidade não persistente, apoiada por discos locais no nó de computação do Azure. Como é não persistente, isso significa que todas as alterações efetuadas ao conteúdo em /mnt/resource são perdidas quando a VM é reiniciada. Por quaisquer alterações, como arquivos armazenados, diretórios criados, aplicativos instalados, etc.
> 
> 

- - -



### <a name="10b041ef-c177-498a-93ed-44b3441ab152"></a>Resiliência de armazenamento do Microsoft Azure
Armazenamento do Microsoft Azure armazena o VHD base (com o sistema operacional) e discos ligados ou os BLOBs em, pelo menos, três nós de armazenamento separada. Esse fato é chamado de armazenamento com redundância Local (LRS). Predefinição para todos os tipos de armazenamento no Azure é LRS. 

Existem vários mais redundância métodos, que são todos descritos no artigo [replicação de armazenamento do Azure](https://docs.microsoft.com/azure/storage/common/storage-redundancy?toc=%2fazure%2fstorage%2fqueues%2ftoc.json).

> [!NOTE]
>A partir de armazenamento Premium do Azure, que é o tipo recomendado de armazenamento para VMs do DBMS e os discos que armazenam ficheiros de base de dados e de registo/Refazer, o método apenas disponível é LRS. Como resultado, tem de configurar métodos de base de dados, como o SQL Server Always On, proteção de dados Oracle ou HANA System Replication para ativar a replicação de dados de base de dados para outra região do Azure ou noutra zona de disponibilidade do Azure.


> [!NOTE]
> A utilização de armazenamento Georredundante como disponível com o armazenamento padrão do Azure não é recomendada para implementações do DBMS, uma vez que tem impacto no desempenho grave e não respeitará a ordem de escrita em VHDs de diferentes que estão ligados a uma VM. O fato de não aceitar a ordem de escrita em VHDs de diferentes pode ser um alto potencial terminar em bases de dados inconsistentes no lado do destino de replicação se os ficheiros de base de dados e de registo/Refazer são distribuídos por vários VHDs (como em grande parte o caso) na origem de lado VM.

 

## <a name="vm-node-resiliency"></a>Resiliência de nó VM
A plataforma do Azure oferece vários SLAs diferentes para as VMs. Os detalhes exatos podem ser encontrados na versão mais recente do [SLA para máquinas virtuais](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_8/). Uma vez que a camada do DBMS, normalmente, é uma parte crítica da disponibilidade de um sistema SAP, tem de tornar-se familiarizado com os conceitos de conjuntos de disponibilidade, zonas de disponibilidade e eventos de manutenção. Os artigos que descrevem todos esses conceitos é [gerir a disponibilidade das máquinas de virtuais do Windows no Azure](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability) e [gerir a disponibilidade das máquinas virtuais do Linux no Azure](https://docs.microsoft.com/azure/virtual-machines/linux/manage-availability).  

A recomendação mínima para cenários do DBMS de produção com carga de trabalho do SAP é:

- Implemente duas VMs num conjunto de disponibilidade separados na mesma região do Azure.
- Estas duas VMs seriam executado na mesma VNet do Azure e seriam ter NICs ligadas fora das sub-redes da mesmas.
- Utilize os métodos de base de dados para manter um modo de espera ativo com a segunda VM. Métodos podem ser SQL Server Always On, proteção de dados Oracle ou HANA System Replication.

Além disso, pode implementar uma terceira VM noutra região do Azure e utilize os mesmos métodos de base de dados para fornecer uma réplica assíncrona noutra região do Azure.

A forma de configurar conjuntos de disponibilidade do Azure é demonstrada neste [tutorial](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets).



## <a name="azure-network-considerations"></a>Considerações de rede do Azure 
Nas implementações de SAP em grande escala, recomenda-se que está a utilizar o plano gráfico de [Datacenter Virtual do Azure](https://docs.microsoft.com/azure/architecture/vdc/networking-virtual-datacenter) para suas atribuições de configuração e as permissões e funções de VNet para diferentes partes da sua organização.

Existem várias práticas recomendadas, o que resultou em centenas de implementações de cliente:

- A aplicação SAP de vnet ou Vnets é implementado no, não tem acesso à Internet.
- A base de dados, as VMs estão em execução na mesma VNet como a camada de aplicativo.
- As VMs dentro da VNet tem uma alocação de estática do endereço IP privado. Consulte o artigo [tipos e métodos de alocação no Azure de endereços IP](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm) como referência.
- Existem restrições de encaminhamento de e para as VMs do DBMS **não** definido com firewalls instalados nas VMs locais a DBMS. Em vez disso, o encaminhamento de tráfego é definido com [grupos de segurança de rede (NSG) do Azure](https://docs.microsoft.com/azure/virtual-network/security-overview)
- Para efeitos de separar e isolar o tráfego para a VM do DBMS, atribuir diferentes NICs à VM. Em que todas as NICs tem um endereço IP diferente e todas as NICs é um valor atribuído a uma sub-rede de VNet diferente, que novamente tem diferentes regras NSG. Tenha em atenção que o isolamento ou a separação do tráfego de rede é apenas uma medida para o encaminhamento e não permite definir quotas para o débito de rede.

> [!NOTE]
> Deve atribuir endereços IP estáticos através do Azure meios para vNICs individuais. Não deve atribuir endereços IP estáticos no SO convidado para um vNIC. Alguns serviços do Azure como o serviço de cópia de segurança do Azure confiam no fato de que, no mínimo a vNIC principal está definida para DHCP e não para endereços IP estáticos. Consulte também o documento [cópia de segurança de máquina virtual de resolução de problemas do Azure](https://docs.microsoft.com/azure/backup/backup-azure-vms-troubleshoot#networking). Se tiver de atribuir vários endereços IP estáticos a uma VM, terá de atribuir múltiplos vNICs a uma VM.
>
>

Utilizar duas VMs para sua implementação de DBMS dentro de um conjunto de disponibilidade do Azure e ainda um encaminhamento separado para a camada de aplicação SAP e o tráfego de gerenciamento e operações para as duas VMs DBMS em produção, o diagrama aproximado teria o seguinte aspeto:

![Diagrama de duas VMs em duas sub-redes](./media/virtual-machines-shared-sap-deployment-guide/general_two_dbms_two_subnets.PNG)


### <a name="azure-load-balancer-for-redirecting-traffic"></a>Balanceador de carga do Azure para redirecionar o tráfego
A utilização de endereços IP virtual privados utilizado em funcionalidades, como a replicação do SQL Server Always On ou sistema HANA requer a configuração de um balanceador de carga do Azure. O Balanceador de carga do Azure é capaz de através de portas da sonda para determinar o nó ativo do DBMS e encaminhar o tráfego exclusivamente para esse nó de base de dados ativa. Em caso de uma ativação pós-falha do nó da base de dados, não é necessário para a aplicação SAP reconfigurar. Em vez disso, as arquiteturas de aplicativos mais comuns do SAP restabelecerá contra o endereço IP virtual privado. Enquanto isso o Balanceador de carga do Azure reagiram sobre a ativação pós-falha de nó ao redirecionar o tráfego em comparação ao endereço IP virtual privado para o segundo nó.

O Azure oferece dois diferentes [SKUs de Balanceador de carga](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview). Um SKU básico e um SKU standard. A menos que queira implemente nas zonas de disponibilidade do Azure, o SKU do Balanceador de carga básico faz bem. 

É o tráfego entre as VMs do DBMS e a camada de aplicação SAP sempre encaminhados através do Balanceador de carga do Azure o tempo todo? A resposta depende de como configurar o Balanceador de carga. No momento, o tráfego de entrada para a VM do DBMS sempre será encaminhado através do Balanceador de carga do Azure. A saída tráfego rota da DBMS VM para a camada de aplicativo que VM depende da configuração de Balanceador de carga do Azure. O Balanceador de carga oferece uma opção de DirectServerReturn. Se essa opção estiver configurada, o tráfego direcionado da DBMS VM para a camada de aplicação SAP serão **não** ser encaminhado através do Balanceador de carga do Azure. Em vez disso, será diretamente vá para a camada de aplicativo. Se DirectServerReturn não estiver configurada, o tráfego de retorno para a camada de aplicação SAP é encaminhado através do Balanceador de carga do Azure

É recomendado configurar DirectServerReturn em combinação com os balanceadores de carga do Azure são posicionados entre a camada de aplicação SAP e a camada DBMS para reduzir a latência de rede entre as duas camadas

Um exemplo de como configurar essa configuração é publicado em torno do SQL server sempre na [este artigo](https://docs.microsoft.com/azure/virtual-machines/windows/sqlclassic/virtual-machines-windows-classic-ps-sql-int-listener).

Se optar por utilizar modelos JSON do github publicada como referência para as implementações de infraestrutura SAP no Azure, deve estudar isso [modelo para um sistema de 3 camadas SAP](https://github.com/Azure/azure-quickstart-templates/tree/4099ad9bee183ed39b88c62cd33f517ae4e25669/sap-3-tier-marketplace-image-converged-md). Neste modelo, também pode estudar as definições corretas de Balanceador de carga do Azure.

### <a name="azure-accelerated-networking"></a>Azure Accelerated Networking
Para reduzir ainda mais a latência de rede entre as VMs do Azure, é altamente recomendável escolher a opção de [Azure Accelerated Networking](https://azure.microsoft.com/blog/maximize-your-vm-s-performance-with-accelerated-networking-now-generally-available-for-both-windows-and-linux/) quando implementar VMs do Azure para a carga de trabalho do SAP. Especialmente para a camada de aplicação SAP e a camada do SAP DBMS. 

> [!NOTE]
> Nem todos os tipos VM estiver dando suporte redes aceleradas. O artigo mencionado é listar os tipos VM que suportam redes aceleradas. 
>  

- - -
> ![Windows][Logo_Windows] Windows
> 
> Para Windows, consulte o artigo [criar uma máquina virtual do Windows com redes aceleradas](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-powershell) para compreender os conceitos e o forma como implementar VMs com redes aceleradas
> 
> ![Linux][Logo_Linux] Linux
> 
> Para Linux, leia o artigo [criar uma máquina virtual Linux com redes aceleradas](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli) para obter os detalhes para distribuição de Linux. 
> 
> 

- - -

> [!NOTE]
> No caso do Red Hat, SUSE e Oracle Linux, a redes aceleradas é suportada com as versões recentes. Versões mais antigas, como o RHEL 7.2 ou o SLES 12 SP2 não estiver sustentando Accelerated Networking do Azure 
>  


## <a name="deployment-of-host-monitoring"></a>Implementação do anfitrião de monitorização
Utilização de produção de aplicações SAP em máquinas de virtuais do Azure, o SAP requer a capacidade de obter anfitrião de monitorização dos dados dos anfitriões físicos em execução de máquinas virtuais do Azure. Um nível de patch do agente de anfitrião do SAP específico é necessário que permite que esta capacidade em SAPOSCOL e o agente de anfitrião do SAP. O nível de patch exata está documentado na nota SAP [1409604].

Para obter mais detalhes sobre a implementação de componentes que fornecem dados de anfitrião para SAPOSCOL e o agente de anfitrião do SAP e o gerenciamento de ciclo de vida desses componentes, consulte o [guia de implementação][deployment-guide]


## <a name="next-steps"></a>Próximos Passos
Para obter documentação sobre DBMS específica, consulte estes artigos:

- [Implementação em SQL Server do DBMS para Máquinas Virtuais do Azure para a carga de trabalho SAP](dbms_guide_sqlserver.md)
- [Implementação em Oracle do DBMS para Máquinas Virtuais do Azure para a carga de trabalho SAP](dbms_guide_oracle.md)
- [Implementação em IBM DB2 do DBMS para Máquinas Virtuais do Azure para a carga de trabalho SAP](dbms_guide_ibm.md)
- [Implementação em SAP ASE do DBMS para Máquinas Virtuais do Azure para a carga de trabalho SAP](dbms_guide_sapase.md)
- [SAP maxDB, a Cache em direto e a implementação de servidor de conteúdo no Azure](dbms_guide_maxdb.md)
- [Manual de operações do SAP HANA no Azure](hana-vm-operations.md)
- [Elevada disponibilidade de SAP HANA para máquinas virtuais do Azure](sap-hana-availability-overview.md)
- [Guia de segurança para SAP HANA em máquinas de virtuais do Azure](sap-hana-backup-guide.md)

