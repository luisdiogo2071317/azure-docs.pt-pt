---
title: "Utilize um alerta para acionar um runbook de automatização do Azure"
description: Saiba como acionar um runbook seja executado quando for gerado um alerta do Azure.
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 03/15/2018
ms.topic: article
manager: carmonm
ms.devlang: na
ms.tgt_pltfrm: na
ms.openlocfilehash: e1fc357e654aa60b94944a93118431b944898bff
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/17/2018
---
# <a name="use-an-alert-to-trigger-an-azure-automation-runbook"></a>Utilize um alerta para acionar um runbook de automatização do Azure

Pode utilizar [Azure Monitor](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md?toc=%2fazure%2fautomation%2ftoc.json) para monitorizar as métricas de nível de base e os registos para a maioria dos serviços no Azure. Pode chamar runbooks de automatização do Azure utilizando [grupos ação](../monitoring-and-diagnostics/monitoring-action-groups.md?toc=%2fazure%2fautomation%2ftoc.json) ou através de alertas clássicas para automatizar as tarefas com base em alertas. Este artigo mostra como configurar e executar um runbook através de alertas.

## <a name="alert-types"></a>Tipos de alerta

Pode utilizar runbooks de automatização com três tipos de alerta:
* Alertas de métricas clássicas
* Alertas de registo de atividade
* Quase em tempo real alertas métricas

Quando um alerta chama um runbook, a chamada real é um pedido de HTTP POST para o webhook. O corpo do pedido POST contém um objeto JSON formated que tem propriedades útil que estão relacionados com o alerta. A tabela seguinte apresenta uma lista de ligações para o esquema de payload para cada tipo de alerta:

|Alerta  |Descrição|Esquema de payload  |
|---------|---------|---------|
|[Alerta de métrica clássica](../monitoring-and-diagnostics/insights-alerts-portal.md?toc=%2fazure%2fautomation%2ftoc.json)    |Envia uma notificação quando qualquer métrica de nível de plataforma cumpre uma condição específica. Por exemplo, quando o valor para **% de CPU** numa VM é superior ao **90** durante os últimos 5 minutos.| [Esquema de payload de alerta de métrica de classe](../monitoring-and-diagnostics/insights-webhooks-alerts.md?toc=%2fazure%2fautomation%2ftoc.json#payload-schema)         |
|[Alerta de registo de atividade](../monitoring-and-diagnostics/monitoring-activity-log-alerts.md?toc=%2fazure%2fautomation%2ftoc.json)    |Envia uma notificação quando condições específicas corresponde a qualquer novo evento no registo de atividade do Azure. Por exemplo, quando um `Delete VM` operação ocorre no **myProductionResourceGroup** ou quando um novo evento de estado de funcionamento de serviço de Azure com um **Active Directory** estado é apresentado.| [Esquema de payload de alerta de registo de atividade](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md?toc=%2fazure%2fautomation%2ftoc.json#payload-schema)        |
|[Quase em tempo real alerta métrica](../monitoring-and-diagnostics/monitoring-near-real-time-metric-alerts.md?toc=%2fazure%2fautomation%2ftoc.json)    |Envia uma notificação mais rapidamente do que a métricas alertas quando um ou mais métricas de nível de plataforma cumprem as condições especificadas. Por exemplo, quando o valor para **% de CPU** numa VM é superior ao **90**e o valor de **na rede** é superior ao **500 MB** nas últimas 5 minutos.| [Junto de esquema do payload de alerta métrica em tempo real](../monitoring-and-diagnostics/monitoring-near-real-time-metric-alerts.md?toc=%2fazure%2fautomation%2ftoc.json#payload-schema)          |

Porque os dados que são fornecidos por cada tipo de alerta são diferentes, cada tipo de alerta é processado de forma. Na secção seguinte, irá aprender a criar um runbook para lidar com diferentes tipos de alertas.

## <a name="create-a-runbook-to-handle-alerts"></a>Criar um runbook para lidar com alertas

Para utilizar a automatização com alertas, precisa de um runbook que tem lógica que gere o alerta payload JSON que é transferido para o runbook. O runbook de exemplo seguinte tem de ser chamado a partir de um alerta do Azure. 

Tal como descrito na secção anterior, cada tipo de alerta tem um esquema diferente. O script aceita os dados de webhook no `WebhookData` parâmetro de entrada do runbook de um alerta. Em seguida, o script avalia o payload JSON para determinar o tipo de alerta foi utilizado. 

Este exemplo utiliza um alerta de uma VM. Obtém os dados da VM do payload e, em seguida, utiliza essa informação para parar a VM. A ligação deve ser configurada na conta de automatização onde o runbook é executado.

O runbook utiliza o **AzureRunAsConnection** [conta Run As](automation-create-runas-account.md) para autenticar com o Azure para realizar a ação de gestão contra a VM.

Utilize este exemplo para criar um runbook denominado **Stop-AzureVmInResponsetoVMAlert**. Pode modificar o script do PowerShell e utilizá-la com muitos recursos diferentes.

1. Aceda à sua conta de automatização do Azure.
1. Em **AUTOMATIZAÇÃO de processos**, selecione **Runbooks**.
1. Na parte superior da lista de runbooks, selecione **adicionar um runbook**. 
1. Na página **Adicionar Runbook**, selecione **Criação Rápida**.
1. Para o nome do runbook, introduza **Stop-AzureVmInResponsetoVMAlert**. Para o tipo de runbook, selecione **PowerShell**. Em seguida, selecione **Criar**.  
1. Copie o seguinte exemplo de PowerShell para o **editar** painel. 

    ```powershell-interactive
    <#
    .SYNOPSIS
    This runbook stops a resource management VM in response to an Azure alert trigger.

    .DESCRIPTION
    This runbook stops a resource management VM in response to an Azure alert trigger.
    The input is alert data that has the information required to identify which VM to stop.
    
    DEPENDENCIES
    - The runbook must be called from an Azure alert via a webhook.
    
    REQUIRED AUTOMATION ASSETS
    - An Automation connection asset called "AzureRunAsConnection" that is of type AzureRunAsConnection.
    - An Automation certificate asset called "AzureRunAsCertificate".

    .PARAMETER WebhookData
    Optional. (The user doesn't need to enter anything, but the service always passes an object.)
    This is the data that's sent in the webhook that's triggered from the alert.

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
        # Get the data object from WebhookData.
        $WebhookBody = (ConvertFrom-Json -InputObject $WebhookData.RequestBody)

        # Get the info needed to identify the VM (depends on the payload schema).
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
            # The schema isn't supported.
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

            # Use this only if this is a resource management VM.
            if ($ResourceType -eq "Microsoft.Compute/virtualMachines")
            {
                # This is the VM.
                Write-Verbose "This is a resource management VM." -Verbose

                # Authenticate to Azure by using the service principal and certificate. Then, set the subscription.
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

                # Stop the VM.
                Write-Verbose "Stopping the VM - $ResourceName - in resource group - $ResourceGroupName -" -Verbose
                Stop-AzureRmVM -Name $ResourceName -ResourceGroupName $ResourceGroupName -Force
                # [OutputType(PSAzureOperationResponse")]
            }
            else {
                # ResourceType isn't supported.
                Write-Error "$ResourceType is not a supported resource type for this runbook."
            }
        }
        else {
            # The alert status was not 'Activated', so no action taken.
            Write-Verbose ("No action taken. Alert status: " + $status) -Verbose
        }
    }
    else {
        # Error
        Write-Error "This runbook is meant to be started from an Azure alert webhook only."
    }
    ```
1. Selecione **publicar** para guardar e publicar o runbook.

## <a name="create-an-action-group"></a>Criar um grupo de ação

Um grupo de ação é uma coleção de ações que são acionados por um alerta. Os Runbooks são apenas uma das várias ações que pode utilizar grupos de ação.

1. No portal do Azure, selecione **Monitor** > **definições** > **grupos ação**.
1. Selecione **adicionar grupo de ação**e, em seguida, introduza as informações necessárias:  
    1. No **nome do grupo de ação** caixa, introduza um nome.
    1. No **nome abreviado** caixa, introduza um nome. O nome abreviado é utilizado em vez de um nome de grupo ação completa, quando as notificações são enviadas através da utilização deste grupo de ação.
    1. O **subscrição** caixa é preenchida automaticamente com a sua subscrição atual. Esta é a subscrição na qual o grupo de ação é guardado.
    1. Selecione o grupo de recursos em que o grupo de ação é guardado.

Neste exemplo, criar duas ações: uma ação de runbook e uma ação de notificação.

### <a name="runbook-action"></a>Ação de Runbook

Para criar uma ação de runbook no grupo de ação:

1. Em **ações**, para **nome da ação**, introduza um nome para a ação. Para **tipo de ação**, selecione **Runbook de automatização**.
1. Em **detalhes**, selecione **editar detalhes**.  
1. No **configurar Runbook** página **Runbook origem**, selecione **utilizador**.  
1. Selecione o **subscrição** e **conta de automatização**e, em seguida, selecione o **Stop-AzureVmInResponsetoVMAlert** runbook.  
1. Quando tiver terminado, selecione **OK**.

### <a name="notification-action"></a>Ação de notificação

Para criar uma acção de notificação no grupo de ação:

1. Em **ações**, para **nome da ação**, introduza um nome para a ação. Para **tipo de ação**, selecione **E-Mail**.  
1. Em **detalhes** selecionar, **editar detalhes**.  
1. No **E-Mail** página, introduza o endereço de e-mail a utilizar para notificação e, em seguida, selecione **OK**. A adição de um endereço de e-mail, para além do runbook como uma ação é útil. Será notificado quando o runbook é iniciado.  

    O grupo de ação deverá ser semelhante a imagem seguinte:

   ![Adicionar página de grupo de ação](./media/automation-create-alert-triggered-runbook/add-action-group.png)
1. Para criar o grupo de ação, selecione **OK**.

Pode utilizar este grupo de ação no [alertas de registo de atividade](../monitoring-and-diagnostics/monitoring-activity-log-alerts.md?toc=%2fazure%2fautomation%2ftoc.json) e [quase em tempo real alertas](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md?toc=%2fazure%2fautomation%2ftoc.json#create-an-alert-rule-with-the-azure-portal) que criar.

## <a name="classic-alert"></a>Alerta clássica

Alertas clássicas baseiam-se nas métricas e não utilizam grupos de ação. No entanto, pode configurar uma ação de runbook com base num alerta clássico. 

Para criar um alerta clássico:

1. Selecione **Adicionar alerta de métrica**.
1. Nome do alerta métrico **myVMCPUAlert**. Introduza uma descrição breve do alerta.
1. A métrica condição de alerta, selecione **maior**. Para o **limiar** valor, selecione **10**. Para o **período** valor, selecione **nos últimos cinco minutos**.
1. Em **ação**, selecione **executar um runbook a partir deste alerta**.
1. No **configurar Runbook** página, para **Runbook origem**, selecione **utilizador**. Escolha a sua conta de automatização e, em seguida, selecione o **Stop-AzureVmInResponsetoVMAlert** runbook. Selecione **OK**.
1. Para guardar a regra de alerta, selecione **OK**.

## <a name="next-steps"></a>Passos Seguintes

* Para obter mais informações sobre a iniciar um runbook de automatização utilizando um webhook, consulte [iniciar um runbook a partir de um webhook](automation-webhooks.md).
* Para obter detalhes sobre diferentes formas de iniciar um runbook, consulte [iniciar um runbook](automation-starting-a-runbook.md).
* Para saber como criar um alerta de registo de atividade, consulte [criam alertas de registo de atividade](../monitoring-and-diagnostics/monitoring-activity-log-alerts.md?toc=%2fazure%2fautomation%2ftoc.json).
* Para saber como criar um alerta em tempo real near, consulte [criar uma regra de alerta no portal do Azure](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md?toc=%2fazure%2fautomation%2ftoc.json#create-an-alert-rule-with-the-azure-portal).
