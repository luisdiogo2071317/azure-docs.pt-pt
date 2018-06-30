---
title: Operações de SAP HANA no Azure | Microsoft Docs
description: Manual de operações para sistemas de SAP HANA que são implementados em máquinas virtuais do Azure.
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: juergent
manager: patfilot
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 04/24/2018
ms.author: msjuergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 59adc0bccd523818aad1feca2d0cf260f45d899c
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37110372"
---
# <a name="sap-hana-on-azure-operations-guide"></a>SAP HANA no guia de operações do Azure
Este documento fornece orientações para o funcionamento sistemas de SAP HANA que são implementados em máquinas de virtuais (VMs) nativas do Azure. Este documento destina-se não substitui a documentação do SAP padrão, que inclui o seguinte conteúdo:

- [Guia de administração do SAP](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/330e5550b09d4f0f8b6cceb14a64cd22.html)
- [Guias de instalação do SAP](https://service.sap.com/instguides)
- [Notas SAP](https://sservice.sap.com/notes)

## <a name="prerequisites"></a>Pré-requisitos
Para utilizar este guia, tem um conhecimento básico dos seguintes componentes do Azure:

- [Máquinas virtuais do Azure](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-vm)
- [Rede do Azure e redes virtuais](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-virtual-network)
- [Armazenamento do Azure](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-disks)

Para saber mais sobre SAP NetWeaver e outros componentes SAP no Azure, consulte o [SAP no Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/get-started) secção o [documentação do Azure](https://docs.microsoft.com/azure/).

## <a name="basic-setup-considerations"></a>Considerações sobre a configuração básica
As secções seguintes descrevem as considerações de configuração básica para implementação de sistemas de SAP HANA em VMs do Azure.

### <a name="connect-into-azure-virtual-machines"></a>Ligar para máquinas virtuais do Azure
Conforme documentado no [máquinas virtuais do Azure guia de planeamento](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide), existem dois métodos básicos para ligar em VMs do Azure:

- Ligar através da internet e os pontos finais públicos numa VM ir ou na VM que está a executar o SAP HANA.
- Ligar através de um [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal) ou Azure [ExpressRoute](https://azure.microsoft.com/services/expressroute/).

Conetividade site a site através de VPN ou ExpressRoute é necessária para cenários de produção. Este tipo de ligação também é necessário para cenários de não produção feed em cenários de produção em que o software SAP está a ser utilizado. A imagem seguinte mostra um exemplo de conetividade entre sites:

![Conetividade entre sites](media/virtual-machines-shared-sap-planning-guide/300-vpn-s2s.png)


### <a name="choose-azure-vm-types"></a>Escolha tipos de VM do Azure
Os tipos de VM do Azure que podem ser utilizados para cenários de produção estão listados no [documentação SAP para o IAAS](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html). Para cenários de produção não está disponível uma vasta variedade de tipos nativos do VM do Azure.

>[!NOTE]
> Para cenários de não produção, utilize os tipos VM que estão listados no [nota SAP #1928533](https://launchpad.support.sap.com/#/notes/1928533). Para a utilização de VMs do Azure para cenários de produção, consulte SAP HANA certificadas as VMs do SAP publicados [lista de plataformas de IaaS certificadas](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html).

Implemente as VMs no Azure utilizando:

- O portal do Azure.
- Cmdlets do PowerShell do Azure.
- A CLI do Azure.

Também pode implementar uma plataforma de SAP HANA instalada completa nos serviços de VM do Azure através de [plataforma em nuvem do SAP](https://cal.sap.com/). O processo de instalação está descrito em [implementar SAP S/4HANA ou BW/4HANA no Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/cal-s4h) ou com a automatização lançadas [aqui](https://github.com/AzureCAT-GSI/SAP-HANA-ARM).

### <a name="choose-azure-storage-type"></a>Escolha o tipo de armazenamento do Azure
O Azure oferece dois tipos de armazenamento que são adequados para VMs do Azure que estejam a executar o SAP HANA:

- [Armazenamento padrão do Azure](https://docs.microsoft.com/azure/virtual-machines/windows/standard-storage)
- [Armazenamento Premium do Azure](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage)

O Azure disponibiliza dois métodos de implementação para os VHDs no Azure Standard e Premium Storage. Se o cenário geral permite, tirar partido das [Azure geridos disco](https://azure.microsoft.com/services/managed-disks/) implementações.

Para obter uma lista de tipos de armazenamento e os respetivos SLAs no débito de armazenamento e de IOPS, reveja o [documentação do Azure para discos geridos](https://azure.microsoft.com/pricing/details/managed-disks/).

### <a name="configuring-the-storage-for-azure-virtual-machines"></a>Configurar o armazenamento para máquinas virtuais do Azure

Far que comprou a aplicações de SAP HANA para no local, nunca teve de se preocupar com subsistemas de e/s e das respetivas funções porque o fornecedor da aplicação tem de certificar-se de que são cumpridos os requisitos de armazenamento mínima para SAP HANA. À medida que cria a infraestrutura do Azure por si, também deve ter conhecimento de algumas esses requisitos também compreender os requisitos de configuração, que sugerimos nas secções seguintes. Ou nos casos em que estiver a configurar as máquinas virtuais que pretende executar SAP HANA. Algumas das características feitas são causando a necessidade de:

- Ativar o volume de leitura/escrita no /hana/log de 250 MB/seg no mínimo com tamanhos de e/s de 1 MB
- Permitir leitura à atividade de, pelo menos, 400MB/seg para /hana/data para tamanhos de e/s de 16 MB e 64 MB
- Ativar a atividade de escrita de pelo menos de 250MB/seg para /hana/data com 16 MB e 64 MB tamanhos de e/s

Final fornecido esse armazenamento baixo latência é crítica para sistemas DBMS, mesmo que os, como o SAP HANA, manter os dados dentro da memória. O caminho crítico no armazenamento é normalmente em torno de gravações de registo de transação dos sistemas DBMS. Mas também operações como escrever pontos de reposição ou carregar dados dentro da memória após a recuperação de falhas pode ser crítica. Por conseguinte, é obrigatório para tirar partido do Azure Premium discos para /hana/data e /hana/log volumes. Para alcançar o débito mínimo/hana/iniciar/hana/de dados de e como pretendido, SAP, terá de criar um RAID 0 utilizando MDADM ou LVM por vários discos de Premium Storage do Azure e utilizar os volumes RAID como hana/dados e hana/volumes de registo. Como stripe tamanhos de RAID 0 a recomendação está a utilizar:

- 64K ou 128K para hana/dados
- 32K para hana/registo

> [!NOTE]
> Não precisa de configurar qualquer nível de redundância utilizar volumes RAID, uma vez que três imagens de um VHD de manter de armazenamento Standard e Premium do Azure. A utilização de um volume RAID é puramente configurar volumes que fornecem o débito de e/s suficiente.

Accumulating um número de VHDs do Azure por baixo de um RAID, é accumulative um lado do débito de armazenamento e de IOPS. Por isso, se o put um RAID 0 através de discos de Premium Storage do Azure P30 x 3,-deverá dar-lhe três vezes o IOPS e três vezes o débito de armazenamento de um único disco P30 de armazenamento do Azure Premium.

Não configure a colocação em cache nos discos utilizados para /hana/data e /hana/log do Premium Storage. Todos os discos criar esses volumes devem ter a colocação em cache destes discos definida como 'None'.

Tenha também o débito global de e/s de VM em mente quando o dimensionamento ou decidir para uma VM. Em geral débito de armazenamento VM é descrito no artigo [tamanhos de máquina virtual com otimização de memória](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-memory).

#### <a name="cost-conscious-azure-storage-configuration"></a>Custo conscientes configuração de armazenamento do Azure
A tabela seguinte mostra uma configuração de VM de tipos de que os clientes utilizam frequentemente para o anfitrião SAP HANA em VMs do Azure. Poderão existir alguns tipos VM que poderão não cumprir todos os critérios mínimo para SAP HANA. Mas, até ao momento dessas VMs seemed ajustar executar para cenários de não produção. 

> [!NOTE]
> Para cenários de produção, verifique se um determinado tipo VM é suportado para SAP HANA por SAP no [documentação SAP para o IAAS](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html).


| SKU DE VM | RAM | Um máximo de E/S DE VM<br /> Débito | hana/dados e de hana/registo<br /> repartidos com LVM ou MDADM | hana/partilhado | /root volume | usr/sap | Hana/cópia de segurança |
| --- | --- | --- | --- | --- | --- | --- | -- |
| DS14v2 | 128 GiB | 768 MB/s | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S15 |
| E16v3 | 128 GiB | 384 MB/s | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S15 |
| E32v3 | 256 GiB | 768 MB/s | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S20 |
| E64v3 | 443 GiB | 1200 MB/s | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S30 |
| GS5 | 448 GiB | 2000 MB/s | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S30 |
| M32ts | 192 giB | 500 MB/s | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S20 |
| M32ls | 256 GiB | 500 MB/s | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S20 |
| M64ls | 512 GiB | 1000 MB/s | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 |1 x S30 |
| M64s | 1000 GiB | 1000 MB/s | 2 x P30 | 1 x S30 | 1 x S6 | 1 x S6 |2 x S30 |
| M64ms | 1750 GiB | 1000 MB/s | 3 x P30 | 1 x S30 | 1 x S6 | 1 x S6 | 3 x S30 |
| M128s | 2000 GiB | 2000 MB/s |3 x P30 | 1 x S30 | 1 x S6 | 1 x S6 | 2 x S40 |
| M128ms | 3800 GiB | 2000 MB/s | 5 x P30 | 1 x S30 | 1 x S6 | 1 x S6 | 2 x S50 |


Os discos recomendados para a VM mais pequena tipos com 3 x P20 oversize os volumes sobre as recomendações de espaço de acordo com o [SAP TDI armazenamento documento](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html). No entanto, a escolha, tal como apresentado na tabela foi efetuada para fornecer o débito de disco suficientes para SAP HANA. Se precisar de alterações para o volume de /hana/backup, que foi dimensionados para manter as cópias de segurança que representam o dobro do volume de memória, pode ajustar.   
Verifique se o débito de armazenamento para os volumes sugeridos diferentes irá satisfazer a carga de trabalho que pretende executar. Se a carga de trabalho requer que os volumes superiores para /hana/data e /hana/log, terá de aumentar o número de VHDs de armazenamento do Azure Premium. Um volume com mais VHDs que apresentados de dimensionamento irá aumentar o débito IOPS e e/s dentro dos limites do tipo de máquina virtual do Azure. 

> [!NOTE]
> As configurações acima não seriam beneficiar [máquina virtual do Azure único SLA de VM](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_6/) , uma vez que a utiliza uma combinação de Premium Storage do Azure e o armazenamento padrão do Azure. No entanto, a seleção foi escolhida para otimizar os custos.


#### <a name="azure-storage-configuration-to-benefit-for-meeting-single-vm-sla"></a>Configuração de armazenamento do Azure para tirar partido para cumprir o SLA de VM única
Se pretender beneficiar [máquina virtual do Azure único SLA de VM](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_6/), tem de utilizar exclusivamente VHDs de armazenamento do Azure Premium.

> [!NOTE]
> Para cenários de produção, verifique se um determinado tipo VM é suportado para SAP HANA por SAP no [documentação SAP para o IAAS](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html).

| SKU DE VM | RAM | Um máximo de E/S DE VM<br /> Débito | hana/dados e de hana/registo<br /> repartidos com LVM ou MDADM | hana/partilhado | /root volume | usr/sap | Hana/cópia de segurança |
| --- | --- | --- | --- | --- | --- | --- | -- |
| DS14v2 | 128 GiB | 768 MB/s | 3 x P20 | 1 x P20 | 1 x P6 | 1 x P6 | 1 x P15 |
| E16v3 | 128 GiB | 384 MB/s | 3 x P20 | 1 x P20 | 1 x P6 | 1 x P6 | 1 x P15 |
| E32v3 | 256 GiB | 768 MB/s | 3 x P20 | 1 x P20 | 1 x P6 | 1 x P6 | 1 x P20 |
| E64v3 | 443 GiB | 1200 MB/s | 3 x P20 | 1 x P20 | 1 x P6 | 1 x P6 | 1 x P30 |
| GS5 | 448 GiB | 2000 MB/s | 3 x P20 | 1 x P20 | 1 x P6 | 1 x P6 | 1 x P30 |
| M32ts | 192 giB | 500 MB/s | 3 x P20 | 1 x P20 | 1 x P6 | 1 x P6 | 1 x P20 |
| M32ls | 256 GiB | 500 MB/s | 3 x P20 | 1 x P20 | 1 x P6 | 1 x P6 | 1 x P20 |
| M64ls | 512 GiB | 1000 MB/s | 3 x P20 | 1 x P20 | 1 x P6 | 1 x P6 | 1 x P30 |
| M64s | 1000 GiB | 1000 MB/s | 2 x P30 | 1 x P30 | 1 x P6 | 1 x P6 |2 x P30 |
| M64ms | 1750 GiB | 1000 MB/s | 3 x P30 | 1 x P30 | 1 x P6 | 1 x P6 | 3 x P30 |
| M128s | 2000 GiB | 2000 MB/s |3 x P30 | 1 x P30 | 1 x P6 | 1 x P6 | 2 x P40 |
| M128ms | 3800 GiB | 2000 MB/s | 5 x P30 | 1 x P30 | 1 x P6 | 1 x P6 | 2 x P50 |


Os discos recomendados para a VM mais pequena tipos com 3 x P20 oversize os volumes sobre as recomendações de espaço de acordo com o [SAP TDI armazenamento documento](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html). No entanto, a escolha, tal como apresentado na tabela foi efetuada para fornecer o débito de disco suficientes para SAP HANA. Se precisar de alterações para o volume de /hana/backup, que foi dimensionados para manter as cópias de segurança que representam o dobro do volume de memória, pode ajustar.  
Verifique se o débito de armazenamento para os volumes sugeridos diferentes irá satisfazer a carga de trabalho que pretende executar. Se a carga de trabalho requer que os volumes superiores para /hana/data e /hana/log, terá de aumentar o número de VHDs de armazenamento do Azure Premium. Um volume com mais VHDs que apresentados de dimensionamento irá aumentar o débito IOPS e e/s dentro dos limites do tipo de máquina virtual do Azure. 



#### <a name="storage-solution-with-azure-write-accelerator-for-azure-m-series-virtual-machines"></a>Solução de armazenamento com o Azure escrever acelerador para máquinas virtuais do Azure M-série
Acelerador de escrita do Azure é uma funcionalidade que está a obter implementada para VMs de série M exclusivamente. Como o nome de Estados, o objetivo da funcionalidade é melhorar a latência de e/s de escritas contra o Premium Storage do Azure. Para SAP HANA, escrever acelerador deveria ser utilizado contra o volume de /hana/log apenas. Por conseguinte as configurações apresentadas até ao momento precisam de ser alterada. A alteração principal é breakup entre o /hana/data e /hana/log para poder utilizar o Azure escrever acelerador contra o volume de /hana/log apenas. 

> [!IMPORTANT]
> Certificação de SAP HANA para máquinas virtuais do Azure M-série é exclusivamente com o Azure escrever acelerador para o volume de /hana/log. Como resultado, implementações de SAP HANA de cenário de produção em máquinas de virtuais do Azure M-série deve ser configurado com o Azure escrever acelerador para o volume de /hana/log.  

> [!NOTE]
> Para cenários de produção, verifique se um determinado tipo VM é suportado para SAP HANA por SAP no [documentação SAP para o IAAS](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html).

Configurações recomendadas ter o seguinte aspeto:

| SKU DE VM | RAM | Um máximo de E/S DE VM<br /> Débito | hana/dados | hana/registo | hana/partilhado | /root volume | usr/sap | Hana/cópia de segurança |
| --- | --- | --- | --- | --- | --- | --- | --- | -- |
| M32ts | 192 giB | 500 MB/s | 3 x P20 | 2 x P20 | 1 x P20 | 1 x P6 | 1 x P6 |1 x P20 |
| M32ls | 256 GiB | 500 MB/s | 3 x P20 | 2 x P20 | 1 x P20 | 1 x P6 | 1 x P6 |1 x P20 |
| M64ls | 512 GiB | 1000 MB/s | 3 x P20 | 2 x P20 | 1 x P20 | 1 x P6 | 1 x P6 |1 x P30 |
| M64s | 1000 GiB | 1000 MB/s | 4 x P20 | 2 x P20 | 1 x P30 | 1 x P6 | 1 x P6 |2 x P30 |
| M64ms | 1750 GiB | 1000 MB/s | 3 x P30 | 2 x P20 | 1 x P30 | 1 x P6 | 1 x P6 | 3 x P30 |
| M128s | 2000 GiB | 2000 MB/s |3 x P30 | 2 x P20 | 1 x P30 | 1 x P6 | 1 x P6 | 2 x P40 |
| M128ms | 3800 GiB | 2000 MB/s | 5 x P30 | 2 x P20 | 1 x P30 | 1 x P6 | 1 x P6 | 2 x P50 |

Verifique se o débito de armazenamento para os volumes sugeridos diferentes irá satisfazer a carga de trabalho que pretende executar. Se a carga de trabalho requer que os volumes superiores para /hana/data e /hana/log, terá de aumentar o número de VHDs de armazenamento do Azure Premium. Um volume com mais VHDs que apresentados de dimensionamento irá aumentar o débito IOPS e e/s dentro dos limites do tipo de máquina virtual do Azure.

Acelerador do Azure escrever só funciona em conjunto com [discos geridos pelo Azure](https://azure.microsoft.com/services/managed-disks/). Por isso, pelo menos, os discos Premium Storage do Azure que formam o volume de /hana/log tem de ser implementado como discos geridos.

Existem limites de VHDs de armazenamento do Azure Premium por VM que pode ser suportado pelo Azure escrever acelerador. Os limites atuais são:

- 16 VHDs para um M128xx VM
- 8 VHDs para um M64xx VM
- 4 VHDs para um M32xx VM

Pode encontrar instruções mais detalhadas sobre como ativar o Azure escrever acelerador no artigo [escrever acelerador](https://docs.microsoft.com/azure/virtual-machines/linux/how-to-enable-write-accelerator).

Os detalhes e restrições para o Azure escrever acelerador podem ser encontradas na documentação do mesma.


### <a name="set-up-azure-virtual-networks"></a>Configurar as redes virtuais do Azure
Quando tiver conectividade de site para site no Azure através de VPN ou ExpressRoute, tem de ter, pelo menos, um [rede virtual do Azure](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) que está ligado através de um Gateway Virtual ao circuito VPN ou ExpressRoute. O Gateway Virtual se encontra numa sub-rede na rede virtual do Azure. Para instalar o SAP HANA, crie duas sub-redes adicionais dentro da rede virtual. Uma sub-rede aloja as VMs para executar as instâncias de SAP HANA. A outra sub-rede executa Jumpbox ou VMs de gestão para alojar o SAP HANA Studio ou outro software de gestão.

Quando instalar as VMs para executar o SAP HANA, tem as VMs:

- Duas NICs virtuais instalados: um NIC para estabelecer ligação à sub-rede de gestão e um NIC para estabelecer a ligação de rede no local ou outras redes, a instância de SAP HANA na VM do Azure.
- Privada endereços IP estáticos que são implementados para os dois NICs virtuais.

Para obter uma descrição geral dos métodos diferentes para atribuir endereços IP, consulte [tipos e métodos de alocação no Azure de endereços IP](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm). 

Para VMs em execução SAP HANA, deve trabalhar em conjunto com os endereços IP estáticos atribuídos. Razão é que alguns atributos de configuração para HANA referenciam endereços IP.

[Grupos de segurança de rede (NSGs) do Azure](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) são utilizadas para direcionar o tráfego é encaminhado para a instância de SAP HANA ou o Jumpbox. Os NSGs estão associados a sub-rede de SAP HANA e a sub-rede de gestão.

A imagem seguinte mostra uma descrição geral de um esquema de implementação aproximada para SAP HANA:

![Esquema de implementação aproximada para SAP HANA](media/hana-vm-operations/hana-simple-networking.PNG)


Para implementar o SAP HANA no Azure sem uma ligação site a site, aceda a instância de SAP HANA apesar de um endereço IP público. O endereço IP tem de ser atribuído à VM do Azure que está a executar a VM Jumpbox. Neste cenário básico, a implementação baseia-se nos serviços DNS incorporados do Azure para resolver os nomes de anfitrião. Numa implementação mais complexa onde são utilizados os endereços IP destinado ao público, os serviços DNS incorporados do Azure são especialmente importantes. Utilize o Azure NSGs para limitar as portas abertas ou intervalos de endereços IP que se podem ligar para as sub-redes do Azure com os recursos que têm endereços IP destinado ao público. A imagem seguinte mostra um esquema aproximado para a implementação de SAP HANA sem uma ligação site a site:
  
![Esquema de implementação aproximada para SAP HANA sem uma ligação site a site](media/hana-vm-operations/hana-simple-networking2.PNG)
 


## <a name="operations-for-deploying-sap-hana-on-azure-vms"></a>Operações para a implementação de SAP HANA em VMs do Azure
As secções seguintes descrevem algumas das operações relacionadas com a implementação de sistemas de SAP HANA em VMs do Azure.

### <a name="back-up-and-restore-operations-on-azure-vms"></a>Cópia de segurança e restaurar as operações em VMs do Azure
Os documentos seguintes descrevem como efetuar cópias de segurança e restaurar a implementação de SAP HANA:

- [Descrição geral da cópia de segurança do SAP HANA](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-guide)
- [Cópia de segurança de nível de ficheiro de SAP HANA](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-file-level)
- [Benchmark de instantâneos de armazenamento de SAP HANA](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-storage-snapshots)


### <a name="start-and-restart-vms-that-contain-sap-hana"></a>Iniciar e reiniciar as VMs que contêm SAP HANA
Uma funcionalidade prominent da nuvem pública do Azure é que está a ser cobrado apenas para sua minutos informáticos. Por exemplo, quando encerrar uma VM que está a executar o SAP HANA, será cobrado apenas para os custos de armazenamento durante esse período. Outra funcionalidade está disponível quando especificar endereços IP estáticos para as suas VMs na implementação inicial. Quando reiniciar uma VM com o SAP HANA, reinicia a VM com os respetivos endereços IP anteriores. 


### <a name="use-saprouter-for-sap-remote-support"></a>Utilizar SAProuter para remota de suporte do SAP
Se tiver uma ligação site a site entre as localizações no local e o Azure e que está a executar componentes SAP, em seguida, provavelmente já estiver a executar o SAProuter. Neste caso, conclua os seguintes itens para remota de suporte:

- Manter o endereço IP estático e privado da VM que aloja SAP HANA na configuração SAProuter.
- Configure o NSG da sub-rede que aloja a VM HANA para permitir o tráfego através da porta de TCP/IP 3299.

Se estiver a ligar ao Azure através da internet e não tiver um router SAP para a VM com o SAP HANA, em seguida, terá de instalar o componente. Instale SAProuter numa VM separada da sub-rede de gestão. A imagem seguinte mostra um esquema para a implementação de SAP HANA sem uma ligação site a site e com SAProuter aproximado:

![Aproximada esquema de implementação de SAP HANA sem uma ligação site a site e SAProuter](media/hana-vm-operations/hana-simple-networking3.PNG)

Lembre-se de que instalar SAProuter numa VM separada e não na Jumpbox VM. A VM separada tem de ter um endereço IP estático. Para ligar o seu SAProuter a SAProuter que está alojado pela SAP, contacte o SAP para um endereço IP. (SAProuter que está alojado pela SAP é homólogo da instância SAProuter que instala no seu VM). Utilize o endereço IP do SAP para configurar a sua instância SAProuter. As definições de configuração, a porta só é necessária é a porta TCP 3299.

Para obter mais informações sobre como configurar e manter as ligações de remota de suporte através de SAProuter, consulte o [documentação SAP](https://support.sap.com/en/tools/connectivity-tools/remote-support.html).

### <a name="high-availability-with-sap-hana-on-azure-native-vms"></a>Elevada disponibilidade com SAP HANA em VMs nativas do Azure
Se estiver a executar o SUSE Linux 12 SP1 ou posterior, pode estabelecer um cluster de Pacemaker com dispositivos STONITH. Pode utilizar os dispositivos para configurar uma configuração de SAP HANA que utiliza a replicação síncrona com HANA sistema replicação e ativação pós-falha automática. Para obter mais informações sobre o procedimento de configuração, consulte [guia de SAP HANA elevada disponibilidade para máquinas virtuais do Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-overview).
