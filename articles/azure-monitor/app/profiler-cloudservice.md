---
title: Perfil live serviços Cloud do Azure com o Application Insights | Documentos da Microsoft
description: Ative o Application Insights Profiler para serviços Cloud do Azure.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.reviewer: cawa
ms.date: 08/06/2018
ms.author: mbullwin
ms.openlocfilehash: 01147f19a6a10361609c01bc6b3f1ac07d1ff86b
ms.sourcegitcommit: a408b0e5551893e485fa78cd7aa91956197b5018
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/17/2019
ms.locfileid: "54358037"
---
# <a name="profile-live-azure-cloud-services-with-application-insights"></a>Perfil em direto serviços Cloud do Azure com o Application Insights

Também pode implementar o Application Insights Profiler estes serviços:
* [Serviço de Aplicações do Azure](profiler.md?toc=/azure/azure-monitor/toc.json)
* [Aplicações do Azure Service Fabric](profiler-servicefabric.md?toc=/azure/azure-monitor/toc.json)
* [Máquinas Virtuais do Azure](profiler-vm.md?toc=/azure/azure-monitor/toc.json)

Application Insights Profiler é instalado com a extensão de diagnóstico do Azure. Terá de configurar os diagnósticos do Azure para instalar o Profiler e enviar perfis para o recurso do Application Insights.

## <a name="enable-profiler-for-azure-cloud-services"></a>Ative Profiler para serviços Cloud do Azure
1. Certifique-se de que está a utilizar [.NET Framework 4.6.1](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed) ou posterior. É suficiente confirmar que o *ServiceConfiguration.\*.cscfg* arquivos têm uma `osFamily` valor "5" ou posterior.

1. Adicione [serviços Cloud do Application Insights SDK para o Azure](../../azure-monitor/app/cloudservices.md?toc=/azure/azure-monitor/toc.json).

1. Acompanhar pedidos com o Application Insights:

    * Para funções da web ASP.NET, Application Insights pode controlar os pedidos automaticamente.

    * Para funções de trabalho, [adicionar código para monitorizar os pedidos](profiler-trackrequests.md?toc=/azure/azure-monitor/toc.json).

1. Configure a extensão de diagnóstico do Azure para ativar o Profiler, fazendo o seguinte:

    a. Localize a [diagnóstico do Azure](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics) *diagnostics.wadcfgx* de ficheiros para a sua função de aplicação, como mostrado aqui:  

      ![Localização do ficheiro de configuração de diagnósticos](./media/profiler-cloudservice/cloudservice-solutionexplorer.png)  

      Se não encontrar o ficheiro, consulte [configurar diagnósticos para serviços Cloud do Azure e máquinas virtuais](https://docs.microsoft.com/azure/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines#enable-diagnostics-in-cloud-service-projects-before-deploying-them).

    b. Adicione as seguintes `SinksConfig` secção como um elemento subordinado do `WadCfg`:  

      ```xml
      <WadCfg>
        <DiagnosticMonitorConfiguration>...</DiagnosticMonitorConfiguration>
        <SinksConfig>
          <Sink name="MyApplicationInsightsProfiler">
            <!-- Replace with your own Application Insights instrumentation key. -->
            <ApplicationInsightsProfiler>00000000-0000-0000-0000-000000000000</ApplicationInsightsProfiler>
          </Sink>
        </SinksConfig>
      </WadCfg>
      ```

    > [!NOTE]
    > Se o *diagnostics.wadcfgx* ficheiro também contém outro sink do tipo Application Insights, todos os três das seguintes chaves de instrumentação tem de corresponder ao:  
    > * A chave que é utilizada pela sua aplicação. 
    > * A chave utilizada pelo sink do Application Insights. 
    > * A chave utilizada pelo coletor ApplicationInsightsProfiler. 
    >
    > Pode encontrar o valor de chave de instrumentação real é utilizado pelos `ApplicationInsights` serem digeridos a *ServiceConfiguration.\*.cscfg* ficheiros. 
    > Após o lançamento do SDK do Azure do Visual Studio 15,5, apenas as chaves de instrumentação que são utilizadas pela aplicação e o sink de ApplicationInsightsProfiler têm de corresponder entre si.

1. Implementar o seu serviço com a nova configuração de diagnósticos e Application Insights Profiler está configurado para ser executado no seu serviço.
 
## <a name="next-steps"></a>Passos Seguintes

* Gerar tráfego para a aplicação (por exemplo, iniciar uma [teste de disponibilidade](https://docs.microsoft.com/azure/application-insights/monitor-web-app-availability)). Em seguida, aguarde 10 a 15 minutos para que os rastreios começar a ser enviados para a instância do Application Insights.
* Ver [rastreios do Profiler](https://docs.microsoft.com/azure/application-insights/profiler-overview?toc=/azure/azure-monitor/toc.json) no portal do Azure.
* Para resolver problemas do Profiler, consulte [Profiler de resolução de problemas](profiler-troubleshooting.md?toc=/azure/azure-monitor/toc.json).
