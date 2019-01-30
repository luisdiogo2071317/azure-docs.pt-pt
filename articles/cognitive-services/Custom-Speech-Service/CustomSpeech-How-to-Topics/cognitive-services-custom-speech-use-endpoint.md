---
title: Utilizar um ponto de final de voz personalizada com o serviço de voz personalizada no Azure | Documentos da Microsoft
description: Saiba como utilizar um ponto de final de voz em texto personalizado com o serviço de voz personalizada nos serviços cognitivos.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.subservice: custom-speech
ms.topic: article
ms.date: 02/08/2017
ms.author: panosper
ms.openlocfilehash: 3f99172f486c374cdbd0b6ebd50fdf7bfec98ec3
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55227153"
---
# <a name="use-a-custom-speech-to-text-endpoint"></a>Utilizar um ponto final de conversão de voz em texto personalizado

[!INCLUDE [Deprecation note](../../../../includes/cognitive-services-custom-speech-deprecation-note.md)]

Pode enviar pedidos para um ponto de final de voz em texto do serviço de voz personalizada do Azure, de forma semelhante possível para o ponto de final de voz de serviços cognitivos predefinido. Estes pontos finais são funcionalmente idênticos para os pontos de extremidade padrão a API de voz. Portanto, a mesma funcionalidade que está disponível através da biblioteca de cliente ou a API REST para a API de voz também está disponível para o ponto final personalizado.

Os pontos de extremidade que cria ao utilizar este serviço podem processar diferentes números de pedidos em simultâneo. O volume depende do escalão de preço associado à subscrição. Se forem recebidos demasiados pedidos, ocorrerá um erro. O escalão gratuito tem um limite mensal de pedidos.

O serviço de parte do princípio de que os dados são transmitidos em tempo real. Se for enviada de forma mais rápida, a solicitação é considerada em execução até que tenha passado sua duração de áudio em tempo real.

> [!NOTE]
> Suportamos o [novos web sockets de](https://docs.microsoft.com/azure/cognitive-services/speech/api-reference-rest/websocketprotocol) ainda. Se planeia utilizar web sockets com o ponto final de voz personalizada, siga as instruções aqui.
>
> A nova [REST API](https://docs.microsoft.com/azure/cognitive-services/speech/getstarted/getstartedrest) será suportada em breve. Se pretender ao chamar o ponto final de voz personalizada através de HTTP, siga as instruções aqui.
>

## <a name="send-requests-by-using-the-speech-client-library"></a>Enviar pedidos com a biblioteca de cliente de voz

Para enviar pedidos para o seu ponto final personalizado com a biblioteca de cliente de voz, inicie o cliente de reconhecimento. Utilizar o SDK de voz do cliente do [NuGet](http://nuget.org/). Procure *reconhecimento de fala*e selecione o pacote de reconhecimento de voz da Microsoft para a sua plataforma. Alguns exemplos de código podem ser encontrado no [GitHub](https://github.com/Microsoft/Cognitive-Speech-STT-Windows). O SDK de voz de cliente fornece uma classe de fábrica **SpeechRecognitionServiceFactory**, que oferece os seguintes métodos:

  *   ```CreateDataClient(...)```: Um cliente de reconhecimento de dados.
  *   ```CreateDataClientWithIntent(...)```: Um cliente de reconhecimento de dados com a intenção.
  *   ```CreateMicrophoneClient(...)```: Um cliente de reconhecimento de microfone.
  *   ```CreateMicrophoneClientWithIntent(...)```: Um cliente de reconhecimento de microfone com a intenção.

Para obter documentação detalhada, consulte a [API de voz do Bing](https://docs.microsoft.com/azure/cognitive-services/speech/home). Os pontos de extremidade do serviço de voz personalizada suportam o mesmo SDK.

O cliente de reconhecimento de dados é adequado para o reconhecimento de fala de dados, como um ficheiro ou de outra fonte de áudio. O cliente de reconhecimento do microfone é adequado para o reconhecimento de fala do microfone. A utilização do propósito de qualquer cliente pode retornar resultados de intenção estruturados do [Language Understanding Intelligent Service](https://www.luis.ai/) (LUIS), se tiver criado uma aplicação do LUIS para o seu cenário.

Todos os quatro tipos de clientes podem ser instanciados de duas formas. A primeira maneira utiliza a API de voz dos serviços cognitivos padrão. A segunda forma permite-lhe especificar um URL que corresponde ao seu ponto final personalizado criado com o serviço de voz personalizada.

Por exemplo, pode criar uma **DataRecognitionClient** que envia pedidos para um ponto final personalizado com o seguinte método:

```csharp
public static DataRecognitionClient CreateDataClient(SpeeechRecognitionMode speechRecognitionMode, string language, string primaryOrSecondaryKey, **string url**);
```

O **your_subscriptionId** e **endpointURL** fazer referência a chave de subscrição e o URL de sockets web, respectivamente, no **informações de implementação** página.

O **AuthenticationUri** é utilizado para receber um token a partir do serviço de autenticação. Este URI tem de ser definido em separado, conforme mostrado no código de exemplo a seguir.

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
> Quando utiliza **Create** métodos no SDK, tem de fornecer o ID de subscrição duas vezes por causa da sobrecarga da **criar** métodos.
>

O serviço de voz personalizada usa dois URLs diferentes para reconhecimento de formato e a forma longa. Ambos estão listadas no **implementações** página. Utilize o URL de ponto final correto para o formulário específico que pretende utilizar.

Para obter mais informações sobre a invocar os vários clientes de reconhecimento com o seu ponto final personalizado, consulte a [SpeechRecognitionServiceFactory](https://www.microsoft.com/cognitive-services/Speech-api/documentation/GetStarted/GetStartedCSharpDesktop) classe. A documentação nesta página se refere à adaptação de um modelo acústico, mas ele se aplica a todos os pontos finais criados com o serviço de voz personalizada.

## <a name="send-requests-by-using-the-speech-protocol"></a>Envie pedidos utilizando o protocolo de voz

Os pontos de extremidade mostrados para o [protocolo de voz](https://docs.microsoft.com/azure/cognitive-services/speech/api-reference-rest/websocketprotocol) são pontos finais para o protocolo de voz de soquete de Web da origem aberto.

Atualmente, a implementação de cliente apenas oficial destina [JavaScript](https://github.com/Azure-Samples/SpeechToText-WebSockets-Javascript). Se pretender começar com o exemplo fornecido, efetue as seguintes alterações ao código e criar o exemplo novamente:

1. Na _src\sdk\speech.browser\SpeechConnectionFactory.ts_, substitua o nome de anfitrião "wss://speech.platform.bing.com" com o nome de anfitrião apresentado na página de detalhes da sua implementação. Não insira o URI completo aqui, mas apenas o *wss* esquema e o nome de anfitrião de protocolo. Por exemplo:

    ```JavaScript
    private get Host(): string {
        return Storage.Local.GetOrAdd("Host", "wss://<your_key_goes_here>.api.cris.ai");
    }
    ```

2. Definir o _recognitionMode_ parâmetro na _samples\browser\Samples.html_ , de acordo com os seus requisitos:
    * _RecognitionMode.Interactive_ suporta pedidos até 15 segundos.
    * _RecognitionMode.Conversation_ e _RecognitionMode.Dictation_ (ambos são equivalentes no serviço de voz personalizada) até 10 minutos de pedidos de suporte.

3. Crie o exemplo ao utilizar a opção "gulp compilação" antes de o utilizar.

> [!NOTE]
> Certifique-se de que utiliza o URI correto para este protocolo. O esquema necessário é *wss* (não *http* tal como o protocolo de cliente). 

Para obter mais informações, consulte a [API de voz do Bing](https://docs.microsoft.com/azure/cognitive-services/speech/getstarted/getstartedclientlibraries) documentação.

## <a name="send-requests-by-using-http"></a>Envie pedidos através de HTTP

Enviar um pedido para seu ponto final personalizado através de um HTTP post é semelhante a enviar um pedido de HTTP para a API de voz do Bing de serviços cognitivos. Modificar o URL para refletir o endereço da sua implementação personalizada.

Existem algumas restrições sobre pedidos enviados através de HTTP para o ponto final de voz de serviços cognitivos e os pontos de extremidade personalizados criados com este serviço. O pedido HTTP não é possível devolver resultados parciais durante o processo de reconhecimento. Além disso, a duração dos pedidos é limitada a 10 segundos para o conteúdo de áudio e 14 segundos geral.

Para criar um pedido post, siga o mesmo processo que utiliza para a API de voz dos serviços cognitivos.

1. Obter um token de acesso com o ID da subscrição. Este token é necessário para aceder ao ponto final de reconhecimento. Ele pode ser reutilizado durante 10 minutos.

    ```
    curl -X POST --header "Ocp-Apim-Subscription-Key:<subscriptionId>" --data "" "https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken"
    ```
      O **subscriptionId** deve ser definido como o ID de subscrição a utilizar para esta implementação. A resposta é o token sem formatação, que precisa para a próxima solicitação.

2. Publicar áudio para o ponto final ao utilizar a mensagem novamente.

    ```
    curl -X POST --data-binary @example.wav -H "Authorization: Bearer <token>" -H "Content-Type: application/octet-stream" "<https_endpoint>"
    ```

    O **token** é o token de acesso que recebeu com a chamada anterior. O **https_endpoint** é o endereço completo do seu ponto de extremidade de voz em texto personalizado, mostrado na **informações de implementação** página.

Para obter mais informações sobre os parâmetros de post HTTP e o formato de resposta, consulte a [Microsoft Bing voz HTTP API dos serviços cognitivos](https://www.microsoft.com/cognitive-services/speech-api/documentation/API-Reference-REST/BingVoiceRecognition#SampleImplementation).

## <a name="send-requests-by-using-the-service-library"></a>Enviar pedidos com a biblioteca de serviço
A biblioteca de serviço permite que o serviço para verificar a utilização de nuvem Microsoft Speech transcrição para converter um idioma falado em texto em tempo real, para que a aplicação cliente pode enviar áudio e receber resultados de reconhecimento parcial de volta em simultâneo e de forma assíncrona. Detalhes do SDK do serviço podem ser encontrado [aqui](https://docs.microsoft.com/azure/cognitive-services/speech/getstarted/getstartedcsharpservicelibrary)

> [!NOTE]
> Ao utilizar a biblioteca de serviço tem de alterar o URI do fornecedor de autorização na implementação do **IAuthorizationProvider** para https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken.

## <a name="next-steps"></a>Passos Seguintes
* Melhorar a precisão com sua [um modelo acústico personalizado](cognitive-services-custom-speech-create-acoustic-model.md).
* Melhorar a precisão com um [modelo de idioma personalizado](cognitive-services-custom-speech-create-language-model.md).
