---
title: Início rápido do SDK de reconhecimento de voz para C++ e o Windows | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Exemplos de código e informações de GET para o ajudar a rapidamente começar a utilizar o SDK de reconhecimento de voz com o Windows e C++ nos serviços cognitivos.
services: cognitive-services
author: wolfma61
manager: onano
ms.service: cognitive-services
ms.technology: Speech
ms.topic: article
ms.date: 06/07/2018
ms.author: wolfma
ms.openlocfilehash: 37228ca2f887f0993f707a2b10a2b5dcd888b1b9
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/25/2018
ms.locfileid: "36753688"
---
# <a name="quickstart-for-c-and-windows"></a>Início rápido para C++ e o Windows

A versão atual do SDK de reconhecimento de voz serviços cognitivos `0.4.0`.

Iremos descrevem como criar uma aplicação de consola baseada na C++ para ambiente de trabalho do Windows que utiliza o SDK de reconhecimento de voz.
A aplicação é baseada no [Microsoft cognitivos serviços SDK NuGet pacote](https://www.nuget.org/packages/Microsoft.CognitiveServices.Speech) e Microsoft VisualStudio 2017.

> [!NOTE]
> Se estiver à procura de um guia de introdução para C++ e Linux, vá [aqui](quickstart-cpp-linux.md).<br>
> Se estiver à procura de um guia de introdução para c# e o Windows, aceda [aqui](quickstart-csharp-windows.md).

> [!NOTE]
> Este guia de introdução requer um computador com um microfone de trabalho.<br>
> Para uma amostra que reconhece o reconhecimento de voz de um ficheiro de entrada de áudio determinado consulte o [exemplo](speech-to-text-sample.md#speech-recognition-from-a-file).

> [!NOTE]
> Certifique-se de que a instalação do Visual Studio inclui o **desenvolvimento de ambiente de trabalho com o C++** carga de trabalho.
> Se não tiver a certeza, utilize estes passos para verificar e corrigir: no Visual Studio 2017, selecione **ferramentas** \> **obter ferramentas e funcionalidades** e confirmar o controlo de conta de utilizador prompt escolhendo **Sim**.
> No **cargas de trabalho** separador, se **desenvolvimento de ambiente de trabalho com o C++** não tem uma caixa de verificação do conjunto junto ao mesmo, defina-o e clique em **modificar** ao guardar as alterações.

[!include[Get a Subscription Key](includes/get-subscription-key.md)]

## <a name="creating-an-empty-console-application-project"></a>Criar um projeto de aplicação de consola vazia

No Visual Studio 2017, crie um novo Visual C++ Windows ambiente de trabalho Windows aplicação de consola com o nome "CppHelloSpeech":

![Criar aplicação de consola do Windows de ambiente de trabalho de Windows do Visual C++](media/sdk/speechsdk-05-vs-cpp-new-console-app.png)

Se estiver a executar uma instalação do Windows de 64 bits, opcionalmente, mudar a sua plataforma de compilação para `x64`:

![Mude a plataforma de compilação para x64](media/sdk/speechsdk-07-vs-cpp-switch-to-x64.png)

## <a name="install-and-reference-the-speech-sdk-nuget-package"></a>Instalar e façam referência ao pacote NuGet do SDK de reconhecimento de voz

> [!NOTE]
> Certifique-se de que o Gestor de pacotes NuGet está ativado para a sua instalação do Visual Studio 2017.
> No Visual Studio 2017, selecione **ferramentas** \> **obter ferramentas e funcionalidades** e confirmar o controlo de conta de utilizador prompt escolhendo **Sim**. Em seguida, selecione o **componentes individuais** separador e procure **Gestor de pacotes NuGet** em **Code ferramentas**.
> Se a caixa de verificação à esquerda não está definida, certifique-se defini-lo e clique em **modificar** ao guardar as alterações.
>
> ![Ativar o Gestor de pacotes NuGet no Visual Studio ](media/sdk/speechsdk-05-vs-enable-nuget-package-manager.png)

No Explorador de soluções, clique com o botão direito na solução e clique em **gerir pacotes NuGet para solução**.

![Contexto gerir pacotes NuGet para solução](media/sdk/speechsdk-09-vs-cpp-manage-nuget-packages.png)

No canto superior direito, no **origem do pacote** campo, escolha "Nuget.org".
Do **procurar** separador, procure o pacote "Microsoft.CognitiveServices.Speech", selecione-o e verifique o **projeto** e **CppHelloSpeech** caixas à direita, e Selecione **instalar** instalá-la para o projeto de CppHelloSpeech.

![Instalar o pacote NuGet de Microsoft.CognitiveServices.Speech](media/sdk/speechsdk-11-vs-cpp-manage-nuget-install.png)

No ecrã de licença que aparece, aceite a licença:

![Aceitar a licença](media/sdk/speechsdk-12-vs-cpp-manage-nuget-license.png)

## <a name="add-the-sample-code"></a>Adicione o código de exemplo

Substitua o código de arranque predefinido pelo seguinte:

[!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/Windows/quickstart-cpp/CppHelloSpeech.cpp#code)]

> [!IMPORTANT]
> Substitua um que obteve a chave de subscrição. <br>
> Substitua a região com a região do [API de REST do serviço de reconhecimento de voz](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/rest-apis), por exemplo, substitua 'westus'.

![Adicionar a chave de subscrição](media/sdk/sub-key-recognize-speech-cpp.png)

## <a name="build-and-run-the-sample"></a>Criar e executar o exemplo

O código deve agora compilar sem erros:

![Compilação efetuada com êxito](media/sdk/speechsdk-16-vs-cpp-build.png)

Inicie o programa sob o depurador com o botão de início ou utilizando o atalho do teclado F5:

![Inicie a aplicação para depuração](media/sdk/speechsdk-17-vs-cpp-f5.png)

Uma janela da consola deve aparecer, que lhe pede para dizer algo (em inglês).
O resultado do reconhecimento será apresentado no ecrã.

![Resultado da consola depois de reconhecimento com êxito](media/sdk/speechsdk-18-vs-cpp-console-output-release.png)

## <a name="downloading-the-sample"></a>Transferir o exemplo

Para o conjunto mais recente de exemplos, consulte o [repositório cognitivos GitHub de exemplo do SDK de reconhecimento de voz de serviços](https://aka.ms/csspeech/samples).

## <a name="next-steps"></a>Passos Seguintes

* Visite o [página amostras](samples.md) exemplos adicionais.
