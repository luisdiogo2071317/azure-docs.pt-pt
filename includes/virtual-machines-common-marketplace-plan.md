---
title: incluir ficheiro
description: incluir ficheiro
services: virtual-machines-windows, virtual-machines-linux
author: dlepow
ms.service: multiple
ms.topic: include
ms.date: 02/28/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: a3c5290eb0179fe5842c495c2e08f22580d02bda
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38944720"
---
## <a name="deploy-an-image-with-marketplace-terms"></a>Implementar uma imagem com os termos do Marketplace

Determinadas imagens VM no Azure Marketplace têm adicionais de licença e que têm de aceitar antes de poder implementá-las por meio de programação de termos de compra.  

Para implementar uma VM a partir de uma imagem desse tipo, terá de aceitar os termos da imagem e ativar a implementação programática. Só precisa de fazer uma vez na sua subscrição. Em seguida, sempre que implantar uma VM através de programação a partir da imagem, também tem de especificar *compra do plano* parâmetros.

As secções seguintes mostram como:

* Descubra se uma imagem do Marketplace tem termos de licenciamento adicionais 
* Aceite os termos de forma programática
* Fornecer os parâmetros de plano de compra ao implementar uma VM através de programação

