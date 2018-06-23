---
title: Começar com a API de reconhecimento de voz do Microsoft através da biblioteca ambiente de trabalho c# | Microsoft Docs
description: Desenvolva básicas Windows as aplicações que utilizam a API de reconhecimento de voz do Microsoft converter áudio ditas texto.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.component: bing-speech
ms.topic: article
ms.date: 09/27/2017
ms.author: zhouwang
ms.openlocfilehash: e59b0e25401fb5182edd52f82985ffed9052286d
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352244"
---
# <a name="get-started-with-the-speech-recognition-api-in-c35-for-net-on-windows"></a>Introdução à API de reconhecimento de voz no C&#35; para .NET no Windows

Esta página mostra como desenvolver uma aplicação básica do Windows que utiliza a API de reconhecimento de voz para converter áudio ditas em texto. Utilizar a biblioteca de cliente permite a transmissão em fluxo em tempo real, que significa que quando a aplicação cliente envia áudio para o serviço, em simultâneo e de forma assíncrona recebe resultados de reconhecimento parcial novamente.

Os programadores que pretendem utilizar o serviço de reconhecimento de voz a partir de aplicações que são executadas em qualquer dispositivo podem utilizar a biblioteca ambiente de trabalho c#. Para utilizar a biblioteca, instalar o [pacote NuGet Microsoft.ProjectOxford.SpeechRecognition x86](https://www.nuget.org/packages/Microsoft.ProjectOxford.SpeechRecognition-x86/) para uma plataforma de 32 bits e [pacote NuGet Microsoft.ProjectOxford.SpeechRecognition x64](https://www.nuget.org/packages/Microsoft.ProjectOxford.SpeechRecognition-x64/) para um plataforma de 64 bits. Para a biblioteca de clientes de referência da API, consulte [Microsoft voz c# ambiente de trabalho biblioteca](https://cdn.rawgit.com/Microsoft/Cognitive-Speech-STT-Windows/master/docs/SpeechSDK/index.html).

As secções seguintes descrevem como instalar, criar e executar a aplicação de exemplo do c# utilizando a biblioteca ambiente de trabalho c#.

## <a name="prerequisites"></a>Pré-requisitos

### <a name="platform-requirements"></a>Requisitos de plataforma

O exemplo seguinte foi desenvolvido para o Windows 8 e posteriores e .NET Framework 4.5 + utilizando [Visual Studio 2015, edição Community](https://www.visualstudio.com/products/visual-studio-community-vs).

### <a name="get-the-sample-application"></a>Obter a aplicação de exemplo

Clonar o exemplo a [exemplo de ambiente de trabalho biblioteca voz c#](https://github.com/microsoft/cognitive-speech-stt-windows) repositório.

### <a name="subscribe-to-the-speech-recognition-api-and-get-a-free-trial-subscription-key"></a>Subscrever a API de reconhecimento de voz e obter uma chave de subscrição de avaliação gratuita

A API de reconhecimento de voz faz parte de serviços cognitivos (anteriormente Oxford de projeto). Pode obter as chaves de subscrição de avaliação gratuita do [subscrição de serviços cognitivos](https://azure.microsoft.com/try/cognitive-services/) página. Depois de selecionar a API de reconhecimento de voz, selecione **obter chave de API** para obter a chave. Devolve uma chave primária e secundária. Ambas as chaves estão associadas a quota mesmo, pelo que pode utilizar a chave.

> [!IMPORTANT]
> * Obter uma chave de subscrição. Antes de utilizar as bibliotecas de cliente de reconhecimento de voz, tem de ter um [chave de subscrição](https://azure.microsoft.com/try/cognitive-services/).
>
> * Utilize a sua chave de subscrição. Com a fornecido c# aplicação de exemplo de ambiente de trabalho, cole a chave de subscrição a caixa de texto quando executar o exemplo. Para obter mais informações, consulte [executar a aplicação de exemplo](#step-3-run-the-sample-application).

## <a name="step-1-install-the-sample-application"></a>Passo 1: Instalar a aplicação de exemplo

1. Inicie o Visual Studio 2015 e selecione **ficheiro** > **abra** > **projeto/solução**.

2. Procure a pasta onde guardou os ficheiros transferidos da API de reconhecimento de voz. Selecione **voz** > **Windows**e, em seguida, selecione a pasta de WP de exemplo.

3. Faça duplo clique para abrir o ficheiro de solução de 2015 do Visual Studio (. sln) com o nome SpeechToText-WPF-Samples.sln. A solução é aberto no Visual Studio.

## <a name="step-2-build-the-sample-application"></a>Passo 2: Criar a aplicação de exemplo

1. Se pretender utilizar *reconhecimento com intenção*, terá primeiro de inscrever-se a [idioma compreender inteligente serviço (LUIS)](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/). Em seguida, utilizar o URL de ponto final da sua aplicação LUIS para definir o valor da chave `LuisEndpointUrl` no ficheiro App. config na pasta amostras/SpeechRecognitionServiceExample. Para obter mais informações sobre o URL de ponto final da aplicação LUIS, consulte [publicar a aplicação](../../luis/luis-get-started-create-app.md#publish-your-app).

   > [!TIP]
   > Substitua o `&` caráter no URL de ponto final LUIS com `&amp;` para se certificar de que o URL é corretamente interpretado pelo analisador de XML.

2. Prima Ctrl + Shift + B, ou selecione **criar** no menu do Friso. Em seguida, selecione **compilar solução**.

## <a name="step-3-run-the-sample-application"></a>Passo 3: Executar a aplicação de exemplo

1. Uma vez concluída a compilação, prima F5 ou selecione **iniciar** no menu do friso para executar o exemplo.

2. Vá para o **reconhecimento de voz do projeto Oxford ao exemplo de texto** janela. Colar a chave de subscrição para o **colar a chave de subscrição aqui para começar a** caixa de texto, como mostrado. Para manter a sua chave de subscrição no seu PC ou portátil, selecione **Guardar chave**. Para eliminar a chave de subscrição do sistema, selecione **eliminar chave** removê-lo a partir do seu PC ou portátil.

   ![Chave de colar da reconhecimento de voz](../Images/SpeechRecog_paste_key.PNG)

3. Em **origem de reconhecimento de voz**, escolha uma das origens de reconhecimento de seis voz, enquadram-se em duas categorias principais de entrada:

   * Utilize microfone do seu computador ou um microfone ligado para capturar o reconhecimento de voz.
   * Reproduza um ficheiro de áudio.

   Cada categoria tem três modos de reconhecimento:

    * **Modo de ShortPhrase**: um utterance até 15 segundos de tempo. Como os dados são enviados para o servidor, o cliente receberá vários resultados parciais e um resultado final com várias opções de n melhor.
    * **Modo de LongDictation**: um utterance até dois minutos de tempo. Como os dados são enviados para o servidor, o cliente receberá vários resultados parciais e vários resultados finais, com base no onde o servidor indica o frase pausa.
    * **Deteção de intenção**: O servidor devolve informações estruturadas adicionais sobre o reconhecimento de voz de entrada. Para utilizar a deteção de intenção, terá de formação de um modelo pela primeira vez utilizando [LUIS](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/).

Utilize ficheiros de áudio de exemplo com esta aplicação de exemplo. Localize os ficheiros no repositório de que transferiu com este exemplo sob a pasta de amostras/SpeechRecognitionServiceExample. Estes ficheiros de áudio do exemplo é executado automaticamente se não existem outros ficheiros são escolhidos Quando seleciona **utilize um ficheiro wav para o modo de Shortphrase** ou **utilize um ficheiro wav para o modo de Longdictation** como o reconhecimento de voz de entrada. Atualmente, apenas o formato de áudio WAV é suportado.

![Reconhecimento de voz para a interface de texto](../Images/HelloJones.PNG)

## <a name="samples-explained"></a>Amostras explicadas

### <a name="recognition-events"></a>Eventos de reconhecimento

* **Eventos de resultados parciais**: Este evento é chamado sempre que o serviço de reconhecimento de voz prevê que poderá ser indicar, mesmo antes de terminar a falando (se utilizar `MicrophoneRecognitionClient`) ou de concluir o envio de dados (se utilizar `DataRecognitionClient`).
* **Eventos de erro**: chamado quando o serviço Deteta um erro.
* **Eventos intenção**: chamado nos clientes "WithIntent" (apenas no modo de ShortPhrase) após o final reconhecimento o resultado é analisado para um objetivo JSON structured.
* **Resultar eventos**:
  * No `ShortPhrase` modo, este evento é chamado e devolve os resultados de n melhor depois de concluir falando.
  * No `LongDictation` modo, o processador de eventos é chamado várias vezes, com base na qual o serviço identifica o frase pausa.
  * **Para cada uma das opções de n melhor**, são devolvidas um valor de confiança e algumas formas diferentes de texto reconhecido. Para obter mais informações, consulte [formato de saída](../Concepts.md#output-format).

Processadores de eventos são já indicados no código no formulário de comentários do código.

## <a name="related-topics"></a>Tópicos relacionados

* [Referência da biblioteca do ambiente de trabalho Microsoft Speech](https://cdn.rawgit.com/Microsoft/Cognitive-Speech-STT-Windows/master/docs/SpeechSDK/index.html)
* [Introdução à API de reconhecimento de voz Microsoft em Java no Android](GetStartedJavaAndroid.md)
* [Introdução à API de reconhecimento de voz Microsoft no Objective-C no iOS](Get-Started-ObjectiveC-iOS.md)
* [Introdução à API de reconhecimento de voz Microsoft em JavaScript](GetStartedJSWebsockets.md)
* [Introdução à API de reconhecimento de voz Microsoft através de REST](GetStartedREST.md)
