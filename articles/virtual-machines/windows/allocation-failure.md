---
title: "Resolução de problemas de falhas de atribuição de VM do Windows | Microsoft Docs"
description: "Resolver problemas de falhas de alocação quando criar, reiniciar ou redimensionar uma VM do Windows no Azure"
services: virtual-machines-windows, azure-resource-manager
documentationcenter: 
author: JiangChen79
manager: felixwu
editor: 
tags: top-support-issue,azure-resource-manager,azure-service-management
ms.assetid: bb939e23-77fc-4948-96f7-5037761c30e8
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 11/03/2016
ms.author: cjiang
ms.openlocfilehash: 2fd99f47cc2051c5b27c3ec8621ae954e9f8ca14
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/03/2018
---
# <a name="troubleshoot-allocation-failures-when-you-create-restart-or-resize-windows-vms-in-azure"></a>Resolver problemas de falhas de alocação quando criar, reiniciar ou redimensionar VMs do Windows no Azure
Quando criar uma VM, reiniciar VMs (desalocadas) paradas ou redimensionar uma VM, o Microsoft Azure aloca recursos de computação para a sua subscrição. Ocasionalmente, poderá receber erros ao efetuar estas operações – mesmo antes de atingir os limites de subscrição do Azure. Este artigo explica as causas de algumas das falhas de alocação comuns e sugere possíveis remediação. As informações também podem ser úteis quando planear a implementação dos seus serviços. Também pode [resolver falhas de alocação ao criar, reiniciar ou redimensionar VMs com Linux no Azure](../linux/allocation-failure.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

[!INCLUDE [virtual-machines-common-allocation-failure](../../../includes/virtual-machines-common-allocation-failure.md)]

