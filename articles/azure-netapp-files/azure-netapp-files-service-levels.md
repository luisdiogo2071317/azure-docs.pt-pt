---
title: Níveis de serviço para ficheiros de NetApp do Azure | Documentos da Microsoft
description: Descreve o desempenho de débito para os níveis de serviço de ficheiros do Azure NetApp.
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
ms.openlocfilehash: a10ac319fe362011531e00f832bb8e471fd56fdf
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/20/2019
ms.locfileid: "56430900"
---
# <a name="service-levels-for-azure-netapp-files"></a>Níveis de serviço para os ficheiros do Azure NetApp
Os ficheiros NetApp do Azure suporta dois níveis de serviço: Premium e Standard. 

## <a name="Premium"></a>Armazenamento Premium

O *Premium* armazenamento fornece até 64 MiB/s por TiB de débito. Desempenho de taxa de transferência é indexado em relação a quota de volume. Por exemplo, um volume do armazenamento Premium com 2 TiB de quota aprovisionado (independentemente do consumo real) tem uma taxa de transferência de 128 MiB/s.

## <a name="Standard"></a>Armazenamento Standard

O *padrão* armazenamento fornece até 16 MiB/s por TiB de débito. Desempenho de taxa de transferência é indexado em relação a quota de volume. Por exemplo, um volume do armazenamento Standard com 2 TiB de quota aprovisionado (independentemente do consumo real) tem um débito de 32 MiB/s.

## <a name="next-steps"></a>Passos Seguintes

- Consulte a [ficheiros do Azure NetApp página de preços](https://azure.microsoft.com/pricing/details/storage/netapp/) pelo preço de níveis de serviço diferentes
- [Configurar um conjunto de capacidade](azure-netapp-files-set-up-capacity-pool.md)
