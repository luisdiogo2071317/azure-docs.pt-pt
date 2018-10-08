---
title: 'Início Rápido: reconhecer voz em C# no .NET Core no Windows com o SDK de Voz dos Serviços Cognitivos'
titleSuffix: Microsoft Cognitive Services
description: Saiba como reconhecer voz em C# no .NET Core no Windows com o SDK de Voz dos Serviços Cognitivos
services: cognitive-services
author: wolfma61
ms.service: cognitive-services
ms.component: speech-service
ms.topic: quickstart
ms.date: 09/24/2018
ms.author: wolfma
ms.openlocfilehash: 2cb89606986645d567136655d5ab3f07223ba70d
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/28/2018
ms.locfileid: "47434777"
---
# <a name="quickstart-recognize-speech-in-c-under-net-core-on-windows-by-using-the-speech-sdk"></a>Início Rápido: reconhecer voz em C# no .NET Core no Windows com o SDK de Voz

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Neste artigo, vai criar uma aplicação de consola C# para .NET Core em Windows, com o [SDK de Voz](speech-sdk.md) dos Serviços Cognitivos. Converta voz em texto em tempo real, a partir do microfone do seu PC. A aplicação é compilada com o [pacote NuGet de SDK de Voz](https://aka.ms/csspeech/nuget) e o Microsoft Visual Studio 2017 (qualquer edição).

> [!NOTE]
> O .NET Core é uma plataforma .NET de código-fonte aberto para várias plataformas, que implementa a especificação [.NET Standard](https://docs.microsoft.com/dotnet/standard/net-standard).

Precisa de uma chave de subscrição do serviço de Voz para concluir este Início Rápido. Pode obter uma gratuitamente. Veja [Experimente o serviço de Voz gratuitamente](get-started.md) para obter detalhes.


## <a name="create-a-visual-studio-project"></a>Criar um projeto do Visual Studio

1. Inicie o Visual Studio 2017.

1. Certifique-se de que a carga de trabalho de **desenvolvimento para várias plataformas .NET**está disponível. Escolha **Ferramentas** > **Obter Ferramentas e Funcionalidades** na barra de menus do Visual Studio para abrir o instalador do Visual Studio. Se esta carga de trabalho já estiver ativada, feche a caixa de diálogo.

    ![Captura de ecrã do instalador do Visual Studio, com o separador Cargas de Trabalho realçado](media/sdk/vs-enable-net-core-workload.png)

    Caso contrário, selecione a caixa junto a **desenvolvimento para várias plataformas .NET Core,** e selecione **Modificar** no canto inferior direito da caixa de diálogo. A instalação da nova funcionalidade demora alguns minutos.

1. Crie uma nova Aplicação de Consola de Visual C# .NET Core. Na caixa de diálogo **Novo Projeto**, no painel à esquerda, expanda **Instalado** > **Visual C#** > **.NET Core**. Em seguida, selecione **Aplicação de Consola (.NET Core)**. No nome do projeto, introduza *helloworld*.

    ![Captura de ecrã da caixa de diálogo Novo Projeto](media/sdk/qs-csharp-dotnetcore-windows-01-new-console-app.png "Criar Aplicação da Consola Visual C# (.NET Core)")

1. Instale e faça referência ao [pacote NuGet do SDK de Voz](https://aka.ms/csspeech/nuget). No Explorador de Soluções, clique com o botão direito do rato na solução e selecione **Gerir Pacotes NuGet para Solução**.

    ![Captura de ecrã do Explorador de Soluções, com Gerir Pacotes NuGet para Solução realçado](media/sdk/qs-csharp-dotnetcore-windows-02-manage-nuget-packages.png "Gerir Pacotes NuGet para Solução")

1. No canto superior direito, no campo **Origem do Pacote**, selecione **nuget.org**. Procure o pacote `Microsoft.CognitiveServices.Speech` e instale-o no projeto **helloworld**.

    ![Captura de ecrã da caixa de diálogo Gerir Pacotes para Solução](media/sdk/qs-csharp-dotnetcore-windows-03-nuget-install-1.0.0.png "Instalar pacote NuGet")

1. Aceite a licença apresentada para iniciar a instalação do pacote NuGet.

    ![Captura de ecrã da caixa de diálogo Aceitação da Licença](media/sdk/qs-csharp-dotnetcore-windows-04-nuget-license.png "Aceitar a Licença")

Depois de o pacote ser instalado, é apresentada uma mensagem de confirmação na consola do Gestor de Pacotes.


## <a name="add-sample-code"></a>Adicionar código de exemplo

1. Abra `Program.cs` e substitua todo o código no mesmo pelo seguinte.

    [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-dotnetcore-windows/helloworld/Program.cs#code)]

1. No mesmo ficheiro, substitua a cadeia de carateres `YourSubscriptionKey` pela sua chave de subscrição.

1. Substitua também a cadeia de carateres `YourServiceRegion`pela [região](regions.md) associada à subscrição (por exemplo, `westus` para a subscrição de avaliação gratuita).

1. Guarde as alterações ao projeto.

## <a name="build-and-run-the-app"></a>Compilar e executar a aplicação

1. Compile a aplicação. Na barra de menus, escolha **Compilar** > **Compilar solução**. O código deve ser compilado sem erros.

    ![Captura de ecrã da aplicação Visual Studio, com a opção Compilar Solução realçada](media/sdk/qs-csharp-dotnetcore-windows-05-build.png "Compilação bem-sucedida")

1. Inicie a aplicação. Na barra de menus, escolha **Depurar** > **Iniciar Depuração** ou prima **F5**.

    ![Captura de ecrã da aplicação Visual Studio, com a opção Iniciar Depuração realçada](media/sdk/qs-csharp-dotnetcore-windows-06-start-debugging.png "Iniciar a depuração da aplicação")

1. É apresentada uma janela de consola que lhe pede para dizer algo. Diga uma expressão ou uma frase em inglês. A sua voz é transmitida ao serviço de Voz e convertida para texto que é apresentado na mesma janela.

    ![Captura de ecrã da saída da consola após o reconhecimento bem-sucedido](media/sdk/qs-csharp-dotnetcore-windows-07-console-output.png "Saída da consola após o reconhecimento bem-sucedido")

[!INCLUDE [Download this sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Procure este exemplo na pasta `quickstart/csharp-dotnetcore-windows`.

## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"]
> [Reconhecer intenções de voz com o SDK de Voz para C#](how-to-recognize-intents-from-speech-csharp.md)

## <a name="see-also"></a>Consulte também

- [Traduzir voz](how-to-translate-speech-csharp.md)
- [Personalizar modelos acústicos](how-to-customize-acoustic-models.md)
- [Personalizar modelos de idioma](how-to-customize-language-model.md)
