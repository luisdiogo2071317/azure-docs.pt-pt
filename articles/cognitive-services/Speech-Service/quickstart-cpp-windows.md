---
title: 'Início rápido: Reconhecer voz em C++ no ambiente de trabalho do Windows utilizando o SDK de voz dos serviços cognitivos'
titleSuffix: Microsoft Cognitive Services
description: Aprender a reconhecer voz em C++ no ambiente de trabalho do Windows utilizando o SDK de voz dos serviços cognitivos
services: cognitive-services
author: wolfma61
ms.service: cognitive-services
ms.technology: Speech
ms.topic: article
ms.date: 07/16/2018
ms.author: wolfma
ms.openlocfilehash: 7c93f9f450cc2d244a0d716401171cde02abf70b
ms.sourcegitcommit: 7ad9db3d5f5fd35cfaa9f0735e8c0187b9c32ab1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/27/2018
ms.locfileid: "39324576"
---
# <a name="quickstart-recognize-speech-in-c-on-windows-desktop-using-the-speech-sdk"></a>Início rápido: Reconhecer voz em C++ no ambiente de trabalho do Windows utilizando o SDK de voz

[!include[Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Nós descrevemos como criar uma aplicação de consola baseada em C++ para a área de trabalho do Windows que utiliza o SDK de voz.
O aplicativo se baseia a [Microsoft Cognitive Services voz SDK do pacote NuGet](https://aka.ms/csspeech/nuget) e Microsoft Visual Studio 2017.

## <a name="prerequisites"></a>Pré-requisitos

* Uma chave de subscrição para o serviço de voz. Ver [experimentar gratuitamente o serviço de voz](get-started.md).
* Um PC Windows com um microfone de trabalho.
* [Microsoft Visual Studio 2017](https://www.visualstudio.com/), Community Edition ou superior.
* O **desenvolvimento com C++** carga de trabalho no Visual Studio e o **Gestor de pacotes NuGet** componente no Visual Studio.
  Pode ativar nas **ferramentas** \> **obter ferramentas e funcionalidades**, no **cargas de trabalho** e **componentes individuais** separadores , respectivamente:

  ![Permitem a programação de área de trabalho com carga de trabalho do C++](media/sdk/vs-enable-cpp-workload.png)

  ![Ativar o Gestor de pacotes de NuGet no Visual Studio ](media/sdk/vs-enable-nuget-package-manager.png)

## <a name="create-a-visual-studio-project"></a>Criar um projeto do Visual Studio

No Visual Studio 2017, crie um novo Visual C++ Windows Desktop Windows aplicativo de Console. Na **novo projeto** caixa de diálogo, no painel à esquerda, expanda **instalado** \> **Visual C++** \> **doambientedetrabalhodoWindows** e, em seguida, selecione **aplicação de consola do Windows**. Para o nome do projeto, introduza *helloworld*.

![Criar aplicação de consola do Windows Desktop de Windows do Visual C++](media/sdk/qs-cpp-windows-01-new-console-app.png)

Se estiver executando numa instalação do Windows de 64 bits, opcionalmente, mude sua plataforma de compilação para `x64`:

![Mude a plataforma de compilação para x64](media/sdk/qs-cpp-windows-02-switch-to-x64.png)

## <a name="install-and-reference-the-speech-sdk-nuget-package"></a>Instalar e fazem referência ao pacote NuGet do SDK de voz

No Solution Explorer, clique com o botão direito a solução e clique em **gerir pacotes NuGet para solução**.

![Contexto gerir pacotes NuGet para solução](media/sdk/qs-cpp-windows-03-manage-nuget-packages.png)

No canto superior direito, no **origem do pacote** campo, escolha "Nuget.org".
Do **navegue** separador, procure o pacote "Microsoft.CognitiveServices.Speech", selecioná-lo e verificar o **projeto** e **helloworld** caixas no lado direito e selecione **Instalar** para instalá-lo para o projeto helloworld.

> [!NOTE]
> A versão atual do SDK de voz dos serviços cognitivos é `0.5.0`.

![Instalar o pacote de NuGet Microsoft.CognitiveServices.Speech](media/sdk/qs-cpp-windows-04-nuget-install-0.5.0.png)

No ecrã de licença que aparece, aceite a licença:

![Aceitar a licença](media/sdk/qs-cpp-windows-05-nuget-license.png)

## <a name="add-the-sample-code"></a>Adicione o código de exemplo

1. Substitua o código de inicialização padrão pelo seguinte:

   [!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/cpp-windows/helloworld/helloworld.cpp#code)]

1. Substitua a cadeia de caracteres `YourSubscriptionKey` com a sua chave de subscrição.

1. Substitua a cadeia de caracteres `YourServiceRegion` com o [região](regions.md) associados à subscrição (por exemplo, `westus` para a subscrição de avaliação gratuita).

1. Guarde as alterações ao projeto.

## <a name="build-and-run-the-sample"></a>Criar e executar o exemplo

1. Crie a aplicação. Na barra de menus, selecione **crie** > **compilar solução**. O código será compilado sem erros agora:

   ![Compilação bem-sucedida](media/sdk/qs-cpp-windows-06-build.png)

1. Inicie a aplicação. Na barra de menus, selecione **depurar** > **iniciar depuração**, ou prima **F5**.

   ![Inicie a aplicação para depuração](media/sdk/qs-cpp-windows-07-start-debugging.png)

1. Uma janela do console será exibida, solicitando a dizer algo (em inglês).
   O resultado desse reconhecimento será apresentado no ecrã.

   ![Resultado da consola após o reconhecimento bem-sucedido](media/sdk/qs-cpp-windows-08-console-output-release.png)

[!include[Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Procure este exemplo no `quickstart/cpp-windows` pasta.

## <a name="next-steps"></a>Passos Seguintes

* [Obtenha os nossos exemplos](speech-sdk.md#get-the-samples)
