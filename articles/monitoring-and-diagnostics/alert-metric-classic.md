---
title: Criar, ver e gerir alertas de métrica clássicas com o Azure Monitor
description: Saiba como utilizar o portal do Azure, CLI ou o Powershell para criar, ver e gerir regras de alerta de métrica clássicas.
author: snehithm
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/18/2018
ms.author: snmuvva
ms.component: alerts
ms.openlocfilehash: e18b670b94962c0e7aa469402228fd4ed95d846b
ms.sourcegitcommit: 02ce0fc22a71796f08a9aa20c76e2fa40eb2f10a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/08/2018
ms.locfileid: "51287256"
---
# <a name="create-view-and-manage-classic-metric-alerts-using-azure-monitor"></a>Criar, ver e gerir alertas de métrica clássicas com o Azure Monitor

Alertas de métricas clássicas no Azure Monitor oferecem uma forma de ser notificado quando uma das suas métricas ultrapassam um determinado limiar. Alertas de métricas clássicas é uma funcionalidade mais antiga que permite para alertar apenas em métricas não dimensionais. Existe uma funcionalidade mais recente existente chamada alertas de métricas que tem funcionalidade melhorada através de alertas de métricas clássicas. Pode saber mais sobre a nova funcionalidade de alertas de métricas no [descrição geral dos alertas de métrica](alert-metric-overview.md). Neste artigo, descreveremos como criar, ver e gerir clássicas métrica regras de alerta através do portal do Azure, CLI do Azure e Powershell.

## <a name="with-azure-portal"></a>Com o portal do Azure

1. Na [portal](https://portal.azure.com/), localize o recurso que pretende monitorizar e, em seguida, selecioná-lo.

2. Na **monitorização** secção, selecione **alertas (clássico)**. O texto e o ícone podem variar um pouco para recursos diferentes. Se não encontrar **alertas (clássico)** aqui, poderá encontrá-lo na **alertas** ou **regras de alerta**.

    ![Monitorização](media/alert-metric-classic/AlertRulesButton.png)

3. Selecione o **Adicionar alerta de métrica (clássico)** de comandos e, em seguida, preencha os campos.

    ![Adicionar Alerta](media/alert-metric-classic/AddAlertOnlyParamsPage.png)

4. **Nome** sua regra de alerta. Em seguida, escolha uma **Descrição**, que também é apresentado em e-mails de notificação.

5. Selecione o **métrica** que pretende monitorizar. Em seguida, escolha uma **condição** e **limiar** valor para a métrica. Escolha também os **período** de tempo que a regra de métrica deve ser satisfeita antes dos acionadores de alerta. Por exemplo, se usar o período de "nos últimos 5 minutos" e o alerta procura uma CPU superior a 80%, o alerta for acionado quando a CPU foi consistentemente acima 80% durante 5 minutos. Depois do primeiro acionador ocorre, ele é acionada novamente quando a CPU permanece inferior a 80% durante 5 minutos. A medida da métrica de CPU ocorre a cada minuto.

6. Selecione **os proprietários de E-Mail...**  se pretender que os administradores e coadministradores para receber notificações por e-mail quando o alerta é acionado.

7. Se pretender enviar notificações para os endereços de e-mail adicionais quando o alerta é acionado, adicione-os na **adicionais do administrador email(s)** campo. Separe várias mensagens de correio eletrónico com ponto e vírgula, no seguinte formato:  *email@contoso.com;email2@contoso.com*

8. Colocar num URI válido no **Webhook** campo se desejar que ela seja chamada quando o alerta é acionado.

9. Se utilizar a automatização do Azure, pode selecionar um runbook a ser executada quando o alerta é acionado.

10. Selecione **OK** para criar o alerta.

Em poucos minutos, o alerta está ativo e aciona conforme descrito anteriormente.

Depois de criar um alerta, pode selecioná-lo e realize uma das seguintes tarefas:

* Ver um gráfico que mostra o limiar de métrica e os valores reais do dia anterior.
* Editar ou eliminá-lo.
* **Desativar** ou **ativar** -los, se deseja temporariamente parar ou retomar a receção de notificações para esse alerta.

## <a name="with-azure-cli"></a>Com a CLI do Azure

As secções anteriores descreveram como criar, ver e gerir regras de alerta de métrica com portal do Azure. Esta secção descreve como fazer o mesmo com várias plataformas [CLI do Azure](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest). CLI do Azure mais rápida maneira de começar a utilizar é através de [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview?view=azure-cli-latest).

### <a name="get-all-classic-metric-alert-rules-in-a-resource-group"></a>Obter todas as regras de alerta métrica clássicas num grupo de recursos

```azurecli
az monitor alert list --resource-group <group name>
```

### <a name="see-details-of-a-particular-classic-metric-alert-rule"></a>Ver detalhes de uma determinada regra de alerta métrica clássica

```azurecli
az monitor alert show --resource-group <group name> --name <alert name>
```

### <a name="create-a-classic-metric-alert-rule"></a>Criar uma regra de alerta de métrica clássica

```azurecli
az monitor alert create --name <alert name> --resource-group <group name> \
    --action email <email1 email2 ...> \
    --action webhook <URI> \
    --target <target object ID> \
    --condition "<METRIC> {>,>=,<,<=} <THRESHOLD> {avg,min,max,total,last} ##h##m##s"
```

### <a name="delete-a-classic-metric-alert-rule"></a>Eliminar uma regra de alerta de métrica clássica

```azurecli
az monitor alert delete --name <alert name> --resource-group <group name>
```

## <a name="with-powershell"></a>Com o PowerShell

Estas secções mostram como utilizar o PowerShell comandos criar, ver e gerir alertas de métricas clássicas. Os exemplos neste artigo mostram como pode utilizar os cmdlets do Azure Monitor para alertas de métricas clássicas.

1. Se ainda não o fez, configure o PowerShell para executar no seu computador. Para obter mais informações, consulte [como instalar e configurar o PowerShell](/powershell/azure/overview). Também pode rever a lista completa de cmdlets do PowerShell de Monitor do Azure no [Cmdlets do Azure Monitor (informações)](https://docs.microsoft.com/powershell/module/azurerm.insights).

2. Inicie sessão em primeiro lugar, na sua subscrição do Azure.

    ```PowerShell
    Connect-AzureRmAccount
    ```

3. Verá um ecrã de início de sessão. Uma vez que iniciar sessão na sua conta, TenantID, e o padrão de ID de subscrição são apresentados. Todos os cmdlets do Azure funciona no contexto de sua assinatura padrão. Para ver a lista de subscrições que tem acesso, utilize o seguinte comando:

    ```PowerShell
    Get-AzureRmSubscription
    ```

4. Para alterar o contexto de trabalho para uma subscrição diferente, utilize o seguinte comando:

    ```PowerShell
    Set-AzureRmContext -SubscriptionId <subscriptionid>
    ```

5. Pode recuperar todos os clássicas métrica regras de alerta num grupo de recursos:

    ```PowerShell
    Get-AzureRmAlertRule -ResourceGroup montest
    ```

6. Pode ver os detalhes de uma regra de alerta de métrica clássica

    ```PowerShell
    Get-AzureRmAlertRule -Name simpletestCPU -ResourceGroup montest -DetailedOutput
    ```

7. Pode recuperar todas as regras de alerta definido para um recurso de destino. Por exemplo, todas as regras de alerta definido numa VM.

    ```PowerShell
    Get-AzureRmAlertRule -ResourceGroup montest -TargetResourceId /subscriptions/s1/resourceGroups/montest/providers/Microsoft.Compute/virtualMachines/testconfig
    ```

8. Pode utilizar o `Add-AlertRule` cmdlet para criar, atualizar ou desativar uma regra de alerta. Pode criar propriedades de e-mail e webhook usando `New-AzureRmAlertRuleEmail` e `New-AzureRmAlertRuleWebhook`, respectivamente. O cmdlet de regra de alerta, atribuir essas propriedades como ações para o **ações** propriedade de regra de alerta. A tabela seguinte descreve os parâmetros e valores utilizados para criar um alerta através de uma métrica.

    | parâmetro | valor |
    | --- | --- |
    | Nome |simpletestdiskwrite |
    | Localização desta regra de alerta |EUA Leste |
    | ResourceGroup |montest |
    | TargetResourceId |/subscriptions/s1/resourceGroups/montest/providers/Microsoft.Compute/virtualMachines/testconfig |
    | MetricName do alerta que é criado |\Disk \PhysicalDisk ( total) / seg. Consulte o `Get-MetricDefinitions` cmdlet sobre como obter os nomes de métricos exatos |
    | Operador |GreaterThan |
    | Valor de limiar (contagem por segundo para esta métrica) |1 |
    | WindowSize (formato hh: mm:) |00:05:00 |
    | agregador (estatística da métrica, que usa a contagem média, neste caso) |Média |
    | e-mails personalizados (matriz de cadeia de caracteres) |'foo@example.com','bar@example.com' |
    | enviar um e-mail para proprietários, contribuidores e leitores |-SendToServiceOwners |

9. Criar uma ação de E-Mail

    ```PowerShell
    $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
    ```

10. Criar uma ação de Webhook

    ```PowerShell
    $actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://example.com?token=mytoken
    ```

## <a name="next-steps"></a>Passos Seguintes

- [Criar um alerta de métrica clássico com um modelo do Resource Manager](monitoring-enable-alerts-using-template.md).
- [Ter um alerta de métrica clássico notificar um sistema não pertencente ao Azure através de um webhook](insights-webhooks-alerts.md).

