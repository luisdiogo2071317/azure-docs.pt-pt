---
title: 'Início rápido: Reconhecer e transcrever voz, .NET Framework (Windows) - Serviço de Voz'
titleSuffix: Azure Cognitive Services
description: Utilize este guia para criar uma aplicação de consola de voz em texto com o .NET framework para Windows e o SDK de Voz. Quando terminar, pode utilizar o microfone do seu computador para converter voz em texto em tempo real.
services: cognitive-services
author: wolfma61
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: quickstart
ms.date: 11/05/2018
ms.author: wolfma
ms.openlocfilehash: 502f59c88808480f26e6ea5a6d2c5b362c78869b
ms.sourcegitcommit: db2cb1c4add355074c384f403c8d9fcd03d12b0c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/15/2018
ms.locfileid: "51683505"
---
# <a name="quickstart-recognize-and-transcribe-speech-using-the-speech-sdk-and-net-framework-windows"></a>Início rápido: Reconhecer e transcrever voz com o SDK de Voz e o .NET Framework (Windows)

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Utilize este guia para criar uma aplicação de consola de voz em texto com o .NET framework para Windows e o SDK de Voz. Quando terminar, pode utilizar o microfone do seu computador para converter voz em texto em tempo real.

Este início rápido exige uma [conta dos Serviços Cognitivos do Azure](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) com o Microsoft Speech ativado. Se não tiver uma conta, pode utilizar a [avaliação gratuita](https://docs.microsoft.com/azure/cognitive-services/speech-service/get-started) para obter uma chave de subscrição.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este projeto, precisa de:

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
* Uma chave de subscrição para o Serviço de Voz
* Acesso ao microfone do seu computador

## <a name="create-a-visual-studio-project"></a>Criar um projeto do Visual Studio

[!INCLUDE [Create project ](../../../includes/cognitive-services-speech-service-create-speech-project-vs-csharp.md)]

## <a name="add-sample-code"></a>Adicionar código de exemplo

1. Abra `Program.cs` e substitua o código gerado automaticamente por este exemplo:

    [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-dotnet-windows/helloworld/Program.cs#code)]

1. Encontre e substitua a cadeia `YourSubscriptionKey` pela sua chave de subscrição do Serviço de Voz.

1. Encontre e substitua a cadeia de carateres `YourServiceRegion` pela [região](regions.md) associada à subscrição. Por exemplo, se estiver a utilizar a versão de avaliação gratuita, a região é `westus`.

1. Guarde as alterações ao projeto.

## <a name="build-and-run-the-app"></a>Compilar e executar a aplicação

1. Na barra de menus, selecione **Compilar** > **Compilar Solução**. Agora o código deverá ser compilado sem erros.

    ![Captura de ecrã da aplicação Visual Studio, com a opção Compilar Solução realçada](media/sdk/qs-csharp-dotnet-windows-08-build.png "Compilação bem-sucedida")

1. Na barra de menus, selecione **Depurar** > **Iniciar Depuração** ou prima **F5** para iniciar a aplicação.

    ![Captura de ecrã da aplicação Visual Studio, com a opção Iniciar Depuração realçada](media/sdk/qs-csharp-dotnet-windows-09-start-debugging.png "Iniciar a depuração da aplicação")

1. Será apresentada uma janela de consola, que lhe pede para falar. Agora, diga algo em inglês. A sua voz é transmitida para o Serviço de Voz e convertida para texto em tempo real. O resultado é impresso na consola.

    ![Captura de ecrã a mostrar o resultado da consola após o reconhecimento bem-sucedido](media/sdk/qs-csharp-dotnet-windows-10-console-output.png "Resultado da consola após o reconhecimento bem-sucedido")

[!INCLUDE [Download this sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
O código está disponível na pasta `quickstart/csharp-dotnet-windows`.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Reconhecer intenções de voz com o SDK de Voz para C#](how-to-recognize-intents-from-speech-csharp.md)

## <a name="see-also"></a>Consulte também

- [Traduzir voz](how-to-translate-speech-csharp.md)
- [Personalizar modelos acústicos](how-to-customize-acoustic-models.md)
- [Personalizar modelos de idioma](how-to-customize-language-model.md)
