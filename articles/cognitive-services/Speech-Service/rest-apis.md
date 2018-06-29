---
title: O serviço de reconhecimento de voz REST APIs | Microsoft Docs
description: Referência de REST APIs para o serviço de reconhecimento de voz.
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.technology: speech
ms.topic: article
ms.date: 05/09/2018
ms.author: v-jerkin
ms.openlocfilehash: a25c2b7ea7fdfcc6bcaa10baff3a5ae14ae9753b
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37044825"
---
# <a name="speech-service-rest-apis"></a>Serviço de reconhecimento de voz REST APIs

As APIs REST do serviço de reconhecimento de voz unificado são semelhantes às APIs fornecidas pelo [API de reconhecimento de voz](https://docs.microsoft.com/azure/cognitive-services/Speech) (anteriormente conhecido como o serviço de reconhecimento de voz do Bing). Os pontos finais diferem dos pontos finais utilizados pelo serviço de reconhecimento de voz anterior.

## <a name="speech-to-text"></a>Conversão de Voz em Texto

No reconhecimento de voz a API de texto, apenas os pontos finais utilizados diferirem do serviço de reconhecimento de voz anterior API de reconhecimento de voz. Os pontos finais novos são apresentados na tabela abaixo. Utilize um que corresponda à sua região de subscrição.

[!include[](includes/endpoints-speech-to-text.md)]

Caso contrário, é semelhante de reconhecimento de voz a API de texto a [REST API](https://docs.microsoft.com/azure/cognitive-services/speech/getstarted/getstartedrest) para a API de reconhecimento de voz anterior.

O reconhecimento de voz para a API de REST de texto suporta apenas utterances curtos. Pedidos podem conter até 10 segundos de áudio e um máximo de segundos 14 gerais da última. A API REST apenas devolve resultados finais, parciais ou provisórias resultados.

> [!NOTE]
> Se tiver personalizado o modelo acústica ou modelo de linguagem ou pronunciação, utilize o ponto final personalizado em vez disso.

## <a name="text-to-speech"></a>Conversão de Texto em Voz

O novo texto para reconhecimento de voz API suporta a saída de áudio 24 KHz. O `X-Microsoft-OutputFormat` cabeçalho agora pode conter os seguintes valores.

|||
|-|-|
`raw-16khz-16bit-mono-pcm`         | `audio-16khz-16kbps-mono-siren`
`riff-16khz-16kbps-mono-siren`     | `riff-16khz-16bit-mono-pcm`
`audio-16khz-128kbitrate-mono-mp3` | `audio-16khz-64kbitrate-mono-mp3`
`audio-16khz-32kbitrate-mono-mp3`  | `raw-24khz-16bit-mono-pcm`
`riff-24khz-16bit-mono-pcm`        | `audio-24khz-160kbitrate-mono-mp3`
`audio-24khz-96kbitrate-mono-mp3`  | `audio-24khz-48kbitrate-mono-mp3`

O serviço de reconhecimento de voz fornece agora dois 24 KHz voices:

Região | Idioma   | Género | Mapeamento de nome de serviço
-------|------------|--------|------------
pt-PT  | Inglês dos Estados Unidos | Feminino | "Microsoft Server voz texto voz de reconhecimento de voz (en-US, Jessa24kRUS)" 
pt-PT  | Inglês dos Estados Unidos | Masculino   | "Microsoft Server voz texto voz de reconhecimento de voz (en-US, Guy24kRUS)"

Seguem-se a pontos finais REST para o unificada serviço texto em voz API. Utilize o ponto final que corresponda à sua região de subscrição.

[!include[](includes/endpoints-text-to-speech.md)]

Manter estas diferenças em mente como tem de referenciar o [documentação da REST API](https://docs.microsoft.com/azure/cognitive-services/speech/api-reference-rest/bingvoiceoutput) para a API de reconhecimento de voz anterior.

## <a name="authentication"></a>Autenticação

Enviar um pedido para a API de REST do serviço de reconhecimento de voz necessita de um token de acesso. Obter um token ao fornecer a chave de subscrição a um serviço de reconhecimento de voz regional `issueToken` ponto final, mostrado na tabela abaixo. Utilize o ponto final que corresponda à sua região de subscrição.

[!include[](includes/endpoints-token-service.md)]

Cada token de acesso é válido durante 10 minutos. Pode obter um novo token em qualquer altura — incluindo, se assim o desejar, apenas antes de cada pedido de API de REST de reconhecimento de voz. Para minimizar o tráfego de rede e a latência, no entanto, recomendamos que utilize o mesmo token para nove minutos.

As secções seguintes mostram como obter um token e como utilizá-la num pedido.

### <a name="getting-a-token-http"></a>Obter um token: HTTP

Segue-se um exemplo de pedido HTTP para obter um token. Substitua `YOUR_SUBSCRIPTION_KEY` com a sua chave de subscrição do serviço de reconhecimento de voz. Se a sua subscrição não está a ser a região EUA oeste, substitua o `Host` cabeçalho com o nome de anfitrião da sua região.

```
POST /sts/v1.0/issueToken HTTP/1.1
Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY
Host: westus.api.cognitive.microsoft.com
Content-type: application/x-www-form-urlencoded
Content-Length: 0
```

O corpo da resposta a este pedido é o token de acesso no formato de Java Web tokens (JWT).

### <a name="getting-a-token-powershell"></a>Obter um token: PowerShell

O script do Windows PowerShell abaixo ilustra como obter um token de acesso. Substitua `YOUR_SUBSCRIPTION_KEY` com a sua chave de subscrição do serviço de reconhecimento de voz. Se a sua subscrição não está a ser a região EUA oeste, altere o nome de anfitrião do URI fornecido em conformidade.

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

cURL é uma ferramenta da linha de comandos disponível no Linux (e no subsistema Windows para Linux). O comando cURL abaixo ilustra como obter um token de acesso. Substitua `YOUR_SUBSCRIPTION_KEY` com a sua chave de subscrição do serviço de reconhecimento de voz. Se a sua subscrição não está a ser a região EUA oeste, altere o nome de anfitrião do URI fornecido em conformidade.

> [!NOTE]
> O comando é apresentado em várias linhas para legibilidade, mas deve ser introduzido numa única linha numa linha de shell.

```
curl -v -X POST 
 "https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken" 
 -H "Content-type: application/x-www-form-urlencoded" 
 -H "Content-Length: 0" 
 -H "Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY"
```

### <a name="getting-a-token-c"></a>Obter um token: c#

O c# classe abaixo ilustra como obter um token de acesso. Transferir a chave de subscrição do serviço de reconhecimento de voz quando instanciar a classe. Se a sua subscrição não está a ser a região EUA oeste, altere o nome do anfitrião de `FetchTokenUri` adequadamente.

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

### <a name="using-a-token"></a>Utilizando um token

Para utilizar um token num pedido de REST API, forneça-na `Authorization` cabeçalho, seguindo o word `Bearer`. Aqui, por exemplo, é um exemplo de texto que contém um token de pedido de REST de reconhecimento de voz. Substitua o token real para `YOUR_ACCESS_TOKEN` e utilizar o nome de anfitrião correto no `Host` cabeçalho.

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

### <a name="renewing-authorization"></a>A renovação de autorização

O token de autorização expira após 10 minutos. Renovar a sua autorização ao obter um novo token antes de expirar — por exemplo, após nove minutos. 

O seguinte código c# é uma substituição de drop-in para a classe apresentada anteriormente. O `Authentication` classe obtém automaticamente um novo token de acesso a cada minutos nove utilizando um temporizador. Esta abordagem garante que um token válido está sempre disponível enquanto o programa está em execução.

> [!NOTE]
> Em vez de utilizar um temporizador, pode armazenar um carimbo de quando o token atual foi obtido, em seguida, pedir um novo apenas se o token de atual é encontra prestes a expirar. Esta abordagem evita solicitar novos tokens desnecessariamente e pode ser mais adequada para programas que efetuam pedidos de reconhecimento de voz pouco frequentes.

Como anteriormente, certifique-se de que o `FetchTokenUri` valor corresponde à sua região de subscrição. Transferir a chave de subscrição ao instanciar a classe.

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

* [Obter a sua subscrição de avaliação de reconhecimento de voz](https://azure.microsoft.com/try/cognitive-services/)
* [Veja como personalizar um modelo de reconhecimento de voz](how-to-customize-speech-models.md)
