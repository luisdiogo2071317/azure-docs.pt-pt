---
title: incluir ficheiro
description: incluir ficheiro
services: virtual-machines-windows, virtual-machines-linux
author: dlepow
ms.service: multiple
ms.topic: include
ms.date: 09/28/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: f9974aae1e5996ffeaa6cde690a5e10ccba4cc32
ms.sourcegitcommit: 7bc4a872c170e3416052c87287391bc7adbf84ff
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/02/2018
ms.locfileid: "48019017"
---
## <a name="deploy-an-image-with-marketplace-terms"></a>Implementar uma imagem com os termos do Marketplace

Determinadas imagens VM no Azure Marketplace têm adicionais de licença e que têm de aceitar antes de poder implementá-las por meio de programação de termos de compra.  

Para implementar uma VM a partir de uma imagem desse tipo, terá de aceitar os termos da imagem e ativar a implementação programática. Só precisa de fazer uma vez na sua subscrição. Em seguida, sempre que implantar uma VM através de programação a partir da imagem, também tem de especificar *compra do plano* parâmetros.

As secções seguintes mostram como:

* Descubra se uma imagem do Marketplace tem termos de licenciamento adicionais 
* Aceite os termos de forma programática
* Fornecer os parâmetros de plano de compra ao implementar uma VM através de programação

