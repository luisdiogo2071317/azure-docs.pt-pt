---
title: O registo de atividade do Azure para os Hubs de eventos de fluxo | Microsoft Docs
description: Saiba como transmitir o registo de atividade para os Event Hubs do Azure.
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: ec4c2d2c-8907-484f-a910-712403a06829
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/30/2018
ms.author: johnkem
ms.openlocfilehash: c3c7ffe00263b8f76d89aa8d15fe2d502538527d
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/01/2018
---
# <a name="stream-the-azure-activity-log-to-event-hubs"></a>Fluxo de registo de atividade do Azure para os Event Hubs
O [ **registo de atividade do Azure** ](monitoring-overview-activity-logs.md) pode transmissão em fluxo em tempo real para qualquer aplicação utilizando a opção "Exportação" incorporada no portal ou ativar o Id de regra de barramento de serviço num perfil de registo através de Cmdlets do PowerShell do Azure ou a CLI do Azure.

## <a name="what-you-can-do-with-the-activity-log-and-event-hubs"></a>O que pode fazer com o registo de atividade e Event Hubs
Seguem-se apenas algumas formas poderá utilizar a capacidade de transmissão em fluxo para o registo de atividade:

* **Fluxo para sistemas de registo e telemetria de terceiros** – ao longo do tempo, os Event Hubs de transmissão em fluxo irá tornar-se o mecanismo para encaminhar o registo de atividade para SIEMs de terceiros e soluções de análise de registo.
* **Criar uma plataforma de registo e telemetria personalizada** – se já tiver uma plataforma de telemetria personalizada ou são apenas de pensar sobre como criar um, altamente dimensionável de publicação-subscrição natureza dos Event Hubs permite-lhe de forma flexível ingerir o registo de atividade. [Consulte o guia de Dan Rosanova para utilizar os Hubs de eventos no aqui uma plataforma de telemetria de escala global.](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/)

## <a name="enable-streaming-of-the-activity-log"></a>Ative a transmissão em fluxo de registo de atividade
Pode ativar a transmissão em fluxo de registo de atividade através de programação ou através do portal. Qualquer forma, escolha um espaço de nomes de barramento de serviço e uma política de acesso partilhado para esse espaço de nomes e um Hub de eventos é criado nesse espaço de nomes quando ocorre o primeiro evento de registo de atividade de novo. Se não tiver um espaço de nomes de barramento de serviço, terá primeiro de criar um. Se anteriormente tiver transmissão em fluxo eventos de registo de atividade para este espaço de nomes de barramento de serviço, será reutilizado o Hub de eventos foi criado anteriormente. A política de acesso partilhado define as permissões que tenha o mecanismo de transmissão em fluxo. Hoje em dia, necessita de transmissão em fluxo para um Event Hubs **gerir**, **enviar**, e **escutar** permissões. Pode criar ou modificar as políticas de acesso de espaço de nomes do barramento de serviço partilhado no portal do Azure, no separador "Configurar" para o espaço de nomes de barramento de serviço. Para atualizar o perfil de registo de registo de atividade para incluir a transmissão em fluxo, o utilizador efetuar a alteração tem de ter a permissão de ListKey nessa regra de autorização de barramento de serviço.

O service bus ou event hub espaço de nomes não tem de estar na mesma subscrição que a subscrição emitir os registos, desde que o utilizador que configura a definição possui acesso RBAC adequado para ambas as subscrições.

### <a name="via-azure-portal"></a>Através do portal do Azure
1. Navegue para o **registo de atividade** painel através da opção os todos os serviços de pesquisa no lado esquerdo do portal.
   
    ![Navegue até ao registo de atividade no portal](./media/monitoring-stream-activity-logs-event-hubs/activity.png)
2. Clique em de **exportar** botão na parte superior do painel de registo de atividade.
   
    ![Botão de exportação no portal](./media/monitoring-stream-activity-logs-event-hubs/export.png)
3. No painel que aparece, pode selecionar as regiões para as quais gostaria de eventos de fluxo e o espaço de nomes de barramento de serviço, na qual gostaria de um Hub de eventos seja criado para estes eventos de transmissão em fluxo. Selecione **todas as regiões**.
   
    ![Painel de registo de atividade de exportação](./media/monitoring-stream-activity-logs-event-hubs/export-audit.png)
4. Clique em **guardar** para guardar estas definições. As definições são imediatamente aplicadas à sua subscrição.
5. Se tiver várias subscrições, deve repetir esta ação e enviar todos os dados para o mesmo hub de eventos.

### <a name="via-powershell-cmdlets"></a>Através de Cmdlets do PowerShell
Se já existir um perfil de registo, terá primeiro de remover esse perfil.

1. Utilize `Get-AzureRmLogProfile` para identificar se existir um perfil de registo
2. Se Sim, utilizar `Remove-AzureRmLogProfile` removê-lo.
3. Utilize `Set-AzureRmLogProfile` para criar um perfil:

```powershell

Add-AzureRmLogProfile -Name my_log_profile -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Locations global,westus,eastus -RetentionInDays 90 -Categories Write,Delete,Action

```

O ID de regra de barramento de serviço é uma cadeia com este formato: {ID de recurso de barramento de serviço} /authorizationrules/ {nome da chave}, por exemplo 

### <a name="via-azure-cli"></a>Através da CLI do Azure
Se já existir um perfil de registo, terá primeiro de remover esse perfil.

1. Utilize `azure insights logprofile list` para identificar se existir um perfil de registo
2. Se Sim, utilizar `azure insights logprofile delete` removê-lo.
3. Utilize `azure insights logprofile add` para criar um perfil:

```azurecli-interactive
azure insights logprofile add --name my_log_profile --storageId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/my_storage --serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey --locations global,westus,eastus,northeurope --retentionInDays 90 –categories Write,Delete,Action
```

O ID de regra de barramento de serviço é uma cadeia com este formato: `{service bus resource ID}/authorizationrules/{key name}`.

## <a name="how-do-i-consume-the-log-data-from-event-hubs"></a>Como posso consumir os dados de registo dos Event Hubs?
[O esquema para o registo de atividades está disponível aqui](monitoring-overview-activity-logs.md). Cada evento está a ser uma matriz de blobs JSON chamado "registos".

## <a name="next-steps"></a>Próximos Passos
* [O registo de atividade para uma conta de armazenamento de arquivo](monitoring-archive-activity-log.md)
* [Leia a descrição geral do registo de atividade do Azure](monitoring-overview-activity-logs.md)
* [Configurar um alerta com base em eventos de um registo de atividade](insights-auditlog-to-webhook-email.md)

