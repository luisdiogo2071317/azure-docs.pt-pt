---
title: Limita a borda de caixa de dados do Azure | Documentos da Microsoft
description: Descreve os limites de sistema e os tamanhos recomendados para o limite de caixa de dados do Microsoft Azure.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 02/05/2019
ms.author: alkohli
ms.openlocfilehash: 30e0c37d3d0c03e77b6dab9c06c0a50bff27e8bc
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/08/2019
ms.locfileid: "55967511"
---
# <a name="azure-data-box-edge-limits-preview"></a>Limites de borda de caixa de dados do Azure (pré-visualização)

Considere estes limites, como implantar e operar a sua solução de borda de caixa de dados do Microsoft Azure.

> [!IMPORTANT]
> O Data Box Edge está em Pré-visualização. Reveja os [termos de utilização para a pré-visualização](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) antes de implementar esta solução.


## <a name="data-box-edge-service-limits"></a>Limites do serviço de borda de caixa de dados

[!INCLUDE [data-box-edge-gateway-service-limits](../../includes/data-box-edge-gateway-service-limits.md)]

## <a name="data-box-edge-device-limits"></a>Limites de dispositivo do Edge de caixa de dados

A tabela seguinte descreve os limites para o dispositivo de limite de caixa de dados.

| Descrição | Value |
|---|---|
|Não. de arquivos por dispositivo |100 milhões |
|Não. de partilhas por dispositivo |24 |
|Não. de partilhas por contentor |1 |
|Tamanho máximo do ficheiro escrito para uma partilha| 5 TB |

## <a name="azure-storage-limits"></a>Limites de armazenamento do Azure

[!INCLUDE [data-box-edge-gateway-storage-limits](../../includes/data-box-edge-gateway-storage-limits.md)]

## <a name="data-upload-caveats"></a>Limitações de carregamento de dados

[!INCLUDE [data-box-edge-gateway-storage-data-upload-caveats](../../includes/data-box-edge-gateway-storage-data-upload-caveats.md)]

## <a name="azure-storage-account-size-and-object-size-limits"></a>Limites de tamanho de tamanho de conta de armazenamento do Azure e o objeto

[!INCLUDE [data-box-edge-gateway-storage-acct-limits](../../includes/data-box-edge-gateway-storage-acct-limits.md)]


## <a name="azure-object-size-limits"></a>Limites de tamanho de objeto do Azure

[!INCLUDE [data-box-edge-gateway-storage-object-limits](../../includes/data-box-edge-gateway-storage-object-limits.md)]

## <a name="next-steps"></a>Passos Seguintes

- [Preparar para implementar o Azure Data Box Gateway](data-box-gateway-deploy-prep.md)
