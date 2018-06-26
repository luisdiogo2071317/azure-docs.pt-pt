---
title: Início rápido do SDK de reconhecimento de voz para C++ e do Linux | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Exemplos de código e informações de GET para o ajudar a rapidamente começar a utilizar o SDK de reconhecimento de voz com Linux e C++ nos serviços cognitivos.
services: cognitive-services
author: wolfma61
manager: onano
ms.service: cognitive-services
ms.technology: Speech
ms.topic: article
ms.date: 06/07/2018
ms.author: wolfma
ms.openlocfilehash: cee70ba585f93dda3249fc5b39f25fb613b57a45
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/25/2018
ms.locfileid: "36753682"
---
# <a name="quickstart-for-c-and-linux"></a>Início rápido para C++ e Linux

A versão atual do SDK de reconhecimento de voz serviços cognitivos `0.4.0`.

O SDK de reconhecimento de voz serviços cognitivos de Linux está disponível para criação de aplicações de 64 bits e de 32 bits. Os ficheiros necessários podem ser transferidos como um ficheiro tar da https://aka.ms/csspeech/linuxbinary.

> [!NOTE]
> Se estiver à procura de um guia de introdução para C++ e o Windows, aceda [aqui](quickstart-cpp-windows.md).
> Se estiver à procura de um guia de introdução para c# e o Windows, aceda [aqui](quickstart-csharp-windows.md).

[!include[Get a Subscription Key](includes/get-subscription-key.md)]

> [!NOTE]
> Estas instruções partem do princípio de que está a executar no Ubuntu 16.04 num PC (x86 ou x64).
> Uma versão diferente do Ubuntu, ou uma distribuição Linux diferente, terá de adaptar os passos necessários.

## <a name="prerequisites"></a>Pré-requisitos

[!include[Ubuntu Prerequisites](includes/ubuntu1604-prerequisites.md)]

## <a name="getting-the-binary-package"></a>Obter o pacote de binário

[!include[License Notice](includes/license-notice.md)]

1. Escolha um diretório (caminho absoluto) onde pretende colocar os binários do SDK de reconhecimento de voz e nos cabeçalhos.
   Por exemplo, escolha o caminho `speechsdk` sob o diretório raiz:

   ```sh
   export SPEECHSDK_ROOT="$HOME/speechsdk"
   ```

1. Crie o diretório se não existir ainda:

   ```sh
   mkdir -p "$SPEECHSDK_ROOT"
   ```

1. Transferir e extrair o `.tar.gz` arquivo com binários de reconhecimento de voz SDK:

   ```sh
   wget -O SpeechSDK-Linux.tar.gz https://aka.ms/csspeech/linuxbinary
   tar --strip 1 -xzf SpeechSDK-Linux.tar.gz -C "$SPEECHSDK_ROOT"
   ```

1. Valide o conteúdo do diretório de nível superior do pacote extraído:

   ```sh
   ls -l "$SPEECHSDK_ROOT"
   ```

   Deve agora mostrar aviso de terceiros e ficheiros de licença, bem como um `include` diretório para os cabeçalhos e um `lib` diretório de bibliotecas.

   [!include[Linux Binary Archive Content](includes/linuxbinary-content.md)]

## <a name="sample-code"></a>Código de exemplo

O seguinte código reconhece voz em inglês do microfone.
Coloque-o no ficheiro denominado `quickstart-linux.cpp`:

[!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/Linux/quickstart-linux/quickstart-linux.cpp#code)]

Substitua a chave de subscrição no código pelo obteve.

## <a name="building"></a>A criar

> [!NOTE]
> Certifique-se copiar e colar os comandos de compilação abaixo como um _única linha_.

* Execute o seguinte comando para criar a aplicação num x64 máquina:

  ```sh
  g++ quickstart-linux.cpp -o quickstart-linux -I "$SPEECHSDK_ROOT/include/cxx_api" -I "$SPEECHSDK_ROOT/include/c_api" --std=c++14 -lpthread -lMicrosoft.CognitiveServices.Speech.core -L "$SPEECHSDK_ROOT/lib/x64" -l:libssl.so.1.0.0 -l:libcurl.so.4 -l:libasound.so.2
  ```

* Execute o seguinte comando para criar a aplicação num x86 máquina:

  ```sh
  g++ quickstart-linux.cpp -o quickstart-linux -I "$SPEECHSDK_ROOT/include/cxx_api" -I "$SPEECHSDK_ROOT/include/c_api" --std=c++14 -lpthread -lMicrosoft.CognitiveServices.Speech.core -L "$SPEECHSDK_ROOT/lib/x86" -l:libssl.so.1.0.0 -l:libcurl.so.4 -l:libasound.so.2
  ```

## <a name="running"></a>A executar

Para executar a aplicação, terá de configurar o caminho de biblioteca do carregador para apontar para a biblioteca do SDK de reconhecimento de voz.

* Num x64 máquina, execute:

  ```sh
  export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:$SPEECHSDK_ROOT/lib/x64"
  ```

* Num x86 máquina, execute:

  ```sh
  export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:$SPEECHSDK_ROOT/lib/x86"
  ```

Execute a aplicação da seguinte forma:

```sh
./quickstart-linux
```

Se tudo correr bem, deverá ver um resultado semelhante a isto:

```text
Say something...
We recognized: What's the weather
```

## <a name="downloading-the-sample"></a>Transferir o exemplo

Para o conjunto mais recente de exemplos, consulte o [repositório cognitivos GitHub de exemplo do SDK de reconhecimento de voz de serviços](https://aka.ms/csspeech/samples).

## <a name="next-steps"></a>Passos Seguintes

* Visite o [página amostras](samples.md) exemplos adicionais.
