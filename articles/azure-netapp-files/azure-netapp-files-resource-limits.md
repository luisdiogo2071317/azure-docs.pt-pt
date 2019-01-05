---
title: Limites de recursos para ficheiros de NetApp do Azure | Documentos da Microsoft
description: Descreve os limites de recursos de ficheiros de NetApp do Azure, incluindo limites para conjuntos de capacidade, volumes e a sub-rede do delegado.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 01/03/2019
ms.author: b-juche
ms.openlocfilehash: f34afb1df2ae38353f29a80bfb6798c16856dbeb
ms.sourcegitcommit: d61faf71620a6a55dda014a665155f2a5dcd3fa2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/04/2019
ms.locfileid: "54057160"
---
# <a name="resource-limits-for-azure-netapp-files"></a>Limites de recursos para os ficheiros do Azure NetApp
Compreender limites de recursos para ficheiros de NetApp do Azure ajuda-o a gerir os seus volumes.

## <a name="capacity_pools"></a>Conjuntos de capacidade

- O tamanho mínimo para um conjunto único de capacidade é 4 TiB e o tamanho máximo é 500 TiB. 
- Cada conjunto de capacidade pode pertencer a apenas uma conta de NetApp. No entanto, pode ter múltiplos conjuntos de capacidade numa conta do NetApp.  

## <a name="volumes"></a>Volumes

- O tamanho mínimo de um único volume é 100 GiB e o tamanho máximo é 92 TiB.
- Pode ter um máximo de 100 volumes por subscrição do Azure por região.  

## <a name="delegated_subnet"></a>Sub-rede delegado 

Cada rede Virtual do Azure (Vnet), apenas uma sub-rede pode ser delegada a ficheiros do Azure NetApp.

## <a name="next-steps"></a>Passos Seguintes

[Compreender a hierarquia de armazenamento de ficheiros do Azure NetApp](azure-netapp-files-understand-storage-hierarchy.md)
