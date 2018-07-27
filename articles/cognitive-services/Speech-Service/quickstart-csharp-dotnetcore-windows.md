---
title: 'Início rápido: Reconhecer voz em c# no .NET Core no Windows utilizando o SDK de voz dos serviços cognitivos'
titleSuffix: Microsoft Cognitive Services
description: Aprender a reconhecer a conversão de voz em c# no .NET Core no Windows utilizando o SDK de voz dos serviços cognitivos
services: cognitive-services
author: wolfma61
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 07/16/2018
ms.author: wolfma
ms.openlocfilehash: f66917ed49393988a378de0a55525105c5d19b7c
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/27/2018
ms.locfileid: "39282379"
---
# <a name="quickstart-recognize-speech-in-c-under-net-core-on-windows-using-the-speech-sdk"></a>Início rápido: Reconhecer voz em c# no .NET Core no Windows utilizando o SDK de voz

Neste artigo, saiba como criar uma aplicação de consola c# para .NET Core no Windows utilizando o SDK de voz dos serviços cognitivos para transcrição de voz em texto.
A aplicação baseia-se com o [Microsoft Cognitive Services voz SDK do pacote NuGet](https://aka.ms/csspeech/nuget) e Microsoft Visual Studio 2017.

> [!NOTE]
> .NET core é uma plataforma de .NET de código-fonte aberto e entre plataformas implementando o [.NET Standard](https://docs.microsoft.com/dotnet/standard/net-standard) especificação.

## <a name="prerequisites"></a>Pré-requisitos

* Uma chave de subscrição para o serviço de voz. Ver [experimentar gratuitamente o serviço de voz](get-started.md).
* Um PC Windows com um microfone de trabalho.
* [Microsoft Visual Studio 2017](https://www.visualstudio.com/), Community Edition ou superior.
* O **desenvolvimento de Multiplataforma do .NET Core** carga de trabalho no Visual Studio. Pode ativá-la no **ferramentas** \> **obter ferramentas e funcionalidades**.

  ![Ativar o desenvolvimento de Multiplataforma do .NET Core](media/sdk/vs-enable-net-core-workload.png)

## <a name="create-a-visual-studio-project"></a>Criar um projeto do Visual Studio

1. No Visual Studio 2017, crie um novo Visual c# .NET Core aplicação de consola. Na **novo projeto** caixa de diálogo, no painel à esquerda, expanda **instalado** \> **Visual C#** \> **.NET Core**e, em seguida, selecione **aplicação de consola (.NET Core)**. Para o nome do projeto, introduza *helloworld*.

    ![Criar Visual C# aplicação de consola (.NET Core)](media/sdk/qs-csharp-dotnetcore-windows-01-new-console-app.png "criar Visual C# aplicação de consola (.NET Core)")

1. Instalar e fazer referência a [pacote NuGet do SDK de voz](https://aka.ms/csspeech/nuget). No Solution Explorer, clique com o botão direito a solução e selecione **gerir pacotes NuGet para solução**.

    ![Contexto gerir pacotes NuGet para solução](media/sdk/qs-csharp-dotnetcore-windows-02-manage-nuget-packages.png "gerir pacotes NuGet para solução")

1. No canto superior direito, no **origem do pacote** campo, selecione **Nuget.org**. Procure e instale o `Microsoft.CognitiveServices.Speech` empacotar e instalá-lo para o **helloworld** projeto.

    ![Instalar o pacote de NuGet Microsoft.CognitiveServices.Speech](media/sdk/qs-csharp-dotnetcore-windows-03-nuget-install-0.5.0.png "pacote Nuget instalar")

1. Aceite a licença na caixa de diálogo que aparece.

    ![Aceitar a licença](media/sdk/qs-csharp-dotnetcore-windows-04-nuget-license.png "aceitar a licença")

## <a name="add-the-sample-code"></a>Adicione o código de exemplo

1. Abra `Program.cs` no Visual Studio do projeto e substitua todo o código neste ficheiro pelo seguinte.

    [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-dotnetcore-windows/helloworld/Program.cs#code)]

1. Substitua a cadeia de caracteres `YourSubscriptionKey` com a sua chave de subscrição.

1. Substitua a cadeia de caracteres `YourServiceRegion` com o [região](regions.md) associados à subscrição (por exemplo, `westus` para a subscrição de avaliação gratuita).

1. Guarde as alterações ao projeto.

## <a name="build-and-run-the-sample"></a>Criar e executar o exemplo

1. Crie a aplicação. Na barra de menus, escolha **crie** > **compilar solução**. O código será compilado sem erros agora.

    ![Compilação bem-sucedida](media/sdk/qs-csharp-dotnetcore-windows-05-build.png "compilação bem-sucedida")

1. Inicie a aplicação. Na barra de menus, escolha **depurar** > **iniciar depuração**, ou prima **F5**.

    ![Iniciar a aplicação em depuração](media/sdk/qs-csharp-dotnetcore-windows-06-start-debugging.png "iniciar a aplicação para depuração")

1. É apresentada uma janela de console, solicitando a dizer algo (em inglês). O texto reconhecido, em seguida, é apresentada na janela da mesma.

    ![Saída da consola após o reconhecimento bem-sucedido](media/sdk/qs-csharp-dotnetcore-windows-07-console-output.png "saída da consola após o reconhecimento bem-sucedido")

[!include[Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Procure este exemplo no `quickstart/csharp-dotnetcore-windows` pasta.

## <a name="next-steps"></a>Passos Seguintes

- [Tradução de voz](how-to-translate-speech.md)
- [Personalizar modelos acústicos](how-to-customize-acoustic-models.md)
- [Personalizar modelos de linguagem](how-to-customize-language-model.md)
