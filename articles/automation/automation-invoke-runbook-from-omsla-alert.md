---
title: "Invocar um runbook da Automatização do Azure a partir de um Alerta do Log Analytics | Microsoft Docs"
description: "Este artigo disponibiliza uma descrição geral de como invocar um runbook da Automatização a partir de um alerta do Log Analytics no Operations Management Suite."
services: automation
documentationcenter: 
author: georgewallace
manager: jwhit
editor: 
ms.assetid: 
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2017
ms.author: magoedte
ms.openlocfilehash: a10be867965eef9746a0f4cc9b14c4fc429f6e35
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/10/2018
---
# <a name="call-an-azure-automation-runbook-from-a-log-analytics-alert"></a>Invocar um runbook da Automatização do Azure a partir de um alerta do Log Analytics

Pode configurar um alerta no Azure Log Analytics para criar um registo de alertas quando os resultados correspondem aos critérios. Esse alerta pode executar automaticamente um runbook da Automatização do Azure numa tentativa para retificar automaticamente o problema. 

Por exemplo, um alerta pode indicar um pico prolongado de utilização do processador. Ou pode indicar quando ocorre uma falha de um processo de aplicação crítico para a funcionalidade de uma aplicação de negócio. Um runbook pode, em seguida, escrever um evento correspondente no registo de eventos do Windows.  

Existem duas opções para chamar um runbook na configuração do alerta:

* Utilize um webhook.
   * Esta é a única opção disponível se a sua área de trabalho do Operations Management Suite não estiver associada a uma conta de Automatização.
   * Se já tiver uma conta de Automatização ligada a uma área de trabalho do Operations Management Suite, esta opção continua disponível.  

* Selecionar um runbook diretamente.
   * Esta opção está disponível apenas se a sua área de trabalho do Operations Management Suite estiver associada a uma conta de Automatização.

## <a name="calling-a-runbook-by-using-a-webhook"></a>Chamar um webhook ao utilizar um runbook

Pode utilizar um webhook para iniciar um runbook específico na Automatização do Azure através de um único pedido HTTP. Antes de configurar o [alerta do Log Analytics](../log-analytics/log-analytics-alerts.md#alert-rules) para chamar o runbook ao utilizar um webhook como ação do alerta, tem de [criar um webhook](automation-webhooks.md#creating-a-webhook) para o runbook chamado através deste método. Não se esqueça de registar o URL do webhook para que o possa referenciar enquanto configura a regra do alerta.   

## <a name="calling-a-runbook-directly"></a>Chamar um runbook diretamente

Pode instalar e configurar a oferta de Automatização e Controlo na sua área de trabalho do Operations Management Suite. Enquanto configura a opção das ações do runbook para o alerta, pode ver todos os runbooks na lista pendente **Selecionar runbook** e selecionar o runbook específico que quer executar como resposta ao alerta. O runbook selecionado pode ser executado numa área de trabalho do Azure ou numa função de trabalho de runbook híbrida. 

Após criar o alerta com a opção do runbook, é criado um webhook para o runbook. Pode ver o webhook ao aceder à conta de Automatização e abrir o painel do webhook do runbook selecionado. 

Se eliminar o alerta, o webhook não é eliminado. Não é um problema. O webhook apenas se torna um item órfão que deve, eventualmente, eliminar manualmente para manter uma conta de Automatização organizada.  

## <a name="characteristics-of-a-runbook"></a>Características de um runbook

Ambos os métodos para chamar o runbook a partir do alerta do Log Analytics têm características que tem de perceber antes de configurar as regras do alerta. 

Os dados de alertas estão no formato JSON numa única propriedade chamada **SearchResult**. Este formato é para ações de runbook e webhook com um payload padrão. Para ações de webhook com payloads personalizados (incluindo **IncludeSearchResults:True** no **RequestBody**), a propriedade é **SearchResults**.

Tem de ter um parâmetro de entrada do runbook com o nome **WebhookData** e que do tipo **Object**. Pode ser obrigatório ou opcional. O alerta utiliza este parâmetro de entrada para transmitir os resultados da pesquisa ao runbook.

```powershell
param  
    (  
    [Parameter (Mandatory=$true)]  
    [object] $WebhookData  
    )
```
Também tem de ter código para converter o **WebhookData** num objeto do PowerShell.

```powershell
$SearchResult = (ConvertFrom-Json $WebhookData.RequestBody).SearchResult.value
```

**$SearchResult** é uma matriz de objetos. Cada objeto contém os campos com valores de um resultado de pesquisa.


## <a name="example-walkthrough"></a>Instruções de exemplo

O exemplo de um runbook gráfico seguinte demonstra como este processo funciona. Inicia um serviço do Windows.

![Runbook gráfico para iniciar um serviço do Windows](media/automation-invoke-runbook-from-omsla-alert/automation-runbook-restartservice.png)

O runbook tem um parâmetro de entrada do tipo **Objeto** que é chamado **WebhookData**. Inclui os dados de webhook transmitidos do alerta que contém **SearchResult**.

![Parâmetros de entrada do runbook](media/automation-invoke-runbook-from-omsla-alert/automation-runbook-restartservice-inputparameter.png)

Neste exemplo, criámos dois campos personalizados no Log Analytics: **SvcDisplayName_CF** e **SvcState_CF**. Estes campos extraem o nome a apresentar do serviço e o estado do serviço (que é, em execução ou parado) do evento que é escrito no registo de eventos do sistema. Depois, criámos uma regra de alerta com a consulta de pesquisa, para que possamos detetar quando o serviço Print Spooler é parado no sistema Windows:

`Type=Event SvcDisplayName_CF="Print Spooler" SvcState_CF="stopped"` 

Pode ser qualquer serviço de interesse. Neste exemplo, fazemos referência a um dos serviços pré-existentes que estão incluídos no SO Windows. A ação do alerta está configurada para executar o runbook utilizado neste exemplo e executado na função de trabalho de runbook híbrida, que está ativada no sistema de destino.   

A atividade do código de runbook **Obter nome do serviço de LA** converte a cadeia formatada em JSON para um tipo de objeto e filtra no item **SvcDisplayName_CF**. Extrai o nome a apresentar do serviço Windows e passa este valor para a atividade seguinte, que verifica se o serviço está parado antes de tentar reiniciá-lo. **SvcDisplayName_CF** é um [campo personalizado](../log-analytics/log-analytics-custom-fields.md) que criámos no Log Analytics para demonstrar este exemplo.

```powershell
$SearchResult = (ConvertFrom-Json $WebhookData.RequestBody).SearchResult.value
$SearchResult.SvcDisplayName_CF  
```

Quando o serviço para, a regra do alerta no Log Analytics deteta uma correspondência, aciona o runbook e envia o contexto do alerta para este. O runbook tenta verificar se o serviço está parado. Se estiver, o runbook tenta reiniciar o serviço, verifica se foi iniciado corretamente e apresenta os resultados.     

Em alternativa, se não tiver a conta de Automatização ligada à sua área de trabalho do Operations Management Suite, pode configurar a regra de alerta com uma ação do webhook. A ação do webhook aciona o runbook. Configura também o runbook para converter a cadeia formatada em JSON e filtrar na **SearchResult** ao seguir as orientações mencionadas anteriormente.    

>[!NOTE]
> Se a sua área de trabalho tiver sido atualizada para a [nova linguagem de consulta do Log Analytics](../log-analytics/log-analytics-log-search-upgrade.md), o payload do webook foi alterado. Os detalhes do formato estão na [API REST do Log Analytics do Azure](https://aka.ms/loganalyticsapiresponse).

## <a name="next-steps"></a>Passos Seguintes

* Para saber mais sobre os alertas no Log Analytics e como criá-los, veja [Alerts in Log Analytics (Alertas no Log Analytics)](../log-analytics/log-analytics-alerts.md).

* Para saber como acionar runbooks ao utilizar um webhook, veja [Azure Automation webhooks (Webhooks da Automatização do Azure)](automation-webhooks.md).
