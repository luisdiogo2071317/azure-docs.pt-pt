---
title: 'Guia de Início Rápido: reconhecer voz em C# no .NET Framework no Windows com o SDK de Voz dos Serviços Cognitivos'
titleSuffix: Microsoft Cognitive Services
description: Saiba como reconhecer voz em C# no .NET Framework no Windows com o SDK de Voz dos Serviços Cognitivos
services: cognitive-services
author: wolfma61
ms.service: cognitive-services
ms.component: speech-service
ms.topic: quickstart
ms.date: 09/24/2018
ms.author: wolfma
ms.openlocfilehash: 32b484451c4ee2264c25cca92b1d03d91b955a29
ms.sourcegitcommit: 715813af8cde40407bd3332dd922a918de46a91a
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "47054001"
---
# <a name="quickstart-recognize-speech-in-c-under-net-framework-on-windows-by-using-the-speech-sdk"></a>Guia de Início Rápido: reconhecer voz em C# no .NET Framework no Windows com o SDK de Voz

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Neste artigo, irá criar uma aplicação de consola C# para .NET Framework em Windows com o [SDK de Voz](speech-sdk.md). Converta voz em texto em tempo real a partir do microfone do seu PC. A aplicação é compilada com o [pacote NuGet de SDK de Voz](https://aka.ms/csspeech/nuget) e o Microsoft Visual Studio 2017 (qualquer edição).

## <a name="prerequisites"></a>Pré-requisitos

Precisa de uma chave de subscrição do serviço de Voz para concluir este Guia de Início Rápido. Pode obter uma gratuitamente. Veja [Experimente o serviço de Voz gratuitamente](get-started.md) para obter detalhes.

## <a name="create-a-visual-studio-project"></a>Criar um projeto do Visual Studio

[!INCLUDE [Create project ](../../../includes/cognitive-services-speech-service-create-speech-project-vs-csharp.md)]

## <a name="add-sample-code"></a>Adicionar código de exemplo

1. Abra `Program.cs` e substitua todo o código no mesmo pelo seguinte.

    [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-dotnet-windows/helloworld/Program.cs#code)]

1. No mesmo ficheiro, substitua a cadeia de carateres `YourSubscriptionKey` pela sua chave de subscrição do serviço de Voz.

1. Substitua também a cadeia de caracteres `YourServiceRegion` pela [região](regions.md) associada à subscrição (por exemplo, `westus` para a subscrição de avaliação gratuita).

1. Guarde as alterações ao projeto.

## <a name="build-and-run-the-app"></a>Compilar e executar a aplicação

1. Compile a aplicação. Na barra de menus, selecione **Compilar** > **Compilar Solução**. Agora o código deverá ser compilado sem erros.

    ![Captura de ecrã a mostrar a aplicação Visual Studio, com a opção Compilar Solução realçada](media/sdk/qs-csharp-dotnet-windows-08-build.png "Compilação bem-sucedida")

1. Inicie a aplicação. Na barra de menus, selecione **Depurar** > **Iniciar Depuração** ou prima **F5**.

    ![Captura de ecrã a mostrar a aplicação Visual Studio, com a opção Iniciar Depuração realçada](media/sdk/qs-csharp-dotnet-windows-09-start-debugging.png "Iniciar a depuração da aplicação")

1. É apresentada uma janela de consola que lhe pede para dizer algo. Diga uma expressão ou uma frase em inglês. A sua voz é transmitida ao serviço de Voz e convertida para texto que é apresentado na mesma janela.

    ![Captura de ecrã a mostrar o resultado da consola após o reconhecimento bem-sucedido](media/sdk/qs-csharp-dotnet-windows-10-console-output.png "Resultado da consola após o reconhecimento bem-sucedido")

[!INCLUDE [Download this sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Procure este exemplo na pasta `quickstart/csharp-dotnet-windows`.

## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"]
> [Reconhecer intenções de voz com o SDK de Voz para C#](how-to-recognize-intents-from-speech-csharp.md)

## <a name="see-also"></a>Consulte também

- [Traduzir voz](how-to-translate-speech-csharp.md)
- [Personalizar modelos acústicos](how-to-customize-acoustic-models.md)
- [Personalizar modelos de idioma](how-to-customize-language-model.md)
