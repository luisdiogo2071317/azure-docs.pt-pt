---
title: 'Início rápido: Reconhecer voz em c# numa aplicação UWP com o SDK de voz dos serviços cognitivos'
titleSuffix: Microsoft Cognitive Services
description: Aprender a reconhecer a voz numa aplicação UWP com o SDK de voz dos serviços cognitivos
services: cognitive-services
author: wolfma61
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 07/16/2018
ms.author: wolfma
ms.openlocfilehash: 549a8fd29cf1f986da0af1ae49dbf0e5335603bd
ms.sourcegitcommit: 7ad9db3d5f5fd35cfaa9f0735e8c0187b9c32ab1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/27/2018
ms.locfileid: "39325705"
---
# <a name="quickstart-recognize-speech-in-a-uwp-app-using-the-speech-sdk"></a>Início rápido: Reconhecer voz numa aplicação UWP utilizando o SDK de voz

[!include[Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Neste artigo, irá aprender a criar uma aplicação plataforma Universal do Windows (UWP) usando o SDK de voz dos serviços cognitivos para transcrição de voz em texto.
A aplicação baseia-se com o [Microsoft Cognitive Services voz SDK do pacote NuGet](https://aka.ms/csspeech/nuget) e Microsoft Visual Studio 2017.

> [!NOTE]
> A plataforma Universal do Windows permite-lhe desenvolver aplicações que são executadas em qualquer dispositivo que suporte o Windows 10, incluindo PCs, Xbox, Surface Hub e outros dispositivos. Aplicações com o SDK de voz ainda não passam o Kit de certificação de aplicações de Windows (WACK). É possível fazer sideload de seu aplicativo, mas pode não atualmente ser submetido para a Windows Store.

## <a name="prerequisites"></a>Pré-requisitos

* Uma chave de subscrição para o serviço de voz. Ver [experimentar gratuitamente o serviço de voz](get-started.md).
* Um PC Windows (Windows 10 Fall Creators Update ou posterior) com um microfone de trabalho.
* [Microsoft Visual Studio 2017](https://www.visualstudio.com/), Community Edition ou superior.
* O **desenvolvimento de plataforma Universal do Windows** carga de trabalho no Visual Studio.You pode ativá-la no **ferramentas** \> **obter ferramentas e funcionalidades**.

  ![Ativar o desenvolvimento de plataforma Universal do Windows](media/sdk/vs-enable-uwp-workload.png)

## <a name="create-a-visual-studio-project"></a>Criar um projeto do Visual Studio

1. No Visual Studio 2017, crie um novo Visual c# Windows Universal aplicação em branco. Na **novo projeto** caixa de diálogo, no painel à esquerda, expanda **instalado** \> **Visual C#** \> **Windows Universal** e, em seguida, selecione **aplicação em branco (Universal Windows)**. Para o nome do projeto, introduza *helloworld*.

    ![](media/sdk/qs-csharp-uwp-01-new-blank-app.png)

1. Na **novo projeto de plataforma Universal do Windows** janela que é exibido, escolha **Windows 10 Fall Creators Update (10.0; Compilação 16299)** como **versão mínima**e isso, ou qualquer versão posterior, como **versão de destino**, em seguida, clique em **OK**.

    ![](media/sdk/qs-csharp-uwp-02-new-uwp-project.png)

1. Se estiver executando numa instalação do Windows de 64 bits, poderá alternar sua plataforma de compilação para `x64`.

   ![Mude a plataforma de compilação para x64](media/sdk/qs-csharp-uwp-03-switch-to-x64.png)

   > [!NOTE]
   > Neste momento, o SDK de voz suporta processadores compatíveis com o Intel, mas não ARM.

1. Instalar e fazer referência a [pacote NuGet do SDK de voz](https://aka.ms/csspeech/nuget). No Solution Explorer, clique com o botão direito a solução e selecione **gerir pacotes NuGet para solução**.

    ![Contexto gerir pacotes NuGet para solução](media/sdk/qs-csharp-uwp-04-manage-nuget-packages.png)

1. No canto superior direito, no **origem do pacote** campo, selecione **Nuget.org**. Procure e instale o `Microsoft.CognitiveServices.Speech` empacotar e instalá-lo para o **helloworld** projeto.

    ![Instalar o pacote de NuGet Microsoft.CognitiveServices.Speech](media/sdk/qs-csharp-uwp-05-nuget-install-0.5.0.png "pacote Nuget instalar")

1. Aceite a licença na caixa de diálogo que aparece.

    ![Aceitar a licença](media/sdk/qs-csharp-uwp-06-nuget-license.png "aceitar a licença")

1. A seguinte linha de saída é apresentada na consola do Gestor de pacotes.

   ```text
   Successfully installed 'Microsoft.CognitiveServices.Speech 0.5.0' to helloworld
   ```

## <a name="add-the-sample-code"></a>Adicione o código de exemplo

1. No Explorador de soluções, faça duplo clique **Package. appxmanifest** para editar o manifesto da aplicação.
   Selecione o **capacidades** separador, selecione a caixa de verificação a **microfone** capacidade e guarde as alterações.

   ![](media/sdk/qs-csharp-uwp-07-capabilities.png)

1. Editar a interface de utilizador do seu aplicativo clicando duas vezes `MainPage.xaml` no Explorador de soluções. 

    Na vista XAML do designer, inserir o seguinte trecho XAML para a etiqueta de grade (entre `<Grid>` e `</Grid>`).

   [!code-xml[UI elements](~/samples-cognitive-services-speech-sdk/quickstart/csharp-uwp/helloworld/MainPage.xaml#StackPanel)]

1. Editar o XAML code-behind clicando duas vezes `MainPage.xaml.cs` no Explorador de soluções (ele é agrupado sob o `MainPage.xaml` item).
   Substitua todo o código neste ficheiro com o seguinte.

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-uwp/helloworld/MainPage.xaml.cs#code)]

1. Na `SpeechRecognitionFromMicrophone_ButtonClicked` manipulador, substitua a cadeia de caracteres `YourSubscriptionKey` com a sua chave de subscrição.

1. Na `SpeechRecognitionFromMicrophone_ButtonClicked` manipulador, substitua a cadeia de caracteres `YourServiceRegion` com o [região](regions.md) associados à subscrição (por exemplo, `westus` para a subscrição de avaliação gratuita).

1. Guarde todas as alterações ao projeto.

## <a name="build-and-run-the-sample"></a>Criar e executar o exemplo

1. Crie a aplicação. Na barra de menus, selecione **crie** > **compilar solução**. O código será compilado sem erros agora.

    ![Compilação bem-sucedida](media/sdk/qs-csharp-uwp-08-build.png "compilação bem-sucedida")

1. Inicie a aplicação. Na barra de menus, selecione **depurar** > **iniciar depuração**, ou prima **F5**.

    ![Iniciar a aplicação em depuração](media/sdk/qs-csharp-uwp-09-start-debugging.png "iniciar a aplicação para depuração")

1. Exibe uma janela de GUI. Clique primeiro o **microfone ativar** botão e confirmar a solicitação de permissão que aparece.

    ![Iniciar a aplicação em depuração](media/sdk/qs-csharp-uwp-10-access-prompt.png "iniciar a aplicação para depuração")

1. Clique nas **reconhecimento de voz com a entrada do microfone** e fala uma frase curta no microfone do dispositivo. O texto reconhecido aparece na janela.

    ![](media/sdk/qs-csharp-uwp-11-ui-result.png)

[!include[Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Procure este exemplo no `quickstart/csharp-uwp` pasta.

## <a name="next-steps"></a>Passos Seguintes

- [Tradução de voz](how-to-translate-speech-csharp.md)
- [Personalizar modelos acústicos](how-to-customize-acoustic-models.md)
- [Personalizar modelos de linguagem](how-to-customize-language-model.md)
