---
title: Texto para reconhecimento de voz API do serviço de reconhecimento de voz Microsoft | Microsoft Docs
description: Utilize o API do reconhecimento de voz para de texto para fornecer conversão text-to-speech em tempo real numa variedade de voices e idiomas
services: cognitive-services
author: priyaravi20
manager: yanbo
ms.service: cognitive-services
ms.component: bing-speech
ms.topic: article
ms.date: 03/16/2017
ms.author: priyar
ms.openlocfilehash: 4b633cefa37c11511a8171d5a7f61b03dfaa4466
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352519"
---
# <a name="bing-text-to-speech-api"></a>Reconhecimento de voz do Bing texto para API

## <a name="Introduction"></a>Introdução

Com o Bing texto para reconhecimento de voz API, a aplicação pode enviar pedidos de HTTP para um servidor de nuvem, em que o texto de forma instantânea é synthesized para reconhecimento de voz sounding humanos e devolvido como um ficheiro de áudio. Esta API pode ser utilizada em vários contextos diferentes para fornecer conversão text-to-speech em tempo real numa variedade de diferentes voices e idiomas.

## <a name="VoiceSynReq"></a>Pedido de synthesis de voz

### <a name="Subscription"></a>Token de autorização

Todos os pedidos de synthesis voz necessita de um token de acesso de Token Web JSON (JWT). O token de acesso JWT é transferido no cabeçalho do pedido de reconhecimento de voz. O token tem uma hora de expiração de 10 minutos. Para obter informações sobre a subscrição e a obtenção de chaves de API que são utilizadas para obter os tokens de acesso do válido JWT, consulte [subscrição de serviços cognitivos](https://azure.microsoft.com/try/cognitive-services/).

A chave de API é passada para o serviço de tokens. Por exemplo:

```HTTP
POST https://api.cognitive.microsoft.com/sts/v1.0/issueToken
Content-Length: 0
```

Segue-se as informações de cabeçalho necessárias para acesso de token.

Nome| Formato | Descrição
----|----|----
OCP Apim-subscrição-chave | ASCII | Chave de subscrição

O serviço de token devolve o token de acesso JWT como `text/plain`. Em seguida, o JWT é transmitida como um `Base64 access_token` para o ponto final de reconhecimento de voz como um prefixo com a cadeia de cabeçalho de autorização `Bearer`. Por exemplo:

`Authorization: Bearer [Base64 access_token]`

Os clientes têm de utilizar o seguinte ponto final para aceder ao serviço text-to-speech:

`https://speech.platform.bing.com/synthesize`

>[!NOTE]
>Até ter adquirir um token de acesso com a sua chave de subscrição, conforme descrito anteriormente, esta ligação gera um `403 Forbidden` erro de resposta.

### <a name="Http"></a>Cabeçalhos de HTTP

A tabela seguinte mostra os cabeçalhos HTTP que são utilizados para pedidos de synthesis de voz.

Cabeçalho |Valor |Comentários
----|----|----
Content-Type | aplicação/ssml + xml | O tipo de conteúdo de entrada.
X-Microsoft-OutputFormat | **1.** ssml-16 khz-16 bits-mono-tts <br> **2.** em bruto-16 khz-16 bits-mono-pcm <br>**3.** áudio-16 khz-16 kbps-mono-siren <br> **4.** riff-16 khz-16 kbps-mono-siren <br> **5.** riff-16 khz-16 bits-mono-pcm <br> **6.** áudio-16 khz-128kbitrate-mono-mp3 <br> **7.** áudio-16 khz-64kbitrate-mono-mp3 <br> **8.** áudio-16 khz-32kbitrate-mono-mp3 | O formato de áudio de saída.
X-pesquisa-AppId | Um GUID (hex apenas, não traços) | Um ID que identifica exclusivamente a aplicação de cliente. Isto pode ser o ID de arquivo para aplicações. Se não estiver disponível, o ID pode ser gerado para uma aplicação do utilizador.
X-pesquisa-ClientID | Um GUID (hex apenas, não traços) | Um ID que identifica exclusivamente uma instância de aplicação para cada instalação.
Agente de utilizador | Nome da aplicação | O nome da aplicação é necessário e tem de ter menos de 255 carateres.
Autorização | Token de autorização |  Consulte o <a href="#Subscription">token de autorização</a> secção.

### <a name="InputParam"></a>Parâmetros de entrada

São efetuados os pedidos para o Bing texto para reconhecimento de voz API utilizando chamadas de HTTP POST. Os cabeçalhos estão especificados na secção anterior. O corpo contém uma entrada de voz Synthesis Markup Language (SSML) que representa o texto a ser synthesized. Para obter uma descrição do markup utilizada para controlar os aspetos de reconhecimento de voz como o idioma e género de altifalante, consulte o [SSML W3C especificação](http://www.w3.org/TR/speech-synthesis/).

>[!NOTE]
>O tamanho máximo da entrada SSML suportada é 1024 caracteres, incluindo todas as etiquetas.

###  <a name="SampleVoiceOR"></a>Exemplo: pedido de saída de voz

Um exemplo de um pedido de saída de voz é o seguinte:

```HTTP
POST /synthesize
HTTP/1.1
Host: speech.platform.bing.com

X-Microsoft-OutputFormat: riff-8khz-8bit-mono-mulaw
Content-Type: application/ssml+xml
Host: speech.platform.bing.com
Content-Length: 197
Authorization: Bearer [Base64 access_token]

<speak version='1.0' xml:lang='en-US'><voice xml:lang='en-US' xml:gender='Female' name='Microsoft Server Speech Text to Speech Voice (en-US, ZiraRUS)'>Microsoft Bing Voice Output API</voice></speak>
```

## <a name="VoiceOutResponse"></a>Resposta de saída de voz

O Bing texto para reconhecimento de voz API utiliza HTTP POST para enviar áudio novamente para o cliente. Resposta da API contém a sequência de áudio e o codec e corresponde ao formato de saída pedida. Áudio devolvido para um determinado pedido não pode exceder os 15 segundos.

### <a name="SuccessfulRecResponse"></a>Exemplo: de resposta de synthesis com êxito

O código seguinte é um exemplo de uma resposta JSON para um pedido de synthesis voz concluída com êxito. Os comentários e formatação do código estão para efeitos deste exemplo apenas e estão omitidos da resposta real.

```HTTP
HTTP/1.1 200 OK
Content-Length: XXX
Content-Type: audio/x-wav

Response audio payload
```

### <a name="RecFailure"></a>Exemplo: falha de synthesis

O código de exemplo seguinte mostra uma resposta JSON uma falha de consulta de voz synthesis:

```HTTP
HTTP/1.1 400 XML parser error
Content-Type: text/xml
Content-Length: 0
```

### <a name="ErrorResponse"></a>Respostas de erro

Erro | Descrição
----|----
Pedido de HTTP/400 incorreto | Um parâmetro necessário está em falta, estar vazio ou é nulo ou o valor transmitido como um parâmetro necessário ou opcional é inválido. Um motivo para obter a resposta "inválida" é a transmissão de um valor de cadeia que é maior do que o comprimento permitido. Uma breve descrição do parâmetro problemática está incluída.
HTTP/401-não autorizado | O pedido não está autorizado.
HTTP/413 RequestEntityTooLarge  | A entrada SSML é maior do que o que é suportado.
HTTP/502 BadGateway | Não há um problema relacionado com a rede ou um problema do lado do servidor.

Um exemplo de uma resposta de erro é o seguinte:

```HTTP
HTTP/1.0 400 Bad Request
Content-Length: XXX
Content-Type: text/plain; charset=UTF-8

Voice name not supported
```

## <a name="ChangeSSML"></a>Alterar a saída de voz através de SSML

Microsoft Text-to-Speech API suporta SSML 1.0, tal como definido no W3C [voz Synthesis Markup Language (SSML) versão 1.0](http://www.w3.org/TR/2009/REC-speech-synthesis-20090303/). Esta secção mostra exemplos de alterar a determinados características da saída de voz gerado como falando classificar, pronunciação etc. através da utilização de etiquetas SSML.

1. Quebra a adicionar

  ```
  <speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'><voice  name='Microsoft Server Speech Text to Speech Voice (en-US, BenjaminRUS)'> Welcome to use Microsoft Cognitive Services <break time="100ms" /> Text-to-Speech API.</voice> </speak>
  ```

2. Taxa de alteração falantes

  ```
  <speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'><voice  name='Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)'><prosody rate="+30.00%">Welcome to use Microsoft Cognitive Services Text-to-Speech API.</prosody></voice> </speak>
  ```

3. Pronunciação

  ```
  <speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'><voice  name='Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)'> <phoneme alphabet="ipa" ph="t&#x259;mei&#x325;&#x27E;ou&#x325;"> tomato </phoneme></voice> </speak>
  ```

4. Volume de alterações

  ```
  <speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'><voice  name='Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)'><prosody volume="+20.00%">Welcome to use Microsoft Cognitive Services Text-to-Speech API.</prosody></voice> </speak>
  ```

5. Tipos de alteração

  ```
  <speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'><voice  name='Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)'>Welcome to use <prosody pitch="high">Microsoft Cognitive Services Text-to-Speech API.</prosody></voice> </speak>
  ```

6. Contour prosody de alteração

  ```
  <speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'><voice  name='Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)'><prosody contour="(80%,+20%) (90%,+30%)" >Good morning.</prosody></voice> </speak>
  ```

> [!NOTE]
> Tenha em atenção os dados de áudio tem de ser 8k ou 16 wav k registada no seguinte formato: **código CRC** (CRC-32): 4 bytes (DWORD) com o intervalo válido 0x00000000 ~ 0xFFFFFFFF; **Sinalizador de formato de áudio**: 4 bytes (DWORD) com o intervalo válido 0x00000000 ~ 0xFFFFFFFF; **Contagem de amostras**: 4 bytes (DWORD) com o intervalo válido 0x00000000 ~ 0x7FFFFFFF; **Tamanho do corpo binário**: 4 bytes (DWORD) com o intervalo válido 0x00000000 ~ 0x7FFFFFFF; **Corpo binário**: n bytes.

## <a name="SampleApp"></a>Exemplo de aplicação

Para detalhes de implementação, consulte o [aplicação de exemplo text-to-speech Visual C# .NET](https://github.com/Microsoft/Cognitive-Speech-TTS/blob/master/Samples-Http/CSharp/TTSProgram.cs).

## <a name="SupLocales"></a>Regiões suportadas e tipos de letra de voz

A tabela seguinte identifica algumas das regiões suportadas e tipos de letra de voz relacionados.

Região | Género | Mapeamento de nome de serviço
---------|--------|------------
ar ex * | Feminino | "Microsoft texto de reconhecimento de voz de servidor para chamadas de voz (ar-por exemplo, Hoda)"
ar SA | Masculino | "Microsoft Server voz texto voz de reconhecimento de voz (ar-SA, Naayf)"
bg-BG | Masculino | "Microsoft Server voz reconhecimento de voz do texto para voz (bg-BG, Ivan)"
ES de AC | Feminino | "Microsoft Server voz reconhecimento de voz do texto para voz (ca-ES, HerenaRUS)"
cs-CZ | Masculino | "Microsoft texto de reconhecimento de voz de servidor para chamadas de voz (cs-CZ, Jakub)"
da-DK | Feminino | "Microsoft Server voz texto voz de reconhecimento de voz (da-DK, HelleRUS)"
AT da Alemanha | Masculino | "Microsoft Server voz texto voz de reconhecimento de voz (Alemanha-AT, Miguel)"
Categoria da Alemanha | Masculino | "Microsoft Server voz texto voz de reconhecimento de voz (Alemanha-categoria, Karsten)"
Alemanha da Alemanha | Feminino | "Microsoft Server voz texto voz de reconhecimento de voz (Alemanha-Alemanha, Hedda)"
Alemanha da Alemanha | Feminino | "Microsoft Server voz texto voz de reconhecimento de voz (Alemanha-Alemanha, HeddaRUS)"
Alemanha da Alemanha | Masculino | "Microsoft Server voz texto voz de reconhecimento de voz (Alemanha-Alemanha, Stefan, Apollo)"
el GR | Masculino | "Microsoft Server voz texto voz de reconhecimento de voz (el-GR, Stefanos)"
EN AU | Feminino | "Microsoft Server voz texto voz de reconhecimento de voz (en-AU, Catherine)"
EN AU | Feminino | "Microsoft Server voz texto voz de reconhecimento de voz (en-AU, HayleyRUS)"
en-CA | Feminino | "Microsoft Server voz texto voz de reconhecimento de voz (en-CA, Linda)"
en-CA | Feminino | "Microsoft Server voz texto voz de reconhecimento de voz (en-CA, HeatherRUS)"
en GB | Feminino | "Microsoft Server voz texto voz de reconhecimento de voz (en-GB, Susan, Apollo)"
en GB | Feminino | "Microsoft Server voz texto voz de reconhecimento de voz (en-GB, HazelRUS)"
en GB | Masculino | "Microsoft Server voz texto voz de reconhecimento de voz (en-GB, George, Apollo)"
EN-i/e | Masculino | "Microsoft Server voz texto voz de reconhecimento de voz (en-IE, Sean)"
EN-IN | Feminino | "Microsoft Server voz texto voz de reconhecimento de voz (en-IN, Heera, Apollo)"
EN-IN | Feminino | "Microsoft Server voz texto voz de reconhecimento de voz (en-IN, PriyaRUS)"
EN-IN | Masculino | "Microsoft Server voz texto voz de reconhecimento de voz (en-IN, Ravi, Apollo)"
pt-PT | Feminino | "Microsoft Server voz texto voz de reconhecimento de voz (en-US, ZiraRUS)"
pt-PT | Feminino | "Microsoft Server voz texto voz de reconhecimento de voz (en-US, JessaRUS)"
pt-PT | Masculino | "Microsoft Server voz texto voz de reconhecimento de voz (en-US, BenjaminRUS)"
es-ES | Feminino | "Microsoft Server voz reconhecimento de voz do texto para voz (es-ES, Laura, Apollo)"
es-ES | Feminino | "Microsoft Server voz reconhecimento de voz do texto para voz (es-ES, HelenaRUS)"
es-ES | Masculino | "Microsoft Server voz reconhecimento de voz do texto para voz (es-ES, Pablo, Apollo)"
es-MX | Feminino | "Microsoft Server voz reconhecimento de voz do texto para voz (es-MX, HildaRUS)"
es-MX | Masculino | "Microsoft Server voz reconhecimento de voz do texto para voz (es-MX, Raul, Apollo)"
fi-FI | Feminino | "Microsoft Server voz texto voz de reconhecimento de voz (fi-FI, HeidiRUS)"
fr-CA | Feminino | "Microsoft Server voz reconhecimento de voz do texto para voz (fr-CA, Caroline)"
fr-CA | Feminino | "Microsoft Server voz reconhecimento de voz do texto para voz (fr-CA, HarmonieRUS)"
FR categoria | Masculino | "Microsoft Server voz reconhecimento de voz do texto para voz (fr-categoria, Guillaume)"
FR-FR | Feminino | "Microsoft Server voz reconhecimento de voz do texto para voz (fr-FR, Julie, Apollo)"
FR-FR | Feminino | "Microsoft Server voz reconhecimento de voz do texto para voz (fr-FR, HortenseRUS)"
FR-FR | Masculino | "Microsoft Server voz reconhecimento de voz do texto para voz (fr-FR, Paul, Apollo)"
IL putador| Masculino| "Microsoft Server voz texto voz de reconhecimento de voz (putador-IL, Asaf)"
IN | Feminino | "Microsoft Server voz texto voz de reconhecimento de voz (Olá-IN, Kalpana, Apollo)"
IN | Feminino | "Microsoft Server voz texto voz de reconhecimento de voz (Olá-IN, Kalpana)"
IN | Masculino | "Microsoft Server voz texto voz de reconhecimento de voz (Olá-IN, Hemant)"
HR de RH | Masculino | "Microsoft Server voz reconhecimento de voz do texto para voz (hr-RH, Matej)"
hu-HU | Masculino | "Microsoft Server voz texto voz de reconhecimento de voz (hu-HU, Szabolcs)"
id-ID | Masculino | "Microsoft Server voz texto voz de reconhecimento de voz (id-ID, Andika)"
it-IT | Masculino | "Microsoft Server voz texto voz de reconhecimento de voz (it-IT, Cosimo, Apollo)"
ja-JP | Feminino | "Microsoft Server voz reconhecimento de voz do texto para voz (ja-JP, Ayumi, Apollo)"
ja-JP | Masculino | "Microsoft Server voz reconhecimento de voz do texto para voz (ja-JP, Ichiro, Apollo)"
ja-JP | Feminino | "Microsoft Server voz reconhecimento de voz do texto para voz (ja-JP, HarukaRUS)"
ja-JP | Feminino | "Microsoft Server voz reconhecimento de voz do texto para voz (ja-JP, LuciaRUS)"
ja-JP | Masculino | "Microsoft Server voz reconhecimento de voz do texto para voz (ja-JP, EkaterinaRUS)"
ko-KR | Feminino | "Microsoft Server voz texto voz de reconhecimento de voz (ko-KR, HeamiRUS)"
Os meus MS | Masculino | "Voz de reconhecimento de voz do texto para reconhecimento de voz de servidor da Microsoft (ms-os meus, Rizwan)"
nb-NO | Feminino | "Microsoft Server voz reconhecimento de voz do texto para voz (não-nb, HuldaRUS)"
NL-NL | Feminino | "Microsoft Server voz reconhecimento de voz do texto para voz (nl-NL, HannaRUS)"
pl-PL | Feminino | "Microsoft Server voz reconhecimento de voz do texto para voz (pl-PL, PaulinaRUS)"
pt-BR | Feminino | "Microsoft Server voz reconhecimento de voz do texto para voz (pt-BR, HeloisaRUS)"
pt-BR | Masculino | "Microsoft Server voz reconhecimento de voz do texto para voz (pt-BR, Daniel, Apollo)"
pt-PT | Feminino | "Microsoft Server voz reconhecimento de voz do texto para voz (pt-PT, HeliaRUS)"
RO RO | Masculino | "Microsoft Server voz texto voz de reconhecimento de voz (ro-RO, Andrei)"
ru-RU | Feminino | "Microsoft Server voz reconhecimento de voz do texto para voz (ru-RU, Irina, Apollo)"
ru-RU | Masculino | "Microsoft Server voz reconhecimento de voz do texto para voz (ru-RU, Pavel, Apollo)"
sk SK | Masculino | "Microsoft Server voz reconhecimento de voz do texto para voz (sk-SK, Filip)"
TAMA SL | Masculino | "Microsoft Server voz reconhecimento de voz do texto para voz (sl-TAMA, Lado)"
SV-SE | Feminino | "Microsoft Server voz texto voz de reconhecimento de voz (sv-SE, HedvigRUS)"
dados em | Masculino | "Microsoft Server voz texto voz de reconhecimento de voz (TIR-IN, Valluvar)"
ésimo-Ésimo | Masculino | "Microsoft Server voz reconhecimento de voz do texto para voz (ésimo-Ésimo, Pattara)"
tr-TR | Feminino | "Microsoft Server voz reconhecimento de voz do texto para voz (tr-TR, SedaRUS)"
vi-VN | Masculino | "Microsoft Server voz reconhecimento de voz do texto para voz (vi-VN, um)"
zh-CN | Feminino | "Microsoft Server voz texto voz de reconhecimento de voz (zh-CN, HuihuiRUS)"
zh-CN | Feminino | "Microsoft Server voz texto voz de reconhecimento de voz (zh-CN, Yaoyao, Apollo)"
zh-CN | Masculino | "Microsoft Server voz texto voz de reconhecimento de voz (zh-CN, Kangkang, Apollo)"
zh-HK | Feminino | "Microsoft Server voz texto voz de reconhecimento de voz (zh-HK Tracy, Apollo)"
zh-HK | Feminino | "Microsoft Server voz texto voz de reconhecimento de voz (zh-HK TracyRUS)"
zh-HK | Masculino | "Microsoft Server voz texto voz de reconhecimento de voz (zh-HK Danny, Apollo)"
zh-TW | Feminino | "Microsoft Server voz texto voz de reconhecimento de voz (zh-TW, Yating, Apollo)"
zh-TW | Feminino | "Microsoft Server voz texto voz de reconhecimento de voz (zh-TW, HanHanRUS)"
zh-TW | Masculino | "Microsoft Server voz texto voz de reconhecimento de voz (zh-TW, Zhiwei, Apollo)"
 * ar-ex suporta moderna padrão Árabe (MSA).

> [!NOTE]
> Tenha em atenção que os nomes de serviço anteriores **texto de reconhecimento de voz de servidor Microsoft para chamadas de voz (cs-CZ, Vit)** e **texto de reconhecimento de voz de servidor Microsoft para reconhecimento de voz voz (en-IE, Shaun)** vão ser preteridas após 31/3/2018, no ordem para otimizar as capacidades da API de reconhecimento de voz do Bing. Atualize o seu código com os nomes de atualizado.

## <a name="TrouNSupport"></a>Resolução de problemas e suporte

Publicar todas as perguntas e problemas para o [serviço de reconhecimento de voz do Bing](https://social.msdn.microsoft.com/Forums/en-US/home?forum=SpeechService) fórum MSDN. Inclua detalhes completos sobre, tais como:

* Um exemplo da cadeia de pedido completo.
* Se aplicável, o resultado completo de um pedido falhado, que inclui o registo IDs.
* A percentagem de pedidos que estão a falhar.
