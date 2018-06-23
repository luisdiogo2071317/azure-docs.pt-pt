---
title: Serviços cognitivos voz SDK de resolução de problemas | Microsoft Docs
description: Bilhete do problema Shooting cognitivos dos serviços de voz SDK
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: wolfma61
manager: onano
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: wolfma
ms.openlocfilehash: 16eaebcf9494ab57521068a9418ccf2ac7f5a8fe
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354548"
---
# <a name="troubleshooting-speech-services-sdk"></a>Resolução de problemas de reconhecimento de voz serviços SDK

Este artigo fornece informações para ajudar a resolver problemas que poderá encontrar ao utilizar o SDK de reconhecimento de voz.

## <a name="error-websocket-upgrade-failed-with-an-authentication-error-403"></a>Erro `WebSocket Upgrade failed with an authentication error (403).`

Pode ter o ponto final incorreto para a região ou serviço. Verifique o URI para se certificar de que está correto. Consulte também a secção seguinte, conforme Isto também pode ser um problema com a sua chave de subscrição ou de autorização token.

## <a name="error-http-403-forbidden-or-error-http-401-unauthorized"></a>Erro `HTTP 403 Forbidden` ou erro `HTTP 401 Unauthorized`

Este erro é frequentemente causado por problemas de autenticação. Pedidos de ligação sem um `Ocp-Apim-Subscription-Key` ou `Authorization` cabeçalho são rejeitados com estado 401 ou 403.

* Se estiver a utilizar uma chave de subscrição para a autenticação, a causa pode ser:

    - a chave de subscrição é inválida ou em falta
    - excedeu a quota de utilização da sua subscrição

* Se estiver a utilizar um token de autorização para autenticação, a causa pode ser:

    - o token de autorização é inválido
    - o token de autorização expirou

### <a name="validate-your-subscription-key"></a>Validar a sua chave de subscrição

Pode verificar para se certificar de que tem uma chave de uma subscrição válida, executando um dos comandos abaixo.

> [!NOTE]
> Substitua `YOUR_SUBSCRIPTION_KEY` e `YOUR_REGION` com a sua própria chave de subscrição e região associado, respetivamente.

* PowerShell

    ```Powershell
    $FetchTokenHeader = @{
      'Content-type'='application/x-www-form-urlencoded'
      'Content-Length'= '0'
      'Ocp-Apim-Subscription-Key' = 'YOUR_SUBSCRIPTION_KEY'
    }
    $OAuthToken = Invoke-RestMethod -Method POST -Uri https://YOUR_REGION.api.cognitive.microsoft.com/sts/v1.0/issueToken -Headers $FetchTokenHeader
    $OAuthToken
    ```

* cURL

    ```
    curl -v -X POST "https://YOUR_REGION.api.cognitive.microsoft.com/sts/v1.0/issueToken" -H "Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY" -H "Content-type: application/x-www-form-urlencoded" -H "Content-Length: 0"
    ```

### <a name="validate-an-authorization-token"></a>Validar um token de autorização

Se utilizar um token de autorização para autenticação, execute um dos seguintes comandos para verificar que o token de autorização ainda é válido. Tokens são válidos durante 10 minutos.

> [!NOTE]
> Substitua `YOUR_AUDIO_FILE` com o caminho para o ficheiro de áudio prerecorded, `YOUR_ACCESS_TOKEN` com o token de autorização devolvido no passo anterior, e `YOUR_REGION` com a região correta.

* PowerShell

    ```Powershell
    $SpeechServiceURI =
    'https://YOUR_REGION.stt.speech.microsoft.com/speech/recognition/interactive/cognitiveservices/v1?language=en-US'
    
    # $OAuthToken is the authorization token returned by the token service.
    $RecoRequestHeader = @{
      'Authorization' = 'Bearer '+ $OAuthToken
      'Transfer-Encoding' = 'chunked'
      'Content-type' = 'audio/wav; codec=audio/pcm; samplerate=16000'
    }
    
    # Read audio into byte array
    $audioBytes = [System.IO.File]::ReadAllBytes("YOUR_AUDIO_FILE")
    
    $RecoResponse = Invoke-RestMethod -Method POST -Uri $SpeechServiceURI -Headers $RecoRequestHeader -Body $audioBytes
    
    # Show the result
    $RecoResponse
    ```

* cURL

    ```
    curl -v -X POST "https://YOUR_REGION.stt.speech.microsoft.com/speech/recognition/interactive/cognitiveservices/v1?language=en-US" -H "Authorization: Bearer YOUR_ACCESS_TOKEN" -H "Transfer-Encoding: chunked" -H "Content-type: audio/wav; codec=audio/pcm; samplerate=16000" --data-binary @YOUR_AUDIO_FILE
    ```

---

## <a name="error-http-400-bad-request"></a>Erro `HTTP 400 Bad Request`

Normalmente, este erro ocorre quando o corpo do pedido contém dados de áudio inválido. Apenas `WAV` formato é suportado. Consulte também os cabeçalhos do pedido para se certificar de que especifica um adequado `Content-Type` e `Content-Length`.

## <a name="error-http-408-request-timeout"></a>Erro `HTTP 408 Request Timeout`

O erro é mais provável porque não existem dados de áudio estão a ser enviados para o serviço. Este erro também pode ser causado por problemas de rede.

## <a name="the-recognitionstatus-in-the-response-is-initialsilencetimeout"></a>O `RecognitionStatus` na resposta é `InitialSilenceTimeout`

Dados de áudio, normalmente, são o motivo pelo qual a causar o problema. Por exemplo:

* Há uma stretch longo de silêncio no início do áudio. O serviço irá parar o reconhecimento após alguns segundos e devolver `InitialSilenceTimeout`.
* Áudio utiliza um formato de codec não suportado, o que faz com que os dados de áudio deve ser tratado como silêncio.

## <a name="next-steps"></a>Passos Seguintes

* [Notas de versão](releasenotes.md)

