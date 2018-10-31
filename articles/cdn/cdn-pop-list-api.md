---
title: Obter a lista atual de Verizon POP da CDN do Azure | Documentos da Microsoft
description: Saiba como obter a lista de Verizon POP atual com a API de REST.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2018
ms.author: kumud
ms.custom: ''
ms.openlocfilehash: f703a934b0eaf4bff5be3811adeed8f0287bc658
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/30/2018
ms.locfileid: "50237830"
---
# <a name="retrieve-the-current-verizon-pop-list-for-azure-cdn"></a>Obter a lista atual de Verizon POP da CDN do Azure

Pode utilizar a API REST para obter o conjunto de IPs do ponto da Verizon dos servidores de presença (POP). Estes servidores POP fazer pedidos para servidores de origem que estão associadas a pontos finais de rede de entrega conteúdo (CDN) num perfil da Verizon (**CDN do Azure Standard da Verizon** ou **CDN do Azure Premium da Verizon**). Tenha em atenção que este conjunto de IPs é diferente dos IPs que um cliente veria quando são efetuados pedidos para os POPs. 

Para a sintaxe da operação de REST API para obter a lista POP, consulte [nós de extremidade - lista](https://docs.microsoft.com/rest/api/cdn/edgenodes/list).

## <a name="typical-use-case"></a>Caso de utilização típica

Por motivos de segurança, pode utilizar esta lista IP para impor que os pedidos para o servidor de origem são feitos apenas a partir de um POP Verizon válido. Por exemplo, se alguém detetado o nome de anfitrião ou endereço IP do servidor de origem de um ponto final da CDN, um efetuar pedidos diretamente para o servidor de origem, por conseguinte, ignorando o dimensionamento e capacidades de segurança fornecidas pela CDN do Azure. Ao definir os IPs na lista devolvida como o único IPs permitido num servidor de origem, pode ser impedido neste cenário. Para garantir que eles têm a lista mais recente do POP, recuperá-la, pelo menos, uma vez por dia. 

## <a name="next-steps"></a>Passos Seguintes

Para obter informações sobre a API REST, consulte [API de REST do CDN do Azure](https://docs.microsoft.com/rest/api/cdn/).
