---
title: Perfil de aplicativos de produção no Azure com o Application Insights Profiler | Documentos da Microsoft
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
ms.openlocfilehash: bba15ee901e7ccecc513f526339bde2fcc6277fa
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/26/2018
ms.locfileid: "50142571"
---
# <a name="profile-production-applications-in-azure-with-application-insights"></a>Aplicações de produção do perfil no Azure com o Application Insights
## <a name="enable-profiler-for-your-application"></a>Ative Profiler para a sua aplicação

Application Insights Profiler fornece rastreios de desempenho para aplicativos em execução em produção no Azure. Ele captura os dados automaticamente à escala sem afetar negativamente os utilizadores finais. Profiler irá ajudá-lo a identificar o caminho de códigos "ativos" que demora muito tempo quando pedir a lidar com uma web específica. 

O criador de perfil funciona com aplicações de .net implementadas nos seguintes serviços do Azure. Instruções específicas para habilitar o criador de perfil para cada tipo de serviço estão nos links a seguir.

* [Serviços Aplicacionais](app-insights-profiler.md?toc=/azure/azure-monitor/toc.json)
* [Serviços Cloud](app-insights-profiler-cloudservice.md?toc=/azure/azure-monitor/toc.json)
* [Aplicações do Service Fabric](app-insights-profiler-servicefabric.md?toc=/azure/azure-monitor/toc.json)
* [Máquinas virtuais e Scalesets VM](app-insights-profiler-vm.md?toc=/azure/azure-monitor/toc.json)

Se tiver ativado o Profiler, mas não estão a ver rastreios, verifique nossos [guia de resolução de problemas.](app-insights-profiler-troubleshooting.md?toc=/azure/azure-monitor/toc.json)

A executar o Profiler no local não é oficialmente suportada, mas temos alguns [instruções pode experimentar.](https://docs.microsoft.com/azure/application-insights/enable-profiler-compute#enable-profiler-on-on-premises-servers)

## <a name="view-profiler-data"></a>Ver dados do gerador de perfil

Para o criador de perfil carregar os rastreios efetuada, seu aplicativo ativamente processar pedidos. Se estiver fazendo uma experimentação, pode gerar solicitações para a sua aplicação web com [testes de desempenho do Application Insights](https://docs.microsoft.com/vsts/load-test/app-service-web-app-performance-test). Se ativou Profiler recentemente, pode executar um teste de carga curto. Enquanto o teste de carga está em execução, prima a **perfil agora** botão no [ **página de definições do Profiler**](). Assim que o criador de perfil está em execução, ele será perfil aleatoriamente uma vez por hora e durante um período de dois minutos. Se a sua aplicação está a processar um fluxo constante de pedidos, o Profiler irá carregar rastreios a cada hora.

Depois da aplicação recebe algum tráfego e o criador de perfil tem tido tempo para carregar os trances, deve ter rastreios para ver. Este processo pode demorar 5 a 10 minutos. Para ver os rastreios, vá para o **desempenho** painel, selecione **tomar ações** para ver os rastreios do gerador de perfil e, em seguida, selecione o **rastreios do Profiler** botão.

![Rastreios do Profiler de visualização de painel de informações de desempenho do aplicativo][performance-blade]

Selecione um exemplo para exibir uma divisão de nível de código de tempo gasto para executar o pedido.

![Explorador de rastreio do Application Insights][trace-explorer]

O Explorador de rastreio apresenta as seguintes informações:

* **Mostrar caminho mais utilizado**: abre o maior folha nó ou, pelo menos algo fechar. Na maioria dos casos, este nó está perto de um estrangulamento do desempenho.
* **Etiqueta**: O nome da função ou eventos. A árvore apresenta uma combinação de código e eventos que ocorreram (como eventos SQL e HTTP). O evento superior representa a duração do pedido geral.
* **Decorrido**: O intervalo de tempo entre o início da operação e o final da operação.
* **Quando**: tempo quando a função ou o evento foi executado em relação a outras funções.

## <a name="how-to-read-performance-data"></a>Como ler dados de desempenho

O criador de perfil de serviço do Microsoft utiliza uma combinação de métodos de amostragem e instrumentação para analisar o desempenho da sua aplicação. Quando a coleção detalhada está em curso, o criador de perfil de serviço copia o ponteiro de instrução de cada CPU do computador cada milissegundo. Cada exemplo captura a pilha de chamadas completa do thread que está sendo executado. Ele fornece informações detalhadas sobre o que esse thread estava fazendo tanto num alto nível num nível baixo de abstração. O criador de perfil de serviço também recolhe outros eventos para controlar a correlação da atividade e causalidade, incluindo eventos, eventos de tarefas TPL (biblioteca paralela) e eventos de pool de threads de alternância de contexto.

A pilha de chamadas que é apresentada na vista de linha de tempo é o resultado da amostragem e instrumentação. Uma vez que cada exemplo captura a pilha de chamadas completa do thread, inclui código de Microsoft .NET Framework e de outras estruturas que referencia.

### <a id="jitnewobj"></a>Alocação de objeto (clr! JIT\_novo ou clr! JIT\_Newarr1)

**CLR! JIT\_New** e **clr! JIT\_Newarr1** são funções auxiliares no .NET Framework que alocar a memória de um heap gerenciado. **CLR! JIT\_New** é invocado quando um objeto é alocado. **CLR! JIT\_Newarr1** é invocado quando uma matriz de objeto é alocada. Essas duas funções são, normalmente, rápidas e levar quantidades relativamente pequenas de tempo. Se vir **clr! JIT\_New** ou **clr! JIT\_Newarr1** exigir uma quantidade substancial de tempo na sua linha cronológica, ele indica que o código poderá ser alocando muitos objetos e consumir quantidades significativas de memória.

### <a id="theprestub"></a>Carregamento de código (clr! ThePreStub)

**CLR! ThePreStub** é uma função de auxiliar no .NET Framework que prepara o código seja executado pela primeira vez. Isso geralmente inclui, mas não está limitado a compilação just-in-time (JIT). Para cada método c#, **clr! ThePreStub** deve ser invocado no máximo uma vez durante o tempo de vida de um processo.

Se **clr! ThePreStub** leva uma grande quantidade de tempo para um pedido, isto indica que é o primeiro pedido que executa esse método. O tempo para o tempo de execução do .NET Framework carregar o primeiro método é significativo. Considere o uso de um processo de aquecimento que executa essa parte do código antes dos utilizadores aceder ou considere executar o gerador de imagem nativa (ngen.exe) em seus assemblies.

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

**AWAIT\_tempo** indica que o código está a aguardar conclusão da outra tarefa. Normalmente, isto acontece com o c# **AWAIT** instrução. Quando o código faz uma C# **AWAIT**, o thread se Desdobra e retorna o controle ao pool de segmentos, e não há nenhum thread que está bloqueada aguardando o **AWAIT** para concluir. No entanto, logicamente, o thread que fez a **AWAIT** for "bloqueado" e está a aguardar a conclusão da operação. O **AWAIT\_tempo** instrução indica o tempo bloqueado aguardando a conclusão da tarefa.

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

## <a name="next-steps"></a>Próximos Passos
Ativar o Application Insights Profiler para a sua aplicação do Azure
* [Serviços Aplicacionais](app-insights-profiler.md?toc=/azure/azure-monitor/toc.json)
* [Serviços Cloud](app-insights-profiler-cloudservice.md?toc=/azure/azure-monitor/toc.json)
* [Aplicações do Service Fabric](app-insights-profiler-servicefabric.md?toc=/azure/azure-monitor/toc.json)
* [Máquinas virtuais e Scalesets VM](app-insights-profiler-vm.md?toc=/azure/azure-monitor/toc.json)


[performance-blade]: ./media/app-insights-profiler/performance-blade-v2-examples.png
[trace-explorer]: ./media/app-insights-profiler/trace-explorer.png
