---
title: O que são zonas de disponibilidade do Azure? | Microsoft Docs
description: Para criar aplicações altamente disponíveis e resilientes no Azure, zonas de disponibilidade fornecer fisicamente separadas localizações, pode utilizar para executar os seus recursos.
services: ''
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
tags: ''
ms.assetid: ''
ms.service: azure
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/08/2018
ms.author: iainfou
ms.custom: mvc I am an ITPro and application developer, and I want to protect (use Availability Zones) my applications and data against data center failure (to build Highly Available applications).
ms.openlocfilehash: 7cfd3fba459f409079cb0c621edab3197070285e
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36232424"
---
# <a name="what-are-availability-zones-in-azure"></a>O que são zonas de disponibilidade no Azure?
Zonas de disponibilidade é uma oferta que protege as suas aplicações e dados de falhas de centro de dados de elevada disponibilidade. Zonas de disponibilidade estão localizações físicas exclusivas dentro de uma região do Azure. Cada zona é constituída por um ou vários centros de dados equipados com energia independentes, arrefecimento e funcionamento em rede. Para garantir a resiliência, há um mínimo de três zonas separadas em todas as regiões ativadas. A separação física das zonas de disponibilidade numa região protege a aplicações e dados de falhas de centro de dados. Zona redundante serviços replicar as suas aplicações e dados em zonas de disponibilidade para proteger contra único pontos de falha. Com zonas de disponibilidade, o Azure oferece SLA de disponibilidade VM da indústria melhor 99,99%. O [SLA do Azure](https://azure.microsoft.com/support/legal/sla/virtual-machines/) completo explica a disponibilidade garantida do Azure em termos globais.

Uma zona de disponibilidade na região do Azure é uma combinação de um domínio de falhas e de um domínio de atualização. Por exemplo, se criar três ou mais VMs em três horários numa região do Azure, as VMs estão eficazmente distribuídas três domínios de falhas e três domínios de atualização. A plataforma do Azure reconhece esta distribuição entre domínios de atualização para se certificar de que as VMs em diferentes zonas não são atualizadas ao mesmo tempo.

Crie elevada disponibilidade para a arquitetura da aplicação por colocalizar os recursos de computação, armazenamento, rede e dados dentro de uma zona e replicar em outras zonas. Serviços do Azure que suportam disponibilidade zonas enquadram-se em duas categorias:

- **Serviços zonal** – afixar o recurso a uma zona específica (por exemplo, as máquinas virtuais, discos geridos, endereços IP), ou
- **Zona redundante serviços** – plataforma replica automaticamente através de zonas (por exemplo, com redundância de zona de armazenamento, base de dados SQL).

Para alcançar a continuidade do negócio abrangente no Azure, crie a sua arquitetura de aplicação com a combinação de zonas de disponibilidade de pares de região do Azure. Pode replicar de forma síncrona as suas aplicações e dados através de zonas de disponibilidade dentro de uma região do Azure para elevada disponibilidade e replicar de forma assíncrona em regiões do Azure para a proteção de recuperação após desastre.
 
![Vista concetual de uma zona ficar numa região](./media/az-overview/az-graphic-two.png)

## <a name="regions-that-support-availability-zones"></a>Regiões que suportam zonas de disponibilidade

- EUA Central
- Centro de França
- E.u. a leste 2 (pré-visualização)
- Europa Ocidental (pré-visualização)
- Sudeste Asiático (pré-visualização)


## <a name="services-that-support-availability-zones"></a>Serviços que suportam zonas de disponibilidade
Os serviços do Azure que suportam zonas de disponibilidade são:

- Máquinas Virtuais do Linux
- Máquinas Virtuais do Windows
- Conjuntos de Dimensionamento de Máquinas Virtuais
- Managed Disks
- Load balancer
- Endereço IP público
- Armazenamento com redundância de zona
- SQL Database


## <a name="pricing"></a>Preços
Não há sem custos adicionais para máquinas virtuais implementadas numa zona de disponibilidade. Tempo de atividade VM de 99,99% SLA é fornecido quando são implementadas duas ou mais VMs em dois ou mais zonas de disponibilidade dentro de uma região do Azure. Será adicionais disponibilidade entre zona-VM para dados taxas aplicáveis às transferências. Para obter mais informações, consulte o [preços de largura de banda](https://azure.microsoft.com/pricing/details/bandwidth/) página.


## <a name="get-started-with-availability-zones"></a>Introdução à disponibilidade zonas
- [Criar uma máquina virtual](../virtual-machines/windows/create-portal-availability-zone.md)
- [Adicionar um disco gerido com o PowerShell](../virtual-machines/windows/attach-disk-ps.md#add-an-empty-data-disk-to-a-virtual-machine)
- [Criar um conjunto de dimensionamento da máquina de virtual redundante de zona](../virtual-machine-scale-sets/virtual-machine-scale-sets-use-availability-zones.md)
- [Balanceamento de carga VMs em horários com um balanceador de carga padrão de front-end com redundância de zona](../load-balancer/load-balancer-standard-public-zone-redundant-cli.md)
- [Balanceamento de carga VMs dentro de uma zona com um balanceador de carga padrão zonal front-end](../load-balancer/load-balancer-standard-public-zonal-cli.md)
- [Armazenamento com redundância de zona](../storage/common/storage-redundancy-zrs.md)
- [Base de Dados SQL](../sql-database/sql-database-high-availability.md#zone-redundant-configuration-preview)


## <a name="next-steps"></a>Passos Seguintes
- [Modelos de início rápido](http://aka.ms/azqs)
