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
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/02/2018
ms.locfileid: "29743377"
---
## <a name="deploy-an-image-with-marketplace-terms"></a>Implementar uma imagem com os termos do Marketplace

Algumas imagens VM no Azure Marketplace tem licenciamento adicionais e que tem de aceitar antes de poder implementar através de programação de termos de compra.  

Para implementar uma VM a partir de uma imagem deste tipo, tem de aceitar os termos da imagem e ativar a implementação programática. Basta executar esta uma vez na sua subscrição. Em seguida, sempre que implementar uma VM programaticamente a partir da imagem, terá também de especificar *compra do plano* parâmetros.

As secções seguintes mostram como:

* Determinar se uma imagem do Marketplace tem termos de licenciamento adicionais 
* Aceitar os termos de licenciamento através de programação
* Fornecer os parâmetros do plano de compra ao implementar uma VM através de programação

