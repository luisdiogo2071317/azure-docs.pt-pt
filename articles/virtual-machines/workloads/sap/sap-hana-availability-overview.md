---
title: Disponibilidade de SAP HANA em VMs do Azure - descrição geral | Microsoft Docs
description: Descreve as operações de SAP HANA em VMs nativas do Azure.
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
ms.openlocfilehash: 7049a4b5159687ab928cda7ddc6b1a35959529ac
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
ms.locfileid: "32187162"
---
# <a name="sap-hana-high-availability-for-azure-virtual-machines"></a>Disponibilidade elevada de SAP HANA para máquinas virtuais do Azure

Pode utilizar várias capacidades do Azure para implementar as bases de dados fundamentais, como o SAP HANA em VMs do Azure. Este artigo fornece orientação sobre como obter uma disponibilidade para as instâncias de SAP HANA estão alojados em VMs do Azure. O artigo descreve os vários cenários que pode implementar utilizando a infraestrutura do Azure para aumentar a disponibilidade de SAP HANA no Azure. 

## <a name="prerequisites"></a>Pré-requisitos

Este artigo assume que está familiarizado com a infraestrutura como um Noções básicas de serviço (IaaS) no Azure, incluindo: 

- Como implementar máquinas virtuais e as redes virtuais através do portal do Azure ou do PowerShell.
- Utilizar a interface do Azure plataforma da linha de comandos (CLI do Azure), incluindo a opção para utilizar modelos de JavaScript Object Notation (JSON).

Este artigo também assume que está familiarizado com a instalação de instâncias de SAP HANA e com administrating e funcionamento de instâncias de SAP HANA. É especialmente importante estar familiarizado com o programa de configuração e as operações de replicação do sistema HANA. Isto inclui tarefas, como a cópia de segurança e restauro de bases de dados SAP HANA.

Estes artigos fornecem uma boa descrição geral da utilização de SAP HANA no Azure:

- [Instalação manual de instância única SAP HANA em VMs do Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-get-started)
- [Configurar a replicação do sistema de SAP HANA em VMs do Azure](sap-hana-high-availability.md)
- [Criar cópias de segurança SAP HANA em VMs do Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-guide)

Também é uma boa ideia deve estar familiarizado com estes artigos sobre SAP HANA:

- [Elevada disponibilidade para SAP HANA](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/6d252db7cdd044d19ad85b46e6c294a4.html)
- [FAQ: Elevada disponibilidade para SAP HANA](https://archive.sap.com/documents/docs/DOC-66702)
- [Execute a replicação do sistema para SAP HANA](https://archive.sap.com/documents/docs/DOC-47702)
- [Do SAP HANA 2.0 SPS 01 que novo: elevada disponibilidade](https://blogs.sap.com/2017/05/15/sap-hana-2.0-sps-01-whats-new-high-availability-by-the-sap-hana-academy/)
- [Recomendações de rede para a replicação do sistema de SAP HANA](https://www.sap.com/documents/2016/06/18079a1c-767c-0010-82c7-eda71af511fa.html)
- [Replicação do sistema de SAP HANA](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/b74e16a9e09541749a745f41246a065e.html)
- [Reinício automático do serviço de SAP HANA](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/cf10efba8bea4e81b1dc1907ecc652d3.html)
- [Configurar a replicação do sistema de SAP HANA](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/676844172c2442f0bf6c8b080db05ae7.html)

Para além de que está a ser familiarizado com a implementação de VMs no Azure, antes de definir a arquitetura de disponibilidade no Azure, recomendamos que leia [gerir a disponibilidade das virtual machines Windows no Azure](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability).

## <a name="service-level-agreements-for-azure-components"></a>Contratos de nível de serviço de componentes do Azure

O Azure tem disponibilidade diferente SLAs de diferentes componentes, como redes, armazenamento e as VMs. Todos os SLA estão documentados. Para obter mais informações, consulte [contratos de nível de serviço do Microsoft Azure](https://azure.microsoft.com/support/legal/sla/). 

[SLA para máquinas virtuais](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_6/) descreve dois SLA diferente, para duas configurações diferentes:

- Uma única VM que utiliza [Premium Storage do Azure](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage) para o disco do SO e todos os discos de dados. Esta opção fornece um tempo de atividade mensal de 99,9%.
- Várias VMs (pelo menos dois) que estão organizados por um [conjunto de disponibilidade do Azure](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets). Esta opção fornece um tempo de atividade mensal de 99,95%.

Meça o requisito de disponibilidade contra os SLAs que podem fornecer componentes do Azure. Em seguida, escolha os cenários de SAP HANA atingir o nível de disponibilidade necessário.

## <a name="next-steps"></a>Passos Seguintes

- Saiba mais sobre [disponibilidade de SAP HANA dentro de uma região do Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-one-region).
- Saiba mais sobre [disponibilidade de SAP HANA em regiões do Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-across-regions). 















  


