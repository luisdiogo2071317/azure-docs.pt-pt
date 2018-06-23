---
title: Autenticar para o serviço de reconhecimento de voz Microsoft | Microsoft Docs
description: Pedido de autenticação a utilizar a API de reconhecimento de voz da Microsoft
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.component: bing-speech
ms.topic: article
ms.date: 09/15/2017
ms.author: zhouwang
ms.openlocfilehash: e36168cf3ff938af44f1028c2d26fd475d60b148
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352208"
---
# <a name="authenticate-to-the-speech-api"></a>Autenticar para o API de reconhecimento de voz

Serviço de reconhecimento de voz suporta a autenticação utilizando:

- Uma chave de subscrição.
- Um token de autorização.

## <a name="use-a-subscription-key"></a>Utilizar uma chave de subscrição

Para utilizar o serviço de reconhecimento de voz, tem primeiro de subscrever a API de reconhecimento de voz que faz parte de serviços cognitivos (anteriormente Oxford de projeto). Pode obter as chaves de subscrição de avaliação gratuita do [subscrição de serviços cognitivos](https://azure.microsoft.com/try/cognitive-services/) página. Depois de selecionar a API de reconhecimento de voz, selecione **obter chave de API** para obter a chave. Devolve uma chave primária e secundária. Ambas as chaves estão associadas a quota mesmo, pelo que pode utilizar a chave.

Para utilizar longa duração ou um aumento de quota, inscreva-se um [conta do Azure](https://azure.microsoft.com/free/).

Para utilizar a API de REST de reconhecimento de voz, tem de passar a chave de subscrição no `Ocp-Apim-Subscription-Key` campo no cabeçalho do pedido.

Nome| Formato| Descrição
----|-------|------------
OCP Apim-subscrição-chave | ASCII | YOUR_SUBSCRIPTION_KEY

Segue-se um exemplo de um cabeçalho de pedido:

```HTTP
POST https://speech.platform.bing.com/speech/recognition/interactive/cognitiveservices/v1?language=en-US&format=detailed HTTP/1.1
Accept: application/json;text/xml
Content-Type: audio/wav; codec=audio/pcm; samplerate=16000
Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY
Host: speech.platform.bing.com
Transfer-Encoding: chunked
Expect: 100-continue
```

> [!IMPORTANT]
> Se utilizar [bibliotecas de cliente](../GetStarted/GetStartedClientLibraries.md) na sua aplicação, certifique-se de que pode obter o token de autorização com a sua chave de subscrição, conforme descrito na secção seguinte. As bibliotecas de cliente utilizam a chave de subscrição para obter um token de autorização e, em seguida, utilizar o token de autenticação.

## <a name="use-an-authorization-token"></a>Utilizar um token de autorização

Em alternativa, pode utilizar um token de autorização para a autenticação como prova de autenticação/autorização. Para obter tokens, primeiro tem de obter uma chave de subscrição a partir da API de reconhecimento de voz, conforme descrito no [anterior a secção](#use-a-subscription-key).

### <a name="get-an-authorization-token"></a>Obter um token de autorização

Depois de ter uma chave de uma subscrição válida, envie um pedido POST para o serviço de token de serviços cognitivos. Na resposta, recebe o token de autorização como um Token Web JSON (JWT).

> [!NOTE]
> O token tem uma expiração de 10 minutos. Para renovar o token, consulte a secção seguinte.

O URI do serviço de token está localizado aqui:

```
https://api.cognitive.microsoft.com/sts/v1.0/issueToken
```

O exemplo de código seguinte mostra como obter um token de acesso. Substitua `YOUR_SUBSCRIPTION_KEY` com a sua própria chave de subscrição:

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/Powershell)

```Powershell
$FetchTokenHeader = @{
  'Content-type'='application/x-www-form-urlencoded';
  'Content-Length'= '0';
  'Ocp-Apim-Subscription-Key' = 'YOUR_SUBSCRIPTION_KEY'
}

$OAuthToken = Invoke-RestMethod -Method POST -Uri https://api.cognitive.microsoft.com/sts/v1.0/issueToken -Headers $FetchTokenHeader

# show the token received
$OAuthToken

```

# <a name="curltabcurl"></a>[curl](#tab/curl)

O exemplo utiliza o curl no Linux com bash. Se não estiver disponível na sua plataforma, poderá ter de instalar o curl. O exemplo também funciona em Cygwin no Windows, Git Bash, zsh e outros shells.

```
curl -v -X POST "https://api.cognitive.microsoft.com/sts/v1.0/issueToken" -H "Content-type: application/x-www-form-urlencoded" -H "Content-Length: 0" -H "Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY"
```

# <a name="ctabcsharp"></a>[C#](#tab/CSharp)

```cs
    /*
     * This class demonstrates how to get a valid access token.
     */
    public class Authentication
    {
        public static readonly string FetchTokenUri = "https://api.cognitive.microsoft.com/sts/v1.0";
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
                uriBuilder.Path += "/issueToken";

                var result = await client.PostAsync(uriBuilder.Uri.AbsoluteUri, null);
                Console.WriteLine("Token Uri: {0}", uriBuilder.Uri.AbsoluteUri);
                return await result.Content.ReadAsStringAsync();
            }
        }
    }
```

---

Segue-se um exemplo de pedido POST:

```HTTP
POST https://api.cognitive.microsoft.com/sts/v1.0/issueToken HTTP/1.1
Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY
Host: api.cognitive.microsoft.com
Content-type: application/x-www-form-urlencoded
Content-Length: 0
Connection: Keep-Alive
```

Se não é possível obter uma autorização token do serviço de token, verifique se a chave de subscrição ainda é válida. Se estiver a utilizar uma chave de avaliação gratuita, vá para o [subscrição de serviços cognitivos](https://azure.microsoft.com/try/cognitive-services/) página, clique em "Início de sessão" para iniciar sessão utilizando a conta que utilizou para aplicar a chave de avaliação gratuita e verifique se a chave de subscrição expirou ou excede o quota.

### <a name="use-an-authorization-token-in-a-request"></a>Utilizar um token de autorização num pedido

Sempre que chamar a API de reconhecimento de voz, tem de passar o token de autorização no `Authorization` cabeçalho. O `Authorization` cabeçalho tem de conter um token de acesso JWT.

O exemplo seguinte mostra como utilizar um token de autorização ao chamar a API de REST de reconhecimento de voz.

> [!NOTE]
> Substitua `YOUR_AUDIO_FILE` com o caminho para o ficheiro de áudio prerecorded. Substitua `YOUR_ACCESS_TOKEN` com o token de autorização obteve no passo anterior [obter um token de autorização](#get-an-authorization-token).

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/Powershell)

```Powershell

$SpeechServiceURI =
'https://speech.platform.bing.com/speech/recognition/interactive/cognitiveservices/v1?language=en-us&format=detailed'

# $OAuthToken is the authrization token returned by the token service.
$RecoRequestHeader = @{
  'Authorization' = 'Bearer '+ $OAuthToken;
  'Transfer-Encoding' = 'chunked'
  'Content-type' = 'audio/wav; codec=audio/pcm; samplerate=16000'
}

# Read audio into byte array
$audioBytes = [System.IO.File]::ReadAllBytes("YOUR_AUDIO_FILE")

$RecoResponse = Invoke-RestMethod -Method POST -Uri $SpeechServiceURI -Headers $RecoRequestHeader -Body $audioBytes

# Show the result
$RecoResponse

```

# <a name="curltabcurl"></a>[curl](#tab/curl)

```
curl -v -X POST "https://speech.platform.bing.com/speech/recognition/interactive/cognitiveservices/v1?language=en-us&format=detailed" -H "Transfer-Encoding: chunked" -H "Authorization: Bearer YOUR_ACCESS_TOKEN" -H "Content-type: audio/wav; codec=audio/pcm; samplerate=16000" --data-binary @YOUR_AUDIO_FILE
```

# <a name="ctabcsharp"></a>[C#](#tab/CSharp)

```cs
HttpWebRequest request = null;
request = (HttpWebRequest)HttpWebRequest.Create(requestUri);
request.SendChunked = true;
request.Accept = @"application/json;text/xml";
request.Method = "POST";
request.ProtocolVersion = HttpVersion.Version11;
request.Host = @"speech.platform.bing.com";
request.ContentType = @"audio/wav; codec=audio/pcm; samplerate=16000";
request.Headers["Authorization"] = "Bearer " + token;

// Send an audio file by 1024 byte chunks
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

---

### <a name="renew-an-authorization-token"></a>Renovar um token de autorização

O token de autorização expira após um determinado período de tempo (atualmente 10 minutos). Terá de renovar o token de autorização antes de expirar.

O seguinte código é uma implementação de exemplo em c# do como renovar o token de autorização:

```cs
    /*
     * This class demonstrates how to get a valid O-auth token.
     */
    public class Authentication
    {
        public static readonly string FetchTokenUri = "https://api.cognitive.microsoft.com/sts/v1.0";
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
                uriBuilder.Path += "/issueToken";

                var result = await client.PostAsync(uriBuilder.Uri.AbsoluteUri, null);
                Console.WriteLine("Token Uri: {0}", uriBuilder.Uri.AbsoluteUri);
                return await result.Content.ReadAsStringAsync();
            }
        }
    }
```
