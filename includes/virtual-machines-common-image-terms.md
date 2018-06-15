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
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/02/2018
ms.locfileid: "29743372"
---
## <a name="terminology"></a>Terminologia

Uma imagem do Marketplace no Azure tem os seguintes atributos:

* **Publicador** -a organização que criou a imagem. Exemplos: Canónico, MicrosoftWindowsServer
* **Oferecer** -nome de um grupo de imagens relacionadas criado por um fabricante. Exemplos: Ubuntu Server, WindowsServer
* **SKU** – uma instância de uma oferta, tais como uma maior lançamento de distribuição. Exemplos: 16.04-LTS, Datacenter de 2016
* **Versão** -o número de versão de uma imagem do SKU. 

Para identificar uma imagem do Marketplace quando implementar uma VM através de programação, forneça estes valores individualmente como parâmetros ou algumas ferramentas de aceitam a imagem *URN*. O URN combina estes valores, separados pelo caráter de dois pontos (:): *publicador*:*oferecem*:*Sku*:*versão*. Num URN, pode substituir o número de versão com "mais recente", que seleciona a versão mais recente da imagem. 

Se o publicador de imagem fornecer licenças adicionais e termos de compra, tem de aceitar os termos de licenciamento e ativar a implementação programática. Terá também de fornecer *compra do plano* parâmetros quando implementar uma VM através de programação. Consulte [implementar uma imagem com os termos de Marketplace](#deploy-an-image-with-marketplace-terms).