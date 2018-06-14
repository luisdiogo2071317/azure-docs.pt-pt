---
title: Media Services do Azure - sinalização excedeu o tempo limite metadados na transmissão em fluxo em direto | Microsoft Docs
description: Esta especificação descreve dois modos que são suportados pelos Media Services para metadados excedeu o tempo limite sinalização dentro de transmissão em fluxo em direto. Isto inclui suporte para metadados excedeu o tempo limite genérico sinais, bem como SCTE 35 sinalização para inserção de splice do ad.
services: media-services
documentationcenter: ''
author: cenkdin
manager: cfowler
editor: johndeu
ms.assetid: 265b94b1-0fb8-493a-90ec-a4244f51ce85
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/17/2018
ms.author: johndeu;
ms.openlocfilehash: 2e736872dc3e471af7c5b3f758516910a02067fe
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
ms.locfileid: "33790525"
---
# <a name="signaling-timed-metadata-in-live-streaming"></a>Sinalização excedeu o tempo limite metadados na transmissão em fluxo em direto


## <a name="1-introduction"></a>1 Introdução 
Para facilitar a inserção de anúncios ou eventos personalizados num leitor de cliente, muitas vezes, broadcasters disponibilizar utilização dos metadados excedeu o tempo limite incorporados o vídeo. Para ativar estes cenários, os Media Services fornecem suporte para o transporte de metadados excedeu o tempo limite, juntamente com o suporte de dados do ponto de inserção do canal de transmissão em fluxo em direto para a aplicação de cliente.
Esta especificação destaca dois modos que são suportados pelos Media Services para metadados excedeu o tempo limite no direta sinais de transmissão em fluxo:

1. [SCTE-35] sinalização que heeds práticas recomendadas descritas por [SCTE 67]

2. Uma genérica excedeu o tempo limite metadados sinalização modo, mensagens que não são [SCTE-35]

### <a name="12-conformance-notation"></a>1.2 a notação de conformidade de
As palavras de chave "tem de", "Não tem", "REQUIRED", "SHALL", "Deverá NOT", "SHOULD", "Deve NOT", "Recomendado", "Poderá" e "Opcional" neste documento estão a ser interpretados conforme descrito em RFC 2119

### <a name="13-terms-used"></a>1.3 termos utilizados

| Termo              | Definição                                                                                                                                                                                                                       |
|-------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Tempo de apresentação | O tempo que um evento é apresentado um Visualizador de mensagens em fila. O tempo representa o momento em que a linha cronológica de suporte de dados que um visualizador deverá ver o evento. Por exemplo, o tempo de apresentação de uma mensagem de comando de splice_info() SCTE 35 é o splice_time(). |
| Hora da chegada de      | O tempo que chega uma mensagem de evento. A hora é normalmente distinta desde o momento de apresentação do evento, uma vez que as mensagens de eventos são enviadas antes do tempo de apresentação do evento.                                     |
| Controlar disperso      | controlar o suporte de dados que não seja contínuo e é o tempo sincronizado com um registo principal ou de controlo.                                                                                                                                    |
| Origem            | O serviço de transmissão em fluxo de Media Services do Azure                                                                                                                                                                                                |
| Receptor de canal      | Serviço de transmissão em fluxo em direto de suporte de dados do Azure                                                                                                                                                                                           |
| HLS               | Protocolo Apple HTTP em direto de transmissão em fluxo                                                                                                                                                                                               |
| TRAÇO              | Dynamic adaptável de transmissão em fluxo através de HTTP                                                                                                                                                                                             |
| Uniforme            | Protocolo de transmissão em fluxo uniforme                                                                                                                                                                                                        |
| MPEG2 TS          | Fluxos de 2 de transporte MPEG                                                                                                                                                                                                         |
| RTMP              | Protocolo de suporte em tempo real                                                                                                                                                                                                    |
| uimsbf            | Número inteiro não assinado, mais significativas bit primeiro.                                                                                                                                                                                    |

-----------------------

## <a name="2-timed-metadata-ingest"></a>A inserção de metadados excedeu o tempo limite de 2
## <a name="21-rtmp-ingest"></a>2.1 de inserção RTMP
RTMP suporta sinais excedeu o tempo limite de metadados enviados como AMF rectângulos mensagens incorporadas no fluxo RTMP. As mensagens de rectângulos poderão ser enviadas algum tempo antes do evento real ou inserção de splice ad tem de ocorrer. Para suportar este cenário, a hora real do segmento ou splice é enviada numa mensagem cure. Para obter mais informações, consulte [AMF0].

A tabela seguinte descreve o formato de payload da mensagem AMF será ingestão de Media Services.

O nome da mensagem AMF pode ser utilizado para diferenciar vários fluxos de eventos do mesmo tipo.  Mensagens de [SCTE-35], o nome da mensagem AMF tem de ser "onAdCue" conforme recomendado nas [SCTE 67].  Quaisquer campos não listados abaixo têm de ser ignorados, para que inovação esta estrutura não está inibida no futuro.

### <a name="signal-syntax"></a>Sintaxe de sinal
Para o modo simple de RTMP, os Media Services suportam uma única mensagem rectângulos AMF chamada "onAdCue" com o seguinte formato:

### <a name="simple-mode"></a>Modo Simple

| Nome do campo | Tipo de campo | Necessário? | Descrições                                                                                                             |
|------------|------------|----------|--------------------------------------------------------------------------------------------------------------------------|
| rectângulos        | Cadeia     | Necessário | A mensagem de evento.  Deverá ser "SpliceOut" para designar um modo simple splice.                                              |
| ID         | Cadeia     | Necessário | Um identificador exclusivo que descreva o splice ou segmento. Identifica esta instância da mensagem                            |
| Duração   | Number     | Necessário | A duração do splice. As unidades são fracional segundos.                                                                |
| elapsed    | Number     | Opcional | Quando o sinal está a ser repetido para suportar otimizar, este campo deverá ser a quantidade de tempo de apresentação que tem decorridos desde a splice começou. As unidades são fracional segundos. Quando utilizar o modo simple, este valor não deve exceder original durante o splice.                                                  |
| hora       | Number     | Necessário | Deverá ser o tempo de splice, hora de apresentação. As unidades são fracional segundos.                                     |

---------------------------

### <a name="scte-35-mode"></a>Modo de SCTE 35

| Nome do campo | Tipo de campo | Necessário? | Descrições                                                                                                             |
|------------|------------|----------|--------------------------------------------------------------------------------------------------------------------------|
| rectângulos        | Cadeia     | Necessário | A mensagem de evento.  Mensagens de [SCTE-35], tem de ser o base64 (IETF RFC 4648) binário codificado splice_info_section() para que as mensagens sejam enviados para HLS, uniforme, Dash clientes e em conformidade com [SCTE 67].                                              |
| tipo       | Cadeia     | Necessário | Um URN ou um URL que identifica o esquema de mensagem; Por exemplo, "urn: exemplo: sinalização: 1.0".  Mensagens de [SCTE-35], tem de ser "urn: scte:scte35:2013a:bin" para que as mensagens sejam enviados para HLS, uniforme, Dash clientes e em conformidade com [SCTE 67].  |
| ID         | Cadeia     | Necessário | Um identificador exclusivo que descreva o splice ou segmento. Identifica esta instância da mensagem.  As mensagens com semântica equivalente deverá ter o mesmo valor.|
| Duração   | Number     | Necessário | A duração do evento ou ad splice-segmento, se conhecida. Se desconhecido, o valor deve ser 0.                                                                 |
| elapsed    | Number     | Opcional | Quando o sinal de ad [SCTE-35] está a ser repetido para otimizar o, este campo deverá ser a quantidade de tempo de apresentação que tem decorridos desde a splice começou. As unidades são fracional segundos. No modo de [SCTE-35], este valor pode exceder a duração especificada original do splice ou segmento.                                                  |
| hora       | Number     | Necessário | O tempo de apresentação do evento ou ad splice.  A hora de apresentação e a duração devem alinhar com pontos de acesso de fluxo (SAP) do tipo 1 ou 2, tal como definido em [ISO-14496-12] Annex I. Saída HLS, a hora e a duração devem alinhar com os limites de segmento. A hora de apresentação e a duração de mensagens de eventos diferentes no mesmo fluxo de eventos não sobrepor. As unidades são fracional segundos.

---------------------------

#### <a name="211-cancelation-and-updates"></a>2.1.1 cancelamento e atualizações

As mensagens podem ser canceladas ou atualizadas enviando várias mensagens com o mesmo tempo de apresentação e o mesmo ID. O tempo de apresentação e o ID de identificam de forma exclusiva o evento, não sendo a última mensagem recebida por um período de tempo de apresentação específico que satisfaça as restrições de pré-agregação de a mensagem que é alterada. O evento atualizado substitui as mensagens anteriormente recebidas. A restrição de pré-agregação é de quatro segundos. Mensagens recebidas, pelo menos, quatro segundos antes do tempo de apresentação serão exigirão a tomada.

## <a name="22-fragmented-mp4-ingest-smooth-streaming"></a>2.2 fragmentados MP4 de inserção (transmissão em fluxo uniforme)
Consulte [FMP4 em DIRETO] para a ingestão dos requisitos de fluxo em direto. As secções seguintes fornecem detalhes sobre a inserção de metadados de apresentação excedeu o tempo limite.  Metadados de apresentação excedeu o tempo limite é ingeridos como um controlar disperso, o que está definido na ambas em direto do manifesto caixa (consulte MS SSTR) e a caixa de filmes ('moov').  Cada fragmento disperso é composta por uma caixa de fragmento de filmes ('moof') e a caixa de dados do suporte de dados ('mdat'), em que a caixa 'mdat' é a mensagem binária.

#### <a name="221-live-server-manifest-box"></a>2.2.1 caixa de manifesto do servidor em direto
A controlar dispersa tem de ser declarado na caixa de manifesto do servidor em direto com uma \<textstream\> entrada e tem de ter os seguintes atributos definir:

| **Nome do atributo** | **Tipo de campo** | **Necessário?** | **Descrição**                                                                                                                                                                                                                                                 |
|--------------------|----------------|---------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| systemBitrate      | Number         | Necessário      | TEM de ser "0", que indica que um registo com velocidade de transmissão desconhecido, a variável.                                                                                                                                                                                                 |
| parentTrackName    | Cadeia         | Necessário      | TEM de ser o nome de controlar o principal, para que os códigos de tempo de controlar dispersa são escala temporal alinhado. Controlar o principal não pode ser um controlar disperso.                                                                                                                    |
| manifestOutput     | Booleano        | Necessário      | TEM de ser "verdadeiro", para indicar que a controlar dispersa será incorporado no manifesto de cliente uniforme.                                                                                                                                                               |
| Subtipo            | Cadeia         | Necessário      | TEM de ser o caráter quatro o código de "Dados".                                                                                                                                                                                                                         |
| Esquema             | Cadeia         | Necessário      | TEM de ser um URN ou um URL que identifica o esquema de mensagem; Por exemplo, "urn: exemplo: sinalização: 1.0". Mensagens de [SCTE-35], tem de ser "urn: scte:scte35:2013a:bin" para que as mensagens sejam enviados para HLS, uniforme, Dash clientes e em conformidade com [SCTE 67]. |
| TrackName          | Cadeia         | Necessário      | TEM de ser o nome de controlar o disperso. O trackName pode ser utilizada para diferenciar vários fluxos de eventos com o mesmo esquema. Cada fluxo de eventos exclusivo têm de ter um nome exclusivo de controlar.                                                                           |
| escala temporal          | Number         | Opcional      | TEM de ser a escala temporal de controlar o principal.                                                                                                                                                                                                                      |

-------------------------------------

### <a name="222-movie-box"></a>2.2.2 caixa de filmes

A caixa de filmes ('moov') segue a em direto manifesto caixa servidor como parte do cabeçalho do fluxo para um registo disperso.

A caixa 'moov' deve conter um **TrackHeaderBox ('tkhd')** caixa como definido em [ISO-14496-12] com as seguintes restrições:

| **Nome do campo** | **Tipo de campo**          | **Necessário?** | **Descrição**                                                                                                |
|----------------|-------------------------|---------------|----------------------------------------------------------------------------------------------------------------|
| Duração       | número inteiro sem sinal de 64 bits | Necessário      | DEVE ser 0, uma vez que a caixa de controlar tem zero exemplos e a duração total de exemplos na caixa de controlar é 0. |
-------------------------------------

A caixa 'moov' deve conter um **HandlerBox ('hdlr')** como definido em [ISO-14496-12] com as seguintes restrições:

| **Nome do campo** | **Tipo de campo**          | **Necessário?** | **Descrição**   |
|----------------|-------------------------|---------------|-------------------|
| handler_type   | número inteiro sem sinal de 32 bits | Necessário      | DEVE ser 'meta'. |
-------------------------------------

A caixa 'stsd' deve conter uma caixa MetaDataSampleEntry com um nome de codificação, como definido em [ISO-14496-12].  Por exemplo, para mensagens de SCTE 35 o nome de codificação deve ser 'scte'.

### <a name="223-movie-fragment-box-and-media-data-box"></a>2.2.3 caixa de fragmento de filmes e a caixa de dados do suporte de dados

Fragmentos de controlar dispersa consistem de uma caixa de fragmento de filmes ('moof') e uma caixa de dados do suporte de dados ('mdat').

A caixa de MovieFragmentBox ('moof') tem de conter um **TrackFragmentExtendedHeaderBox ('uuid')** caixa como definido em [MS-SSTR] com os seguintes campos:

| **Nome do campo**         | **Tipo de campo**          | **Necessário?** | **Descrição**                                                                               |
|------------------------|-------------------------|---------------|-----------------------------------------------------------------------------------------------|
| fragment_absolute_time | número inteiro sem sinal de 64 bits | Necessário      | TEM de ser a hora da chegada do evento. Este valor Alinha a mensagem com o registo principal.   |
| fragment_duration      | número inteiro sem sinal de 64 bits | Necessário      | TEM de ser a duração do evento. A duração pode ser zero para indicar que a duração é desconhecida. |

------------------------------------


A caixa de MediaDataBox ('mdat') tem de ter o seguinte formato:

| **Nome do campo**          | **Tipo de campo**                   | **Necessário?** | **Descrição**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|-------------------------|----------------------------------|---------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| versão                 | número inteiro sem sinal de 32 bits (uimsbf) | Necessário      | Determina o formato do conteúdo da caixa de 'mdat'. Versões não reconhecidas serão ignoradas. Atualmente a única versão suportada é 1.                                                                                                                                                                                                                                                                                                                                                      |
| ID                      | número inteiro sem sinal de 32 bits (uimsbf) | Necessário      | Identifica esta instância da mensagem. As mensagens com semântica equivalente deverá ter o mesmo valor; ou seja, qualquer caixa de mensagem de um evento com o mesmo id de processamento é suficiente.                                                                                                                                                                                                                                                                                                            |
| presentation_time_delta | número inteiro sem sinal de 32 bits (uimsbf) | Necessário      | A soma de fragment_absolute_time, especificada a TrackFragmentExtendedHeaderBox e o presentation_time_delta tem de ser o tempo de apresentação do evento. A hora de apresentação e a duração devem alinhar com pontos de acesso de fluxo (SAP) do tipo 1 ou 2, tal como definido em [ISO-14496-12] Annex I. Saída HLS, a hora e a duração devem alinhar com os limites de segmento. A hora de apresentação e a duração de mensagens de eventos diferentes no mesmo fluxo de eventos não sobrepor. |
| message                 | matriz de bytes                       | Necessário      | A mensagem de evento. Mensagens de [SCTE-35], a mensagem é splice_info_section() binário, embora [SCTE 67] recomenda outra coisa. Mensagens de [SCTE-35], tem de ser splice_info_section() para que as mensagens sejam enviados para HLS, uniforme, Dash clientes e em conformidade com [SCTE 67]. Mensagens de [SCTE-35], o binário splice_info_section() é o payload da caixa de 'mdat' e não é codificado em base64.                                                            |

------------------------------


### <a name="224-cancelation-and-updates"></a>2.2.4 cancelamento e atualizações
As mensagens podem ser canceladas ou atualizadas enviando várias mensagens com o mesmo tempo de apresentação e o mesmo ID.  O tempo de apresentação e o ID de identificar exclusivamente o evento. A última mensagem recebida para uma hora de apresentação específico que satisfaça as restrições de pré-agregação, é a mensagem que é alterada. A mensagem atualizada substitui as mensagens anteriormente recebidas.  A restrição de pré-agregação é de quatro segundos. Mensagens recebidas, pelo menos, quatro segundos antes do tempo de apresentação serão exigirão a tomada. 


## <a name="3-timed-metadata-delivery"></a>Entrega de 3 metadados excedeu o tempo limite

Dados de fluxo de eventos são opaco aos Media Services. Os Media Services passa simplesmente três informações entre o ponto final de inserção e o ponto final de cliente. As seguintes propriedades são entregues para o cliente, em conformidade com [SCTE 67] e/ou o cliente para transmissão em fluxo do protocolo:

1.  Esquema – um URN ou um URL que identifica o esquema da mensagem.

2.  Apresentação hora – a apresentação do evento a linha cronológica de suporte de dados.

3.  Duração – a duração do evento.

4.  ID – um identificador exclusivo opcional para o evento.

5.  Mensagem – os dados do evento.


## <a name="31-smooth-streaming-delivery"></a>3.1 entrega de transmissão em fluxo uniforme

Consulte a controlar dispersa lidar com os detalhes na [MS-SSTR].

#### <a name="smooth-client-manifest-example"></a>Exemplo de manifesto do cliente uniforme
~~~ xml
<?xml version=”1.0” encoding=”utf-8”?>
<SmoothStreamingMedia MajorVersion=”2” MinorVersion=”0” TimeScale=”10000000” IsLive=”true” Duration=”0”
  LookAheadFragmentCount=”2” DVRWindowLength=”6000000000”>

  <StreamIndex Type=”video” Name=”video” Subtype=”” Chunks=”0” TimeScale=”10000000”
    Url=”QualityLevels({bitrate})/Fragments(video={start time})”>
    <QualityLevel Index=”0” Bitrate=”230000”
      CodecPrivateData=”250000010FC3460B50878A0B5821FF878780490800704704DC0000010E5A67F840” FourCC=”WVC1”
      MaxWidth=”364” MaxHeight=”272”/>
    <QualityLevel Index=”1” Bitrate=”305000”
      CodecPrivateData=”250000010FC3480B50878A0B5821FF87878049080894E4A7640000010E5A67F840” FourCC=”WVC1”
      MaxWidth=”364” MaxHeight=”272”/>
    <c t=”0” d=”20000000” r=”300” />
  </StreamIndex>
  <StreamIndex Type=”audio” Name=”audio” Subtype=”” Chunks=”0” TimeScale=”10000000”
    Url=”QualityLevels({bitrate})/Fragments(audio={start time})”>
    <QualityLevel Index=”0” Bitrate=”96000” CodecPrivateData=”1000030000000000000000000000E00042C0”
      FourCC=”WMAP” AudioTag=”354” Channels=”2” SamplingRate=”44100” BitsPerSample=”16” PacketSize=”4459”/>
    <c t=”0” d=”20000000” r=”300” />
  </StreamIndex>
  <StreamIndex Type=”text” Name=”scte35-event-stream” Subtype=”DATA” Chunks=”0” TimeScale=”10000000”
    ParentStreamIndex=”video” ManifestOutput=”true” 
    Url=”QualityLevels({bitrate})/Fragments(captions={start time})”>
    <QualityLevel Index=”0” Bitrate=”0” CodecPrivateData=”” FourCC=””>
      <CustomAttributes>
        <Attribute Name=”Scheme” Value=”urn:scte:scte35:2013a:bin”/>
      </CustomAttributes>
    </QualityLevel>
    <!-- <f> contains base-64 encoded splice_info_section() message 
    <c t=”600000000” d=”300000000”>
<f>PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0idXRmLTgiPz48QWNxdWlyZWRTaWduYWwgeG1sbnM9InVybjpjYWJsZWxhYnM6bWQ6eHNkOnNpZ25hbGluZzozLjAiIGFjcXVpc2l0aW9uUG9pbnRJZGVudGl0eT0iRVNQTl9FYXN0X0FjcXVpc2l0aW9uX1BvaW50XzEiIGFjcXVpc2l0aW9uU2lnbmFsSUQ9IjRBNkE5NEVFLTYyRkExMUUxQjFDQTg4MkY0ODI0MDE5QiIgYWNxdWlzaXRpb25UaW1lPSIyMDEyLTA5LTE4VDEwOjE0OjI2WiI+PFVUQ1BvaW50IHV0Y1BvaW50PSIyMDEyLTA5LTE4VDEwOjE0OjM0WiIvPjxTQ1RFMzVQb2ludERlc2NyaXB0b3Igc3BsaWNlQ29tbWFuZFR5cGU9IjUiPjxTcGxpY2VJbnNlcnQgc3BsaWNlRXZlbnRJRD0iMzQ0NTY4NjkxIiBvdXRPZk5ldHdvcmtJbmRpY2F0b3I9InRydWUiIHVuaXF1ZVByb2dyYW1JRD0iNTUzNTUiIGR1cmF0aW9uPSJQVDFNMFMiIGF2YWlsTnVtPSIxIiBhdmFpbHNFeHBlY3RlZD0iMTAiLz48L1NDVEUzNVBvaW50RGVzY3JpcHRvcj48U3RyZWFtVGltZXM+PFN0cmVhbVRpbWUgdGltZVR5cGU9IkhTUyIgdGltZVZhbHVlPSI1MTUwMDAwMDAwMDAiLz48L1N0cmVhbVRpbWVzPjwvQWNxdWlyZWRTaWduYWw+</f>
    </c>
    <c t=”1200000000” d=”400000000”>      <f>PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0idXRmLTgiPz48QWNxdWlyZWRTaWduYWwgeG1sbnM9InVybjpjYWJsZWxhYnM6bWQ6eHNkOnNpZ25hbGluZzozLjAiIGFjcXVpc2l0aW9uUG9pbnRJZGVudGl0eT0iRVNQTl9FYXN0X0FjcXVpc2l0aW9uX1BvaW50XzEiIGFjcXVpc2l0aW9uU2lnbmFsSUQ9IjRBNkE5NEVFLTYyRkExMUUxQjFDQTg4MkY0ODI0MDE5QiIgYWNxdWlzaXRpb25UaW1lPSIyMDEyLTA5LTE4VDEwOjE0OjI2WiI+PFVUQ1BvaW50IHV0Y1BvaW50PSIyMDEyLTA5LTE4VDEwOjE0OjM0WiIvPjxTQ1RFMzVQb2ludERlc2NyaXB0b3Igc3BsaWNlQ29tbWFuZFR5cGU9IjUiPjxTcGxpY2VJbnNlcnQgc3BsaWNlRXZlbnRJRD0iMzQ0NTY4NjkxIiBvdXRPZk5ldHdvcmtJbmRpY2F0b3I9InRydWUiIHVuaXF1ZVByb2dyYW1JRD0iNTUzNTUiIGR1cmF0aW9uPSJQVDFNMFMiIGF2YWlsTnVtPSIxIiBhdmFpbHNFeHBlY3RlZD0iMTAiLz48L1NDVEUzNVBvaW50RGVzY3JpcHRvcj48U3RyZWFtVGltZXM+PFN0cmVhbVRpbWUgdGltZVR5cGU9IkhTUyIgdGltZVZhbHVlPSI1MTYyMDAwMDAwMDAiLz48L1N0cmVhbVRpbWVzPjwvQWNxdWlyZWRTaWduYWw+</f>
    </c>
  </StreamIndex>
</SmoothStreamingMedia> 
~~~ 

## <a name="32--apple-hls-delivery"></a>3.2 Apple HLS entrega
Metadados excedeu o tempo limite para a Apple HTTP Live Streaming (HLS) podem ser incorporado nas listas de reprodução de segmento dentro de uma tag M3U personalizada.  Camada da aplicação pode analisar as listas de reprodução de M3U e processar M3U etiquetas. Media Services do Azure irá incorpore metadados excedeu o tempo limite na tag EXT-X-RECTÂNGULOS definido em [HLS].  A tag de EXT-X-RECTÂNGULOS é atualmente utilizada por DynaMux para mensagens do tipo ADI3.  Para suportar mensagens de SCTE 35 SCTE 35 e não, defina os atributos da tag EXT-X-RECTÂNGULOS como definido abaixo:

| **Nome do atributo** | **Tipo**                      | **Necessário?**                             | **Descrição**                                                                                                                                                                                                                                                                      |
|--------------------|-------------------------------|-------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| RECTÂNGULOS                | cadeia delimitada por aspas                 | Necessário                                  | A mensagem codificada como uma cadeia base64, conforme descrito em [IETF RFC 4648](http://tools.ietf.org/html/rfc4648). [SCTE-35] mensagens, este é o splice_info_section() codificado em base64.                                                                                                |
| TIPO               | cadeia delimitada por aspas                 | Necessário                                  | Um URN ou um URL que identifica o esquema de mensagem; Por exemplo, "urn: exemplo: sinalização: 1.0". Mensagens de [SCTE-35], o tipo aceita o valor de especial "scte35".                                                                                                                                |
| ID                 | cadeia delimitada por aspas                 | Necessário                                  | Um identificador exclusivo para o evento. Se o ID não for especificado, quando a mensagem é ingerida, os Media Services do Azure irá gerar um id exclusivo.                                                                                                                                          |
| DURAÇÃO           | número de ponto flutuante decimal | Necessário                                  | A duração do evento. Se desconhecido, o valor deve ser 0. As unidades são factional segundos.                                                                                                                                                                                           |
| DECORRIDO            | número de ponto flutuante decimal | Opcional, mas necessário para numa janela deslizante | Quando o sinal está a ser repetido para suportar uma janela deslizante de apresentação, este campo tem de ser a quantidade de tempo de apresentação que decorreu, uma vez que o evento começou. As unidades são fracional segundos. Este valor pode exceder a duração especificada original do splice ou segmento. |
| HORA               | número de ponto flutuante decimal | Necessário                                  | O tempo de apresentação do evento. As unidades são fracional segundos.                                                                                                                                                                                                                    |


A camada de aplicação do leitor HLS irá utilizar o tipo para identificar o formato da mensagem, descodificar a mensagem, aplicar os conversões de tempo necessário e processar o evento.  Os eventos são tempo sincronizado na lista de reprodução de segmento de controlar principal, de acordo com o carimbo de eventos.  Estes forem inseridos antes do segmento mais próximo (#EXTINF tag).

#### <a name="hls-segment-playlist-example"></a>Exemplo de listas de reprodução de segmento HLS
~~~
#EXTM3U
#EXT-X-VERSION:4
#EXT-X-ALLOW-CACHE:NO
#EXT-X-MEDIA-SEQUENCE:0
#EXT-X-TARGETDURATION:6
#EXT-X-PROGRAM-DATE-TIME:1970-01-01T00:00:00.000+00:00
#EXTINF:6.000000,no-desc
Fragments(video=0,format=m3u8-aapl)
#EXTINF:6.000000,no-desc
Fragments(video=60000000,format=m3u8-aapl)
#EXTINF:6.000000,no-desc
#EXT-X-CUE: ID=”metadata-12.000000”,TYPE=”urn:example:signaling:1.0”,TIME=”12.000000”, DURATION=”18.000000”,CUE=”HrwOi8vYmWVkaWEvhhaWFRlRDa=”
Fragments(video=120000000,format=m3u8-aapl)
#EXTINF:6.000000,no-desc
Fragments(video=180000000,format=m3u8-aapl)
#EXTINF:6.000000,no-desc
Fragments(video=240000000,format=m3u8-aapl)
#EXTINF:6.000000,no-desc
Fragments(video=300000000,format=m3u8-aapl)
#EXTINF:6.000000,no-desc
Fragments(video=360000000,format=m3u8-aapl)
#EXT-X-CUE: ID=”metadata-42.000000”,TYPE=”urn:example:signaling:1.0”,TIME=”42.000000”, DURATION=”60.000000”,CUE=”PD94bWwgdm0iMS4wIiBlbmNvpD4=”
#EXTINF:6.000000,no-desc
Fragments(video=420000000,format=m3u8-aapl)
#EXTINF:6.000000,no-desc
Fragments(video=480000000,format=m3u8-aapl)
…
~~~

#### <a name="hls-message-handling"></a>Processamento de mensagens HLS

Eventos são indicados na lista de reprodução de segmento de controlar cada áudio e vídeo. A posição da etiqueta EXT-X-RECTÂNGULOS deve sempre ser imediatamente antes dos primeiro HLS segmentar (para splice terminar ou iniciar de segmento) ou imediatamente após os último HLS segmentar (para splice na ou fim de segmento) que os respetivos atributos de hora e a duração referir-se, conforme requerido pelo [ HLS].

Quando uma janela deslizante de apresentação está ativada, a tag de EXT-X-RECTÂNGULOS deve ser repetida de com frequência suficiente que o splice ou segmento sempre totalmente descrito na lista de reprodução segmento e o atributo DECORRIDO tem de ser utilizado para indicar a quantidade de tempo a splice ou tem de segmento foi Active Directory, conforme exigido pelo [HLS].

Quando uma janela deslizante de apresentação está ativada, as etiquetas de EXT-X-RECTÂNGULOS são removidas da lista de reprodução de segmento quando o tempo de suporte de dados que consultarem foi revertida fora da janela deslizante de apresentação.

## <a name="33--dash-delivery"></a>3.3 entrega de TRAÇO
[DASH] fornece três maneiras de sinal eventos:

1.  Eventos indicados no MPD
2.  Eventos indicado na banda em representação (utilizando a caixa de mensagem de evento ('emsg')
3.  Uma combinação de 1 e 2

Eventos indicados no MPD são úteis para VOD de transmissão em fluxo porque os clientes tenham acesso a todos os eventos, imediatamente quando é transferido o MPD. A solução na banda é útil para a transmissão em direto porque os clientes não têm de transferir o MPD novamente. Relativamente à segmentação de baseados no tempo, o cliente determina o URL do segmento seguinte adicionando a duração e o carimbo do segmento atual. Se o pedido que falhar, o cliente assume uma descontinuidade e transfere o MPD, mas continua caso contrário, a transmissão em fluxo sem a transferir o MPD.

Media Services do Azure irá não ambas sinalização no MPD e sinalização utilizando a caixa de mensagem de evento na banda.

#### <a name="mpd-signaling"></a>Sinalização MPD

Eventos serão indicados no MPD utilizando o elemento EventStream, que aparece no elemento período.

O elemento de EventStream tem os seguintes atributos:

| **Nome do atributo** | **Tipo**                | **Necessário?** | **Descrição**                                                                                                                                                                                                                                                                                   |
|--------------------|-------------------------|---------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| scheme_id_uri      | string                  | Necessário      | Identifica o esquema da mensagem. O esquema está definido para o valor do atributo de esquema na caixa de manifesto do servidor em direto. O valor deve ser um URN ou um URL que identifica o esquema de mensagem; Por exemplo, "urn: exemplo: sinalização: 1.0".                                                                |
| valor              | string                  | Opcional      | Um valor de cadeia de carateres adicional utilizado pelos proprietários do esquema de personalizar a semântica de mensagem. Para diferenciar os vários fluxos de eventos com o mesmo esquema, o valor tem de ser definido para o nome do fluxo de eventos (trackName para uniforme de inserção ou nome de mensagem AMF para RTMP de inserção). |
| escala temporal          | número inteiro sem sinal de 32 bits | Necessário      | Escala de temporal, no ticks por segundo, das horas e os campos de duração na caixa de 'emsg'.                                                                                                                                                                                                       |


Zero ou mais elementos de eventos estão contidos no elemento EventStream e têm os seguintes atributos:

| **Nome do atributo**  | **Tipo**                | **Necessário?** | **Descrição**                                                                                                                                                                                                             |
|---------------------|-------------------------|---------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| presentation_time   | número inteiro sem sinal de 64 bits | Opcional      | TEM de ser o suporte de dados de tempo de apresentação do evento relativo para o início do período de. A hora de apresentação e a duração devem alinhar com pontos de acesso de fluxo (SAP) do tipo 1 ou 2, tal como definido em [ISO-14496-12] Annex I. |
| Duração            | número inteiro sem sinal de 32 bits | Opcional      | A duração do evento. Isto tem de ser omitido se a duração é desconhecida.                                                                                                                                                 |
| ID                  | número inteiro sem sinal de 32 bits | Opcional      | Identifica esta instância da mensagem. As mensagens com semântica equivalente deverá ter o mesmo valor. Se o ID não for especificado, quando a mensagem é ingerida, os Media Services do Azure irá gerar um id exclusivo.             |
| Valor de elemento do evento | string                  | Necessário      | A mensagem de evento como uma cadeia base64, conforme descrito em [IETF RFC 4648](http://tools.ietf.org/html/rfc4648).                                                                                                                   |

#### <a name="xml-syntax-and-example-for-dash-manifest-mpd-signaling"></a>Sintaxe de XML e o exemplo para DASH manifestam Signaling (MPD)

~~~ xml
<!-- XML Syntax -->
<xs:complexType name=”EventStreamType”>
  <xs:sequence>
    <xs:element name=”Event” type=”EventType” minOccurs=”0” maxOccurs=”unbounded”/>
    <xs:any namespace=”##other” processContents=”lax” minOccurs=”0” maxOccurs=”unbounded”/>
  </xs:sequence>
  <xs:attribute ref=”xlink:href”/>
  <xs:attribute ref=”xlink:actuate” default=”onRequest”/>
  <xs:attribute name=”schemeIdUri” type=”xs:anyURI” use=”required”/>
  <xs:attribute name=”value” type=”xs:string”/>
  <xs:attribute name=”timescale” type=”xs:unsignedInt”/>
</xs:complexType>
<!-- Event -->
<xs:complexType name=”EventType”>
  <xs:sequence>
    <xs:any namespace=”##other” processContents=”lax” minOccurs=”0” maxOccurs=”unbounded”/>
  </xs:sequence>
  <xs:attribute name=”presentationTime” type=”xs:unsignedLong” default=”0”/>
  <xs:attribute name=”duration” type=”xs:unsignedLong”/>
  <xs:attribute name=”id” type=”xs:unsignedInt”/>
  <xs:anyAttribute namespace=”##other” processContents=”lax”/>
</xs:complexType><?xml version=”1.0” encoding=”utf-8”?>


<!-- Example Section in MPD -->

<EventStream schemeIdUri=”urn:example:signaling:1.0” timescale=”1000” value=”player-statistics”>
  <Event presentationTime=”0” duration=”10000” id=”0”> PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0idXRmLTgiPz48QWNxdWlyZWRTaWduYWwgeG1sbnM9InVybjpjYWJsZWxhYnM6bWQ6eHNkOnNpZ25hbGluZzozLjAiIGFjcXVpc2l0aW9uUG9pbnRJZGVudGl0eT0iRVNQTl9FYXN0X0FjcXVpc2l0aW9uX1BvaW50XzEiIGFjcXVpc2l0aW9uU2lnbmFsSUQ9IjRBNkE5NEVFLTYyRkExMUUxQjFDQTg4MkY0ODI0MDE5QiIgYWNxdWlzaXRpb25UaW1lPSIyMDEyLTA5LTE4VDEwOjE0OjI2WiI+PFVUQ1BvaW50IHV0Y1BvaW50PSIyMDEyLTA5LTE4VDEwOjE0OjM0WiIvPjxTQ1RFMzVQb2ludERlc2NyaXB0b3Igc3BsaWNlQ29tbWFuZFR5cGU9IjUiPjxTcGxpY2VJbnNlcnQgc3BsaWNlRXZlbnRJRD0iMzQ0NTY4NjkxIiBvdXRPZk5ldHdvcmtJbmRpY2F0b3I9InRydWUiIHVuaXF1ZVByb2dyYW1JRD0iNTUzNTUiIGR1cmF0aW9uPSJQVDFNMFMiIGF2YWlsTnVtPSIxIiBhdmFpbHNFeHBlY3RlZD0iMTAiLz48L1NDVEUzNVBvaW50RGVzY3JpcHRvcj48U3RyZWFtVGltZXM+PFN0cmVhbVRpbWUgdGltZVR5cGU9IkhTUyIgdGltZVZhbHVlPSI1MTUwMDAwMDAwMDAiLz48L1N0cmVhbVRpbWVzPjwvQWNxdWlyZWRTaWduYWw+</Event>
  <Event presentationTime=”20000” duration=”10000” id=”1”> PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0idXRmLTgiPz48QWNxdWlyZWRTaWduYWwgeG1sbnM9InVybjpjYWJsZWxhYnM6bWQ6eHNkOnNpZ25hbGluZzozLjAiIGFjcXVpc2l0aW9uUG9pbnRJZGVudGl0eT0iRVNQTl9FYXN0X0FjcXVpc2l0aW9uX1BvaW50XzEiIGFjcXVpc2l0aW9uU2lnbmFsSUQ9IjRBNkE5NEVFLTYyRkExMUUxQjFDQTg4MkY0ODI0MDE5QiIgYWNxdWlzaXRpb25UaW1lPSIyMDEyLTA5LTE4VDEwOjE0OjI2WiI+PFVUQ1BvaW50IHV0Y1BvaW50PSIyMDEyLTA5LTE4VDEwOjE0OjM0WiIvPjxTQ1RFMzVQb2ludERlc2NyaXB0b3Igc3BsaWNlQ29tbWFuZFR5cGU9IjUiPjxTcGxpY2VJbnNlcnQgc3BsaWNlRXZlbnRJRD0iMzQ0NTY4NjkxIiBvdXRPZk5ldHdvcmtJbmRpY2F0b3I9InRydWUiIHVuaXF1ZVByb2dyYW1JRD0iNTUzNTUiIGR1cmF0aW9uPSJQVDFNMFMiIGF2YWlsTnVtPSIxIiBhdmFpbHNFeHBlY3RlZD0iMTAiLz48L1NDVEUzNVBvaW50RGVzY3JpcHRvcj48U3RyZWFtVGltZXM+PFN0cmVhbVRpbWUgdGltZVR5cGU9IkhTUyIgdGltZVZhbHVlPSI1MTYyMDAwMDAwMDAiLz48L1N0cmVhbVRpbWVzPjwvQWNxdWlyZWRTaWduYWw+</Event>
</EventStream>
~~~

>[!NOTE]
>Tenha em atenção que presentationTime é o tempo de apresentação do evento, não a hora da chegada da mensagem.
>

### <a name="431-in-band-event-message-box-signaling"></a>4.3.1 na banda eventos sinalização de caixa de mensagens
Um fluxo de eventos na banda requer MPD ter um elemento de InbandEventStream ao nível do Adaptation definido.  Este elemento tem um atributo obrigatório schemeIdUri e o atributo escala temporal opcional, que também são apresentados de eventos a caixa de mensagem ('emsg').  Caixas de mensagem de evento com identificadores de esquema que não estão definidas no MPD não devem estar presentes. Se um cliente DASH Deteta uma caixa de mensagem de eventos com um esquema que não está definido no MPD, o cliente espera é ignorá-lo.
A caixa de mensagem de evento ('emsg') fornece sinalização genérico eventos relacionados com a hora de apresentação do suporte de dados. Se estiver presente, qualquer caixa 'emsg' deverá ser colocada antes de qualquer caixa 'moof'.

### <a name="dash-event-message-box-emsg"></a>Caixa de mensagem de evento DASH 'emsg'
~~~
Box Type: `emsg’
Container: Segment
Mandatory: No
Quantity: Zero or more
~~~

~~~ c
aligned(8) class DASHEventMessageBox extends FullBox(‘emsg’, version = 0, flags =0) 
{
    string scheme_id_uri;
    string value;
    unsigned int(32) timescale;
    unsigned int(32) presentation_time_delta;
    unsigned int(32) event_duration;
    unsigned int(32) id;
    unsigned int(8) message_data[];
}
~~~

Os campos do DASHEventMessageBox são definidos abaixo:

| **Nome do campo**          | **Tipo de campo**          | **Necessário?** | **Descrição**                                                                                                                                                                                                                                                                                                                                                    |
|-------------------------|-------------------------|---------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| scheme_id_uri           | string                  | Necessário      | Identifica o esquema da mensagem. O esquema está definido para o valor do atributo de esquema na caixa de manifesto do servidor em direto. O valor deve ser um URN ou um URL que identifica o esquema de mensagem; Por exemplo, "urn: exemplo: sinalização: 1.0". [SCTE-35] mensagens, esta ação demora o valor especial "urn: scte:scte35:2013a:bin", embora [SCTE 67] recomenda outra coisa. |
| Valor                   | string                  | Necessário      | Um valor de cadeia de carateres adicional utilizado pelos proprietários do esquema de personalizar a semântica de mensagem. Para diferenciar os vários fluxos de eventos com o mesmo esquema, o valor será definido para o nome do fluxo de eventos (trackName para uniforme de inserção ou nome de mensagem AMF para RTMP de inserção).                                                                  |
| escala temporal               | número inteiro sem sinal de 32 bits | Necessário      | Escala de temporal, no ticks por segundo, das horas e os campos de duração na caixa de 'emsg'.                                                                                                                                                                                                                                                                        |
| Presentation_time_delta | número inteiro sem sinal de 32 bits | Necessário      | O suporte de dados apresentação intervalo de tempo do tempo de apresentação do evento e o tempo de apresentação mais antigo neste segmento. A hora de apresentação e a duração devem alinhar com pontos de acesso de fluxo (SAP) do tipo 1 ou 2, tal como definido em [ISO-14496-12] Annex I.                                                                                            |
| event_duration          | número inteiro sem sinal de 32 bits | Necessário      | A duração do evento ou 0xFFFFFFFF para indicar uma duração de desconhecido.                                                                                                                                                                                                                                                                                          |
| Id                      | número inteiro sem sinal de 32 bits | Necessário      | Identifica esta instância da mensagem. As mensagens com semântica equivalente deverá ter o mesmo valor. Se o ID não for especificado, quando a mensagem é ingerida, os Media Services do Azure irá gerar um id exclusivo.                                                                                                                                                    |
| Message_data            | matriz de bytes              | Necessário      | A mensagem de evento. Mensagens [SCTE-35], os dados da mensagem são splice_info_section() binário, embora [SCTE 67] recomenda outra coisa.                                                                                                                                                                                                                                 |

### <a name="332-dash-message-handling"></a>3.3.2 TRAÇO processamento de mensagens

Eventos são indicados na banda, na caixa 'emsg', para a atividade de áudio e vídeo.  O sinalização ocorre para segmentar todos os pedidos para o qual o presentation_time_delta é 15 segundos ou menos. Quando uma janela deslizante de apresentação está ativada, mensagens de eventos são removidas a MPD quando a soma do tempo e a duração da mensagem de evento são inferior à hora dos dados de suporte de dados no manifesto.  Por outras palavras, as mensagens de evento são removidas do manifesto quando o tempo de suporte de dados para o qual Consulte foi revertido fora da janela deslizante de apresentação.

## <a name="4-scte-35-ingest"></a>4 SCTE-35 de inserção

[SCTE-35] mensagens são ingeridas em formato binário, utilizando o esquema **"urn: scte:scte35:2013a:bin"** para uniforme inserção e o tipo **"scte35"** para RTMP de inserção. Para facilitar a conversão de temporização [SCTE-35], que se baseia no MPEG-2 transporte fluxo apresentação tempo carimbos (PTS), um mapeamento entre PTS (pts_time + pts_adjustment o splice_time()) e a linha cronológica de suporte de dados é fornecido pelo (de tempo de apresentação de eventos o campo de fragment_absolute_time para uniforme ingestão e o campo de hora para RTMP de inserção). O mapeamento é necessário porque o valor PTS 33 bits faz através de aproximadamente a cada 26.5 horas.

Ingestão da transmissão em fluxo uniforme requer que a caixa de dados do suporte de dados ('mdat') tem de conter o **splice_info_section()** definida na tabela 8-1 [SCTE-35]. Para RTMP de inserção, o atributo rectângulos da mensagem AMF é definido para o base64encoded **splice_info_section()**. Quando as mensagens têm o formato descrito acima, são enviadas para HLS, uniforme, Dash clientes e em conformidade com [SCTE 67].


## <a name="5-references"></a>5 referências

**[SCTE-35]**  ANSI/SCTE 35 2013a – programa Digital inserção Cueing mensagem de cabo, 2013a

**[SCTE 67]**  ANSI/SCTE 67 2014 – recomendado prática para SCTE 35: programa Digital inserção Cueing mensagem para cabo

**[DASH]**  ISO/IEC 23009-1 2014 – tecnologia de informações – Dynamic através de HTTP (DASH) – parte 1 de transmissão em fluxo adaptável: formatos de descrição e o segmento de apresentação de suporte de dados, 2nd edition

**[HLS]**  ["HTTP em direto de transmissão em fluxo", draft-pantos-http-live-streaming-14, 14 de Outubro de 2014](http://tools.ietf.org/html/draft-pantos-http-live-streaming-14)

**[MS-SSTR]**  ["Microsoft uniforme transmissão em fluxo de protocolo", 15 de Maio de 2014](http://download.microsoft.com/download/9/5/E/95EF66AF-9026-4BB0-A41D-A4F81802D92C/%5bMS-SSTR%5d.pdf)

**[AMF0]**  ["AMF0 de formato de mensagem de ação"](http://download.macromedia.com/pub/labs/amf/amf0_spec_121207.pdf)

**[FMP4 EM DIRETO]**  [Especificação de inserção de Media Services do azure Live MP4 fragmentados](https://docs.microsoft.com/azure/media-services/media-services-fmp4-live-ingest-overview)

**[ISO-14496-12]**  ISO/IEC 14496-12: formato, quarta 2012 edição-07-15 de ficheiros de suporte de dados base parte 12 ISO.

**[RTMP]**  ["Do adobe em tempo real mensagens protocolo", 21 de Dezembro de 2012](https://www.adobe.com/devnet/rtmp.html) 

------------------------------------------

## <a name="next-steps"></a>Passos Seguintes
Vista dos Media Services percursos de aprendizagem.

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Enviar comentários
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]
