---
title: Metas de desempenho e escalabilidade do armazenamento do Azure | Documentos da Microsoft
description: Saiba mais sobre os destinos de escalabilidade e desempenho do armazenamento do Azure, incluindo a capacidade, a taxa de pedidos e a largura de banda de entrada e saída para ambas as contas de armazenamento standard e premium. Compreenda as metas de desempenho para partições dentro de cada um dos serviços de armazenamento do Azure.
services: storage
author: roygara
ms.service: storage
ms.topic: article
ms.date: 10/24/2017
ms.author: rogarana
ms.component: common
ms.openlocfilehash: 758871537b89a9c010cfaddf324e2208f9846afb
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51241331"
---
# <a name="azure-storage-scalability-and-performance-targets"></a>Alvos de Dimensionamento e Desempenho do Armazenamento do Azure
## <a name="overview"></a>Descrição geral
Este artigo descreve os tópicos de escalabilidade e desempenho do armazenamento do Azure. Para obter um resumo dos outros limites do Azure, veja [subscrição do Azure e limites do serviço, Quotas e restrições](../../azure-subscription-service-limits.md).

> [!NOTE]
> Todas as contas de armazenamento executam na nova topologia de rede simples e suportam os destinos de escalabilidade e desempenho descritos neste artigo, independentemente de quando foram criadas. Para obter mais informações sobre a arquitetura de rede simples de armazenamento do Azure e na escalabilidade, consulte [armazenamento do Microsoft Azure: A altamente disponível serviço armazenamento na Cloud com consistência forte](https://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx).
> 

> [!IMPORTANT]
> Os destinos de escalabilidade e desempenho aqui listados são os destinos de alta tecnologia, mas são alcançáveis. Em todos os casos, a taxa de pedidos e a largura de banda obtida pelo seu armazenamento de conta depende do tamanho dos objetos armazenados, os padrões de acesso utilizados, e o tipo de carga de trabalho executa a sua aplicação. Certifique-se de que seu serviço para determinar se o seu desempenho cumpre os requisitos de teste. Se possível, evite picos repentinos na taxa de tráfego e certifique-se de que o tráfego é bem distribuído por partições.
> 
> Quando seu aplicativo atinge o limite do que pode manipular uma partição para a sua carga de trabalho, o armazenamento do Azure começa a retornar o código de erro 503 (servidor ocupado) ou o código de erro 500 respostas de (tempo limite da operação). Se estes erros estão a ocorrer, a aplicação deve utilizar uma política de término exponencial para repetições. O término exponencial permite que a carga na partição para diminuir e para facilitar a picos de tráfego para essa partição.
> 
> 

Se as necessidades da sua aplicação excederem os destinos de escalabilidade de uma conta de armazenamento única, pode criar a sua aplicação para utilizar várias contas de armazenamento. Em seguida, pode particionar seus objetos de dados entre essas contas de armazenamento. Ver [preços de armazenamento do Azure](https://azure.microsoft.com/pricing/details/storage/) para obter informações sobre os preços de volumes.

## <a name="scalability-targets-for-a-storage-account"></a>Destinos de escalabilidade para uma conta de armazenamento
[!INCLUDE [azure-storage-limits](../../../includes/azure-storage-limits.md)]

### <a name="storage-resource-provider-limits"></a>Limites de fornecedor de recursos de armazenamento 

[!INCLUDE [azure-storage-limits-azure-resource-manager](../../../includes/azure-storage-limits-azure-resource-manager.md)]

## <a name="azure-blob-storage-scale-targets"></a>Metas de dimensionamento de armazenamento de Blobs do Azure
[!INCLUDE [storage-blob-scale-targets](../../../includes/storage-blob-scale-targets.md)]

## <a name="azure-files-scale-targets"></a>Alvos de dimensionamento de ficheiros do Azure
Para obter mais informações sobre os alvos de dimensionamento e desempenho para ficheiros do Azure e Azure File Sync, consulte [metas de escalabilidade e desempenho de ficheiros do Azure](../files/storage-files-scale-targets.md).

[!INCLUDE [storage-files-scale-targets](../../../includes/storage-files-scale-targets.md)]

### <a name="azure-file-sync-scale-targets"></a>Alvos de dimensionamento do Azure File Sync
Com o Azure File Sync, tentamos tanto quanto possível para conceber para utilização ilimitada, no entanto, isso nem sempre é possível. A tabela abaixo indica os limites de nosso teste, e os destinos são limites, na verdade, disco rígidos:

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
* [Armazenamento do Microsoft Azure: Um Cloud altamente disponível, serviço de armazenamento com consistência forte](https://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)

