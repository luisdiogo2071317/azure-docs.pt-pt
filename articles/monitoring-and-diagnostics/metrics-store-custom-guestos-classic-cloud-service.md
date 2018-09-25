---
title: Enviar o serviço de Cloud clássico de armazenar as métricas de SO convidado para a métrica do Azure Monitor
description: Enviar o serviço de Cloud clássico de armazenar as métricas de SO convidado para a métrica do Azure Monitor
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: howto
ms.date: 09/24/2018
ms.author: ancav
ms.component: metrics
ms.openlocfilehash: be27ff3f8dda3209a011c3ad79d1a7a1f1d259fe
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46986919"
---
# <a name="send-guest-os-metrics-to-the-azure-monitor-metric-store-classic-cloud-service"></a>Enviar o serviço de Cloud clássico de armazenar as métricas de SO convidado para a métrica do Azure Monitor

O Azure Monitor [extensão de diagnóstico do Windows Azure](azure-diagnostics.md) (WAD) permite-lhe recolher métricas e registos do sistema operativo convidado (guestOS) em execução como parte de um cluster do Service Fabric, serviço Cloud ou Máquina Virtual.  A extensão pode enviar telemetria para vários locais diferentes listados no artigo ligado anteriormente.  

Este artigo descreve o processo de métricas de desempenho de SO de convidado de envio para serviços Cloud clássico do Azure para o arquivo de métrica do Azure Monitor. Começando com WAD versão 1.11, pode escrever métricas diretamente com o armazenamento de métricas do Azure Monitor, onde as métricas de plataforma padrão já são recolhidas. Armazená-los nesta localização permite que acesse as mesmas ações disponíveis para as métricas de plataforma.  Ações incluem quase em tempo real de alerta, criação de gráficos, encaminhamento, aceder a partir da REST API e muito mais.  No passado, a extensão WAD escreveu para o armazenamento do Azure, mas não o arquivo de dados do Azure Monitor.  

O processo descrito neste artigo só funciona para contadores de desempenho em serviços Cloud do Azure. Ele não funciona para outras métricas personalizadas. 
   

## <a name="pre-requisites"></a>Pré-requisitos

- Tem de ser um [administrador de serviços ou coadministrador](https://docs.microsoft.com/azure/billing/billing-add-change-azure-subscription-administrator.md) na sua subscrição do Azure 

- A sua subscrição tem de estar registrada com [Microsoft. insights](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-6.8.1) 

- Tem de ter [do Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-6.8.1) instalado, ou pode utilizar [Azure CloudShell](https://docs.microsoft.com/azure/cloud-shell/overview.md) 


## <a name="provision-cloud-service-and-storage-account"></a>Aprovisionar o serviço em nuvem e de conta de armazenamento 

1. Criar e implementar um serviço Cloud clássico (um exemplo de aplicação de serviço cloud clássico e a implementação podem ser encontrados [aqui](../cloud-services/cloud-services-dotnet-get-started.md). 

2. Pode utilizar uma conta de armazenamento existente ou implementar uma nova conta de armazenamento. É melhor se a conta de armazenamento está na mesma região que o serviço Cloud clássico que acabou de criar. No portal do Azure, navegue para o painel de recursos da conta de armazenamento e escolha o **chaves**. Tome nota da conta nome e o armazenamento de conta de armazenamento, elas serão necessárias em passos posteriores.

   ![Chaves de Contas de Armazenamento](./media/metrics-store-custom-guestos-classic-cloud-service/storage-keys.png)



## <a name="create-a-service-principal"></a>Criar um Principal de serviço 

Criar um principal de serviço no seu inquilino do Azure Active Directory com as instruções em.... / azure/azure-resource-manager/resource-group-create-service-principal-portal. Tenha em atenção o seguinte ao percorrer este processo: 
  - Pode colocar qualquer URL para o URL de início de sessão.  
  - Criar novo segredo do cliente para esta aplicação  
  - Guarde a chave e o id de cliente para uso em passos posteriores.  

Dê à aplicação criada no passo anterior *Editor de métricas de monitorização* permissões para o recurso que pretende emitir métricas contra. Se planeia utilizar a aplicação para emitir métricas personalizadas em relação a muitos recursos, pode conceder estas permissões ao nível de grupo ou uma subscrição do recurso.  

> [!NOTE]
> A extensão de diagnóstico utiliza o principal de serviço para autenticar no Azure Monitor e emite métricas do serviço em nuvem 

## <a name="author-diagnostics-extension-configuration"></a>Configuração de extensão de diagnóstico do autor 

Prepare o ficheiro de configuração de extensão de diagnóstico WAD. Esse arquivo dita que os registos e contadores de desempenho a extensão de diagnóstico deve recolher do serviço em nuvem. Segue-se um ficheiro de configuração de diagnósticos de exemplo.  

```XML
<?xml version="1.0" encoding="utf-8"?> 
<DiagnosticsConfiguration xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration"> 
  <PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration"> 
    <WadCfg> 
      <DiagnosticMonitorConfiguration overallQuotaInMB="4096"> 
        <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter="Error" /> 
        <Directories scheduledTransferPeriod="PT1M"> 
          <IISLogs containerName="wad-iis-logfiles" /> 
          <FailedRequestLogs containerName="wad-failedrequestlogs" /> 
        </Directories> 
        <PerformanceCounters scheduledTransferPeriod="PT1M"> 
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT15S" /> 
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT15S" /> 
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Committed Bytes" sampleRate="PT15S" /> 
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Page Faults/sec" sampleRate="PT15S" /> 
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

Na secção "SinksConfig" do seu ficheiro de diagnóstico defina um sink do Azure Monitor novo: 

```XML
  <SinksConfig> 
    <Sink name="AzMonSink"> 
    <AzureMonitor> 
      <ResourceId>-Provide ClassicCloudService’s Resource ID-</ResourceId> 
      <Region>-Azure Region your Cloud Service is deployed in-</Region> 
    </AzureMonitor> 
    </Sink> 
  </SinksConfig> 
```

A secção do ficheiro de configuração em que contadores de lista de desempenho a recolher, adicione o Sink do Azure Monitor. Esta entrada garante que todos os contadores de desempenho especificados são encaminhados para o Azure Monitor, como métricas. Fique à vontade Adicionar/remover contadores de desempenho de acordo com suas necessidades. 

```XML
<PerformanceCounters scheduledTransferPeriod="PT1M" sinks="AzMonSink"> 
 <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT15S" /> 
  … 
</PerformanceCounters> 
```
Por fim, a configuração privada, adicione uma *conta do Azure Monitor* secção. Introduza o ID de cliente do principal de serviço e o segredo que foram criados no passo anterior. 

```XML
<PrivateConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration"> 
  <StorageAccount name="" endpoint="" /> 
    <AzureMonitorAccount> 
      <ServicePrincipalMeta> 
        <PrincipalId>clientId from step 3</PrincipalId> 
        <Secret>client secret from step 3</Secret> 
      </ServicePrincipalMeta> 
    </AzureMonitorAccount> 
</PrivateConfig> 
```
 
Guarde este ficheiro de diagnóstico localmente.  

## <a name="deploy-the-diagnostics-extension-to-your-cloud-service"></a>Implementar a extensão de diagnóstico ao seu serviço Cloud 

Inicie o PowerShell e o início de sessão para o Azure 

```PowerShell
Login-AzureRmAccount 
```

Store os detalhes sobre os detalhes da conta de armazenamento criados no passo anterior em variáveis com os comandos seguintes. 

```PowerShell
$storage_account = <name of your storage account from step 3> 
$storage_keys = <storage account key from step 3> 
```
 
Da mesma forma, definir o caminho de ficheiro de diagnóstico para uma variável usando o comando abaixo. 

```PowerShell
$diagconfig = “<path of the diagnostics configuration file with the Azure Monitor sink configured>” 
```
 
Implemente a extensão de diagnóstico no seu serviço cloud com o arquivo de diagnóstico com o sink de Monitor do Azure configurado com o comando abaixo 

```PowerShell
Set-AzureServiceDiagnosticsExtension -ServiceName <classicCloudServiceName> -StorageAccountName $storage_account -StorageAccountKey $storage_keys -DiagnosticsConfigurationPath $diagconfig 
```
 
> [!NOTE] 
> É ainda obrigatório para fornecer uma conta de armazenamento como parte da instalação da extensão do diagnóstico. Todos os registos e/ou contadores de desempenho especificados no ficheiro de configuração de diagnósticos de escrita para a conta de armazenamento especificada.  

## <a name="plot-metrics-in-the-azure-portal"></a>Gráfico de métricas no portal do Azure 

Navegue para o portal do Azure 

 ![Métricas de portal do Azure](./media/metrics-store-custom-guestos-classic-cloud-service/navigate-metrics.png)

1. No menu da esquerda, clique no Monitor 

1. No painel Monitor, clique no separador pré-visualização de métricas 

1. No recurso lista pendente, selecione o seu serviço Cloud clássico 

1. Nos espaços de nome pendente, selecione **azure.vm.windows.guest** 

1. As métricas de lista pendente, selecione *Memory\Committed Bytes em utilização* 

Pode optar por ver o total de memória utilizada por uma função específica e cada instância de função usando a filtragem de dimensão e a divisão de recursos. 

 ![Métricas de portal do Azure](./media/metrics-store-custom-guestos-classic-cloud-service/metrics-graph.png)

## <a name="next-steps"></a>Passos Seguintes
- Saiba mais sobre [métricas personalizadas](metrics-custom-overview.md).



