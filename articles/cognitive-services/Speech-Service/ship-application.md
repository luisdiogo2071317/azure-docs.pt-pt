---
title: Serviços cognitivos do Azure, cognitivos dos serviços de voz API do SDK documentação - tutoriais, referência da API | Microsoft Docs
description: Saiba como criar e a desenvolver aplicações com o SDK de reconhecimento de voz serviços cognitivos
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: wolfma61
manager: onano
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 06/07/2018
ms.author: wolfma
ms.openlocfilehash: d410dda09fdd30181b633c454b1d44610b10c472
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/07/2018
ms.locfileid: "35356239"
---
# <a name="shipping-an-application"></a>Expedir uma aplicação

Observar o [licença do SDK de reconhecimento de voz](license.md), bem como o [avisos de software de terceiros](third-party-notices.md) ao distribuir o SDK dos serviços de reconhecimento de voz cognitivos. Além disso, reveja o [declaração de privacidade do Microsoft](https://aka.ms/csspeech/privacy).

Consoante a plataforma, diferentes dependências existem para executar a aplicação.

## <a name="windows"></a>Windows

O SDK dos serviços de reconhecimento de voz cognitivos é testado no Windows 10 e no Windows Server 2016.

O SDK de reconhecimento de voz serviços cognitivos requer o [Microsoft Visual C++ Redistributable for Visual Studio 2017](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads) no sistema. Pode transferir programas de instalação para a versão mais recente para o `Microsoft Visual C++ Redistributable for Visual Studio 2017` aqui:

- [Win32](https://aka.ms/vs/15/release/vc_redist.x86.exe)
- [x64](https://aka.ms/vs/15/release/vc_redist.x64.exe)

Se a aplicação estiver a utilizar o código gerido, o `.Net Framework 4.6.1` ou posterior é necessária no computador de destino.

Para a entrada microfone, as bibliotecas do Media Foundation tem de ser instalado. Estas bibliotecas fazem parte do Windows 10 e Windows Server 2016. É possível utilizar o SDK de reconhecimento de voz sem estas bibliotecas, desde que microfone não é utilizado como o dispositivo de entrada de áudio.

Os ficheiros necessários do SDK de reconhecimento de voz podem ser implementados no mesmo diretório que a aplicação. Desta forma, que a aplicação pode aceder diretamente às bibliotecas. Certifique-se de que seleciona a versão correta (Win32/x64) correspondentes à sua aplicação.

| Nome | Função
|:-----|:----|
| `Microsoft.CognitiveServices.Speech.core.dll` | Core SDK, necessária para a implementação nativa e gerida
| `Microsoft.CognitiveServices.Speech.csharp.bindings.dll` | necessário para a implementação gerida
| `Microsoft.CognitiveServices.Speech.csharp.dll` | necessário para a implementação gerida

## <a name="linux"></a>Linux

Para uma aplicação nativa, terá de enviar a biblioteca do SDK de reconhecimento de voz, `libMicrosoft.CognitiveServices.Speech.core.so`.
Certifique-se de que seleciona a versão (x86, x64) correspondentes à sua aplicação. Consoante a versão do Linux, também poderá ter de incluir as seguintes dependências:

* As bibliotecas partilhadas da biblioteca de C GNU (incluindo a biblioteca de programação de Threads POSIX `libpthreads`)
* A biblioteca de OpenSSL (`libssl.so.1.0.0`)
* A biblioteca de cURL (`libcurl.so.4`)
* A biblioteca partilhada para aplicações de ALSA (`libasound.so.2`)

No Ubuntu 16.04, por exemplo, as bibliotecas de GNU C devem ser instaladas por predefinição. A última três podem ser instalado utilizando estes comandos:

```sh
sudo apt-get update
sudo apt-get install libssl1.0.0 libcurl3 libasound2 wget
```

## <a name="next-steps"></a>Passos Seguintes

* [Obter a sua subscrição de avaliação de reconhecimento de voz](https://azure.microsoft.com/try/cognitive-services/)
* [Ver como reconhecer voz em c#](quickstart-csharp-windows.md)
