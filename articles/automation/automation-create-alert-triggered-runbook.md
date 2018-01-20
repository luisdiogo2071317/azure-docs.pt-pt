---
title: "Responder a alertas do Azure com um runbook da automatização | Microsoft Docs"
description: "Saiba como acionar um runbook seja executado quando são geradas alertas do Azure."
services: automation
keywords: 
author: georgewallace
ms.author: gwallace
ms.date: 01/11/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: 486a3c80c9d6c19ca0a7ba7d4e3cdcde927053b9
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/19/2018
---
# <a name="respond-to-azure-alerts-with-an-automation-runbook"></a>Responder a alertas do Azure com um runbook de automatização

[Monitor do Azure](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md?toc=%2fazure%2fautomation%2ftoc.json) fornece registos e as métricas de nível de base para a maioria dos serviços dentro do Microsoft Azure. Runbooks de automatização do Azure pode ser chamados utilizando [grupos ação](../monitoring-and-diagnostics/monitoring-action-groups.md?toc=%2fazure%2fautomation%2ftoc.json) ou a partir dos alertas clássicas para automatizar as tarefas com base em alertas. Este artigo mostra como configurar e executar um runbook baseado nestes alertas.

## <a name="alert-types"></a>Tipos de alerta

Os Runbooks são ações suportadas em todos os três tipos de alertas. Quando um alerta chama o runbook, a chamada real é um pedido de HTTP POST para o webhook. O corpo do pedido POST contém um objeto JSON formated que contém as propriedades úteis relacionadas com o alerta. A tabela seguinte contém ligações para o esquema de payload para cada tipo de alerta:

|Alerta  |Descrição|Esquema de payload  |
|---------|---------|---------|
|[Alerta de métrica clássica](../monitoring-and-diagnostics/insights-alerts-portal.md?toc=%2fazure%2fautomation%2ftoc.json)    |Receber uma notificação quando qualquer métrica de nível de plataforma cumpre uma condição específica (por exemplo, numa VM de CPU % é superior a 90 durante os últimos 5 minutos).| [Esquema de payload](../monitoring-and-diagnostics/insights-webhooks-alerts.md?toc=%2fazure%2fautomation%2ftoc.json#payload-schema)         |
|[Alerta de registo de atividade](../monitoring-and-diagnostics/monitoring-activity-log-alerts.md?toc=%2fazure%2fautomation%2ftoc.json)    |Receba uma notificação quando condições específicas (por exemplo, quando uma operação de "Eliminar VM" ocorre no myProductionResourceGroup ou quando um novo evento de estado de funcionamento do serviço com "Ativo" como o estado é apresentado) corresponde a qualquer novo evento no registo de atividade do Azure.| [Esquema de payload](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md?toc=%2fazure%2fautomation%2ftoc.json#payload-schema)        |
|[Quase em tempo real alerta métrica](../monitoring-and-diagnostics/monitoring-near-real-time-metric-alerts.md?toc=%2fazure%2fautomation%2ftoc.json)    |Receber uma notificação mais rapidamente do que a métricas alertas quando um ou mais métricas de nível de plataforma cumprem as condições especificadas (por exemplo, numa VM de CPU % é superior a 90 e rede é maior do que 500 MB para os últimos 5 minutos).| [Esquema de payload](../monitoring-and-diagnostics/monitoring-near-real-time-metric-alerts.md?toc=%2fazure%2fautomation%2ftoc.json#payload-schema)          |

Uma vez que os dados fornecidos por cada alerta diferentes, cada alerta tem de ser processada de forma. Na secção seguinte, irá aprender a criar um runbook para processar os diferentes tipos de alertas.

## <a name="create-a-runbook-to-handle-alerts"></a>Criar um runbook para lidar com alertas

Para utilizar a automatização com alertas, precisa de um runbook que tem lógica para gerir o payload JSON que é transferida para o runbook de alerta. O runbook de exemplo seguinte tem de ser chamado a partir de um alerta do Azure. Tal como descrito na secção anterior, cada tipo de tipo de alerta tem um esquema diferente. O script aceita os dados de webhook no `WebhookData` runbook parâmetro a partir de um alerta de entrada e avalia o payload JSON para determinar o tipo de alerta foi utilizado. O exemplo seguinte seria utilizado num alerta de uma VM. Obtém os dados da VM do payload e utiliza essa informação para parar a VM. A ligação tem de ser configurada na automatização conta onde o runbook é executada.

O runbook utiliza o **AzureRunAsConnection** [conta run as](automation-create-runas-account.md) para autenticar com o Azure para efetuar a ação de gestão contra a VM.

O seguinte script do PowerShell pode ser alterado para utilização com muitos recursos diferentes.

Faça o seguinte exemplo e criar um runbook denominado **Stop-AzureVmInResponsetoVMAlert** com o exemplo do PowerShell.

1. Abra a sua conta de Automatização.
1. Clique em **Runbooks** em **AUTOMATIZAÇÃO DE PROCESSOS**. A lista de runbooks é apresentada.
1. Clique no botão **Adicionar um runbook** encontrado na parte superior da lista. No **adicionar Runbook página**, selecione **criação rápida**.
1. Introduza "Stop-AzureVmInResponsetoVMAlert" para o runbook **nome**e selecione **PowerShell** para **tipo de Runbook**. Clique em **Criar**.
1. Copie o seguinte exemplo do PowerShell para o painel de edição. Clique em **publicar** para guardar e publicar o runbook.

```powershell-interactive
<#
.SYNOPSIS
  This runbook will stop a resource management VM in response to an Azure alert trigger.

.DESCRIPTION
  This runbook will stop an resource management VM in response to an Azure alert trigger.
  Input is alert data with information needed to identify which VM to stop.
  
  DEPENDENCIES
  - The runbook must be called from an Azure alert via a webhook.
  
  REQUIRED AUTOMATION ASSETS
  - An Automation connection asset called "AzureRunAsConnection" that is of type AzureRunAsConnection.
  - An Automation certificate asset called "AzureRunAsCertificate".

.PARAMETER WebhookData
   Optional (user does not need to enter anything, but the service will always pass an object)
   This is the data that is sent in the webhook that is triggered from the alert.

.NOTES
   AUTHOR: Azure Automation Team
   LASTEDIT: 2017-11-22
#>

[OutputType("PSAzureOperationResponse")]

param
(
    [Parameter (Mandatory=$false)]
    [object] $WebhookData
)

$ErrorActionPreference = "stop"

if ($WebhookData)
{
    # Get the data object from WebhookData
    $WebhookBody = (ConvertFrom-Json -InputObject $WebhookData.RequestBody)

    # Get the info needed to identify the VM (depends on the payload schema)
    $schemaId = $WebhookBody.schemaId
    Write-Verbose "schemaId: $schemaId" -Verbose
    if ($schemaId -eq "AzureMonitorMetricAlert") {
        # This is the near-real-time Metric Alert schema
        $AlertContext = [object] ($WebhookBody.data).context
        $ResourceName = $AlertContext.resourceName
        $status = ($WebhookBody.data).status
    }
    elseif ($schemaId -eq "Microsoft.Insights/activityLogs") {
        # This is the Activity Log Alert schema
        $AlertContext = [object] (($WebhookBody.data).context).activityLog
        $ResourceName = (($AlertContext.resourceId).Split("/"))[-1]
        $status = ($WebhookBody.data).status
    }
    elseif ($schemaId -eq $null) {
        # This is the original Metric Alert schema
        $AlertContext = [object] $WebhookBody.context
        $ResourceName = $AlertContext.resourceName
        $status = $WebhookBody.status
    }
    else {
        # Schema not supported
        Write-Error "The alert data schema - $schemaId - is not supported."
    }

    Write-Verbose "status: $status" -Verbose
    if ($status -eq "Activated")
    {
        $ResourceType = $AlertContext.resourceType
        $ResourceGroupName = $AlertContext.resourceGroupName
        $SubId = $AlertContext.subscriptionId
        Write-Verbose "resourceType: $ResourceType" -Verbose
        Write-Verbose "resourceName: $ResourceName" -Verbose
        Write-Verbose "resourceGroupName: $ResourceGroupName" -Verbose
        Write-Verbose "subscriptionId: $SubId" -Verbose

        # Do work only if this is a resource management VM
        if ($ResourceType -eq "Microsoft.Compute/virtualMachines")
        {
            # This is the VM
            Write-Verbose "This is a resource management VM." -Verbose

            # Authenticate to Azure with service principal and certificate and set subscription
            Write-Verbose "Authenticating to Azure with service principal and certificate" -Verbose
            $ConnectionAssetName = "AzureRunAsConnection"
            Write-Verbose "Get connection asset: $ConnectionAssetName" -Verbose
            $Conn = Get-AutomationConnection -Name $ConnectionAssetName
            if ($Conn -eq $null)
            {
                throw "Could not retrieve connection asset: $ConnectionAssetName. Check that this asset exists in the Automation account."
            }
            Write-Verbose "Authenticating to Azure with service principal." -Verbose
            Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint | Write-Verbose
            Write-Verbose "Setting subscription to work against: $SubId" -Verbose
            Set-AzureRmContext -SubscriptionId $SubId -ErrorAction Stop | Write-Verbose

            # Stop the VM
            Write-Verbose "Stopping the VM - $ResourceName - in resource group - $ResourceGroupName -" -Verbose
            Stop-AzureRmVM -Name $ResourceName -ResourceGroupName $ResourceGroupName -Force
            # [OutputType(PSAzureOperationResponse")]
        }
        else {
            # ResourceType not supported
            Write-Error "$ResourceType is not a supported resource type for this runbook."
        }
    }
    else {
        # The alert status was not 'Activated' so no action taken
        Write-Verbose ("No action taken. Alert status: " + $status) -Verbose
    }
}
else {
    # Error
    Write-Error "This runbook is meant to be started from an Azure alert webhook only."
}
```

## <a name="create-an-action-group"></a>Criar um grupo de ação

Um grupo de ação é uma coleção de ações que são executadas com base termina a sessão de um alerta. Os Runbooks são apenas uma das várias ações que estão disponíveis com grupos de ação.

1. No portal, selecione **Monitor**.

1. Selecione **grupos ação** em **definições**.

1. Selecione **adicionar grupo de ação**e preencha os campos.

1. Introduza um nome na caixa de nome de ação grupo e introduza um nome na caixa de nome curto. O nome abreviado é utilizado em vez de um nome de grupo ação completa, quando as notificações são enviadas através deste grupo.

1. A caixa de subscrição é autofills com a sua subscrição atual. Esta subscrição é o que o grupo de ação é guardado.
Selecione o grupo de recursos em que o grupo de ação é guardado.

Neste exemplo, crie duas ações, uma ação de runbook e uma ação de notificação.

### <a name="runbook-action"></a>Ação de Runbook

Uma ação de runbook dentro do grupo de ação de criar os seguintes passos.

1. Em **ações**, dê um nome de ação.

1. Selecione **Runbook de automatização** para o **tipo de ação**.

1. Em **detalhes** selecionar, **editar detalhes**

1. No **configurar Runbook** página, selecione **utilizador** em **Runbook origem**.

1. Escolha o **subscrição** e **conta de automatização**e, finalmente, selecione o runbook que criou no passo anterior chamado "Stop-AzureVmInResponsetoVMAlert".

1. Quando terminar, clique em **OK**.

### <a name="notification-action"></a>Ação de notificação

Os seguintes passos criar uma acção de notificação no grupo de ação.

1. Em **ações**, dê um nome de ação.

1. Selecione **E-Mail** para o **tipo de ação**.

1. Em **detalhes** selecionar, **editar detalhes**

1. No **E-Mail** página, introduza o endereço de correio eletrónico a notificar e clique em **OK**. A adição de um endereço de e-mail, bem como o runbook como uma ação é útil como que seja notificado quando o runbook for iniciado. O grupo de ação deverá ser semelhante a imagem seguinte:

   ![Adicionar página de grupo de ação](./media/automation-create-alert-triggered-runbook/add-action-group.png)

1. Clique em **OK** ao criar o grupo de ação.

Com o grupo de ação criado, pode criar [alertas de registo de atividade](../monitoring-and-diagnostics/monitoring-activity-log-alerts.md?toc=%2fazure%2fautomation%2ftoc.json) ou [quase em tempo real alertas](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md?toc=%2fazure%2fautomation%2ftoc.json#create-an-alert-rule-with-the-azure-portal) e utilizar o grupo de ação que criou.

## <a name="classic-alert"></a>Alerta clássica

Alertas clássicas baseiam-se nas métricas e não não utilize grupos de ação, mas têm ações de runbook com base nos mesmos. Para criar um alerta clássico, utilize os seguintes passos:

1. Selecione **+ Adicionar alerta métrica**.

1. Nome do alerta métrico 'myVMCPUAlert' e forneça uma breve descrição do alerta.

1. Definir a condição do alerta métrica como 'Maior que', defina o limiar como '10' e o período de como 'nos últimos 5 minutos'.

1. Em **ação**, selecione **executar um runbook a partir deste alerta**

1. No **configurar Runbook** página, selecione **utilizador** para **Runbook origem**. Escolha a sua conta de automatização e selecione o **Stop-AzureVmInResponsetoVMAlert** runbook. Clique em **OK**e, em seguida, clique em **OK** novamente para guardar a regra de alerta.

## <a name="next-steps"></a>Passos Seguintes

* Para obter mais informações sobre a iniciar runbooks de automatização com webhooks, consulte [iniciar um runbook a partir de um webhook](automation-webhooks.md)
* Para obter detalhes sobre diferentes formas de iniciar um runbook, consulte [iniciar um Runbook](automation-starting-a-runbook.md).
* Para saber como criar um alerta de registo de atividade, consulte [criam alertas de registo de atividade](../monitoring-and-diagnostics/monitoring-activity-log-alerts.md?toc=%2fazure%2fautomation%2ftoc.json)
* Para ver como criar um alerta em tempo real near, visite [criar uma regra de alerta com o portal do Azure](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md?toc=%2fazure%2fautomation%2ftoc.json#create-an-alert-rule-with-the-azure-portal).