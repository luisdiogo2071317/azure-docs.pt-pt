---
title: incluir ficheiro
description: incluir ficheiro
services: virtual-machines-linux
author: dlepow
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 03/19/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: c1e57ea28f597293d8bb52207bbbb76892b1d5f5
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/20/2018
---
## <a name="deployment-considerations"></a>Considerações sobre implementação

* Para disponibilidade de série N VMs, consulte [produtos disponíveis por região](https://azure.microsoft.com/en-us/regions/services/).

* Série N VMs só podem ser implementadas no modelo de implementação Resource Manager.

* Série N VMs diferem no tipo de armazenamento do Azure que suportem para os respetivos discos. NC e NV VMs só suportam discos VM que sejam copiados pelo armazenamento de disco Standard (HDD). NCv2, ND e NCv3 VMs só suportam discos VM que sejam copiados pelo armazenamento de disco Premium (SSD).

* Se pretender implementar mais do que alguns série N VMs, considere uma subscrição pay as you go ou outras opções de compra. Se estiver a utilizar uma [conta gratuita do Azure](https://azure.microsoft.com/free/), pode utilizar apenas um número limitado de núcleos de computação do Azure.

* Poderá ter de aumentar a quota de núcleos (por região) na sua subscrição do Azure e aumentar a quota de núcleos de NC, NCv2, NCv3, ND ou NV separada. Para pedir um aumento de quota [abrir um pedido de suporte ao cliente online](../articles/azure-supportability/how-to-create-azure-support-request.md) , sem encargos. Limites de predefinição podem variar consoante a categoria de subscrição.




