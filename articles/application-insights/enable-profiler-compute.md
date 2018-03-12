---
title: "Ativar o gerador de perfis do Application Insights para aplicações alojadas nos recursos de Cloud Services do Azure | Microsoft Docs"
description: "Saiba como configurar o gerador de perfis do Application Insights numa aplicação em execução nos serviços de nuvem do Azure."
services: application-insights
documentationcenter: 
author: ramach-msft
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 10/16/2017
ms.author: ramach
ms.openlocfilehash: a24695f7bbb5fb0546e27c934319a60a3418b9e1
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/08/2018
---
# <a name="enable-application-insights-profiler-for-azure-vms-service-fabric-and-azure-cloud-services"></a>Ativar o gerador de perfis do Application Insights para as VMs do Azure, recursos de infraestrutura de serviço e serviços em nuvem do Azure

Este artigo demonstra como ativar o gerador de perfis do Azure Application Insights numa aplicação de ASP.NET que é alojada por um recurso de Cloud Services do Azure.

Os exemplos neste artigo incluem suporte para máquinas virtuais do Azure, conjuntos de dimensionamento de máquina virtual, do Azure Service Fabric e Cloud Services do Azure. Os exemplos dependem de modelos que suportam o [do Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) modelo de implementação.  


## <a name="overview"></a>Descrição geral

A imagem seguinte mostra como o gerador de perfis do Application Insights funciona com aplicações que estão alojadas nos recursos de Cloud Services do Azure. Recursos de serviços em nuvem do Azure incluem máquinas virtuais, conjuntos de dimensionamento, os serviços de nuvem e clusters de Service Fabric. A imagem utiliza uma máquina virtual do Azure como um exemplo.  

  ![Diagrama que mostra como funciona o gerador de perfis do Application Insights com recursos do Cloud Services do Azure](./media/enable-profiler-compute/overview.png)

Para ativar completamente o gerador de perfis, tem de alterar a configuração em três locais:

* O painel de instância do Application Insights no portal do Azure.
* O código fonte da aplicação (por exemplo, uma aplicação da web ASP.NET).
* O ambiente implementação definição código fonte (por exemplo, um modelo Azure Resource Manager no ficheiro. JSON).


## <a name="set-up-the-application-insights-instance"></a>Configurar a instância do Application Insights

1. [Criar um novo recurso do Application Insights](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-create-new-resource), ou selecione um existente. 

2. Vá para o recurso do Application Insights e, em seguida, copie a chave de instrumentação.

   ![Localização da chave de instrumentação](./media/enable-profiler-compute/CopyAIKey.png)

3. Para concluir a configuração da instância do Application Insights para o gerador de perfis, conclua o procedimento descrito [ativar o gerador de perfis. Não precisa de associar as aplicações web, porque os passos são específicos para o recurso de serviços de aplicações. Certifique-se de que o gerador de perfis está ativado no **configurar o gerador de perfis** painel.


## <a name="set-up-the-application-source-code"></a>Configurar o código fonte da aplicação

### <a name="aspnet-web-applications-azure-cloud-services-web-roles-or-the-service-fabric-aspnet-web-front-end"></a>Aplicações web ASP.NET, funções da web de Cloud Services do Azure ou o ASP.NET de recursos de infraestrutura de serviço web front-end
Configurar a sua aplicação para enviar dados de telemetria para uma instância do Application Insights em cada `Request` operação.  

Adicionar o [Application Insights SDK](https://docs.microsoft.com/azure/application-insights/app-insights-overview#get-started) ao seu projeto de aplicação. Certifique-se de que as versões do pacote NuGet da seguinte forma:  
  - Para aplicações ASP.NET: [applicationinsights](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) 2.3.0 ou posterior.
  - Para aplicações ASP.NET Core: [Microsoft.ApplicationInsights.AspNetCore](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore/) 2.1.0 ou posterior.
  - Para outras aplicações de .NET e .NET Core (por exemplo, um serviço sem monitorização de estado de Service Fabric ou uma função de trabalho de serviços em nuvem): [Microsoft.ApplicationInsights](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) ou [applicationinsights](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) 2.3.0 ou posterior.  

### <a name="azure-cloud-services-worker-roles-or-the-service-fabric-stateless-back-end"></a>As funções de trabalho de serviços em nuvem do Azure ou o Service Fabric sem monitorização de estado de back-end
Para além de ter concluído o passo anterior, se a aplicação for *não* uma aplicação ASP.NET ou ASP.NET Core (por exemplo, se se tratar de uma função de trabalho do Cloud Services do Azure ou APIs sem monitorização de estado do Service Fabric), efetue o seguinte procedimento:  

  1. No início da duração de aplicação, adicione o seguinte código:  

        ```csharp
        using Microsoft.ApplicationInsights.Extensibility;
        ...
        // Replace with your own Application Insights instrumentation key.
        TelemetryConfiguration.Active.InstrumentationKey = "00000000-0000-0000-0000-000000000000";
        ```
      Para obter mais informações sobre esta configuração de chave de instrumentação globais, consulte [utilize Service Fabric com o Application Insights](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/blob/dev/appinsights/ApplicationInsights.md).  

  2. Para qualquer fragmento de código que pretende instrumentar, adicione um `StartOperation<RequestTelemetry>` **USING** declaração à volta da mesma, conforme mostrado no exemplo seguinte:

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

        Chamar `StartOperation<RequestTelemetry>` dentro de outra `StartOperation<RequestTelemetry>` âmbito não é suportado. Pode utilizar `StartOperation<DependencyTelemetry>` o aninhada no âmbito em vez disso. Por exemplo:  
        
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

O ambiente em que o gerador de perfis e executar a aplicação podem ser uma máquina virtual, um conjunto de dimensionamento de máquina virtual, um cluster do Service Fabric, ou a instância dos serviços de nuvem.  

### <a name="virtual-machines-scale-sets-or-service-fabric"></a>Máquinas virtuais, conjuntos de dimensionamento ou Service Fabric

Para obter exemplos completos, consulte:  
  * [Máquina virtual](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachine.json)
  * [Conjunto de dimensionamento da máquina virtual](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachineScaleSet.json)
  * [Cluster do Service Fabric](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/ServiceFabricCluster.json)

Para configurar o seu ambiente, efetue o seguinte:
1. Para se certificar de que está a utilizar [.NET Framework 4.6.1](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed) ou posterior, é suficiente confirmar que o SO implementado é `Windows Server 2012 R2` ou posterior.

2. Procure o [diagnósticos do Azure](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics) extensão no modelo de implementação de ficheiros e, em seguida, adicione o seguinte `SinksConfig` secção como um elemento subordinado do `WadCfg`. Substitua o `ApplicationInsightsProfiler` valor da propriedade com a sua própria chave de instrumentação do Application Insights:  

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

      Para obter informações sobre como adicionar a extensão de diagnóstico para o modelo de implementação, consulte [monitorização de utilização e diagnóstico com modelos de VM do Windows e o Azure Resource Manager](https://docs.microsoft.com/azure/virtual-machines/windows/extensions-diagnostics-template?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

> [!TIP]
> Para máquinas virtuais uma alternativa para os passos de json com base acima é navegue no portal do Azure para **máquinas virtuais** > **definições de diagnóstico**  >   **Sinks** > Set enviar dados de diagnóstico para o Application Insights para **ativado** e selecione uma conta do Application Insights ou um ikey específico.

### <a name="azure-cloud-services"></a>Cloud Services do Azure

1. Para se certificar de que está a utilizar [.NET Framework 4.6.1](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed) ou posterior, é suficiente confirmar que o *ServiceConfiguration.\*. cscfg* os ficheiros têm um `osFamily` valor de "5" ou posterior.

2. Localize o [diagnósticos do Azure](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics) *diagnostics.wadcfgx* de ficheiros para a função de aplicação, conforme mostrado aqui:  

   ![Localização do ficheiro de configuração de diagnóstico](./media/enable-profiler-compute/cloudservice-solutionexplorer.png)  

   Se não é possível localizar o ficheiro, para saber como ativar a extensão de diagnóstico do seu projeto de Cloud Services do Azure, consulte o artigo [configurar diagnósticos para máquinas virtuais e serviços Cloud do Azure](https://docs.microsoft.com/azure/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines#enable-diagnostics-in-cloud-service-projects-before-deploying-them).

3. Adicione o seguinte `SinksConfig` secção como um elemento subordinado do `WadCfg`:  

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
> Se o *diagnostics.wadcfgx* também contém outro dependente do tipo `ApplicationInsights`, todas as três das seguintes chaves de instrumentação tem de corresponder ao:  
>  * A chave que é utilizada pela sua aplicação.  
>  * A chave que é utilizada pelo `ApplicationInsights` sink.  
>  * A chave que é utilizada pelo `ApplicationInsightsProfiler` sink.  
>
> Pode encontrar o valor de chave de instrumentação real é utilizado pelo `ApplicationInsights` sink *ServiceConfiguration.\*. cscfg* ficheiros.  
> Após o lançamento do Visual Studio 15.5 Azure SDK, apenas as chaves de instrumentação que são utilizados pela aplicação e o `ApplicationInsightsProfiler` sink necessidade para corresponderem entre si.


## <a name="configure-environment-deployment-and-runtime"></a>Configurar a implementação de ambiente e o tempo de execução

1. Implemente a definição de implementação do ambiente modificadas.  

   Para aplicar as alterações, normalmente, envolve uma implementação de modelo completo ou uma nuvem serviços com base publicar através de cmdlets do PowerShell ou o Visual Studio.  

   Segue-se uma abordagem alternativa de máquinas virtuais existentes que tocar apenas a extensão de diagnóstico do Azure:  

    ```powershell
    $ConfigFilePath = [IO.Path]::GetTempFileName()
    # After you export the currently deployed Diagnostics config to a file, edit it to include the ApplicationInsightsProfiler sink.
    (Get-AzureRmVMDiagnosticsExtension -ResourceGroupName "MyRG" -VMName "MyVM").PublicSettings | Out-File -Verbose $ConfigFilePath
    # Set-AzureRmVMDiagnosticsExtension might require the -StorageAccountName argument
    # If your original diagnostics configuration had the storageAccountName property in the protectedSettings section (which is not downloadable), be sure to pass the same original value you had in this cmdlet call.
    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName "MyRG" -VMName "MyVM" -DiagnosticsConfigurationPath $ConfigFilePath
    ```

2. Se estiver a executar a aplicação pretendida [IIS](https://www.microsoft.com/web/platform/server.aspx), ativar o `IIS Http Tracing` funcionalidade do Windows ao fazer o seguinte:  

   a. Estabelecer um acesso remoto para o ambiente e, em seguida, utilize o [funcionalidades do Windows adicionar]( https://docs.microsoft.com/iis/configuration/system.webserver/tracing/) janela ou execute o seguinte comando do PowerShell (como administrador):  

    ```powershell
    Enable-WindowsOptionalFeature -FeatureName IIS-HttpTracing -Online -All
    ```  
   b. Se estabelecer o acesso remoto é um problema, pode utilizar [CLI do Azure](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) para executar o seguinte comando:  

    ```powershell
    az vm run-command invoke -g MyResourceGroupName -n MyVirtualMachineName --command-id RunPowerShellScript --scripts "Enable-WindowsOptionalFeature -FeatureName IIS-HttpTracing -Online -All"
    ```


## <a name="enable-profiler-on-on-premises-servers"></a>Ativar o gerador de perfis em servidores no local

Ativar o gerador de perfis num servidor no local também é conhecido como em execução gerador de Insights aplicação perfis no modo autónomo. Não está associado às modificações de extensão de diagnóstico do Azure.

Não temos nenhum plano para suportar oficialmente gerador de perfis para servidores no local. Se estiver interessado em conseguirmos uma com este cenário, pode [transferir o código de suporte](https://github.com/ramach-msft/AIProfiler-Standalone). Estamos *não* responsável pela manutenção esse código ou para responder a pedidos de funcionalidades que estão relacionados com o código e os problemas.

## <a name="next-steps"></a>Passos Seguintes

- Gerar tráfego à sua aplicação (por exemplo, inicie um [teste de disponibilidade](https://docs.microsoft.com/azure/application-insights/app-insights-monitor-web-app-availability)). Em seguida, aguarde 10 a 15 minutos para rastreios começar a ser enviadas para a instância do Application Insights.
- Consulte [rastreios de gerador de perfis](https://docs.microsoft.com/azure/application-insights/app-insights-profiler#enable-the-profiler) no portal do Azure.
- Obter ajuda para resolver problemas de gerador de perfis no [gerador de perfis de resolução de problemas](app-insights-profiler.md#troubleshooting).
- Saiba mais sobre o gerador de perfis no [gerador de perfis do Application Insights](app-insights-profiler.md).
