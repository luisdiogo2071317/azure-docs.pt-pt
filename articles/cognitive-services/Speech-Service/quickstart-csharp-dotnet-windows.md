---
title: 'Início rápido: Reconhecer voz em c# no .NET Framework no Windows utilizando o SDK de voz dos serviços cognitivos | Documentos da Microsoft'
titleSuffix: Microsoft Cognitive Services
description: Aprender a reconhecer a conversão de voz em c# no .NET Framework no Windows utilizando o SDK de voz dos serviços cognitivos
services: cognitive-services
author: wolfma61
manager: onano
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 07/16/2018
ms.author: wolfma
ms.openlocfilehash: 07f22345d2bf8c481091652d9faa0bdd10205fdb
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/17/2018
ms.locfileid: "39072590"
---
# <a name="quickstart-recognize-speech-in-c-under-net-framework-on-windows-using-the-speech-sdk"></a>Início rápido: Reconhecer voz em c# no .NET Framework no Windows utilizando o SDK de voz

Neste artigo, saiba como criar uma aplicação de consola c# para .NET Framework no Windows utilizando o SDK de voz dos serviços cognitivos para transcrição de voz em texto.
O aplicativo se baseia a [Microsoft Cognitive Services voz SDK do pacote NuGet](https://aka.ms/csspeech/nuget) e Microsoft Visual Studio 2017.

## <a name="prerequisites"></a>Pré-requisitos

* Uma chave de subscrição para o serviço de voz. Ver [experimentar gratuitamente o serviço de voz](get-started.md).
* Um PC Windows com um microfone de trabalho.
* Visual Studio 2017, os Community Edition ou superior.
* O **desenvolvimento no ambiente de trabalho .NET** carga de trabalho no Visual Studio. Pode ativá-la no **ferramentas** \> **obter ferramentas e funcionalidades**.

  ![Ativar o desenvolvimento de área de trabalho .NET](media/sdk/vs-enable-net-desktop-workload.png)

  ![Ativar o desenvolvimento de Multiplataforma do .NET Core](media/sdk/vs-enable-net-desktop-workload.png)

## <a name="create-a-visual-studio-project"></a>Criar um projeto do Visual Studio

1. No Visual Studio 2017, crie um novo Visual C# aplicação de consola. Na **novo projeto** caixa de diálogo, no painel à esquerda, expanda **instalado** \> **Visual C#** \> **doambientedetrabalhodoWindows** e, em seguida, selecione **aplicação de consola (.NET Framework)**. Para o nome do projeto, introduza *helloworld*.

    ![Criar Visual C# aplicação de consola (.NET Framework)](media/sdk/qs-csharp-dotnet-windows-01-new-console-app.png "criar Visual C# aplicação de consola (.NET Framework)")

1. Instalar e fazer referência a [pacote NuGet do SDK de voz](https://aka.ms/csspeech/nuget). No Solution Explorer, clique com o botão direito a solução e selecione **gerir pacotes NuGet para solução**.

    ![Contexto gerir pacotes NuGet para solução](media/sdk/qs-csharp-dotnet-windows-02-manage-nuget-packages.png "gerir pacotes NuGet para solução")

1. No canto superior direito, no **origem do pacote** campo, selecione **Nuget.org**. Procure o `Microsoft.CognitiveServices.Speech` empacotamento e instalá-lo para o **helloworld** projeto.

    ![Instalar o pacote de NuGet Microsoft.CognitiveServices.Speech](media/sdk/qs-csharp-dotnet-windows-03-nuget-install-0.5.0.png "pacote Nuget instalar")

1. No ecrã de licença que aparece, aceite a licença:

    ![Aceitar a licença](media/sdk/qs-csharp-dotnet-windows-04-nuget-license.png "aceitar a licença")

## <a name="create-a-platform-configuration-matching-your-pc-architecture"></a>Criar uma configuração de plataforma que corresponda a arquitetura de seu PC

Nesta secção, vai adicionar uma nova plataforma para a configuração que corresponda a arquitetura do processador.

1. Inicie o Gestor de configuração. Selecione **crie** > **do Configuration Manager**.

    ![Inicie o Gestor de configuração](media/sdk/qs-csharp-dotnet-windows-05-cfg-manager-click.png "iniciar o configuration manager")

1. Na **Configuration Manager** diálogo caixa, adicione uma nova plataforma. Partir do **plataforma de solução Active Directory** na lista pendente, selecione **New**.

    ![Adicionar uma nova plataforma sob a janela do Gestor de configuração](media/sdk/qs-csharp-dotnet-windows-06-cfg-manager-new.png "adicionar uma nova plataforma sob a janela do Gestor de configuração")

1. Se estiver a executar o Windows de 64 bits, crie uma nova configuração de plataforma com o nome `x64`. Se estiver a executar o Windows de 32 bits, crie uma nova configuração de plataforma com o nome `x86`. Neste artigo, vai criar um `x64` configuração de plataforma.

    ![No Windows de 64 bits, adicione uma nova plataforma com o nome "x64"](media/sdk/qs-csharp-dotnet-windows-07-cfg-manager-add-x64.png "plataforma adicionar x64")

## <a name="add-the-sample-code"></a>Adicione o código de exemplo

1. Na `Program.cs` de ficheiros no seu projeto do Visual Studio, substitua todo o código pelo seguinte:

    [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-dotnet-windows/helloworld/Program.cs#code)]

1. Substitua a cadeia de caracteres `YourSubscriptionKey` com a sua chave de subscrição.

1. Substitua a cadeia de caracteres `YourServiceRegion` com o [região](regions.md) associados à subscrição (por exemplo, `westus` para a subscrição de avaliação gratuita).

1. Guarde as alterações ao projeto.

## <a name="build-and-run-the-sample"></a>Criar e executar o exemplo

1. Crie a aplicação. Na barra de menus, selecione **crie** > **compilar solução**. O código será compilado sem erros agora:

    ![Compilação bem-sucedida](media/sdk/qs-csharp-dotnet-windows-08-build.png "compilação bem-sucedida")

1. Inicie a aplicação. Na barra de menus, selecione **depurar** > **iniciar depuração**, ou prima **F5**.

    ![Iniciar a aplicação em depuração](media/sdk/qs-csharp-dotnet-windows-09-start-debugging.png "iniciar a aplicação para depuração")

1. Uma janela do console será exibida, solicitando a dizer algo (em inglês).
   O resultado desse reconhecimento é apresentado no ecrã.

    ![Saída da consola após o reconhecimento bem-sucedido](media/sdk/qs-csharp-dotnet-windows-10-console-output.png "saída da consola após o reconhecimento bem-sucedido")

[!include[Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Procure este exemplo no `quickstart/csharp-dotnet-windows` pasta.

## <a name="next-steps"></a>Passos Seguintes

- [Tradução de voz](how-to-translate-speech.md)
- [Personalizar modelos acústicos](how-to-customize-acoustic-models.md)
- [Personalizar modelos de linguagem](how-to-customize-language-model.md)
