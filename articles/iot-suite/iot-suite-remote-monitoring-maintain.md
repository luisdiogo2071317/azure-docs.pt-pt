---
title: "Resolver problemas de dispositivos na solução de monitorização remota - Azure | Microsoft Docs"
description: "Este tutorial mostra como resolver problemas e remediar problemas de dispositivos na solução de monitorização remota."
services: 
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 02/22/2018
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: dd01246075a5c0db0ed49133ed51fb56d8fcf8e5
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/27/2018
---
# <a name="troubleshoot-and-remediate-device-issues"></a>Resolver problemas e resolver problemas do dispositivo

Este tutorial mostra como utilizar o **manutenção** página na solução para resolver problemas e resolver problemas do dispositivo. Para apresentar estas capacidades, o tutorial utiliza um cenário na aplicação Contoso IoT.

Contoso consiste em testar uma nova **protótipo** dispositivo no campo. Como um operador de Contoso, repare durante os testes que o **protótipo** dispositivo inesperadamente é acionar um alarme temperatura no dashboard. Agora tem investigue o comportamento neste defeituoso **protótipo** dispositivo.

Neste tutorial, ficará a saber como:

>[!div class="checklist"]
> * Utilize o **manutenção** página para investigar o alarme
> * Chamar um método de dispositivo para corrigir o problema

## <a name="prerequisites"></a>Pré-requisitos

Para seguir este tutorial, precisa de uma instância implementada da solução de monitorização remota na sua subscrição do Azure.

Se ainda não implementado a solução de monitorização remota ainda, deve efetuar o [implementar a solução pré-configurada de monitorização remota](iot-suite-remote-monitoring-deploy.md) tutorial.

## <a name="use-the-maintenance-dashboard"></a>Utilize o dashboard de manutenção

No **Dashboard** página reparar que existem alarmes temperatura inesperado proveniente da regra associada a **protótipo** dispositivos:

![Alarmes Mostrar no dashboard](media/iot-suite-remote-monitoring-maintain/dashboardalarm.png)

Para investigar o problema, escolha o **explorar alarme** opção junto de alarme:

![Explorar alarme a partir do dashboard](media/iot-suite-remote-monitoring-maintain/dashboardexplorealarm.png)

A vista de detalhes do alarme mostra:

* Quando o alarme foi acionada
* Informações de estado sobre os dispositivos associados a alarme
* Telemetria de dispositivos associados a alarme

![Detalhes de alarme](media/iot-suite-remote-monitoring-maintain/maintenancealarmdetail.png)

Para confirmar o alarme, selecione o **alarme ocorrências** e escolha **Acknowledge**. Esta ação permite outros operadores que tem visto a alarme e estiver a trabalhar no mesmo.

![Confirmar os alarmes](media/iot-suite-remote-monitoring-maintain/maintenanceacknowledge.png)

Quando confirmar o alarme, o estado da ocorrência mudar para **confirmado**.

Na lista, pode ver o **protótipo** responsável pela que despoletou o alarme temperatura do dispositivo:

![Lista os dispositivos a causar o alarme](media/iot-suite-remote-monitoring-maintain/maintenanceresponsibledevice.png)

## <a name="remediate-the-issue"></a>Corrigir o problema

Para corrigir o problema com o **protótipo** dispositivo, tem de chamar o **DecreaseTemperature** método no dispositivo.

Para funcionar num dispositivo, selecione-o na lista de dispositivos e, em seguida, escolha **agenda**. O **protótipo** o modelo de dispositivo especifica quatro métodos de um dispositivo tem de suportar:

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
> * Utilize o **manutenção** página para investigar o alarme
> * Chamar um método de dispositivo para corrigir o problema

Agora que aprendeu como gerir problemas de dispositivos, o passo seguinte sugerido é saber como [testar a sua solução com dispositivos simulados](iot-suite-remote-monitoring-test.md).

<!-- Next tutorials in the sequence -->