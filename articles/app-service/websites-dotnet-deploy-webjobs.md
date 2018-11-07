---
title: Desenvolver e implementar o WebJobs com o Visual Studio - Azure
description: Saiba como desenvolver e implementar WebJobs do Azure para o serviço de aplicações do Azure com o Visual Studio.
services: app-service
documentationcenter: ''
author: ggailey777
manager: erikre
editor: jimbe
ms.assetid: a3a9d320-1201-4ac8-9398-b4c9535ba755
ms.service: app-service
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: vs-azure
ms.workload: azure-vs
ms.date: 09/12/2017
ms.author: glenga;david.ebbo;suwatch;pbatum;naren.soni
ms.openlocfilehash: 08cbff7bc58f5925dee9b77ff195d362af4379d8
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51245748"
---
# <a name="develop-and-deploy-webjobs-using-visual-studio---azure-app-service"></a>Desenvolver e implementar o WebJobs com o Visual Studio - serviço de aplicações do Azure

## <a name="overview"></a>Descrição geral

Este tópico explica como utilizar o Visual Studio para implementar um projeto de aplicação de consola numa aplicação web no [serviço de aplicações](app-service-web-overview.md) como um [WebJob do Azure](https://go.microsoft.com/fwlink/?LinkId=390226). Para obter informações sobre como implementar WebJobs com o [portal do Azure](https://portal.azure.com), consulte [tarefas de execução em segundo plano com o WebJobs](web-sites-create-web-jobs.md).

Quando o Visual Studio implementa um projeto de aplicativo de Console habilitados para o WebJobs, ele executa duas tarefas:

* Copia os ficheiros de tempo de execução para a pasta apropriada na aplicação web (*aplicação_dados/tarefas/contínua* para o WebJobs contínuos, *aplicação_dados/tarefas/acionada* para WebJobs agendados e a pedido).
* Configura [agendador do Azure](https://docs.microsoft.com/azure/scheduler/) tarefas para WebJobs agendados para execução em horários específicos. (Isso não é necessária para o WebJobs contínuos.)

Um projeto de WebJobs capacidade tem os seguintes itens adicionados à mesma:

* O [Microsoft.Web.WebJobs.Publish](http://www.nuget.org/packages/Microsoft.Web.WebJobs.Publish/) pacote NuGet.
* R [Settings webjob publicar](#publishsettings) ficheiro que contém as definições de implementação e o scheduler. 

![Diagrama que mostra o que é adicionado a uma aplicação de consola para permitir uma implantação como um WebJob](./media/websites-dotnet-deploy-webjobs/convert.png)

Pode adicionar estes itens para um projeto de aplicação de consola existente ou utilizar um modelo para criar um novo projeto de aplicação de consola preparada para WebJobs. 

Pode implementar um projeto como um WebJob por si só ou ligá-lo num projeto web para que ele implementa automaticamente sempre que implementar o projeto web. Para ligar a projetos, o Visual Studio inclui o nome do projeto WebJobs habilitado numa [webjobs list.json](#webjobslist) arquivo no projeto da web.

![Diagrama que mostra a vinculação ao projeto de web de projetos de trabalho Web](./media/websites-dotnet-deploy-webjobs/link.png)

## <a name="prerequisites"></a>Pré-requisitos

Se estiver a utilizar o Visual Studio 2015, instale o [Azure SDK para .NET (Visual Studio 2015)](https://azure.microsoft.com/downloads/).

Se estiver a utilizar o Visual Studio 2017, instale o [carga de trabalho de desenvolvimento do Azure](https://docs.microsoft.com/visualstudio/install/install-visual-studio#step-4---select-workloads).

## <a id="convert"></a> Ativar a implementação do WebJobs para um projeto de aplicação de consola existente

Tem duas opções:

* [Ativar a implementação automática com um projeto web](#convertlink).

  Configure um projeto de aplicação de consola existente para que ele implementa automaticamente como um WebJob quando implementar um projeto web. Utilize esta opção quando pretender executar o WebJob na mesma aplicação web em que executar o aplicativo da web relacionados.

* [Ativar a implementação sem um projeto web](#convertnolink).

  Configure um projeto de aplicação de consola existente para implementar como um WebJob por si só, com nenhum link para um projeto web. Utilize esta opção quando pretender executar um WebJob num aplicativo web por si só, com nenhuma aplicação web em execução na aplicação web. Pode querer fazer isso para que seja possível dimensionar os recursos de WebJob independentemente dos recursos da aplicação web.

### <a id="convertlink"></a> Ativar a implementação automática de WebJobs com um projeto da web

1. Com o botão direito no projeto web no **Explorador de soluções**e, em seguida, clique em **Add** > **projeto existente como Azure WebJob**.
   
    ![Projeto existente como WebJob do Azure](./media/websites-dotnet-deploy-webjobs/eawj.png)
   
    O [adicionar WebJob do Azure](#configure) é apresentada a caixa de diálogo.
2. Na **nome do projeto** na lista pendente, selecione a aplicação de consola do projeto para adicionar como um WebJob.
   
    ![Selecionar projeto na caixa de diálogo Adicionar WebJob do Azure](./media/websites-dotnet-deploy-webjobs/aaw1.png)
3. Concluir o [adicionar WebJob do Azure](#configure) caixa de diálogo e clique em **OK**. 

### <a id="convertnolink"></a> Ativar a implementação de WebJobs sem um projeto da web
1. Com o botão direito no projeto de aplicação de consola no **Explorador de soluções**e, em seguida, clique em **publicar como WebJob do Azure...** . 
   
    ![Publicar como WebJob do Azure](./media/websites-dotnet-deploy-webjobs/paw.png)
   
    O [adicionar WebJob do Azure](#configure) caixa de diálogo é apresentada, com o projeto selecionado na **nome do projeto** caixa.
2. Concluir o [adicionar WebJob do Azure](#configure) caixa de diálogo e clique em **OK**.
   
   O **publicar Web** é apresentado o assistente.  Se não pretender publicar imediatamente, feche o assistente. As definições que introduzir são guardadas para quando deseja [implementar o projeto](#deploy).

## <a id="create"></a>Criar um novo projeto habilitados para o WebJobs
Para criar um novo projeto habilitados para o WebJobs, pode utilizar o modelo de projeto de aplicativo de Console e ativar a implementação do WebJobs, conforme explicado [secção anterior](#convert). Como alternativa, pode utilizar o modelo de novo projeto do WebJobs:

* [Utilizar o modelo de novo projeto do WebJobs para um WebJob independente](#createnolink)
  
    Crie um projeto e configurá-lo para implementar por si só como um WebJob, com nenhum link para um projeto web. Utilize esta opção quando pretender executar um WebJob num aplicativo web por si só, com nenhuma aplicação web em execução na aplicação web. Pode querer fazer isso para que seja possível dimensionar os recursos de WebJob independentemente dos recursos da aplicação web.
* [Utilizar o modelo de novo projeto do WebJobs para um WebJob ligado a um projeto da web](#createlink)
  
    Crie um projeto que está configurado para implementar automaticamente como um WebJob quando um projeto web na mesma solução é implementado. Utilize esta opção quando pretender executar o WebJob na mesma aplicação web em que executar o aplicativo da web relacionados.

> [!NOTE]
> O modelo de novo projeto do WebJobs automaticamente instala os pacotes de NuGet e inclui o código na *Program.cs* para o [SDK do WebJobs](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/getting-started-with-windows-azure-webjobs). Se não quiser utilizar o SDK de WebJobs, remova ou altere a `host.RunAndBlock` instrução na *Program.cs*.
> 
> 

### <a id="createnolink"></a> Utilizar o modelo de novo projeto do WebJobs para um WebJob independente
1. Clique em **arquivo** > **novo projeto**e, em seguida, no **novo projeto** clique da caixa de diálogo **Cloud**  >   **WebJob do Azure (.NET Framework)**.
   
    ![Caixa de diálogo do projeto nova que mostra o modelo de WebJob](./media/websites-dotnet-deploy-webjobs/np.png)
2. Siga as indicações mostradas anteriormente ao [tornar o aplicativo de Console, um projeto de WebJobs independente do projeto](#convertnolink).

### <a id="createlink"></a> Utilizar o modelo de novo projeto do WebJobs para um WebJob ligado a um projeto da web
1. Com o botão direito no projeto web no **Explorador de soluções**e, em seguida, clique em **Add** > **novo projeto de WebJob do Azure**.
   
    ![Nova entrada de menu do projeto de WebJob do Azure](./media/websites-dotnet-deploy-webjobs/nawj.png)
   
    O [adicionar WebJob do Azure](#configure) é apresentada a caixa de diálogo.
2. Concluir o [adicionar WebJob do Azure](#configure) caixa de diálogo e clique em **OK**.

## <a id="configure"></a>A caixa de diálogo Adicionar WebJob do Azure
O **adicionar WebJob do Azure** caixa de diálogo permite-lhe introduzir o nome de WebJob e modo de configuração para o WebJob de execução. 

![Adicionar a caixa de diálogo do trabalho Web do Azure](./media/websites-dotnet-deploy-webjobs/aaw2.png)

Os campos nesta caixa de diálogo correspondem aos campos na **WebJob adicionar** caixa de diálogo do portal do Azure. Para obter mais informações, consulte [tarefas de execução em segundo plano com o WebJobs](web-sites-create-web-jobs.md).

> [!NOTE]
> * Para obter informações sobre a implementação da linha de comandos, consulte [ativação da linha de comandos ou a entrega contínua do WebJobs do Azure](https://azure.microsoft.com/blog/2014/08/18/enabling-command-line-or-continuous-delivery-of-azure-webjobs/).
> * Se implementar um WebJob e, em seguida, decidir que pretende alterar o tipo de WebJob e volte a implementar, terá de eliminar a *Settings webjobs publicar* ficheiro. Esta ação irá tornar o Visual Studio para mostrar as opções de publicação mais uma vez, para que possa alterar o tipo de trabalho Web.
> * Se implementar um WebJob e posteriormente alterar o modo de execução do contínua para não contínuos e vice-versa, o Visual Studio cria um WebJob de novo no Azure quando implementar novamente. Se alterar outras definições de agendamento, mas deixe modo de execução o mesmo ou alternar entre agendada e a pedido, Visual Studio atualiza a tarefa existente em vez de criar um novo.
> 
> 

## <a id="publishsettings"></a>webjob-publish-settings.json
Quando configurar uma aplicação de consola para a implementação de WebJobs, o Visual Studio instala o [Microsoft.Web.WebJobs.Publish](http://www.nuget.org/packages/Microsoft.Web.WebJobs.Publish/) NuGet pacote e arquivos de agendamento de informações num *Settings webjob publicar*  arquivo no projeto *propriedades* pasta do projeto WebJobs. Eis um exemplo desse ficheiro:

        {
          "$schema": "http://schemastore.org/schemas/json/webjob-publish-settings.json",
          "webJobName": "WebJob1",
          "startTime": "null",
          "endTime": "null",
          "jobRecurrenceFrequency": "null",
          "interval": null,
          "runMode": "Continuous"
        }

Pode editar este ficheiro diretamente, e o Visual Studio fornece o IntelliSense. O esquema de ficheiro é armazenado em [ http://schemastore.org ](http://schemastore.org/schemas/json/webjob-publish-settings.json) e pode ser vista aqui.  

## <a id="webjobslist"></a>webjobs-list.json
Quando liga um projeto habilitados para o WebJobs para um projeto web, o Visual Studio armazena o nome do projeto de WebJobs uma *webjobs list.json* arquivo no projeto da web *propriedades* pasta. A lista pode conter vários projetos de WebJobs, conforme mostrado no exemplo a seguir:

        {
          "$schema": "http://schemastore.org/schemas/json/webjobs-list.json",
          "WebJobs": [
            {
              "filePath": "../ConsoleApplication1/ConsoleApplication1.csproj"
            },
            {
              "filePath": "../WebJob1/WebJob1.csproj"
            }
          ]
        }

Pode editar este ficheiro diretamente, e o Visual Studio fornece o IntelliSense. O esquema de ficheiro é armazenado em [ http://schemastore.org ](http://schemastore.org/schemas/json/webjobs-list.json) e pode ser vista aqui.

## <a id="deploy"></a>Implementar um projeto de WebJobs
Um projeto de WebJobs que a ligação a um projeto web implementa automaticamente com o projeto de web. Para obter informações sobre a implantação do projeto de web, consulte **guias de procedimentos** > **implementar aplicação** na navegação à esquerda.

Para implementar um projeto de WebJobs por si só, com o botão direito do rato no **Explorador de soluções** e clique em **publicar como WebJob do Azure...** . 

![Publicar como WebJob do Azure](./media/websites-dotnet-deploy-webjobs/paw.png)

Para um WebJob independente, o mesmo **publicar Web** assistente que é utilizado para projetos web é apresentada, mas com menos definições disponíveis para alterar.
