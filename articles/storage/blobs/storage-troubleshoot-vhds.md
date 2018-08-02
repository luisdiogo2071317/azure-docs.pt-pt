---
title: Resolver problemas relacionados com discos anexados a VMs do Azure | Documentos da Microsoft
description: O Armazenamento de blobs do Azure foi criado para armazenar grandes quantidades de dados de objetos não estruturados, como dados de texto ou binários. As aplicações podem aceder a objetos no Armazenamento de blobs do PowerShell ou da CLI do Azure, a partir do código através de bibliotecas de cliente do Armazenamento do Azure ou através do REST.
services: storage
author: genlin
ms.service: storage
ms.topic: article
ms.date: 05/01/2018
ms.author: genli
ms.component: disks
ms.openlocfilehash: 0dbd89c28d18d64908d92cd38d8bd1cf3138fd5c
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/01/2018
ms.locfileid: "39397972"
---
# <a name="troubleshoot-disks-attached-to-azure-vms"></a>Resolver problemas relacionados com discos anexados a VMs do Azure 

Máquinas virtuais do Azure (VMs) baseiam-se em discos de dados de discos rígidos virtuais (VHDs) para o disco do SO e qualquer ligados. VHD são armazenadas como blobs de páginas num ou mais contas de armazenamento do Azure. Este artigo aponta para conteúdo para problemas comuns que podem surgir com VHDs de resolução de problemas. 

## <a name="troubleshoot-storage-deletion-errors-for-a-vm"></a>Resolver problemas de erros de eliminação de armazenamento para uma VM

Em certos casos, pode encontrar um erro enquanto a eliminar um recurso de armazenamento, quando uma VM numa implementação do Gestor de recursos contém ligado VHDs. Para ajudar a resolver este problema, consulte um dos seguintes artigos: 

  * Em VMs do Linux: [erros de eliminação de armazenamento na implementação do Resource Manager](../../virtual-machines/linux/storage-resource-deletion-errors.md)  
  * Em VMs do Windows: [erros de eliminação de armazenamento na implementação do Resource Manager](../../virtual-machines/windows/storage-resource-deletion-errors.md)  

## <a name="troubleshoot-unexpected-reboots-of-vms-with-attached-vhds"></a>Resolver problemas de reinícios inesperados de VMs com VHDs ligadas

Se encontrar reinícios inesperados de uma VM com um grande número de VHDs ligadas, consulte um dos seguintes artigos:

  * Em VMs do Linux: [inesperado reinicia de VMs com VHDs ligadas](../../virtual-machines/linux/unexpected-reboots-attached-vhds.md)
  * Em VMs do Windows: [inesperado reinicia de VMs com VHDs ligadas](../../virtual-machines/linux/unexpected-reboots-attached-vhds.md)
