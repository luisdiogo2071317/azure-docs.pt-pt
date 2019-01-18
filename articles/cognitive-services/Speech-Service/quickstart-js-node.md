---
title: 'Início rápido: Reconhecer a conversão de voz, node. js - serviços de voz'
titleSuffix: Azure Cognitive Services
description: Utilize este guia para criar uma aplicação de consola de voz em texto com o SDK de voz para node. js. Quando terminar, pode utilizar o microfone do seu computador para converter voz em texto em tempo real.
services: cognitive-services
author: fmegen
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: quickstart
ms.date: 1/16/2019
ms.author: fmegen
ms.openlocfilehash: e0ae916687ca32835dd8daf6e5059b8f6eea0ff6
ms.sourcegitcommit: ba9f95cf821c5af8e24425fd8ce6985b998c2982
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/17/2019
ms.locfileid: "54382175"
---
# <a name="quickstart-recognize-speech-with-the-speech-sdk-for-nodejs"></a>Início rápido: Reconhecer a conversão de voz com o SDK de voz para node. js

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Neste artigo, aprenderá como criar um projeto de node. js a utilizar o enlace de JavaScript do SDK de voz dos serviços cognitivos para transcrição de voz em texto.
A aplicação baseia-se no Microsoft [SDK de voz dos serviços cognitivos](https://aka.ms/csspeech/npmpackage).

## <a name="prerequisites"></a>Pré-requisitos

* Uma chave de subscrição do Azure para o serviço de voz. [Obter uma gratuitamente](get-started.md).
* Uma versão atual do [node. js](https://nodejs.org).

## <a name="create-a-new-project"></a>Criar um novo projeto

Criar uma nova pasta e inicializar o projeto.

```sh
npm init -f
```

Isto irá init os arquivos de Package. JSON com valores predefinidos. Provavelmente desejará editar este ficheiro mais tarde.

## <a name="install-the-speech-sdk"></a>Instalar o SDK de voz

Adicione o SDK de voz ao seu projeto de node. js.

```
npm install microsoft-cognitiveservices-speech-sdk
```

Isto transfere e instala a versão mais recente do SDK de voz e os pré-requisitos necessários de npmjs. O SDK será instalado no `node_modules` diretório dentro da pasta do projeto.

## <a name="use-the-speech-sdk"></a>Utilizar o SDK de voz

Crie um novo ficheiro na pasta, com o nome `index.js`, e abra este ficheiro com um editor de texto.

> [!NOTE]
> Tenha em atenção que no node. js o SDK de voz não suporta o microfone ou o tipo de dados de ficheiro. Ambos são suportadas apenas em browsers. Em alternativa, utilize a interface do Stream para o SDK de voz através de `AudioInputStream.createPushStream()` ou `AudioInputStream.createPullStream()`.

Neste exemplo, utilizamos o `PushAudioInputStream` interface.

Adicione o seguinte código JavaScript:

[!code-javascript[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/js-node/index.js#code)]

## <a name="run-the-sample"></a>Executar o exemplo

Para iniciar a aplicação, adaptar `YourSubscriptionKey`, `YourServiceRegion`, e `YourAudioFile.wav` à sua configuração. Em seguida, pode executá-lo ao chamar o seguinte comando:

```sh
node index.js
```

Ele irá acionar um reconhecimento com o nome de ficheiro fornecido e apresente o resultado no console.

Aqui está uma saída de exemplo da execução `index.js` depois de atualizar a chave de subscrição e a utilizar o ficheiro `whatstheweatherlike.wav`.

```json
SpeechRecognitionResult {
  "privResultId": "9E30EEBD41AC4571BB77CF9164441F46",
  "privReason": 3,
  "privText": "What's the weather like?",
  "privDuration": 15900000,
  "privOffset": 300000,
  "privErrorDetails": null,
  "privJson": {
    "RecognitionStatus": "Success",
    "DisplayText": "What's the weather like?",
    "Offset": 300000,
    "Duration": 15900000
  },
  "privProperties": null
}
```

## <a name="install-and-use-the-speech-sdk-with-visual-studio-code"></a>Instalar e utilizar o SDK de voz com o Visual Studio Code

Também pode executar o exemplo do Visual Studio Code. Siga estes passos para instalar, abra e execute o início rápido:

1. Iniciar o Visual Studio Code e clique em "Abrir pasta", em seguida, navegue para a pasta de início rápido

   ![Captura de ecrã do Abrir pasta](media/sdk/qs-js-node-01-open_project.png)

1. Abra um terminal no Visual Studio Code

   ![Captura de ecrã da janela de terminal](media/sdk/qs-js-node-02_open_terminal.png)

1. Execute o npm para instalar as dependências

   ![Captura de ecrã da instalação de npm](media/sdk/qs-js-node-03-npm_install.png)

1. Agora, está pronto para abrir `index.js`e defina um ponto de interrupção

   ![Captura de ecrã do Index com um ponto de interrupção na linha 16](media/sdk/qs-js-node-04-setup_breakpoint.png)

1. Para iniciar a depuração, pressionar F5 ou selecione a depuração/iniciar depuração no menu

   ![Captura de ecrã do debug menu](media/sdk/qs-js-node-05-start_debugging.png)

1. Quando for atingido um ponto de interrupção, pode inspecionar a pilha de chamadas e as variáveis

   ![Captura de ecrã do depurador](media/sdk/qs-js-node-06-hit_breakpoint.png)

1. Qualquer saída será apresentada na janela da consola de depuração

   ![Captura de ecrã da consola de depuração](media/sdk/qs-js-node-07-debug_output.png)

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Explore exemplos de node. js no GitHub](https://aka.ms/csspeech/samples)
