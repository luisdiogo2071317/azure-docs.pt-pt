---
title: 'Início rápido: Reconhecer voz em C++ no Linux com o SDK de voz dos serviços cognitivos'
titleSuffix: Microsoft Cognitive Services
description: Aprender a reconhecer voz em C++ no Linux com o SDK de voz dos serviços cognitivos
services: cognitive-services
author: wolfma61
ms.service: cognitive-services
ms.technology: Speech
ms.topic: article
ms.date: 07/16/2018
ms.author: wolfma
ms.openlocfilehash: b8bc24123fe75e876f607b07e37423ae68d92ee4
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/20/2018
ms.locfileid: "41987538"
---
# <a name="quickstart-recognize-speech-in-c-on-linux-using-the-speech-sdk"></a>Início rápido: Reconhecer voz em C++ no Linux com o SDK de voz

[!include[Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Neste artigo, saiba como criar uma aplicação de consola C++ no Linux (Ubuntu 16.04) com o SDK de voz dos serviços cognitivos para transcrição de voz em texto.

## <a name="prerequisites"></a>Pré-requisitos

* Uma chave de subscrição para o serviço de voz. Ver [experimentar gratuitamente o serviço de voz](get-started.md).
* Um PC de 16.04 Ubuntu com um microfone de trabalho.
* Para instalar pacotes necessários para criar e executar este exemplo, execute o seguinte:

  ```sh
  sudo apt-get update
  sudo apt-get install build-essential libssl1.0.0 libcurl3 libasound2 wget
  ```

## <a name="get-the-speech-sdk"></a>Obtenha o SDK de voz

[!include[License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

A versão atual do SDK de voz dos serviços cognitivos é `0.6.0`.

O SDK de voz dos serviços cognitivos para Linux está disponível para criação de aplicativos de 64 bits e 32 bits.
Os ficheiros necessários podem ser baixados como um ficheiro de destino da https://aka.ms/csspeech/linuxbinary.
Transfira e instale o SDK da seguinte forma:

1. Escolha um diretório (caminho absoluto) onde pretende colocar os binários do SDK de voz e cabeçalhos.
   Por exemplo, escolha o caminho `speechsdk` sob o diretório raiz:

   ```sh
   export SPEECHSDK_ROOT="$HOME/speechsdk"
   ```

1. Crie o diretório se não existir ainda:

   ```sh
   mkdir -p "$SPEECHSDK_ROOT"
   ```

1. Baixe e extraia o `.tar.gz` arquivo com os binários do SDK de voz:

   ```sh
   wget -O SpeechSDK-Linux.tar.gz https://aka.ms/csspeech/linuxbinary
   tar --strip 1 -xzf SpeechSDK-Linux.tar.gz -C "$SPEECHSDK_ROOT"
   ```

1. Valide o conteúdo do diretório de nível superior do pacote extraído:

   ```sh
   ls -l "$SPEECHSDK_ROOT"
   ```

   Deve mostrar aviso de terceiros e arquivos de licença, bem como um `include` diretório para cabeçalhos e um `lib` diretório para bibliotecas.

   [!include[Linux Binary Archive Content](../../../includes/cognitive-services-speech-service-linuxbinary-content.md)]

## <a name="add-the-sample-code"></a>Adicione o código de exemplo

1. Adicione o seguinte código para um ficheiro denominado `helloworld.cpp`:

  [!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/cpp-linux/helloworld.cpp#code)]

1. Substitua a cadeia de caracteres `YourSubscriptionKey` com a sua chave de subscrição.

1. Substitua a cadeia de caracteres `YourServiceRegion` com o [região](regions.md) associados à subscrição (por exemplo, `westus` para a subscrição de avaliação gratuita).

## <a name="building"></a>A criar

> [!NOTE]
> Certifique-se de copiar e colar os comandos de compilação abaixo como uma _uma linha_.

* Num **x64** máquina, execute o seguinte comando para criar a aplicação:

  ```sh
  g++ helloworld.cpp -o helloworld -I "$SPEECHSDK_ROOT/include/cxx_api" -I "$SPEECHSDK_ROOT/include/c_api" --std=c++14 -lpthread -lMicrosoft.CognitiveServices.Speech.core -L "$SPEECHSDK_ROOT/lib/x64" -l:libssl.so.1.0.0 -l:libcurl.so.4 -l:libasound.so.2
  ```

* Num **x86** máquina, execute o seguinte comando para criar a aplicação:

  ```sh
  g++ helloworld.cpp -o helloworld -I "$SPEECHSDK_ROOT/include/cxx_api" -I "$SPEECHSDK_ROOT/include/c_api" --std=c++14 -lpthread -lMicrosoft.CognitiveServices.Speech.core -L "$SPEECHSDK_ROOT/lib/x86" -l:libssl.so.1.0.0 -l:libcurl.so.4 -l:libasound.so.2
  ```

## <a name="run-the-sample"></a>Executar o exemplo

1. Configure a configurar o caminho de biblioteca do carregador para apontar para a biblioteca do SDK de voz.

   * Num **x64** máquina, execute:

     ```sh
     export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:$SPEECHSDK_ROOT/lib/x64"
     ```

   * Num **x86** máquina, execute:

     ```sh
     export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:$SPEECHSDK_ROOT/lib/x86"
     ```

1. Execute a aplicação da seguinte forma:

   ```sh
   ./helloworld
   ```

1. Deverá ver um resultado semelhante ao seguinte:

   ```text
   Say something...
   We recognized: What's the weather
   ```

[!include[Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Procure este exemplo no `quickstart/cpp-linux` pasta.

## <a name="next-steps"></a>Passos Seguintes

* [Obtenha os nossos exemplos](speech-sdk.md#get-the-samples)
