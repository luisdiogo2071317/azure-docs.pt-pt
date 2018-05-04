---
title: Processar mensagens grandes no Azure Logic Apps | Microsoft Docs
description: Saber como lidar com tamanhos de mensagem grande com as logic apps de agrupamento
services: logic-apps
documentationcenter: ''
author: shae-hurst
manager: SyntaxC4
editor: ''
ms.assetid: ''
ms.service: logic-apps
ms.devlang: ''
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: logic-apps
ms.date: 4/27/2018
ms.author: shhurst; LADocs
ms.openlocfilehash: 421a207456908fa3b10582c2287b1b2467ff74b1
ms.sourcegitcommit: 6e43006c88d5e1b9461e65a73b8888340077e8a2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/01/2018
---
# <a name="handle-large-messages-with-chunking-in-logic-apps"></a>Processar grandes mensagens com a segmentação em Logic Apps

Quando o processamento de mensagens, as Logic Apps limita o conteúdo da mensagem para um tamanho máximo.
Este limite ajuda a reduzir a sobrecarga criada ao armazenar e processar mensagens de grandes dimensões.
Para processar mensagens excede este limite, as Logic Apps podem *segmento* uma mensagem de grandes dimensões em mensagens mais pequenas. Dessa forma, pode ainda transferir ficheiros grandes utilizando as Logic Apps em condições específicas.
Ao comunicar com outros serviços através de HTTP ou conectores, as Logic Apps podem consumir grandes mensagens mas *apenas* em segmentos.
Isto significa que os conectores também tem de suportar segmentação ou a troca de mensagem HTTP subjacente entre as Logic Apps e estes serviços têm de utilizar o agrupamento.

Este artigo mostra como pode configurar suporte para processamento de mensagens em fila que excedem o limite de agrupamento.

## <a name="what-makes-messages-large"></a>O que faz com que mensagens "large"?

As mensagens são "large" baseados no serviço a processar as mensagens.
O limite de tamanho exato nas mensagens de grande difere entre as Logic Apps e conectores.
As Logic Apps e conectores diretamente não podem consumir mensagens grande, o que devem ser partes. Para o limite de tamanho de mensagem de Logic Apps, consulte [limites Logic Apps e a configuração](../logic-apps/logic-apps-limits-and-config.md).
Para o limite de tamanho da mensagem de cada conector, consulte o [detalhes técnicos da específico do conector](../connectors/apis-list.md).

### <a name="chunked-message-handling-for-logic-apps"></a>Mensagem de segmentos de processamento para aplicações lógicas

As Logic Apps diretamente não é possível utilizar saídas de mensagens de segmentos que excedem o limite de tamanho de mensagem de Logic Apps.
Apenas as ações que suportem segmentação podem aceder ao conteúdo de mensagem nestes saídas.
Por isso, tem uma ação que processa mensagens grande *ou*:

* Suportam nativamente o agrupamento quando essa ação pertence a um conector.
* Ter suporte ativado na configuração de tempo de execução da ação de agrupamento.

Caso contrário, receberá um erro de tempo de execução quando tenta aceder a conteúdo de saída grande.
Para ativar a segmentação, consulte [configurar suporte de agrupamento](#set-up-chunking).

### <a name="chunked-message-handling-for-connectors"></a>Mensagem de segmentos de processamento para conectores

Os serviços que comunicam com as Logic Apps podem ter os seus próprios limites de tamanho da mensagem.
Estes limites são, muitas vezes, mais pequenos do que o limite de Logic Apps. Por exemplo, partindo do princípio que um conector suporta o agrupamento, um conector pode considerar uma mensagem de 30 MB como grande, enquanto não as Logic Apps.
Para cumprir o limite deste conector, as Logic Apps divide qualquer mensagem com mais de 30 MB numa segmentos mais pequenos.

Para os conectores que suporte segmentação, o protocolo das secções subjacente é invisível para os utilizadores finais.
No entanto, nem todos os conectores suportem segmentação, para que estes conectores geram erros de tempo de execução quando a receber mensagens excedem os limites de tamanho dos conectores.

<a name="set-up-chunking"></a>

## <a name="set-up-chunking-over-http"></a>Definir a segmentação através de HTTP

Em cenários HTTP genéricos, pode dividir a cópia de segurança grandes transferências de conteúdo e carrega através de HTTP, para que a sua aplicação lógica e um ponto final podem trocar mensagens grandes. No entanto, tem chunk mensagens da forma que espera Logic Apps.

Se um ponto final tem ativada a segmentação de transferências ou carregamentos, as ações de HTTP na sua aplicação lógica automaticamente segmento grande mensagens. Caso contrário, terá de configurar suporte no ponto final de agrupamento. Se não detém ou controla o ponto final ou o conector, não terá a opção para configurar o agrupamento.

Além disso, se uma ação de HTTP já não ativar a segmentação, tem também de configurar a ação de agrupamento `runTimeConfiguration` propriedade.
Pode definir esta propriedade no interior de ação, diretamente no editor de vista de código, conforme descrito mais tarde, ou no Designer de aplicações lógicas conforme descrito aqui:

1. No canto superior direito da ação de HTTP, faça duplo clique no botão de reticências (**...** ) e escolha **definições**.

2. Em **transferir conteúdo**, defina **permitir segmentação** para **no**.

3. Para continuar a configuração de agrupamento para transferências ou carregamentos, prossiga com as seguintes secções.

<a name="download-chunks"></a>

## <a name="download-content-in-chunks"></a>Transferir o conteúdo em segmentos

Muitos pontos finais automaticamente enviam mensagens grandes segmentos quando transferidos através de um pedido de HTTP GET.
Para transferir mensagens partes de um ponto final através de HTTP, o ponto final tem de suportar pedidos de conteúdo parciais, ou *partes transferências*. Quando a sua aplicação lógica envia um pedido de HTTP GET para um ponto final para transferir o conteúdo e o ponto final responde com um código de estado "206", a resposta contém conteúdo fragmentado.
As Logic Apps não é possível controlar se um ponto final suporta pedidos parciais.
No entanto, quando a sua aplicação lógica obtém o primeiro "206" resposta, a sua aplicação lógica envia automaticamente vários pedidos para transferir todo o conteúdo.

Para verificar se um ponto final pode suportar conteúdo parcial, envie um pedido HEAD. Este pedido ajuda a determinar se a resposta contém o `Accept-Ranges` cabeçalho.
Dessa forma, se o ponto final suporta transferências de partes mas não a enviar conteúdo de partes, pode *sugerir* esta opção, definindo o `Range` cabeçalho no seu pedido de HTTP GET.

Estes passos descrevem o processo de detalhado que Logic Apps utiliza para transferirem conteúdo de partes de um ponto final à sua aplicação lógica:

1. A aplicação lógica envia um pedido de HTTP GET para o ponto final.

   O cabeçalho do pedido, opcionalmente, pode incluir um `Range` campo que descreve um intervalo de bytes para pedir a segmentos de conteúdo.

2. O ponto final responde com o código de estado "206" e um corpo de mensagem HTTP.

    Detalhes sobre o conteúdo neste segmento são apresentados na resposta `Content-Range` cabeçalho, incluindo informações que ajuda as Logic Apps determinar o início e de fim para o segmento, mais o tamanho total dos todo o conteúdo antes de agrupamento.

3. A aplicação lógica envia automaticamente o seguimento de pedidos de HTTP GET.

    A aplicação lógica envia seguimento pedidos GET até obter todo o conteúdo.

Por exemplo, esta definição de ação mostra um pedido de HTTP GET que define o `Range` cabeçalho, *sugerindo* que o ponto final responder com partes conteúdo:

```json
"getAction": {
    "inputs": {
        "headers": {
            "Range": "bytes=0-1023"
        },
       "method": "GET",
       "uri": "http://myAPIendpoint/api/downloadContent"
    },
    "runAfter": {},
    "type": "Http"
}
```

O pedido GET define o cabeçalho "Intervalo" para "bytes = 0 1023", que é o intervalo de bytes. Se o ponto final suporta pedidos de conteúdo parcial, o ponto final responde com um segmento de conteúdo de intervalo pedido.
Com base no ponto final, o formato exato para o campo de cabeçalho "Intervalo" pode ser diferente.

<a name="upload-chunks"></a>

## <a name="upload-content-in-chunks"></a>Carregar conteúdo segmentos

Para carregar conteúdo partes de uma ação de HTTP, a ação tem de ter ativada suporte das secções através da ação `runtimeConfiguration` propriedade.
Esta definição permite a ação para iniciar o protocolo das secções.
A aplicação lógica, em seguida, pode enviar uma mensagem POST ou PUT inicial para o ponto final de destino.
Depois do ponto final responde com um tamanho de segmentos sugeridos, a aplicação lógica segue enviando pedidos de HTTP aplicar o PATCH de mensagens em fila que contêm os segmentos de conteúdo.

Estes passos descrevem o processo de detalhado que Logic Apps utiliza para carregar conteúdo partes da sua aplicação lógica para um ponto final:

1. A aplicação lógica envia um pedido de HTTP POST ou PUT inicial com um corpo de mensagem vazia. O cabeçalho do pedido, inclui esta informação sobre o conteúdo que a sua aplicação lógica que pretende carregar em segmentos:

   | Campo de cabeçalho de pedido de aplicações lógicas | Valor | Tipo | Descrição |
   |---------------------------------|-------|------|-------------|
   | **x-ms-transferência-modo** | partes | Cadeia | Indica que o conteúdo foi carregado no segmentos |
   | **x-ms-comprimento do conteúdo** | <*comprimento do conteúdo*> | Número inteiro | O tamanho do conteúdo completo em bytes antes de agrupamento |
   ||||

2. O ponto final responde com o código de estado de êxito "200" e estas informações opcionais:

   | Campo de cabeçalho de resposta do ponto final | Tipo | Necessário | Descrição |
   |--------------------------------|------|----------|-------------|
   | **x-ms--tamanho do segmento** | Número inteiro | Não | O tamanho do segmento sugeridos em bytes |
   | **Localização** | Cadeia | Não | A localização de URL para onde enviar as mensagens de correção de HTTP |
   ||||

3. A aplicação lógica cria e envia mensagens de aplicar o PATCH HTTP seguimento – cada um com estas informações:

   * Um segmento de conteúdo com base no **x-ms--tamanho do segmento** ou algumas internamente calculado tamanho até que todos os o conteúdo perfazendo **x-ms-content-length** sequencialmente está carregado

   * Estes detalhes de cabeçalho sobre os segmentos menos o conteúdo enviado em cada mensagem de PATCH:

     | Campo de cabeçalho de pedido de aplicações lógicas | Valor | Tipo | Descrição |
     |---------------------------------|-------|------|-------------|
     | **Intervalo de conteúdo** | <*intervalo*> | Cadeia | O intervalo de byte para o segmento atual do conteúdo, incluindo o valor inicial, que termina valor e o tamanho total do conteúdo, por exemplo: "bytes = 0-1023/10100" |
     | **Tipo de conteúdo** | <*tipo de conteúdo*> | Cadeia | O tipo de conteúdo fragmentado |
     | **Comprimento do conteúdo** | <*comprimento do conteúdo*> | Cadeia | O comprimento do tamanho em bytes do segmento atual |
     |||||

4. Depois de cada pedido de PATCH, o ponto final confirma a receção de cada segmento por responder com o código de estado "200".

Por exemplo, esta definição de ação mostra um pedido POST de HTTP para carregar conteúdo fragmentado para um ponto final.
Na ação de `runTimeConfiguration` propriedade, o `contentTransfer` conjuntos de propriedade `transferMode` para `chunked`:

```json
"postAction": {
    "runtimeConfiguration": {
        "contentTransfer": {
            "transferMode": "chunked"
        }
    },
    "inputs": {
        "method": "POST",
        "uri": "http://myAPIendpoint/api/action",
        "body": "@body('getAction')"
    },
    "runAfter": {
    "getAction": ["Succeeded"]
    },
    "type": "Http"
}
```