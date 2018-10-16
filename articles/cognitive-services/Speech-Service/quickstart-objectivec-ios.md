---
title: 'Início rápido: Recognize speech no Objective-C no iOS com o SDK de voz dos serviços cognitivos'
titleSuffix: Microsoft Cognitive Services
description: Aprender a reconhecer a conversão de voz no Objective-C no iOS com o SDK de voz dos serviços cognitivos
services: cognitive-services
author: chlandsi
ms.service: cognitive-services
ms.component: Speech
ms.topic: article
ms.date: 10/12/2018
ms.author: chlandsi
ms.openlocfilehash: ce9979d8d300f2308a4b7a22791c242409f2c988
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/16/2018
ms.locfileid: "49341179"
---
# <a name="quickstart-recognize-speech-in-objective-c-on-ios-using-the-cognitive-services-speech-sdk"></a>Início rápido: Recognize speech no Objective-C no iOS com o SDK de voz dos serviços cognitivos

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Neste artigo, saiba como criar uma aplicação iOS no Objective-C com o SDK de voz dos serviços cognitivos de transcrever um arquivo de áudio com gravado de voz em texto.

## <a name="prerequisites"></a>Pré-requisitos

* Uma chave de subscrição para o serviço de voz. Ver [experimentar gratuitamente o serviço de voz](get-started.md).
* Um Mac com Xcode 9.4.1 instalado como ambiente de desenvolvimento de iOS. Este tutorial destina-se versões 11.4 iOS. Se não tiver Xcode ainda, pode instalá-lo do [App Store](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12).

## <a name="get-the-speech-sdk-for-ios"></a>Obtenha o SDK de voz para iOS

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

A versão atual do SDK de Voz dos Serviços Cognitivos é `1.0.1`.

O SDK de voz dos serviços cognitivos para Mac e iOS atualmente é distribuído como uma estrutura de Cocoa.
Pode ser transferido a partir https://aka.ms/csspeech/iosbinary. Transfira o ficheiro para o diretório raiz.

## <a name="create-an-xcode-project"></a>Criar um projeto do Xcode 

Inicie o Xcode e iniciar um novo projeto clicando **arquivo** > **New** > **projeto**.
Na caixa de diálogo de seleção de modelo, escolha o "iOS única vista de aplicação" modelo.

As caixas de diálogo que siga, faça as seleções seguintes:

1. Caixa de diálogo de opções de projeto
    1. Introduza um nome para a aplicação de início rápido, por exemplo `helloworld`.
    1. Introduza um nome de organização adequada e o identificador de organização, se já tiver uma conta de programador da Apple. Para fins de teste, apenas pode escolher qualquer nome, como `testorg`. Para se inscrever a aplicação, também tem um perfil de aprovisionamento correto. Consulte a [site do desenvolvedor Apple](https://developer.apple.com/) para obter detalhes.
    1. Certifique-se de que Objective-C é escolhido como o idioma para o projeto.
    1. Desative todas as caixas de verificação para testes e os dados principais.
    ![Definições do projeto](media/sdk/qs-objectivec-project-settings.png)
1. Selecione o diretório do projeto
    1. Escolha o diretório de raiz para colocar o projeto. Esta ação irá criar um `helloworld` diretório no seu diretório de raiz que contém todos os ficheiros para o projeto Xcode.
    1. Desative a criação de um repositório de Git para este projeto de exemplo.
    1. Ajustar os caminhos para o SDK no *definições do projeto*.
        1. No **gerais** separador sob a **binários incorporados** cabeçalho, adicionar a biblioteca do SDK como uma estrutura: **adicionar os binários incorporados** > **adicionar outro ...**  > Navegar para o diretório raiz e escolha o ficheiro `MicrosoftCognitiveServicesSpeech.framework`. Isso também adiciona automaticamente a biblioteca do SDK para o cabeçalho **ligado Framework e bibliotecas**.
        ![Framework foi adicionado](media/sdk/qs-objectivec-framework.png)
        1. Vá para o **definições de criação** separador e ativar **todos os** definições.
        1. Adicione o diretório `$(SRCROOT)/..` para o *caminhos de pesquisa do Framework* sob o **caminhos de pesquisa** cabeçalho.
        ![Definição do caminho de pesquisa de estrutura](media/sdk/qs-objectivec-framework-search-paths.png)

## <a name="set-up-the-ui"></a>Definir a interface do usuário

A aplicação de exemplo terá uma interface do Usuário muito simples: dois botões para iniciar o reconhecimento de voz a partir de ficheiro ou de entrada do microfone e uma etiqueta de texto para exibir o resultado.
A interface do Usuário for definido no `Main.storyboard` parte do projeto.
Abrir a vista XML do storyboard clicando com o `Main.storyboard` entrada do projeto de árvore e selecionar **aberto como...**   >  **Código-fonte**.
Substitua o XML do gerado automaticamente com isto:

[!code-xml[](~/samples-cognitive-services-speech-sdk/quickstart/objectivec-ios/helloworld/helloworld/Base.lproj/Main.storyboard)]

## <a name="add-the-sample-code"></a>Adicione o código de exemplo

1. Transfira o [arquivo wav de exemplo](https://raw.githubusercontent.com/Azure-Samples/cognitive-services-speech-sdk/f9807b1079f3a85f07cbb6d762c6b5449d536027/samples/cpp/windows/console/samples/whatstheweatherlike.wav) ao clicar na ligação e escolher **Guardar destino como...** . Adicione o arquivo wav ao projeto como um recurso ao arrastá-lo a partir de uma janela do localizador para o nível de raiz de vista de projeto.
Clique em **concluir** na caixa de diálogo seguinte sem alterar as definições.
1. Substitua os conteúdos do gerado automaticamente `ViewController.m` de ficheiros por:

   [!code-objectivec[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/objectivec-ios/helloworld/helloworld/ViewController.m#code)]
1. Substitua a cadeia de carateres `YourSubscriptionKey` pela sua chave de subscrição.
1. Substitua a cadeia de carateres `YourServiceRegion` pela [região](regions.md) associada à subscrição (por exemplo, `westus` para a subscrição de avaliação gratuita).
1. Adicione o pedido de acesso ao microfone. Clique com botão direito a `Info.plist` entrada da árvore de projeto e selecione **aberto como...**   >  **Código-fonte**. Adicione as seguintes linhas para o `<dict>` secção e, em seguida, guarde o ficheiro.
    ```xml
    <key>NSMicrophoneUsageDescription</key>
    <string>Need microphone access for speech recognition from microphone.</string>
    ```

## <a name="building-and-running-the-sample"></a>Criar e executar o exemplo

1. Tornar a saída de depuração visível (**View** > **depurar área** > **ativar consola**).
1. Escolha o simulador do iOS ou um dispositivo iOS ligado para o seu computador de desenvolvimento como o destino para a aplicação na lista os **produto** -> **destino** menu.
1. Criar e executar o código de exemplo no simulador do iOS selecionando **produto** -> **executar** no menu ou clicando no **reproduzir** botão.
Atualmente, o SDK de voz apenas suporta plataformas iOS de 64 bits.
1. Depois de clicar no botão "Recognize (arquivo)" na aplicação, deverá ver o conteúdo do arquivo de áudio "Qual é o clima, como"? na parte inferior do ecrã.

 ![Aplicação do iOS simulada](media/sdk/qs-objectivec-simulated-app.png)

1. Depois de clicar no botão "Recognize (microfone)" na aplicação e dizer algumas palavras, deverá ver o texto que tenha se apresentou na parte inferior do ecrã.

[!INCLUDE [Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Procure este exemplo na pasta `quickstart/objectivec-ios`.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Obtenha os nossos exemplos](speech-sdk.md#get-the-samples)

