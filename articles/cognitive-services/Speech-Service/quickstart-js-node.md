---
title: 'Início rápido: Reconhecer a conversão de voz em JavaScript no node. js utilizando o SDK do serviço de voz'
titleSuffix: Azure Cognitive Services
description: Aprender a reconhecer a conversão de voz em JavaScript no node. js utilizando o SDK do serviço de voz
services: cognitive-services
author: fmegen
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: quickstart
ms.date: 12/18/2018
ms.author: fmegen
ms.openlocfilehash: 6dc482f8629bf8ca75af5f1643999ae896fa2435
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/19/2018
ms.locfileid: "53609434"
---
# <a name="quickstart-recognize-speech-in-javascript-in-nodejs-using-the-speech-service-sdk"></a>Início rápido: Reconhecer a conversão de voz em JavaScript no node. js utilizando o SDK do serviço de voz

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Neste artigo, aprenderá como criar um projeto de node. js a utilizar o enlace de JavaScript do SDK de voz dos serviços cognitivos para transcrição de voz em texto.
A aplicação baseia-se no Microsoft [SDK de voz dos serviços cognitivos](https://aka.ms/csspeech/npmpackage).

## <a name="prerequisites"></a>Pré-requisitos

* Uma chave de subscrição para o serviço de Voz. Veja [Experimente o Serviço de Voz gratuitamente](get-started.md).
* Uma versão atual do [node. js](https://nodejs.org).

## <a name="create-a-new-project-folder"></a>Crie uma nova pasta de projeto

Crie uma pasta nova e vazia e inicializá-la como um novo projeto de JavaScript e node. js.

```sh
npm init -f
```

Isto irá init os arquivos de Package. JSON com valores predefinidos. Provavelmente desejará editar este ficheiro mais tarde.

## <a name="install-the-speech-sdk-for-javascript-into-that-folder"></a>Instale o SDK de voz para JavaScript dentro dessa pasta

Adicionar o SDK de voz por meio de `npm install microsoft-cognitiveservices-speech-sdk` ao seu projeto de node. js.

Isto irá transferir e instalar a versão mais recente do SDK de voz e os pré-requisitos necessários de npmjs. O SDK será instalado no `node_modules` diretório dentro da pasta do projeto.

## <a name="using-the-speech-sdk"></a>Utilizando o SDK de voz

Crie um novo ficheiro na pasta, com o nome `index.js`, e abra este ficheiro com um editor de texto.

> [!NOTE]
> Tenha em atenção que no node. js o SDK de voz não suporta o microfone ou o tipo de dados de ficheiro. Ambos são suportadas apenas em browsers. Em alternativa, utilize a interface do Stream para o SDK de voz através de `AudioInputStream.createPushStream()` ou `AudioInputStream.createPullStream()`.

Neste exemplo, utilizamos o `PushAudioInputStream` interface.

Adicione o seguinte código JavaScript:

[!code-javascript[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/js-node/index.js#code)]

## <a name="running-the-sample-from-command-line"></a>Execução do exemplo de linha de comandos

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

## <a name="running-the-sample-from-visual-studio-code"></a>Execução do exemplo do Visual Studio Code

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

[!INCLUDE [Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Procure este exemplo na pasta `quickstart/js-node`.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Obter os nossos exemplos](speech-sdk.md#get-the-samples)
