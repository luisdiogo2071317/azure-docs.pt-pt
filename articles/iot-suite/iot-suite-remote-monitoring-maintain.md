---
title: Resolver problemas de dispositivos na solução de monitorização remota - Azure | Microsoft Docs
description: Este tutorial mostra como resolver problemas e remediar problemas de dispositivos na solução de monitorização remota.
services: iot-suite
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 05/01/2018
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: 99cf9e341edf0acee434e2d01f6346291cbcea5a
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/17/2018
---
# <a name="troubleshoot-and-remediate-device-issues"></a>Resolver problemas e resolver problemas do dispositivo

Este tutorial mostra como utilizar o **manutenção** página na solução para resolver problemas e resolver problemas do dispositivo. Para apresentar estas capacidades, o tutorial utiliza um cenário na aplicação Contoso IoT.

Contoso consiste em testar uma nova **protótipo** dispositivo no campo. Como um operador de Contoso, repare durante os testes que o **protótipo** dispositivo inesperadamente é acionar um alerta de temperatura no dashboard. Agora tem investigue o comportamento neste defeituoso **protótipo** dispositivo.

Neste tutorial, ficará a saber como:

>[!div class="checklist"]
> * Utilize o **manutenção** página para investigar o alerta
> * Chamar um método de dispositivo para corrigir o problema

## <a name="prerequisites"></a>Pré-requisitos

Para seguir este tutorial, precisa de uma instância implementada da solução de monitorização remota na sua subscrição do Azure.

Se ainda não implementado a solução de monitorização remota ainda, deve efetuar o [implementar o acelerador de solução de monitorização remota](../iot-accelerators/iot-accelerators-remote-monitoring-deploy.md) tutorial.

## <a name="use-the-maintenance-dashboard"></a>Utilize o dashboard de manutenção

No **Dashboard** página reparar que existem alertas de temperatura inesperado proveniente da regra associada a **protótipo** dispositivos:

![A mostrar no dashboard de alertas](media/iot-suite-remote-monitoring-maintain/dashboardalarm.png)

Para investigar o problema, escolha o **explorar alerta** opção junto ao alerta do:

![Explore o alerta a partir do dashboard](media/iot-suite-remote-monitoring-maintain/dashboardexplorealarm.png)

A vista de detalhes do alerta mostra:

* Quando o alerta foi acionado
* Informações de estado sobre os dispositivos associados ao alerta
* Telemetria dos dispositivos associados ao alerta

![Detalhes do alerta](media/iot-suite-remote-monitoring-maintain/maintenancealarmdetail.png)

Para confirmar o alerta, selecione o **alerta ocorrências** e escolha **Acknowledge**. Esta ação permite outros operadores que tem visto o alerta e estiver a trabalhar no mesmo.

![Confirmar os alertas](media/iot-suite-remote-monitoring-maintain/maintenanceacknowledge.png)

Quando confirmar o alerta, o estado da ocorrência mudar para **confirmado**.

Na lista, pode ver o **protótipo** responsável pela que despoletou o alerta de temperatura do dispositivo:

![Lista os dispositivos a causar o alerta](media/iot-suite-remote-monitoring-maintain/maintenanceresponsibledevice.png)

## <a name="remediate-the-issue"></a>Corrigir o problema

Para corrigir o problema com o **protótipo** dispositivo, tem de chamar o **DecreaseTemperature** método no dispositivo.

Para funcionar num dispositivo, selecione-o na lista de dispositivos e, em seguida, escolha **tarefas**. O **protótipo** o modelo de dispositivo Especifica seis métodos de um dispositivo tem de suportar:

![Ver os métodos do que dispositivo suporta](media/iot-suite-remote-monitoring-maintain/maintenancemethods.png)

Escolha **DecreaseTemperature** e defina o nome da tarefa para **DecreaseTemperature**. Em seguida, escolha **aplicar**:

![Criar a tarefa para diminuir a temperatura](media/iot-suite-remote-monitoring-maintain/maintenancecreatejob.png)

Para controlar o estado da tarefa no **manutenção** página, escolha **tarefas**. Utilize o **tarefas** ver para controlar todas as tarefas e chama o método na solução:

![Monitorizar a tarefa para diminuir a temperatura](media/iot-suite-remote-monitoring-maintain/maintenancerunningjob.png)

Para ver os detalhes de uma tarefa específica ou chamada de método, escolha a mesma na lista no **tarefas** vista:

![Ver detalhes da tarefa](media/iot-suite-remote-monitoring-maintain/maintenancejobdetail.png)

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, vimos como:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Utilize o **manutenção** página para investigar o alerta
> * Chamar um método de dispositivo para corrigir o problema

Agora que aprendeu como gerir problemas de dispositivos, o passo seguinte sugerido é saber como [testar a sua solução com dispositivos simulados](iot-suite-remote-monitoring-test.md).

<!-- Next tutorials in the sequence -->