---
title: Começar com a API de reconhecimento de voz do Microsoft através da biblioteca de serviço do c# | Microsoft Docs
description: Utilize a biblioteca de serviço de reconhecimento de voz Microsoft para converter o idioma ditas para texto.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.component: bing-speech
ms.topic: article
ms.date: 09/17/2017
ms.author: zhouwang
ms.openlocfilehash: 0320f41658a7ac4d6bf9e88ed998c853b665d485
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352279"
---
# <a name="get-started-with-the-speech-recognition-service-library-in-c35-for-net-windows"></a>Introdução à biblioteca de serviço de reconhecimento de voz do C&#35; para .NET Windows

A biblioteca de serviço é para os programadores que tenham os seus próprios serviço em nuvem e pretendem chamar o serviço de reconhecimento de voz do seu serviço. Se pretender chamar o serviço de reconhecimento de voz a partir de aplicações de dispositivo vinculados, não utilize este SDK. (Utilizar outras bibliotecas de cliente ou REST APIs para esse.)

Para utilizar a biblioteca de serviço do c#, instalar o [pacote NuGet Microsoft.Bing.Speech](https://www.nuget.org/packages/Microsoft.Bing.Speech/). Para a biblioteca de referência da API, consulte o [biblioteca do serviço Microsoft voz c#](https://cdn.rawgit.com/Microsoft/Cognitive-Speech-STT-ServiceLibrary/master/docs/index.html).

As secções seguintes descrevem como instalar, criar e executar a aplicação de exemplo do c# utilizando a biblioteca de serviço do c#.

## <a name="prerequisites"></a>Pré-requisitos

### <a name="platform-requirements"></a>Requisitos de plataforma

O exemplo seguinte foi desenvolvido para o Windows 8 e posteriores e o .NET 4.5 + Framework utilizando [Visual Studio 2015, edição Community](https://www.visualstudio.com/products/visual-studio-community-vs).

### <a name="get-the-sample-application"></a>Obter a aplicação de exemplo

Clonar o exemplo a [exemplo biblioteca serviço voz c#](https://github.com/Microsoft/Cognitive-Speech-STT-ServiceLibrary) repositório.

### <a name="subscribe-to-the-speech-recognition-api-and-get-a-free-trial-subscription-key"></a>Subscrever a API de reconhecimento de voz e obter uma chave de subscrição de avaliação gratuita

A API de reconhecimento de voz faz parte de serviços cognitivos (anteriormente Oxford de projeto). Pode obter as chaves de subscrição de avaliação gratuita do [subscrição de serviços cognitivos](https://azure.microsoft.com/try/cognitive-services/) página. Depois de selecionar a API de reconhecimento de voz, selecione **obter chave de API** para obter a chave. Devolve uma chave primária e secundária. Ambas as chaves estão associadas a quota mesmo, pelo que pode utilizar a chave.

> [!IMPORTANT]
> * Obter uma chave de subscrição. Antes de poder utilizar as bibliotecas de cliente de reconhecimento de voz, tem de ter um [chave de subscrição](https://azure.microsoft.com/try/cognitive-services/).
>
> * Utilize a sua chave de subscrição. Com o fornecido c# serviço biblioteca exemplo de aplicação, terá de fornecer a chave de subscrição como um dos parâmetros da linha de comandos. Para obter mais informações, consulte [executar a aplicação de exemplo](#step-3-run-the-sample-application).

## <a name="step-1-install-the-sample-application"></a>Passo 1: Instalar a aplicação de exemplo

1. Inicie o Visual Studio 2015 e selecione **ficheiro** > **abra** > **projeto/solução**.

2. Faça duplo clique para abrir o ficheiro de solução de 2015 do Visual Studio (. sln) com o nome SpeechClient.sln. A solução é aberto no Visual Studio.

## <a name="step-2-build-the-sample-application"></a>Passo 2: Criar a aplicação de exemplo

Prima Ctrl + Shift + B, ou selecione **criar** no menu do Friso. Em seguida, selecione **compilar solução**.

## <a name="step-3-run-the-sample-application"></a>Passo 3: Executar a aplicação de exemplo

1. Uma vez concluída a compilação, prima F5 ou selecione **iniciar** no menu do friso para executar o exemplo.

2. Abra o diretório de saída para o exemplo, por exemplo, SpeechClientSample\bin\Debug. Prima Shift + direito clique e selecione **aqui a janela de comandos aberta**.

3. Executar `SpeechClientSample.exe` com os argumentos seguintes:

   * Arg [0]: Especifique um ficheiro de áudio WAV entrada.
   * Arg [1]: Especifique a região de áudio.
   * Arg [2]: Especificar os modos de reconhecimento: *curto* para o `ShortPhrase` modo e *longo* para o `LongDictation` modo.
   * Arg [3]: Especifique a chave de subscrição para aceder ao serviço de reconhecimento de voz.

## <a name="samples-explained"></a>Amostras explicadas

### <a name="recognition-modes"></a>Modos de reconhecimento

* `ShortPhrase` modo: um utterance até 15 segundos de tempo. Como os dados são enviados para o servidor, o cliente recebe vários resultados parciais e um resultado de melhor final.
* `LongDictation` modo: um utterance até 10 minutos de tempo. Como os dados são enviados para o servidor, o cliente receberá vários resultados parciais e vários resultados finais, com base no onde o servidor indica o frase pausa.

### <a name="supported-audio-formats"></a>Formatos de áudio suportados

A API de reconhecimento de voz suporta áudio/WAV ao utilizar codecs seguintes:

* Canal único PCM
* Siren
* SirenSR

### <a name="preferences"></a>Preferências

Para criar um SpeechClient, terá de criar primeiro um objeto de preferências. O objeto de preferências é um conjunto de parâmetros que configura o comportamento do serviço de reconhecimento de voz. É composto pelos seguintes campos:

* `SpeechLanguage`: A região de áudio enviado para o serviço de reconhecimento de voz.
* `ServiceUri`: O ponto final utilizado para chamar o serviço de reconhecimento de voz.
* `AuthorizationProvider`: Uma implementação de IAuthorizationProvider utilizada para obter os tokens para aceder ao serviço de reconhecimento de voz. Apesar da amostra fornece um fornecedor de autorização de serviços cognitivos, recomendamos vivamente que crie a sua própria implementação para lidar com colocação em cache de token.
* `EnableAudioBuffering`: Uma opção avançada. Consulte [gestão ligação](#connection-management).

### <a name="speech-input"></a>Entrada de voz

O objeto de SpeechInput consiste em dois campos:

* **Áudio**: uma implementação de sequência à sua escolha de que o SDK obtém áudio. Pode ser qualquer [fluxo](https://msdn.microsoft.com/library/system.io.stream(v=vs.110).aspx) que suporta a leitura.
   > [!NOTE]
   > O SDK Deteta o fim da sequência quando o fluxo devolve **0** na leitura.

* **RequestMetadata**: metadados sobre o pedido de reconhecimento de voz. Para obter mais informações, consulte o [referência](https://cdn.rawgit.com/Microsoft/Cognitive-Speech-STT-ServiceLibrary/master/docs/index.html).

### <a name="speech-request"></a>Pedido de reconhecimento de voz

Depois de ter instanciar um objetos SpeechClient e SpeechInput, utilize RecognizeAsync para efetuar um pedido ao serviço de reconhecimento de voz.

```cs
    var task = speechClient.RecognizeAsync(speechInput);
```

Depois do pedido é concluído, a tarefa devolvida pelo RecognizeAsync termina. A última RecognitionResult é final o reconhecimento. A tarefa pode falhar se o serviço ou o SDK falhar inesperadamente.

### <a name="speech-recognition-events"></a>Eventos de reconhecimento de voz

#### <a name="partial-results-event"></a>Evento de resultados parciais

Este evento é chamado sempre que o serviço de reconhecimento de voz prevê que poderá ser indicar, mesmo antes de terminar a falando (se utilizar `MicrophoneRecognitionClient`) ou de concluir o envio de dados (se utilizar `DataRecognitionClient`). Pode subscrever o evento utilizando `SpeechClient.SubscribeToPartialResult()`. Ou pode utilizar o método de subscrição de eventos genérico `SpeechClient.SubscribeTo<RecognitionPartialResult>()`.

**Formato de retorno** | Descrição |
------|------
**LexicalForm** | Este formulário é ideal para utilização por aplicações que precisam de resultados de reconhecimento de voz não processados, não processados.
**DisplayText** | O frase reconhecido com a normalização de texto inverso, maiúsculas/minúsculas, pontuação e máscara de profanity aplicada. Profanity está oculto com a série de asteriscos após o caráter inicial, por exemplo, "d ***". Este formulário é ideal para utilização por aplicações apresentar os resultados de reconhecimento de voz para um utilizador.
**Confiança** | O nível de confiança de que o frase reconhecido representa de áudio associado conforme definido pelo servidor de reconhecimento de voz.
**MediaTime** | O tempo atual relativo para o início da sequência de áudio (por 100 nanossegundos unidades de tempo).
**MediaDuration** | O frase duração/comprimento atual relativo ao segmento de áudio (por 100 nanossegundos unidades de tempo).

#### <a name="result-event"></a>Evento de resultado
Quando concluir falando (no `ShortPhrase` modo), este evento é chamado. Está a fornecida com n melhor opções para o resultado. No `LongDictation` modo, o evento pode ser chamado várias vezes, com base no onde o servidor indica o frase pausa. Pode subscrever o evento utilizando `SpeechClient.SubscribeToRecognitionResult()`. Ou pode utilizar o método de subscrição de eventos genérico `SpeechClient.SubscribeTo<RecognitionResult>()`.

**Formato de retorno** | Descrição |
------|------|
**RecognitionStatus** | O estado na forma como o reconhecimento foram produzido. Por exemplo, foram-produzido como resultado de reconhecimento com êxito ou como resultado de cancelar a ligação, etc.
**Expressões** | O conjunto de expressões de reconhecido n melhor com a confiança de reconhecimento.

Para obter mais informações sobre os resultados de reconhecimento, consulte [formato de saída](../Concepts.md#output-format).

### <a name="speech-recognition-response"></a>Resposta de reconhecimento de voz

Exemplo de resposta de reconhecimento de voz:
```
--- Partial result received by OnPartialResult  
---what  
--- Partial result received by OnPartialResult  
--what's  
--- Partial result received by OnPartialResult  
---what's the web  
--- Partial result received by OnPartialResult   
---what's the weather like  
---***** Phrase Recognition Status = [Success]   
***What's the weather like? (Confidence:High)  
What's the weather like? (Confidence:High) 
```

## <a name="connection-management"></a>Gestão de ligações

A API utiliza uma ligação de WebSocket única por pedido. Experiência de utilizador ideais, o SDK tenta restabelecer a ligação ao serviço de reconhecimento de voz e iniciar o reconhecimento a partir do último RecognitionResult recebida. Por exemplo, se o pedido de áudio é dois minutos de tempo, o SDK recebeu um RecognitionEvent na marca de um minuto e Ocorreu uma falha de rede após cinco segundos, o SDK inicia uma nova ligação é iniciado a partir de marca de um minuto.

>[!NOTE]
>O SDK não procurar o ressarcimento de volta a marca de um minuto porque o fluxo não suportem a pesquisa. Em vez disso, o SDK mantém uma memória intermédia interna que utiliza para a memória intermédia de áudio e limpa a memória intermédia como receber eventos de RecognitionResult.

## <a name="buffering-behavior"></a>Comportamento de colocação em memória intermédia

Por predefinição, o SDK serve de memória intermédia áudio, para que pode recuperar quando ocorre uma interrupção de rede. Um cenário onde é preferível para eliminar áudio perdido durante a rede se desligar e reiniciar a ligação, é melhor desativar a memória intermédia áudio definindo `EnableAudioBuffering` no objeto preferências para `false`.

## <a name="related-topics"></a>Tópicos relacionados

[Referência da biblioteca Microsoft voz c# serviço](https://cdn.rawgit.com/Microsoft/Cognitive-Speech-STT-ServiceLibrary/master/docs/index.html)
