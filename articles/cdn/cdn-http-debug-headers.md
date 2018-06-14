---
title: Cabeçalhos de HTTP X-EC-Debug para o motor de regras do Azure CDN | Microsoft Docs
description: O cabeçalho de pedido de cache de depuração EC-X-Debug fornece informações adicionais sobre a política de cache que é aplicada ao elemento de pedido. Estes cabeçalhos são específicos da Verizon.
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
ms.date: 04/12/2018
ms.author: v-deasim
ms.openlocfilehash: 3a99e322d81748c54585e7dd0eb06959bfeb9569
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/18/2018
ms.locfileid: "31517437"
---
# <a name="x-ec-debug-http-headers-for-azure-cdn-rules-engine"></a>Cabeçalhos de HTTP X-EC-Debug para o motor de regras da CDN do Azure
O cabeçalho de pedido de cache de depuração, `X-EC-Debug`, fornece informações adicionais sobre a política de cache que é aplicada ao elemento de pedido. Estes cabeçalhos estão específicos **CDN do Azure Premium da Verizon** produtos.

## <a name="usage"></a>Utilização
A resposta enviada dos servidores POP para um utilizador inclui o `X-EC-Debug` cabeçalho apenas quando são satisfeitas as seguintes condições:

- O [funcionalidade de cabeçalhos de resposta de Cache de depuração](cdn-rules-engine-reference-features.md#debug-cache-response-headers) foi ativada no motor de regras para o pedido especificado.
- O pedido especificado define o conjunto de cabeçalhos de resposta de cache de depuração que será incluído na resposta.

## <a name="requesting-debug-cache-information"></a>Solicitar informações de depuração de cache
Utilize as seguintes diretivas o pedido especificado para definir as informações de cache de depuração que serão incluídas na resposta:

Cabeçalho de pedido | Descrição |
---------------|-------------|
X-EC-Debug: x-ec-cache | [Código de estado da cache](#cache-status-code-information)
X-EC-Debug: x-ec-cache-remoto | [Código de estado da cache](#cache-status-code-information)
X-EC-Debug: x-ec-verifique-colocáveis | [Colocáveis](#cacheable-response-header)
X-EC-Debug: x ec-cache-chave | [Chave da cache](#cache-key-response-header)
X-EC-Debug: x ec-cache-Estado | [Estado da cache](#cache-state-response-header)

### <a name="syntax"></a>Sintaxe

Depurar cabeçalhos podem ser solicitados, incluindo o seguinte cabeçalho e as diretivas especificadas no pedido de resposta de cache:

`X-EC-Debug: Directive1,Directive2,DirectiveN`

### <a name="sample-x-ec-debug-header"></a>Cabeçalho X-EC-Debug de exemplo

`X-EC-Debug: x-ec-cache,x-ec-check-cacheable,x-ec-cache-key,x-ec-cache-state`

## <a name="cache-status-code-information"></a>Informações de código de estado de cache
O cabeçalho de resposta EC-X-Debug possa identificar um servidor e como processada, a resposta através as diretivas de seguintes:

Cabeçalho | Descrição
-------|------------
X-EC-Debug: x-ec-cache | Este cabeçalho é comunicado sempre que o conteúdo é encaminhado através da CDN. Identifica o servidor POP que atendeu ao pedido.
X-EC-Debug: x-ec-cache-remoto | Este cabeçalho é comunicado apenas quando o conteúdo solicitado foi colocado em cache de um servidor de proteção da origem ou de um servidor de gateway ADN.

### <a name="response-header-format"></a>Formato de cabeçalho de resposta

O cabeçalho X-EC-Debug relatórios de informações de código de estado de cache no seguinte formato:

- `X-EC-Debug: x-ec-cache: <StatusCode from Platform (POP/ID)>`

- `X-EC-Debug: x-ec-cache-remote: <StatusCode from Platform (POP/ID)>`

Termos utilizados na sintaxe de cabeçalho de resposta acima são definidos do seguinte modo:
- StatusCode: Indica como o conteúdo pedido foi processado pela CDN, que é representada através de um código de estado da cache.
    
    O código de estado TCP_DENIED será possível reportar em vez de nenhuma quando um pedido não autorizado é negado devido a autenticação baseada em tokens. No entanto, o código de estado NONE irá continuar a utilizar ao visualizar relatórios de estado da Cache ou dados de registo não processados.

- Plataforma: Indica a plataforma em que o conteúdo foi pedido. Os códigos seguintes são válidos para este campo:

    Código  | Plataforma
    ------| --------
    ECAcc | HTTP grande
    ECS   | Pequeno de HTTP
    ECD   | Rede de entrega de aplicações (ADN)

- POP: Indica o [POP](cdn-pop-abbreviations.md) que processada o pedido. 

### <a name="sample-response-headers"></a>Cabeçalhos de resposta de amostra

Os cabeçalhos de exemplo seguintes fornecem informações de código de estado de cache para um pedido:

- `X-EC-Debug: x-ec-cache: TCP_HIT from ECD (lga/0FE8)`

- `X-EC-Debug: x-ec-cache-remote: TCP_HIT from ECD (dca/EF00)`

## <a name="cacheable-response-header"></a>Cabeçalho de resposta colocáveis
O `X-EC-Debug: x-ec-check-cacheable` cabeçalho de resposta indica que o conteúdo solicitado foi ter sido colocadas em cache.

Este cabeçalho de resposta não indica se a colocação em cache demorou local. Em vez disso, indica se o pedido foi elegível para a colocação em cache.

### <a name="response-header-format"></a>Formato de cabeçalho de resposta

O `X-EC-Debug` cabeçalho de resposta de relatórios que um pedido foi ter sido colocadas em cache é o seguinte formato:

`X-EC-Debug: x-ec-check-cacheable: <cacheable status>`

O termo utilizado na sintaxe de cabeçalho de resposta acima é definido do seguinte modo:

Valor  | Descrição
-------| --------
SIM    | Indica que foi elegível para a colocação em cache os conteúdos solicitados.
NÃO     | Indica que foi elegível para a colocação em cache os conteúdos solicitados. Este estado pode ser devido a um dos seguintes motivos: <br /> -Configuração de cliente específico: uma configuração específica para a sua conta pode impedir que os servidores de pop-um recurso a colocação em cache. Por exemplo, o motor de regras pode impedir um recurso que está a ser colocados em cache ao ativar a funcionalidade de ignorar a Cache qualificáveis pedidos.<br /> -Cabeçalhos de resposta de cache: Cabeçalhos de Cache-Control e expira o recurso solicitado podem impedir que os servidores de POP a colocação em cache-lo.
DESCONHECIDO | Indica que os servidores eram não é possível avaliar se o recurso pedido foi colocáveis. Este estado ocorre normalmente quando o pedido é negado devido a autenticação baseada em tokens.

### <a name="sample-response-header"></a>Cabeçalho de resposta de amostra

O cabeçalho de resposta de exemplo seguinte indica que o conteúdo solicitado foi ter sido colocadas em cache:

`X-EC-Debug: x-ec-check-cacheable: YES`

## <a name="cache-key-response-header"></a>Cabeçalho de resposta da chave da cache
O `X-EC-Debug: x-ec-cache-key` cabeçalho de resposta indica o físico-chave da cache associado com os conteúdos solicitados. Uma chave de cache física é composta por um caminho que identifique um recurso para efeitos de colocação em cache. Por outras palavras, irão verificar os servidores para uma versão em cache de um recurso, de acordo com o caminho como definido pela respetiva chave de cache.

Esta chave de cache físico começa com uma barra dupla (/ /) seguido do protocolo utilizado para pedir o conteúdo (HTTP ou HTTPS). Este protocolo é seguido o caminho relativo para o elemento de pedido, que começa com o ponto de acesso ao conteúdo (por exemplo, _/000001/_).

Por predefinição, as plataformas HTTP estão configuradas para utilizar *padrão cache*, o que significa que as cadeias de consulta são ignoradas pelo mecanismo de colocação em cache. Este tipo de configuração impede que a chave de cache, incluindo os dados de cadeia de consulta.

Se uma cadeia de consulta é registada no chave da cache, converter equivalente hash e, em seguida, é inserido entre o nome do elemento pedido e a extensão de ficheiro (por exemplo, asset&lt;valor de hash&gt;*.HTML).

### <a name="response-header-format"></a>Formato de cabeçalho de resposta

O `X-EC-Debug` cabeçalho de resposta de relatórios de informações de chave da cache físicas no seguinte formato:

`X-EC-Debug: x-ec-cache-key: CacheKey`

### <a name="sample-response-header"></a>Cabeçalho de resposta de amostra

O cabeçalho de resposta de exemplo seguinte indica a chave de cache de física para o conteúdo pedido:

`X-EC-Debug: x-ec-cache-key: //http/800001/origin/images/foo.jpg`

## <a name="cache-state-response-header"></a>Cabeçalho de resposta de estado de cache
O `X-EC-Debug: x-ec-cache-state` cabeçalho de resposta indica o estado da cache do conteúdo pedido o momento em que foi solicitada.

### <a name="response-header-format"></a>Formato de cabeçalho de resposta

O `X-EC-Debug` cabeçalho de resposta de relatórios de informações de estado de cache no seguinte formato:

`X-EC-Debug: x-ec-cache-state: max-age=MASeconds (MATimePeriod); cache-ts=UnixTime (ddd, dd MMM yyyy HH:mm:ss GMT); cache-age=CASeconds (CATimePeriod); remaining-ttl=RTSeconds (RTTimePeriod); expires-delta=ExpiresSeconds`

Termos utilizados na sintaxe de cabeçalho de resposta acima são definidos do seguinte modo:

- MASeconds: Indica a idade máxima (em segundos), tal como definido por cabeçalhos de Cache-Control os conteúdos solicitados.

- MATimePeriod: Converte o valor de idade máxima (ou seja, MASeconds) aproximado equivalente de uma unidade maior (por exemplo, dias). 

- UnixTime: Indica o carimbo de cache do conteúdo pedido no tempo de Unix (a.k.a. POSIX tempo ou época de Unix). O carimbo de cache indica a data/hora inicial partir do qual será calculado TTL de um recurso. 

    Se o servidor de origem não utilizar um HTTP de terceiros colocação em cache no servidor ou se que o servidor não devolver o cabeçalho de resposta de idade, timestamp a cache será sempre a data/hora quando o recurso foi obtido ou revalidated. Caso contrário, os servidores de POP utilizará o campo de antiguidade para calcular TTL o elemento da seguinte forma: obtenção/RevalidateDateTime - idade.

- ddd, dd MMM aaaa hh: mm: GMT: indica o carimbo de cache do conteúdo pedido. Para obter mais informações, consulte o termo UnixTime acima.

- CASeconds: Indica o número de segundos decorridos desde o carimbo de cache.

- RTSeconds: Indica o número de segundos restante para o qual o conteúdo em cache será considerado raiz. Este valor é calculado da seguinte forma: RTSeconds = idade máxima - idade da cache.

- RTTimePeriod: Converte o valor de TTL (ou seja, RTSeconds) restantes aproximado equivalente de uma unidade maior (por exemplo, dias).

- ExpiresSeconds: Indica o número de segundos restante antes da data/hora especificada no `Expires` cabeçalho de resposta. Se o `Expires` cabeçalho de resposta não estava incluído na resposta, em seguida, o valor deste prazo é *nenhum*.

### <a name="sample-response-header"></a>Cabeçalho de resposta de amostra

O cabeçalho de resposta de exemplo seguintes indica o estado da cache do conteúdo pedido momento em que foi pedido:

```X-EC-Debug: x-ec-cache-state: max-age=604800 (7d); cache-ts=1341802519 (Mon, 09 Jul 2012 02:55:19 GMT); cache-age=0 (0s); remaining-ttl=604800 (7d); expires-delta=none```

