---
title: -Máquinas virtuais do Azure - descrição-geral de grupos de disponibilidade do SQL Server | Documentos da Microsoft
description: Este artigo apresenta os grupos de disponibilidade do SQL Server em máquinas virtuais do Azure.
services: virtual-machines
documentationCenter: na
authors: MikeRayMSFT
manager: craigg
editor: monicar
tags: azure-service-management
ms.assetid: 601eebb1-fc2c-4f5b-9c05-0e6ffd0e5334
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/13/2017
ms.author: mikeray
ms.openlocfilehash: 5f8ae6d9138a7413b0cca4cca7bcc47c13212674
ms.sourcegitcommit: a408b0e5551893e485fa78cd7aa91956197b5018
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/17/2019
ms.locfileid: "54358056"
---
# <a name="introducing-sql-server-always-on-availability-groups-on-azure-virtual-machines"></a>Introdução ao SQL Server Always On grupos de disponibilidade em máquinas virtuais do Azure #

Este artigo apresenta os grupos de disponibilidade do SQL Server em máquinas de virtuais do Azure. 

Always On grupos de disponibilidade em máquinas de virtuais do Azure são semelhantes às sempre em grupos de disponibilidade no local. Para obter mais informações, consulte [grupos de Disponibilidade AlwaysOn (SQL Server)](https://msdn.microsoft.com/library/hh510230.aspx). 

O diagrama ilustra as partes de um grupo de disponibilidade de servidor de SQL completo em máquinas de virtuais do Azure.

![Grupo de disponibilidade](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/00-EndstateSampleNoELB.png)

A principal diferença para um grupo de disponibilidade em máquinas de virtuais do Azure é que as máquinas virtuais do Azure, exigir um [Balanceador de carga](../../../load-balancer/load-balancer-overview.md). O Balanceador de carga contém os endereços IP para o serviço de escuta do grupo de disponibilidade. Se tiver mais de um grupo de disponibilidade cada grupo requer um serviço de escuta. Um balanceador de carga pode oferecer suporte a vários serviços de escuta.

Além disso, num cluster de ativação pós-falha de convidado de VM de IaaS do Azure, recomendamos um único NIC por servidor (nó de cluster) e uma única sub-rede. Redes do Azure tem redundância física que torna desnecessário NICs e sub-redes adicionais num cluster de convidados de VM de IaaS do Azure. Embora o relatório de validação de cluster irá emitir um aviso de que os nós só são acessíveis numa única rede, este aviso pode ser ignorado com segurança em clusters de ativação pós-falha de convidado de VM de IaaS do Azure. 

Quando estiver pronto para criar um grupo de disponibilidade do SQL Server em máquinas de virtuais do Azure, veja estes tutoriais.

## <a name="automatically-create-an-availability-group-from-a-template"></a>Criar automaticamente um grupo de disponibilidade a partir de um modelo

[Configurar grupo de disponibilidade Always On na VM do Azure automaticamente - Resource Manager](virtual-machines-windows-portal-sql-alwayson-availability-groups.md)

## <a name="manually-create-an-availability-group-in-azure-portal"></a>Criar manualmente um grupo de disponibilidade no portal do Azure

Também pode criar as máquinas virtuais mesmo sem o modelo. Em primeiro lugar, concluir os pré-requisitos, em seguida, criar o grupo de disponibilidade. Consulte os seguintes tópicos: 

- [Configurar os pré-requisitos para grupos de disponibilidade Always On do SQL Server em máquinas de virtuais do Azure](virtual-machines-windows-portal-sql-availability-group-prereq.md)

- [Criar grupo de Disponibilidade AlwaysOn para melhorar a disponibilidade e recuperação após desastre](virtual-machines-windows-portal-sql-availability-group-tutorial.md)

## <a name="next-steps"></a>Passos Seguintes

[Configurar um SQL Server AlwaysOn no grupo de disponibilidade em máquinas virtuais do Azure em regiões diferentes](virtual-machines-windows-portal-sql-availability-group-dr.md).
