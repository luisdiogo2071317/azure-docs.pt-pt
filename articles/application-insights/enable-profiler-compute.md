---
title: Ativar o Application Insights Profiler para aplicações que estão alojadas nos recursos de serviços Cloud do Azure | Documentos da Microsoft
description: Saiba como configurar o Application Insights Profiler num aplicativo em execução nos serviços Cloud do Azure.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 10/16/2017
ms.reviewer: ramach
ms.author: mbullwin
ms.openlocfilehash: 9eb99ecea8efbbce322e61ac281cd534a112728b
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/10/2018
ms.locfileid: "37950676"
---
# <a name="enable-application-insights-profiler-for-azure-vms-service-fabric-and-azure-cloud-services"></a>Ativar o Application Insights Profiler para VMs do Azure, o Service Fabric e serviços Cloud do Azure

Este artigo demonstra como ativar o Azure Application Insights Profiler num aplicativo ASP.NET que é alojado por um recurso de serviços Cloud do Azure.

Os exemplos neste artigo incluem suporte para máquinas virtuais do Azure, conjuntos de dimensionamento de máquina virtual, do Azure Service Fabric e serviços Cloud do Azure. Os exemplos dependem de modelos que suportam o [do Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) modelo de implementação.  


## <a name="overview"></a>Descrição geral

A imagem seguinte mostra como o Application Insights Profiler funciona com aplicações alojadas nos recursos de serviços Cloud do Azure. Recursos de serviços Cloud do Azure incluem máquinas virtuais, conjuntos de dimensionamento, serviços cloud e clusters do Service Fabric. A imagem utiliza uma máquina virtual do Azure como um exemplo.  

  ![Diagrama que mostra como o Application Insights Profiler funciona com os recursos de serviços Cloud do Azure](./media/enable-profiler-compute/overview.png)

Para habilitar completamente o Profiler, tem de alterar a configuração em três locais:

* O painel de instância do Application Insights no portal do Azure.
* O código de origem do aplicativo (por exemplo, um aplicativo da web ASP.NET).
* O ambiente implementação definição código-fonte (por exemplo, um modelo Azure Resource Manager no ficheiro. JSON).


## <a name="set-up-the-application-insights-instance"></a>Configurar a instância do Application Insights

1. [Criar um novo recurso do Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-create-new-resource), ou selecione um existente. 

2. Aceda ao seu recurso do Application Insights e, em seguida, copie a chave de instrumentação.

   ![Localização da chave de instrumentação](./media/enable-profiler-compute/CopyAIKey.png)

3. Para concluir a configuração da instância do Application Insights para Profiler, conclua o procedimento descrito [ative Profiler. Não precisa de associar as aplicações web, uma vez que as etapas são específicas para o recurso de serviços de aplicações. Certifique-se de que o Profiler está ativado no **Profiler configurar** painel.


## <a name="set-up-the-application-source-code"></a>Configurar o código de origem do aplicativo

### <a name="aspnet-web-applications-azure-cloud-services-web-roles-or-the-service-fabric-aspnet-web-front-end"></a>Aplicações web ASP.NET, funções da web de serviços Cloud do Azure ou o ASP.NET de recursos de infraestrutura do serviço web front-end
Configurar a aplicação para enviar dados telemétricos para uma instância do Application Insights em cada `Request` operação.  

Adicionar a [SDK do Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview#get-started) ao seu projeto de aplicativo. Certifique-se de que as versões do pacote NuGet da seguinte forma:  
  - Para aplicativos ASP.NET: [applicationinsights](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) 2.3.0 ou posterior.
  - Para aplicativos do ASP.NET Core: [Microsoft.ApplicationInsights.AspNetCore](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore/) 2.1.0 ou posterior.
  - Para outras aplicações de .NET e .NET Core (por exemplo, um serviço sem estado do Service Fabric ou uma função de trabalho de serviços Cloud): [applicationinsights](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) ou [applicationinsights](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) 2.3.0 ou posterior.  

### <a name="azure-cloud-services-worker-roles-or-the-service-fabric-stateless-back-end"></a>Funções de trabalho de serviços Cloud do Azure ou o Service Fabric back-end sem estado
Além de executar o passo anterior, se seu aplicativo estiver *não* um aplicativo ASP.NET ou ASP.NET Core (por exemplo, se se trata de uma função de trabalho de serviços Cloud do Azure ou APIs sem monitoração de estado do Service Fabric), faça o seguinte:  

  1. No início do tempo de vida do aplicativo, adicione o seguinte código:  

        ```csharp
        using Microsoft.ApplicationInsights.Extensibility;
        ...
        // Replace with your own Application Insights instrumentation key.
        TelemetryConfiguration.Active.InstrumentationKey = "00000000-0000-0000-0000-000000000000";
        ```
      Para obter mais informações sobre esta configuração de chave de instrumentação globais, consulte [utilizar o Service Fabric com o Application Insights](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/blob/dev/appinsights/ApplicationInsights.md).  

  2. Para qualquer parte do código que deseja instrumentar, adicione uma `StartOperation<RequestTelemetry>` **USING** instrução em torno dele, conforme mostrado no exemplo a seguir:

        ```csharp
        using Microsoft.ApplicationInsights;
        using Microsoft.ApplicationInsights.DataContracts;
        ...
        var client = new TelemetryClient();
        ...
        using (var operation = client.StartOperation<RequestTelemetry>("Insert_Your_Custom_Event_Unique_Name"))
        {
          // ... Code I want to profile.
        }
        ```

        A invocar `StartOperation<RequestTelemetry>` dentro de outra `StartOperation<RequestTelemetry>` âmbito não é suportado. Pode usar `StartOperation<DependencyTelemetry>` aninhada no escopo em vez disso. Por exemplo:  
        
        ```csharp
        using (var getDetailsOperation = client.StartOperation<RequestTelemetry>("GetProductDetails"))
        {
        try
        {
          ProductDetail details = new ProductDetail() { Id = productId };
          getDetailsOperation.Telemetry.Properties["ProductId"] = productId.ToString();
        
          // By using DependencyTelemetry, 'GetProductPrice' is correctly linked as part of the 'GetProductDetails' request.
          using (var getPriceOperation = client.StartOperation<DependencyTelemetry>("GetProductPrice"))
          {
              double price = await _priceDataBase.GetAsync(productId);
              if (IsTooCheap(price))
              {
                  throw new PriceTooLowException(productId);
              }
              details.Price = price;
          }
        
          // Similarly, note how 'GetProductReviews' doesn't establish another RequestTelemetry.
          using (var getReviewsOperation = client.StartOperation<DependencyTelemetry>("GetProductReviews"))
          {
              details.Reviews = await _reviewDataBase.GetAsync(productId);
          }
        
          getDetailsOperation.Telemetry.Success = true;
          return details;
        }
        catch(Exception ex)
        {
          getDetailsOperation.Telemetry.Success = false;
        
          // This exception gets linked to the 'GetProductDetails' request telemetry.
          client.TrackException(ex);
          throw;
        }
        }
        ```

## <a name="set-up-the-environment-deployment-definition"></a>Configurar a definição de implementação do ambiente

O ambiente no qual Profiler e execute seu aplicativo podem ser uma máquina virtual, um conjunto de dimensionamento de máquina virtual, um cluster do Service Fabric ou instância dos serviços de uma nuvem.  

### <a name="virtual-machines-scale-sets-or-service-fabric"></a>Service Fabric, máquinas virtuais ou conjuntos de dimensionamento

Para obter exemplos completos, consulte:  
  * [Máquina virtual](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachine.json)
  * [Conjunto de dimensionamento de máquina virtual](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachineScaleSet.json)
  * [Cluster do Service Fabric](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/ServiceFabricCluster.json)

Para configurar o ambiente, faça o seguinte:
1. Para se certificar de que está a utilizar [.NET Framework 4.6.1](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed) ou posterior, é suficiente confirmar que o sistema operativo implementado é `Windows Server 2012 R2` ou posterior.

2. Procure o [diagnóstico do Azure](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics) extensão no modelo de implementação de ficheiros e, em seguida, adicione as seguintes `SinksConfig` secção como um elemento subordinado do `WadCfg`. Substitua o `ApplicationInsightsProfiler` valor da propriedade com a sua própria chave de instrumentação do Application Insights:  

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

> [!TIP]
> Para máquinas virtuais é uma alternativa para os passos de baseados em json acima navegar no portal do Azure para **máquinas virtuais** > **definições de diagnóstico**  >   **Coletores** > conjunto enviar dados de diagnóstico para o Application Insights para **ativado** e selecione uma conta do Application Insights ou uma ikey específico.

### <a name="azure-cloud-services"></a>Cloud Services do Azure

1. Para se certificar de que está a utilizar [.NET Framework 4.6.1](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed) ou posterior, é suficiente confirmar que o *ServiceConfiguration.\*. cscfg* arquivos têm uma `osFamily` valor "5" ou posterior.

2. Localize a [diagnóstico do Azure](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics) *diagnostics.wadcfgx* de ficheiros para a sua função de aplicação, como mostrado aqui:  

   ![Localização do ficheiro de configuração de diagnósticos](./media/enable-profiler-compute/cloudservice-solutionexplorer.png)  

   Se não encontrar o ficheiro, para saber como ativar a extensão de diagnóstico no seu projeto de serviços Cloud do Azure, veja [configurar diagnósticos para serviços Cloud do Azure e máquinas virtuais](https://docs.microsoft.com/azure/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines#enable-diagnostics-in-cloud-service-projects-before-deploying-them).

3. Adicione as seguintes `SinksConfig` secção como um elemento subordinado do `WadCfg`:  

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
> Se o *diagnostics.wadcfgx* ficheiro também contém outro sink do tipo `ApplicationInsights`, três das seguintes chaves de instrumentação tem de corresponder ao:  
>  * A chave que é utilizada pela sua aplicação.  
>  * A chave utilizada pelo `ApplicationInsights` sink.  
>  * A chave utilizada pelo `ApplicationInsightsProfiler` sink.  
>
> Pode encontrar o valor de chave de instrumentação real é utilizado pelos `ApplicationInsights` serem digeridos a *ServiceConfiguration.\*. cscfg* ficheiros.  
> Após o lançamento do SDK do Azure do Visual Studio 15,5, apenas as chaves de instrumentação que são utilizados pela aplicação e o `ApplicationInsightsProfiler` necessidade de sink para corresponderem entre si.


## <a name="configure-environment-deployment-and-runtime"></a>Configurar a implementação de ambiente e tempo de execução

1. Implemente a definição de implementação do ambiente modificado.  

   Para aplicar as modificações, geralmente envolve uma implementação de modelo completo ou uma nuvem de serviços com base em Publicar através de cmdlets do PowerShell ou o Visual Studio.  

   Segue-se uma abordagem alternativa para máquinas virtuais existentes que atinge apenas a extensão de diagnóstico do Azure:  

    ```powershell
    $ConfigFilePath = [IO.Path]::GetTempFileName()
    # After you export the currently deployed Diagnostics config to a file, edit it to include the ApplicationInsightsProfiler sink.
    (Get-AzureRmVMDiagnosticsExtension -ResourceGroupName "MyRG" -VMName "MyVM").PublicSettings | Out-File -Verbose $ConfigFilePath
    # Set-AzureRmVMDiagnosticsExtension might require the -StorageAccountName argument
    # If your original diagnostics configuration had the storageAccountName property in the protectedSettings section (which is not downloadable), be sure to pass the same original value you had in this cmdlet call.
    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName "MyRG" -VMName "MyVM" -DiagnosticsConfigurationPath $ConfigFilePath
    ```

2. Se estiver a executar a aplicação pretendida [IIS](https://www.microsoft.com/web/downloads/platform.aspx), ativar o `IIS Http Tracing` funcionalidade do Windows ao fazer o seguinte:  

   a. Estabeleça o acesso remoto para o ambiente e, em seguida, utilize o [funcionalidades do Windows adicionar]( https://docs.microsoft.com/iis/configuration/system.webserver/tracing/) janela ou execute o seguinte comando no PowerShell (como administrador):  

    ```powershell
    Enable-WindowsOptionalFeature -FeatureName IIS-HttpTracing -Online -All
    ```  
   b. Se estabelecer o acesso remoto é um problema, pode utilizar [CLI do Azure](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) para executar o seguinte comando:  

    ```powershell
    az vm run-command invoke -g MyResourceGroupName -n MyVirtualMachineName --command-id RunPowerShellScript --scripts "Enable-WindowsOptionalFeature -FeatureName IIS-HttpTracing -Online -All"
    ```


## <a name="enable-profiler-on-on-premises-servers"></a>Ativar Profiler nos servidores no local

Ativar Profiler num servidor no local também é conhecido como de do Application Insights Profiler em execução no modo autônomo. Não está associado a modificações de extensão de diagnóstico do Azure.

Não temos nenhum plano para suportar oficialmente Profiler para servidores no local. Se estiver interessado em fazer experiências com este cenário, pode [baixar código de suporte](https://github.com/ramach-msft/AIProfiler-Standalone). Estamos *não* responsável por manter esse código, ou para responder a problemas e pedidos de funcionalidades que estão relacionadas com o código.

## <a name="next-steps"></a>Passos Seguintes

- Gerar tráfego para a aplicação (por exemplo, iniciar uma [teste de disponibilidade](https://docs.microsoft.com/azure/application-insights/app-insights-monitor-web-app-availability)). Em seguida, aguarde 10 a 15 minutos para que os rastreios começar a ser enviados para a instância do Application Insights.
- Ver [rastreios do Profiler](https://docs.microsoft.com/azure/application-insights/app-insights-profiler#enable-the-profiler) no portal do Azure.
- Obtenha ajuda com a resolução de problemas do criador de perfil na [Profiler de resolução de problemas](app-insights-profiler.md#troubleshooting).
- Saiba mais sobre o Profiler na [Application Insights Profiler](app-insights-profiler.md).
