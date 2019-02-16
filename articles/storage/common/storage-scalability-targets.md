---
title: Escalabilidade e objetivos de desempenho do Armazenamento do Azure
description: Saiba mais sobre os destinos de escalabilidade e desempenho, incluindo a capacidade, a taxa de pedidos e a largura de banda entrada e saída, para contas de armazenamento do Azure standard.
services: storage
author: roygara
ms.service: storage
ms.topic: article
ms.date: 11/08/2018
ms.author: rogarana
ms.subservice: common
ms.openlocfilehash: 2f00b01bb07aafca847897f0c31d24d4add7cdbf
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/16/2019
ms.locfileid: "56328882"
---
# <a name="azure-storage-scalability-and-performance-targets-for-standard-storage-accounts"></a>Azure Storage desempenho metas de escalabilidade e para contas de armazenamento standard

Este artigo detalha os destinos de escalabilidade e desempenho para as contas de armazenamento do Azure. Os destinos de escalabilidade e desempenho aqui listados são os destinos de alta tecnologia, mas são alcançáveis. Em todos os casos, a taxa de pedidos e a largura de banda obtida pelo seu armazenamento de conta depende do tamanho dos objetos armazenados, os padrões de acesso utilizados, e o tipo de carga de trabalho executa a sua aplicação. 

Certifique-se de que seu serviço para determinar se o seu desempenho cumpre os requisitos de teste. Se possível, evite picos repentinos na taxa de tráfego e certifique-se de que o tráfego é bem distribuído por partições.

Quando seu aplicativo atinge o limite do que pode manipular uma partição para a sua carga de trabalho, o armazenamento do Azure começa a retornar o código de erro 503 (servidor ocupado) ou o código de erro 500 respostas de (tempo limite da operação). Se estão a ocorrer 503 erros, considere modificar a sua aplicação para utilizar uma política de término exponencial para repetições. O término exponencial permite que a carga na partição para diminuir e para facilitar a picos de tráfego para essa partição.

## <a name="standard-storage-account-scale-limits"></a>Limites de dimensionamento de conta de armazenamento Standard
[!INCLUDE [azure-storage-limits](../../../includes/azure-storage-limits.md)]

## <a name="premium-storage-account-scale-limits"></a>Limites de dimensionamento de conta de armazenamento Premium
[!INCLUDE [azure-premium-limits](../../../includes/azure-storage-limits-premium.md)]

## <a name="storage-resource-provider-scale-limits"></a>Limites de dimensionamento de fornecedor de recursos de armazenamento

[!INCLUDE [azure-storage-limits-azure-resource-manager](../../../includes/azure-storage-limits-azure-resource-manager.md)]

## <a name="azure-blob-storage-scale-targets"></a>Metas de dimensionamento de armazenamento de Blobs do Azure
[!INCLUDE [storage-blob-scale-targets](../../../includes/storage-blob-scale-targets.md)]

## <a name="azure-files-scale-targets"></a>Alvos de dimensionamento de ficheiros do Azure
Para obter mais informações sobre os alvos de dimensionamento e desempenho para ficheiros do Azure e Azure File Sync, consulte [metas de escalabilidade e desempenho de ficheiros do Azure](../files/storage-files-scale-targets.md).

[!INCLUDE [storage-files-scale-targets](../../../includes/storage-files-scale-targets.md)]

### <a name="azure-file-sync-scale-targets"></a>Alvos de dimensionamento do Azure File Sync
O Azure File Sync foi desenvolvido com o objetivo de utilização ilimitada, mas a utilização ilimitada nem sempre é possível. A tabela seguinte indica os limites de testes da Microsoft e também indica os destinos são limites fixos:

[!INCLUDE [storage-sync-files-scale-targets](../../../includes/storage-sync-files-scale-targets.md)]

## <a name="azure-queue-storage-scale-targets"></a>Metas de dimensionamento de armazenamento de filas do Azure
[!INCLUDE [storage-queues-scale-targets](../../../includes/storage-queues-scale-targets.md)]

## <a name="azure-table-storage-scale-targets"></a>Metas de dimensionamento de armazenamento de tabela do Azure
[!INCLUDE [storage-table-scale-targets](../../../includes/storage-tables-scale-targets.md)]

## <a name="see-also"></a>Consultar Também
* [Os detalhes dos preços de armazenamento](https://azure.microsoft.com/pricing/details/storage/)
* [Subscrição do Azure e limites do serviço, Quotas e restrições](../../azure-subscription-service-limits.md)
* [Replicação de armazenamento do Azure](../storage-redundancy.md)
* [Lista de Verificação de Desempenho e Escalabilidade do Armazenamento do Microsoft Azure](../storage-performance-checklist.md)

