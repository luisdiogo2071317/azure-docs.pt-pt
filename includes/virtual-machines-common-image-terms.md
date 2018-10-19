---
title: incluir ficheiro
description: incluir ficheiro
services: virtual-machines-windows, virtual-machines-linux
author: dlepow
ms.service: multiple
ms.topic: include
ms.date: 10/09/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 6c7a9857f6d6d57dc9e314bcb1ef848a326b7ed2
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/18/2018
ms.locfileid: "49437034"
---
## <a name="terminology"></a>Terminologia

Uma imagem do Marketplace no Azure tem os seguintes atributos:

* **Publicador**: A organização que criou a imagem. Exemplos: Canonical, MicrosoftWindowsServer
* **Oferecer**: O nome de um grupo de imagens relacionadas criadas por um fabricante. Exemplos: Ubuntu Server, WindowsServer
* **SKU**: uma instância de uma oferta, por exemplo, uma versão principal de uma distribuição. Exemplos: 16.04-LTS, 2016-Datacenter
* **Versão**: O número da versão de uma imagem do SKU. 

Para identificar uma imagem do Marketplace, ao implementar uma VM através de programação, fornece estes valores individualmente como parâmetros. Algumas ferramentas aceitam uma imagem *URN*, que combina estes valores, separados pelo caractere dois-pontos (:): *publicador*:*oferecem*:*Sku*: *Versão*. Num URN, pode substituir o número de versão com "mais recente", que seleciona a versão mais recente da imagem. 

Se o publicador de imagens fornece adicionais de licença e os termos de compra, tem de aceitar esses termos e ativar a implementação programática. Também precisará fornecer *compra do plano* parâmetros ao implementar uma VM através de programação. Ver [implementar uma imagem com os termos do Marketplace](#deploy-an-image-with-marketplace-terms).