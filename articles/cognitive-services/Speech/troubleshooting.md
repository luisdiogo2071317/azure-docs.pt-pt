---
title: Resolução de problemas | Microsoft Docs
description: Como resolver problemas ao utilizar o serviço de reconhecimento de voz de Microsoft.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.component: bing-speech
ms.topic: article
ms.date: 09/15/2017
ms.author: zhouwang
ms.openlocfilehash: 04f3da19939d523d201d357b2b0293db1508431d
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352184"
---
# <a name="troubleshooting"></a>Resolução de problemas

## <a name="error-http-403-forbidden"></a>Erro `HTTP 403 Forbidden`

Quando utilizar a API de reconhecimento de voz, devolve uma `HTTP 403 Forbidden` erro.

### <a name="cause"></a>Causa

Este erro é frequentemente causado por problemas de autenticação. Os pedidos de ligação sem válido `Ocp-Apim-Subscription-Key` ou `Authorization` cabeçalho são rejeitados pelo serviço com um `HTTP 403 Forbidden` resposta.

Se estiver a utilizar a chave de subscrição para a autenticação, a razão pode ser

- a chave de subscrição é inválida ou em falta
- a quota de utilização da chave de subscrição for excedida
- o `Ocp-Apim-Subscription-Key` campo não está definido no cabeçalho do pedido, quando é chamada REST API

Se estiver a utilizar o token de autorização para autenticação, as razões seguintes pode fazer com que o erro.

- o `Authorization` cabeçalho está em falta no pedido quando utilizar REST
- o token de autorização especificado no cabeçalho de autorização é inválido
- o token de autorização expirou. O token de acesso tem uma expiração de 10 minutos

Para obter mais informações sobre a autenticação, consulte o [autenticação](How-to/how-to-authentication.md) página.

### <a name="troubleshooting-steps"></a>Passos de resolução de problemas

#### <a name="verify-that-your-subscription-key-is-valid"></a>Certifique-se de que a chave de subscrição válida

Pode executar o seguinte comando para verificação. Tenha em atenção para substituir *YOUR_SUBSCRIPTION_KEY* com a sua própria chave de subscrição. Se a chave de subscrição é válida, receberá na resposta o token de autorização como um Token Web JSON (JWT). Caso contrário, receberá um erro na resposta.

> [!NOTE]
> Substitua `YOUR_SUBSCRIPTION_KEY` com a sua própria chave de subscrição.

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

O exemplo utiliza o curl no Linux com bash. Se não estiver disponível na sua plataforma, poderá ter de instalar o curl. O exemplo deve também funcionar no Cygwin no Windows, Git Bash, zsh e outros shells.

```
curl -v -X POST "https://api.cognitive.microsoft.com/sts/v1.0/issueToken" -H "Content-type: application/x-www-form-urlencoded" -H "Content-Length: 0" -H "Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY"
```
---

Certifique-se de que utilizam a mesma chave de subscrição na sua aplicação ou no pedido REST, que é utilizado acima.

#### <a name="verify-the-authorization-token"></a>Verifique se o token de autorização

Este passo só é necessário se utilizar o token de autorização para autenticação. Execute o seguinte comando para verificar que o token de autorização ainda é válido. O comando faz um pedido POST para o serviço e espera que uma mensagem de resposta do serviço. Se continuar a receber HTTP `403 Forbidden` erro, verifique o acesso de token está corretamente definida e não expirou.

> [!IMPORTANT]
> O token tem uma expiração de 10 minutos.
> [!NOTE]
> Substitua `YOUR_AUDIO_FILE` com o caminho para o ficheiro de áudio prerecorded, e `YOUR_ACCESS_TOKEN` com o token de autorização devolvida no passo anterior.

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

---

## <a name="error-http-400-bad-request"></a>Erro `HTTP 400 Bad Request`

Esta razão é que o corpo do pedido contém dados de áudio inválido. Atualmente só suportamos ficheiro WAV.

## <a name="error-http-408-request-timeout"></a>Erro `HTTP 408 Request Timeout`

O erro é muito provável que porque que não áudio são enviados dados para o serviço e o serviço devolve este erro depois de tempo limite. Para a REST API, os dados de áudio devem ser colocados no corpo do pedido.

## <a name="the-recognitionstatus-in-the-response-is-initialsilencetimeout"></a>O `RecognitionStatus` na resposta é `InitialSilenceTimeout`

Dados de áudio, normalmente, são o motivo pelo qual a causar o problema. Por exemplo,

- áudio tem um período de tempo muito silêncio no início. O serviço irá parar o reconhecimento após alguns número de segundos e devolve `InitialSilenceTimeout`.
- áudio utiliza o formato de codec não suportado, o que faz com que os dados de áudio ser tratado como silêncio.
