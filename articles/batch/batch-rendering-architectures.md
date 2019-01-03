---
title: Arquiteturas de referência de composição do Azure - Batch do Azure
description: Farm de composição de arquiteturas de utilização do Azure Batch e outros serviços do Azure para expandir no local disparando para a cloud
services: batch
author: davefellows
manager: jeconnoc
ms.author: lahugh
ms.date: 08/13/2018
ms.topic: conceptual
ms.custom: seodec18
ms.openlocfilehash: d5102ba94e2b7808a457df00a87b35ef7022c454
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/17/2018
ms.locfileid: "53543500"
---
# <a name="reference-architectures-for-azure-rendering"></a>Arquiteturas de referência para a composição do Azure

Este artigo mostra-diagramas de arquitetura de alto nível para cenários expandir ou "Expandir", no local compor farm para o Azure. Os exemplos mostram as diferentes opções para serviços de computação, rede e armazenamento do Azure.

## <a name="hybrid-with-nfs-or-cfs"></a>Híbrido com o NFS ou CFS

O diagrama seguinte mostra um cenário híbrido, que inclui os seguintes serviços do Azure:

* **Computação** -conjunto do Azure Batch ou conjunto de dimensionamento de Máquina Virtual.

* **Rede** -no local: O Azure ExpressRoute ou VPN. Azure: VNet do Azure.

* **Armazenamento** - entrada e de ficheiros de saída: NFS ou CFS com VMs do Azure, sincronizadas com o armazenamento no local através do Azure File Sync ou RSync.

  ![Segurança - da cloud híbrida com o NFS ou CFS](./media/batch-rendering-architectures/hybrid-nfs-cfs.png)

## <a name="hybrid-with-blobfuse"></a>Híbrido com Blobfuse

O diagrama seguinte mostra um cenário híbrido, que inclui os seguintes serviços do Azure:

* **Computação** -conjunto do Azure Batch ou conjunto de dimensionamento de Máquina Virtual.

* **Rede** -no local: O Azure ExpressRoute ou VPN. Azure: VNet do Azure.

* **Armazenamento** - entrada e de ficheiros de saída: Armazenamento de BLOBs, montado para recursos através do Azure Blobfuse de computação.

  ![Segurança - da cloud híbrida com Blobfuse](./media/batch-rendering-architectures/hybrid-blob-fuse.png)

## <a name="hybrid-compute-and-storage"></a>Armazenamento e computação híbrido

O diagrama seguinte mostra um cenário híbrido totalmente conectada para computação e armazenamento e inclui os seguintes serviços do Azure:

* **Computação** -conjunto do Azure Batch ou conjunto de dimensionamento de Máquina Virtual.

* **Rede** -no local: O Azure ExpressRoute ou VPN. Azure: VNet do Azure.

* **Armazenamento** -em vários locais: Avere vFXT. Arquivamento opcional de locais ficheiros através do Azure Data Box para o armazenamento de Blobs.

  ![Segurança - da cloud híbrida de computação e armazenamento](./media/batch-rendering-architectures/hybrid-compute-storage.png)


## <a name="next-steps"></a>Passos Seguintes

* Saiba mais sobre como utilizar [renderizar gerentes](batch-rendering-render-managers.md) com o Azure Batch.

* Saiba mais sobre as opções para [composição no Azure](batch-rendering-service.md).