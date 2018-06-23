---
title: Utilizar um ponto final de reconhecimento de voz personalizadas com o serviço de reconhecimento de voz personalizadas no Azure | Microsoft Docs
description: Saiba como utilizar um ponto final de reconhecimento de voz para texto personalizado com o serviço de reconhecimento de voz personalizadas nos serviços cognitivos.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 02/08/2017
ms.author: panosper
ms.openlocfilehash: d28065d7962ee660cafd4b3321abdd6a8f94abcb
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351812"
---
# <a name="use-a-custom-speech-to-text-endpoint"></a>Utilizar um ponto final de reconhecimento de voz para texto personalizado
Pode enviar pedidos para um ponto de final de reconhecimento de voz para texto do serviço de reconhecimento de voz personalizadas do Azure, de forma semelhante, poderá utilizar para o ponto de final de reconhecimento de voz de serviços cognitivos predefinido. Estes pontos finais são funcionalmente idênticos para os pontos finais predefinidos da API de reconhecimento de voz. Assim, a mesma funcionalidade que está disponível através da biblioteca de clientes ou a API REST para a API de reconhecimento de voz também está disponível para o ponto final personalizado.

Os pontos finais que cria ao utilizar este serviço podem processar números diferentes de pedidos em simultâneo. O volume depende o escalão de preço associado à subscrição. Se forem recebidos demasiados pedidos, ocorre um erro. O escalão gratuito tem um limite mensal de pedidos.

O serviço de parte do princípio de que os dados são transmitidos em tempo real. Se são enviada de forma mais rápida, o pedido é considerado em execução até que a sua duração áudio em tempo real foi efectuada com êxito.

> [!NOTE]
> Suportamos o [os sockets web novo](https://docs.microsoft.com/azure/cognitive-services/speech/api-reference-rest/websocketprotocol) ainda. Se planeia utilizar web sockets com o ponto final de reconhecimento de voz personalizadas, siga as instruções aqui.
>
> A nova [REST API](https://docs.microsoft.com/azure/cognitive-services/speech/getstarted/getstartedrest) suporte está disponível em breve. Se planear chamar o ponto final de reconhecimento de voz personalizadas através de HTTP, siga as instruções aqui.
>

## <a name="send-requests-by-using-the-speech-client-library"></a>Enviar pedidos utilizando a biblioteca de clientes de reconhecimento de voz

Para enviar pedidos para o ponto final personalizado utilizando a biblioteca de clientes de reconhecimento de voz, inicie o cliente de reconhecimento. Utilize o reconhecimento de voz do cliente SDK do [NuGet](http://nuget.org/). Procurar *reconhecimento de voz*e selecione o pacote de reconhecimento de voz da Microsoft para a sua plataforma. Algum código de exemplo pode ser encontrado no [GitHub](https://github.com/Microsoft/Cognitive-Speech-STT-Windows). O SDK de reconhecimento de voz do cliente fornece uma classe de fábrica **SpeechRecognitionServiceFactory**, que oferece os seguintes métodos:

  *   ```CreateDataClient(...)```: Um cliente de reconhecimento de dados.
  *   ```CreateDataClientWithIntent(...)```: Um cliente de reconhecimento de dados com o objetivo.
  *   ```CreateMicrophoneClient(...)```: Um cliente de reconhecimento de microfone.
  *   ```CreateMicrophoneClientWithIntent(...)```: Um cliente de reconhecimento de microfone com tentativa.

Para obter documentação detalhada, consulte o [API de reconhecimento de voz do Bing](https://docs.microsoft.com/azure/cognitive-services/speech/home). Os pontos finais de serviço de reconhecimento de voz personalizadas suportam o SDK do mesmo.

O cliente de reconhecimento de dados é adequado para reconhecimento de voz de dados, tais como um ficheiro ou de outra origem de áudio. O cliente de reconhecimento microfone é adequado para reconhecimento de voz do microfone. A utilização de intenção do cliente pode devolver resultados estruturados de intenção do [serviço inteligente de compreensão de linguagem](https://www.luis.ai/) (LUIS), se criou uma aplicação LUIS para o seu cenário.

Todos os quatro tipos de clientes podem ser instanciados de duas formas. A forma primeiro utiliza a API de reconhecimento de voz serviços cognitivos padrão. A segunda forma permite-lhe especificar um URL que corresponde à sua endpoint personalizado criado com o serviço de reconhecimento de voz personalizadas.

Por exemplo, pode criar um **DataRecognitionClient** que envia pedidos para um ponto de final personalizado utilizando o método seguinte:

```csharp
public static DataRecognitionClient CreateDataClient(SpeeechRecognitionMode speechRecognitionMode, string language, string primaryOrSecondaryKey, **string url**);
```

O **your_subscriptionId** e **endpointURL** referir-se a chave de subscrição e o URL de sockets web, respetivamente, no **informações de implementação** página.

O **AuthenticationUri** é utilizado para receber um token a partir do serviço de autenticação. Este URI tem de ser definido em separado, conforme mostrado no seguinte código de exemplo.

Este código de exemplo mostra como utilizar o SDK do cliente:

```csharp
var dataClient = SpeechRecognitionServiceFactory.CreateDataClient(
  SpeechRecognitionMode.LongDictation,
  "en-us",
  "your_subscriptionId",
  "your_subscriptionId",
  "endpointURL");
// set the authorization Uri
dataClient.AuthenticationUri = "https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken";
```

> [!NOTE]
> Quando utiliza **criar** métodos no SDK, tem de fornecer o ID de subscrição duas vezes devido a sobrecarga do **criar** métodos.
>

O serviço de reconhecimento de voz personalizadas utiliza dois URLs diferentes para reconhecimento de formulário de curta e longa formulário. Ambos são listadas no **implementações** página. Utilize o URL de ponto final correto para o formulário específico que pretende utilizar.

Para mais informações sobre como invocar os vários clientes de reconhecimento com o ponto final personalizado, consulte o [SpeechRecognitionServiceFactory](https://www.microsoft.com/cognitive-services/Speech-api/documentation/GetStarted/GetStartedCSharpDesktop) classe. A documentação nesta página se refere a adaptation modelo acústica, mas se aplica a todos os pontos finais criados utilizando o serviço de reconhecimento de voz personalizadas.

## <a name="send-requests-by-using-the-speech-protocol"></a>Enviar pedidos utilizando o protocolo de reconhecimento de voz

Os pontos finais a apresentar para o [protocolo de reconhecimento de voz](https://docs.microsoft.com/azure/cognitive-services/speech/api-reference-rest/websocketprotocol) são pontos finais para o protocolo de reconhecimento de voz do Socket de Web origem aberta.

Atualmente, a implementação de cliente apenas oficial é para [JavaScript](https://github.com/Azure-Samples/SpeechToText-WebSockets-Javascript). Se quiser começar com o exemplo fornecido não existe, efetue as seguintes alterações ao código e criar a amostra novamente:

1. No _src\sdk\speech.browser\SpeechConnectionFactory.ts_, substitua o nome de anfitrião "wss://speech.platform.bing.com" com o nome de anfitrião mostrado na página de detalhes da implementação. Não inserir o URI completo aqui, mas apenas o *wss* protocolo esquema e o nome do anfitrião. Por exemplo:

    ```JavaScript
    private get Host(): string {
        return Storage.Local.GetOrAdd("Host", "wss://<your_key_goes_here>.api.cris.ai");
    }
    ```

2. Definir o _recognitionMode_ parâmetro _samples\browser\Samples.html_ , de acordo com os requisitos:
    * _RecognitionMode.Interactive_ suporta pedidos até 15 segundos.
    * _RecognitionMode.Conversation_ e _RecognitionMode.Dictation_ (ambos são equivalentes no serviço de reconhecimento de voz personalizadas) suporte pedidos até 10 minutos.

3. Crie a amostra através da utilização de "gulp compilação" antes de o utilizar.

> [!NOTE]
> Certifique-se de que utiliza o URI correto para este protocolo. O esquema necessário é *wss* (não *http* como o protocolo de cliente). 

Para obter mais informações, consulte o [API de reconhecimento de voz do Bing](https://docs.microsoft.com/azure/cognitive-services/speech/getstarted/getstartedclientlibraries) documentação.

## <a name="send-requests-by-using-http"></a>Enviar pedidos utilizando HTTP

Enviar um pedido para o ponto final personalizado utilizando um HTTP post é semelhante ao enviar um pedido por HTTP para a API de reconhecimento de voz de Bing serviços cognitivos. Modificar o URL para refletir o endereço da sua implementação personalizada.

Existem algumas restrições em pedidos enviados através de HTTP para o ponto final de reconhecimento de voz de serviços cognitivos e os pontos finais personalizados criados com este serviço. O pedido HTTP não é possível devolver resultados parciais durante o processo de reconhecimento. Além disso, a duração dos pedidos está limitada a 10 segundos para o conteúdo de áudio e 14 segundos gerais.

Para criar um pedido post, siga o mesmo processo que utiliza para a API de reconhecimento de voz serviços cognitivos.

1. Obter um token de acesso, utilizando o seu ID de subscrição. Este token é necessário para aceder ao ponto final de reconhecimento. Podem ser reutilizado durante 10 minutos.

    ```
    curl -X POST --header "Ocp-Apim-Subscription-Key:<subscriptionId>" --data "" "https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken"
    ```
      O **subscriptionId** deve ser definido para o ID de subscrição a utilizar para esta implementação. A resposta é o token simples, que é necessário para o pedido seguinte.

2. Publique áudio para o ponto final utilizando POST novamente.

    ```
    curl -X POST --data-binary @example.wav -H "Authorization: Bearer <token>" -H "Content-Type: application/octet-stream" "<https_endpoint>"
    ```

    O **token** é o token de acesso que recebeu com a chamada anterior. O **https_endpoint** é o endereço completo da sua personalizado ponto final reconhecimento de voz para texto, apresentado no **informações de implementação** página.

Para obter mais informações sobre os parâmetros de post HTTP e o formato de resposta, consulte o [Microsoft cognitivos API dos serviços do Bing voz HTTP](https://www.microsoft.com/cognitive-services/speech-api/documentation/API-Reference-REST/BingVoiceRecognition#SampleImplementation).

## <a name="send-requests-by-using-the-service-library"></a>Enviar pedidos através da biblioteca de serviço
A biblioteca de serviço permite o serviço para se certificar de que a utilização de nuvem Microsoft Speech transcription converter idioma ditas texto em tempo real, para que a aplicação cliente pode enviar áudio e receber resultados de reconhecimento parcial fazer uma cópia em simultâneo e de forma assíncrona. Detalhes do serviço SDK podem ser encontrado [aqui](https://docs.microsoft.com/azure/cognitive-services/speech/getstarted/getstartedcsharpservicelibrary)

> [!NOTE]
> Quando utilizar a biblioteca de serviço tem de alterar o URI do fornecedor de autorização na implementação de **IAuthorizationProvider** para https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken.

## <a name="next-steps"></a>Passos Seguintes
* Melhorar a precisão com seu [personalizado modelo acústica](cognitive-services-custom-speech-create-acoustic-model.md).
* Melhorar a precisão com um [modelo idioma personalizado](cognitive-services-custom-speech-create-language-model.md).
