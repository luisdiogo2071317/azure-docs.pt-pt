---
title: Resolver problemas relacionados com o Azure Application Insights Profiler | Documentos da Microsoft
description: Esta página contém passos de resolução de problemas e informações para ajudar os desenvolvedores que estão a ter problemas ao ativar ou usando o criador de perfil do Application Insights.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.reviewer: cawa
ms.date: 08/06/2018
ms.author: mbullwin
ms.openlocfilehash: 28de0f8bdcaa730c5beea0c630d4e86e15642809
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/26/2018
ms.locfileid: "50142563"
---
# <a name="troubleshoot-problems-enabling-or-viewing-application-insights-profiler"></a>Resolução de problemas de ativação ou ao visualizar o Application Insights Profiler

## <a id="troubleshooting"></a>Resolução de problemas

### <a name="profiles-are-only-uploaded-if-there-are-requests-to-your-application-while-the-profiler-is-running"></a>Perfis apenas são carregados se existirem pedidos à sua aplicação, enquanto o profiler está em execução
Application Insights Profiler recolhe dados do gerador de perfil de dois minutos por hora, ou quando o [ **perfil agora** ](app-insights-profiler-settings.md?toc=/azure/azure-monitor/toc.json) botão é pressionado sobre o **configurar Application Insights Profiler**página. Mas os dados de criação de perfis apenas são carregados quando podem ser anexado a um pedido que aconteceu enquanto estava a ser executado o criador de perfil. 

O criador de perfil escreve as mensagens de rastreio e eventos personalizados para o seu recurso do application insights. Pode utilizar esses eventos como o criador de perfil está em execução. Se acha que o criador de perfil deve ser em execução e capturar rastreios, mas não vê-los na página de desempenho, pode verificar como está a executar o criador de perfil:

1. Procure mensagens de rastreio e eventos personalizados enviados pelo criador de perfil para o recurso do Application Insights. Pode usar essa cadeia de caracteres de pesquisa para encontrar os dados relevantes:

    ```
    stopprofiler OR startprofiler OR upload OR ServiceProfilerSample
    ```
    Eis um exemplo de duas pesquisas de dois recursos de IA diferentes na captura de ecrã abaixo. 
    
    * É um no lado esquerdo de um aplicativo que não está a obter pedidos enquanto o profiler está em execução. Pode ver a mensagem que o carregamento foi cancelado devido a nenhuma atividade. 

    * No exemplo à direita, pode ver que o gerador de perfis iniciado e enviados eventos personalizados quando este detetado pedidos que aconteceram enquanto estava a ser executado o criador de perfil. Se vir o evento personalizado ServiceProfilerSample, significa que o criador de perfil anexado um rastreio a um pedido e pode ver o rastreio da página de desempenho do Application Insights.

    * Se não vir qualquer telemetria de todo, em seguida, o criador de perfil não está em execução. Se pretender ler as secções de resolução de problemas para o seu tipo de aplicação específica sobre este documento abaixo.  

     ![Pesquisar telemetria do Profiler][profiler-search-telemetry]

1. Se existirem pedidos durante o período de tempo executou o criador de perfil, certifique-se de pedidos são processados pela parte do aplicativo que tenha o criador de perfil ativado. Por vezes, aplicativos consistem em vários componentes, mas Profiler está ativada apenas para alguns, não todos, os componentes. A página de configurar o Application Insights Profiler irá mostrar os componentes que carregou rastreios.

### <a name="net-core-21-bug"></a>**Bug do .net core 2.1** 
Há um bug no agente do criador de perfil que o impede de carregamento de rastreios obtidos a partir de aplicações em execução no ASP.NET Core 2.1. Estamos a trabalhar numa correção e irá deixá-lo pronto em breve. A correção para esse bug será implantada no final de Outubro.

### <a name="other-things-to-check"></a>**Outros aspetos a verificar:**
* A aplicação está em execução no .NET Framework 4.6.
* Se a sua aplicação web é uma aplicação ASP.NET Core, tem de executar, pelo menos, ASP.NET Core 2.0.
* Se os dados que está a tentar a ver com mais de duas semanas, experimente a limitar o filtro de tempo e tente novamente. Rastreios são eliminados após sete dias.
* Certifique-se de que uma firewall ou proxies não bloqueou o acesso a https://gateway.azureserviceprofiler.net.

### <a id="double-counting"></a>**Contagem de threads paralelos de valor de duplo**

Em alguns casos, a métrica de tempo total no Visualizador de pilha é mais do que a duração do pedido.

Esta situação poderá ocorrer quando dois ou mais threads estão associados um pedido, e que estão a funcionar em paralelo. Nesse caso, o tempo total de threads é mais do que o tempo decorrido. Um thread pode estar esperando por outra seja concluída. O Visualizador tenta detetar esta situação e omite a espera desinteressante, mas ele erra por exibir informações em excesso em vez de omitir o que pode ser informações críticas.

Quando vir threads paralelos em seus rastreios, determine quais segmentos estão aguardando para que pode determinar o caminho crítico para o pedido. Na maioria dos casos, o thread que rapidamente entra num Estado de espera simplesmente está aguardando outros threads. Concentrar-se a outros threads e ignorar o tempo em que os threads em espera.

### <a name="error-report-in-the-profiling-viewer"></a>**Relatório de erro no Visualizador de criação de perfis**
Submeta um pedido de suporte no portal. Certifique-se de que inclui o ID de correlação da mensagem de erro.

## <a name="troubleshooting-profiler-on-app-services"></a>Profiler de resolução de problemas nos serviços de aplicações
### <a name="for-the-profiler-to-work-properly"></a>**Para o criador de perfil funcionar corretamente:**
* Seu plano de serviço de aplicações web tem de ser escalão básico ou superior.
* A aplicação web tem de ter a extensão do Application Insights para serviços de aplicações (2.6.5) instalada.
* A aplicação web tem de ter o **APPINSIGHTS_INSTRUMENTATIONKEY** definição de aplicação configurada com a mesma chave de instrumentação que é utilizada pelo SDK do Application Insights.
* A aplicação web tem de ter o **APPINSIGHTS_PROFILERFEATURE_VERSION** definição de aplicação definida e configurada para 1.0.0.
* O **ApplicationInsightsProfiler2** tem de executar a tarefa de web. Pode verificar a tarefa de web, acedendo a [Kudu](https://blogs.msdn.microsoft.com/cdndevs/2015/04/01/the-kudu-debug-console-azure-websites-best-kept-secret/)e abrir o **Dashboard de WebJobs** no menu Ferramentas. Como pode ver nas capturas de ecrã abaixo, clicando no link de ApplicationInsightsProfiler2, pode ver detalhes do webjob, incluindo o registo.

    Segue-se a ligação que tem de clicar para ver os detalhes do trabalho Web: 

    ![Criador de perfil do webjob]

    Aqui é a página que mostra os detalhes. Pode transferir o registo e enviá-lo à nossa equipe, se não pode descobrir por que o criador de perfil não está a funcionar para.
    
    ![registo de criador de perfil de trabalho Web]

### <a name="manual-installation"></a>**Instalação manual**

Quando configura o Profiler, as atualizações são efetuadas às definições da aplicação web. Se o seu ambiente requer pode aplicar as atualizações manualmente. Por exemplo, poderá que seu aplicativo seja executado num ambiente de aplicações Web para o PowerApps.

1. Na **controlo de aplicação Web** painel, abra **definições**.
1. Definir **.Net Framework versão** ao **v4.6**.
1. Definir **Always On** ao **no**.
1. Adicionar a **APPINSIGHTS_INSTRUMENTATIONKEY** aplicação definição e defina o valor para a mesma chave de instrumentação que é utilizada pelo SDK.
1. Adicionar a **APPINSIGHTS_PROFILERFEATURE_VERSION** definição de aplicação e defina o valor como 1.0.0.
1. Open **ferramentas avançadas**.
1. Selecione **ir** para abrir o site Kudu.
1. No site do Kudu, selecione **extensões de Site**.
1. Instale **Application Insights** da Galeria de aplicações Web do Azure.
1. Reinicie a aplicação web.

### <a name="too-many-active-profiling-sessions"></a>**Demasiadas sessões ativas de criação de perfis**

Atualmente, pode ativar Profiler, no máximo, quatro de web do Azure de aplicações e ranhuras de implementação que estão em execução no mesmo plano de serviço. Se a tarefa de web do Profiler está a comunicar demasiadas sessões ativas de criação de perfis, mova algumas aplicações web para um plano de serviço diferentes.

### <a name="deployment-error-directory-not-empty-dhomesitewwwrootappdatajobs"></a>Erro de implementação: diretório não vazio "D:\\doméstica\\site\\wwwroot\\App_Data\\das tarefas

Se estiver a Reimplementar a aplicação web a um recurso de aplicações Web com o Profiler ativado, poderá ver a mensagem seguinte:

*Diretório não vazio "D:\\doméstica\\site\\wwwroot\\App_Data\\das tarefas*

Este erro ocorre se executar o Web Deploy de scripts ou do Pipeline de implementação de DevOps do Azure. A solução é adicionar os seguintes parâmetros de implementação adicionais para a tarefa de implementação da Web:

```
-skip:Directory='.*\\App_Data\\jobs\\continuous\\ApplicationInsightsProfiler.*' -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data\\jobs\\continuous$' -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data\\jobs$'  -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data$'
```

Estes parâmetros elimine a pasta que é utilizada pelo Application Insights Profiler e desbloquear o processo de voltar a implementar. Não afetam a instância de Profiler está atualmente em execução.

### <a name="how-do-i-determine-whether-application-insights-profiler-is-running"></a>Como posso determinar se o Application Insights Profiler está em execução?

Profiler é executado como um webjob contínua na aplicação web. É possível abrir o recurso de aplicação web no [portal do Azure](https://portal.azure.com). Na **WebJobs** painel, verifique o estado dos **ApplicationInsightsProfiler**. Se não estiver em execução, abra **registos** para obter mais informações.

## <a name="troubleshooting-problems-with-profiler-and-wad"></a>Resolução de problemas com o Profiler e WAD

Existem três coisas para verificar que o criador de perfil está corretamente configurada fazendo com WAD. Em primeiro lugar, terá de verificar que o conteúdo da configuração WAD que é implementado é os esperados. Em segundo lugar, precisa verificar que WAD passa a iKey correta na linha de comandos de criador de perfil. Em terceiro lugar, pode verificar o ficheiro de registo do criador de perfil para ver se o gerador de perfis foi executada mas está recebendo um erro. 

Para verificar as definições que foram utilizadas para configurar WAD, terá de iniciar sessão na VM e abrir o ficheiro de registo nesta localização: 
```
c:\logs\Plugins\Microsoft.Azure.Diagnostics.PaaSDiagnostics\1.11.3.12\DiagnosticsPlugin.logs  
```
Nesse arquivo, pode procurar a cadeia de caracteres "WadCfg" e encontrará as definições que foram transmitidas para a VM para configurar WAD. Pode verificar esse iKey usado pelo coletor de criador de perfil está correto.

Em segundo lugar, pode verificar a linha de comando que é utilizada para iniciar o gerador de perfis. O ficheiro seguinte contém os argumentos usados para iniciar o gerador de perfis.
```
D:\ProgramData\ApplicationInsightsProfiler\config.json
```
Verifique se a ikey na linha de comandos de criador de perfil está correta. 

Em terceiro lugar, a utilizar o caminho encontrado no arquivo config acima, verifique o ficheiro de registo do criador de perfil. Ela mostrará as informações de depuração que indica as mensagens de estado e o erro do criador de perfil e configurações que é usando o criador de perfil. Se o criador de perfil está em execução enquanto seu aplicativo está recebendo solicitações, verá esta mensagem: detetada atividade a partir iKey. Quando o rastreio está a ser carregado, verá esta mensagem: iniciar o rastreio de carregamento. 


[profiler-search-telemetry]:./media/app-insights-profiler/Profiler-Search-Telemetry.png
[Criador de perfil do webjob]:./media/app-insights-profiler/Profiler-webjob.png
[registo de criador de perfil de trabalho Web]:./media/app-insights-profiler/Profiler-webjob-log.png








