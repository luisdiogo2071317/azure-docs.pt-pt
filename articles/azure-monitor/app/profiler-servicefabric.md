---
title: Perfil de aplicativos em direto do Azure Service Fabric com o Application Insights | Documentos da Microsoft
description: Ativar o gerador de perfis para uma aplicação do Service Fabric
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
ms.openlocfilehash: 2513511326ff5574e8dcf3eedfde977cf9877efd
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/08/2019
ms.locfileid: "54083383"
---
# <a name="profile-live-azure-service-fabric-applications-with-application-insights"></a>Perfil de aplicativos em direto do Azure Service Fabric com o Application Insights

Também pode implementar o criador de perfil do Application Insights estes serviços:
* [Serviço de Aplicações do Azure](../../azure-monitor/app/profiler.md?toc=/azure/azure-monitor/toc.json)
* [Serviços Cloud](profiler-cloudservice.md ?toc=/azure/azure-monitor/toc.json)
* [Máquinas Virtuais](profiler-vm.md?toc=/azure/azure-monitor/toc.json)


## <a name="set-up-the-environment-deployment-definition"></a>Configurar a definição de implementação do ambiente

Application Insights Profiler está incluído com o Windows Azure Diagnostics (WAD). A extensão WAD pode ser instalada através de um modelo do Azure RM para o seu cluster do Service Fabric. Existe um modelo de exemplo aqui: [**Modelo que instala WAD num Cluster do Service Fabric.**](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/ServiceFabricCluster.json)

Para configurar o ambiente, siga as seguintes ações:
1. Para se certificar de que está a utilizar [.NET Framework 4.6.1](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed) ou posterior, é suficiente confirmar que o sistema operativo implementado é `Windows Server 2012 R2` ou posterior.

1. Procure o [diagnóstico do Azure](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics) extensão no modelo de implementação de ficheiros e, em seguida, adicione as seguintes `SinksConfig` secção como um elemento subordinado do `WadCfg`. Substitua o `ApplicationInsightsProfiler` valor da propriedade com a sua própria chave de instrumentação do Application Insights:  

      ```json
      "SinksConfig": {
        "Sink": [
          {
            "name": "MyApplicationInsightsProfilerSink",
            "ApplicationInsightsProfiler": "00000000-0000-0000-0000-000000000000"
          }
        ]
      }
      ```

      Para obter informações sobre como adicionar a extensão de diagnóstico ao seu modelo de implementação, consulte [monitorização de utilização e diagnóstico com modelos de VM do Windows e do Azure Resource Manager](https://docs.microsoft.com/azure/virtual-machines/windows/extensions-diagnostics-template?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
1. Implemente cluster do Service Fabric com o modelo do Azure Resource Manager. Se as definições estão corretas, Application Insights Profiler irá instalar e ativar quando a extensão WAD é instalada. 
1. Adicione o Application Insights à sua aplicação do Service Fabric. Seu aplicativo deve ser a enviar dados de pedido para o application insights para que o criador de perfil recolher perfis para seus pedidos. Pode encontrar instruções [aqui.](https://github.com/Microsoft/ApplicationInsights-ServiceFabric)
1. Reimplemente a sua aplicação.

> [SUGESTÃO] Para máquinas virtuais é uma alternativa para os passos de baseados em json acima navegar no portal do Azure para **máquinas virtuais** > **definições de diagnóstico** >  **Coletores** > conjunto enviar dados de diagnóstico para o Application Insights para **ativado** e selecione uma conta do Application Insights ou uma ikey específico.

## <a name="next-steps"></a>Passos Seguintes

- Gerar tráfego para a aplicação (por exemplo, iniciar uma [teste de disponibilidade](https://docs.microsoft.com/azure/application-insights/app-insights-monitor-web-app-availability)). Em seguida, aguarde 10 a 15 minutos para que os rastreios começar a ser enviados para a instância do Application Insights.
- Ver [rastreios do Profiler](https://docs.microsoft.com/azure/application-insights/app-insights-profiler-overview?toc=/azure/azure-monitor/toc.json) no portal do Azure.
- Obtenha ajuda com a resolução de problemas do criador de perfil na [Profiler de resolução de problemas](profiler-troubleshooting.md ?toc=/azure/azure-monitor/toc.json).