---
title: 'Início rápido: Reconhecer voz em JavaScript num navegador com o SDK de voz dos serviços cognitivos'
titleSuffix: Microsoft Cognitive Services
description: Aprender a reconhecer a voz em JavaScript num navegador com o SDK de voz dos serviços cognitivos
services: cognitive-services
author: fmegen
ms.service: cognitive-services
ms.component: Speech
ms.topic: article
ms.date: 09/24/2018
ms.author: fmegen
ms.openlocfilehash: 75dcda643741e3aeb1238f82128e4c5b058be840
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/09/2018
ms.locfileid: "48883663"
---
# <a name="quickstart-recognize-speech-in-javascript-in-a-browser-using-the-cognitive-services-speech-sdk"></a>Início rápido: Reconhecer voz em JavaScript num navegador com o SDK de voz dos serviços cognitivos

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Neste artigo, aprenderá como criar um Web site usando a ligação de JavaScript do SDK de voz dos serviços cognitivos para transcrição de voz em texto.
A aplicação baseia-se o SDK de voz de serviços cognitivos da Microsoft ([transfira a versão 1.0.0](https://aka.ms/csspeech/jsbrowserpackage)).

## <a name="prerequisites"></a>Pré-requisitos

* Uma chave de subscrição para o serviço de voz. Ver [experimentar gratuitamente o serviço de voz](get-started.md).
* Um PC ou Mac, com um microfone de trabalho.
* Um editor de texto.
* Uma versão atual do Chrome ou o Microsoft Edge.
* Opcionalmente, um servidor web que suporta o alojamento de PHP scripts.

## <a name="create-a-new-website-folder"></a>Crie uma nova pasta do Web site

Crie uma pasta nova e vazia. No caso de pretender alojar o exemplo num servidor web, certifique-se de que o servidor web pode aceder à pasta.

## <a name="unpack-the-speech-sdk-for-javascript-into-that-folder"></a>Descompactar o SDK de voz para JavaScript dentro dessa pasta

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

Transferir o SDK de voz como uma [pacote. zip](https://aka.ms/csspeech/jsbrowserpackage) e Descompacte-o para a pasta recentemente criada. Isso deve resultar em dois arquivos, a ser descompactados, ou seja, `microsoft.cognitiveservices.speech.sdk.bundle.js` e `microsoft.cognitiveservices.speech.sdk.bundle.js.map`.
O último arquivo é opcional e utilizados para ajudar a depuração no código do SDK, se necessário.

## <a name="create-an-indexhtml-page"></a>Criar uma página Index. HTML

Crie um novo ficheiro na pasta, com o nome `index.html` e abrir este ficheiro com um editor de texto.

1. Crie a seguinte estrutura HTML:

  ```html
  <html>
  <head>
      <title>Microsoft Cognitive Service Speech SDK JavaScript Quickstart</title>
  </head>
  <body>
    <!-- UI code goes here -->

    <!-- SDK reference goes here -->

    <!-- Optional authorization token request goes here -->

    <!-- Sample code goes here -->
  </body>
  </html>
  ```

1. Adicione o seguinte código de interface do Usuário ao seu ficheiro, abaixo do primeiro comentário:

  [!code-html[](~/samples-cognitive-services-speech-sdk/quickstart/js-browser/index.html#uidiv)]

1. Adicionar uma referência para o SDK de voz

  [!code-html[](~/samples-cognitive-services-speech-sdk/quickstart/js-browser/index.html#speechsdkref)]

1. Conectar manipuladores para o botão de reconhecimento, o resultado do reconhecimento e a subscrição campos relacionados definidos pelo código da interface do Usuário:

  [!code-html[](~/samples-cognitive-services-speech-sdk/quickstart/js-browser/index.html#quickstartcode)]

## <a name="create-the-token-source-optional"></a>Criar a origem do token (opcional)

No caso de pretender alojar a página da web num servidor web, opcionalmente, pode fornecer uma origem de token para a sua aplicação de demonstração.
Dessa forma, a chave de subscrição nunca deixar seu servidor, permitindo que os utilizadores a utilizar as capacidades de voz sem introduzir qualquer código de autorização propriamente ditas.

1. Crie um novo ficheiro com o nome `token.php`. Neste exemplo partimos do princípio de que seu servidor web oferece suporte a linguagem de script do PHP. Introduza o seguinte código:

  [!code-php[](~/samples-cognitive-services-speech-sdk/quickstart/js-browser/token.php)]

1. Editar o `index.html` de ficheiros e adicione o seguinte código ao seu ficheiro:

  [!code-html[](~/samples-cognitive-services-speech-sdk/quickstart/js-browser/index.html#authorizationfunction)]

> [!NOTE]
> Tokens de autorização só tem uma duração limitada.
> Este exemplo simplificado não mostra como atualizar automaticamente os tokens de autorização. Como um utilizador, pode recarregue a página ou pressione F5 para atualizar manualmente.

## <a name="build-and-run-the-sample-locally"></a>Criar e executar o exemplo localmente

Para iniciar a aplicação, faça duplo clique no ficheiro Index. HTML ou abra Index. HTML com o browser favorito. Ele apresentará uma GUI simple, permitindo-lhe introduzir a chave de subscrição e [região](regions.md) e acionar um reconhecimento de utilizar o microfone.

## <a name="build-and-run-the-sample-via-a-web-server"></a>Criar e executar o exemplo através de um servidor web

Inicie a sua aplicação, abra o browser favorito e apontá-lo para o URL público que alojam a pasta no, introduza o seu [região](regions.md)e acionar um reconhecimento de utilizar o microfone. Se configurado, ele irá adquirir um token da sua origem de token.

[!INCLUDE [Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Procure este exemplo na pasta `quickstart/js-browser`.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Obtenha os nossos exemplos](speech-sdk.md#get-the-samples)
