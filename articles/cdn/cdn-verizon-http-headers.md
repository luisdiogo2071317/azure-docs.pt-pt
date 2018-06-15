---
title: Cabeçalhos HTTP da Verizon específicos para o motor de regras da CDN do Azure | Microsoft Docs
description: Este artigo descreve como utilizar os cabeçalhos de HTTP da Verizon específicas com o motor de regras da CDN do Azure.
services: cdn
documentationcenter: ''
author: dksimpson
manager: akucer
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/16/2018
ms.author: v-deasim
ms.openlocfilehash: 1a4bb48efe2a91c85b773341bb38b0f3ce4c7d9b
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/18/2018
ms.locfileid: "31517451"
---
# <a name="verizon-specific-http-headers-for-azure-cdn-rules-engine"></a>Cabeçalhos HTTP da Verizon específicos para o motor de regras da CDN do Azure

Para **CDN do Azure Premium da Verizon** produtos, quando um pedido de HTTP são enviados para o servidor de origem, o servidor do ponto de presença (POP) pode adicionar um ou mais cabeçalhos reservados (ou cabeçalhos especiais proxy) no pedido do cliente para o POP. Estes cabeçalhos estão além do padrão de reencaminhamento recebidos de cabeçalhos. Para obter informações sobre cabeçalhos de pedido padrão, consulte [pedido campos](https://en.wikipedia.org/wiki/List_of_HTTP_header_fields#Request_fields).

Se pretender impedir que um destes cabeçalhos reservados a ser adicionado no pedido de POP da CDN do Azure (rede de entrega de conteúdos) para o servidor de origem, tem de criar uma regra com o [funcionalidade de Proxy de cabeçalhos especiais](cdn-rules-engine-reference-features.md#proxy-special-headers) no motor de regras. Esta regra, exclua o cabeçalho que pretende remover da lista predefinida de cabeçalhos no campo de cabeçalhos. Se ativou o [funcionalidade de cabeçalhos de resposta de Cache de depuração](cdn-rules-engine-reference-features.md#debug-cache-response-headers), certifique-se de adicionar o necessário `X-EC-Debug` cabeçalhos. 

Por exemplo, para remover o `Via` cabeçalho, o campo de cabeçalhos da regra deve incluir na seguinte lista de cabeçalhos: *X-reencaminhados-de, X-reencaminhados-Proto, X-anfitrião, X-Midgress X-Gateway-lista, X-EC-Name, anfitrião*. 

![Regra de cabeçalhos especiais de proxy](./media/cdn-http-headers/cdn-proxy-special-header-rule.png)

A tabela seguinte descreve os cabeçalhos que podem ser adicionados pelo POP do CDN da Verizon no pedido:

Cabeçalho de pedido | Descrição | Exemplo
---------------|-------------|--------
[através do](#via-request-header) | Identifica o servidor POP efetuado ou o pedido para um servidor de origem. | ECS HTTP/1.1 (dca/1A2B)
X-reencaminhados-para | Indica o endereço IP do autor do pedido.| 10.10.10.10
Proto X reencaminhados | Indica o protocolo do pedido. | http
X-anfitrião | Indica o nome de anfitrião do pedido. | CDN.mydomain.com
X Midgress | Indica se o pedido foi efetuado através de um servidor adicional do CDN. Por exemplo, um servidor de proteção da origem de servidor POP ou um servidor de gateway do servidor para ADN POP. <br />Este cabeçalho é adicionado ao pedido apenas quando o tráfego de midgress ocorre. Neste caso, o cabeçalho está definido como 1 para indicar que o pedido foi efetuado através de um servidor adicional do CDN.| 1
[Anfitrião](#host-request-header) | Identifica o anfitrião e a porta onde o conteúdo solicitado pode ser encontrado. | marketing.mydomain.com:80
[X-Gateway-lista](#x-gateway-list-request-header) | ADN: Identifica a lista de ativação pós-falha de servidores de ADN Gateway atribuídos a uma origem de cliente. <br />Proteção de origem: indica o conjunto de servidores de proteção de origem atribuído a uma origem de cliente. | `icn1,hhp1,hnd1`
X-EC -_&lt;nome&gt;_ | Cabeçalhos de pedido que começam com *X EC* (por exemplo, X EC-Tag, [EC-X-Debug](cdn-http-debug-headers.md)) estão reservados para utilização pela CDN.| produção waf

## <a name="via-request-header"></a>Através do cabeçalho do pedido
O formato através do qual o `Via` pedido cabeçalho identifica um servidor POP é especificado pela seguinte sintaxe:

`Via: Protocol from Platform (POP/ID)` 

Termos utilizados na sintaxe são definidos do seguinte modo:
- Protocolo: Indica que a versão do protocolo (por exemplo, HTTP/1.1) utilizado para proxy o pedido. 

- Plataforma: Indica a plataforma em que o conteúdo foi pedido. Os códigos seguintes são válidos para este campo: 
    Código | Plataforma
    -----|---------
    ECAcc | HTTP grande
    ECS   | Pequeno de HTTP
    ECD   | Rede de entrega de aplicações (ADN)

- POP: Indica o [POP](cdn-pop-abbreviations.md) que processada o pedido. 

- ID: apenas para utilização interna.

### <a name="example-via-request-header"></a>Exemplo através do cabeçalho de pedido

`Via: HTTP/1.1 ECD (dca/1A2B)`

## <a name="host-request-header"></a>Cabeçalho de anfitrião de pedido
Os servidores de POP irão substituir o `Host` cabeçalho quando ambas as condições seguintes forem verdadeiras:
- A origem para o conteúdo pedido é um servidor de origem do cliente.
- Opção de cabeçalho de HTTP da origem de cliente correspondente não está em branco.

O `Host` cabeçalho do pedido será substituído para refletir o valor definido na opção de cabeçalho de HTTP.
Se a opção de cabeçalho de HTTP da origem de cliente está definida como em branco, em seguida, o `Host` cabeçalho de pedido que é submetido pelo autor do pedido será reencaminhado para o servidor de origem do cliente.

## <a name="x-gateway-list-request-header"></a>Cabeçalho de pedido X-Gateway-lista
Um servidor POP adiciona/substituirá o "cabeçalho de pedido X-Gateway-lista quando qualquer uma das seguintes condições é cumprida:
- O pedido aponta para a plataforma ADN.
- O pedido seja reencaminhado para um servidor de origem do cliente que está protegido pela funcionalidade de proteção de origem.

