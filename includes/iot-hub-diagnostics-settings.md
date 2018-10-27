---
title: incluir ficheiro
description: incluir ficheiro
services: iot-hub
author: dominicbetts
ms.service: iot-hub
ms.topic: include
ms.date: 05/17/2018
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: 0039c4eff3571a96cba1ab36136e0a588d78eb75
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/26/2018
ms.locfileid: "50159133"
---
### <a name="enable-logging-with-diagnostics-settings"></a>Ativar o registo com as definições de diagnóstico

1. Inicie sessão para o [portal do Azure](https://portal.azure.com) e navegue até ao seu IoT Hub.

2. Selecione **as definições de diagnóstico**.

3. Selecione **ativar os diagnósticos**.

   ![Ativar os diagnósticos](./media/iot-hub-diagnostics-settings/turnondiagnostics.png)

4. Dê um nome às definições de diagnóstico.

5. Escolha onde pretende enviar os registos. Pode selecionar qualquer combinação das três opções:

   * Arquivar numa conta de armazenamento
   * Transmitir em fluxo para um hub de eventos
   * Enviar para o Log Analytics

6. Escolher as operações que pretende monitorizar e ativar os registos para essas operações. As operações que podem reportar as definições de diagnóstico são:

   * Ligações
   * Telemetria do dispositivo
   * Mensagens da cloud para dispositivo
   * Operações de identidade de dispositivo
   * Carrega o ficheiro
   * Encaminhamento de mensagens
   * Operações de cloud para o dispositivo duplo
   * Operações de gémeos de dispositivo para a cloud
   * Operações de duplo
   * Operações de tarefa
   * Métodos diretos  

6. Guarde as definições de novo. 

Se pretender ativar as definições de diagnóstico com o PowerShell, utilize o seguinte código:

```azurepowershell
Connect-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName <subscription that includes your IoT Hub>
Set-AzureRmDiagnosticSetting -ResourceId <your resource Id> -ServiceBusRuleId <your service bus rule Id> -Enabled $true
```

Novas definições entrem em vigor em cerca de 10 minutos. Depois disso, os registos de aparecem no arquivo de destino configurado no **as definições de diagnóstico** painel. Para obter mais informações sobre como configurar diagnósticos, consulte [recolher e consumir dados de registo dos seus recursos do azure](../articles/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md).