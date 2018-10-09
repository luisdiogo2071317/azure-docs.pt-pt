---
author: wolfma61
ms.service: cognitive-services
ms.topic: include
ms.date: 09/13/2018
ms.author: wolfma
ms.openlocfilehash: 8ff8e8341b6f39f66c2cc8014d41d3d3a2918d2b
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/28/2018
ms.locfileid: "47454406"
---
1. Inicie o Visual Studio 2017.
 
1. Certifique-se de que a carga de trabalho do **ambiente de trabalho .NET** está disponível. Selecione **Ferramentas** \> **Obter Ferramentas e Funcionalidades** na barra de menus do Visual Studio para abrir o instalador do Visual Studio. Se esta carga de trabalho já estiver ativada, feche a caixa de diálogo. 

    Caso contrário, selecione a caixa de verificação junto a **Desenvolvimento do ambiente de trabalho .NET** e, em seguida, clique no botão **Modificar** no canto inferior direito da caixa de diálogo. A instalação da nova funcionalidade demora alguns minutos.

    ![Ativar o desenvolvimento do ambiente de trabalho .NET](~/articles/cognitive-services/speech-service/media/sdk/vs-enable-net-desktop-workload.png)

1. Crie uma nova Aplicação de Consola do Visual C#. Na caixa de diálogo **Novo Projeto**, no painel à esquerda, expanda **Instalado** \> **Visual C#** \> **Ambiente de Trabalho do Windows** e, em seguida, selecione **Aplicação de Consola (.NET Framework)**. No nome do projeto, introduza *helloworld*.

    ![Criar Uma Aplicação de Consola do Visual C# (.NET Framework)](~/articles/cognitive-services/speech-service/media/sdk/qs-csharp-dotnet-windows-01-new-console-app.png "Criar Uma Aplicação de Consola do Visual C# (.NET Framework)")

1. Instale e faça referência ao [pacote NuGet do SDK de Voz](https://aka.ms/csspeech/nuget). No Explorador de Soluções, clique com o botão direito do rato na solução e selecione **Gerir Pacotes NuGet para Solução**.

    ![Clique com o botão direito do rato em Gerir Pacotes NuGet para Solução](~/articles/cognitive-services/speech-service/media/sdk/qs-csharp-dotnet-windows-02-manage-nuget-packages.png "Gerir Pacotes NuGet para Solução")

1. No canto superior direito, no campo **Origem do Pacote**, selecione **nuget.org**. Procure o pacote `Microsoft.CognitiveServices.Speech` e instale-o no projeto **helloworld**.

    ![Instalar o Pacote NuGet Microsoft.CognitiveServices.Speech](~/articles/cognitive-services/speech-service/media/sdk/qs-csharp-dotnet-windows-03-nuget-install-1.0.0.png "Instalar o Pacote NuGet")

1. Aceite a licença apresentada para iniciar a instalação do pacote NuGet.

    ![Aceitar a licença](~/articles/cognitive-services/speech-service/media/sdk/qs-csharp-dotnet-windows-04-nuget-license.png "Aceitar a licença")

    Depois de o pacote ser instalado, será apresentada uma mensagem de confirmação na consola do Gestor de Pacotes.

1. Crie uma configuração de plataforma que corresponda à arquitetura do PC através do Configuration Manager. Selecione **Criar** > **Configuration Manager**.

    ![Iniciar o Configuration Manager](~/articles/cognitive-services/speech-service/media/sdk/qs-csharp-dotnet-windows-05-cfg-manager-click.png "Iniciar o Configuration Manager")

1. Na caixa de diálogo **Configuration Manager**, adicione uma nova plataforma. Na lista pendente **Plataforma de solução ativa**, selecione **Novo**.

    ![Adicionar uma nova plataforma na janela do Configuration Manager](~/articles/cognitive-services/speech-service/media/sdk/qs-csharp-dotnet-windows-06-cfg-manager-new.png "Adicionar uma nova plataforma na janela do Configuration Manager")

1. Se estiver a executar o Windows de 64 bits, crie uma nova configuração de plataforma com o nome `x64`. Se estiver a executar o Windows de 32 bits, crie uma nova configuração de plataforma com o nome `x86`.

    ![No Windows de 64 bits, adicione uma nova plataforma com o nome "x64"](~/articles/cognitive-services/speech-service/media/sdk/qs-csharp-dotnet-windows-07-cfg-manager-add-x64.png "Adicionar plataforma x64")


