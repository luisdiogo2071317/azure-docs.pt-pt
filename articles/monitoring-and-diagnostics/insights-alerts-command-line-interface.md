---
title: Utilizar a CLI do Azure de várias plataformas para criar alertas clássicas para serviços do Azure | Microsoft Docs
description: Acionar mensagens de correio eletrónico ou notificações ou chame URLs de Web sites (webhooks) ou automatização quando são satisfeitas as condições que especificar.
author: rboucher
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 10/24/2016
ms.author: robb
ms.component: alerts
ms.openlocfilehash: 8112b868bc8d2ca2a9478d38ee702d8b3350d48e
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36287113"
---
# <a name="use-the-cross-platform-azure-cli-to-create-classic-metric-alerts-in-azure-monitor-for-azure-services"></a>Utilizar a CLI do Azure de várias plataformas para criar alertas métricas clássicas no Monitor do Azure para serviços do Azure 

> [!div class="op_single_selector"]
> * [Portal](insights-alerts-portal.md)
> * [PowerShell](insights-alerts-powershell.md)
> * [CLI](insights-alerts-command-line-interface.md)
>
>
> [!NOTE]
> Este artigo descreve como criar alertas de métricas clássicas mais antigas. Azure suporta agora a monitorizar [mais recente, melhor alertas métricas](monitoring-near-real-time-metric-alerts.md). Estes alertas podem monitorizar várias métricas e permitir alertas nas métricas dimensional. Suporte da CLI do Azure para alertas métricas mais recentes está disponível em breve.
>
>

Este artigo mostra como configurar alertas de métricas clássicas do Azure utilizando a interface de linha de comandos de várias plataformas (CLI do Azure).

> [!NOTE]
> Monitor do Azure é o novo nome para o que foi chamado "Informações do Azure" até 25 de Setembro de 2016. No entanto, os espaços de nomes e, consequentemente, os comandos que são descritos aqui ainda contenham a palavra "informações".

Pode receber um alerta com base nas métricas para os serviços do Azure ou com base em eventos que ocorrem no Azure.

* **Valores métricos**: O alerta é acionado quando o valor de uma métrica especificado atravesse um limiar que atribuir em qualquer direção. Ou seja, aciona ambas quando a condição for satisfeita primeiro e, em seguida, quando que já não está a ser condição.    

* **Eventos de registo de atividade**: pode acionar um alerta num *cada* eventos ou quando ocorrem determinados eventos. Para saber mais sobre os registos de atividade, consulte [criar atividade alertas de registo (clássica)](monitoring-activity-log-alerts.md). 

Pode configurar um alerta de métrico clássico para fazer o seguinte quando aciona:

* Envie notificações por e-mail para o administrador de serviços e coadministradores. 
* Envie endereços de correio eletrónico para correio eletrónico que especificar.
* Chame um webhook.
* Inicie a execução de um runbook do Azure (apenas a partir do portal do Azure neste momento).

Pode configurar e obter informações sobre regras de alerta métricas clássicas utilizando o seguinte: 

* [O portal do Azure](insights-alerts-portal.md)
* [PowerShell](insights-alerts-powershell.md)
* [CLI do Azure](insights-alerts-command-line-interface.md)
* [API de REST de Monitor do Azure](https://msdn.microsoft.com/library/azure/dn931945.aspx)

Também pode obter ajuda para comandos escrevendo um comando com **-ajudar** no final. Segue-se um exemplo: 

```console
 azure insights alerts -help
 azure insights alerts actions email create -help
 ```

## <a name="create-alert-rules-by-using-azure-cli"></a>Criar regras de alertas ao utilizar a CLI do Azure
1. Depois de instalar os pré-requisitos, inicie sessão no Azure. Consulte [amostras da CLI do Azure Monitor](insights-cli-samples.md) para os comandos que necessita para começar. Estes comandos ajudam a obter com sessão iniciada, mostram-lhe que subscrição que está a utilizar e prepará-lo para executar comandos de Monitor do Azure.

    ```console
    azure login
    azure account show
    azure config mode arm

    ```

2. Para listar as regras existentes num grupo de recursos, utilize o seguinte formato: 

   **informações do Azure a lista de regras de alertas** *[opções] &lt;resourceGroup&gt;*

   ```console
   azure insights alerts rule list myresourcegroupname

   ```
3. Para criar uma regra, tem de ter várias partes importantes de informações pela primeira vez.
    * O **ID de recurso** para o recurso que pretende definir um alerta para.
    * O **as definições de métrica** que estão disponíveis para esse recurso.

     Uma forma de obter o ID de recurso é utilizar o portal do Azure. Partindo do princípio de que o recurso já está a ser criado, selecione-o no portal. Em seguida, no painel do seguinte, no **definições** secção, selecione **propriedades**. **ID de recurso** é um campo no painel seguinte. 
     
     Também pode obter o ID de recurso utilizando [Explorador de recursos do Azure](https://resources.azure.com/).

     Segue-se um ID de recurso de exemplo para uma aplicação web:

     ```console
     /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename
     ```

     Para obter uma lista de unidades e as métricas disponíveis para as métricas no anterior exemplo de recursos, utilize o seguinte comando da CLI do Azure:  

     ```console
     azure insights metrics list /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename PT1M
     ```

     *PT1M* a granularidade também o a medição disponível (em intervalos de 1 minuto). Tem opções de métricas diferentes ao utilizar granularidades diferentes.
     
4. Para criar uma regra de alerta baseada em métrica, utilize um comando no seguinte formato:

    **as informações do Azure métrico conjunto de regras de alertas** *[opções] &lt;ruleName&gt; &lt;localização&gt; &lt;resourceGroup&gt; &lt;windowSize&gt; &lt;operador&gt; &lt;limiar&gt; &lt;targetResourceId&gt; &lt;metricName&gt; &lt;timeAggregationOperator&gt;*

    O exemplo a seguir configura um alerta no recurso Web site. Os acionadores de alerta sempre que recebe consistentemente qualquer tráfego de 5 minutos e novamente quando não recebe nenhum tráfego de 5 minutos.

    ```console
    azure insights alerts rule metric set myrule eastus myreasourcegroup PT5M GreaterThan 2 /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename BytesReceived Total

    ```
5. Para criar um webhook ou envie um e-mail quando é acionado um alerta de métrico clássico, crie primeiro o e-mail ou o webhook. Em seguida, criar a regra imediatamente posteriormente. Não é possível associar webhooks ou e-mails com regras que já foram criadas.

    ```console
    azure insights alerts actions email create --customEmails myemail@contoso.com

    azure insights alerts actions webhook create https://www.contoso.com

    azure insights alerts rule metric set myrulewithwebhookandemail eastus myreasourcegroup PT5M GreaterThan 2 /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename BytesReceived Total
    ```

6. Pode verificar que os alertas não foi criados corretamente ao observar uma regra de individuais.

    ```console
    azure insights alerts rule list myresourcegroup --ruleName myrule
    ```
7. Para eliminar regras, utilize um comando no seguinte formato:

    **informações de eliminação de regras de alertas** [opções] &lt;resourceGroup&gt; &lt;ruleName&gt;

    Estes comandos, elimine as regras que foram anteriormente criadas neste artigo.

    ```console
    azure insights alerts rule delete myresourcegroup myrule
    azure insights alerts rule delete myresourcegroup myrulewithwebhookandemail
    azure insights alerts rule delete myresourcegroup myActivityLogRule
    ```

## <a name="next-steps"></a>Passos Seguintes
* [Obter uma descrição geral da monitorização do Azure](monitoring-overview.md), incluindo os tipos de informações que pode recolher e monitorizar.
* Saiba mais sobre [configurar webhooks alertas](insights-webhooks-alerts.md).
* Saiba mais sobre [configurar alertas de eventos de registo de atividade](monitoring-activity-log-alerts.md).
* Saiba mais sobre [runbooks de automatização do Azure](../automation/automation-starting-a-runbook.md).
* Obter um [descrição geral de recolha de registos de diagnóstico](monitoring-overview-of-diagnostic-logs.md) para recolher métricas de alta frequência detalhadas para o seu serviço.
* Obter um [descrição geral da coleção de métricas](insights-how-to-customize-monitoring.md) para se certificar de que o serviço está disponível e reativa.
