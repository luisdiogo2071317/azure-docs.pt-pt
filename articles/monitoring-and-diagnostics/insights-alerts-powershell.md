---
title: Utilizar o PowerShell para criar alertas clássicas para serviços do Azure | Microsoft Docs
description: Acionar mensagens de correio eletrónico ou notificações ou chame URLs de Web site (webhooks) ou automatização quando são satisfeitas as condições que especificar.
author: rboucher
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 03/28/2018
ms.author: robb
ms.component: alerts
ms.openlocfilehash: b08a8f66add45d64085ac05605fe3c7d7f91b705
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36286204"
---
# <a name="use-powershell-to-create-alerts-for-azure-services"></a>Utilizar o PowerShell para criar alertas para os serviços do Azure
> [!div class="op_single_selector"]
> * [Portal](insights-alerts-portal.md)
> * [PowerShell](insights-alerts-powershell.md)
> * [CLI](insights-alerts-command-line-interface.md)
>
>

> [!NOTE]
> Este artigo descreve como criar alertas de métricas clássicas mais antigas. Azure suporta agora a monitorizar [mais recente, melhor alertas métricas](monitoring-near-real-time-metric-alerts.md). Estes alertas podem monitorizar várias métricas e permitir alertas nas métricas dimensional. Suporte do PowerShell para alertas métricas mais recentes está disponível em breve.
>
>

Este artigo mostra como configurar alertas de métricas clássicas do Azure utilizando o PowerShell.  

Pode receber alertas com base nas métricas para os serviços do Azure ou pode receber alertas de eventos que ocorrem no Azure.

* **Valores métricos**: O alerta é acionado quando o valor de uma métrica especificado atravesse um limiar atribuir em qualquer direção. Ou seja, aciona ambas quando a condição for satisfeita primeiro e, em seguida, quando que já não está a ser condição.    
* **Eventos de registo de atividade**: pode acionar um alerta num *cada* eventos ou quando ocorre a determinados eventos. Para obter mais informações sobre alertas de registo de atividade, consulte [criar atividade alertas de registo (clássica)](monitoring-activity-log-alerts.md).

Pode configurar um alerta de métrico clássico para efetuar as seguintes tarefas quando aciona:

* Envie notificações por e-mail para o administrador de serviços e coadministradores.
* Envie correio eletrónico para os endereços de e-mail adicionais que especificar.
* Chame um webhook.
* Inicie a execução de um runbook do Azure (apenas a partir do portal do Azure).

Pode configurar e obter informações sobre regras de alertas a partir das seguintes localizações: 

* [Portal do Azure](insights-alerts-portal.md)
* [PowerShell](insights-alerts-powershell.md)
* [Interface de linha de comandos (CLI) do Azure](insights-alerts-command-line-interface.md)
* [API de REST de Monitor do Azure](https://msdn.microsoft.com/library/azure/dn931945.aspx)

Para obter informações adicionais, pode sempre escrever ```Get-Help``` seguido pelo comando do PowerShell que precisa de ajuda.

## <a name="create-alert-rules-in-powershell"></a>Criar regras de alertas no PowerShell
1. Inicie sessão no Azure.   

    ```PowerShell
    Connect-AzureRmAccount

    ```
2. Obter uma lista de subscrições disponíveis. Certifique-se de que está a trabalhar com a subscrição correta. Se não estiver, obtenha para a subscrição correta, utilizando o resultado da `Get-AzureRmSubscription`.

    ```PowerShell
    Get-AzureRmSubscription
    Get-AzureRmContext
    Set-AzureRmContext -SubscriptionId <subscriptionid>
    ```
3. Lista as regras existentes num grupo de recursos utilizando o seguinte comando:

   ```PowerShell
   Get-AzureRmAlertRule -ResourceGroup <myresourcegroup> -DetailedOutput
   ```
4. Para criar uma regra, tem de ter várias partes importantes de informações pela primeira vez.

    - O **ID de recurso** para o recurso que pretende definir um alerta para.
    - O **as definições de métrica** que estão disponíveis para esse recurso.

     Uma forma de obter o ID de recurso é utilizar o portal do Azure. Partindo do princípio de que o recurso já foi criado, selecione-o no portal. Em seguida, no painel seguinte, no **definições** secção, selecione **propriedades**. **ID de recurso** é um campo no painel seguinte. 
     
     Também pode obter o ID de recurso utilizando [Explorador de recursos do Azure](https://resources.azure.com/).

     Segue-se um ID de recurso de exemplo para uma aplicação web:

     ```
     /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename
     ```

     Pode utilizar `Get-AzureRmMetricDefinition` para ver a lista de todas as definições de métricas para um recurso específico:

     ```PowerShell
     Get-AzureRmMetricDefinition -ResourceId <resource_id>
     ```

     O exemplo seguinte gera uma tabela com o nome da métrica e a unidade para esse métrica:

     ```PowerShell
     Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit

     ```
     Para obter uma lista completa das opções disponíveis para o Get-AzureRmMetricDefinition, execute `Get-Help Get-AzureRmMetricDefinition -Detailed`.
5. O exemplo a seguir configura um alerta no recurso Web site. Os acionadores de alerta sempre que recebe consistentemente qualquer tráfego de 5 minutos e novamente quando não recebe nenhum tráfego de 5 minutos.

    ```PowerShell
    Add-AzureRmMetricAlertRule -Name myMetricRuleWithWebhookAndEmail -Location "East US" -ResourceGroup myresourcegroup -TargetResourceId /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename -MetricName "BytesReceived" -Operator GreaterThan -Threshold 2 -WindowSize 00:05:00 -TimeAggregationOperator Total -Description "alert on any website activity"

    ```
6. Para criar um webhook ou enviar o e-mail quando um alerta é acionado, crie primeiro o e-mail ou o webhook. Em seguida, criar de imediato a regra posteriormente com-etiqueta de ações, como mostrado no exemplo seguinte. Não é possível associar webhooks ou e-mails com regras que já foram criadas.

    ```PowerShell
    $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
    $actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://www.contoso.com?token=mytoken

    Add-AzureRmMetricAlertRule -Name myMetricRuleWithWebhookAndEmail -Location "East US" -ResourceGroup myresourcegroup -TargetResourceId /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename -MetricName "BytesReceived" -Operator GreaterThan -Threshold 2 -WindowSize 00:05:00 -TimeAggregationOperator Total -Actions $actionEmail, $actionWebhook -Description "alert on any website activity"
    ```

7. Observe as regras individuais para verificar se os alertas foi criados corretamente.

    ```PowerShell
    Get-AzureRmAlertRule -Name myMetricRuleWithWebhookAndEmail -ResourceGroup myresourcegroup -DetailedOutput

    Get-AzureRmAlertRule -Name myLogAlertRule -ResourceGroup myresourcegroup -DetailedOutput
    ```
8. Elimine os alertas. Estes comandos, elimine as regras que foram criadas anteriormente neste artigo.

    ```PowerShell
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myrule
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myMetricRuleWithWebhookAndEmail
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myLogAlertRule
    ```

## <a name="next-steps"></a>Passos Seguintes
* [Obter uma descrição geral da monitorização do Azure](monitoring-overview.md), incluindo os tipos de informações que pode recolher e monitorizar.
* Saiba como [configurar webhooks alertas](insights-webhooks-alerts.md).
* Saiba como [configurar alertas de eventos de registo de atividade](monitoring-activity-log-alerts.md).
* Saiba mais sobre [runbooks de automatização do Azure](../automation/automation-starting-a-runbook.md).
* Obter um [descrição geral de recolha de registos de diagnóstico](monitoring-overview-of-diagnostic-logs.md) para recolher métricas de alta frequência detalhadas do seu serviço.
* Obter um [descrição geral da coleção de métricas](insights-how-to-customize-monitoring.md) para se certificar de que o serviço está disponível e reativa.
