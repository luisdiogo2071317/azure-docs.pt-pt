---
title: Obter a lista de POP da Verizon atual para o Azure CDN | Microsoft Docs
description: Saiba como obter a lista de POP da Verizon atual utilizando a API REST.
services: cdn
documentationcenter: ''
author: dksimpson
manager: cfowler
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2018
ms.author: v-deasim
ms.custom: ''
ms.openlocfilehash: f796d18a03e14fdf652af72366762e1365523f09
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/25/2018
ms.locfileid: "36754673"
---
# <a name="retrieve-the-current-verizon-pop-list-for-azure-cdn"></a>Obter a lista de POP da Verizon atual para a CDN do Azure

Pode utilizar a API REST para obter o conjunto de IPs para o ponto da Verizon dos servidores de presença (POP). Estes servidores POP efetuam pedidos para servidores de origem que estão associadas a pontos finais do Azure rede de entrega conteúdos (CDN) num perfil da Verizon (**CDN do Azure Standard da Verizon** ou **CDN do Azure Premium da Verizon**). Tenha em atenção que este conjunto de IPs é diferente do que um cliente a é apresentado quando efetuar pedidos para os POPs IPs. 

Para a sintaxe da operação de REST API para obter a lista de POP, consulte [Edge nós - lista](https://docs.microsoft.com/rest/api/cdn/edgenodes/list).

## <a name="typical-use-case"></a>Caso de utilização típica

Por motivos de segurança, pode utilizar esta lista IP para impor que são efetuados os pedidos para o servidor de origem apenas a partir de um POP da Verizon válido. Por exemplo, se alguém detetado o nome de anfitrião ou endereço IP do servidor de origem de um ponto final da CDN, um efetuam pedidos diretamente para o servidor de origem, ignorando assim a escala e as capacidades de segurança fornecidas pela CDN do Azure. Ao definir os IPs da lista devolvido como o único permitido IPs num servidor de origem, pode ser impedido neste cenário. Para se certificar de tem a lista mais recente do POP, obter, pelo menos uma vez por dia. 

## <a name="next-steps"></a>Passos Seguintes

Para obter informações sobre a API REST, consulte [API REST da Azure CDN](https://docs.microsoft.com/en-us/rest/api/cdn/).