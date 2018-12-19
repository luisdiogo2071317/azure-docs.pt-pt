---
title: 'Início rápido: Reconhecer a conversão de voz, C++ (Windows) - serviços de voz'
titleSuffix: Azure Cognitive Services
description: Aprenda a reconhecer voz em C++ no Ambiente de Trabalho do Windows com o SDK de Serviço de Voz
services: cognitive-services
author: wolfma61
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: quickstart
ms.date: 11/06/2018
ms.author: wolfma
ms.openlocfilehash: 1a26cbea3aaca700f5e501d3b6be0f7d1c942f36
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/19/2018
ms.locfileid: "53607545"
---
# <a name="quickstart-recognize-speech-in-c-on-windows-by-using-the-speech-sdk"></a>Início rápido: Reconhecer voz em C++ no Windows com o SDK de voz

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Neste artigo, vai criar uma aplicação de consola C++ para Windows. Utilize o[SDK de Voz](speech-sdk.md) dos Serviços Cognitivos para a conversão de voz em texto em tempo real, a partir do microfone do seu PC. A aplicação é compilada com o [pacote NuGet de SDK de Voz](https://aka.ms/csspeech/nuget) e o Microsoft Visual Studio 2017 (qualquer edição).

## <a name="prerequisites"></a>Pré-requisitos

Precisa de uma chave de subscrição do serviço de Voz para concluir este Guia de Início Rápido. Pode obter uma gratuitamente. Veja [Experimente o serviço de Voz gratuitamente](get-started.md) para obter detalhes.

## <a name="create-a-visual-studio-project"></a>Criar um projeto do Visual Studio

1. Inicie o Visual Studio 2017.

1. Certifique-se de que a carga de trabalho de **Desenvolvimento de ambiente de trabalho com C++** está disponível. Escolha **Ferramentas** > **Obter Ferramentas e Funcionalidades** na barra de menus do Visual Studio para abrir o instalador do Visual Studio. Se esta carga de trabalho já estiver ativada, avance para o passo seguinte.

    ![Captura de ecrã do separador Cargas de Trabalho do Visual Studio](media/sdk/vs-enable-cpp-workload.png)

    Caso contrário, assinale a caixa junto a **Desenvolvimento de ambiente de trabalho com C++**.

1. Certifique-se de que o componente **Gestor de pacotes NuGet** está disponível. Mude para o **componentes individuais** separador de caixa de diálogo do instalador do Visual Studio. Selecione **Gestor de pacotes NuGet** se não estiver já ativada.

      ![Captura de ecrã do separador Componentes individuais do Visual Studio](media/sdk/vs-enable-nuget-package-manager.png)

1. Se precisar de ativar a carga de trabalho C++ ou o NuGet, selecione **Modificar** (no canto inferior direito da caixa de diálogo). A instalação das novas funcionalidades demora alguns minutos. Se as duas funcionalidades já estiverem ativadas, feche a caixa de diálogo em vez disso.

1. Crie uma nova Aplicação de Consola do Ambiente de Trabalho do Windows com o Visual C++. Em primeiro lugar, escolha **Ficheiro** > **Novo** > **Projeto** no menu. Na caixa de diálogo **Novo Projeto**, expanda **Instalado** > **Visual C++** > **Ambiente de Trabalho do Windows** no painel esquerdo. Em seguida, selecione **Aplicação de Consola do Windows**. No nome do projeto, introduza *helloworld*.

    ![Captura de ecrã da caixa de diálogo Novo Projeto](media/sdk/qs-cpp-windows-01-new-console-app.png)

1. Se estiver a executar o Windows de 64 bits, poderá alternar a sua plataforma de compilação para `x64`, com o menu de lista pendente da barra de ferramentas do Visual Studio. (versões de 64 bits do Windows podem executar aplicativos de 32 bits diretamente.)

    ![Captura de ecrã da barra de ferramentas do Visual Studio, com a opção x64 realçada](media/sdk/qs-cpp-windows-02-switch-to-x64.png)

1. No Explorador de Soluções, clique com o botão direito do rato na solução e, em seguida, escolha **Gerir Pacotes NuGet para Solução**.

    ![Captura de ecrã do Explorador de Soluções, com a opção Gerir Pacotes NuGet para Solução realçada](media/sdk/qs-cpp-windows-03-manage-nuget-packages.png)

1. No canto superior direito, no campo **Origem do Pacote**, selecione **nuget.org**. Procure o pacote `Microsoft.CognitiveServices.Speech` e instale-o no projeto **helloworld**.

    ![Captura de ecrã da caixa de diálogo Gerir Pacotes para Solução](media/sdk/qs-cpp-windows-04-nuget-install-1.0.0.png)

    > [!NOTE]
    > A versão atual do SDK de Voz dos Serviços Cognitivos é `1.2.0`.

1. Aceite a licença apresentada para iniciar a instalação do pacote NuGet.

    ![Captura de ecrã da caixa de diálogo Aceitação da Licença](media/sdk/qs-cpp-windows-05-nuget-license.png)

Depois de o pacote ser instalado, é apresentada uma mensagem de confirmação na consola do Gestor de Pacotes.

## <a name="add-sample-code"></a>Adicionar código de exemplo

1. Abra o ficheiro de origem *helloworld.cpp*. Substitua todo o código abaixo da instrução de inclusão inicial (`#include "stdafx.h"` ou `#include "pch.h"`) pelo seguinte:

   [!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/cpp-windows/helloworld/helloworld.cpp#code)]

1. No mesmo ficheiro, substitua a cadeia de carateres `YourSubscriptionKey` pela sua chave de subscrição.

1. Substitua a cadeia de carateres `YourServiceRegion` pela [região](regions.md) associada à subscrição (por exemplo, `westus` para a subscrição de avaliação gratuita).

1. Guarde as alterações ao projeto.

## <a name="build-and-run-the-app"></a>Compilar e executar a aplicação

1. Compile a aplicação. Na barra de menus, escolha **Compilar** > **Compilar solução**. O código deve ser compilado sem erros.

   ![Captura de ecrã da aplicação Visual Studio, com a opção Compilar Solução realçada](media/sdk/qs-cpp-windows-06-build.png)

1. Inicie a aplicação. Na barra de menus, escolha **Depurar** > **Iniciar Depuração** ou prima **F5**.

   ![Captura de ecrã da aplicação Visual Studio, com a opção Iniciar Depuração realçada](media/sdk/qs-cpp-windows-07-start-debugging.png)

1. É apresentada uma janela de consola que lhe pede para dizer algo. Diga uma expressão ou uma frase em inglês. A sua voz é transmitida ao serviço de Voz e convertida para texto que é apresentado na mesma janela.

   ![Captura de ecrã da saída da consola após o reconhecimento bem-sucedido](media/sdk/qs-cpp-windows-08-console-output-release.png)

[!INCLUDE [Download this sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Procure este exemplo na pasta `quickstart/cpp-windows`.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Reconhecer intenções de voz com o SDK de Voz para C++](how-to-recognize-intents-from-speech-cpp.md)

## <a name="see-also"></a>Consulte também

- [Traduzir voz](how-to-translate-speech-csharp.md)
- [Personalizar modelos acústicos](how-to-customize-acoustic-models.md)
- [Personalizar modelos de idioma](how-to-customize-language-model.md)
