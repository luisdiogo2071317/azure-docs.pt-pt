---
title: Resolver problemas relacionados com discos anexados a VMs do Azure | Documentos da Microsoft
description: Fornece links para recursos para a máquina virtual do Azure de discos rígidos virtuais (VHDs) de resolução de problemas.
services: storage
author: genlin
ms.service: storage
ms.topic: article
ms.date: 05/01/2018
ms.author: genli
ms.openlocfilehash: 81db3a819c28aab8f4b644a940eeb5e6c5ecf3d6
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/30/2018
ms.locfileid: "43307177"
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
