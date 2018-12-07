---
author: wesmc7777
ms.service: redis-cache
ms.topic: include
ms.date: 11/21/2018
ms.author: wesmc
ms.openlocfilehash: 3252a6454bf3f70250d2d792ca1f36a819ab22bf
ms.sourcegitcommit: 698ba3e88adc357b8bd6178a7b2b1121cb8da797
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/07/2018
ms.locfileid: "53020183"
---
### <a name="cacheskuname"></a>cacheSKUName
O escalão de preço da Cache do Azure do Azure nova para Redis.

    "cacheSKUName": {
      "type": "string",
      "allowedValues": [
        "Basic",
        "Standard"
      ],
      "defaultValue": "Basic",
      "metadata": {
        "description": "The pricing tier of the new Azure Azure Cache for Redis."
      }
    },

O modelo define os valores que são permitidos para este parâmetro (básico ou Standard) e atribui um valor predefinido (básico), se for especificado nenhum valor. Basic fornece um único nó com vários tamanhos disponíveis se a 53 GB.
Standard fornece dois nós primário/réplica com vários tamanhos disponíveis de cópia de segurança para o SLA de 53 GB e 99,9%.

### <a name="cacheskufamily"></a>cacheSKUFamily
A família para o sku.

    "cacheSKUFamily": {
      "type": "string",
      "allowedValues": [
        "C"
      ],
      "defaultValue": "C",
      "metadata": {
        "description": "The family for the sku."
      }
    },


### <a name="cacheskucapacity"></a>cacheSKUCapacity
O tamanho da Cache do Azure do Azure nova para a instância de Redis. 

    "cacheSKUCapacity": {
      "type": "int",
      "allowedValues": [
        0,
        1,
        2,
        3,
        4,
        5,
        6
      ],
      "defaultValue": 0,
      "metadata": {
        "description": "The size of the new Azure Azure Cache for Redis instance. "
      }
    }


O modelo define os valores que são permitidos para este parâmetro (0, 1, 2, 3, 4, 5 ou 6) e atribui um valor predefinido (0) se for especificado nenhum valor. Esses números correspondem aos seguintes tamanhos de cache: 0 = 250 MB, 1 = 1 GB, 2 = 2,5 GB, 3 = 6 GB, 4 = 13 GB, 5 = 26 GB, 6 = 53 GB

