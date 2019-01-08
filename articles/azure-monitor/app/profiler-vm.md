---
title: Perfil de aplicações web em execução numa VM do Azure com o Application Insights Profiler | Documentos da Microsoft
description: Criar perfis para aplicações web numa VM do Azure com o Application Insights Profiler.
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
ms.openlocfilehash: e8f80e7d19a961c22b4e1e88556ac165d2558034
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/08/2019
ms.locfileid: "54082872"
---
# <a name="profile-web-apps-running-on-an-azure-virtual-machine-or-virtual-machine-scale-set-with-application-insights-profiler"></a>Conjunto de aplicações de web de perfil em execução numa máquina virtual do Azure ou o dimensionamento de máquinas virtuais com o Application Insights Profiler
Também pode implementar o criador de perfil do Application Insights estes serviços:
* [Serviço de Aplicações do Azure](../../azure-monitor/app/profiler.md?toc=/azure/azure-monitor/toc.json)
* [Serviços Cloud](profiler-cloudservice.md ?toc=/azure/azure-monitor/toc.json)
* [Service Fabric](profiler-vm.md?toc=/azure/azure-monitor/toc.json)

## <a name="deploy-profiler-on-a-virtual-machine-or-scale-set"></a>Implementar o Profiler numa máquina Virtual ou conjunto de dimensionamento
Esta página irá guiá-lo pelos passos necessários obter o criador de perfil do Application Insights em execução na sua VM do Azure ou o dimensionamento de máquina virtual do Azure definido. Application Insights Profiler é instalado com a extensão de diagnóstico do Windows Azure para as VMs. A extensão tem de ser configurado para executar o criador de perfil e o SDK de informações da aplicação deve ser criado na sua aplicação.

1. Adicionar o application Insights SDK para sua [aplicativo ASP.Net](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net) ou regular [aplicativo .NET.](https://docs.microsoft.com/azure/application-insights/app-insights-windows-services?toc=/azure/azure-monitor/toc.json) Tem enviar telemetria de pedido para o Application Insights Consulte perfis para seus pedidos.
1. Instale a extensão de diagnóstico do Windows Azure na sua VM. Para exemplos de modelo do Resource Manager completo, veja:  
    * [Máquina virtual](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachine.json)
    * [Conjunto de dimensionamento de máquina virtual](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachineScaleSet.json)
    
    A parte de chave é ApplicationInsightsProfilerSink no WadCfg. Adicione outro sink a esta secção para informar ao WAD para permitir que o criador de perfil enviar dados para a iKey.
    ```json
      "SinksConfig": {
        "Sink": [
          {
            "name": "ApplicationInsightsSink",
            "ApplicationInsights": "85f73556-b1ba-46de-9534-606e08c6120f"
          },
          {
            "name": "MyApplicationInsightsProfilerSink",
            "ApplicationInsightsProfiler": "85f73556-b1ba-46de-9534-606e08c6120f"
          }
        ]
      },
    ```

1. Implemente a definição de implementação do ambiente modificado.  

   Para aplicar as modificações, geralmente envolve uma implementação de modelo completo ou um serviço cloud baseado publicar através de cmdlets do PowerShell ou o Visual Studio.  

   Os seguintes comandos do powershell são uma abordagem alternativa para máquinas virtuais existentes que atinge apenas a extensão de diagnóstico do Azure. Terá de adicionar o ProfilerSink conforme indicado acima para a configuração que é devolvida pelo comando Get-AzureRmVMDiagnosticsExtension. Em seguida, passe a configuração atualizada para o comando Set-AzureRmVMDiagnosticsExcension.

    ```powershell
    $ConfigFilePath = [IO.Path]::GetTempFileName()
    # After you export the currently deployed Diagnostics config to a file, edit it to include the ApplicationInsightsProfiler sink.
    (Get-AzureRmVMDiagnosticsExtension -ResourceGroupName "MyRG" -VMName "MyVM").PublicSettings | Out-File -Verbose $ConfigFilePath
    # Set-AzureRmVMDiagnosticsExtension might require the -StorageAccountName argument
    # If your original diagnostics configuration had the storageAccountName property in the protectedSettings section (which is not downloadable), be sure to pass the same original value you had in this cmdlet call.
    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName "MyRG" -VMName "MyVM" -DiagnosticsConfigurationPath $ConfigFilePath
    ```

1. Se estiver a executar a aplicação pretendida [IIS](https://www.microsoft.com/web/downloads/platform.aspx), ativar o `IIS Http Tracing` funcionalidade do Windows.

   a. Estabeleça o acesso remoto para o ambiente e, em seguida, utilize o [funcionalidades do Windows adicionar]( https://docs.microsoft.com/iis/configuration/system.webserver/tracing/) janela ou execute o seguinte comando no PowerShell (como administrador):  

    ```powershell
    Enable-WindowsOptionalFeature -FeatureName IIS-HttpTracing -Online -All
    ```  
   b. Se estabelecer o acesso remoto é um problema, pode utilizar [CLI do Azure](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) para executar o seguinte comando:  

    ```powershell
    az vm run-command invoke -g MyResourceGroupName -n MyVirtualMachineName --command-id RunPowerShellScript --scripts "Enable-WindowsOptionalFeature -FeatureName IIS-HttpTracing -Online -All"
    ```

1. Implemente a sua aplicação.

## <a name="can-profiler-run-on-on-premises-servers"></a>Criador de perfil pode ser executado em servidores no local?
Não temos planos para suportar o Application Insights Profiler para servidores no local.

## <a name="next-steps"></a>Passos Seguintes

- Gerar tráfego para a aplicação (por exemplo, iniciar uma [teste de disponibilidade](https://docs.microsoft.com/azure/application-insights/app-insights-monitor-web-app-availability)). Em seguida, aguarde 10 a 15 minutos para que os rastreios começar a ser enviados para a instância do Application Insights.
- Ver [rastreios do Profiler](https://docs.microsoft.com/azure/application-insights/app-insights-profiler-overview?toc=/azure/azure-monitor/toc.json) no portal do Azure.
- Obtenha ajuda com a resolução de problemas do criador de perfil na [Profiler de resolução de problemas](profiler-troubleshooting.md ?toc=/azure/azure-monitor/toc.json).
