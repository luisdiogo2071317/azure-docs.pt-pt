---
title: Monitorização de utilização e desempenho de aplicações de ambiente de trabalho do Windows
description: Analise a utilização e o desempenho da sua aplicação de ambiente de trabalho do Windows com o Application Insights.
services: application-insights
documentationcenter: windows
author: mrbullwinkle
manager: carmonm
ms.assetid: 19040746-3315-47e7-8c60-4b3000d2ddc4
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 05/15/2018
ms.author: mbullwin
ms.openlocfilehash: 10044f17baee8fb4d7afe9223abdbb518952e71f
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/01/2018
ms.locfileid: "52741863"
---
# <a name="monitoring-usage-and-performance-in-classic-windows-desktop-apps"></a>Monitorização de utilização e desempenho de aplicações de Ambiente de Trabalho do Windows Clássico

As aplicações alojadas no local, no Azure e noutras clouds podem tirar todo o partido do Application Insights. A única limitação é a necessidade de [permitir a comunicação](app-insights-ip-addresses.md) ao serviço Application Insights. Para monitorizar aplicações da Plataforma Universal do Windows (UWP), recomendamos o [Visual Studio App Center](app-insights-mobile-center-quickstart.md).

## <a name="to-send-telemetry-to-application-insights-from-a-classic-windows-application"></a>Para enviar a telemetria para o Application Insights a partir de uma aplicação do Windows Clássico
1. No [portal do Azure](https://portal.azure.com), [crie um recurso do Application Insights](app-insights-create-new-resource.md). Para o tipo de aplicação, escolha a aplicação ASP.NET.
2. Faça uma cópia da Chave de Instrumentação. Encontre a chave na lista pendente Essentials do novo recurso que acabou de criar. 
3. No Visual Studio, edite os pacotes NuGet do seu projeto da aplicação e adicione Microsoft.ApplicationInsights.WindowsServer. (Ou escolha Microsoft.ApplicationInsights se apenas pretender a API bare, sem os módulos de coleção de telemetria standard.)
4. Defina a chave de instrumentação no seu código:
   
    `TelemetryConfiguration.Active.InstrumentationKey = "` *a sua chave* `";`
   
    ou no Applicationinsights (se tiver instalado um dos pacotes de telemetria standard):
   
    `<InstrumentationKey>`*a sua chave*`</InstrumentationKey>` 
   
    Se utilizar o ApplicationInsights.config, certifique-se de que as propriedades no Explorador de Soluções estão definidas para **Ação de Compilação = Conteúdo, Copiar para o Diretório de Saída = Copiar**.
5. [Utilize a API](app-insights-api-custom-events-metrics.md) para enviar telemetria.
6. Execute a aplicação e veja a telemetria no recurso que criou no Portal do Azure.

## <a name="telemetry"></a>Código de exemplo
```csharp

    public partial class Form1 : Form
    {
        private TelemetryClient tc = new TelemetryClient();
        ...
        private void Form1_Load(object sender, EventArgs e)
        {
            // Alternative to setting ikey in config file:
            tc.InstrumentationKey = "key copied from portal";

            // Set session data:
            tc.Context.User.Id = Environment.UserName;
            tc.Context.Session.Id = Guid.NewGuid().ToString();
            tc.Context.Device.OperatingSystem = Environment.OSVersion.ToString();

            // Log a page view:
            tc.TrackPageView("Form1");
            ...
        }

        protected override void OnClosing(CancelEventArgs e)
        {
            stop = true;
            if (tc != null)
            {
                tc.Flush(); // only for desktop apps

                // Allow time for flushing:
                System.Threading.Thread.Sleep(1000);
            }
            base.OnClosing(e);
        }

```

## <a name="next-steps"></a>Passos Seguintes
* [Criar um dashboard](app-insights-dashboards.md)
* [Pesquisa de Diagnóstico](app-insights-diagnostic-search.md)
* [Explorar métricas](app-insights-metrics-explorer.md)
* [Escrever consultas da Análise](app-insights-analytics.md)

