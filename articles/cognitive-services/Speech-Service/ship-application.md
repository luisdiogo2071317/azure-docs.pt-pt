---
title: Serviços cognitivos do Azure, cognitivos dos serviços de API do SDK de voz documentação - tutoriais, referência da API
description: Saiba como criar e desenvolver aplicações com o SDK de voz dos serviços cognitivos
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: wolfma61
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 06/07/2018
ms.author: wolfma
ms.openlocfilehash: df6224208e93f4a332247440933b3a4d11c1bb43
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/27/2018
ms.locfileid: "39283036"
---
# <a name="shipping-an-application"></a>Um aplicativo de envio

Observe a [licença do SDK de voz](license.md), bem como o [notificações de software de terceiros](third-party-notices.md) ao distribuir o SDK de voz dos serviços cognitivos. Além disso, reveja os [declaração de privacidade do Microsoft](https://aka.ms/csspeech/privacy).

Consoante a plataforma, existem dependências diferentes para executar seu aplicativo.

## <a name="windows"></a>Windows

O SDK de voz dos serviços cognitivos é testado no Windows 10 e no Windows Server 2016.

O SDK de voz dos serviços cognitivos requer o [Microsoft Visual C++ Redistributable para Visual Studio 2017](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads) no sistema. Pode baixar os programas de instalação para a versão mais recente do `Microsoft Visual C++ Redistributable for Visual Studio 2017` aqui:

- [Win32](https://aka.ms/vs/15/release/vc_redist.x86.exe)
- [x64](https://aka.ms/vs/15/release/vc_redist.x64.exe)

Se a aplicação estiver a utilizar o código gerenciado, o `.Net Framework 4.6.1` ou posterior é necessário no computador de destino.

Para a entrada do microfone, as bibliotecas de base de dados tem de ser instalado. Essas bibliotecas fazem parte do Windows 10 e Windows Server 2016. É possível utilizar o SDK de voz dessas bibliotecas, desde que microfone não é utilizado como o dispositivo de entrada de áudio.

Os ficheiros necessários do SDK de voz podem ser implementados no mesmo diretório que seu aplicativo. Desta forma, que seu aplicativo pode acessar diretamente as bibliotecas. Certificar-se de que seleciona a versão correta (Win32/x64) correspondentes a sua aplicação.

| Nome | Função
|:-----|:----|
| `Microsoft.CognitiveServices.Speech.core.dll` | Core SDK, necessária para a implementação nativa e gerenciada
| `Microsoft.CognitiveServices.Speech.csharp.bindings.dll` | necessário para a implementação gerida
| `Microsoft.CognitiveServices.Speech.csharp.dll` | necessário para a implementação gerida

## <a name="linux"></a>Linux

Para uma aplicação nativa, terá de enviar a biblioteca do SDK de voz, `libMicrosoft.CognitiveServices.Speech.core.so`.
Certifique-se de selecionar a versão (x86, x64) correspondentes a sua aplicação. Dependendo da versão do Linux, também poderá incluir as seguintes dependências:

* As bibliotecas compartilhadas da biblioteca de C GNU (incluindo a biblioteca de programação de Threads POSIX, `libpthreads`)
* A biblioteca OpenSSL (`libssl.so.1.0.0`)
* A biblioteca de cURL (`libcurl.so.4`)
* A biblioteca partilhada para aplicativos de ALSA (`libasound.so.2`)

No Ubuntu 16.04, por exemplo, as bibliotecas de C do GNU devem já ser instaladas por predefinição. Os três últimos podem ser instalados utilizando estes comandos:

```sh
sudo apt-get update
sudo apt-get install libssl1.0.0 libcurl3 libasound2 wget
```

## <a name="next-steps"></a>Passos Seguintes

* [Obter a subscrição de avaliação de Voz](https://azure.microsoft.com/try/cognitive-services/)
* [Veja como a reconhecer a conversão de voz em c#](quickstart-csharp-dotnet-windows.md)
