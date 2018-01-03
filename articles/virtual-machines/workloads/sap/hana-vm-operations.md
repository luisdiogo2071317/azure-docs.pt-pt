---
title: "Operações de SAP HANA no Azure | Microsoft Docs"
description: "Manual de operações para sistemas de SAP HANA que são implementados em máquinas virtuais do Azure."
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: 
author: juergent
manager: patfilot
editor: 
tags: azure-resource-manager
keywords: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 11/17/2017
ms.author: msjuergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1d6991d40b9bb8543898bbbdc9d7c905dfe11536
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/02/2018
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
>Para cenários de não produção, utilize os tipos VM que estão listados no [nota SAP #1928533](https://launchpad.support.sap.com/#/notes/1928533).

Implemente as VMs no Azure utilizando:

- O portal do Azure.
- Cmdlets do PowerShell do Azure.
- A CLI do Azure.

Também pode implementar uma plataforma de SAP HANA instalada completa nos serviços de VM do Azure através de [plataforma em nuvem do SAP](https://cal.sap.com/). O processo de instalação está descrito em [implementar SAP S/4HANA ou BW/4HANA no Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/cal-s4h).

### <a name="choose-azure-storage-type"></a>Escolha o tipo de armazenamento do Azure
O Azure oferece dois tipos de armazenamento que são adequados para VMs do Azure que estejam a executar o SAP HANA:

- [Armazenamento padrão do Azure](https://docs.microsoft.com/azure/virtual-machines/windows/standard-storage)
- [Armazenamento Premium do Azure](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage)

O Azure disponibiliza dois métodos de implementação para os VHDs no Azure Standard e Premium Storage. Se o cenário geral permite, tirar partido das [Azure geridos disco](https://azure.microsoft.com/services/managed-disks/) implementações.

Para obter uma lista de tipos de armazenamento e os respetivos SLAs, consulte o [documentação do Azure para discos geridos](https://azure.microsoft.com/pricing/details/managed-disks/).

Para volumes /hana/data e /hana/log, recomenda-se os discos Premium do Azure. Pode criar um RAID LVM através de vários discos de armazenamento Premium e utilizar os volumes RAID como /hana/data e /hana/log volumes.

A tabela seguinte mostra uma configuração de VM de tipos de que os clientes utilizam frequentemente para o anfitrião SAP HANA em VMs do Azure:

| SKU DE VM | RAM | Um máximo de E/S DE VM<br /> Débito | hana/dados e de hana/registo<br /> repartidos com LVM ou MDADM | hana/partilhado | /root volume | usr/sap | Hana/cópia de segurança |
| --- | --- | --- | --- | --- | --- | --- | -- |
| E16v3 | 128 GiB | 384 MB | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S15 |
| E32v3 | 256 giB | 768 MB | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S20 |
| E64v3 | 443 giB | 1200 GB | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S30 |
| GS5 | 448 giB | 2000 GB | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S30 |
| M64s | 1000 giB | 1000 GB | 2 x P30 | 1 x S30 | 1 x S6 | 1 x S6 |2 x S30 |
| M64ms | 1750 giB | 1000 GB | 3 x P30 | 1 x S30 | 1 x S6 | 1 x S6 | 3 x S30 |
| M128s | 2000 giB | 2000 GB |3 x P30 | 1 x S30 | 1 x S6 | 1 x S6 | 2 x S40 |
| M128ms | 3800 giB | 2000 GB | 5 x P30 | 1 x S30 | 1 x S6 | 1 x S6 | 2 x S50 |


> [!NOTE]
> Os discos recomendados para a VM mais pequena tipos com 3 x P20 oversize os volumes sobre as recomendações de espaço de acordo com o [SAP TDI armazenamento documento](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html). No entanto, a escolha, tal como apresentado na tabela foi efetuada para fornecer o débito de disco suficientes para SAP HANA. Se precisar de menos débito de e/s, pode ajustar a escolha de discos de armazenamento Premium para /hana/data e /hana/log. Mesmo se aplica o dimensionamento de volume /hana/backup, que foi dimensão adequado para manter as cópias de segurança que representam o dobro do volume de memória. Se precisar de menos espaço, em seguida, pode ajustar. Tenha também o débito global de e/s de VM em mente quando o dimensionamento ou decidir para uma VM. Em geral débito VM é descrito no artigo [tamanhos de máquina virtual com otimização de memória](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-memory)  

> [!NOTE]
> Se pretender beneficiar [máquina virtual do Azure único SLA de VM](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_6/) precisar de alterar a todos os VHDs que estão listados como armazenamento Standard (Sxx) para o Premium Storage (Pxx). 


### <a name="set-up-azure-virtual-networks"></a>Configurar as redes virtuais do Azure
Quando tiver conectividade de site para site no Azure através de VPN ou ExpressRoute, tem de ter, pelo menos, um [rede virtual do Azure](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) que está ligado através de um Gateway Virtual ao circuito VPN ou ExpressRoute. O Gateway Virtual se encontra numa sub-rede na rede virtual do Azure. Para instalar o SAP HANA, crie duas sub-redes adicionais dentro da rede virtual. Uma sub-rede aloja as VMs para executar as instâncias de SAP HANA. A outra sub-rede executa Jumpbox ou VMs de gestão para alojar o SAP HANA Studio ou outro software de gestão.

Quando instalar as VMs para executar o SAP HANA, tem as VMs:

- Duas NICs virtuais instalados: um NIC para estabelecer ligação à sub-rede de gestão e um NIC para estabelecer a ligação de rede no local ou outras redes, a instância de SAP HANA na VM do Azure.
- Privada endereços IP estáticos que são implementados para os dois NICs virtuais.

Para obter uma descrição geral dos métodos diferentes para atribuir endereços IP, consulte [tipos e métodos de alocação no Azure de endereços IP](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm). 

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
Se estiver a executar o SUSE Linux 12 SP1 ou posterior, pode estabelecer um cluster de Pacemaker com dispositivos STONITH. Pode utilizar os dispositivos para configurar uma configuração de SAP HANA que utiliza a replicação síncrona com HANA sistema replicação e ativação pós-falha automática. Para obter mais informações sobre o procedimento de configuração, consulte [elevada disponibilidade de SAP HANA em máquinas virtuais do Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-high-availability).
