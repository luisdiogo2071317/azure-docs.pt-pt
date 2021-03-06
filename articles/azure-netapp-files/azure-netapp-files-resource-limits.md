---
title: Limites de recursos para ficheiros de NetApp do Azure | Documentos da Microsoft
description: Descreve os limites de recursos de ficheiros de NetApp do Azure, incluindo limites de contas, conjuntos de capacidade, volumes, instantâneos e a sub-rede do delegado NetApp.
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
ms.topic: concepts
ms.date: 02/14/2019
ms.author: b-juche
ms.openlocfilehash: 196d85917e0a9900e141d58bff171beeb8540409
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/20/2019
ms.locfileid: "56430018"
---
# <a name="resource-limits-for-azure-netapp-files"></a>Limites de recurso para os Azure NetApp Files

Compreender limites de recursos para ficheiros de NetApp do Azure ajuda-o a gerir os seus volumes.

- Cada subscrição do Azure pode ter um máximo de 10 contas de NetApp.
- Cada conta de NetApp pode ter um máximo de 25 conjuntos de capacidade.
- Cada conjunto de capacidade pode pertencer a apenas uma conta de NetApp.  
- O tamanho mínimo para um conjunto único de capacidade é 4 TiB e o tamanho máximo é 500 TiB. 
- Cada conjunto de capacidade pode ter um máximo de 500 volumes.
- O tamanho mínimo de um único volume é 100 GiB e o tamanho máximo é 92 TiB.
- Cada volume pode ter um máximo de 255 instantâneos.
- Cada rede Virtual do Azure (Vnet) pode ter apenas uma sub-rede de delegado ao serviço ficheiros do Azure NetApp.

**Passos seguintes?**

[Compreender a hierarquia de armazenamento de ficheiros do Azure NetApp](azure-netapp-files-understand-storage-hierarchy.md)
