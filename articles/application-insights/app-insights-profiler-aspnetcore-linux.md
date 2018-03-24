---
title: Perfil de aplicações web do ASP.NET core Linux do Azure com o gerador de perfis do Application Insights | Microsoft Docs
description: Descrição geral de conceito e tutorial passo a passo sobre como ativar
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
ms.openlocfilehash: 63a7ceacffe1ee33227d3a8272dda7de7b3b1135
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="profile-aspnet-core-azure-linux-web-apps-with-application-insights-profiler"></a>Perfil ASP.NET Core as aplicações Web de Linux do Azure, com o gerador de perfis do Application Insights

Esta funcionalidade está atualmente em pré-visualização

Saber quanto tempo é gasto a cada método de aplicação web em direto quando utilizar [Application Insights](app-insights-overview.md). O gerador de perfis está agora disponível para aplicações web núcleo ASP.NET alojadas no Linux nos serviços de aplicação. Este guia fornece instruções passo a passo sobre como os rastreios de gerador de perfis podem ser recolhidos para aplicações web do ASP.NET core Linux.

Depois de concluir estas instruções, a aplicação irá recolher rastreios de gerador de perfis semelhantes à captura de ecrã abaixo. Neste exemplo, o rastreio de gerador de perfis indica que um pedido web específico estiver lento, porque a maioria das tempo é gasto em espera. O caminho de acesso frequente no código que abrandado a aplicação é precedido pelo ícone de flame. Este exemplo mostra `About` método `HomeController` abrandado a aplicação web, porque foi chamar `Thread.Sleep`.

![Rastreios de gerador de perfis](./media/app-insights-profiler-aspnetcore-linux/profiler-traces.png)

## <a name="pre-requisites"></a>Pré-requisitos
Instruções abaixo aplicadas a todos os ambientes de desenvolvimento do Windows, Linux e Mac:

* Instalar [.NET core SDK 2.1.2 ou posterior](https://www.microsoft.com/net/download/windows/build)
* Instalar o Git seguir as instruções apresentadas em [introdução - instalar o Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)

## <a name="setup-project-locally"></a>Configurar o projeto localmente

1. Abra uma linha de comandos no seu computador. As instruções abaixo funciona para todos os ambientes de desenvolvimento do Windows, Linux e Mac.

2. Criar uma aplicação web de MVC do ASP.NET core
    ```
    dotnet new mvc -n LinuxProfilerTest
    ```
3. Altere o diretório na linha de comandos para a pasta de raiz do projeto

4. Adicione o pacote Nuget para recolher rastreios de gerador de perfis.
    ```
    dotnet add package Microsoft.ApplicationInsights.Profiler.AspNetCore
    ```
5. Adicionar uma linha de código para atrasar aleatoriamente alguns segundos em HomeController.cs

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
6. Guarde e consolidar as alterações para o repositório local

    ```
        git init
        git add .
        git commit -m "first commit"
    ```

## <a name="create-azure-app-service-for-hosting-your-project"></a>Criar App Service do Azure para alojar o seu projeto
1. Criar um ambiente de Linux de serviços de aplicação

    ![Criar serviços de aplicação do Linux](./media/app-insights-profiler-aspnetcore-linux/create-linux-appservice.png)

2. Crie credenciais de implementação. Tome nota da sua palavra-passe conforme irá precisar deste mais tarde quando implementar a sua aplicação.

    ![Criar as credenciais de implementação](./media/app-insights-profiler-aspnetcore-linux/create-deployment-credentials.png)

3. Escolha a opção de implementação. Configure um repositório de Git local na aplicação web, seguindo as instruções do portal do Azure. Será automaticamente criado um repositório de Git.

    ![Repositório de Git do programa de configuração](./media/app-insights-profiler-aspnetcore-linux/setup-git-repo.png)

Estão disponíveis mais opções de implementação [aqui](https://docs.microsoft.com/azure/app-service/containers/choose-deployment-type)

## <a name="deploy-your-project"></a>Implementar o projeto

1. Na sua linha de comandos, navegue para a pasta de raiz do projeto. Adicione o repositório do Git remoto para apontar para um nos serviços de aplicação:

    ```
    git remote add azure https://<username>@<app_name>.scm.azurewebsites.net:443/<app_name>.git
    ```
    * Utilizar 'username' a partir do passo de "criar credenciais de implementação".
    * Utilizar o 'nome de aplicação' do passo de "criar serviço de aplicações".

2. Implementar o projeto ao enviar as alterações para Azure

    ```
    git push azure master
    ```
Irá ver o resultado semelhante ao seguinte:

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
1. [Crie um recurso do Application Insights](./app-insights-create-new-resource.md)
2. Copie a iKey do recurso Application Insights e defina as seguintes definições nos serviços de aplicação

    ```
    APPINSIGHTS_INSTRUMENTATIONKEY: [YOUR_APPINSIGHTS_KEY]
    ASPNETCORE_HOSTINGSTARTUPASSEMBLIES: Microsoft.ApplicationInsights.Profiler.AspNetCore
    ```

    ![Configurar as definições de aplicação](./media/app-insights-profiler-aspnetcore-linux/set-appsettings.png)

    Alterar as definições de aplicação irá reiniciar automaticamente o site. Depois das novas definições são aplicadas, será iniciado o gerador de perfis de executar imediatamente 2 minutos. em seguida, será executada de hora a hora de 2 minutos.

3. Gere algum tráfego ao seu Web site. Pode atualizar o site ```About``` página algumas vezes.

4. Aguarde 2 a 5 minutos para que os eventos podem ser agregados ao Application Insights.

5. Navegue para o painel de desempenho do Application Insights no portal do Azure. Irá ver os rastreios de gerador de perfis disponíveis no canto inferior direito.

    ![Rastreios de vista](./media/app-insights-profiler-aspnetcore-linux/view-traces.png)

## <a name="known-issues"></a>Problemas conhecidos

### <a name="enable-button-in-profiler-configuration-pane-does-not-work"></a>Permitir botão na configuração de gerador de perfis painel não funciona
**Se alojar a sua aplicação com o Linux de serviços de aplicação, não precisa de ativar novamente o gerador de perfis no painel de desempenho no portal das informações de aplicação. Incluindo o pacote NuGet no projeto e a definição de aplicação Insights iKey nas definições de aplicação são suficientes para ativar o gerador de perfis**.

Se seguir o [App Insights gerador de perfis para o Windows](./app-insights-profiler.md) fluxo de trabalho de ativação de clicar em **ativar** no painel de configurar o gerador de perfis, irá receber um erro, como o botão será a tentar instalar a versão do Windows do agente de gerador de perfis no ambiente do Linux.

Estamos a trabalhar sobre como resolver este problema na experiência de ativação.

![Não precisa de ativar o gerador de perfis novamente no painel de desempenho tornar o gerador de perfis de trabalho nos serviços de aplicação do Linux](./media/app-insights-profiler-aspnetcore-linux/issue-enable-profiler.png)


## <a name="next-steps"></a>Próximos Passos
Se estiver a utilizar contentores personalizadas alojadas pelos serviços de aplicação, siga as instruções do [ ativar o serviço de gerador de perfis de aplicação de ASP.NET Core](https://github.com/Microsoft/ApplicationInsights-Profiler-AspNetCore/tree/master/examples/EnableServiceProfilerForContainerApp) para ativar o gerador de perfis do App Insights

Se tiver quaisquer problemas ou sugestões, comunique ao nosso repositório do github: [AspNetCore de gerador de perfis de ApplicationInsights: problemas](https://github.com/Microsoft/ApplicationInsights-Profiler-AspNetCore/issues)
