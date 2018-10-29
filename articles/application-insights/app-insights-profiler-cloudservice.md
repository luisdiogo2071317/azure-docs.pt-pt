---
title: Perfil live serviços cloud do Azure com o Application Insights | Documentos da Microsoft
description: Ative o Application Insights Profiler para serviços em nuvem.
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
ms.openlocfilehash: 445e607b6b0a21f840ab633b3a5a3779f49fdd98
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/26/2018
ms.locfileid: "50142504"
---
# <a name="profile-live-azure-cloud-services-with-application-insights"></a>Perfil live serviços cloud do Azure com o Application Insights

Também pode implementar o criador de perfil do Application Insights estes serviços:
* [Aplicações Web do Azure](app-insights-profiler.md?toc=/azure/azure-monitor/toc.json)
* [Aplicações do Service Fabric](app-insights-profiler-servicefabric.md?toc=/azure/azure-monitor/toc.json)
* [Máquinas Virtuais](app-insights-profiler-vm.md?toc=/azure/azure-monitor/toc.json)

Application Insights Profiler é instalado com a extensão do Windows Azure Diagnostics (WAD). Terá de configurar WAD para instalar o gerador de perfis e enviar perfis para o recurso do Application Insights.

## <a name="enable-profiler-for-your-azure-cloud-service"></a>Ative profiler para o seu serviço Cloud do Azure
1. Verifique que usando [.NET Framework 4.6.1](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed) ou posterior.  É suficiente confirmar que o *ServiceConfiguration.\*. cscfg* arquivos têm uma `osFamily` valor "5" ou posterior.
1. Adicione [Application Insights SDK para o serviço em nuvem](app-insights-cloudservices.md?toc=/azure/azure-monitor/toc.json).
1. Acompanhar pedidos com o Application Insights:

    Para funções da web ASP.Net, Application Insights pode controlar os pedidos automaticamente.

    Para funções de trabalho, [adicionar código para monitorizar os pedidos.](app-insights-profiler-trackrequests.md?toc=/azure/azure-monitor/toc.json)

    

1. Configure a extensão do Windows Azure Diagnostics (WAD) para ativar o gerador de perfil.



    1. Localize a [diagnóstico do Azure](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics) *diagnostics.wadcfgx* de ficheiros para a sua função de aplicação, como mostrado aqui:  

       ![Localização do ficheiro de configuração de diagnósticos](./media/enable-profiler-compute/cloudservice-solutionexplorer.png)  

        Se não encontrar o ficheiro, para saber como ativar a extensão de diagnóstico no seu projeto de serviços Cloud do Azure, veja [configurar diagnósticos para serviços Cloud do Azure e máquinas virtuais](https://docs.microsoft.com/azure/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines#enable-diagnostics-in-cloud-service-projects-before-deploying-them).

    1. Adicione as seguintes `SinksConfig` secção como um elemento subordinado do `WadCfg`:  

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

    >   **Nota:** se o *diagnostics.wadcfgx* ficheiro também contém outro sink do tipo `ApplicationInsights`, três das seguintes chaves de instrumentação tem de corresponder ao:  
    >  * A chave que é utilizada pela sua aplicação.  
    >  * A chave utilizada pelo `ApplicationInsights` sink.  
    >  * A chave utilizada pelo `ApplicationInsightsProfiler` sink.  
    >
    > Pode encontrar o valor de chave de instrumentação real é utilizado pelos `ApplicationInsights` serem digeridos a *ServiceConfiguration.\*. cscfg* ficheiros.  
    > Após o lançamento do SDK do Azure do Visual Studio 15,5, apenas as chaves de instrumentação que são utilizados pela aplicação e o `ApplicationInsightsProfiler` necessidade de sink para corresponderem entre si.
1. Implementar o seu serviço com a nova configuração de diagnóstico e Application Insights Profiler irá ser configurado para executar no seu serviço.
 
## <a name="next-steps"></a>Passos Seguintes

- Gerar tráfego para a aplicação (por exemplo, iniciar uma [teste de disponibilidade](https://docs.microsoft.com/azure/application-insights/app-insights-monitor-web-app-availability)). Em seguida, aguarde 10 a 15 minutos para que os rastreios começar a ser enviados para a instância do Application Insights.
- Ver [rastreios do Profiler](https://docs.microsoft.com/azure/application-insights/app-insights-profiler-overview?toc=/azure/azure-monitor/toc.json) no portal do Azure.
- Obtenha ajuda com a resolução de problemas do criador de perfil na [Profiler de resolução de problemas](app-insights-profiler-troubleshooting.md?toc=/azure/azure-monitor/toc.json).