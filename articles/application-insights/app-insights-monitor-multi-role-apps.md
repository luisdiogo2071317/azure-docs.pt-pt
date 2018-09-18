---
title: O Azure Application Insights suporte para vários componentes, microsserviços e contentores | Documentos da Microsoft
description: Monitorização de aplicações que consistem em vários componentes ou funções para o desempenho e utilização.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 09/17/2018
ms.author: mbullwin
ms.openlocfilehash: bf247748415822d5ba1a0e652fdeff384d8e8db1
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/18/2018
ms.locfileid: "45982056"
---
# <a name="monitor-multi-component-applications-with-application-insights-preview"></a>Monitorizar aplicações com vários componentes com o Application Insights (pré-visualização)

Pode monitorizar aplicações que consistem em vários componentes de servidor, funções ou serviços com [do Azure Application Insights](app-insights-overview.md). O estado de funcionamento dos componentes e as relações entre eles são exibidos num mapa de aplicativo único. Pode rastrear operações individuais por meio de vários componentes com automática correlação de HTTP. Diagnóstico de contentores pode ser integrado e correlacionado com telemetria da aplicação. Utilize um único recurso do Application Insights para todos os componentes da sua aplicação. 

![Mapa da aplicação com vários componentes](./media/app-insights-monitor-multi-role-apps/application-map-001.png)

Utilizamos "componente" aqui significa: qualquer parte funcional de um aplicativo grande. Por exemplo, um aplicativo de negócios típico pode consistir em código de cliente em execução nos navegadores da web, conversar com um ou mais serviços de aplicação web, que por sua vez voltar a utilizar serviços de fim. Componentes de servidor podem ser alojado no local na cloud, ou podem ser funções web e de trabalho do Azure ou podem ser executadas em contentores, como o Docker ou o Service Fabric. 

### <a name="sharing-a-single-application-insights-resource"></a>Partilhar um único recurso do Application Insights 

A técnica de chave aqui é enviar telemetria a partir de todos os componentes na sua aplicação para o mesmo recurso do Application Insights, mas utilizar o `cloud_RoleName` propriedade para distinguir os componentes quando for necessário. O SDK do Application Insights adiciona o `cloud_RoleName` emitir de propriedade para os componentes de telemetria. Por exemplo, o SDK irá adicionar um nome do web site ou o nome da função de serviço para o `cloud_RoleName` propriedade. É possível substituir este valor com um telemetryinitializer. O mapa da aplicação utiliza o `cloud_RoleName` propriedade para identificar os componentes no mapa.

Para obter mais informações sobre como substituir a `cloud_RoleName` propriedade veja [adicionar propriedades: ITelemetryInitializer](app-insights-api-filtering-sampling.md#add-properties-itelemetryinitializer).  

Em alguns casos, isso pode não ser apropriado e, talvez prefira usar recursos separados para diferentes grupos de componentes. Por exemplo, poderá ter de utilizar recursos diferentes para a gestão ou efeitos de faturação.

Com essa limitação, vamos supor que no restante deste documento que pretende enviar dados de vários componentes para um recurso do Application Insights.

## <a name="configure-multi-component-applications"></a>Configurar aplicações com vários componentes

Para obter um mapa da aplicação com vários componentes, o que precisa para alcançar estes objetivos:

* **Instalar a versão de pré-lançamento mais recente** pacote do Application Insights em cada componente da aplicação. 
* **Partilhar um único recurso do Application Insights** para todos os componentes da sua aplicação.
* **Ativar o mapa da aplicação composto** no painel pré-visualizações.

Configure cada componente do seu aplicativo usando o método apropriado para o respetivo tipo. ([ASP.NET](app-insights-asp-net.md), [Java](app-insights-java-get-started.md), [node. js](app-insights-nodejs.md), [JavaScript](app-insights-javascript.md).)

### <a name="1-install-the-latest-pre-release-package"></a>1. Instalar o pacote de versão de pré-lançamento mais recente

Atualizar ou instalar os pacotes do Application Insights no projeto para cada componente de servidor. Se estiver a utilizar o Visual Studio:

1. Um projeto com o botão direito e selecione **gerir pacotes NuGet**. 
2. Selecione **incluir pré-lançamento**.
3. Se aparecerem a atualizações de pacotes do Application Insights, selecioná-los. 

    Caso contrário, procurar e instalar o pacote apropriado:
    
    * Microsoft.ApplicationInsights.WindowsServer
    * Microsoft.ApplicationInsights.ServiceFabric - dos componentes em execução como convidado executáveis e de contentores do Docker a executar uma aplicação de Service Fabric em
    * Microsoft.ApplicationInsights.ServiceFabric.Native - para reliable services em aplicativos do ServiceFabric
    * Microsoft.ApplicationInsights.Kubernetes dos componentes em execução no Docker no Kubernetes

### <a name="2-share-a-single-application-insights-resource"></a>2. Partilhar um único recurso do Application Insights

* No Visual Studio, um projeto com o botão direito e selecione **configurar o Application Insights**, ou **Application Insights > configurar**. Para o projeto primeiro, utilize o Assistente para criar um recurso do Application Insights. Para projetos subseqüentes, selecione o mesmo recurso.
* Se não houver nenhum menu do Application Insights, configure manualmente:

   1. Na [portal do Azure](https://portal,azure.com), abra o recurso do Application Insights que já criou para outro componente.
   2. No painel de descrição geral, abra a lista pendente Essentials separador e copie o **chave de instrumentação.**
   3. No seu projeto, abra o applicationinsights. config e inserir: `<InstrumentationKey>your copied key</InstrumentationKey>`

![Copie a chave de instrumentação para o arquivo. config](./media/app-insights-monitor-multi-role-apps/copy-instrumentation-key.png)


### <a name="3-enable-composite-application-map"></a>3. Ativar o mapa de aplicativo composto

No portal do Azure, abra o recurso para a sua aplicação. Sob o cabeçalho de frações de configurar, clique em pré-visualizações para abrir o painel de visualizações. No painel pré-visualizações, ative *mapa de aplicativo composto*.

### <a name="4-enable-docker-metrics-optional"></a>4. Ativar as métricas de Docker (opcional) 

Se um componente for executado num Docker alojado numa VM do Windows Azure, pode recolher métricas adicionais do contêiner. Insira-o no seu [diagnóstico do Azure](../monitoring-and-diagnostics/azure-diagnostics.md) ficheiro de configuração:

```
"DiagnosticMonitorConfiguration": {
        ...
        "sinks": "applicationInsights",
        "DockerSources": {
                "Stats": {
                    "enabled": true,
                    "sampleRate": "PT1M"
                }
            },
        ...
    }
    ...   
    "SinksConfig": {
        "Sink": [{
            "name": "applicationInsights",
            "ApplicationInsights": "<your instrumentation key here>"
        }]
    }
    ...
}

```

## <a name="use-cloudrolename-to-separate-components"></a>Utilizar cloud_RoleName para separar os componentes

O `cloud_RoleName` propriedade está ligada a toda a telemetria. Identifica o componente - a função ou serviço - originados a telemetria. (Não é o mesmo que cloud_RoleInstance, que separa idênticas funções em execução em paralelo em vários processos de servidor ou máquinas.)

No portal, pode filtrar ou segmentar a sua telemetria com esta propriedade. Neste exemplo, o painel de falhas é filtrado para mostrar apenas as informações do serviço web de front-end, filtram de falhas de back-end da API de CRM:

![Gráfico de métricas segmentado por nome da função na nuvem](./media/app-insights-monitor-multi-role-apps/cloud-role-name.png)

## <a name="trace-operations-between-components"></a>Operações de rastreamento entre os componentes

Pode rastrear de um componente para outro, as chamadas efetuadas durante o processamento de uma operação individual.


![Mostrar telemetria para a operação](./media/app-insights-monitor-multi-role-apps/show-telemetry-for-operation.png)

Clicar para obter uma lista correlacionada de telemetria para esta operação entre o servidor web front-end e a API de back-end:

![Procure entre componentes](./media/app-insights-monitor-multi-role-apps/search-across-components.png)


## <a name="next-steps"></a>Passos Seguintes

* [Telemetria separado de desenvolvimento, teste e produção](app-insights-separate-resources.md)
