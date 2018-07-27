---
title: REST APIs do serviço de voz
description: Referência para as APIs REST para o serviço de voz.
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.technology: speech
ms.topic: article
ms.date: 05/09/2018
ms.author: v-jerkin
ms.openlocfilehash: 9991e0a1fc54e6aa4a99b8bfbd93abdfe974b01b
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/27/2018
ms.locfileid: "39283074"
---
# <a name="speech-service-rest-apis"></a>REST APIs do serviço de voz

As APIs REST do serviço de voz unificado são semelhantes às APIs fornecidas pelos [API de voz](https://docs.microsoft.com/azure/cognitive-services/Speech) (anteriormente conhecido como o serviço de voz do Bing). Os pontos de extremidade diferem dos pontos de extremidade utilizados pelo serviço de voz anterior.

## <a name="speech-to-text"></a>Conversão de Voz em Texto

Conversão de voz para a API de texto, apenas os pontos finais utilizados são diferentes do serviço de voz anterior API de reconhecimento de voz. Os novos pontos de extremidade são mostrados na tabela abaixo. Utilize um que corresponde à sua região de subscrição.

[!include[](../../../includes/cognitive-services-speech-service-endpoints-speech-to-text.md)]

A conversão de voz em texto API caso contrário, é semelhante para o [REST API](https://docs.microsoft.com/azure/cognitive-services/speech/getstarted/getstartedrest) para a API de voz anterior.

A conversão de voz em texto REST API suporta apenas os discursos curtos. Os pedidos podem conter até 10 segundos de áudio e um máximo de 14 segundos geral da última. A API REST devolve apenas resultados finais, não resultados parciais ou provisórias.

> [!NOTE]
> Se personalizou o modelo acústico ou modelo de idioma ou pronúncia, em vez disso, a utilizar o seu ponto final personalizado.

## <a name="text-to-speech"></a>Conversão de Texto em Voz

O novo texto em voz API suporta a saída de áudio de 24 KHz. O `X-Microsoft-OutputFormat` cabeçalho agora pode conter os seguintes valores.

|||
|-|-|
`raw-16khz-16bit-mono-pcm`         | `audio-16khz-16kbps-mono-siren`
`riff-16khz-16kbps-mono-siren`     | `riff-16khz-16bit-mono-pcm`
`audio-16khz-128kbitrate-mono-mp3` | `audio-16khz-64kbitrate-mono-mp3`
`audio-16khz-32kbitrate-mono-mp3`  | `raw-24khz-16bit-mono-pcm`
`riff-24khz-16bit-mono-pcm`        | `audio-24khz-160kbitrate-mono-mp3`
`audio-24khz-96kbitrate-mono-mp3`  | `audio-24khz-48kbitrate-mono-mp3`

O serviço de voz fornece agora dois 24 KHz vozes:

Região | Idioma   | Género | Mapeamento de nome de serviço
-------|------------|--------|------------
pt-PT  | Inglês dos Estados Unidos | Feminino | "Microsoft Server voz texto para voz de voz (en-US, Jessa24kRUS)" 
pt-PT  | Inglês dos Estados Unidos | Masculino   | "Microsoft Server voz texto para voz de voz (en-US, Guy24kRUS)"

Seguem-se os pontos de extremidade REST para o unificada serviço texto em voz API. Utilize o ponto final que corresponde à sua região de subscrição.

[!include[](../../../includes/cognitive-services-speech-service-endpoints-text-to-speech.md)]

Manter essas diferenças em mente que consulte a [documentação da REST API](https://docs.microsoft.com/azure/cognitive-services/speech/api-reference-rest/bingvoiceoutput) para a API de voz anterior.

## <a name="authentication"></a>Autenticação

Enviar um pedido para a API de REST do serviço de voz exige um token de acesso. Obter um token ao fornecer a chave de subscrição para um serviço de voz regional `issueToken` ponto final, mostrada na tabela abaixo. Utilize o ponto final que corresponde à sua região de subscrição.

[!include[](../../../includes/cognitive-services-speech-service-endpoints-token-service.md)]

Cada token de acesso é válido durante 10 minutos. Para obter um novo token em qualquer altura, se desejar, incluindo, imediatamente antes de cada pedido de API de REST de voz. Para minimizar o tráfego de rede e a latência, no entanto, recomendamos que utilize o mesmo token para nove minutos.

As secções seguintes mostram como obter um token e como usá-lo num pedido.

### <a name="getting-a-token-http"></a>Obter um token: HTTP

Segue-se um pedido HTTP de exemplo para obter um token. Substitua `YOUR_SUBSCRIPTION_KEY` com a sua chave de subscrição do serviço de voz. Se a sua subscrição não está na região E.U.A. oeste, substitua o `Host` cabeçalho com o nome de anfitrião de sua região.

```
POST /sts/v1.0/issueToken HTTP/1.1
Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY
Host: westus.api.cognitive.microsoft.com
Content-type: application/x-www-form-urlencoded
Content-Length: 0
```

O corpo da resposta a essa solicitação é o token de acesso no formato de Java Web Token (JWT).

### <a name="getting-a-token-powershell"></a>Obter um token: PowerShell

O script do Windows PowerShell abaixo ilustra como obter um token de acesso. Substitua `YOUR_SUBSCRIPTION_KEY` com a sua chave de subscrição do serviço de voz. Se a sua subscrição não está na região E.U.A. oeste, altere o nome de anfitrião do URI determinado em conformidade.

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

### <a name="getting-a-token-curl"></a>Obter um token: cURL

cURL é uma ferramenta da linha de comandos disponível no Linux (e no subsistema Windows para Linux). O comando cURL abaixo ilustra como obter um token de acesso. Substitua `YOUR_SUBSCRIPTION_KEY` com a sua chave de subscrição do serviço de voz. Se a sua subscrição não está na região E.U.A. oeste, altere o nome de anfitrião do URI determinado em conformidade.

> [!NOTE]
> O comando é mostrado em várias linhas para facilitar a leitura, mas deve ser inserido numa única linha no prompt do shell.

```
curl -v -X POST 
 "https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken" 
 -H "Content-type: application/x-www-form-urlencoded" 
 -H "Content-Length: 0" 
 -H "Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY"
```

### <a name="getting-a-token-c"></a>Obter um token: c#

A classe c# abaixo ilustra como obter um token de acesso. Passe a chave de subscrição do serviço de voz ao instanciar a classe. Se a sua subscrição não está na região E.U.A. oeste, altere o nome de anfitrião de `FetchTokenUri` adequadamente.

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

### <a name="using-a-token"></a>Através de um token

Para utilizar um token num pedido de REST API, forneça-na `Authorization` cabeçalho, seguindo a palavra `Bearer`. Aqui, por exemplo, é um exemplo de texto para solicitação de REST de voz que contém um token. Substituir o token real para `YOUR_ACCESS_TOKEN` e utilize o nome de anfitrião correto no `Host` cabeçalho.

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

### <a name="renewing-authorization"></a>Renovar autorização

O token de autorização expira após 10 minutos. Renovar a sua autorização, obtendo um novo token antes de expirar — por exemplo, depois de nove minutos. 

O seguinte código c# é uma substituição completa para a classe apresentada anteriormente. O `Authentication` classe obtém automaticamente um novo token de acesso em nove minutos através de um temporizador. Esta abordagem garante que um token válido está sempre disponível enquanto seu programa está em execução.

> [!NOTE]
> Em vez de usar um timer, pode armazenar um carimbo de quando o token atual foi obtido, em seguida, solicitar um novo, apenas se o token atual for encontra prestes a expirar. Esta abordagem evita solicitar novos tokens desnecessariamente e pode ser mais adequada para programas que efetuam pedidos de voz raros.

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
- [Personalizar modelos de linguagem](how-to-customize-language-model.md)

