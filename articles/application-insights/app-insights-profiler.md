---
title: Perfil de aplicações web em direto no Azure com o Application Insights Profiler | Documentos da Microsoft
description: Identifique o caminho instantâneo em seu código de servidor web com um criador de perfil de baixa capacidade.
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
ms.openlocfilehash: 839e462522be4f492010ca1c22631cb4dd6affe4
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/25/2018
ms.locfileid: "47064438"
---
# <a name="profile-live-azure-web-apps-with-application-insights"></a>Aplicações web do Azure em direto de perfil com o Application Insights

Esse recurso do Application Insights do Azure está geralmente disponível para a funcionalidade de aplicações Web do serviço de aplicações do Azure e os recursos de computação do Azure. Para obter informações sobre [na utilização de locais de criador de perfil](https://docs.microsoft.com/azure/application-insights/enable-profiler-compute#enable-profiler-on-on-premises-servers).

Este artigo aborda a quantidade de tempo é gasto em cada método da sua aplicação web em direto, quando usa [Application Insights](app-insights-overview.md). A ferramenta do Application Insights Profiler exibe detalhados de perfis de pedidos em direto que foram fornecidos pela sua aplicação. Profiler destaca os *o hot path* que utiliza mais tempo. Pedidos com vários tempos de resposta, as quais criar perfis são uma forma de amostragem. Ao utilizar uma variedade de técnicas, pode minimizar a sobrecarga associada a aplicação.

Profiler trabalha atualmente para aplicações de web ASP.NET e ASP.NET Core em execução em aplicações Web. Basic camada de serviço ou superior é obrigatório para utilizar o Profiler.

## <a id="installation"></a> Ative Profiler para as suas aplicações Web

Assim que tiver implementado uma aplicação Web, independentemente de se incluir o SDK do App Insights no código-fonte, faça o seguinte:

1. Vá para o **dos serviços de aplicações** painel no portal do Azure.
1. Navegue para **definições > monitorização** painel.

   ![Ativar o App Insights no portal dos serviços de aplicações](./media/app-insights-profiler/AppInsights-AppServices.png)

1. Ou siga as instruções no painel para criar um novo recurso ou selecione um recurso do App Insights existente para monitorizar a aplicação web. Aceite todas as opções padrão. **Diagnóstico ao nível de código** está ativada por predefinição e permite que o Profiler.

   ![Adicionar extensão de site do App Insights][Enablement UI]

1. Profiler está agora instalado com a extensão de site do App Insights e é habilitada com uma definição de aplicação de serviços de aplicação.

    ![Definição de aplicação para Profiler][profiler-app-setting]

### <a name="enable-profiler-for-azure-compute-resources"></a>Ativar Profiler para os recursos de computação do Azure

Para obter informações, consulte a [versão do Profiler para recursos de computação do Azure](https://go.microsoft.com/fwlink/?linkid=848155).

## <a name="view-profiler-data"></a>Ver dados do gerador de perfil

Certifique-se de que a aplicação está a receber tráfego. Se estiver fazendo uma experimentação, pode gerar solicitações para a sua aplicação web com [testes de desempenho do Application Insights](https://docs.microsoft.com/vsts/load-test/app-service-web-app-performance-test). Se recentemente tiver ativado o Profiler, pode executar um teste de carga curto para cerca de 15 minutos, que deve gerar rastreios do profiler. Se tenha tido Profiler ativada já há algum tempo, mantenha em mente que o Profiler aleatoriamente executa duas vezes por hora e durante um período de dois minutos toda vez que é executado. Recomendamos que primeiro a executar o teste de carga para uma hora para se certificar de que obtém os rastreios do gerador de perfil de exemplo.

Depois da aplicação recebe algum tráfego, vá para o **desempenho** painel, selecione **tomar ações** para ver os rastreios do gerador de perfil e, em seguida, selecione o **rastreios do Profiler** botão.

![Rastreios do Profiler de visualização de painel de informações de desempenho do aplicativo][performance-blade-v2-examples]

Selecione um exemplo para exibir uma divisão de nível de código de tempo gasto para executar o pedido.

![Explorador de rastreio do Application Insights][trace-explorer]

O Explorador de rastreio apresenta as seguintes informações:

* **Mostrar caminho mais utilizado**: abre o maior folha nó ou, pelo menos algo fechar. Na maioria dos casos, este nó está adjacente a um estrangulamento do desempenho.
* **Etiqueta**: O nome da função ou eventos. A árvore apresenta uma combinação de código e eventos que ocorreram (como eventos SQL e HTTP). O evento superior representa a duração do pedido geral.
* **Decorrido**: O intervalo de tempo entre o início da operação e o final da operação.
* **Quando**: tempo quando a função ou o evento foi executado em relação a outras funções.

## <a name="how-to-read-performance-data"></a>Como ler dados de desempenho

O criador de perfil de serviço do Microsoft utiliza uma combinação de métodos de amostragem e instrumentação para analisar o desempenho da sua aplicação. Quando a coleção detalhada está em curso, o criador de perfil de serviço copia o ponteiro de instrução de cada CPU do computador cada milissegundo. Cada exemplo captura a pilha de chamadas completa do thread que está sendo executado. Ele fornece informações detalhadas sobre o que esse thread estava fazendo tanto num alto nível num nível baixo de abstração. O criador de perfil de serviço também recolhe outros eventos para controlar a correlação da atividade e causalidade, incluindo eventos, eventos de tarefas TPL (biblioteca paralela) e eventos de pool de threads de alternância de contexto.

A pilha de chamadas que é apresentada na vista de linha de tempo é o resultado da amostragem e instrumentação. Uma vez que cada exemplo captura a pilha de chamadas completa do thread, inclui código de Microsoft .NET Framework e de outras estruturas que referencia.

### <a id="jitnewobj"></a>Alocação de objeto (clr! JIT\_novo ou clr! JIT\_Newarr1)

**CLR! JIT\_New** e **clr! JIT\_Newarr1** são funções auxiliares no .NET Framework que alocar a memória de um heap gerenciado. **CLR! JIT\_New** é invocado quando um objeto é alocado. **CLR! JIT\_Newarr1** é invocado quando uma matriz de objeto é alocada. Essas duas funções são, normalmente, rápidas e levar quantidades relativamente pequenas de tempo. Se vir **clr! JIT\_New** ou **clr! JIT\_Newarr1** exigir uma quantidade substancial de tempo na sua linha cronológica, ele indica que o código poderá ser alocando muitos objetos e consumir quantidades significativas de memória.

### <a id="theprestub"></a>Carregamento de código (clr! ThePreStub)

**CLR! ThePreStub** é uma função de auxiliar no .NET Framework que prepara o código seja executado pela primeira vez. Isso geralmente inclui, mas não se limita a compilação just-in-time (JIT). Para cada método c#, **clr! ThePreStub** deve ser invocado no máximo uma vez durante o tempo de vida de um processo.

Se **clr! ThePreStub** leva uma quantidade substancial de tempo para um pedido, isto indica que o pedido é o primeiro que executa esse método. O tempo para o tempo de execução do .NET Framework carregar o primeiro método é significativo. Considere o uso de um processo de aquecimento que executa essa parte do código antes dos utilizadores aceder ou considere executar o gerador de imagem nativa (ngen.exe) em seus assemblies.

### <a id="lockcontention"></a>Contenção de bloqueio (clr! JITutil\_MonContention ou clr! JITutil\_MonEnterWorker)

**CLR! JITutil\_MonContention** ou **clr! JITutil\_MonEnterWorker** indica que o thread atual está a aguardar um bloqueio ser lançado. Este texto, muitas vezes, é apresentado quando executa um c# **bloqueio** instrução, ao invocar o **ENTER** método, ou ao invocar um método com o **Methodimploptions** atributo. Contenção de bloqueio geralmente ocorre quando thread _uma_ adquire um bloqueio e o thread _B_ tenta adquirir o bloqueio mesmo antes de thread _A_ libera-lo.

### <a id="ngencold"></a>Carregamento de código ([frio])

Se o nome do método contiver **[frio]**, tal como **mscorlib.ni! [ COLD]System.Reflection.CustomAttribute.IsDefined**, o tempo de execução do .NET Framework esteja executando código pela primeira vez que não é otimizada através da <a href="https://msdn.microsoft.com/library/e7k32f4k.aspx">otimização orientada por perfil</a>. Para cada método, que deve ser exibida no máximo uma vez durante o tempo de vida do processo.

Se o carregamento de código leva uma quantidade substancial de tempo para um pedido, isso indica que o pedido é o primeiro para executar a parte não otimizada do método. Considere a utilização de um processo de aquecimento que executa essa parte do código antes dos utilizadores aceder a ele.

### <a id="httpclientsend"></a>Enviar o pedido HTTP

Como métodos **HttpClient.Send** indicar que o código está aguardando uma solicitação HTTP para ser concluído.

### <a id="sqlcommand"></a>Operação de base de dados

Como métodos **SqlCommand.Execute** indicar que o código está à espera que uma operação de base de dados concluir.

### <a id="await"></a>A aguardar (AWAIT\_tempo)

**AWAIT\_tempo** indica que o código está a aguardar conclusão da outra tarefa. Normalmente, isto acontece com o c# **AWAIT** instrução. Quando o código faz uma c# **AWAIT**, o thread se Desdobra e retorna o controle ao pool de segmentos, e não há nenhum thread que está bloqueado aguardando a **AWAIT** para concluir. No entanto, logicamente, o thread que fez a **AWAIT** for "bloqueado" e está a aguardar a conclusão da operação. O **AWAIT\_tempo** instrução indica o tempo bloqueado aguardando a conclusão da tarefa.

### <a id="block"></a>Tempo bloqueado

**BLOCKED_TIME** indica que o código está a aguardar outro recurso esteja disponível. Por exemplo, pode aguardar para um objeto de sincronização, para um thread esteja disponível ou para um pedido para concluir.

### <a id="cpu"></a>Tempo de CPU

A CPU está ocupada executando as instruções.

### <a id="disk"></a>Tempo de disco

A aplicação está a efetuar operações de disco.

### <a id="network"></a>Tempo da rede

A aplicação está a efetuar operações de rede.

### <a id="when"></a>Quando coluna

O **quando** coluna é uma visualização de como os exemplos, INCLUSIVE recolhidos para um nó variam ao longo do tempo. O intervalo total do pedido está dividido em 32 registos de tempo. Os exemplos, inclusivos para esse nó são acumulados nessas 32 partições. Cada classificação é representada como uma barra. A altura da barra de representa um valor dimensionado. Para nós que estão marcados **CPU_TIME** ou **BLOCKED_TIME**, ou onde existe uma relação óbvia a consumindo um recurso (por exemplo, uma CPU, disco ou thread), a barra representa o consumo de um dos os recursos durante o período desse registo. Para estas métricas, é possível obter um valor superior a 100 por cento de consumir vários recursos. Por exemplo, se usar, em média, duas CPUs durante um intervalo, terá 200 por cento.

## <a name="limitations"></a>Limitações

O período de retenção de dados predefinido é de cinco dias. A máxima de dados que é ingerida por dia é de 10 GB.

Não existem custos para utilizar o serviço do Profiler. Para poder utilizar o serviço do Profiler, a aplicação web tem de ser hospedado em, pelo menos, o escalão básico das aplicações Web.

## <a name="overhead-and-sampling-algorithm"></a>Sobrecarga e algoritmo de amostragem

Profiler aleatoriamente executa dois minutos por hora em cada máquina virtual que aloja a aplicação que tenha ativado para capturar rastreios do Profiler. Quando o Profiler está em execução, ele adiciona de 5% a sobrecarga de CPU de 15% para o servidor.

Os mais servidores que estão disponíveis para que aloja a aplicação, tem de Profiler o menor impacto no desempenho geral do aplicativo. Isto acontece porque o algoritmo de amostragem resulta em Profiler em execução em apenas 5 por cento dos servidores em qualquer altura. Mais servidores estão disponíveis para servir pedidos web para compensar a sobrecarga de servidor causada executando o Profiler.

## <a name="disable-profiler"></a>Desativar Profiler

Para parar ou reiniciar Profiler para a instância de uma aplicação de web individuais, em **Webjobs**, vá para o recurso de aplicações Web. Para eliminar o Profiler, aceda a **extensões**.

![Desativar Profiler para um trabalho web][disable-profiler-webjob]

Recomendamos que tenha Profiler ativada em todas as suas aplicações web para detetar problemas de desempenho mais cedo possível.

Se usar o WebDeploy para implementar as alterações ao seu aplicativo web, certifique-se de que exclua a pasta App_Data seja eliminado durante a implementação. Caso contrário, os ficheiros da extensão de Profiler são eliminados da próxima vez em que implementou a aplicação web no Azure.


## <a id="troubleshooting"></a>Resolução de Problemas

### <a name="too-many-active-profiling-sessions"></a>Demasiadas sessões ativas de criação de perfis

Atualmente, pode ativar Profiler, no máximo, quatro de web do Azure de aplicações e ranhuras de implementação que estão em execução no mesmo plano de serviço. Se a tarefa de web do Profiler está a comunicar demasiadas sessões ativas de criação de perfis, mova algumas aplicações web para um plano de serviço diferentes.

### <a name="how-do-i-determine-whether-application-insights-profiler-is-running"></a>Como posso determinar se o Application Insights Profiler está em execução?

Profiler é executado como um webjob contínua na aplicação web. É possível abrir o recurso de aplicação web no [portal do Azure](https://portal.azure.com). Na **WebJobs** painel, verifique o estado dos **ApplicationInsightsProfiler**. Se não estiver em execução, abra **registos** para obter mais informações.

### <a name="why-cant-i-find-any-stack-examples-even-though-profiler-is-running"></a>Por que motivo não consigo encontrá exemplos qualquer pilha, mesmo que o Profiler está em execução?

Aqui estão algumas coisas que pode verificar:

* Certifique-se de que o seu plano de serviço de aplicações web é escalão básico ou superior.
* Certifique-se de que a aplicação web tem a versão Beta do Application Insights SDK 2.2 ou ativado mais tarde.
* Certifique-se de que a sua aplicação web tem o **APPINSIGHTS_INSTRUMENTATIONKEY** definição configurada com a mesma chave de instrumentação que é utilizada pelo SDK do Application Insights.
* Certifique-se de que a aplicação web está em execução no .NET Framework 4.6.
* Se a sua aplicação web é uma aplicação ASP.NET Core, tem de executar, pelo menos, ASP.NET Core 2.0.

Depois do Profiler é iniciado, há um período de aquecimento curto durante o qual o Profiler recolhe ativamente vários rastreios de desempenho. Depois disso, o Profiler recolhe rastreios de desempenho durante dois minutos por hora.

> [!NOTE]
> Há um bug no agente do criador de perfil que o impede de carregamento de rastreios obtidos a partir de aplicações em execução no ASP.NET Core 2.1. Estamos a trabalhar numa correção e irá deixá-lo pronto em breve.

### <a name="i-was-using-azure-service-profiler-what-happened-to-it"></a>Eu estava usando o criador de perfil de serviço do Azure. O que aconteceu?

Quando ativar o Application Insights Profiler, o agente do profiler de serviço do Azure está desativado.

### <a id="double-counting"></a>Contagem de threads paralelos de valor de duplo

Em alguns casos, a métrica de tempo total no Visualizador de pilha é mais do que a duração do pedido.

Esta situação poderá ocorrer quando dois ou mais threads estão associados um pedido, e que estão a funcionar em paralelo. Nesse caso, o tempo total de threads é mais do que o tempo decorrido. Um thread pode estar esperando por outra seja concluída. O Visualizador tenta detetar isto e omite a espera desinteressante, mas ele erra por exibir informações em excesso em vez de omitir o que pode ser informações críticas.

Quando vir threads paralelos em seus rastreios, determine quais segmentos estão aguardando para que pode determinar o caminho crítico para o pedido. Na maioria dos casos, o thread que rapidamente entra num Estado de espera simplesmente está aguardando outros threads. Concentrar-se a outros threads e ignorar o tempo em que os threads em espera.

### <a id="issue-loading-trace-in-viewer"></a>Não existem dados de criação de perfis

Aqui estão algumas coisas que pode verificar:

* Se os dados que está a tentar a ver com mais de duas semanas, experimente a limitar o filtro de tempo e tente novamente.
* Certifique-se de que uma firewall ou proxies não bloqueou o acesso a https://gateway.azureserviceprofiler.net.
* Certifique-se de que a chave de instrumentação do Application Insights que está a utilizar na sua aplicação é o mesmo que o recurso do Application Insights que utilizou para ativar a criação de perfis. A chave é, normalmente, no ficheiro applicationinsights. config, mas também pode ser no ficheiro Web. config ou App. config.

### <a name="error-report-in-the-profiling-viewer"></a>Relatório de erro no Visualizador de criação de perfis

Submeta um pedido de suporte no portal. Certifique-se de que inclui o ID de correlação da mensagem de erro.

### <a name="deployment-error-directory-not-empty-dhomesitewwwrootappdatajobs"></a>Erro de implementação: diretório não vazio "D:\\doméstica\\site\\wwwroot\\App_Data\\das tarefas

Se estiver a Reimplementar a aplicação web a um recurso de aplicações Web com o Profiler ativado, poderá ver uma mensagem semelhante ao seguinte:

*Diretório não vazio "D:\\doméstica\\site\\wwwroot\\App_Data\\das tarefas*

Este erro ocorre se executar o Web Deploy de scripts ou do Pipeline de implementação de DevOps do Azure. A solução é adicionar os seguintes parâmetros de implementação adicionais para a tarefa de implementação da Web:

```
-skip:Directory='.*\\App_Data\\jobs\\continuous\\ApplicationInsightsProfiler.*' -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data\\jobs\\continuous$' -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data\\jobs$'  -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data$'
```

Estes parâmetros elimine a pasta que é utilizada pelo Application Insights Profiler e desbloquear o processo de voltar a implementar. Não afetam a instância de Profiler está atualmente em execução.

## <a name="manual-installation"></a>Instalação manual

Quando configura o Profiler, as atualizações são efetuadas às definições da aplicação web. Se o seu ambiente requer pode aplicar as atualizações manualmente. Por exemplo, poderá que seu aplicativo seja executado num ambiente de aplicações Web para o PowerApps.

1. Na **controlo de aplicação Web** painel, abra **definições**.
1. Definir **.Net Framework versão** ao **v4.6**.
1. Definir **Always On** ao **no**.
1. Adicionar a **APPINSIGHTS_INSTRUMENTATIONKEY** aplicação definição e defina o valor para a mesma chave de instrumentação que é utilizada pelo SDK.
1. Open **ferramentas avançadas**.
1. Selecione **ir** para abrir o site Kudu.
1. No site do Kudu, selecione **extensões de Site**.
1. Instale **Application Insights** da Galeria de aplicações Web do Azure.
1. Reinicie a aplicação web.

## <a id="profileondemand"></a> Acionar manualmente o Profiler

Profiler pode ser acionada manualmente com o clique de um botão. Suponha que está executando um teste de desempenho da web. Precisará rastreios para ajudar a compreender sobre a execução da sua aplicação web sob carga. É crucial ter controlo sobre o quando os rastreios são capturados, uma vez que sabe quando executará o teste de carga, mas o intervalo de amostragem aleatória pode perdê-lo.
Os passos seguintes mostram como funciona este cenário:

### <a name="optional-step-1-generate-traffic-to-your-web-app-by-starting-a-web-performance-test"></a>(Opcional) Passo 1: Gerar o tráfego para a sua aplicação web ao iniciar um teste de desempenho da web

Se a sua aplicação web já tiver o tráfego de entrada ou se quiser gerar tráfego manualmente, ignore esta secção e avance para o passo 2.

Navegue até ao portal do Application Insights **configurar > testes de desempenho**. Clique no botão de nova para iniciar um novo teste de desempenho.
![Criar novo teste de desempenho][create-performance-test]

Na **novo teste de desempenho** painel, configurar o URL de destino do teste. Aceite todas as configurações padrão e comece a executar o teste de carga.

![Configurar teste de carga][configure-performance-test]

Verá que o novo teste é colocada na fila em primeiro lugar, seguido de um Estado em curso.

![teste de carga é submetido e colocados em fila][load-test-queued]

![teste de carga está em execução em curso][load-test-in-progress]

### <a name="step-2-start-profiler-on-demand"></a>Passo 2: Iniciar o gerador de perfis sob demanda

Assim que estiver a executar o teste de carga, podemos começar a profiler para capturar rastreios na aplicação web, enquanto ele está recebendo a carga.
Navegue até ao painel Configurar Profiler:

![configurar a entrada do painel de criador de perfil][configure-profiler-entry]

Sobre o **painel Configurar Profiler**, há um **perfil agora** botão para acionar o criador de perfil em todas as instâncias das aplicações web ligadas. Além disso, são fornecidos a visibilidade no quando o criador de perfil estava em execução no passado.

![Profiler sob demanda][profiler-on-demand]

Verá a notificação e o estado de alteração no estado de execução o criador de perfil.

### <a name="step-3-view-traces"></a>Passo 3: Rastreios de vista

Assim que o criador de perfil termina a execução, siga as instruções na notificação para aceder à rastreios de painel e vista de desempenho.

### <a name="troubleshooting-on-demand-profiler"></a>Criador de perfil sob demanda de resolução de problemas

Às vezes, poderá ver a mensagem de erro de tempo limite do Profiler depois de uma sessão a pedido:

![Erro de tempo limite do Profiler][profiler-timeout]

Pode haver dois motivos por que vir este erro:

1. A sessão de criador de perfil sob demanda foi concluída com êxito, mas demorou mais tempo a processar os dados recolhidos do Application Insights. Se os dados não foram concluída a ser processado em 15 minutos, o portal irá apresentar uma mensagem de tempo limite. Embora após algum tempo, rastreios do Profiler serão exibido. Se isto acontecer, simplesmente ignore a mensagem de erro por agora. Estamos a trabalhar ativamente numa solução

1. A aplicação web tem uma versão mais antiga do agente do Profiler que não tenha a funcionalidade sob demanda. Se ativou anteriormente o perfil do Application Insights, é provável que tem de atualizar o agente do Profiler para começar a utilizar a capacidade a pedido.
  
Siga estes passos para verificar e instalar o Profiler mais recente:

1. Aceda às definições de aplicação de serviços de aplicação e verificar se as seguintes definições estão configuradas:
    * **APPINSIGHTS_INSTRUMENTATIONKEY**: substitua a chave de instrumentação adequada para o Application Insights.
    * **APPINSIGHTS_PORTALINFO**: ASP.NET
    * **APPINSIGHTS_PROFILERFEATURE_VERSION**: 1.0.0 se qualquer uma destas definições não forem definidas, vá para o painel de ativação do Application Insights para instalar a extensão de site mais recente.

1. Aceda ao painel do Application Insights no portal dos serviços de aplicações.

    ![Ativar o Application Insights a partir do portal dos serviços de aplicações][enable-app-insights]

1. Se vir um botão 'Update' na página seguinte, clique nele para atualizar a extensão de site do Application Insights que irá instalar o agente mais recente do Profiler.
![Atualizar a extensão de site][update-site-extension]

1. Em seguida, clique em **alterar** para garantir que o Profiler está ativado e selecione **OK** para guardar as alterações.

    ![Alterar e salvar o app insights][change-and-save-appinsights]

1. Volte ao **as definições da aplicação** separador para o serviço de aplicações para verificar novamente os seguintes itens de definições de aplicação estão definidos:
    * **APPINSIGHTS_INSTRUMENTATIONKEY**: substitua a chave de instrumentação adequada para o application insights.
    * **APPINSIGHTS_PORTALINFO**: ASP.NET
    * **APPINSIGHTS_PROFILERFEATURE_VERSION**: 1.0.0

    ![definições de aplicações para o criador de perfil][app-settings-for-profiler]

1. Opcionalmente, verifique a versão de extensão e certificar-se de que não existe nenhuma atualização disponível.

    ![verificação de atualização da extensão][check-for-extension-update]

## <a name="next-steps"></a>Passos Seguintes

* [Trabalhar com o Application Insights no Visual Studio](https://docs.microsoft.com/azure/application-insights/app-insights-visual-studio)

[appinsights-in-appservices]:./media/app-insights-profiler/AppInsights-AppServices.png
[Enablement UI]: ./media/app-insights-profiler/Enablement_UI.png
[profiler-app-setting]:./media/app-insights-profiler/profiler-app-setting.png
[performance-blade]: ./media/app-insights-profiler/performance-blade.png
[performance-blade-examples]: ./media/app-insights-profiler/performance-blade-examples.png
[performance-blade-v2-examples]:./media/app-insights-profiler/performance-blade-v2-examples.png
[trace-explorer]: ./media/app-insights-profiler/trace-explorer.png
[trace-explorer-toolbar]: ./media/app-insights-profiler/trace-explorer-toolbar.png
[trace-explorer-hint-tip]: ./media/app-insights-profiler/trace-explorer-hint-tip.png
[trace-explorer-hot-path]: ./media/app-insights-profiler/trace-explorer-hot-path.png
[enable-profiler-banner]: ./media/app-insights-profiler/enable-profiler-banner.png
[disable-profiler-webjob]: ./media/app-insights-profiler/disable-profiler-webjob.png
[linked app services]: ./media/app-insights-profiler/linked-app-services.png
[create-performance-test]: ./media/app-insights-profiler/new-performance-test.png
[configure-performance-test]: ./media/app-insights-profiler/configure-performance-test.png
[load-test-queued]: ./media/app-insights-profiler/load-test-queued.png
[load-test-in-progress]: ./media/app-insights-profiler/load-test-inprogress.png
[profiler-on-demand]: ./media/app-insights-profiler/Profiler-on-demand.png
[configure-profiler-entry]: ./media/app-insights-profiler/configure-profiler-entry.png
[enable-app-insights]: ./media/app-insights-profiler/enable-app-insights-blade-01.png
[update-site-extension]: ./media/app-insights-profiler/update-site-extension-01.png
[change-and-save-appinsights]: ./media/app-insights-profiler/change-and-save-appinsights-01.png
[app-settings-for-profiler]: ./media/app-insights-profiler/appsettings-for-profiler-01.png
[check-for-extension-update]: ./media/app-insights-profiler/check-extension-update-01.png
[profiler-timeout]: ./media/app-insights-profiler/profiler-timeout.png
