---
title: 'Início rápido: Reconhecer a conversão de voz, C# (UWP) - serviços de voz'
titleSuffix: Azure Cognitive Services
description: Neste artigo, vai criar um C# aplicação plataforma Universal do Windows (UWP) com o SDK de voz dos serviços cognitivos. Converta voz em texto em tempo real, a partir do microfone do seu dispositivo. A aplicação baseia-se com o pacote de NuGet do SDK de voz e o Microsoft Visual Studio 2017.
services: cognitive-services
author: wolfma61
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: quickstart
ms.date: 12/06/2018
ms.author: wolfma
ms.custom: seodec18
ms.openlocfilehash: 036c5230104e9915f9bcbaa5dd535a359fecf55a
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/19/2018
ms.locfileid: "53599455"
---
# <a name="quickstart-recognize-speech-in-a-uwp-app-by-using-the-speech-sdk"></a>Início rápido: Reconhecer voz numa aplicação UWP com o SDK de voz

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Neste artigo, vai desenvolver uma C# plataforma Universal do Windows (UWP; Versão 1709 do Windows mais tarde) aplicativos usando os serviços cognitivos [SDK de voz](speech-sdk.md). O programa será transcrição de voz para texto em tempo real do microfone do dispositivo. A aplicação é compilada com o [Pacote NuGet de SDK de Voz](https://aka.ms/csspeech/nuget) e o Microsoft Visual Studio 2017 (qualquer edição).

> [!NOTE]
> A Plataforma Universal do Windows permite-lhe desenvolver aplicações que são executadas em qualquer dispositivo que suporte o Windows 10, incluindo PCs, Xbox, Surface Hub e outros dispositivos.

## <a name="prerequisites"></a>Pré-requisitos

Precisa de uma chave de subscrição do serviço de Voz para concluir este Guia de Início Rápido. Pode obter uma gratuitamente. Veja [Experimente o serviço de Voz gratuitamente](get-started.md) para obter detalhes.

## <a name="create-a-visual-studio-project"></a>Criar um projeto do Visual Studio

1. Inicie o Visual Studio 2017.

1. Certifique-se de que a carga de trabalho de **desenvolvimento da Plataforma Universal do Windows** está disponível. Escolha **Ferramentas** > **Obter Ferramentas e Funcionalidades** na barra de menus do Visual Studio para abrir o instalador do Visual Studio. Se esta carga de trabalho já estiver ativada, feche a caixa de diálogo.

    ![Captura de ecrã do instalador do Visual Studio, com o separador Cargas de Trabalho realçado](media/sdk/vs-enable-uwp-workload.png)

    Caso contrário, selecione a caixa junto a **desenvolvimento para várias plataformas .NET,** e selecione **Modificar** no canto inferior direito da caixa de diálogo. A instalação das novas funcionalidades demora alguns minutos.

1. Crie uma aplicação Universal do Windows do Visual C#. Em primeiro lugar, escolha **Ficheiro** > **Novo** > **Projeto** no menu. Na caixa de diálogo **Novo Projeto**, expanda **Instalado** > **Visual C#** > **Windows Universal** no painel esquerdo. Em seguida, selecione **Aplicação em branco (Universal Windows)**. No nome do projeto, introduza *helloworld*.

    ![Captura de ecrã da caixa de diálogo Novo Projeto](media/sdk/qs-csharp-uwp-01-new-blank-app.png)

1. O SDK de voz requer que criar seu aplicativo para o Windows 10 Fall Creators Update ou posterior. Na janela **Novo Projeto da Plataforma Universal do Windows** que é apresentada, escolha **Atualização do Windows 10 Fall Creators (10.0; Compilação 16299)** como **Versão mínima**. Na caixa **Versão de destino**, selecione esta ou qualquer versão posterior e, em seguida, clique em **OK**.

    ![Captura de ecrã da janela Novo Projeto de Plataforma Universal do Windows](media/sdk/qs-csharp-uwp-02-new-uwp-project.png)

1. Se estiver a executar o Windows de 64 bits, pode alternar a sua plataforma de compilação para `x64`, com o menu de lista pendente da barra de ferramentas do Visual Studio. (O Windows de 64 bits pode executar aplicações de 32 bits, por isso pode deixá-lo definir para `x86`, se preferir.)

   ![Captura de ecrã da barra de ferramentas do Visual Studio, com x64 realçado](media/sdk/qs-csharp-uwp-03-switch-to-x64.png)

   > [!NOTE]
   > O SDK de Voz suporta apenas processadores compatíveis com Intel. O ARM não é atualmente suportado.

1. Instale e faça referência ao [pacote NuGet do SDK de Voz](https://aka.ms/csspeech/nuget). No Explorador de Soluções, clique com o botão direito do rato na solução e selecione **Gerir Pacotes NuGet para Solução**.

    ![Captura de ecrã do Explorador de Soluções, com a opção Gerir Pacotes NuGet para Solução realçada](media/sdk/qs-csharp-uwp-04-manage-nuget-packages.png)

1. No canto superior direito, no campo **Origem do Pacote**, selecione **nuget.org**. Procure o pacote `Microsoft.CognitiveServices.Speech` e instale-o no projeto **helloworld**.

    ![Captura de ecrã da caixa de diálogo Gerir Pacotes para Solução](media/sdk/qs-csharp-uwp-05-nuget-install-1.0.0.png "Instalar pacote NuGet")

1. Aceite a licença apresentada para iniciar a instalação do pacote NuGet.

    ![Captura de ecrã da caixa de diálogo Aceitação da Licença](media/sdk/qs-csharp-uwp-06-nuget-license.png "Aceitar a Licença")

1. A seguinte linha de saída aparece na consola do Gestor de Pacotes.

   ```text
   Successfully installed 'Microsoft.CognitiveServices.Speech 1.2.0' to helloworld
   ```

1. Uma vez que a aplicação utiliza o microfone para entrada de voz, adicione a capacidade **Microfone** ao projeto. No Explorador de Soluções, faça duplo clique em **Package.appxmanifest** para editar o manifesto de aplicação. Em seguida, mude para o separador **Capacidades**, selecione a caixa para a capacidade **Microfone** e guarde as alterações.

   ![Captura de ecrã do manifesto de aplicação de Visual Studio com Capacidades e Microfone realçado](media/sdk/qs-csharp-uwp-07-capabilities.png)


## <a name="add-sample-code"></a>Adicionar código de exemplo

1. A interface do utilizador da aplicação é definida com o XAML. Abra `MainPage.xaml` no Explorador de Soluções. Na vista XAML do designer, insira o seguinte fragmento de código XAML na etiqueta da Grelha (entre `<Grid>` e `</Grid>`).

   [!code-xml[UI elements](~/samples-cognitive-services-speech-sdk/quickstart/csharp-uwp/helloworld/MainPage.xaml#StackPanel)]

1. Abra o ficheiro de origem do code-behind `MainPage.xaml.cs` (encontre-o agrupado sob `MainPage.xaml`). Substitua todo o código no mesmo pelo seguinte.

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-uwp/helloworld/MainPage.xaml.cs#code)]

1. No processador `YourSubscriptionKey` neste ficheiro, substitua a cadeia de carateres `SpeechRecognitionFromMicrophone_ButtonClicked` pela sua chave de subscrição.

1. No processador `westus`, substitua também a cadeia de carateres `SpeechRecognitionFromMicrophone_ButtonClicked` pela `YourServiceRegion`região[ associada à subscrição (por exemplo, ](regions.md) para a subscrição de avaliação gratuita).

1. Guarde todas as alterações no projeto.

## <a name="build-and-run-the-app"></a>Compilar e executar a aplicação

1. Compile a aplicação. Na barra de menus, selecione **Compilar** > **Compilar Solução**. Agora o código deverá ser compilado sem erros.

    ![Captura de ecrã da aplicação Visual Studio, com a opção Compilar Solução realçada](media/sdk/qs-csharp-uwp-08-build.png "Compilação bem-sucedida")

1. Inicie a aplicação. Na barra de menus, selecione **Depurar** > **Iniciar Depuração** ou prima **F5**.

    ![Captura de ecrã da aplicação Visual Studio, com a opção Iniciar Depuração realçada](media/sdk/qs-csharp-uwp-09-start-debugging.png "Iniciar a depuração da aplicação")

1. Surge uma janela. Selecione **Ativar Microfone** e confirme o pedido de permissão que aparece.

    ![Captura de ecrã do pedido de permissão](media/sdk/qs-csharp-uwp-10-access-prompt.png "Iniciar a aplicação para depuração")

1. Selecione **Reconhecimento de voz com entrada do microfone** e fale uma frase ou um trecho em inglês no microfone do dispositivo. A sua voz é transmitida ao serviço de Voz e convertida para texto que é apresentado na janela.

    ![Captura de ecrã da interface de utilizador do reconhecimento de voz](media/sdk/qs-csharp-uwp-11-ui-result.png)

[!INCLUDE [Download this sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Procure este exemplo na pasta `quickstart/csharp-uwp`.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Reconhecer intenções de voz com o SDK de Voz para C#](how-to-recognize-intents-from-speech-csharp.md)

## <a name="see-also"></a>Consulte também

- [Traduzir voz](how-to-translate-speech-csharp.md)
- [Personalizar modelos acústicos](how-to-customize-acoustic-models.md)
- [Personalizar modelos de idioma](how-to-customize-language-model.md)
