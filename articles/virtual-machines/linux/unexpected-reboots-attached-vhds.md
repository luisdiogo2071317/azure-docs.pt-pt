---
title: Resolver problemas de reinícios inesperados de VMs com anexado VHDs em VMs do Linux do Azure | Microsoft Docs
description: Como resolver problemas de reinícios inesperados de VMs do Linux.
keywords: SSH ligação recusada, ssh erro, azure ssh, ligação SSH falhou
services: virtual-machines-linux
author: tamram
manager: jeconnoc
tags: top-support-issue,azure-service-management,azure-resource-manager
ms.service: virtual-machines-linux
ms.tgt_pltfrm: vm-linux
ms.topic: article
ms.date: 05/01/2018
ms.author: tamram
ms.openlocfilehash: 8ccb6b61c8de1599cd3db011d6401c781cefc31a
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/03/2018
---
# <a name="troubleshoot-unexpected-reboots-of-vms-with-attached-vhds"></a>Resolver problemas de reinícios inesperados de VMs com VHDs ligados

Se uma Máquina Virtual do Azure (VM) tem um grande número de VHDs anexados que estão na mesma conta do storage, pode exceder os destinos de escalabilidade para uma conta de armazenamento individuais, fazendo com que a VM reiniciar inesperadamente. Verifique as métricas de minutos para a conta de armazenamento (**TotalRequests**/**TotalIngress**/**TotalEgress**) para picos que excedem o objetivos de escalabilidade para uma conta de armazenamento. Consulte [métricas mostram um aumento no PercentThrottlingError](../../storage/common/storage-monitoring-diagnosing-troubleshooting.md#metrics-show-an-increase-in-PercentThrottlingError) para obter ajuda a determinar se a limitação ocorreu na sua conta de armazenamento.

Em geral, cada entrada individuais ou a operação de saída num VHD de uma Máquina Virtual traduz-se **obter página** ou **colocar página** operações no blob de página subjacente. Por conseguinte, pode utilizar o IOPS estimado para o seu ambiente para otimizar os VHDs quantos pode numa única conta de armazenamento com base no comportamento específico da sua aplicação. A Microsoft recomenda ter 40 ou menos discos uma única conta de armazenamento. Consulte [metas de desempenho e escalabilidade do Storage do Azure](../../storage/common/storage-scalability-targets.md) para obter detalhes sobre os objetivos de escalabilidade para contas de armazenamento, em particular a taxa de pedidos total e a largura de banda total para o tipo de conta de armazenamento que está a utilizar.

Se está a exceder os destinos de escalabilidade para a sua conta de armazenamento, coloque os seus VHDs em várias contas de armazenamento para reduzir da atividade em cada conta individual.
