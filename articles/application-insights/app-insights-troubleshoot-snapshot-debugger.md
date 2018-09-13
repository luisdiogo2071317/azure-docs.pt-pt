---
title: Guia de resolução de problemas de depurador de instantâneos do Application Insights do Azure | Documentos da Microsoft
description: Perguntas mais frequentes sobre o Snapshot Debugger do Azure Application Insights.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: ''
ms.service: application-insights
ms.workload: ''
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 11/15/2017
ms.author: mbullwin
ms.openlocfilehash: 285f42a6b52819077b92abce78c1f51756780604
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/12/2018
ms.locfileid: "35648869"
---
# <a name="snapshot-debugger-troubleshooting-guide"></a>Snapshot Debugger: Guia de resolução de problemas

Snapshot Debugger do Application Insights permite-lhe recolher automaticamente um instantâneo de depuração de aplicativos web em direto. O instantâneo mostra o estado do código-fonte e variáveis no momento que foi emitida uma exceção. Este artigo explica como o depurador funciona e fornece soluções para problemas comuns.

## <a name="how-does-application-insights-snapshot-debugger-work"></a>Como funciona o Snapshot Debugger do Application Insights

Snapshot Debugger do Application Insights é parte do pipeline de telemetria do Application Insights (uma instância de ITelemetryProcessor). O Recoletor de instantâneos monitoriza as exceções geradas em seu código (AppDomain.FirstChanceException) e a telemetria de exceção comunicados ao Application Insights através de `TelemetryClient.TrackException`. Depois de ter adicionado com êxito o pacote de Recoletor de instantâneos para seu projeto e detetou um evento no pipeline de telemetria, um evento personalizado com o nome 'AppInsightsSnapshotCollectorLogs' e 'SnapshotCollectorEnabled"nos dados personalizados serão enviado. Ao mesmo tempo, iniciará um processo chamado 'SnapshotUploader.exe' (versão 1.2.0 ou superior) ou MinidumpUploader.exe (antes da versão 1.2.0) carregar o recolhidos de ficheiros de dados para o Application Insights instantâneos.  Quando o processo de carregador é iniciado, envia um evento personalizado com o nome 'UploaderStart'. Depois disso, o recoletor de instantâneos irá introduzir o seu comportamento normal de monitorização.

Embora o recoletor de instantâneos está a monitorizar a telemetria de exceção do Application Insights, ele usa os parâmetros (por exemplo, ThresholdForSnapshotting, MaximumSnapshotsRequired, MaximumCollectionPlanSize, ProblemCounterResetInterval) definidos no configuração para determinar quando deve recolher um instantâneo. Quando todas as regras são cumpridas, o recoletor irá pedir um instantâneo para a próxima exceção emitida no mesmo local. Simultaneamente, será enviado um evento personalizado do Application Insights com o nome 'AppInsightsSnapshotCollectorLogs' e 'RequestSnapshots'. Uma vez que o compilador irá otimizar o código da "Versão", variáveis locais poderão não estar visíveis no instantâneo recolhido. O recoletor de instantâneos irá tentar deoptimize o método que gerou a exceção, quando faz a solicitação instantâneos. Durante este período, será enviado um evento personalizado Application Insights com o nome 'AppInsightsSnapshotCollectorLogs' e "ProductionBreakpointsDeOptimizeMethod" nos dados personalizados.  Quando o instantâneo da exceção seguinte é coletado, as variáveis locais estará disponíveis. Depois do instantâneo é recolhido, ele será reoptimize o código. 

> [!NOTE]
> Deoptimization requer a extensão de site do Application Insights ser instalado.

Quando um instantâneo é solicitado para uma exceção específica, o recoletor de instantâneos irá iniciar a monitorização de pipeline (AppDomain.FirstChanceException) de manipulação de exceção do seu aplicativo. Quando a exceção ocorre novamente, o recoletor será iniciado um instantâneo (evento personalizado do Application Insights com o nome 'AppInsightsSnapshotCollectorLogs' e 'SnapshotStart"nos dados personalizados). Em seguida, é feita uma cópia de sombra do processo em execução (a tabela de página será duplicada). Isso normalmente, irá demorar 10 a 20 milissegundos. Depois disso, serão enviado um evento personalizado do Application Insights com o nome 'AppInsightsSnapshotCollectorLogs' e 'SnapshotStop"nos dados personalizados. Quando o processo de bifurcado é criado, o total de memória paginada aumenta na mesma proporção como a memória paginada da sua aplicação em execução (o conjunto de trabalho será muito menor). Enquanto seu processo de aplicação está a funcionar normalmente, as cópias de sombra copiadas memória do processo será colocada em disco e carregada para o Application Insights. Depois do instantâneo é carregado, será enviado um evento personalizado do Application Insights com o nome 'UploadSnapshotFinish'.

## <a name="is-the-snapshot-collector-working-properly"></a>O recoletor de instantâneos está a funcionar corretamente?

### <a name="how-to-find-snapshot-collector-logs"></a>Como localizar os registos de Recoletor de instantâneos
Registos de recoletor de instantâneos são enviados para a sua conta do Application Insights, se o [pacote NuGet do Recoletor de instantâneos](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) é a versão 1.1.0 ou posterior. Certifique-se de que o *ProvideAnonymousTelemetry* não está definido como false (o valor é true por padrão).

* Navegue para o recurso do Application Insights no portal do Azure.
* Clique em *pesquisa* na seção de visão geral.
* Introduza a seguinte cadeia de caracteres na caixa de pesquisa:
    ```
    AppInsightsSnapshotCollectorLogs OR AppInsightsSnapshotUploaderLogs OR UploadSnapshotFinish OR UploaderStart OR UploaderStop
    ```
* Nota: alterar a *intervalo de tempo* se for necessário.

![Captura de ecrã do Recoletor de instantâneos pesquisa de registos](./media/app-insights-troubleshoot-snapshot-debugger/001.png)


### <a name="examine-snapshot-collector-logs"></a>Examine os registos de recoletor de instantâneos
Ao pesquisar registos de Recoletor de instantâneos, deve haver 'UploadSnapshotFinish' eventos no intervalo de tempo de destino. Se ainda não vir o botão "Abrir instantâneo de depuração" para abrir o instantâneo, envie um email para snapshothelp@microsoft.com com sua chave de instrumentação do Application Insights.

![Captura de ecrã de examinar registos de recoletor de instantâneos](./media/app-insights-troubleshoot-snapshot-debugger/005.png)

## <a name="i-cannot-find-a-snapshot-to-open"></a>Não consigo encontrar um instantâneo para abrir
Se os seguintes passos não ajudá-lo a resolver o problema, envie um email para snapshothelp@microsoft.com com chave de instrumentação do Application Insights.

### <a name="step-1-make-sure-your-application-is-sending-telemetry-data-and-exception-data-to-application-insights"></a>Passo 1: Certificar-se de que a aplicação está a enviar dados de telemetria e dados de exceção para o Application Insights
Navegue até ao recurso do Application Insights, verifique que não há dados enviados a partir da sua aplicação.

### <a name="step-2-make-sure-snapshot-collector-is-added-correctly-to-your-applications-application-insights-telemetry-pipeline"></a>Passo 2: Certificar-se de que o recoletor de instantâneos é adicionado corretamente ao pipeline de telemetria do Application Insights da sua aplicação
Se pode encontrar registos no passo "Como encontrar registos de Recoletor de instantâneos", em seguida, o recoletor de instantâneos corretamente é adicionado ao seu projeto e pode ignorar este passo.

Se não existirem não existem registos de recoletor de instantâneos, verifique o seguinte:
* Procurar para aplicativos ASP.NET clássicos, esta linha *<Add Type="Microsoft.ApplicationInsights.SnapshotCollector.SnapshotCollectorTelemetryProcessor, Microsoft.ApplicationInsights.SnapshotCollector">* no *applicationinsights. config* ficheiro.

* Para aplicativos do ASP.NET Core, certifique-se de que o *ITelemetryProcessorFactory* com *SnapshotCollectorTelemetryProcessor* é adicionado ao *IServiceCollection* serviços .

* Também pode verificar que está a utilizar a chave de instrumentação correto no seu aplicativo publicado.

* O recoletor de instantâneos não oferece suporte a várias chaves de instrumentação dentro de um aplicativo, ele enviará instantâneos para a chave de instrumentação da primeira exceção que observa.

* Se definir o *InstrmentationKey* manualmente no seu código, atualize o *InstrumentationKey* elemento a partir do *applicationinsights. config*.

### <a name="step-3-make-sure-the-minidump-uploader-is-started"></a>Passo 3: Certifique-se de que o carregador de minidespejo é iniciado
Nos logs de recoletor de instantâneos, procure *UploaderStart* (digite UploaderStart na caixa de texto de pesquisa). Deve haver um evento quando o recoletor de instantâneos monitorizados a primeira exceção. Se este evento não existir, verifique outros registos para obter detalhes. Uma forma possível para resolver este problema está a reiniciar a aplicação.

### <a name="step-4-make-sure-snapshot-collector-expressed-its-intent-to-collect-snapshots"></a>Passo 4: Certifique-se de que o Recoletor de instantâneos expressa a sua intenção para recolher os instantâneos
Nos logs de recoletor de instantâneos, procure *RequestSnapshots* (tipo ```RequestSnapshots``` na caixa de texto de pesquisa).  Se não existir qualquer, verifique a configuração. Por exemplo, *ThresholdForSnapshotting*, que indica o número de uma exceção específica que pode ocorrer antes de iniciar a recolha de instantâneos.

### <a name="step-5-make-sure-that-snapshot-is-not-disabled-due-to-memory-protection"></a>Passo 5: Certifique-se de que o instantâneo não está desativado devido a proteção de memória
Para proteger o desempenho do aplicativo, um instantâneo só ser capturado quando existe uma boa memória intermédia. Nos logs de recoletor de instantâneos, procure 'CannotSnapshotDueToMemoryUsage'. Os dados do evento personalizado, terá uma razão detalhada. Se a sua aplicação está em execução numa aplicação Web do Azure, a restrição pode ser rígida. Uma vez que a aplicação Web do Azure irá reiniciar a aplicação quando forem cumpridas determinadas regras de memória. Pode tentar aumentar verticalmente o seu plano do serviço para máquinas com mais memória para resolver este problema.

### <a name="step-6-make-sure-snapshots-were-captured"></a>Passo 6: Certifique-se de instantâneos foram capturados
Nos logs de recoletor de instantâneos, procure ```RequestSnapshots```.  Se não houver, verifique a configuração. Por exemplo, *ThresholdForSnapshotting*, que indica o número de uma exceção específica antes de coletar um instantâneo.

### <a name="step-7-make-sure-snapshots-are-uploaded-correctly"></a>Passo 7: Certifique-se de que os instantâneos são carregados corretamente
Nos logs de recoletor de instantâneos, procure ```UploadSnapshotFinish```.  Se não estiver presente, envie um email para snapshothelp@microsoft.com com chave de instrumentação do Application Insights. Se o evento existir, abra um dos registos e copie o valor de "SnapshotId" nos dados personalizado. Em seguida, procure o valor. Esta ação encontrará a exceção correspondente para o instantâneo. Em seguida, clique a exceção e abrir instantâneo de depuração. (Se não houver nenhuma exceção correspondente, a telemetria de exceção pode ser objeto de amostragem devido ao elevado volume. Tente outra snapshotId).

![Captura de ecrã do SnapshotId de Find](./media/app-insights-troubleshoot-snapshot-debugger/002.png)

![Captura de ecrã da exceção aberta](./media/app-insights-troubleshoot-snapshot-debugger/004b.png)

![Captura de ecrã de abrir instantâneo de depuração](./media/app-insights-troubleshoot-snapshot-debugger/003.png)

## <a name="snapshot-view-local-variables-are-not-complete"></a>Variáveis de Local de vista do instantâneo não estão concluídas

Algumas das variáveis locais estão em falta. Se a aplicação estiver a executar o código da versão, o compilador otimizará algumas variáveis de distância. Por exemplo:

```csharp
    const int a = 1; // a will be discarded by compiler and the value 1 will be inline.
    Random rand = new Random();
    int b = rand.Next() % 300; // b will be discarded and the value will be directly put to the 'FindNthPrimeNumber' call stack.
    long primeNumber = FindNthPrimeNumber(b);
```

Se o seu caso é diferente, poderá indicar um bug. Envie um e-mail para snapshothelp@microsoft.com com chave de instrumentação do Application Insights, juntamente com o trecho de código.

## <a name="snapshot-view-cannot-obtain-value-of-the-local-variable-or-argument"></a>Vista de instantâneo: Não é possível obter o valor da variável local ou do argumento
Certifique-se de que o [extensão de site do Application Insights](https://www.siteextensions.net/packages/Microsoft.ApplicationInsights.AzureWebSites/) está instalado. Se o problema persistir, envie um email para snapshothelp@microsoft.com com chave de instrumentação do Application Insights.
