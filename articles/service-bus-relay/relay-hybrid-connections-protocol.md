---
title: Guia de protocolo as ligações híbridas do reencaminhamento do Azure | Microsoft Docs
description: Guia de protocolo de ligações híbridas de reencaminhamento do Azure.
services: service-bus-relay
documentationcenter: na
author: clemensv
manager: timlt
editor: ''
ms.assetid: 149f980c-3702-4805-8069-5321275bc3e8
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/02/2018
ms.author: clemensv
ms.openlocfilehash: 306a21add76261dce99c954a2ba373e4b5047a75
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/08/2018
ms.locfileid: "33895415"
---
# <a name="azure-relay-hybrid-connections-protocol"></a>Protocolo de ligações híbridas de reencaminhamento do Azure

Reencaminhamento do Azure é uma das pillars a capacidade de chave de plataforma do Service Bus do Azure. A nova _ligações híbridas_ capacidade de reencaminhamento é uma evolução segura e protocolo open com base em HTTP e WebSockets. Este substitui o anterior, igualmente denominado _BizTalk Services_ funcionalidade que foi criada no foundation protocolo proprietário. A integração de ligações híbridas para os serviços de aplicações do Azure irá continuar a funcionar como-é.

As ligações híbridas permite a comunicação bidirecional, binário fluxo e datagrama simples fluxo entre duas aplicações de rede. Um ou ambos os intervenientes podem residir atrás de firewalls ou dos NATs.

Este artigo descreve as interações do lado do cliente com o reencaminhamento de ligações híbridas para ligar a clientes nas funções do serviço de escuta e o remetente. Também descreve a forma como os serviços de escuta aceitam novas ligações e os pedidos.

## <a name="interaction-model"></a>Modelo de interação

O reencaminhamento de ligações híbridas liga-se duas partes, fornecendo um ponto de encontro na nuvem do Azure que terceiros podem detetar e ligar a partir da perspetiva da sua própria rede. O ponto de encontro é chamado "Ligação híbrida" neste e outra documentação, nas APIs e também no portal do Azure. O ponto final de serviço de ligações híbridas é referido como "serviço" para o resto deste artigo.

Permite que o serviço para reencaminhar ligações Web Socket e pedidos de HTTP (S) e as respostas.

O modelo de interação leans no nomenclature estabelecido pelo muitas outras APIs de rede. Não há um serviço de escuta que primeiro indica preparação para processar ligações de entrada e, subsequentemente, aceita-las à medida que chegam. No outro lado, um cliente liga-se para o serviço de escuta, era esperado essa ligação sejam aceites para estabelecer um caminho de comunicação bidirecional. "Ligar", "Escuta" e "Aceitar" é mesmos termos que encontrará no socket a maioria das APIs.

Qualquer modelo de comunicação retransmitidas tem a efetuar ligações de saída para um ponto final do serviço de terceiros. Isto torna o "serviço de escuta" também um "cliente" em utilização colloquial e também pode fazer com que outras sobrecargas terminologia. Segue-se a terminologia exatos, por conseguinte, é utilizada para as ligações híbridas:

Os programas em ambos os lados de uma ligação são denominados "clientes", uma vez que estes clientes para o serviço. O cliente que aguarda e aceita ligações é o "serviço de escuta" ou possui correspondências é denominado "função de serviço de escuta." O cliente que inicia uma nova ligação para um serviço de escuta através do serviço é chamado "remetente", ou está na "função de remetente".

### <a name="listener-interactions"></a>Interações do serviço de escuta

O serviço de escuta tem cinco interações com o serviço; todos os detalhes de transmissão são descritos neste artigo na secção de referência.

As mensagens de escuta, aceitar e pedido são recebidas a partir do serviço. A renovação e operações de Ping são enviadas pelo serviço de escuta.

#### <a name="listen-message"></a>Escutar a mensagem

Para indicar a preparação para o serviço que um serviço de escuta está pronto para aceitar ligações, cria uma ligação de WebSocket saída. O handshake de ligação acarreta o nome de uma ligação híbrida configurado no espaço de nomes de reencaminhamento e um token de segurança que confers "Escuta" correto esse nome.

Quando o WebSocket é aceite pelo serviço, o registo é concluído e o WebSocket estabelecido é mantido activa como "canal de controlo" para ativar todas as interações subsequentes. O serviço permite até 25 escuta em simultâneo uma ligação híbrida. É possível determinar a quota para AppHooks.

Para as ligações híbridas, se existirem dois ou mais ativos os serviços de escuta, ligações de entrada são equilibradas entre-los na ordem aleatória; distribuição justa é tentada com melhor esforço.

#### <a name="accept-message"></a>Aceitar a mensagem

Quando um remetente abre uma nova ligação do serviço, o serviço escolhe e notifica um dos serviços de escuta de Active Directory na ligação híbrida. Esta notificação é enviada para o serviço de escuta através do canal de controlo aberto como uma mensagem JSON. A mensagem contém o URL do ponto final de WebSocket que o serviço de escuta tem de ligar para aceitar a ligação.

O URL pode e tem de ser utilizado diretamente pelo serviço de escuta sem qualquer trabalho adicional.
As informações codificadas só são válidas durante um curto período de tempo, essencialmente, desde que o remetente está disposto a aguardar que a ligação ser estabelecida ponto-a-ponto. O máximo assuma é 30 segundos. O URL só pode ser utilizado para a tentativa de uma ligação com êxito. Assim que a ligação de WebSocket com o URL de encontro é estabelecida, toda a atividade adicional neste WebSocket é retransmitida de e para o remetente. Isto acontece sem qualquer intervenção ou a interpretação pelo serviço.

### <a name="request-message"></a>Mensagem de pedido

Para além das ligações de WebSocket, o serviço de escuta pode também receber frames de pedido HTTP de um remetente, se esta capacidade é explicitamente ativada na ligação híbrida.

Serviços de escuta que ligar às ligações híbridas com suporte para HTTP devem processar o `request` gesto. Um serviço de escuta não processa `request` e, por conseguinte, faz com que a erros de tempo limite repetidos enquanto a ser ligado pode ser blacklisted pelo serviço no futuro.

Metadados de cabeçalho de moldura HTTP é convertido JSON para processamento de mais simples, a arquitetura do serviço de escuta, também porque bibliotecas de análise de cabeçalho HTTP são rarer que parsers JSON. Metadados HTTP que só são relevantes para a relação entre o remetente e o gateway de reencaminhamento HTTP, incluindo informações de autorização, não serão reencaminhados. Corpos de pedido HTTP são transferidos transparente como binários frames de WebSocket.

O serviço de escuta pode responder a pedidos de HTTP utilizando um gesto de resposta equivalente.

O fluxo de pedido/resposta utiliza o canal de controlo, por predefinição, mas pode ser "Atualizar" para um encontro distinto WebSocket sempre que necessário. Ligações de WebSocket distintas melhorar o débito para cada conversação de cliente, mas estes burden o serviço de escuta com mais ligações que precisam de ser processada, que não pode ser desire capaz para clientes simples.

O canal de controlo, corpos de pedido e resposta estão limitados a um máximo de 64 kB de tamanho. Metadados de cabeçalho HTTP estão limitado a um total de 32 kB. Se o pedido ou resposta exceder esse limiar, o serviço de escuta tem de atualizar para um WebSocket utilizar um gesto de equivalente ao processamento de encontro a [aceitar](#accept-message).

Para pedidos, o serviço decide se encaminhar pedidos através do canal de controlo. Isto inclui, mas não pode ser limitado para casos em que um pedido excede 64 kB (cabeçalhos mais corpo) imediato ou se o pedido é enviado com ["a" codificação de transferência](https://tools.ietf.org/html/rfc7230#section-4.1) e o serviço tem razão esperar para o pedido exceder 64 kB ou ler o pedido não é instantânea. Se o serviço de optar por fornecer o pedido através de encontro, transmite apenas o endereço de encontro para o serviço de escuta.
O serviço de escuta, em seguida, tem de estabelecer o encontro WebSocket e o serviço de entrega retomadas rapidamente o pedido completo, incluindo corpos através de encontro WebSocket. A resposta também tem de utilizar o encontro WebSocket.

Para pedidos que chegam através do canal de controlo, o serviço de escuta decide se responder através do canal de controlo ou através de encontro. O serviço tem de incluir um endereço de encontro com cada pedido encaminhado através do canal de controlo. Este endereço só é válido para o pedido atual a atualizar.

Se o serviço de escuta optar por atualizar, estabelece ligação e oferece imediatamente a resposta ao longo do socket encontro estabelecida.

Uma vez o encontro que websocket tiver sido estabelecida, o serviço de escuta deve manter para obter mais processamento de pedidos e respostas do mesmo cliente. O serviço irá manter o WebSocket para desde que o HTTPS socket ligação com o remetente persistir e encaminhará todos os pedidos subsequentes desse remetente através de WebSocket manter. Se o serviço de escuta optar por remover o encontro WebSocket do seu lado, o serviço também irá remover a ligação para o remetente, independentemente se um pedido subsequente pode já estar em curso.

#### <a name="renew-operation"></a>Renovar operação

O token de segurança que tem de ser utilizado para registar o serviço de escuta e manter o canal de controlo pode expirar enquanto o serviço de escuta está ativo. A expiração do token não afeta as ligações em curso, mas fazer com que o canal de controlo ser removido pelo serviço, ou logo após o momento de expiração. A operação "renovar" é uma mensagem JSON que o serviço de escuta pode enviar para substituir o token associado com o canal de controlo, para que o canal de controlo pode ser mantido por períodos prolongados.

#### <a name="ping-operation"></a>Operação de ping

Se o canal de controlo permanece inativo durante muito tempo, intermediários na forma, tais como a carga balanceadores ou os NATs podem remover a ligação de TCP. A operação de "ping" evita que enviando uma pequena quantidade de dados no canal que relembra todas as pessoas a rota de rede que a ligação se destinar a ser alive, e também serve como um teste de "dinâmicos" para o serviço de escuta. Se o ping falhar, o canal de controlo deve ser considerado inutilizável e o serviço de escuta deve voltar a ligar.

### <a name="sender-interaction"></a>Interação de remetente

O remetente tem dois interações com o serviço: se liga um Socket Web ou envia pedidos através de HTTPS. Não não possível enviar pedidos através de um Socket Web, da função de remetente.

#### <a name="connect-operation"></a>A operação de ligação

A operação "ligar" abre um WebSocket no serviço, fornecendo o nome da ligação híbrida e (opcionalmente, mas necessário por predefinição) token de segurança conferring permissão "Enviar" na cadeia de consulta. O serviço, em seguida, interage com o serviço de escuta da forma descrito anteriormente, e o serviço de escuta cria uma ligação de encontro que está associada este WebSocket. Depois de ter sido aceite o WebSocket, todas as interações adicionais em que WebSocket são com um serviço de escuta ligado.

#### <a name="request-operation"></a>Operação de pedido

Para as ligações híbridas para o qual foi ativada a funcionalidade, o remetente pode enviar pedidos de HTTP amplamente sem restrições para os serviços de escuta.

Exceto um token de acesso de reencaminhamento que é que um embedded na cadeia de consulta ou num cabeçalho de HTTP do pedido, o reencaminhamento é completamente transparente para todas as operações de HTTP no endereço de reencaminhamento e todos os sufixos do caminho de endereço de reencaminhamento, abandonar o fileparser o serviço de escuta completamente no controlo dos en autorização de d-a-ponto e as funcionalidades de extensão do mesmo HTTP como [CORS](https://www.w3.org/TR/cors/).

Autorização de remetente com o ponto final de reencaminhamento está ativada por predefinição, mas é opcional. O proprietário da ligação híbrida pode optar por permitir que os remetentes anónimos. O serviço irá intercetar, Inspecione e eliminar as informações de autorização da seguinte forma:

1. Se a cadeia de consulta contém um `sb-hc-token` expressão, a expressão será sempre ser eliminada da cadeia de consulta. Esta é avaliada se a autorização de reencaminhamento está ativada.
2. Se os cabeçalhos do pedido contém um `ServiceBusAuthorization` cabeçalho, o cabeçalho de expressão será sempre ser eliminada da coleção de cabeçalho.
   Esta é avaliada se a autorização de reencaminhamento está ativada.
3. Apenas se a autorização de reencaminhamento está ativada e, se os cabeçalhos do pedido contém um `Authorization` cabeçalho e nenhuma das expressões anteriores está presente, o cabeçalho será avaliado e repartido. Caso contrário, o `Authorization`sempre é transmitida como-é.

Se não houver nenhuma escuta de Active Directory, o serviço irá devolver um código de erro 502 "Gateway incorreto". Se o serviço não aparecer processar o pedido, o serviço irá devolver um 504 "tempo limite do Gateway" após 60 segundos.

### <a name="interaction-summary"></a>Interação resumo

O resultado deste modelo de interação é que o cliente do remetente vem fora do handshake com um WebSocket "Limpar", que está ligado a um serviço de escuta e que não necessita de nenhum mais preambles ou preparação. Este modelo permite praticamente qualquer implementação de cliente WebSocket existente prontamente tirar partido do serviço de ligações híbridas, fornecendo um URL construído corretamente na respetiva camada de cliente WebSocket.

A ligação de encontro WebSocket que o serviço de escuta obtém através da interação aceitar também está limpa e pode ser entregar para qualquer implementação de servidor de WebSocket existente com alguns abstração extra mínima que distingue entre operações de "aceitar" nos serviços de escuta do respetivo framework rede local e as operações de remoto "aceitar" ligações híbridas.

O modelo de pedido/resposta HTTP fornece o remetente uma grande parte sem restrições HTTP protocolo área de superfície com uma camada de autorização opcional. O serviço de escuta obtém uma previamente analisada HTTP pedido cabeçalho secção que pode ser ativada novamente para um pedido HTTP a jusante ou processada como for, com frames binários as devidas corpos de HTTP. Respostas de utilizam o mesmo formato. Interações com menos de 64 KB de pedido e resposta de corpo podem ser processadas através de um único Socket Web que é partilhada por todos os remetentes. Maior pedidos e respostas podem ser processadas utilizando o modelo de encontro.

## <a name="protocol-reference"></a>Referência do protocolo

Esta secção descreve os detalhes das interações de protocolo descritos anteriormente.

Todas as ligações de WebSocket que são efetuadas na porta 443 como uma atualização do 1.1 de HTTPS, o que é normalmente abstracted por alguns framework WebSocket ou API. A descrição aqui é mantida implementação independente, sem sugerindo uma arquitetura específica.

### <a name="listener-protocol"></a>Protocolo de serviço de escuta

O protocolo serviço de escuta é constituído por dois gestos de ligação e três operações de mensagem.

#### <a name="listener-control-channel-connection"></a>Ligação de canal de controlo do serviço de escuta

O canal de controlo é aberto com criação de uma ligação de WebSocket para:

`wss://{namespace-address}/$hc/{path}?sb-hc-action=...[&sb-hc-id=...]&sb-hc-token=...`

O `namespace-address` é o nome de domínio completamente qualificado do espaço de nomes do reencaminhamento do Azure que aloja a ligação híbrida, normalmente com a forma `{myname}.servicebus.windows.net`.

As opções de parâmetros de cadeia de consulta são os seguintes.

| Parâmetro        | Necessário | Descrição
| ---------------- | -------- | -------------------------------------------
| `sb-hc-action`   | Sim      | Para a função de serviço de escuta, o parâmetro tem de ser **sb-hc-action = escuta**
| `{path}`         | Sim      | O caminho de espaço de nomes com codificação URL da ligação híbrida previamente configurada para registar este serviço de escuta no. É acrescentada esta expressão para o fixo `$hc/` parte do caminho.
| `sb-hc-token`    | Sim\*    | O serviço de escuta tem de fornecer um válido, com codificação URL Service Bus partilhado acesso Token para o espaço de nomes ou da ligação híbrida que confers o **escutar** à direita.
| `sb-hc-id`       | Não       | Este ID fornecido pelo cliente opcional que permite que o rastreio de diagnóstico ponto-a-ponto.

Se a ligação de WebSocket falhar devido ao caminho da ligação híbrida não a ser registado, ou um token inválido ou em falta ou alguns outros erros, os comentários de erro é fornecido com o modelo de comentários de estado de HTTP 1.1 regular. A descrição de estado contém um controlo-id de erro que pode ser comunicado ao pessoal de suporte do Azure:

| Código | Erro          | Descrição
| ---- | -------------- | -------------------------------------------------------------------
| 404  | Não Encontrado      | O caminho da ligação híbrida é inválido ou o URL de base tem um formato incorreto.
| 401  | Não autorizado   | O token de segurança está em falta ou com formato incorreto ou é inválido.
| 403  | Proibido      | O token de segurança não é válido para este caminho para esta ação.
| 500  | Erro interno | Ocorreu um erro no serviço.

Se a ligação de WebSocket é intencionalmente encerrada pelo serviço depois de este foi inicialmente configurada, o motivo para fazê-lo pelo que é comunicado utilizando um código de erro de protocolo de WebSocket adequado, juntamente com uma mensagem de erro descritivo também inclui um ID de controlo. O serviço irá não encerrar o canal de controlo sem encontrar uma condição de erro. Qualquer encerramento correto é controlado de cliente.

| Estado de WS | Descrição
| --------- | -------------------------------------------------------------------------------
| 1001      | O caminho da ligação híbrida foi eliminado ou desativado.
| 1008      | O token de segurança expirou, pelo que a política de autorização é violada.
| 1011      | Ocorreu um erro no serviço.

#### <a name="accept-handshake"></a>Aceitar handshake

A notificação "aceitar" é enviada pelo serviço para o serviço de escuta, através do canal de controlo anteriormente estabelecido como uma mensagem JSON num intervalo de texto de WebSocket. Não há nenhuma resposta a esta mensagem.

A mensagem contém um objeto JSON com o nome "aceitar", que define as propriedades seguintes neste momento:

* **endereço** – a cadeia de URL a utilizar para estabelecer o WebSocket para o serviço para aceitar uma ligação recebida.
* **ID** – o identificador exclusivo para esta ligação. Se o ID de foi fornecido pelo cliente de remetente, que é o valor do remetente fornecido, caso contrário, é um valor de gerada pelo sistema.
* **connectHeaders** – todos os cabeçalhos HTTP que foi fornecidos para o ponto final de reencaminhamento pelo remetente, que também inclui o protocolo de WebSocket seg e os cabeçalhos de extensões de WebSocket de seg.

```json
{
    "accept" : {
        "address" : "wss://dc-node.servicebus.windows.net:443/$hc/{path}?..."
        "id" : "4cb542c3-047a-4d40-a19f-bdc66441e736",
        "connectHeaders" : {
            "Host" : "...",
            "Sec-WebSocket-Protocol" : "...",
            "Sec-WebSocket-Extensions" : "..."
        }
     }
}
```

O URL de endereço fornecido na mensagem JSON é utilizado pelo serviço de escuta para estabelecer o WebSocket para aceitar ou rejeitar o socket do remetente.

##### <a name="accepting-the-socket"></a>Ao aceitar o socket

Para aceitar, o serviço de escuta estabelece uma ligação de WebSocket para o endereço fornecido.

Se a mensagem "aceitar" acarreta um `Sec-WebSocket-Protocol` cabeçalho, é esperado que o serviço de escuta só aceita o WebSocket se suporta esse protocolo. Além disso, define o cabeçalho como o WebSocket é estabelecido.

O mesmo se aplica para o `Sec-WebSocket-Extensions` cabeçalho. Se a estrutura suporta uma extensão, defini-lo deve o cabeçalho de resposta de lado do servidor de necessários `Sec-WebSocket-Extensions` handshake para a extensão.

O URL tem de ser utilizado como-é para estabelecer o socket de aceitar, mas contém os seguintes parâmetros:

| Parâmetro      | Necessário | Descrição
| -------------- | -------- | -------------------------------------------------------------------
| `sb-hc-action` | Sim      | Para aceitar um socket, o parâmetro tem de ser `sb-hc-action=accept`
| `{path}`       | Sim      | (consulte o parágrafo seguinte)
| `sb-hc-id`     | Não       | Ver descrição anterior **id**.

`{path}` é o caminho de espaço de nomes com codificação URL da ligação híbrida pré-configurada em que pretende registar este serviço de escuta. É acrescentada esta expressão para o fixo `$hc/` parte do caminho.

O `path` expressão pode ser expandida com um sufixo e uma expressão de cadeia de consulta que se segue o nome registado após uma barra separating.
Isto permite que o cliente do remetente passar os argumentos de distribuição para o serviço de escuta aceitar quando não é possível incluir cabeçalhos de HTTP. As expectativas são de que a arquitetura do serviço de escuta analisa a parte do caminho fixo e o nome do caminho de registado e faz com que o resto, possivelmente sem quaisquer argumentos de cadeia de consulta como prefixo `sb-`, disponível para a aplicação para decidir se pretende aceitar a ligação.

Para obter mais informações, consulte a secção "Remetente protocolo".

Se houver um erro, o serviço pode responder a forma:

| Código | Erro          | Descrição
| ---- | -------------- | -----------------------------------
| 403  | Proibido      | O URL não é válido.
| 500  | Erro interno | Ocorreu um erro no serviço

 Depois da ligação for estabelecida, o servidor encerra o WebSocket quando o remetente WebSocket encerrado para baixo, ou com o estado do seguinte:

| Estado de WS | Descrição                                                                     |
| --------- | ------------------------------------------------------------------------------- |
| 1001      | O cliente do remetente será encerrado a ligação.                                    |
| 1001      | O caminho da ligação híbrida foi eliminado ou desativado.                        |
| 1008      | O token de segurança expirou, pelo que a política de autorização é violada. |
| 1011      | Ocorreu um erro no serviço.                                            |

##### <a name="rejecting-the-socket"></a>Rejeitar o socket

 Rejeitar o socket após inspecionar o `accept` mensagem requer um handshake semelhante para que o código de estado e a descrição de Estado comunicar o motivo para a rejeição possam circular de volta para o remetente.

 A escolha de design do protocolo está a utilizar um handshake de WebSocket (que foi concebido para terminar com um Estado de erro definido), para que as implementações de cliente do serviço de escuta pode continuar a depender de um cliente WebSocket e não precisa de utilizar um extra, bare cliente HTTP.

 Rejeitar o socket, o cliente demora o endereço URI do `accept` mensagem e acrescenta dois parâmetros de cadeia de consulta, da seguinte forma:

| Param                   | Necessário | Descrição                              |
| ----------------------- | -------- | ---------------------------------------- |
| SB-hc-statusCode        | Sim      | Código de estado HTTP numérico.                |
| SB-hc-statusDescription | Sim      | Motivo legível humano a rejeição. |

O URI resultante, em seguida, é utilizado para estabelecer uma ligação de WebSocket.

Quando concluir corretamente, este handshake intencionalmente ocorre uma falha com um código de erro HTTP 410, desde que foi estabelecida não WebSocket. Se houver algum problema, os códigos seguintes descrevem o erro:

| Código | Erro          | Descrição                          |
| ---- | -------------- | ------------------------------------ |
| 403  | Proibido      | O URL não é válido.                |
| 500  | Erro interno | Ocorreu um erro no serviço. |

#### <a name="request-message"></a>Mensagem de pedido

O `request` é enviado pelo serviço para o serviço de escuta através do canal de controlo. A mesma mensagem também é enviada através de encontro WebSocket depois de estabelecido.

O `request` consiste em duas partes: um frame(s) de corpo do cabeçalho e binary.
Se não houver nenhum corpo, as frames Jumbo corpo estão omitidos. O indicador para se estiver presente um corpo é o booleano `body` propriedade na mensagem de pedido.

Para um pedido com um corpo do pedido, a estrutura poderá ter o seguinte aspeto:

``` text
----- Web Socket text frame ----
{
    "request" :
    {
        "body" : true,
        ...
    }
}
----- Web Socket binary frame ----
FEFEFEFEFEFEFEFEFEFEF...
----- Web Socket binary frame ----
FEFEFEFEFEFEFEFEFEFEF...
----- Web Socket binary frame -FIN
FEFEFEFEFEFEFEFEFEFEF...
----------------------------------
```

O serviço de escuta deve processar o corpo do pedido dividir por vários frames binários a receber (consulte [fragmentos de WebSocket](https://tools.ietf.org/html/rfc6455#section-5.4)).
O pedido termina quando foi recebida uma moldura binária com o sinalizador FIN definido.

Para um pedido sem um corpo, é apenas uma moldura de texto.

``` text
----- Web Socket text frame ----
{
    "request" :
    {
        "body" : false,
        ...
    }
}
----------------------------------
```

O conteúdo JSON `request` é o seguinte:

* **endereço** -cadeia URI. Este é o endereço de encontro a utilizar para este pedido. Se o pedido de entrada é superior a 64 kB, o restante período desta mensagem for deixado em branco e o cliente tem de iniciar um handshake de encontro equivalente para o `accept` operação descrito abaixo. O serviço, em seguida, irá colocar o concluída `request` no socket Web estabelecido. Se a resposta pode ser esperada exceder 64 kB, o serviço de escuta tem também a iniciar um handshake de encontro e, em seguida, transferir a resposta através de socket Web estabelecido.
* **ID** – cadeia. O identificador exclusivo para este pedido.
* **requestHeaders** – este objeto contém todos os cabeçalhos HTTP que foram fornecidos para o ponto final pelo remetente, com exceção das informações de autorização conforme explicado [acima](#request-operation)e cabeçalhos estritamente relacionados com o ligação com o gateway. Especificamente, todos os cabeçalhos definido ou reservado no [RFC7230](https://tools.ietf.org/html/rfc7230), exceto `Via`, estão repartidas e não reencaminhadas:

  * `Connection` (RFC7230, secção 6.1)
  * `Content-Length`  (RFC7230, secção 3.3.2)
  * `Host`  (RFC7230, secção 5.4)
  * `TE`  (RFC7230, secção 4.3)
  * `Trailer`  (RFC7230, secção 4.4)
  * `Transfer-Encoding`  (RFC7230, secção 3.3.1)
  * `Upgrade` (RFC7230, secção 6.7)
  * `Close`  (RFC7230, secção 8.1)

* **requestTarget** – cadeia. Esta propriedade contém o ["Destino do pedido" (RFC7230, secção 5.3)](https://tools.ietf.org/html/rfc7230#section-5.3) do pedido. Isto inclui a parte de cadeia de consulta, que é eliminada de todas as `sb-hc-` prefixo parâmetros.
* **método** -cadeia. Este é o método de pedido, por [RFC7231, secção 4](https://tools.ietf.org/html/rfc7231#section-4). O `CONNECT` método não podem ser utilizado.
* **corpo** – booleano. Indica se uma moldura de corpo mais mais binária segue.

``` JSON
{
    "request" : {
        "address" : "wss://dc-node.servicebus.windows.net:443/$hc/{path}?...",
        "id" : "42c34cb5-7a04-4d40-a19f-bdc66441e736",
        "requestTarget" : "/abc/def?myarg=value&otherarg=...",
        "method" : "GET",
        "requestHeaders" : {
            "Host" : "...",
            "Content-Type" : "...",
            "User-Agent" : "..."
        },
        "body" : true
     }
}
```

##### <a name="responding-to-requests"></a>Responder a pedidos

O recetor tem de responder. Falhas repetidas para responder a pedidos enquanto mantém a ligação poderão resultar no serviço de escuta obter blacklisted.

As respostas podem ser enviadas por qualquer ordem, mas cada pedido tem de ser respondeu para durante 60 segundos ou a entrega será reportada como tendo falhou. O prazo de 60 segundo é contabilizado até o `response` moldura foram recebida pelo serviço. Uma resposta em curso com vários frames binários não é possível fiquem inativa durante mais de 60 segundos ou é encerrado.

Se o pedido é recebido através do canal de controlo, a resposta tem seja enviada do canal de controlo de onde o pedido foi recebido ou deve ser enviado através de um canal de encontro.

A resposta é um objeto JSON com o nome "resposta". As regras para processar o conteúdo do corpo são exatamente como com a `request` mensagem e com base no `body` propriedade.

* **requestId** – cadeia. NECESSÁRIO. O `id` valor da propriedade do `request` mensagem a ser emitida.
* **statusCode** – número. NECESSÁRIO. um numérico código de estado do HTTP, que indica o resultado da notificação. Todos os códigos de estado de [RFC7231, 6 de secção](https://tools.ietf.org/html/rfc7231#section-6) são permitidos, exceto para [502 "Gateway incorreto"](https://tools.ietf.org/html/rfc7231#section-6.6.3) e [504 "tempo limite do Gateway"](https://tools.ietf.org/html/rfc7231#section-6.6.5).
* **statusDescription** -cadeia. OPCIONAL. Frase de razão do código de estado HTTP por [RFC7230, secção 3.1.2](https://tools.ietf.org/html/rfc7230#section-3.1.2)
* **responseHeaders** – cabeçalhos HTTP para ser definido uma resposta HTTP externa.
  Tal como com a `request`, RFC7230 definidos cabeçalhos não podem ser utilizados.
* **corpo** – booleano. Indica se o binário corpo frame(s) follow(s).

``` text
----- Web Socket text frame ----
{
    "response" : {
        "requestId" : "42c34cb5-7a04-4d40-a19f-bdc66441e736",
        "statusCode" : "200",
        "responseHeaders" : {
            "Content-Type" : "application/json",
            "Content-Encoding" : "gzip"
        }
         "body" : true
     }
}
----- Web Socket binary frame -FIN
{ "hey" : "mydata" }
----------------------------------
```

##### <a name="responding-via-rendezvous"></a>Responder através de encontro

Para as respostas que excedem 64 kB, a resposta tem de ser entregue ao longo de um socket de encontro. Também, se o pedido exceder 64 kB e o `request` contém apenas o campo de endereço, tem de ser estabelecido um socket de encontro para obter o `request`. Quando um socket de encontro for estabelecido, respostas para o respetivo cliente e os pedidos subsequentes do respetivo cliente tem de ser entregue através de socket encontro enquanto se persistir.

O `address` URL no `request` podem ser utilizados como-é para estabelecer o socket de encontro, mas contém os seguintes parâmetros:

| Parâmetro      | Necessário | Descrição
| -------------- | -------- | -------------------------------------------------------------------
| `sb-hc-action` | Sim      | Para aceitar um socket, o parâmetro tem de ser `sb-hc-action=request`

Se houver um erro, o serviço pode responder a forma:

| Código | Erro           | Descrição
| ---- | --------------- | -----------------------------------
| 400  | Pedido inválido | Action não reconhecida ou URL não é válido.
| 403  | Proibido       | O URL expirou.
| 500  | Erro interno  | Ocorreu um erro no serviço

 Depois da ligação for estabelecida, o servidor encerra o WebSocket quando socket HTTP do cliente encerra, ou com o estado do seguinte:

| Estado de WS | Descrição                                                                     |
| --------- | ------------------------------------------------------------------------------- |
| 1001      | O cliente do remetente será encerrado a ligação.                                    |
| 1001      | O caminho da ligação híbrida foi eliminado ou desativado.                        |
| 1008      | O token de segurança expirou, pelo que a política de autorização é violada. |
| 1011      | Ocorreu um erro no serviço.                                            |


#### <a name="listener-token-renewal"></a>Renovação de token de serviço de escuta

Quando o token de serviço de escuta está prestes a expirar, pode substituir esta enviando uma mensagem de moldura de texto para o serviço através do canal de controlo estabelecida. A mensagem contém um objeto JSON chamado `renewToken`, que define a seguinte propriedade neste momento:

* **token** – um token de acesso de partilhado de barramento de serviço válido, com codificação URL para o espaço de nomes ou da ligação híbrida que confers o **escutar** à direita.

```json
{
  "renewToken": {
    "token":
      "SharedAccessSignature sr=http%3a%2f%2fcontoso.servicebus.windows.net%2fhyco%2f&amp;sig=XXXXXXXXXX%3d&amp;se=1471633754&amp;skn=SasKeyName"
  }
}
```

Se a falha de validação de token, o acesso é negado e o serviço em nuvem se fechar o canal de controlo de WebSocket com um erro. Caso contrário, não há nenhuma resposta.

| Estado de WS | Descrição                                                                     |
| --------- | ------------------------------------------------------------------------------- |
| 1008      | O token de segurança expirou, pelo que a política de autorização é violada. |

### <a name="web-socket-connect-protocol"></a>Protocolo de ligação de Web Socket

O protocolo de remetente é efetivamente idêntico para a forma como um serviço de escuta é estabelecido.
O objetivo é transparência máxima para o WebSocket ponto-a-ponto. O endereço para ligar ao é igual para o serviço de escuta, mas "action" é diferente e o token tem uma permissão diferentes:

```
wss://{namespace-address}/$hc/{path}?sb-hc-action=...&sb-hc-id=...&sbc-hc-token=...
```

O _endereço de espaço de nomes_ é o nome de domínio completamente qualificado do espaço de nomes do reencaminhamento do Azure que aloja a ligação híbrida, normalmente com a forma `{myname}.servicebus.windows.net`.

O pedido pode conter arbitrários Extras os cabeçalhos de HTTP, incluindo aqueles definido pela aplicação. Todos os cabeçalhos fornecidos fluir para o serviço de escuta e pode ser encontrados no `connectHeader` objeto do **aceitar** mensagem de controlo.

As opções de parâmetros de cadeia de consulta são os seguintes:

| Param          | Necessário? | Descrição
| -------------- | --------- | -------------------------- |
| `sb-hc-action` | Sim       | Para a função de remetente, o parâmetro tem de ser `sb-hc-action=connect`.
| `{path}`       | Sim       | (consulte o parágrafo seguinte)
| `sb-hc-token`  | Sim\*     | O serviço de escuta tem de fornecer um válido, com codificação URL Service Bus partilhado acesso Token para o espaço de nomes ou da ligação híbrida que confers o **enviar** à direita.
| `sb-hc-id`     | Não        | Um ID opcional que permite o rastreio de diagnóstico ponto a ponto e é disponibilizado para o serviço de escuta durante o handshake de aceitar.

 O `{path}` é o caminho de espaço de nomes com codificação URL da ligação híbrida pré-configurada em que pretende registar este serviço de escuta. O `path` expressão pode ser expandida com um sufixo e uma expressão de cadeia de consulta para obter mais comunicar. Se a ligação híbrida estiver registada no caminho `hyco`, a `path` expressão pode ser `hyco/suffix?param=value&...` seguido os parâmetros de cadeia de consulta definidos aqui. Uma expressão completa, em seguida, pode ser da seguinte forma:

```
wss://{namespace-address}/$hc/hyco/suffix?param=value&sb-hc-action=...[&sb-hc-id=...&]sbc-hc-token=...
```

O `path` expressão é transferida para o serviço de escuta no endereço URI contido na mensagem de controlo "aceitar".

Se a ligação de WebSocket falhar devido ao caminho da ligação híbrida não registada, um token inválido ou em falta ou alguns outros erros, os comentários de erro é fornecido com o modelo de comentários de estado de HTTP 1.1 regular. A descrição de estado contém um erro de controlo ID que pode ser comunicada ao pessoal de suporte do Azure:

| Código | Erro          | Descrição
| ---- | -------------- | -------------------------------------------------------------------
| 404  | Não Encontrado      | O caminho da ligação híbrida é inválido ou o URL de base tem um formato incorreto.
| 401  | Não autorizado   | O token de segurança está em falta ou com formato incorreto ou é inválido.
| 403  | Proibido      | O token de segurança não é válido para este caminho e para esta ação.
| 500  | Erro interno | Ocorreu um erro no serviço.

Se a ligação de WebSocket foi intencionalmente encerrada pelo serviço depois de ter sido definido inicialmente cópias de segurança, o motivo para fazê-lo pelo que é comunicado utilizando um código de erro de protocolo de WebSocket adequado, juntamente com uma mensagem de erro descritivo também inclui um ID de controlo.

| Estado de WS | Descrição
| --------- | ------------------------------------------------------------------------------- 
| 1000      | O serviço de escuta encerrar o socket.
| 1001      | O caminho da ligação híbrida foi eliminado ou desativado.
| 1008      | O token de segurança expirou, pelo que a política de autorização é violada.
| 1011      | Ocorreu um erro no serviço.

### <a name="http-request-protocol"></a>Protocolo de pedido HTTP

O protocolo de pedido HTTP permite pedidos HTTP arbitrários, exceto as atualizações de protocolo.
Pedidos de HTTP são indicados no endereço de tempo de execução regular da entidade, sem o infix $hc que é utilizado para as ligações híbridas clientes de WebSocket.

```
https://{namespace-address}/{path}?sbc-hc-token=...
```

O _endereço de espaço de nomes_ é o nome de domínio completamente qualificado do espaço de nomes do reencaminhamento do Azure que aloja a ligação híbrida, normalmente com a forma `{myname}.servicebus.windows.net`.

O pedido pode conter arbitrários Extras os cabeçalhos de HTTP, incluindo aqueles definido pela aplicação. Indicar todos os cabeçalhos, exceto aos que estão diretamente definido no RFC7230 (consulte [mensagem de pedido](#Request message)) fluir para o serviço de escuta e pode ser encontrado no `requestHeader` objeto do **pedido** mensagem.

As opções de parâmetros de cadeia de consulta são os seguintes:

| Param          | Necessário? | Descrição
| -------------- | --------- | ---------------- |
| `sb-hc-token`  | Sim\*     | O serviço de escuta tem de fornecer um válido, com codificação URL Service Bus partilhado acesso Token para o espaço de nomes ou da ligação híbrida que confers o **enviar** à direita.

O token também pode ser executado no `ServiceBusAuthorization` ou `Authorization` cabeçalho de HTTP. O token pode ser omitido se a ligação híbrida estiver configurada para permitir pedidos anónimos.

Porque o serviço de forma eficaz age como um proxy, mesmo que não como um proxy HTTP for VERDADEIRO, ou adiciona um `Via` cabeçalho ou annotates existente `Via` cabeçalho em conformidade com [RFC7230, secção 5.7.1](https://tools.ietf.org/html/rfc7230#section-5.7.1).
O serviço adiciona o nome de anfitrião de espaço de nomes de reencaminhamento para `Via`.

| Código | Mensagem  | Descrição                    |
| ---- | -------- | ------------------------------ |
| 200  | OK       | O pedido foi processou pelo menos, um serviço de escuta.  |
| 202  | Aceite | O pedido foi aceite pelo menos, um serviço de escuta. |

Se houver um erro, o serviço pode responder a forma. Indica se a resposta origina do serviço ou do serviço de escuta pode ser identificado através de presença de `Via` cabeçalho. Se o cabeçalho estiver presente, a resposta tem do serviço de escuta.

| Código | Erro           | Descrição
| ---- | --------------- |--------- |
| 404  | Não Encontrado       | O caminho da ligação híbrida é inválido ou o URL de base tem um formato incorreto.
| 401  | Não autorizado    | O token de segurança está em falta ou com formato incorreto ou é inválido.
| 403  | Proibido       | O token de segurança não é válido para este caminho e para esta ação.
| 500  | Erro interno  | Ocorreu um erro no serviço.
| 503  | Gateway incorrecto     | O pedido não foi possível ser encaminhado para qualquer serviço de escuta.
| 504  | Tempo limite do gateway | O pedido foi encaminhado para um serviço de escuta, mas o serviço de escuta não reconheceu recibo dentro do tempo necessário.

## <a name="next-steps"></a>Passos Seguintes

* [FAQ de Reencaminhamento](relay-faq.md)
* [Criar um espaço de nomes](relay-create-namespace-portal.md)
* [Introdução ao .NET](relay-hybrid-connections-dotnet-get-started.md)
* [Introdução ao Node](relay-hybrid-connections-node-get-started.md)
