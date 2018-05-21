---
title: Resolver problemas de discos ligados para VMs do Azure | Microsoft Docs
description: O Armazenamento de blobs do Azure foi criado para armazenar grandes quantidades de dados de objetos não estruturados, como dados de texto ou binários. As aplicações podem aceder a objetos no Armazenamento de blobs do PowerShell ou da CLI do Azure, a partir do código através de bibliotecas de cliente do Armazenamento do Azure ou através do REST.
services: storage
author: genlin
manager: cshepard
ms.service: storage
ms.topic: article
ms.date: 05/01/2018
ms.author: genli
ms.openlocfilehash: 766062b085c359499046151f337921a51d948715
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/20/2018
---
# <a name="troubleshoot-disks-attached-to-azure-vms"></a>Resolver problemas de discos ligados para VMs do Azure 

Máquinas virtuais do Azure (VMs) baseiam-se em discos de dados de discos rígidos virtuais (VHDs) para o disco de SO e nenhum ligados. Os VHDs são armazenados como blobs de páginas num ou mais contas de armazenamento do Azure. Este artigo aponta para a resolução de problemas de conteúdo para problemas comuns que podem surgir com VHDs. 

## <a name="troubleshoot-storage-deletion-errors-for-a-vm"></a>Resolver erros de eliminação de armazenamento para uma VM

Em certos casos, poderá encontrar um erro ao eliminar um recurso de armazenamento, quando uma VM numa implementação do Resource Manager contém ligado VHDs. Para ajudar a resolver este problema, consulte um dos seguintes artigos: 

  * Em VMs do Linux: [erros de eliminação de armazenamento na implementação do Resource Manager](../../virtual-machines/linux/storage-resource-deletion-errors.md)  
  * Em VMs do Windows: [erros de eliminação de armazenamento na implementação do Resource Manager](../../virtual-machines/windows/storage-resource-deletion-errors.md)  

## <a name="troubleshoot-unexpected-reboots-of-vms-with-attached-vhds"></a>Resolver problemas de reinícios inesperados de VMs com VHDs ligados

Se ocorrerem reinícios inesperados de uma VM com um grande número de VHDs anexados, consulte um dos seguintes artigos:

  * Em VMs do Linux: [inesperado reinicia de VMs com VHDs ligados](../../virtual-machines/linux/unexpected-reboots-attached-vhds.md)
  * Em VMs do Windows: [inesperado reinicia de VMs com VHDs ligados](../../virtual-machines/linux/unexpected-reboots-attached-vhds.md)
