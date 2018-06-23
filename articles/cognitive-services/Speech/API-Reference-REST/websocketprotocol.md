---
title: O reconhecimento de voz Microsoft protocolo WebSocket | Microsoft Docs
description: Documentação do protocolo para o serviço de reconhecimento de voz com base no WebSockets
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.component: bing-speech
ms.topic: article
ms.date: 09/15/2017
ms.author: zhouwang
ms.openlocfilehash: 17954536e8bdb49c09204c2e522586b79cb1bef5
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352316"
---
# <a name="speech-service-websocket-protocol"></a>Protocolo de WebSocket do serviço de reconhecimento de voz

  Serviço de reconhecimento de voz é uma plataforma baseado na nuvem que inclui os algoritmos mais avançados disponíveis para áudio ditas converter em texto. O protocolo serviço de reconhecimento de voz define o [configuração de ligação](#connection-establishment) entre as aplicações de cliente e o serviço e as mensagens de reconhecimento de voz trocadas entre homólogos ([cliente-teve origem mensagens](#client-originated-messages)e [teve origem serviço mensagens](#service-originated-messages)). Além disso, [mensagens de telemetria](#telemetry-schema) e [processamento de erros](#error-handling) descritas.

## <a name="connection-establishment"></a>Estabelecimento da ligação

O protocolo serviço de reconhecimento de voz segue a especificação de padrão de WebSocket [IETF RFC 6455](https://tools.ietf.org/html/rfc6455). Uma ligação de WebSocket começa como um pedido HTTP que contém os cabeçalhos de HTTP que indiquem a intenção do cliente de atualizar a ligação para um WebSocket em vez de utilizar a semântica HTTP. O servidor indica o willingness participar na ligação de WebSocket devolvendo um HTTP `101 Switching Protocols` resposta. Após a troca deste handshake, o cliente e o serviço, mantém o socket aberta e começam a utilizar um protocolo baseado na mensagem para enviar e receber informações.

Para iniciar o handshake de WebSocket, a aplicação cliente envia um pedido GET de HTTPS para o serviço. Inclui padrão cabeçalhos de atualização de WebSocket juntamente com outros cabeçalhos que são específicos para reconhecimento de voz.

```HTTP
GET /speech/recognition/interactive/cognitiveservices/v1 HTTP/1.1
Host: speech.platform.bing.com
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==
Sec-WebSocket-Version: 13
Authorization: t=EwCIAgALBAAUWkziSCJKS1VkhugDegv7L0eAAJqBYKKTzpPZOeGk7RfZmdBhYY28jl&p=
X-ConnectionId: A140CAF92F71469FA41C72C7B5849253
Origin: https://speech.platform.bing.com
```

O serviço responde com:

```HTTP
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: upgrade
Sec-WebSocket-Key: 2PTTXbeeBXlrrUNsY15n01d/Pcc=
Set-Cookie: SpeechServiceToken=AAAAABAAWTC8ncb8COL; expires=Wed, 17 Aug 2016 15:39:06 GMT; domain=bing.com; path="/"
Date: Wed, 17 Aug 2016 15:03:52 GMT
```

Todos os pedidos de reconhecimento de voz requerem o [TLS](https://en.wikipedia.org/wiki/Transport_Layer_Security) encriptação. Não é suportada a utilização de pedidos de reconhecimento de voz não encriptada. A versão do TLS seguinte é suportada:

* TLS 1.2

### <a name="connection-identifier"></a>Identificador de ligação

Serviço de reconhecimento de voz requer que todos os clientes incluem um ID exclusivo para identificar a ligação. Os clientes *tem* incluem o *X ConnectionId* cabeçalho quando são iniciados um handshake de WebSocket. O *X ConnectionId* cabeçalho tem de ser um [Identificador exclusivo universalmente](https://en.wikipedia.org/wiki/Universally_unique_identifier) valor (UUID). Pedidos de atualização de WebSocket que não incluam o *X ConnectionId*, não especifique um valor para o *X ConnectionId* cabeçalho, ou não incluir um valor UUID são rejeitados pelo serviço com um HTTP `400 Bad Request` resposta.

### <a name="authorization"></a>Autorização

Para além dos cabeçalhos de handshake de WebSocket padrão, os pedidos de reconhecimento de voz solicitar um *autorização* cabeçalho. Pedidos de ligação sem este cabeçalho são rejeitados pelo serviço com um HTTP `403 Forbidden` resposta.

O *autorização* cabeçalho tem de conter um token de acesso de Token Web JSON (JWT).

Para obter informações sobre como subscrever e obter chaves de API que são utilizadas para obter os tokens de acesso do válido JWT, consulte o [subscrição de serviços cognitivos](https://azure.microsoft.com/try/cognitive-services/) página.

A chave de API é passada para o serviço de tokens. Por exemplo:

``` HTTP
POST https://api.cognitive.microsoft.com/sts/v1.0/issueToken
Content-Length: 0
```

As seguintes informações de cabeçalho são necessárias para acesso de token.

| Nome | Formato | Descrição |
|----|----|----|
| OCP Apim-subscrição-chave | ASCII | Chave de subscrição |

O serviço de token devolve o token de acesso JWT como `text/plain`. Em seguida, o JWT é transmitida como um `Base64 access_token` para o handshake como um *autorização* prefixo com a cadeia de cabeçalho `Bearer`. Por exemplo:

`Authorization: Bearer [Base64 access_token]`

### <a name="cookies"></a>Cookies

Os clientes *tem* suportam cookies HTTP, tal como especificado nas [RFC 6265](https://tools.ietf.org/html/rfc6265).

### <a name="http-redirection"></a>Redirecionamento de HTTP

Os clientes *tem* suporta os mecanismos de redirecionamento padrão especificados pelo [a especificação de protocolo HTTP](http://www.w3.org/Protocols/rfc2616/rfc2616.html).

### <a name="speech-endpoints"></a>Pontos finais de reconhecimento de voz

Os clientes *tem* utilizar um ponto final adequado do serviço de reconhecimento de voz. O ponto final é baseado no idioma e modo de reconhecimento. A tabela mostra alguns exemplos.

| Modo | Caminho | URI de serviço |
| -----|-----|-----|
| Interativo | /Speech/Recognition/Interactive/cognitiveservices/v1 |https://speech.platform.bing.com/speech/recognition/interactive/cognitiveservices/v1?language=pt-BR |
| Conversação | /Speech/Recognition/conversation/cognitiveservices/v1 |https://speech.platform.bing.com/speech/recognition/conversation/cognitiveservices/v1?language=en-US |
| ditado | /Speech/Recognition/Dictation/cognitiveservices/v1 |https://speech.platform.bing.com/speech/recognition/dictation/cognitiveservices/v1?language=fr-FR |

Para obter mais informações, consulte o [URI de serviço](../GetStarted/GetStartedREST.md#service-uri) página.

### <a name="report-connection-problems"></a>Problemas de ligação do relatório

Os clientes imediatamente devem reportar todos os problemas encontrados ao efetuar uma nova ligação. O protocolo de mensagem para ligações de falha na criação de relatórios está descrito em [telemetria de falhas de ligação](#connection-failure-telemetry).

### <a name="connection-duration-limitations"></a>Limitações de duração da ligação

Quando comparados com ligações de HTTP do serviço web típica, ligações de WebSocket última um *longo* tempo. Serviço de reconhecimento de voz coloca limitações na duração das ligações de WebSocket para o serviço:

 * A duração máxima de qualquer ligação de WebSocket ativa é de 10 minutos. Uma ligação está activa se o serviço ou o cliente envia mensagens de WebSocket através dessa ligação. O serviço de termina a ligação sem aviso quando é atingido o limite. Os clientes devem desenvolver cenários de utilizador que não requerem a ligação ao permanecem ativos ou perto da duração de máximo de ligação.

 * A duração máxima para qualquer ligação de WebSocket inativa é de 180 segundos. Uma ligação está inativa se nem o serviço nem o cliente enviou uma mensagem de WebSocket através da ligação. Após a duração máxima de inativa for atingida, o serviço de termina a ligação de WebSocket inativa.

## <a name="message-types"></a>Tipos de mensagens

Depois de é estabelecida uma ligação de WebSocket entre o cliente e o serviço, o cliente e o serviço podem enviar mensagens. Esta secção descreve o formato destas mensagens de WebSocket.

[IETF RFC 6455](https://tools.ietf.org/html/rfc6455) Especifica que as mensagens de WebSocket podem transmitir dados através da utilização de um texto ou uma codificação binária. As codificações de dois utilizam diferentes formatos na transmissão. Cada formato está otimizado para eficiente codificação, transmissão e ao descodificar de payload da mensagem.

### <a name="text-websocket-messages"></a>Mensagens de texto de WebSocket

Mensagens de texto de WebSocket tem um payload de informações textual que é composta por uma secção de cabeçalhos e um corpo separados pelo par de avanço de página de retorno de avanço de duplo familiar utilizado para mensagens de HTTP. E, como mensagens HTTP, mensagens de texto de WebSocket especificar cabeçalhos na *nome: valor* formato separados por um par de avanço de página única-avanço de retorno. Qualquer texto incluído numa mensagem SMS WebSocket *tem* utilizar [UTF-8](https://tools.ietf.org/html/rfc3629) codificação.

Mensagens de texto de WebSocket tem de especificar um caminho de mensagem no cabeçalho de *caminho*. O valor do cabeçalho tem de ser um dos tipos de mensagem de protocolo de reconhecimento de voz definidos posteriormente neste documento.

### <a name="binary-websocket-messages"></a>Mensagens de WebSocket binárias

Mensagens de WebSocket binárias tem um payload de binário. O protocolo serviço de reconhecimento de voz, áudio é transmitido para em recebeu do serviço utilizando mensagens de WebSocket binárias. Todas as outras mensagens são mensagens de texto de WebSocket. 

Como as mensagens de WebSocket de texto, mensagens de WebSocket binárias consistem de um cabeçalho e uma secção de corpo. Especificar os primeiros 2 bytes de mensagem de WebSocket binária, na [big endian](https://en.wikipedia.org/wiki/Endianness) ordenar, o tamanho de número inteiro de 16 bits da secção de cabeçalho. O tamanho da secção de cabeçalho mínima tem 0 bytes. O tamanho máximo é 8.192 bytes. O texto nos cabeçalhos de mensagens de WebSocket binários *tem* utilizar [US-ASCII](https://tools.ietf.org/html/rfc20) codificação.

Cabeçalhos numa mensagem WebSocket binário são codificados no mesmo formato como em mensagens de texto de WebSocket. O *: valor de nome* formato é separado por um par de avanço de página única-avanço de retorno. Mensagens de WebSocket binárias tem de especificar um caminho de mensagem no cabeçalho de *caminho*. O valor do cabeçalho tem de ser um dos tipos de mensagem de protocolo de reconhecimento de voz definidos posteriormente neste documento.

O texto e as mensagens de WebSocket binárias são utilizadas no protocolo serviço de reconhecimento de voz. 

## <a name="client-originated-messages"></a>Mensagens de origem do cliente

Depois da ligação for estabelecida, o cliente e o serviço podem começar a enviar mensagens. Esta secção descreve o formato e o payload de mensagens em fila que aplicações cliente enviam para o serviço de reconhecimento de voz. A secção [teve origem serviço mensagens](#service-originated-messages) apresenta as mensagens que têm origem no serviço de reconhecimento de voz e são enviadas para as aplicações de cliente.

As principais mensagens enviadas pelo cliente para os serviços são `speech.config`, `audio`, e `telemetry` mensagens. Antes de considerar cada mensagem em detalhe, comuns necessário cabeçalhos de mensagens para todos os estas mensagens são descritos.

### <a name="required-message-headers"></a>Cabeçalhos de mensagens necessária

Os cabeçalhos seguintes são necessários para todas as mensagens da origem de cliente.

| Cabeçalho | Valor |
|----|----|
| Caminho | O caminho de mensagem conforme especificado neste documento |
| X-RequestId | UUID no formato "não-dash" |
| X Timestamp | Carimbo de hora do cliente UTC relógio no formato ISO 8601 |

#### <a name="x-requestid-header"></a>Cabeçalho X-RequestId

Pedidos com origem cliente exclusivamente são identificados pelo *X-RequestId* cabeçalho da mensagem. Este cabeçalho é necessário para todas as mensagens da origem de cliente. O *X-RequestId* valor do cabeçalho tem de ser um UUID na forma de "não-dash", por exemplo, *123e4567e89b12d3a456426655440000*. - *Não é possível* estar no formato canónico *123e4567-e89b-12d3-a456-426655440000*. Pedidos sem um *X-RequestId* cabeçalho ou com um valor de cabeçalho que utiliza o formato errado para UUIDs não fazer com que o serviço para terminar a ligação de WebSocket.

#### <a name="x-timestamp-header"></a>Cabeçalho X-Timestamp

Cada mensagem enviada para o serviço de reconhecimento de voz por uma aplicação de cliente *tem* incluem um *X Timestamp* cabeçalho. O valor para este cabeçalho é a hora quando o cliente envia a mensagem. Pedidos sem um *X Timestamp* cabeçalho ou com um valor de cabeçalho que utiliza o formato errado fazer com que o serviço para terminar a ligação de WebSocket.

O *X Timestamp* valor do cabeçalho deve ter o formato 'aaaa'-'MM'-'dd'T' HH': 'mm':'ss '.' fffffffZ' onde 'fffffff' é uma fração de um segundo. Por exemplo, '12,5' significa '12 + 5/10 segundos e '12.526' significa ' 12 plus 526/1000 segundos'. Este formato está em conformidade com [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) e, ao contrário da norma HTTP *data* cabeçalho, pode fornecer uma resolução abaixo dos. As aplicações cliente poderão arredondar carimbos de data / hora para o milissegundo mais próximo. Aplicações de cliente tem de garantir que o relógio do dispositivo com precisão controla tempo utilizando um [servidor protocolo NTP (Network Time)](https://en.wikipedia.org/wiki/Network_Time_Protocol).

### <a name="message-speechconfig"></a>Mensagem `speech.config`

Serviço de reconhecimento de voz tem de saber as características da sua aplicação para fornecer o melhor reconhecimento de voz possíveis. Os dados das características necessárias incluem informações sobre o dispositivo e o sistema operativo que for ligado a sua aplicação. Forneça estas informações no `speech.config` mensagem.

Os clientes *tem* enviar um `speech.config` mensagem imediatamente após estes estabeleçam a ligação ao serviço de reconhecimento de voz e antes de poderem enviar qualquer `audio` mensagens. Terá de enviar um `speech.config` mensagem apenas uma vez por ligação.

| Campo | Descrição |
|----|----|
| Codificação da mensagem de WebSocket | Texto |
| Corpo | O payload como uma estrutura JSON |

#### <a name="required-message-headers"></a>Cabeçalhos de mensagens necessária

| Nome do cabeçalho | Valor |
|----|----|
| Caminho | `speech.config` |
| X Timestamp | Carimbo de hora do cliente UTC relógio no formato ISO 8601 |
| Content-Type | aplicação/json; conjunto de carateres = utf-8 |

Tal como acontece com todas as mensagens com a origem de cliente no protocolo serviço de reconhecimento de voz, o `speech.config` mensagem *tem* incluem um *X Timestamp* cabeçalho que regista o tempo de relógio cliente UTC quando a mensagem foi enviada para o serviço. O `speech.config` mensagem *não* requerem um *X-RequestId* cabeçalho porque esta mensagem não está associada a um pedido de reconhecimento de voz específico.

#### <a name="message-payload"></a>Payload da mensagem
O payload do `speech.config` mensagem é uma estrutura JSON que contém informações sobre a aplicação. O exemplo seguinte mostra esta informação. Informações de contexto de cliente e o dispositivo estão incluídas no *contexto* elemento da estrutura de JSON. 

```JSON
{
  "context": {
    "system": {
      "version": "2.0.12341",
    },
    "os": {
      "platform": "Linux",
      "name": "Debian",
      "version": "2.14324324"
    },
    "device": {
      "manufacturer": "Contoso",
      "model": "Fabrikan",
      "version": "7.341"
      }
    },
  }
}
```

##### <a name="system-element"></a>Elemento de sistema

O elemento de ter do `speech.config` mensagem contém a versão de reconhecimento de voz software SDK utilizada pela aplicação cliente ou dispositivo. O valor é o formato *major.minor.build.branch*. Pode omitir o *ramo* componente se não for aplicável.

##### <a name="os-element"></a>Elemento de SO

| Campo | Descrição | Utilização |
|-|-|-|
| os.Platform | O SO plataforma que aloja a aplicação, por exemplo, Windows, Android, iOS ou Linux |Necessário |
| os.Name | O nome de produto do SO, por exemplo, Debian ou Windows 10 | Necessário |
| os.Version | A versão do SO no formato *major.minor.build.branch* | Necessário |

##### <a name="device-element"></a>Elemento de dispositivo

| Campo | Descrição | Utilização |
|-|-|-|
| Device.Manufacturer | O fabricante de hardware do dispositivo | Necessário |
| Device.Model | O modelo de dispositivo | Necessário |
| Device.Version | A versão do software do dispositivo fornecida pelo fabricante do dispositivo. Este valor Especifica uma versão do dispositivo que pode ser controlado pelo fabricante. | Necessário |

### <a name="message-audio"></a>Mensagem `audio`

Enviam áudio aplicações de cliente com capacidade de reconhecimento de voz para reconhecimento de voz serviço convertendo a sequência de áudio para uma série de segmentos de áudio. Cada segmento de áudio acarreta um segmento de áudio ditas que está a ser transcribed pelo serviço. O tamanho máximo de um segmento de áudio único é 8.192 bytes. As mensagens da sequência de áudio são *WebSocket binário mensagens*.

Os clientes utilizam o `audio` mensagem a enviar um segmento de áudio para o serviço. Os clientes áudio do microfone em segmentos de leitura e enviar estes segmentos para o serviço de reconhecimento de voz para transcription. O primeiro `audio` mensagem tem de conter um cabeçalho de formado corretamente Especifica que o áudio está em conformidade com um dos formatos de codificação suportados pelo serviço. Adicionais `audio` mensagens contenham apenas dados lidos em microfone de sequência de áudio binário.

Os clientes, opcionalmente, podem enviar um `audio` mensagem com um corpo de comprimento zero. Esta mensagem indica que o serviço que o cliente sabe que o utilizador parado falando, o utterance estiver concluído e microfone está desativada.

Serviço de reconhecimento de voz utiliza o primeiro `audio` mensagem que contém um identificador exclusivo do pedido para assinalar o início de um novo ciclo de pedido/resposta ou *ativar*. Depois do serviço de receber um `audio` mensagem com um novo identificador de pedido, elimina quaisquer mensagens em fila ou enviadas que estão associadas a qualquer ativado anterior.

| Campo | Descrição |
|-------------|----------------|
| Codificação da mensagem de WebSocket | Binário |
| Corpo | Os dados binários para o segmento de áudio. Tamanho máximo é 8.192 bytes. |

#### <a name="required-message-headers"></a>Cabeçalhos de mensagens necessária

Os cabeçalhos seguintes são necessários para todos os `audio` mensagens.

| Cabeçalho         |  Valor     |
| ------------- | ---------------- |
| Caminho | `audio` |
| X-RequestId | UUID no formato "não-dash" |
| X Timestamp | Carimbo de hora do cliente UTC relógio no formato ISO 8601 |
| Content-Type | O tipo de conteúdo de áudio. O tipo tem de ser um *áudio/x-wav* (PCM) ou *áudio/silk* (SILK). |

#### <a name="supported-audio-encodings"></a>Codificações suportadas de áudio

Esta secção descreve o codecs áudio suportado pelo serviço de reconhecimento de voz.

##### <a name="pcm"></a>PCM

Serviço de reconhecimento de voz aceita áudio de modulação (PCM) de código pulse descomprimidos. Áudio é enviado para o serviço no [WAV](https://en.wikipedia.org/wiki/WAV) formatar, pelo que o segmento de áudio primeiro *tem* contêm um [formato de ficheiro de intercâmbio de recurso](https://en.wikipedia.org/wiki/Resource_Interchange_File_Format) cabeçalho (RIFF). Se um cliente inicia uma vez com um segmento de áudio que *não* incluem um cabeçalho de RIFF válido, o serviço rejeita o pedido e termina a ligação de WebSocket.

Áudio PCM *tem* ser objeto de amostragem em 16 kHz com 16 bits por um canal de exemplo e (*riff-16khz-16 bits-mono-pcm*). Serviço de reconhecimento de voz não suporta fluxos de áudio stereo e rejeita os fluxos de áudio não utilizam a taxa de bits especificado, a frequência de amostragem ou o número de canais.

##### <a name="opus"></a>Opus

Opus é um codec aberto, royalty-gratuita, altamente versátil áudio. Serviço de reconhecimento de voz suporta Opus uma taxa de bits de constante de `32000` ou `16000`. Apenas o `OGG` contentor para Opus é atualmente suportado que é especificado pelo `audio/ogg` tipo de mime.

Para utilizar Opus, modifique o [JavaScript exemplo](https://github.com/Azure-Samples/SpeechToText-WebSockets-Javascript/blob/master/samples/browser/Sample.html#L101) e altere o `RecognizerSetup` método para devolver.

```javascript
return SDK.CreateRecognizerWithCustomAudioSource(
            recognizerConfig,
            authentication,
            new SDK.MicAudioSource(
                     new SDK.OpusRecorder(
                     {
                         mimeType: "audio/ogg",
                         bitsPerSecond: 32000
                     }
              )
          ));
```

#### <a name="detect-end-of-speech"></a>Detetar a fim de reconhecimento de voz

Humans não assinalar explicitamente quando se tiver terminados falando. Qualquer aplicação que aceite o reconhecimento de voz como entrada tem duas opções para processamento final de uma sequência de áudio de reconhecimento de voz: deteção de fim de voz e deteção de fim de voz do cliente de serviço. Destas duas opções, a deteção do serviço fim-de-reconhecimento de voz, normalmente, fornece uma melhor experiência de utilizador.

##### <a name="service-end-of-speech-detection"></a>Deteção do serviço fim-de-reconhecimento de voz

Para criar a experiência de reconhecimento de voz de mãos livres ideal, aplicações, permitir que o serviço detetar quando o utilizador terminou falando. Os clientes enviam áudio do microfone como *áudio* segmentos até que o serviço Deteta silêncio e responde novamente com um `speech.endDetected` mensagem.

##### <a name="client-end-of-speech-detection"></a>Deteção de fim de voz do cliente

Aplicações de cliente que permitem ao utilizador assinalar o fim do reconhecimento de voz de alguma forma também podem fornecer o serviço que sinal. Por exemplo, uma aplicação cliente pode ter um "Parar de" ou "Mute" botão que o utilizador pode premir. Sinalizar fim-de-reconhecimento de voz, as aplicações cliente enviar um *áudio* mensagem segmentos com um corpo de comprimento zero. Serviço de reconhecimento de voz interpreta esta mensagem como o fim do fluxo de áudio de entrada.

### <a name="message-telemetry"></a>Mensagem `telemetry`

As aplicações cliente *tem* reconhecer o fim de cada vez através do envio de telemetria sobre o ative ao serviço de reconhecimento de voz. Por sua vez-end confirmação permite que o serviço de reconhecimento de voz para garantir que todas as mensagens necessárias para a conclusão do pedido e a respetiva resposta corretamente foram recebidas pelo cliente. Por sua vez-end confirmação também permite que o serviço de reconhecimento de voz verificar se as aplicações de cliente estão a funcionar conforme esperado. Esta informação é valiosas se precisar de ajuda para a aplicação com capacidade de reconhecimento de voz de resolução de problemas.

Os clientes tem reconhecer o fim da vez enviando um `telemetry` mensagem logo após a receção um `turn.end` mensagem. Os clientes devem tentar a confirmar o `turn.end` logo que possível. Se uma aplicação de cliente não conseguir reconhecer o fim de por sua vez, o serviço de reconhecimento de voz poderá termine a ligação com um erro. Os clientes devem enviar apenas um `telemetry` mensagem para cada pedido e resposta identificado pelo *X-RequestId* valor.

| Campo | Descrição |
| ------------- | ---------------- |
| Codificação da mensagem de WebSocket | Texto |
| Caminho | `telemetry` |
| X Timestamp | Carimbo de hora do cliente UTC relógio no formato ISO 8601 |
| Content-Type | `application/json` |
| Corpo | Uma estrutura JSON que contém informações de cliente sobre a ative |

O esquema para o corpo do `telemetry` mensagem está definida no [esquema de telemetria](#telemetry-schema) secção.

#### <a name="telemetry-for-interrupted-connections"></a>Telemetria para ligações interrompidas

Se a ligação de rede falha por alguma razão durante uma vez e o cliente não *não* receber um `turn.end` de mensagens do serviço, o cliente envia um `telemetry` mensagem. Esta mensagem descreve os pedidos falhados da próxima vez que o cliente efetua uma ligação ao serviço. Os clientes não dispõe de imediatamente a tentar uma ligação ao enviar o `telemetry` mensagem. A mensagem possam ser colocado na memória intermédia no cliente e enviada através de uma ligação de utilizador pediu futura. O `telemetry` mensagem para os pedidos falhados *tem* utilizar o *X-RequestId* valor os pedidos falhados. Poderão ser enviadas para o serviço, assim que uma ligação é estabelecida, sem aguardar para enviar ou receber para outras mensagens.

## <a name="service-originated-messages"></a>Origem de serviço de mensagens

Esta secção descreve as mensagens que têm origem no serviço de reconhecimento de voz e são enviadas para o cliente. Serviço de reconhecimento de voz mantém um registo das capacidades de cliente e gera as mensagens necessárias para cada cliente, por isso, não que todos os clientes recebem todas as mensagens que são descritas aqui. Para uma forma abreviada, as mensagens são referenciadas pelo valor da *caminho* cabeçalho. Por exemplo, vamos referir-se a uma mensagem de texto de WebSocket com a *caminho* valor `speech.hypothesis` como uma mensagem de speech.hypothesis.

### <a name="message-speechstartdetected"></a>Mensagem `speech.startDetected`

O `speech.startDetected` mensagem indica que o serviço de reconhecimento de voz detetado voz na sequência de áudio.

| Campo | Descrição |
| ------------- | ---------------- |
| Codificação da mensagem de WebSocket | Texto |
| Caminho | `speech.startDetected` |
| Content-Type | aplicação/json; conjunto de carateres = utf-8 |
| Corpo | A estrutura JSON que contém informações sobre as condições quando foi detetado o início de reconhecimento de voz. O *desvio* campo nesta estrutura Especifica o deslocamento (em unidades de 100 nanossegundos) quando o reconhecimento de voz foi detetado na sequência de áudio relativos ao início da sequência. |

#### <a name="sample-message"></a>Mensagens de exemplo

```HTML
Path: speech.startDetected
Content-Type: application/json; charset=utf-8
X-RequestId: 123e4567e89b12d3a456426655440000

{
  "Offset": 100000
}
```

### <a name="message-speechhypothesis"></a>Mensagem `speech.hypothesis`

Durante o reconhecimento de voz, o serviço de reconhecimento de voz periodicamente gera hypotheses sobre as palavras serviço reconhecido. Serviço de reconhecimento de voz envia estes hypotheses para o cliente aproximadamente a cada 300 milissegundos. O `speech.hypothesis` é adequado *apenas* otimização a experiência de reconhecimento de voz do utilizador. Não tem de efetuar qualquer dependência sobre o conteúdo ou a precisão do texto nestas mensagens.

 O `speech.hypothesis` mensagem é aplicável a esses clientes que tenham alguma capacidade de composição de texto e pretendem fornecer comentários quase em tempo real do reconhecimento em curso a quem é falando.

| Campo | Descrição |
| ------------- | ---------------- |
| Codificação da mensagem de WebSocket | Texto |
| Caminho | `speech.hypothesis` |
| X-RequestId | UUID no formato "não-dash" |
| Content-Type | application/json |
| Corpo | A hipótese de reconhecimento de voz estrutura JSON |

#### <a name="sample-message"></a>Mensagens de exemplo

```HTML
Path: speech.hypothesis
Content-Type: application/json; charset=utf-8
X-RequestId: 123e4567e89b12d3a456426655440000

{
  "Text": "this is a speech hypothesis",
  "Offset": 0,
  "Duration": 23600000
}
```

O *desvio* elemento Especifica o deslocamento (em unidades de 100 nanossegundos) quando o frase foi reconhecido, relativos ao início da sequência de áudio.

O *duração* elemento Especifica a duração (em unidades de 100 nanossegundos) desta frase de reconhecimento de voz.

Os clientes não pode fazer qualquer pressupostos sobre a frequência, a temporização ou o texto contido num hipótese de reconhecimento de voz ou a consistência de texto em qualquer hypotheses de reconhecimento de dois voz. Os hypotheses são instantâneos apenas para o processo de transcription no serviço. Não representam uma acumulação de transcription estável. Por exemplo, uma primeira hipótese de reconhecimento de voz pode conter as palavras "bem diversão" e o segundo hipótese pode conter as palavras "encontrar funny." Serviço de reconhecimento de voz não efetuar qualquer processamento posterior ao (por exemplo, maiúsculas/minúsculas, pontuação) no texto a hipótese de reconhecimento de voz.

### <a name="message-speechphrase"></a>Mensagem `speech.phrase`

Quando o serviço de reconhecimento de voz determina que tem informações suficientes para produzir um resultado de reconhecimento não mudará, o serviço produz um `speech.phrase` mensagem. Serviço de reconhecimento de voz produz estes resultados, depois de detetar que o utilizador terminou uma frase de acesso ou frases.

| Campo | Descrição |
| ------------- | ---------------- |
| Codificação da mensagem de WebSocket | Texto |
| Caminho | `speech.phrase` |
| Content-Type | application/json |
| Corpo | A frase de reconhecimento de voz estrutura JSON |

O esquema JSON de expressão de reconhecimento de voz inclui os seguintes campos: `RecognitionStatus`, `DisplayText`, `Offset`, e `Duration`. Para obter mais informações sobre estes campos, consulte [respostas Transcription](../concepts.md#transcription-responses).

#### <a name="sample-message"></a>Mensagens de exemplo

```HTML
Path: speech.phrase
Content-Type: application/json; charset=utf-8
X-RequestId: 123e4567e89b12d3a456426655440000

{
  "RecognitionStatus": "Success",
  "DisplayText": "Remind me to buy 5 pencils.",
  "Offset": 0,
  "Duration": 12300000
}
```

### <a name="message-speechenddetected"></a>Mensagem `speech.endDetected`

O `speech.endDetected` mensagem Especifica que a aplicação cliente deve ser interrompida áudio para o serviço de transmissão em fluxo.

| Campo | Descrição |
| ------------- | ---------------- |
| Codificação da mensagem de WebSocket | Texto |
| Caminho | `speech.endDetected` |
| Corpo | A estrutura JSON que contém o desvio quando o fim do reconhecimento de voz foi detetado. O desvio é representado no deslocamento de unidades de 100 nanossegundos desde o início de áudio que é utilizado para reconhecimento. |
| Content-Type | aplicação/json; conjunto de carateres = utf-8 |

#### <a name="sample-message"></a>Mensagens de exemplo

```HTML
Path: speech.endDetected
Content-Type: application/json; charset=utf-8
X-RequestId: 123e4567e89b12d3a456426655440000

{
  "Offset": 0
}
```

O *desvio* elemento Especifica o deslocamento (em unidades de 100 nanossegundos) quando o frase foi reconhecido, relativos ao início da sequência de áudio.

### <a name="message-turnstart"></a>Mensagem `turn.start`

O `turn.start` sinalizar o início de uma vez na perspetiva do serviço. O `turn.start` mensagem sempre é a primeira mensagem de resposta receberá para qualquer pedido. Se não receber uma `turn.start` da mensagem, partem do princípio de que o estado da ligação de serviço é inválido.

| Campo | Descrição |
| ------------- | ---------------- |
| Codificação da mensagem de WebSocket | Texto |
| Caminho | `turn.start` |
| Content-Type | aplicação/json; conjunto de carateres = utf-8 |
| Corpo | Estrutura JSON |

#### <a name="sample-message"></a>Mensagens de exemplo

```HTML
Path: turn.start
Content-Type: application/json; charset=utf-8
X-RequestId: 123e4567e89b12d3a456426655440000

{
  "context": {
    "serviceTag": "7B33613B91714B32817815DC89633AFA"
  }
}
```

O corpo do `turn.start` mensagem é uma estrutura JSON que contém o contexto de início da ativar. O *contexto* elemento contém um *serviceTag* propriedade. Esta propriedade especifica um valor de etiqueta que o serviço está associada a ativar. Este valor pode ser utilizado pela Microsoft se necessitar de ajuda para a resolução de falhas na sua aplicação.

### <a name="message-turnend"></a>Mensagem `turn.end`

O `turn.end` sinalizar o fim da vez da perspetiva do serviço. O `turn.end` mensagem sempre é a última mensagem de resposta receberá para qualquer pedido. Os clientes podem utilizar a receção desta mensagem como um sinal para atividades de limpeza e a transição para um estado inativo. Se não receber uma `turn.end` da mensagem, partem do princípio de que o estado da ligação de serviço é inválido. Nesses casos, feche a ligação existente para o serviço e voltar a ligar.

| Campo | Descrição |
| ------------- | ---------------- |
| Codificação da mensagem de WebSocket | Texto |
| Caminho | `turn.end` |
| Corpo | Nenhuma |

#### <a name="sample-message"></a>Mensagens de exemplo

```HTML
Path: turn.end
X-RequestId: 123e4567e89b12d3a456426655440000
```

## <a name="telemetry-schema"></a>Esquema de telemetria

O corpo do *telemetria* mensagem é uma estrutura JSON que contém informações de cliente sobre vez ou uma tentativa de ligação. A estrutura é constituída por carimbos de hora do cliente que registam quando ocorrem eventos de cliente. Cada carimbo de hora tem de estar no formato ISO 8601 conforme descrito na secção intitulada "Cabeçalho X-Timestamp." *Telemetria* mensagens que não especifique todos os campos necessários na estrutura de JSON ou que não utilize o formato de carimbo de data / hora correta poderão fazer com que o serviço para terminar a ligação ao cliente. Os clientes *tem* de indicar valores válidos para todos os campos obrigatórios. Os clientes *deve* fornecer valores para os campos opcionais sempre que adequado. Os valores apresentados nos exemplos nesta secção destinam-se apenas a ilustração.

Esquema de telemetria está dividida nas seguintes partes: recebeu carimbos de data / hora de mensagem e métricas. O formato e a utilização de cada peça é especificado nas secções seguintes.

### <a name="received-message-time-stamps"></a>Mensagem recebida carimbos de data / hora

Os clientes têm de incluir valores de tempo de receção de todas as mensagens que recebem depois de estabelecer ligação com êxito ao serviço. Estes valores tem de registar a hora quando o cliente *recebido* cada mensagem a partir da rede. O valor não deve gravar outro sempre. Por exemplo, o cliente não deve gravar o tempo quando- *ter procedido à eliminação* na mensagem. Carimbos de data / hora da mensagem recebida é especificadas numa matriz *: valor de nome* pares. Especifica o nome do par de *caminho* valor da mensagem. O valor do par de Especifica o tempo de cliente quando a mensagem foi recebida. Em alternativa, se foi recebida a mais do que uma mensagem com o nome especificado, o valor do par de é uma matriz de carimbos de data / hora que indica que as mensagens foram recebeu.

```JSON
  "ReceivedMessages": [
    { "speech.hypothesis": [ "2016-08-16T15:03:48.172Z", "2016-08-16T15:03:48.331Z", "2016-08-16T15:03:48.881Z" ] },
    { "speech.endDetected": "2016-08-16T15:03:49.721Z" },
    { "speech.phrase": "2016-08-16T15:03:50.001Z" },
    { "turn.end": "2016-08-16T15:03:51.021Z" }
  ]
```

Os clientes *tem* confirmar a receção de todas as mensagens enviadas pelo serviço, incluindo carimbos de data / hora para essas mensagens no corpo JSON. Se um cliente não conseguir reconhecer a receção de uma mensagem, o serviço poderá termine a ligação.

### <a name="metrics"></a>Métricas

Os clientes têm de incluir informações sobre eventos que ocorreram durante a duração de um pedido. São suportadas as métricas seguintes: `Connection`, `Microphone`, e `ListeningTrigger`.

### <a name="metric-connection"></a>Métrica `Connection`

O `Connection` métrica Especifica os detalhes sobre as tentativas de ligação pelo cliente. A métrica tem de incluir carimbos de data / hora de quando a ligação de WebSocket foi iniciada e concluída. O `Connection` métrica é necessária *apenas para a primeira vez de uma ligação*. Activa subsequentes não é necessários para incluir estas informações. Se um cliente efetua várias tentativas de ligação antes de uma ligação é estabelecida, informações sobre *todos os* as tentativas de ligação devem ser incluídas. Para obter mais informações, consulte [telemetria de falhas de ligação](#connection-failure-telemetry).

| Campo | Descrição | Utilização |
| ----- | ----------- | ----- |
| Nome | `Connection` | Necessário |
| Id | O valor do identificador de ligação que foi utilizado no *X ConnectionId* cabeçalho para este pedido de ligação | Necessário |
| Iniciar | A hora quando o cliente enviou o pedido de ligação | Necessário |
| Terminar | A hora quando o cliente recebeu a notificação de que a ligação foi estabelecida com êxito, em casos de erro, rejeitado, recusou ou falha | Necessário |
| Erro | Uma descrição do erro ocorrido, se aplicável. Se a ligação foi efetuada com êxito, os clientes devem omita este campo. O comprimento máximo deste campo é de 50 carateres. | Necessário para casos de erro, omitidos caso contrário |

A descrição de erro deve ser, no máximo 50 carateres e Idealmente, deve ser um dos valores listados na seguinte tabela. Se a condição de erro não corresponde a um destes valores, os clientes podem utilizar uma descrição da condição de erro sucinta utilizando [CamelCasing](https://en.wikipedia.org/wiki/Camel_case) sem espaço em branco. A capacidade de enviar um *telemetria* mensagem requer uma ligação ao serviço, assim, apenas transitório ou podem ser comunicadas as condições de erro temporário no *telemetria* mensagem. Condições de erro que *permanentemente* bloquear um cliente estabelecer uma ligação ao serviço de impedir que o cliente enviar qualquer mensagem para o serviço, incluindo *telemetria* mensagens.

| Erro | Utilização |
| ----- | ----- |
| DNSfailure | O cliente não conseguiu ligar ao serviço devido a uma falha DNS na pilha de rede. |
| NoNetwork | O cliente tentada uma ligação, mas a pilha de rede reportou que a nenhuma rede física estava disponível. |
| NoAuthorization | A ligação de cliente falhou ao tentar adquirir um token de autorização para a ligação. |
| NoResources | O cliente ficou sem algum recurso local (por exemplo, memória) ao tentar estabelecer uma ligação. |
| Proibido | O cliente não foi possível ligar ao serviço porque o serviço devolveu um HTTP `403 Forbidden` código de estado sobre o pedido de atualização de WebSocket. |
| Não autorizado | O cliente não foi possível ligar ao serviço porque o serviço devolveu um HTTP `401 Unauthorized` código de estado sobre o pedido de atualização de WebSocket. |
| BadRequest | O cliente não foi possível ligar ao serviço porque o serviço devolveu um HTTP `400 Bad Request` código de estado sobre o pedido de atualização de WebSocket. |
| ServerUnavailable | O cliente não foi possível ligar ao serviço porque o serviço devolveu um HTTP `503 Server Unavailable` código de estado sobre o pedido de atualização de WebSocket. |
| ServerError | O cliente não foi possível ligar ao serviço porque o serviço devolveu um `HTTP 500` código de estado de erro interno no pedido de atualização de WebSocket. |
| Tempo Limite (excedido) | Pedido de ligação do cliente excedido o tempo limite sem uma resposta do serviço. O *final* campo contém a hora quando o cliente excedeu o tempo e parado a aguardar a ligação. |
| ClientError | O cliente terminada a ligação devido a algum erro interno do cliente. | 

### <a name="metric-microphone"></a>Métrica `Microphone`

O `Microphone` métrica é necessária para todos os activa de reconhecimento de voz. Esta métrica mede o tempo no cliente durante os quais entrada de áudio está a ser utilizada ativamente para um pedido de reconhecimento de voz.

Utilize os seguintes exemplos como diretrizes para gravação *iniciar* tempo valores para o `Microphone` métrica na sua aplicação de cliente:

* Uma aplicação de cliente requer que um utilizador deve premir um botão física para iniciar o microfone. Depois de premir o botão, a aplicação cliente lê a entrada da microfone e envia-a para o serviço de reconhecimento de voz. O *iniciar* valor para o `Microphone` métrica regista o tempo após o push de botão quando microfone é inicializada e pronto para fornecer comentários. O *final* valor para o `Microphone` métrica regista o tempo quando a aplicação cliente parou de transmissão em fluxo áudio ao serviço depois que recebeu a `speech.endDetected` mensagens do serviço.

* Uma aplicação de cliente utiliza um spotter de palavra-chave "sempre" está a escutar. Apenas depois do spotter de palavra-chave Deteta uma expressão de Acionador ditas a aplicação cliente recolher a entrada do microfone e enviá-lo para o serviço de reconhecimento de voz. O *iniciar* valor para o `Microphone` métrica regista o tempo quando spotter a palavra-chave notificado o cliente para começar a utilizar a entrada a partir do microfone. O *final* valor para o `Microphone` métrica regista o tempo quando a aplicação cliente parou de transmissão em fluxo áudio ao serviço depois que recebeu a `speech.endDetected` mensagens do serviço.

* Uma aplicação de cliente tem acesso a uma sequência de áudio constante e executa a deteção de silêncio/voz em que a sequência de áudio num *módulo de deteção de reconhecimento de voz*. O *iniciar* valor para o `Microphone` métrica regista o tempo quando o *módulo de deteção de reconhecimento de voz* notificado o cliente para começar a utilizar a entrada de sequência de áudio. O *final* valor para o `Microphone` métrica regista o tempo quando a aplicação cliente parou de transmissão em fluxo áudio ao serviço depois que recebeu a `speech.endDetected` mensagens do serviço.

* Uma aplicação de cliente está a processar a segunda vez de um pedido de ativar multi e é informada por uma mensagem de resposta do serviço para ativar o microfone para reunir a entrada para a segunda vez. O *iniciar* valor para o `Microphone` métrica regista o tempo, quando a aplicação de cliente permite microfone e é iniciado com a entrada do que a origem de áudio. O *final* valor para o `Microphone` métrica regista o tempo quando a aplicação cliente parou de transmissão em fluxo áudio ao serviço depois que recebeu a `speech.endDetected` mensagens do serviço.

O *final* tempo valor para o `Microphone` métrica regista o tempo quando a aplicação cliente parou de transmissão em fluxo de entrada de áudio. Na maioria das situações, este evento ocorre pouco tempo depois do cliente recebido a `speech.endDetected` mensagens do serviço. Aplicações de cliente podem verificar que se estão corretamente cumprindo o protocolo, garantindo que o *final* tempo valor para o `Microphone` métrica ocorre mais tarde do que o valor de tempo de receção para o `speech.endDetected` mensagem. E, porque não existe, normalmente, um atraso entre a extremidade de uma vez e o início da outra por sua vez, os clientes podem verificar conformidade de protocolo, garantindo que o *iniciar* tempo do `Microphone` métrica para qualquer ative subsequente corretamente regista a hora quando o cliente *iniciado* utilizando microfone a entrada de áudio fluxo para o serviço.

| Campo | Descrição | Utilização |
| ----- | ----------- | ----- |
| Nome | Microfone | Necessário |
| Iniciar | A hora quando o cliente foi iniciado utilizando a entrada de áudio do microfone ou outra sequência de áudio ou recebeu um acionador spotter a palavra-chave | Necessário |
| Terminar | A hora quando o cliente parado utilizando o fluxo microfone ou no áudio | Necessário |
| Erro | Uma descrição do erro ocorrido, se aplicável. Se as operações de microfone teve êxito, os clientes devem omita este campo. O comprimento máximo deste campo é de 50 carateres. | Necessário para casos de erro, omitidos caso contrário |

### <a name="metric-listeningtrigger"></a>Métrica `ListeningTrigger`
O `ListeningTrigger` métrica mede a hora quando o utilizador executa a ação que inicia a entrada de voz. O `ListeningTrigger` métrica é opcional, mas os clientes que podem fornecer esta métrica são encorajados a fazê-lo.

Utilize os seguintes exemplos como diretrizes para gravação *iniciar* e *final* tempo valores para o `ListeningTrigger` métrica na sua aplicação de cliente.

* Uma aplicação de cliente requer que um utilizador deve premir um botão física para iniciar o microfone. O *iniciar* valor para esta métrica regista o tempo de push botão. O *final* valor regista o tempo quando terminar, o push de botão.

* Uma aplicação de cliente utiliza um spotter de palavra-chave "sempre" está a escutar. Depois da palavra-chave spotter Deteta uma expressão de Acionador ditas, a aplicação cliente lê a entrada do microfone e envia-a para o serviço de reconhecimento de voz. O *iniciar* valor para esta métrica regista o tempo que o spotter de palavra-chave recebeu áudio, em seguida, foi detetado como o frase de Acionador. O *final* valor regista o tempo quando a palavra da última do frase acionador foi autenticação ditas pelo utilizador.

* Uma aplicação de cliente tem acesso a uma sequência de áudio constante e executa a deteção de silêncio/voz em que a sequência de áudio num *módulo de deteção de reconhecimento de voz*. O *iniciar* valor para esta métrica regista o tempo que o *módulo de deteção de reconhecimento de voz* recebido áudio, em seguida, foi detetado como reconhecimento de voz. O *final* valor regista o tempo quando o *módulo de deteção de reconhecimento de voz* detetado reconhecimento de voz.

* Uma aplicação de cliente está a processar a segunda vez de um pedido de ativar multi e é informada por uma mensagem de resposta do serviço para ativar o microfone para reunir a entrada para a segunda vez. A aplicação cliente deve *não* incluem um `ListeningTrigger` métrica para este ative.

| Campo | Descrição | Utilização |
| ----- | ----------- | ----- |
| Nome | ListeningTrigger | Opcional |
| Iniciar | A hora quando o acionador de escuta de cliente iniciado | Necessário |
| Terminar | A hora em que terminou o acionador de escuta do cliente | Necessário |
| Erro | Uma descrição do erro ocorrido, se aplicável. Se a operação de Acionador foi concluída com êxito, os clientes devem omita este campo. O comprimento máximo deste campo é de 50 carateres. | Necessário para casos de erro, omitidos caso contrário |

#### <a name="sample-message"></a>Mensagens de exemplo

O exemplo seguinte mostra uma mensagem de telemetria com métricas e ReceivedMessages partes:

```HTML
Path: telemetry
Content-Type: application/json; charset=utf-8
X-RequestId: 123e4567e89b12d3a456426655440000
X-Timestamp: 2016-08-16T15:03:54.183Z

{
  "ReceivedMessages": [
    { "speech.hypothesis": [ "2016-08-16T15:03:48.171Z", "2016-08-16T15:03:48.331Z", "2016-08-16T15:03:48.881Z" ] },
    { "speech.endDetected": "2016-08-16T15:03:49.721Z" },
    { "speech.phrase": "2016-08-16T15:03:50.001Z" },
    { "turn.end": "2016-08-16T15:03:51.021Z" }
  ],
  "Metrics": [
    {
      "Name": "Connection",
      "Id": "A140CAF92F71469FA41C72C7B5849253",
      "Start": "2016-08-16T15:03:47.921Z",
      "End": "2016-08-16T15:03:48.000Z",
    },
    {
      "Name": "ListeningTrigger",
      "Start": "2016-08-16T15:03:48.776Z",
      "End": "2016-08-16T15:03:48.777Z",
    },
    {
      "Name": "Microphone",
      "Start": "2016-08-16T15:03:47.921Z",
      "End": "2016-08-16T15:03:51.921Z",
    },
  ],
}
```

## <a name="error-handling"></a>Processamento de erros

Esta secção descreve os tipos de mensagens de erro e condições que a aplicação tem de processar.

### <a name="http-status-codes"></a>Códigos de estado HTTP

Durante o pedido de atualização de WebSocket, serviço de reconhecimento de voz poderá devolver nenhum dos códigos de estado HTTP padrão, tais como `400 Bad Request`, etc. A aplicação corretamente deve processar estas condições de erro.

#### <a name="authorization-errors"></a>Erros de autorização

Se a autorização incorreta é fornecida durante a atualização de WebSocket, o serviço de reconhecimento de voz devolve um HTTP `403 Forbidden` código de estado. Entre as condições que podem acionar este código de erro são:

* Falta *autorização* cabeçalho

* Token de autorização inválido

* Token de autorização expirado

O `403 Forbidden` mensagem de erro não indica um problema com o serviço de reconhecimento de voz. Esta mensagem de erro indica um problema com a aplicação de cliente.

### <a name="protocol-violation-errors"></a>Erros de violação do protocolo

Se o serviço de reconhecimento de voz Deteta quaisquer violações de protocolo de um cliente, o serviço de termina a ligação de WebSocket após regressar um *código de estado* e *razão* para a terminação. Aplicações de cliente podem utilizar estas informações para resolver problemas e corrigir as violações.

#### <a name="incorrect-message-format"></a>Formato de mensagem incorreto

Se um cliente envia um texto ou mensagem binária para o serviço que não é codificado no formato correto especificado nesta especificação, o serviço fecha a ligação com um *dados de Payload inválido 1007* código de estado. 

O serviço devolve este código de estado para diversos motivos, conforme mostrado nos exemplos seguintes:

* "Formato de mensagem incorreto. Mensagem binária tem prefixo de tamanho de cabeçalho inválido." O cliente enviou uma mensagem binária que tem um prefixo de tamanho de cabeçalho inválido.

* "Formato de mensagem incorreto. Mensagem binária tem o tamanho de cabeçalho inválido." O cliente enviou uma mensagem binária com um tamanho de cabeçalho inválido especificado.

* "Formato de mensagem incorreto. Falha de cabeçalhos de mensagem binária descodificar em UTF-8." O cliente enviou uma mensagem binária que contém os cabeçalhos que não foram codificados corretamente em UTF-8.

* "Formato de mensagem incorreto. Mensagem de texto não contém dados." O cliente enviou uma mensagem de texto que não contém dados nenhum corpo.

* "Formato de mensagem incorreto. Falha ao descodificar em UTF-8 de mensagem de texto." O cliente enviou uma mensagem de texto que não foi corretamente codificada em UTF-8.

* "Formato de mensagem incorreto. Mensagem de texto contém não separador de cabeçalho." O cliente enviou uma mensagem de texto que não continha um separador de cabeçalho ou utilizar o separador de cabeçalho errado.

#### <a name="missing-or-empty-headers"></a>Cabeçalhos em falta ou vazios

Se um cliente envia uma mensagem que não tem cabeçalhos de necessários *X-RequestId* ou *caminho*, o serviço fecha a ligação com um *erro de protocolo de 1002* código de estado. A mensagem é "cabeçalho em falta/vazio. {Nome do cabeçalho}."

#### <a name="requestid-values"></a>Valores de RequestId

Se um cliente envia uma mensagem que especifica um *X-RequestId* cabeçalho com um formato incorreto, o serviço fecha a ligação e devolve um *erro de protocolo de 1002* estado. A mensagem é "pedido inválido. O valor de cabeçalho X-RequestId não foi especificado no formato UUID de não-dash."

#### <a name="audio-encoding-errors"></a>Erros de codificação de áudio

Se um cliente envia um segmento de áudio que inicia uma vez e o formato de áudio ou codificação não está em conformidade com a especificação necessária, o serviço fecha a ligação e devolve um *dados de Payload inválido 1007* código de estado. A mensagem indica que o formato de codificação de origem do erro.

#### <a name="requestid-reuse"></a>Reutilização RequestId

Após a conclusão vez, se um cliente envia uma mensagem que reutiliza o identificador de pedido do que por sua vez, o serviço fecha a ligação e devolve um *erro de protocolo de 1002* código de estado. A mensagem é "pedido inválido. Reutilização de identificadores de pedido não é permitida."

## <a name="connection-failure-telemetry"></a>Telemetria de falhas de ligação

Para garantir a melhor experiência de utilizador possível, os clientes devem informar serviço de reconhecimento de voz dos carimbos de hora para pontos de verificação importantes dentro de uma ligação utilizando o *telemetria* mensagem. É igualmente importante que os clientes informam o serviço de ligações que foram tentada, mas falha.

Para cada tentativa de ligação que falharam, clientes criam um *telemetria* mensagem com um único *X-RequestId* valor do cabeçalho. Porque o cliente não foi possível estabelecer uma ligação a *ReceivedMessages* campo no corpo JSON pode ser omitido. Apenas o `Connection` entrada no *métricas* campo está incluído. Esta entrada inclui o início e carimbos de data / hora de fim, bem como a condição de erro que foi encontrada.

### <a name="connection-retries-in-telemetry"></a>Tentativas de ligação na telemetria

Os clientes devem distinguir *tentativas* de *várias tentativas de ligação* pelo evento que aciona a tentativa de ligação. As tentativas de ligação que são executadas através de programação sem qualquer intervenção do utilizador são as repetições. Várias tentativas de ligação que são executadas em resposta a intervenção do utilizador são várias tentativas de ligação. Os clientes dar cada tentativa de ligação de utilizador acionada por um único *X-RequestId* e *telemetria* mensagem. Os clientes reutilizar o *X-RequestId* para tentativas programáticas. Se foram efetuadas várias tentativas de uma tentativa de ligação único, cada tentativa de repetição é incluída como um `Connection` entrada no *telemetria* mensagem.

Por exemplo, suponha que um utilizador speaks o acionador de palavra-chave para iniciar uma ligação e a primeira tentativa de ligação falhar devido a erros DNS. No entanto, uma segunda tentativa indicando que é efetuada através de programação pelo cliente é concluída com êxito. Porque o cliente repetida a ligação sem exigir a intervenção adicional do utilizador, o cliente utiliza um único *telemetria* mensagem com múltiplos `Connection` entradas para descrever a ligação.

Outro exemplo, suponha que um utilizador speaks o acionador de palavra-chave para iniciar uma ligação e esta tentativa de ligação falhar após três tentativas. Em seguida, o cliente desistir, interrompe a tentar ligar ao serviço e informa o utilizador que algo correu mal. O utilizador speaks, em seguida, o acionador de palavra-chave novamente. Neste momento, suponha que o cliente se liga ao serviço. Depois de ligar, o cliente envia imediatamente um *telemetria* mensagem para o serviço que contém três `Connection` entradas que descrevem as falhas de ligação. Após a receção de `turn.end` mensagem, o cliente envia outro *telemetria* mensagem a descrever a ligação com êxito.

## <a name="error-message-reference"></a>Referência de mensagens de erro

### <a name="http-status-codes"></a>Códigos de estado HTTP

| Código de estado de HTTP | Descrição | Resolução de problemas |
| - | - | - |
| Pedido de 400 incorreta | O cliente enviou um pedido de ligação de WebSocket estava incorreto. | Certifique-se de que forneceu todos os parâmetros necessários e os cabeçalhos de HTTP e que os valores estão corretos. |
| 401 não autorizado | O cliente não incluiu as informações de autorização necessário. | Verifique se está a enviar o *autorização* cabeçalho na ligação de WebSocket. |
| 403 Proibido | O cliente enviou as informações de autorização, mas era inválido. | Verifique se a um valor inválido ou expirado não está a enviar no *autorização* cabeçalho. |
| 404 Não Encontrado | O cliente tentou aceder a um caminho de URL que não é suportado. | Certifique-se de que está a utilizar o URL correto para a ligação de WebSocket. |
| 500 Erro de servidor | O serviço encontrou um erro interno e não foi possível satisfazer o pedido. | Na maioria dos casos, este erro é transitório. Repita o pedido. |
| 503 Serviço Indisponível | O serviço esteve indisponível para processar o pedido. | Na maioria dos casos, este erro é transitório. Repita o pedido. |

### <a name="websocket-error-codes"></a>Códigos de erro de WebSocket

| Código de WebSocketsStatus | Descrição | Resolução de problemas |
| - | - | - |
| Fecho normal 1000 | O serviço fechou a ligação de WebSocket sem um erro. | Se o fecho de WebSocket era inesperado, reread a documentação para se certificar de que compreende como e quando o serviço pode terminar a ligação de WebSocket. |
| Erro de protocolo de 1002 | O cliente não cumprir os requisitos de protocolo. | Certifique-se de que compreender a documentação do protocolo e são limpar sobre os requisitos. Leia a documentação anterior acerca razões do erro para ver se está a violar os requisitos do protocolo. |
| Dados de Payload inválido 1007 | O cliente enviou um payload inválido na mensagem de protocolo. | Verifique a última mensagem enviada para o serviço de erros. Leia a documentação sobre os erros de payload anterior. |
| Erro de servidor 1011 | O serviço encontrou um erro interno e não foi possível satisfazer o pedido. | Na maioria dos casos, este erro é transitório. Repita o pedido. |

## <a name="related-topics"></a>Tópicos relacionados

Consulte uma [JavaScript SDK](https://github.com/Azure-Samples/SpeechToText-WebSockets-Javascript) que é uma implementação do protocolo WebSocket com base no serviço de reconhecimento de voz.
