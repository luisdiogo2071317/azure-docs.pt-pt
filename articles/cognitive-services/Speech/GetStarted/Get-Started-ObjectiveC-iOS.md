---
title: Introdução à API de reconhecimento de voz Microsoft no Objective-C no iOS | Microsoft Docs
description: Utilize a API de reconhecimento de voz do Microsoft para desenvolver aplicações iOS que converter áudio ditas em texto.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.component: bing-speech
ms.topic: article
ms.date: 09/29/2017
ms.author: zhouwang
ms.openlocfilehash: bbb8d3975cdab537135b97ca9bbf6e845aa3fa0e
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352238"
---
# <a name="get-started-with-the-speech-recognition-api-in-objective-c-on-ios"></a>Introdução à API de reconhecimento de voz no Objective-C no iOS

Com a API de reconhecimento de voz, pode desenvolver aplicações de iOS que utilizam o serviço baseado na nuvem de reconhecimento de voz para converter áudio ditas para texto. A API suporta a transmissão em fluxo em tempo real, para que a aplicação possa em simultâneo e no modo assíncrono receber os resultados de reconhecimento parcial ao mesmo tempo que está a enviar para o serviço de áudio.

Este artigo utiliza uma aplicação de exemplo para demonstrar as noções básicas sobre como começar a utilizar com a API de reconhecimento de voz para desenvolver uma aplicação iOS. Para uma referência de API completa, consulte o [referência da biblioteca cliente SDK de reconhecimento de voz](https://cdn.rawgit.com/Microsoft/Cognitive-Speech-STT-iOS/master/com.Microsoft.SpeechSDK-1_0-for-iOS.docset/Contents/Resources/Documents/index.html).

## <a name="prerequisites"></a>Pré-requisitos

### <a name="platform-requirements"></a>Requisitos de plataforma

Certifique-se de que o IDE do XCode Mac está instalado.

### <a name="get-the-client-library-and-examples"></a>Instalar o cliente da biblioteca e exemplos

A biblioteca de cliente de reconhecimento de voz e exemplos de iOS estão disponíveis no [voz SDK cliente para iOS](https://github.com/microsoft/cognitive-speech-stt-ios).

### <a name="subscribe-to-the-speech-recognition-api-and-get-a-free-trial-subscription-key"></a>Subscrever a API de reconhecimento de voz e obter uma chave de subscrição de avaliação gratuita

A API de reconhecimento de voz faz parte de serviços cognitivos (anteriormente Oxford de projeto). Pode obter as chaves de subscrição de avaliação gratuita do [subscrição de serviços cognitivos](https://azure.microsoft.com/try/cognitive-services/) página. Depois de selecionar a API de reconhecimento de voz, selecione **obter chave de API** para obter a chave. Devolve uma chave primária e secundária. Ambas as chaves estão associadas a quota mesmo, pelo que pode utilizar a chave.

Se pretender utilizar *reconhecimento com intenção*, terá também de inscrever-se a [idioma compreender inteligente serviço (LUIS)](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/).

> [!IMPORTANT]
> * Obter uma chave de subscrição. Antes de poder utilizar bibliotecas de cliente de reconhecimento de voz, tem de ter um [chave de subscrição](https://azure.microsoft.com/try/cognitive-services/).
>
> * Utilize a sua chave de subscrição. Com a aplicação de exemplo fornecidos iOS, terá de atualizar o ficheiro Samples/SpeechRecognitionServerExample/settings.plist com a sua chave de subscrição. Para obter mais informações, consulte [compilar e executar amostras](#build-and-run-samples).

## <a name="use-the-speech-client-library"></a>Utilize a biblioteca de cliente de reconhecimento de voz

Para adicionar a biblioteca de clientes para um projeto XCode, siga estes [instruções](https://github.com/Azure-Samples/Cognitive-Speech-STT-iOS#the-client-library).

Para localizar o cliente referência da biblioteca para iOS, consulte este [página Web](https://cdn.rawgit.com/Microsoft/Cognitive-Speech-STT-iOS/master/com.Microsoft.SpeechSDK-1_0-for-iOS.docset/Contents/Resources/Documents/index.html).

## <a name="build-and-run-samples"></a>Compilar e executar as amostras

Para obter informações sobre a compilação e execução de exemplos, consulte este [página Leia-me](https://github.com/Azure-Samples/Cognitive-Speech-STT-iOS#the-sample).

## <a name="samples-explained"></a>Amostras explicadas

### <a name="create-recognition-clients"></a>Criar os clientes de reconhecimento

O código no exemplo seguinte mostra como criar classes de cliente de reconhecimento com base em cenários de utilizador:

```
{
    NSString* language = @"en-us";

    NSString* path = [[NSBundle mainBundle] pathForResource:@"settings" ofType:@"plist"];
    NSDictionary* settings = [[NSDictionary alloc] initWithContentsOfFile:path];

    NSString* primaryOrSecondaryKey = [settings objectForKey:(@"primaryKey")];
    NSString* luisAppID = [settings objectForKey:(@"luisAppID")];
    NSString* luisSubscriptionID = [settings objectForKey:(@"luisSubscriptionID")];

    if (isMicrophoneReco) {
        if (!isIntent) {
            micClient = [SpeechRecognitionServiceFactory createMicrophoneClient:(recoMode)
                                                                   withLanguage:(language)
                                                                        withKey:(primaryOrSecondaryKey)
                                                                   withProtocol:(self)];
        }
        else {
            MicrophoneRecognitionClientWithIntent* micIntentClient;
            micIntentClient = [SpeechRecognitionServiceFactory createMicrophoneClientWithIntent:(language)
                                                                                        withKey:(primaryOrSecondaryKey)
                                                                                  withLUISAppID:(luisAppID)
                                                                                 withLUISSecret:(luisSubscriptionID)
                                                                                   withProtocol:(self)];
            micClient = micIntentClient;
        }
    }
    else {
        if (!isIntent) {
            dataClient = [SpeechRecognitionServiceFactory createDataClient:(recoMode)
                                                              withLanguage:(language)
                                                                   withKey:(primaryOrSecondaryKey)
                                                              withProtocol:(self)];
        }
        else {
            DataRecognitionClientWithIntent* dataIntentClient;
            dataIntentClient = [SpeechRecognitionServiceFactory createDataClientWithIntent:(language)
                                                                                   withKey:(primaryOrSecondaryKey)
                                                                             withLUISAppID:(luisAppID)
                                                                            withLUISSecret:(luisSubscriptionID)
                                                                              withProtocol:(self)];
            dataClient = dataIntentClient;
        }
    }
}

```

A biblioteca de clientes fornece reconhecimento previamente implementado as classes de cliente para cenários típicos de reconhecimento de voz:

* `DataRecognitionClient`: O reconhecimento de voz com dados PCM (por exemplo, a partir de uma origem de ficheiro ou de áudio). Os dados são divididos cópias de segurança em memórias intermédias e cada memória intermédia é enviada para o serviço de reconhecimento de voz. Sem modificações é feita para memórias intermédias, pelo que os utilizadores podem aplicar as seus próprios deteção de silêncio, se assim o desejar. Se os dados de ficheiros WAV, pode enviar dados a partir da direita do ficheiro para o servidor. Se tiver dados não processados, por exemplo, através de Bluetooth, áudio, primeiro enviar um cabeçalho de formato para o servidor seguido os dados.
* `MicrophoneRecognitionClient`: O reconhecimento de voz com áudio feitos microfone. Certifique-se microfone está ativado e que os dados do microfone são enviados para o serviço de reconhecimento de voz. Um "silêncio Detector" incorporado é aplicada aos dados microfone antes de ser enviada para o serviço de reconhecimento.
* `DataRecognitionClientWithIntent` e `MicrophoneRecognitionClientWithIntent`: para além do texto de reconhecimento, estes clientes devolvem estruturadas informações sobre a intenção de orador, que as aplicações podem utilizar para implementarem mais ações. Para utilizar "Intenção", precisa de formação de um modelo pela primeira vez utilizando [LUIS](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/).

### <a name="recognition-language"></a>Idioma de reconhecimento

Quando utiliza `SpeechRecognitionServiceFactory` para criar o cliente, tem de selecionar um idioma. Para obter a lista completa dos idiomas suportados pelo serviço de reconhecimento de voz, consulte [idiomas suportados](../API-Reference-REST/supportedlanguages.md).

### <a name="speechrecognitionmode"></a>SpeechRecognitionMode

Também tem de especificar `SpeechRecognitionMode` quando cria o cliente com `SpeechRecognitionServiceFactory`:

* `SpeechRecognitionMode_ShortPhrase`: Um utterance até 15 segundos de tempo. Como os dados são enviados para o serviço, o cliente receberá vários resultados parciais e um resultado final com várias opções de n melhor.
* `SpeechRecognitionMode_LongDictation`: Um utterance até dois minutos longa. Como os dados são enviados para o serviço, o cliente receberá vários resultados parciais e vários resultados finais, com base no onde o servidor identifica o frase pausa.

### <a name="attach-event-handlers"></a>Anexar processadores de eventos

Pode anexar vários processadores de eventos para o cliente que criou:

* **Eventos de resultados parciais**: Este evento é chamado sempre que o serviço de reconhecimento de voz prevê que poderá ser indicar, mesmo antes de terminar a falando (se utilizar `MicrophoneRecognitionClient`) ou de concluir o envio de dados (se utilizar `DataRecognitionClient`).
* **Eventos de erro**: chamado quando o serviço Deteta um erro.
* **Eventos intenção**: chamado nos clientes "WithIntent" (apenas no modo de ShortPhrase) após o final reconhecimento o resultado é analisado para um objetivo JSON structured.
* **Resultar eventos**:
  * No `SpeechRecognitionMode_ShortPhrase` modo, este evento é chamado e devolve n melhores resultados, depois de concluir falando.
  * No `SpeechRecognitionMode_LongDictation` modo, o processador de eventos é chamado várias vezes, com base na qual o serviço identifica o frase pausa.
  * **Para cada uma das opções de n melhor**, são devolvidas um valor de confiança e algumas formas diferentes de texto reconhecido. Para obter mais informações, consulte [formato de saída](../Concepts.md#output-format).

## <a name="related-topics"></a>Tópicos relacionados

* [Referência da biblioteca de cliente para iOS](https://cdn.rawgit.com/Microsoft/Cognitive-Speech-STT-iOS/master/com.Microsoft.SpeechSDK-1_0-for-iOS.docset/Contents/Resources/Documents/index.html)
* [Começar com o reconhecimento de voz da Microsoft e/ou intenção em Java no Android](GetStartedJavaAndroid.md)
* [Introdução à API de reconhecimento de voz Microsoft em JavaScript](GetStartedJSWebsockets.md)
* [Introdução à API de reconhecimento de voz Microsoft através de REST](GetStartedREST.md)
