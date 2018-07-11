---
title: Utilizar alertas e corrigir problemas de dispositivos na solução de monitorização remota - Azure | Microsoft Docs
description: Este tutorial mostra como Utilizar alertas para identificar e corrigir problemas com dispositivos ligados ao acelerador de soluções de Monitorização Remota.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 06/18/2018
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: 9607705220450b30d2ffaf0f2be9fa2a5664b879
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/28/2018
ms.locfileid: "37081793"
---
# <a name="troubleshoot-and-remediate-device-issues"></a>Resolver e remediar problemas nos dispositivos

Neste tutorial, vai utilizar o acelerador de soluções de Monitorização Remota para identificar e corrigir problemas com os seus dispositivos IoT ligados. Utilize alertas no dashboard do acelerador de soluções para identificar problemas e, em seguida, executar tarefas remotas para corrigir esses problemas.

A Contoso está a testar um novo dispositivo **Protótipo** no campo. Enquanto operador da Contoso, observe durante os testes que o dispositivo **Protótipo** está a acionar um alerta de temperatura inesperadamente no dashboard. Agora deve investigar o comportamento deste dispositivo **Protótipo** com falhas e resolver o problema.

Neste tutorial:

>[!div class="checklist"]
> * Investigar um alerta de um dispositivo
> * Resolver o problema do dispositivo

## <a name="prerequisites"></a>Pré-requisitos

Para seguir este tutorial, precisa de uma instância implementada do acelerador de soluções de Monitorização remota na sua subscrição do Azure.

Se ainda não tiver implementado o acelerador de soluções de Monitorização Remota, deverá concluir o início rápido [Implementar uma solução de monitorização remota baseada na cloud](quickstart-remote-monitoring-deploy.md).

## <a name="investigate-an-alert"></a>Investigar um alerta

Na página **Dashboard**, observe que existem alertas de temperatura inesperados provenientes da regra associada aos dispositivos **Protótipo**:

[![Alertas apresentados no dashboard](./media/iot-accelerators-remote-monitoring-maintain/dashboardalarm-inline.png)](./media/iot-accelerators-remote-monitoring-maintain/dashboardalarm-expanded.png#lightbox)

Para investigar melhor o problema, escolha a opção **Explorar Alerta** junto ao alerta:

[![Explorar alerta do dashboard](./media/iot-accelerators-remote-monitoring-maintain/dashboardexplorealarm-inline.png)](./media/iot-accelerators-remote-monitoring-maintain/dashboardexplorealarm-expanded.png#lightbox)

A vista de detalhe do alerta mostra:

* Quando o alerta foi acionado
* Informações de estado sobre os dispositivos associados ao alerta
* Telemetria dos dispositivos associados ao alerta

[![Detalhes do alerta](./media/iot-accelerators-remote-monitoring-maintain/maintenancealarmdetail-inline.png)](./media/iot-accelerators-remote-monitoring-maintain/maintenancealarmdetail-expanded.png#lightbox)

Para reconhecer o alerta, selecione todas as **Ocorrências de alerta** e escolha **Reconhecer**. Esta ação permite que outros operadores saibam que viu o alerta e está a tentar resolver o problema:

[![Reconhecer os alertas](./media/iot-accelerators-remote-monitoring-maintain/maintenanceacknowledge-inline.png)](./media/iot-accelerators-remote-monitoring-maintain/maintenanceacknowledge-expanded.png#lightbox)

Ao reconhecer o alerta, o estado da ocorrência é alterado para **Reconhecido**.

Na lista, pode ver o dispositivo **Protótipo** responsável por acionar o alerta de temperatura do dispositivo:

[![Listar os dispositivos que causam o alerta](./media/iot-accelerators-remote-monitoring-maintain/maintenanceresponsibledevice-inline.png)](./media/iot-accelerators-remote-monitoring-maintain/maintenanceresponsibledevice-expanded.png#lightbox)

## <a name="resolve-the-issue"></a>Resolver o problema

Para resolver o problema com o dispositivo **Protótipo**, terá de chamar o método **DecreaseTemperature** no dispositivo.

Para tomar decisões sobre um dispositivo, selecione-o na lista de dispositivos e, em seguida, escolha **Tarefas**. O modelo do dispositivo **Protótipo** especifica seis métodos que um dispositivo tem de suportar:

[![Ver os métodos que o dispositivo suporta](./media/iot-accelerators-remote-monitoring-maintain/maintenancemethods-inline.png)](./media/iot-accelerators-remote-monitoring-maintain/maintenancemethods-expanded.png#lightbox)

Escolha **DecreaseTemperature** e defina o nome da tarefa como **DecreaseTemperature**. Em seguida, escolha **Aplicar**:

[![Criar a tarefa para diminuir a temperatura](./media/iot-accelerators-remote-monitoring-maintain/maintenancecreatejob-inline.png)](./media/iot-accelerators-remote-monitoring-maintain/maintenancecreatejob-expanded.png#lightbox)

Para controlar o estado da tarefa, clique em **Ver estado da tarefa**. Utilize a vista **Tarefas** para controlar todas as tarefas e chamadas de método na solução:

[![Monitorizar a tarefa para diminuir a temperatura](./media/iot-accelerators-remote-monitoring-maintain/maintenancerunningjob-inline.png)](./media/iot-accelerators-remote-monitoring-maintain/maintenancerunningjob-expanded.png#lightbox)

Pode verificar que a temperatura do dispositivo diminuiu ao ver a telemetria na página **Dashboard**:

[![Ver a diminuição na temperatura](./media/iot-accelerators-remote-monitoring-maintain/jobresult-inline.png)](./media/iot-accelerators-remote-monitoring-maintain/jobresult-expanded.png#lightbox)

## <a name="next-steps"></a>Passos seguintes

Este tutorial mostrou-lhe como utilizar os alertas para identificar problemas com os seus dispositivos e como agir sobre estes dispositivos para resolver os problemas. Para saber como ligar um dispositivo físico ao acelerador de soluções, avance para os artigos de procedimentos.

Agora que aprendeu a gerir problemas de dispositivos, o próximo passo sugerido é saber como [Ligar o seu dispositivo ao acelerador de soluções de Monitorização Remota](iot-accelerators-connecting-devices.md).
