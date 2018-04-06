---
title: Perfil de aplicações web de ASP.NET Core Azure Linux com o gerador de perfis do Application Insights | Microsoft Docs
description: Uma descrição geral conceptual e um tutorial passo a passo sobre como utilizar o gerador de perfis do Application Insights.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 02/23/2018
ms.author: mbullwin
ms.openlocfilehash: 5596c4efeba14e9d2bfdadd7ce92bb6b2c9fcbf0
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/05/2018
---
# <a name="profile-aspnet-core-azure-linux-web-apps-with-application-insights-profiler"></a>Web apps do perfil ASP.NET Core Azure Linux com o gerador de perfis do Application Insights

Esta funcionalidade encontra-se em pré-visualização.

Saber quanto tempo é gasto a cada método de aplicação web em direto quando utilizar [Application Insights](app-insights-overview.md). O gerador de perfis do Application Insights está agora disponível para aplicações web ASP.NET Core que estão alojadas em Linux no App Service do Azure. Este guia fornece instruções passo a passo sobre como os rastreios de gerador de perfis podem ser recolhidos para aplicações web de ASP.NET Core Linux.

Depois de concluir estas instruções, a aplicação pode recolher rastreios de gerador de perfis, como os rastreios que são apresentados na imagem. Neste exemplo, o rastreio de gerador de perfis indica que um pedido web específico lento devido a tempo gasto espera. O *caminho frequente* no código que é abrandamento a aplicação esteja marcada por um ícone de flame. O **sobre** método o **HomeController** secção é abrandamento da aplicação web, porque está a chamar o método de **Sleep** função.

![Rastreios de gerador de perfis](./media/app-insights-profiler-aspnetcore-linux/profiler-traces.png)

## <a name="prerequisites"></a>Pré-requisitos
As seguintes instruções aplicam-se a todos os ambientes de desenvolvimento Windows, Linux e Mac:

* Instalar o [.NET Core SDK 2.1.2 ou posterior](https://www.microsoft.com/net/download/windows/build).
* Instalar o Git, seguindo as instruções apresentadas em [introdução - instalar o Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).

## <a name="set-up-the-project-locally"></a>Configurar o projeto localmente

1. Abra uma janela de linha de comandos no seu computador. As seguintes instruções funcionam para todos os ambientes de desenvolvimento Windows, Linux e Mac.

2. Crie uma aplicação web de MVC do ASP.NET Core:

    ```
    dotnet new mvc -n LinuxProfilerTest
    ```

3. Altere o diretório de trabalho para a pasta de raiz para o projeto.

4. Adicione o pacote NuGet para recolher rastreios de gerador de perfis:

    ```
    dotnet add package Microsoft.ApplicationInsights.Profiler.AspNetCore
    ```

5. Adicionar uma linha de código no **Homecontroller** secção aleatoriamente atrasar a alguns segundos:

    ```csharp
        using System.Threading;
        ...

        public IActionResult About()
            {
                Random r = new Random();
                int delay = r.Next(5000, 10000);
                Thread.Sleep(delay);
                return View();
            }
    ```

6. Guarde e consolidar as alterações para o repositório local:

    ```
        git init
        git add .
        git commit -m "first commit"
    ```

## <a name="create-the-linux-web-app-to-host-your-project"></a>Criar a aplicação web do Linux para alojar o seu projeto

1. Crie o ambiente de aplicação web utilizando o serviço de aplicações no Linux:

    ![Criar a aplicação web do Linux](./media/app-insights-profiler-aspnetcore-linux/create-linux-appservice.png)

2. Crie as credenciais de implementação:

    > [!NOTE]
    > Registe a sua palavra-passe para utilizar mais tarde quando implementar a aplicação web.

    ![Criar as credenciais de implementação](./media/app-insights-profiler-aspnetcore-linux/create-deployment-credentials.png)

3. Escolha as opções de implementação. Configure um repositório de Git local na aplicação web, seguindo as instruções no portal do Azure. É criado automaticamente um repositório de Git.

    ![Configurar o repositório de Git](./media/app-insights-profiler-aspnetcore-linux/setup-git-repo.png)

Para obter mais opções de implementação, consulte [neste artigo](https://docs.microsoft.com/azure/app-service/containers/choose-deployment-type).

## <a name="deploy-your-project"></a>Implementar o projeto

1. Na janela da linha de comandos, navegue para a pasta de raiz para o seu projeto. Adicione um repositório do Git remoto para apontar para o repositório no App Service:

    ```
    git remote add azure https://<username>@<app_name>.scm.azurewebsites.net:443/<app_name>.git
    ```

    * Utilize o **username** que utilizou para criar as credenciais de implementação.
    * Utilize o **nome da aplicação** que utilizou para criar a aplicação web utilizando o serviço de aplicações no Linux.

2. Implemente o projeto ao enviar as alterações para Azure:

    ```
    git push azure master
    ```

Deverá ver um resultado semelhante ao seguinte exemplo:

    ```
    Counting objects: 9, done.
    Delta compression using up to 8 threads.
    Compressing objects: 100% (8/8), done.
    Writing objects: 100% (9/9), 1.78 KiB | 911.00 KiB/s, done.
    Total 9 (delta 3), reused 0 (delta 0)
    remote: Updating branch 'master'.
    remote: Updating submodules.
    remote: Preparing deployment for commit id 'd7369a99d7'.
    remote: Generating deployment script.
    remote: Running deployment command...
    remote: Handling ASP.NET Core Web Application deployment.
    remote: ......
    remote:   Restoring packages for /home/site/repository/EventPipeExampleLinux.csproj...
    remote: .
    remote:   Installing Newtonsoft.Json 10.0.3.
    remote:   Installing Microsoft.ApplicationInsights.Profiler.Core 1.1.0-LKG
    …

    ```

## <a name="add-application-insights-to-monitor-your-web-apps"></a>Adicionar o Application Insights para monitorizar as suas aplicações web

1. [Crie um recurso do Application Insights](./app-insights-create-new-resource.md).

2. Copiar o **iKey** valor do recurso Application Insights e defina as seguintes definições nas suas aplicações web:

    ```
    APPINSIGHTS_INSTRUMENTATIONKEY: [YOUR_APPINSIGHTS_KEY]
    ASPNETCORE_HOSTINGSTARTUPASSEMBLIES: Microsoft.ApplicationInsights.Profiler.AspNetCore
    ```

    ![Configurar as definições da aplicação](./media/app-insights-profiler-aspnetcore-linux/set-appsettings.png)

    Quando as definições da aplicação são alteradas, o site é reiniciado automaticamente. Depois de serem aplicadas as novas definições, o gerador de perfis é executado imediatamente para dois minutos. Em seguida, executa o gerador de perfis de dois minutos a cada hora.

3. Gere algum tráfego ao seu Web site. Pode gerar tráfego por atualizar o site **sobre** página algumas vezes.

4. Aguarde duas a cinco minutos para os eventos a agregação para o Application Insights.

5. Navegue para o Application Insights **desempenho** painel no portal do Azure. Pode ver os rastreios de gerador de perfis no direito na parte inferior do painel.

    ![Ver os rastreios de gerador de perfis](./media/app-insights-profiler-aspnetcore-linux/view-traces.png)

## <a name="known-issues"></a>Problemas conhecidos

### <a name="the-enable-action-in-the-profiler-configuration-pane-doesnt-work"></a>A ação de ativação no painel de configuração do gerador de perfis não funciona

> [!NOTE]
> Se alojar a aplicação através do serviço de aplicações no Linux, não terá de voltar a ativar o gerador de perfis no **desempenho** painel no portal do Application Insights. Pode incluir o pacote NuGet do seu projeto e definir o Application Insights **iKey** valor nas suas definições de aplicação web para ativar o gerador de perfis.

Se seguir o fluxo de trabalho de ativação para [Application Insights gerador de perfis para o Windows](./app-insights-profiler.md) e selecione **ativar** no **configurar o gerador de perfis** painel, receberá um erro. A ação de ativação tenta instalar a versão do Windows do agente do gerador de perfis no ambiente de Linux.

Estamos a trabalhar uma resolução possível para este problema.

![Não tente voltar a ativar o gerador de perfis no painel de desempenho](./media/app-insights-profiler-aspnetcore-linux/issue-enable-profiler.png)


## <a name="next-steps"></a>Passos Seguintes
Se utilizar contentores personalizados que são alojadas pelo App Service do Azure, siga as instruções em [ ativar o serviço de gerador de perfis para uma aplicação ASP.NET Core de](https://github.com/Microsoft/ApplicationInsights-Profiler-AspNetCore/tree/master/examples/EnableServiceProfilerForContainerApp) para ativar o gerador de perfis do Application Insights.

Reportar a quaisquer problemas ou sugestões para o repositório do GitHub do Application Insights: [AspNetCore de gerador de perfis de ApplicationInsights: problemas](https://github.com/Microsoft/ApplicationInsights-Profiler-AspNetCore/issues).
