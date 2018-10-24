---
title: Enviar métricas de SO de convidado para o arquivo de dados do Azure Monitor para um Windows Virtual Machine (clássico)
description: Enviar métricas de SO de convidado para o arquivo de dados do Azure Monitor para um Windows Virtual Machine (clássico)
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: ancav
ms.component: ''
ms.openlocfilehash: 235eda231dfb0f936bf55c7c8d93a8f709fdf9bc
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/24/2018
ms.locfileid: "49954860"
---
# <a name="send-guest-os-metrics-to-the-azure-monitor-data-store-for-a-windows-virtual-machine-classic"></a>Enviar métricas de SO de convidado para o arquivo de dados do Azure Monitor para um Windows Virtual Machine (clássico)

O Azure Monitor [extensão de diagnóstico do Windows Azure](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics) (WAD) permite-lhe recolher métricas e registos da execução de sistema de operativo convidado (SO convidado) como parte de um cluster do Service Fabric, serviço Cloud ou Máquina Virtual. A extensão pode enviar telemetria para vários locais diferentes listados no artigo ligado anteriormente.

Este artigo descreve o processo de métricas de desempenho de SO de convidado de envio para um Windows Máquina Virtual (clássico para o arquivo de métrica do Azure Monitor). Começando com WAD versão 1.11, pode escrever métricas diretamente com o armazenamento de métricas do Azure Monitor, onde as métricas de plataforma padrão já são recolhidas. Armazená-los nesta localização permite que acesse as mesmas ações disponíveis para as métricas de plataforma.  Ações incluem quase em tempo real de alerta, criação de gráficos, encaminhamento, aceder a partir da REST API e muito mais.  No passado, a extensão WAD escreveu para o armazenamento do Azure, mas não o arquivo de dados do Azure Monitor. 

O processo descrito neste artigo só funciona com o sistema operativo do Windows de máquinas de virtuais clássicas.

## <a name="pre-requisites"></a>Pré-requisitos

- Tem de ser um [administrador de serviços ou coadministrador](https://docs.microsoft.com/azure/billing/billing-add-change-azure-subscription-administrator.md) na sua subscrição do Azure 

- A sua subscrição tem de estar registrada com [Microsoft. insights](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-6.8.1) 

- Tem de ter [do Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-6.8.1) instalado, ou pode utilizar [Azure CloudShell](https://docs.microsoft.com/azure/cloud-shell/overview.md) 

## <a name="create-a-classic-virtual-machine-and-storage-account"></a>Criar uma Máquina Virtual clássica e uma conta de armazenamento

1. Criar uma VM clássica no portal do Azure ![Create VM clássica](./media/metrics-store-custom-guestos-classic-vm/create-classic-vm.png)

1. Ao criar esta VM, optar por criar uma nova conta de armazenamento clássicas. Utilizamos esta conta de armazenamento em passos posteriores.

1. No portal do Azure, navegue para o painel de recursos da conta de armazenamento e escolha o **chaves** e anote o nome da conta de armazenamento e a chave da conta de armazenamento. Precisa essas chaves em passos posteriores ![chaves de acesso de armazenamento](./media/metrics-store-custom-guestos-classic-vm/storage-access-keys.png)

## <a name="create-a-service-principal"></a>Criar um Principal de Serviço

Criar um principal de serviço no seu inquilino do Azure Active Directory com as instruções em [criar um principal de serviço](../active-directory/develop/howto-create-service-principal-portal.md). Tenha em atenção o seguinte ao percorrer este processo: 
- Criar novo segredo do cliente para esta aplicação  
- Guarde a chave e o id de cliente para uso em passos posteriores.

Conceda esta aplicação permissões de "Editor de métricas de monitorização" para o recurso que pretende emitir métricas relativamente a. Pode utilizar um grupo de recursos ou uma subscrição completa.  

> [!NOTE]
> A extensão de diagnóstico irá utilizar o principal de serviço para autenticar no Azure Monitor e emite métricas para a sua VM clássica.

## <a name="author-diagnostics-extension-configuration"></a>Configuração de extensão de diagnóstico do autor

1. Prepare o ficheiro de configuração de extensão de diagnóstico WAD. Esse arquivo dita que os registos e contadores de desempenho a extensão de diagnóstico deve recolher para a sua VM clássica. Abaixo está um exemplo.

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <DiagnosticsConfiguration xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
    <PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
        <WadCfg>
        <DiagnosticMonitorConfiguration overallQuotaInMB="4096" sinks="applicationInsights.errors">
            <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter="Error" />
            <Directories scheduledTransferPeriod="PT1M">
                <IISLogs containerName="wad-iis-logfiles" />
                <FailedRequestLogs containerName="wad-failedrequestlogs" />
            </Directories>
            <PerformanceCounters scheduledTransferPeriod="PT1M">
                <PerformanceCounterConfiguration counterSpecifier="\Processor(*)\% Processor Time" sampleRate="PT15S" />
                <PerformanceCounterConfiguration counterSpecifier="\Memory\Available Bytes" sampleRate="PT15S" />
                <PerformanceCounterConfiguration counterSpecifier="\Memory\Committed Bytes" sampleRate="PT15S" />
                <PerformanceCounterConfiguration counterSpecifier="\Memory\% Committed Bytes" sampleRate="PT15S" />
                <PerformanceCounterConfiguration counterSpecifier="\LogicalDisk(*)\Disk Read Bytes/sec" sampleRate="PT15S" />
            </PerformanceCounters>
            <WindowsEventLog scheduledTransferPeriod="PT1M">
                <DataSource name="Application!*[System[(Level=1 or Level=2 or Level=3)]]" />
                <DataSource name="Windows Azure!*[System[(Level=1 or Level=2 or Level=3 or Level=4)]]" />
            </WindowsEventLog>
            <CrashDumps>
                <CrashDumpConfiguration processName="WaIISHost.exe" />
                <CrashDumpConfiguration processName="WaWorkerHost.exe" />
                <CrashDumpConfiguration processName="w3wp.exe" />
            </CrashDumps>
            <Logs scheduledTransferPeriod="PT1M" scheduledTransferLogLevelFilter="Error" />
            <Metrics resourceId="/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResourceGroup/providers/Microsoft.ClassicCompute/virtualMachines/MyClassicVM">
                <MetricAggregation scheduledTransferPeriod="PT1M" />
                <MetricAggregation scheduledTransferPeriod="PT1H" />
            </Metrics>
        </DiagnosticMonitorConfiguration>
        <SinksConfig>
        </SinksConfig>
        </WadCfg>
        <StorageAccount />
    </PublicConfig>
    <PrivateConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
        <StorageAccount name="" endpoint="" />
    </PrivateConfig>
    <IsEnabled>true</IsEnabled>
    </DiagnosticsConfiguration>
    ```
1. Na secção "SinksConfig" do seu ficheiro de diagnóstico defina um sink do Azure Monitor novo:

    ```xml
    <SinksConfig>
        <Sink name="AzMonSink">
            <AzureMonitor>
                <ResourceId>Provide your Classic VM’s Resource ID </ResourceId>
                <Region>Region your VM is deployed in</Region>
            </AzureMonitor>
        </Sink>
    </SinksConfig>
    ```

1. Na secção do ficheiro de configuração, onde a lista dos contadores de desempenho a recolher está listada, encaminhe os contadores de desempenho para o Azure Monitor Sink "AzMonSink".

    ```xml
    <PerformanceCounters scheduledTransferPeriod="PT1M" sinks="AzMonSink">
        <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT15S" />
    ...
    </PerformanceCounters>
    ```

1. A configuração privada definir a conta do Azure Monitor e adicione as informações do principal de serviço a utilizar para emitir métricas.

    ```xml
    <PrivateConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
    <StorageAccount name="" endpoint="" />
        <AzureMonitorAccount>
            <ServicePrincipalMeta>
                <PrincipalId>clientId for your service principal</PrincipalId>
                <Secret>client secret of your service principal</Secret>
            </ServicePrincipalMeta>
        </AzureMonitorAccount>
    </PrivateConfig>
    ```

1. Guarde este ficheiro localmente.

## <a name="deploy-diagnostics-extension-to-your-cloud-service"></a>Implementar a extensão de diagnóstico ao seu serviço Cloud

1. Inicie o PowerShell e inicie sessão

    ```powershell
    Login-AzureRmAccount
    ```

1. Comece por definir o contexto para a VM clássica

    ```powershell
    $VM = Get-AzureVM -ServiceName <VM’s Service_Name> -Name <VM Name>
    ```

1. Defina o contexto da conta de armazenamento clássicas que foi criado com a VM.

    ```powershell
    $StorageContext = New-AzureStorageContext -StorageAccountName <name of your storage account from earlier steps> -storageaccountkey "<storage account key from earlier steps>"
    ```

1.  Definir o caminho de ficheiro de diagnóstico para uma variável usando o comando abaixo.

    ```powershell
    $diagconfig = “<path of the diagnostics configuration file with the Azure Monitor sink configured>”
    ```

1.  Preparar a atualização para a VM clássica com o arquivo de diagnóstico com o sink do Azure Monitor configurado

    ```powershell
    $VM_Update = Set-AzureVMDiagnosticsExtension -DiagnosticsConfigurationPath $diagconfig -VM $VM -StorageContext $Storage_Context
    ```

1.  Implantar a atualização para a VM ao executar o comando abaixo

    ```powershell
    Update-AzureVM -ServiceName "ClassicVMWAD7216" -Name "ClassicVMWAD" -VM $VM_Update.VM
    ```

> [!NOTE]
> É ainda obrigatório para fornecer uma conta de armazenamento como parte da instalação da extensão do diagnóstico. Todos os registos e/ou contadores de desempenho especificados no ficheiro de configuração de diagnósticos de escrita para a conta de armazenamento especificada.

## <a name="plot-the-metrics-in-the-azure-portal"></a>Desenhar as métricas no portal do Azure

1.  Navegue para o portal do Azure

1.  No menu do lado esquerdo, clique no Monitor

1.  No painel do Monitor, clique nas **métricas**
   ![navegue métricas](./media/metrics-store-custom-guestos-classic-vm/navigate-metrics.png)

1. No recurso da lista pendente, selecione a VM clássica

1. Nos espaços de nome pendente, selecione **azure.vm.windows.guest**

1. As métricas de lista pendente, selecione **Memory\Committed Bytes em utilização**
   ![métricas de desenho](./media/metrics-store-custom-guestos-classic-vm/plot-metrics.png)


## <a name="next-steps"></a>Passos Seguintes
- Saiba mais sobre [métricas personalizadas](metrics-custom-overview.md).
