---
title: REST APIs do serviço de voz
description: Referência para as APIs REST para o serviço de voz.
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: speech
ms.topic: article
ms.date: 05/09/2018
ms.author: v-jerkin
ms.openlocfilehash: fea4f762a46963f923ba8f44644df37b29fc77a6
ms.sourcegitcommit: 7b0778a1488e8fd70ee57e55bde783a69521c912
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/10/2018
ms.locfileid: "49068745"
---
# <a name="speech-service-rest-apis"></a>REST APIs do serviço de voz

As APIs REST do serviço de voz dos serviços cognitivos do Azure são semelhantes às APIs fornecidas pelos [API de voz do Bing](https://docs.microsoft.com/azure/cognitive-services/Speech). Os pontos de extremidade diferem dos pontos de extremidade utilizados pelo serviço de voz do Bing. Pontos finais regionais estão disponíveis e tem de utilizar uma chave de assinatura que corresponde ao ponto final que está a utilizar.

## <a name="speech-to-text"></a>Conversão de Voz em Texto

Os pontos finais para a conversão de voz em texto REST API são mostrados na tabela seguinte. Utilize um que corresponde à sua região de subscrição. 

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-speech-to-text.md)]

> [!NOTE]
> Se personalizou o modelo acústico ou modelo de idioma ou pronúncia, em vez disso, a utilizar o seu ponto final personalizado.

Esta API suporta apenas os discursos curtos. Os pedidos podem conter até 10 segundos de áudio e um máximo de 14 segundos geral da última. A API REST devolve resultados apenas finais, não resultados parciais ou provisórias. O serviço de voz também tem um [transcrição do batch](batch-transcription.md) API que pode transcrição de áudio mais tempo.


### <a name="query-parameters"></a>Parâmetros de consulta

Os seguintes parâmetros podem ser incluídos na cadeia de consulta da solicitação REST.

|Nome do parâmetro|Obrigatório/opcional|Significado|
|-|-|-|
|`language`|Necessário|O identificador do idioma a ser reconhecido. Ver [idiomas suportados](language-support.md#speech-to-text).|
|`format`|Opcional<br>predefinição: `simple`|Formato de resultado `simple` ou `detailed`. Os resultados da simples incluem `RecognitionStatus`, `DisplayText`, `Offset`e a duração. Resultados detalhados incluem vários candidatos com valores de confiança e quatro diferentes representações.|
|`profanity`|Opcional<br>predefinição: `masked`|Como lidar com linguagem inapropriada nos resultados de reconhecimento. Pode ser `masked` (substitui linguagem inapropriada por asteriscos), `removed` (Remove todos os profanidades), ou `raw` (inclui a linguagem inapropriada).

### <a name="request-headers"></a>Cabeçalhos do pedido

Os campos seguintes são enviados no cabeçalho do pedido HTTP.

|Cabeçalho|Significado|
|------|-------|
|`Ocp-Apim-Subscription-Key`|A chave de subscrição do serviço de voz. Este cabeçalho de qualquer um dos ou `Authorization` tem de ser fornecido.|
|`Authorization`|Um token de autorização precedidas pela palavra `Bearer`. Este cabeçalho de qualquer um dos ou `Ocp-Apim-Subscription-Key` tem de ser fornecido. Ver [autenticação](#authentication).|
|`Content-type`|Descreve o formato e o codec dos dados de áudio. Atualmente, este valor tem de ser `audio/wav; codec=audio/pcm; samplerate=16000`.|
|`Transfer-Encoding`|Opcional. Se for indicado, tem de ser `chunked` para permitir que os dados de áudio a serem enviados por vários pequenas partes em vez de um único arquivo.|
|`Expect`|Se utilizar a transferência em partes, enviar `Expect: 100-continue`. O serviço de voz reconhece o pedido inicial e awaits dados adicionais.|
|`Accept`|Opcional. Se for fornecido, tem de incluir `application/json`, como o serviço de voz fornece resultados no formato JSON. (Algumas estruturas de solicitação da Web, forneça um valor incompatível predefinido se não especificar um, portanto, é recomendável sempre incluir `Accept`.)|

### <a name="audio-format"></a>Formato de áudio

O áudio é enviado no corpo do HTTP `PUT` pedido. Deve estar no formato WAV de 16 bits com canal único PCM (mono) em 16 KHz da formatos/codificação seguinte.

* Formato WAV com PCM codec
* Formato de OGG com OPUS codec

>[!NOTE]
>Os formatos acima são suportados através da REST API e WebSocket no serviço de voz. O [SDK de voz](/index.yml) atualmente apenas suporta o WAV formatar com o codec PCM. 

### <a name="chunked-transfer"></a>Transferência em partes

A transferência (`Transfer-Encoding: chunked`) pode ajudar a reduzir a latência de reconhecimento, pois permite que o serviço de voz para começar a processar o ficheiro de áudio, enquanto que está a ser transmitido. A API REST não fornece resultados parciais ou provisórias. Esta opção destina-se exclusivamente para melhorar a capacidade de resposta.

O código a seguir ilustra como enviar áudio em blocos. Apenas o primeiro segmento deve conter cabeçalho do arquivo de áudio. `request` está ligado um objeto HTTPWebRequest para o ponto final REST adequado. `audioFile` é o caminho para um arquivo de áudio no disco.

```csharp
using (fs = new FileStream(audioFile, FileMode.Open, FileAccess.Read))
{

    /*
    * Open a request stream and write 1024 byte chunks in the stream one at a time.
    */
    byte[] buffer = null;
    int bytesRead = 0;
    using (Stream requestStream = request.GetRequestStream())
    {
        /*
        * Read 1024 raw bytes from the input audio file.
        */
        buffer = new Byte[checked((uint)Math.Min(1024, (int)fs.Length))];
        while ((bytesRead = fs.Read(buffer, 0, buffer.Length)) != 0)
        {
            requestStream.Write(buffer, 0, bytesRead);
        }

        // Flush
        requestStream.Flush();
    }
}
```

### <a name="example-request"></a>Pedido de exemplo

Segue-se uma solicitação típica.

```HTTP
POST speech/recognition/conversation/cognitiveservices/v1?language=en-US&format=detailed HTTP/1.1
Accept: application/json;text/xml
Content-Type: audio/wav; codec=audio/pcm; samplerate=16000
Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY
Host: westus.stt.speech.microsoft.com
Transfer-Encoding: chunked
Expect: 100-continue
```

### <a name="http-status"></a>Estado de HTTP

O estado HTTP de resposta indica o êxito ou condições de erro comuns.

Código de HTTP|Significado|Razão possível
-|-|-|
100|Continuar|O pedido inicial foi aceite. Continue com a enviar o resto dos dados. (Utilizado com a transferência em partes).
200|OK|O pedido foi concluída com êxito; o corpo da resposta é um objeto JSON.
400|Pedido incorreto|Código de idioma fornecido ou não é um idioma suportado; arquivo de áudio inválido.
401|Não autorizado|Chave de subscrição ou autorização token é inválido na região especificada ou ponto final inválido.
403|Proibido|Chave de subscrição ou autorização em falta token.

### <a name="json-response"></a>Resposta JSON

Os resultados são devolvidos em formato JSON. O `simple` formato inclui apenas os campos de nível superior seguintes.

|Nome do campo|Conteúdo|
|-|-|
|`RecognitionStatus`|Estado, tais como `Success` para reconhecimento bem-sucedido. Consulte a tabela seguinte.|
|`DisplayText`|O texto reconhecido depois de capitalização, pontuação, normalização do texto inversa (conversão de texto falado forms mais curto, por exemplo, 200 para "duzentos" ou "Dr. Smith"para"doutor smith") e a máscara de linguagem inapropriada. Apresentar apenas com êxito.|
|`Offset`|O tempo (em unidades de 100 nanossegundos), no qual a voz reconhecido começa no fluxo de áudio.|
|`Duration`|A duração (em unidades de 100 nanossegundos) do voz reconhecido no fluxo de áudio.|

O `RecognitionStatus` campo pode conter os seguintes valores.

|Valor de estado|Descrição
|-|-|
| `Success` | O reconhecimento foi concluída com êxito e o campo de DisplayText está presente. |
| `NoMatch` | Conversão de voz foi detetada no fluxo de áudio, mas sem palavras do idioma de destino foram correspondidas. Normalmente, significa que o idioma de reconhecimento é um idioma diferente daquele que o utilizador está a falar. |
| `InitialSilenceTimeout` | O início do fluxo de áudio continha apenas silêncio e o serviço excedeu o tempo aguardar por voz. |
| `BabbleTimeout` | O início do fluxo de áudio continha apenas ruído e o serviço excedeu o tempo aguardar por voz. |
| `Error` | O serviço de reconhecimento de obteve um erro interno e não foi possível continuar. Tente novamente se possível. |

> [!NOTE]
> Se o áudio consiste apenas em linguagem inapropriada e o `profanity` parâmetro de consulta está definido como `remove`, o serviço não devolve um resultado de conversão de voz. 


O `detailed` formato inclui os mesmos campos que o `simple` formatar, juntamente com um `NBest` campo. O `NBest` campo é uma lista de alternativas interpretações sobre a mesma conversão de voz, classificados do maior probabilidade de, pelo menos, provavelmente. A primeira entrada é o mesmo, como o resultado do reconhecimento principal. Cada entrada contém os seguintes campos:

|Nome do campo|Conteúdo|
|-|-|
|`Confidence`|A pontuação de confiança da entrada de 0,0 (sem confiança) para 1,0 (confiança total)
|`Lexical`|O formulário léxico do texto reconhecido: as palavras reais reconhecidas.
|`ITN`|O formulário ("canonical") normalizado de texto de inverso do texto reconhecido, com o telefone números, números, abreviaturas ("doutor smith" para "dr smith") e outras transformações aplicadas.
|`MaskedITN`| O formulário ITN com a máscara de linguagem inapropriada aplicada, se solicitado.
|`Display`| O formulário de apresentação do texto reconhecido, com a pontuação e capitalização adicionado. Mesmo que `DisplayText` no resultado de nível superior.

### <a name="sample-responses"></a>Respostas de exemplo

Segue-se uma resposta típica para `simple` reconhecimento.

```json
{
  "RecognitionStatus": "Success",
  "DisplayText": "Remind me to buy 5 pencils.",
  "Offset": "1236645672289",
  "Duration": "1236645672289"
}
```

Segue-se uma resposta típica para `detailed` reconhecimento.

```json
{
  "RecognitionStatus": "Success",
  "Offset": "1236645672289",
  "Duration": "1236645672289",
  "NBest": [
      {
        "Confidence" : "0.87",
        "Lexical" : "remind me to buy five pencils",
        "ITN" : "remind me to buy 5 pencils",
        "MaskedITN" : "remind me to buy 5 pencils",
        "Display" : "Remind me to buy 5 pencils.",
      },
      {
        "Confidence" : "0.54",
        "Lexical" : "rewind me to buy five pencils",
        "ITN" : "rewind me to buy 5 pencils",
        "MaskedITN" : "rewind me to buy 5 pencils",
        "Display" : "Rewind me to buy 5 pencils.",
      }
  ]
}
```

## <a name="text-to-speech"></a>Conversão de Texto em Voz

Seguem-se os pontos de extremidade REST API de voz para o serviço de voz texto. Utilize o ponto final que corresponde à sua região de subscrição.

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-text-to-speech.md)]

O serviço de voz oferece suporte a saída de áudio de 24 KHz, além da saída de 16 Khz suportada por voz do Bing. Quatro formatos de saída de 24 KHz estão disponíveis para utilização no `X-Microsoft-OutputFormat` cabeçalho de HTTP, conforme são dois de 24 KHz vozes `Jessa24kRUS` e `Guy24kRUS`.

Região | Idioma   | Género | Mapeamento de nome de serviço
-------|------------|--------|------------
pt-PT  | Inglês dos Estados Unidos | Feminino | "Microsoft Server voz texto para voz de voz (en-US, Jessa24kRUS)" 
pt-PT  | Inglês dos Estados Unidos | Masculino   | "Microsoft Server voz texto para voz de voz (en-US, Guy24kRUS)"

Uma lista completa de vozes disponíveis está disponível no [idiomas suportados](language-support.md#text-to-speech).

### <a name="request-headers"></a>Cabeçalhos do pedido

Os campos seguintes são enviados no cabeçalho do pedido HTTP.

|Cabeçalho|Significado|
|------|-------|
|`Authorization`|Um token de autorização precedidas pela palavra `Bearer`. Necessário. Ver [autenticação](#authentication).|
|`Content-Type`|O tipo de conteúdo de entrada: `application/ssml+xml`.|
|`X-Microsoft-OutputFormat`|O formato de áudio de saída. Consulte a tabela seguinte.|
|`User-Agent`|Nome da aplicação. Necessário. tem de conter menos de 255 carateres.|

Formatos de saída de áudio disponível (`X-Microsoft-OutputFormat`) incorporar uma taxa de bits e uma codificação.

|||
|-|-|
`raw-16khz-16bit-mono-pcm`         | `raw-8khz-8bit-mono-mulaw`
`riff-8khz-8bit-mono-mulaw`     | `riff-16khz-16bit-mono-pcm`
`audio-16khz-128kbitrate-mono-mp3` | `audio-16khz-64kbitrate-mono-mp3`
`audio-16khz-32kbitrate-mono-mp3`  | `raw-24khz-16bit-mono-pcm`
`riff-24khz-16bit-mono-pcm`        | `audio-24khz-160kbitrate-mono-mp3`
`audio-24khz-96kbitrate-mono-mp3`  | `audio-24khz-48kbitrate-mono-mp3`

> [!NOTE]
> Se sua voz selecionado e o formato de saída tiverem taxas de bits diferentes, o áudio é resampled conforme necessário. No entanto, não suportam vozes de 24khz `audio-16khz-16kbps-mono-siren` e `riff-16khz-16kbps-mono-siren` formatos de saída. 

### <a name="request-body"></a>Corpo do pedido

O texto a converter a voz é enviado como o corpo de um HTTP `POST` pedido em qualquer texto simples (ASCII ou UTF-8) ou [Speech Synthesis Markup Language](speech-synthesis-markup.md) formato de (SSML) (UTF-8). Pedidos de texto simples de utilizar voz predefinido do serviço e de idioma. Envie SSML para utilizar uma voz diferente.

### <a name="sample-request"></a>Pedido de exemplo

O pedido HTTP seguinte utiliza um corpo SSML para escolher a voz. O corpo não pode ter mais de 1000 carateres.

```xml
POST /cognitiveservices/v1 HTTP/1.1

X-Microsoft-OutputFormat: raw-16khz-16bit-mono-pcm
Content-Type: application/ssml+xml
Host: westus.tts.speech.microsoft.com
Content-Length: 225
Authorization: Bearer [Base64 access_token]

<speak version='1.0' xml:lang='en-US'><voice xml:lang='en-US' xml:gender='Female' 
    name='Microsoft Server Speech Text to Speech Voice (en-US, ZiraRUS)'>
        Microsoft Speech Service Text-to-Speech API
</voice></speak>
```

### <a name="http-response"></a>Resposta HTTP

O estado HTTP de resposta indica o êxito ou condições de erro comuns.

Código de HTTP|Significado|Razão possível
-|-|-|
200|OK|O pedido foi concluída com êxito; o corpo da resposta é um arquivo de áudio.
400 |Pedido Inválido |Um parâmetro necessário está em falta, vazios ou nulos. Em alternativa, o valor transmitido como um parâmetro obrigatório ou opcional é inválido. Um problema comum é um cabeçalho que é demasiado longo.
401|Não autorizado |O pedido não está autorizado. Certifique-se a chave de subscrição ou o token é válido e na região correto.
413|Entidade do pedido demasiado grande|A entrada SSML é superior a 1024 carateres.
429|Demasiados Pedidos|Excedeu a quota ou a taxa de pedidos permitidos na sua subscrição.
502|Gateway incorrecto | Problema de rede ou do lado do servidor. Também pode indicar a cabeçalhos inválidos.

Se o estado HTTP é `200 OK`, o corpo da resposta contém um arquivo de áudio no formato solicitado. Este ficheiro pode ser reproduzido à medida que ele tem transferidos ou guardada para um ficheiro para posterior reprodução ou outro uso ou memória intermédia.

## <a name="authentication"></a>Autenticação

Enviar um pedido para a API de REST do serviço de voz requer uma chave de subscrição ou um token de acesso. Em geral, é mais fácil enviar a chave de subscrição diretamente. O serviço de voz, em seguida, obtém o token de acesso para si. Para minimizar o tempo de resposta, pode querer utilizar um token de acesso em vez disso.

Para obter um token, apresentar a chave de subscrição para um serviço de voz regional `issueToken` ponto de extremidade, conforme mostrado na seguinte tabela. Utilize o ponto final que corresponde à sua região de subscrição.

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-token-service.md)]

Cada token de acesso é válido durante 10 minutos. Pode obter um novo token em qualquer altura. Se quiser, pode obter um token apenas antes de cada pedido de API de REST de voz. Para minimizar o tráfego de rede e a latência, recomendamos que utilize o mesmo token para nove minutos.

As secções seguintes mostram como obter um token e como usá-lo num pedido.

### <a name="get-a-token-http"></a>Obter um token: HTTP

O exemplo seguinte é um pedido HTTP de exemplo para obter um token. Substitua `YOUR_SUBSCRIPTION_KEY` com a sua chave de subscrição do serviço de voz. Se a sua subscrição não está na região E.U.A. oeste, substitua o `Host` cabeçalho com o nome de anfitrião de sua região.

```
POST /sts/v1.0/issueToken HTTP/1.1
Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY
Host: westus.api.cognitive.microsoft.com
Content-type: application/x-www-form-urlencoded
Content-Length: 0
```

O corpo da resposta a essa solicitação é o token de acesso no formato de Java Web Token (JWT).

### <a name="get-a-token-powershell"></a>Obter um token: PowerShell

O seguinte script do Windows PowerShell ilustra como obter um token de acesso. Substitua `YOUR_SUBSCRIPTION_KEY` com a sua chave de subscrição do serviço de voz. Se a sua subscrição não está na região E.U.A. oeste, altere o nome de anfitrião do URI determinado em conformidade.

```Powershell
$FetchTokenHeader = @{
  'Content-type'='application/x-www-form-urlencoded';
  'Content-Length'= '0';
  'Ocp-Apim-Subscription-Key' = 'YOUR_SUBSCRIPTION_KEY'
}

$OAuthToken = Invoke-RestMethod -Method POST -Uri https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken
 -Headers $FetchTokenHeader

# show the token received
$OAuthToken

```

### <a name="get-a-token-curl"></a>Obter um token: cURL

cURL é uma ferramenta da linha de comandos disponível no Linux (e no subsistema Windows para Linux). O comando cURL seguinte ilustra como obter um token de acesso. Substitua `YOUR_SUBSCRIPTION_KEY` com a sua chave de subscrição do serviço de voz. Se a sua subscrição não está na região E.U.A. oeste, altere o nome de anfitrião do URI determinado em conformidade.

> [!NOTE]
> O comando é mostrado em várias linhas para facilitar a leitura, mas introduzi-lo numa única linha no prompt do shell.

```
curl -v -X POST 
 "https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken" 
 -H "Content-type: application/x-www-form-urlencoded" 
 -H "Content-Length: 0" 
 -H "Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY"
```

### <a name="get-a-token-c"></a>Obter um token: c#

A seguinte classe c# ilustra como obter um token de acesso. Passe a chave de subscrição do serviço de voz ao instanciar a classe. Se a sua subscrição não está na região E.U.A. oeste, altere o nome de anfitrião do `FetchTokenUri` adequadamente.

```cs
/*
    * This class demonstrates how to get a valid access token.
    */
public class Authentication
{
    public static readonly string FetchTokenUri =
        "https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken";
    private string subscriptionKey;
    private string token;

    public Authentication(string subscriptionKey)
    {
        this.subscriptionKey = subscriptionKey;
        this.token = FetchTokenAsync(FetchTokenUri, subscriptionKey).Result;
    }

    public string GetAccessToken()
    {
        return this.token;
    }

    private async Task<string> FetchTokenAsync(string fetchUri, string subscriptionKey)
    {
        using (var client = new HttpClient())
        {
            client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", subscriptionKey);
            UriBuilder uriBuilder = new UriBuilder(fetchUri);

            var result = await client.PostAsync(uriBuilder.Uri.AbsoluteUri, null);
            Console.WriteLine("Token Uri: {0}", uriBuilder.Uri.AbsoluteUri);
            return await result.Content.ReadAsStringAsync();
        }
    }
}
```

### <a name="use-a-token"></a>Utilizar um token

Para utilizar um token num pedido de REST API, forneça-na `Authorization` cabeçalho, seguindo a palavra `Bearer`. Eis um exemplo de texto para solicitação de REST de voz que contém um token. Substituir o token real para `YOUR_ACCESS_TOKEN`. Utilize o nome de anfitrião correto no `Host` cabeçalho.

```xml
POST /cognitiveservices/v1 HTTP/1.1
Authorization: Bearer YOUR_ACCESS_TOKEN
Host: westus.tts.speech.microsoft.com
Content-type: application/ssml+xml
Content-Length: 199
Connection: Keep-Alive

<speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
<voice name='Microsoft Server Speech Text to Speech Voice (en-US, Jessa24kRUS)'>
    Hello, world!
</voice></speak>
```

### <a name="renew-authorization"></a>Renovar autorização

O token de autorização expira após 10 minutos. Renove a sua autorização, obtendo um novo token antes de expirar. Por exemplo, pode obter um novo token após nove minutos.

O seguinte código c# é uma substituição completa para a classe apresentada anteriormente. O `Authentication` classe obtém automaticamente um novo token de acesso em nove minutos, uso um timer. Esta abordagem garante que um token válido está sempre disponível enquanto seu programa está em execução.

> [!NOTE]
> Em vez de usar um timer, pode armazenar um carimbo de quando o último token foi obtido. Em seguida, pode pedir um novo apenas se for encontra prestes a expirar. Esta abordagem evita solicitar novos tokens desnecessariamente e poderá ser mais adequada para programas que efetuam pedidos de voz raros.

Como antes, certifique-se de que o `FetchTokenUri` valor corresponde à sua região de subscrição. Passe a chave de subscrição ao instanciar a classe.

```cs
/*
    * This class demonstrates how to maintain a valid access token.
    */
public class Authentication
{
    public static readonly string FetchTokenUri = 
        "https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken";
    private string subscriptionKey;
    private string token;
    private Timer accessTokenRenewer;

    //Access token expires every 10 minutes. Renew it every 9 minutes.
    private const int RefreshTokenDuration = 9;

    public Authentication(string subscriptionKey)
    {
        this.subscriptionKey = subscriptionKey;
        this.token = FetchToken(FetchTokenUri, subscriptionKey).Result;

        // renew the token on set duration.
        accessTokenRenewer = new Timer(new TimerCallback(OnTokenExpiredCallback),
                                        this,
                                        TimeSpan.FromMinutes(RefreshTokenDuration),
                                        TimeSpan.FromMilliseconds(-1));
    }

    public string GetAccessToken()
    {
        return this.token;
    }

    private void RenewAccessToken()
    {
        this.token = FetchToken(FetchTokenUri, this.subscriptionKey).Result;
        Console.WriteLine("Renewed token.");
    }

    private void OnTokenExpiredCallback(object stateInfo)
    {
        try
        {
            RenewAccessToken();
        }
        catch (Exception ex)
        {
            Console.WriteLine(string.Format("Failed renewing access token. Details: {0}", ex.Message));
        }
        finally
        {
            try
            {
                accessTokenRenewer.Change(TimeSpan.FromMinutes(RefreshTokenDuration), TimeSpan.FromMilliseconds(-1));
            }
            catch (Exception ex)
            {
                Console.WriteLine(string.Format("Failed to reschedule the timer to renew access token. Details: {0}", ex.Message));
            }
        }
    }

    private async Task<string> FetchToken(string fetchUri, string subscriptionKey)
    {
        using (var client = new HttpClient())
        {
            client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", subscriptionKey);
            UriBuilder uriBuilder = new UriBuilder(fetchUri);

            var result = await client.PostAsync(uriBuilder.Uri.AbsoluteUri, null);
            Console.WriteLine("Token Uri: {0}", uriBuilder.Uri.AbsoluteUri);
            return await result.Content.ReadAsStringAsync();
        }
    }
}
```

## <a name="next-steps"></a>Passos Seguintes

- [Obter a subscrição de avaliação de Voz](https://azure.microsoft.com/try/cognitive-services/)
- [Personalizar modelos acústicos](how-to-customize-acoustic-models.md)
- [Personalizar modelos de idioma](how-to-customize-language-model.md)

