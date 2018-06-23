---
title: Introdução à API de reconhecimento de voz Microsoft em Java no Android | Microsoft Docs
description: Utilize a API de reconhecimento de voz da Microsoft para desenvolver aplicações Android que converter áudio ditas em texto.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.component: bing-speech
ms.topic: article
ms.date: 09/29/2017
ms.author: zhouwang
ms.openlocfilehash: a10f7be1c36fb431016a9867f606e26be858069e
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352249"
---
# <a name="get-started-with-speech-recognition-in-java-on-android"></a>Começar com o reconhecimento de voz em Java no Android

Com a API de reconhecimento de voz, pode desenvolver aplicações Android que utilizam o serviço baseado na nuvem de reconhecimento de voz para converter áudio ditas para texto. A API suporta a transmissão em fluxo em tempo real, para que a aplicação possa em simultâneo e no modo assíncrono receber os resultados de reconhecimento parcial ao mesmo tempo que está a enviar para o serviço de áudio.

Este artigo utiliza uma aplicação de exemplo para demonstrar como utilizar a biblioteca de clientes de reconhecimento de voz para Android para desenvolver aplicações de reconhecimento de voz para texto em Java para dispositivos Android.

## <a name="prerequisites"></a>Pré-requisitos

### <a name="platform-requirements"></a>Requisitos de plataforma

O exemplo é desenvolvido pela [Android Studio](http://developer.android.com/sdk/index.html) para o Windows em Java.

### <a name="get-the-client-library-and-sample-application"></a>Instalar o cliente aplicação de exemplo e biblioteca

A biblioteca de clientes de reconhecimento de voz e amostras para Android estão disponíveis no [cliente de reconhecimento de voz SDK para Android](https://github.com/microsoft/cognitive-speech-stt-android). Pode encontrar o exemplo buildable sob o diretório de exemplos/SpeechRecoExample. Pode encontrar duas bibliotecas tem de utilizar as suas próprias aplicações SpeechSDK/libs sob o armeabi e x86 pasta. O tamanho do ficheiro libandroid_platform.so é 22 MB, mas seja reduzido para 4 MB no momento da implementação.

#### <a name="subscribe-to-the-speech-api-and-get-a-free-trial-subscription-key"></a>Subscrever a API de reconhecimento de voz e obter uma chave de subscrição de avaliação gratuita

A API de reconhecimento de voz faz parte de serviços cognitivos (anteriormente Oxford de projeto). Pode obter as chaves de subscrição de avaliação gratuita do [subscrição de serviços cognitivos](https://azure.microsoft.com/try/cognitive-services/) página. Depois de selecionar a API de reconhecimento de voz, selecione **obter chave de API** para obter a chave. Devolve uma chave primária e secundária. Ambas as chaves estão associadas a quota mesmo, pelo que pode utilizar a chave.

Se pretender utilizar *reconhecimento com intenção*, terá também de inscrever-se a [idioma compreender inteligente serviço (LUIS)](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/).

> [!IMPORTANT]
>* Obter uma chave de subscrição. Antes de poder utilizar bibliotecas de cliente de reconhecimento de voz, tem de ter um [chave de subscrição](https://azure.microsoft.com/try/cognitive-services/).
>
>* Utilize a sua chave de subscrição. Com a aplicação Android de exemplo fornecido, atualize o ficheiro samples/SpeechRecoExample/res/values/strings.xml com as chaves de subscrição. Para obter mais informações, consulte [compilar e executar amostras](#build-and-run-samples).

## <a name="use-the-speech-client-library"></a>Utilize a biblioteca de cliente de reconhecimento de voz

Para utilizar a biblioteca de clientes na sua aplicação, siga o [instruções](https://github.com/microsoft/cognitive-speech-stt-android#the-client-library).

Pode encontrar o cliente referência da biblioteca para Android na pasta de documentos a [cliente de reconhecimento de voz SDK para Android](https://github.com/microsoft/cognitive-speech-stt-android).

## <a name="build-and-run-samples"></a>Compilar e executar as amostras

Para saber como criar e executar exemplos, consulte este [página Leia-me](https://github.com/microsoft/cognitive-speech-stt-android#the-sample).

## <a name="samples-explained"></a>Amostras explicadas

### <a name="create-recognition-clients"></a>Criar os clientes de reconhecimento

O código no exemplo seguinte mostra como criar classes de cliente de reconhecimento com base em cenários de utilizador:

```java
void initializeRecoClient()
    {
        String language = "en-us";

        String subscriptionKey = this.getString(R.string.subscription_key);
        String luisAppID = this.getString(R.string.luisAppID);
        String luisSubscriptionID = this.getString(R.string.luisSubscriptionID);

        if (m_isMicrophoneReco && null == m_micClient) {
            if (!m_isIntent) {
                m_micClient = SpeechRecognitionServiceFactory.createMicrophoneClient(this,
                                                                                     m_recoMode,
                                                                                     language,
                                                                                     this,
                                                                                     subscriptionKey);
            }
            else {
                MicrophoneRecognitionClientWithIntent intentMicClient;
                intentMicClient = SpeechRecognitionServiceFactory.createMicrophoneClientWithIntent(this,
                                                                                                   language,
                                                                                                   this,
                                                                                                   subscriptionKey,
                                                                                                   luisAppID,
                                                                                                   luisSubscriptionID);
                m_micClient = intentMicClient;

            }
        }
        else if (!m_isMicrophoneReco && null == m_dataClient) {
            if (!m_isIntent) {
                m_dataClient = SpeechRecognitionServiceFactory.createDataClient(this,
                                                                                m_recoMode,
                                                                                language,
                                                                                this,
                                                                                subscriptionKey);
            }
            else {
                DataRecognitionClientWithIntent intentDataClient;
                intentDataClient = SpeechRecognitionServiceFactory.createDataClientWithIntent(this,
                                                                                              language,
                                                                                              this,
                                                                                              subscriptionKey,
                                                                                              luisAppID,
                                                                                              luisSubscriptionID);
                m_dataClient = intentDataClient;
            }
        }
    }

```

A biblioteca de clientes fornece reconhecimento previamente implementado as classes de cliente para cenários típicos de reconhecimento de voz:

* `DataRecognitionClient`: O reconhecimento de voz com dados PCM (por exemplo, a partir de uma origem de ficheiro ou de áudio). Os dados são divididos cópias de segurança em memórias intermédias e cada memória intermédia é enviada para o serviço de reconhecimento de voz. Sem modificações é feita para memórias intermédias, para que o utilizador possa aplicar as suas próprias deteção de silêncio, se assim o desejar. Se os dados de ficheiros WAV, pode enviar dados do direito de ficheiro de serviço de reconhecimento de voz. Se tiver dados não processados, por exemplo, através de Bluetooth, áudio primeiro enviar um cabeçalho de formato ao serviço de reconhecimento de voz seguido os dados.
* `MicrophoneRecognitionClient`: O reconhecimento de voz com áudio feitos microfone. Certifique-se microfone está ativado e que os dados do microfone são enviados para o serviço de reconhecimento de voz. Um "silêncio Detector" incorporado é aplicada aos dados microfone antes de ser enviada para o serviço de reconhecimento.
* `DataRecognitionClientWithIntent` e `MicrophoneRecognitionClientWithIntent`: estes clientes devolverem, além de texto de reconhecimento, estruturados informações sobre a intenção de orador, que pode ser utilizado para implementarem o ações pelas suas aplicações. Para utilizar "Intenção", precisa de formação de um modelo pela primeira vez utilizando [LUIS](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/).

### <a name="recognition-language"></a>Idioma de reconhecimento

Quando utiliza `SpeechRecognitionServiceFactory` para criar o cliente, tem de selecionar um idioma. Para obter a lista completa dos idiomas suportados pelo serviço de reconhecimento de voz, consulte [idiomas suportados](../API-Reference-REST/supportedlanguages.md).

### `SpeechRecognitionMode`

Também tem de especificar `SpeechRecognitionMode` quando cria o cliente com `SpeechRecognitionServiceFactory`:

* `ShortPhrase`: Um utterance até 15 segundos de tempo. Como os dados são enviados para o serviço, o cliente receberá vários resultados parciais e um resultado final com várias opções de n melhor.
* `LongDictation`: Um utterance até dois minutos longa. Como os dados são enviados para o serviço, o cliente receberá vários resultados parciais e vários resultados finais, com base na qual o serviço identifica o frase pausa.

### <a name="attach-event-handlers"></a>Anexar processadores de eventos

Pode anexar vários processadores de eventos para o cliente que criou:

* **Eventos de resultados parciais**: Este evento é chamado sempre que o serviço de reconhecimento de voz prevê que poderá ser indicar, mesmo antes de terminar a falando (se utilizar `MicrophoneRecognitionClient`) ou de concluir o envio de dados (se utilizar `DataRecognitionClient`).
* **Eventos de erro**: chamado quando o serviço Deteta um erro.
* **Eventos intenção**: chamado "WithIntent" clientes (apenas no `ShortPhrase` modo) após o resultado final reconhecimento é analisado para um objetivo JSON structured.
* **Resultar eventos**:
  * No `ShortPhrase` modo, este evento é chamado e devolve n melhores resultados, depois de concluir falando.
  * No `LongDictation` modo, o processador de eventos é chamado várias vezes, com base na qual o serviço identifica o frase pausa.
  * **Para cada uma das opções de n melhor**, são devolvidas um valor de confiança e algumas formas diferentes de texto reconhecido. Para obter mais informações, consulte [formato de saída](../Concepts.md#output-format).

## <a name="related-topics"></a>Tópicos relacionados

* [Referência da biblioteca de cliente para Android](https://github.com/Azure-Samples/Cognitive-Speech-STT-Android/tree/master/docs)
* [Introdução à API de reconhecimento de voz Microsoft em c# para Windows no .NET](GetStartedCSharpDesktop.md)
* [Introdução à API de reconhecimento de voz Microsoft no Objective-C no iOS](Get-Started-ObjectiveC-iOS.md)
* [Introdução à API de reconhecimento de voz Microsoft em JavaScript](GetStartedJSWebsockets.md)
* [Introdução à API de reconhecimento de voz Microsoft através de REST](GetStartedREST.md)
