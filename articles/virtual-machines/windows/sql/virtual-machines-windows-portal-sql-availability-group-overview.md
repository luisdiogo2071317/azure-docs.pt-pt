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
ms.openlocfilehash: b71ba12f6c533d67b04366f05b9334e1993823fa
ms.sourcegitcommit: a3a0f42a166e2e71fa2ffe081f38a8bd8b1aeb7b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/01/2018
ms.locfileid: "43382033"
---
# <a name="introducing-sql-server-always-on-availability-groups-on-azure-virtual-machines"></a>Introdução ao SQL Server Always On grupos de disponibilidade em máquinas virtuais do Azure #

Este artigo apresenta os grupos de disponibilidade do SQL Server em máquinas de virtuais do Azure. 

Always On grupos de disponibilidade em máquinas de virtuais do Azure são semelhantes às sempre em grupos de disponibilidade no local. Para obter mais informações, consulte [grupos de Disponibilidade AlwaysOn (SQL Server)](http://msdn.microsoft.com/library/hh510230.aspx). 

O diagrama ilustra as partes de um grupo de disponibilidade de servidor de SQL completo em máquinas de virtuais do Azure.

![Grupo de disponibilidade](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/00-EndstateSampleNoELB.png)

A principal diferença para um grupo de disponibilidade em máquinas de virtuais do Azure é que as máquinas virtuais do Azure, exigir um [Balanceador de carga](../../../load-balancer/load-balancer-overview.md). O Balanceador de carga contém os endereços IP para o serviço de escuta do grupo de disponibilidade. Se tiver mais de um grupo de disponibilidade cada grupo requer um serviço de escuta. Um balanceador de carga pode oferecer suporte a vários serviços de escuta.

Quando estiver pronto para criar um grupo de disponibilidade do SQL Server em máquinas de virtuais do Azure, veja estes tutoriais.

## <a name="automatically-create-an-availability-group-from-a-template"></a>Criar automaticamente um grupo de disponibilidade a partir de um modelo

[Configurar grupo de disponibilidade Always On na VM do Azure automaticamente - Resource Manager](virtual-machines-windows-portal-sql-alwayson-availability-groups.md)

## <a name="manually-create-an-availability-group-in-azure-portal"></a>Criar manualmente um grupo de disponibilidade no portal do Azure

Também pode criar as máquinas virtuais mesmo sem o modelo. Em primeiro lugar, concluir os pré-requisitos, em seguida, criar o grupo de disponibilidade. Consulte os seguintes tópicos: 

- [Configurar os pré-requisitos para grupos de disponibilidade Always On do SQL Server em máquinas de virtuais do Azure](virtual-machines-windows-portal-sql-availability-group-prereq.md)

- [Criar grupo de Disponibilidade AlwaysOn para melhorar a disponibilidade e recuperação após desastre](virtual-machines-windows-portal-sql-availability-group-tutorial.md)

## <a name="next-steps"></a>Passos Seguintes

[Configurar um SQL Server AlwaysOn no grupo de disponibilidade em máquinas virtuais do Azure em regiões diferentes](virtual-machines-windows-portal-sql-availability-group-dr.md).
