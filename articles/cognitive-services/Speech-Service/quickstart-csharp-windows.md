---
title: 'Início rápido: Reconhecer voz utilizando o cognitivos serviços voz c# SDK para Windows | Microsoft Docs'
description: Saiba como reconhecer voz utilizando o SDK do c# para o serviço de reconhecimento de voz.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: wolfma61
manager: onano
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: wolfma
ms.openlocfilehash: 64281215d139731b61365936bc1b837798ad8fbf
ms.sourcegitcommit: 0fa8b4622322b3d3003e760f364992f7f7e5d6a9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37016839"
---
# <a name="quickstart-recognize-speech-using-the-cognitive-services-speech-c-sdk"></a>Início rápido: Reconhecer voz utilizando voz c# SDK de serviços cognitivos

Neste artigo, irá aprender a criar uma aplicação de consola c# no Windows utilizando o SDK dos serviços de reconhecimento de voz cognitivos para transcribe reconhecimento de voz para texto.

## <a name="prerequisites"></a>Pré-requisitos

* Uma chave de subscrição para o serviço de reconhecimento de voz. Consulte [experimentarem o serviço de reconhecimento de voz gratuitamente](get-started.md).
* Visual Studio 2017, edição Community ou superior.
* O **desenvolvimento de ambiente de trabalho .NET** carga de trabalho no Visual Studio. Pode ativá-la na **ferramentas** \> **obter ferramentas e funcionalidades**. 

## <a name="create-a-visual-studio-project"></a>Criar um projeto do Visual Studio

1. No Visual Studio 2017, crie um novo Visual c# aplicação de consola. No **novo projeto** caixa de diálogo, no painel esquerdo, expanda **instalada** e, em seguida, selecione **aplicação de consola (.NET Framework)**. Para o nome de projeto, introduza *CsharpHelloSpeech*.

    ![Criar Visual c# aplicação de consola (.NET Framework)](media/sdk/speechsdk-05-vs-cs-new-console-app.png "Criar aplicação de consola Visual c#")

2. Instalar e referenciar o [pacote NuGet do SDK de reconhecimento de voz](https://aka.ms/csspeech/nuget). No Explorador de soluções, clique com o botão direito na solução e selecione **gerir pacotes NuGet para solução**.

    ![Contexto gerir pacotes NuGet para solução](media/sdk/speechsdk-06-vs-cs-manage-nuget-packages.png "gerir pacotes NuGet para solução")

3. No canto superior direito, no **origem do pacote** campo, selecione **Nuget.org**. Procurar e instalar o `Microsoft.CognitiveServices.Speech` do pacote e instalá-la para o **CsharpHelloSpeech** projeto.

    ![Instalar o pacote NuGet de Microsoft.CognitiveServices.Speech](media/sdk/speechsdk-08-vs-cs-nuget-install.png "pacote Nuget instalar")

4. No ecrã de licença que aparece, aceite a licença:

    ![Aceitar a licença](media/sdk/speechsdk-09-vs-cs-nuget-license.png "aceitar a licença")

## <a name="create-a-platform-configuration-matching-your-pc-architecture"></a>Criar uma configuração de plataforma correspondência da arquitetura de PC

Nesta secção, é possível adicionar uma nova plataforma para a configuração que corresponda a arquitetura do processador.

1. Inicie o Gestor de configuração. Selecione **criar** > **do Configuration Manager**.

    ![Inicie o Gestor de configuração](media/sdk/speechsdk-12-vs-cs-cfg-manager-click.png "iniciar o Gestor de configuração")

2. No **do Configuration Manager** diálogo caixa, adicione uma nova plataforma. Do **plataforma do Active Directory solução** na lista pendente, selecione **novo**.

    ![Adicionar uma nova plataforma sob a janela do Gestor de configuração](media/sdk/speechsdk-14-vs-cs-cfg-manager-new.png "adicionar uma nova plataforma sob a janela do Gestor de configuração")

3. Se estiver a executar o Windows de 64 bits, crie uma nova configuração de plataforma com o nome `x64`. Se estiver a executar o Windows de 32 bits, crie uma nova configuração de plataforma com o nome `x86`. Neste artigo, criou um `x64` configuração de plataforma. 

    ![No Windows de 64 bits, adicione uma nova plataforma com o nome "x64"](media/sdk/speechsdk-15-vs-cs-cfg-manager-add-x64.png "plataforma adicionar x64")

## <a name="add-the-sample-code"></a>Adicione o código de exemplo

1. No `Program.cs` para o seu projeto de Visual Studio, substitua o corpo do `Program` classe com o seguinte. Certifique-se de que substitua a chave de subscrição e região que obteve para o serviço.

    [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/Windows/quickstart-csharp/Program.cs#code)]

2. Após a colagem de código, o `Main()` método deve assemelhar-se conforme mostrado na captura de ecrã seguinte:

    ![O método Main após colar o código](media/sdk/speechsdk-17-vs-cs-paste-code.png "método Final Main")

3. Visual Studio IntelliSense realça as referências de classes do SDK de reconhecimento de voz que não foi possível resolver. Para corrigir este erro, adicione o seguinte `using` instrução para o início do código (ou manualmente, ou utilizando o Visual Studio [ações rápidas](https://docs.microsoft.com/visualstudio/ide/quick-actions)).

    [!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/Windows/quickstart-csharp/Program.cs#usingstatement)]

    ![Utilize a ação rápida para adicionar o em falta utilizando a instrução](media/sdk/speechsdk-18-vs-cs-add-using.png "IntelliSense resolver problemas")

4. Certifique-se de que o IntelliSense realce resolvido e guarde as alterações ao projeto.

## <a name="build-and-run-the-sample"></a>Criar e executar o exemplo

1. Crie a aplicação. Na barra de menus, selecione **criar** > **compilar solução**. O código deve agora compilar sem erros:

    ![Compilação efetuada com êxito](media/sdk/speechsdk-20-vs-cs-build.png "compilação efetuada com êxito")

2. Inicie a aplicação. Na barra de menus, selecione **depurar** > **iniciar depuração**, ou prima **F5**. 

    ![Iniciar a aplicação para depuração](media/sdk/speechsdk-21-vs-cs-f5.png "iniciar a aplicação para depuração")

3. Uma janela da consola aparece, que lhe pede para dizer algo (em inglês).
O resultado do reconhecimento é apresentado no ecrã.

    ![Resultado da consola depois de efetuada com êxito reconhecimento](media/sdk/speechsdk-22-cs-vs-console-output.png "consola saída após reconhecimento com êxito")

## <a name="download-code"></a>Transferir o código

Para o conjunto mais recente de exemplos, consulte o [repositório cognitivos GitHub de exemplo do SDK de reconhecimento de voz de serviços](https://aka.ms/csspeech/samples).

## <a name="next-steps"></a>Passos Seguintes

- [Converter o reconhecimento de voz](how-to-translate-speech.md)
- [Personalizar os modelos de reconhecimento de voz](how-to-customize-speech-models.md)
