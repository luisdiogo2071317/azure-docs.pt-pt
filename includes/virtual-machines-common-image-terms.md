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
ms.openlocfilehash: 47ef014f31c628fed9d7b2b005e67e8138d3e7e9
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38942352"
---
## <a name="terminology"></a>Terminologia

Uma imagem do Marketplace no Azure tem os seguintes atributos:

* **Publicador** -a organização que criou a imagem. Exemplos: Canonical, MicrosoftWindowsServer
* **Oferecer** -nome de um grupo de imagens relacionadas criadas por um fabricante. Exemplos: Ubuntu Server, WindowsServer
* **SKU** – uma instância de uma oferta, por exemplo, uma versão principal de uma distribuição. Exemplos: 16.04-LTS, 2016 Datacenter
* **Versão** -o número da versão de uma imagem do SKU. 

Para identificar uma imagem do Marketplace, ao implementar uma VM através de programação, forneça estes valores individualmente como parâmetros ou algumas ferramentas aceitam a imagem *URN*. A URN combina estes valores, separados pelo caractere dois-pontos (:): *publicador*:*oferecer*:*Sku*:*versão*. Num URN, pode substituir o número de versão com "mais recente", que seleciona a versão mais recente da imagem. 

Se o publicador de imagens fornece adicionais de licença e os termos de compra, tem de aceitar esses termos e ativar a implementação programática. Também precisa de fornecer *compra do plano* parâmetros ao implementar uma VM através de programação. Ver [implementar uma imagem com os termos do Marketplace](#deploy-an-image-with-marketplace-terms).