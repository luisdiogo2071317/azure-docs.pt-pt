---
title: incluir ficheiro
description: incluir ficheiro
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: ae36adc78d8c87d85c64fd61cb3a50dfcae60b85
ms.sourcegitcommit: fd488a828465e7acec50e7a134e1c2cab117bee8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/03/2019
ms.locfileid: "53995762"
---
Vai pagar por duas coisas -- os custos de computação à hora do gateway de rede virtual e a transferência de dados de saída a partir do gateway de rede virtual. As informações sobre os preços estão disponíveis na página [Preços](https://azure.microsoft.com/pricing/details/vpn-gateway).

**Custos de computação do gateway de rede virtual**<br>Cada gateway ea rede virtual tem um custo de computação à hora. O preço é baseado no SKU de gateway que especificar quando cria um gateway de rede virtual. O custo está relacionado com o próprio gateway e acresce à transferência de dados que flui através do mesmo. Custo de uma configuração de ativo-ativo é o mesmo que ativa-passiva.

**Custos da transferência de dados**<br>Os custos da transferência de dados são calculados com base no tráfego de saída a partir do gateway de rede virtual de origem.

* Se enviar tráfego para o seu dispositivo VPN no local, este será cobrado à tarifa da transferência de dados de saída da Internet.
* Se enviar tráfego entre redes virtuais em regiões diferentes, o preço baseia-se na região.
* Se enviar tráfego apenas entre redes virtuais que estejam na mesma região, não existem custos de dados. O tráfego entre VNets na mesma região é gratuito.