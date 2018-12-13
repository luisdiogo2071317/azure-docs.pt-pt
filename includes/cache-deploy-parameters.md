---
author: wesmc7777
ms.service: redis-cache
ms.topic: include
ms.date: 11/21/2018
ms.author: wesmc
ms.openlocfilehash: dd9700c9472e07daf294eca12b766e3dc4832955
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2018
ms.locfileid: "53111897"
---
### <a name="cacheskuname"></a>cacheSKUName
O escalão de preço da Cache do Azure nova para Redis.

    "cacheSKUName": {
      "type": "string",
      "allowedValues": [
        "Basic",
        "Standard"
      ],
      "defaultValue": "Basic",
      "metadata": {
        "description": "The pricing tier of the new Azure Cache for Redis."
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
O tamanho da Cache do Azure nova para a instância de Redis. 

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
        "description": "The size of the new Azure Cache for Redis instance. "
      }
    }


O modelo define os valores que são permitidos para este parâmetro (0, 1, 2, 3, 4, 5 ou 6) e atribui um valor predefinido (0) se for especificado nenhum valor. Esses números correspondem aos seguintes tamanhos de cache: 0 = 250 MB, 1 = 1 GB, 2 = 2,5 GB, 3 = 6 GB, 4 = 13 GB, 5 = 26 GB, 6 = 53 GB

