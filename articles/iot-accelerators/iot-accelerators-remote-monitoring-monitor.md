---
title: Avançadas de monitorização na solução de monitorização remota - Azure | Microsoft Docs
description: Este tutorial mostra como monitorizar dispositivos com o dashboard de solução de monitorização remota.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 02/22/2018
ms.topic: conceptual
ms.openlocfilehash: 4d2dabd348d7fda4fa7ca3aac9975fd4179400c5
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34627404"
---
# <a name="perform-advanced-monitoring-using-the-remote-monitoring-solution"></a>Efetuar a monitorização avançada com a solução de monitorização remota

Este tutorial mostra as capacidades do dashboard de monitorização remota. Para apresentar estas capacidades, o tutorial utiliza um cenário na aplicação Contoso IoT.

Neste tutorial, utilize dois dispositivos de camião Contoso simulados para saber como monitorizar os seus dispositivos a partir do dashboard de acelerador solução. Como um operador de Contoso, terá de monitorizar a localização e o comportamento dos seus trucks no campo.

Neste tutorial, ficará a saber como:

>[!div class="checklist"]
> * Filtrar os dispositivos no dashboard
> * Ver telemetria em tempo real
> * Ver detalhes do dispositivo
> * Ver alertas dos seus dispositivos
> * Ver o sistema KPIs

## <a name="prerequisites"></a>Pré-requisitos

Para seguir este tutorial, precisa de uma instância implementada a solução de monitorização remota na sua subscrição do Azure.

Se ainda não implementado a solução de monitorização remota ainda, deve efetuar o [implementar o acelerador de solução de monitorização remota](iot-accelerators-remote-monitoring-deploy.md) tutorial.

## <a name="choose-the-devices-to-display"></a>Escolha os dispositivos para apresentar

Para selecionar os dispositivos que apresentam no **Dashboard** página, utilize filtros. Para apresentar apenas o **camião** dispositivos, escolha o incorporado **Trucks** filtro na lista pendente de filtro:

![Filtrar por trucks no dashboard](./media/iot-accelerators-remote-monitoring-monitor/dashboardtruckfilter.png)

Ao aplicar um filtro, apenas os dispositivos que correspondam às condições filtro apresentam no mapa no **Dashboard** página:

![Apresentar trucks no mapa](./media/iot-accelerators-remote-monitoring-monitor/dashboardtruckmap.png)

O filtro também determina quais os dispositivos que vê no **telemetria** gráfico:

![Apresenta a telemetria camião no dashboard](./media/iot-accelerators-remote-monitoring-monitor/dashboardtelemetry.png)

Para criar, editar e eliminar os filtros, escolha **gerir filtros**.

## <a name="view-real-time-telemetry"></a>Ver telemetria em tempo real

O acelerador solução rastreia dados de telemetria em tempo real de detalhado no gráfico no **Dashboard** página. O gráfico de telemetria mostra informações de telemetria para os dispositivos selecionados pelo filtro de atual:

![Desenho de telemetria camião](./media/iot-accelerators-remote-monitoring-monitor/dashboardtelemetryview.png)

Para selecionar os valores de telemetria para ver, escolha o tipo de telemetria na parte superior do gráfico:

![Desenho de telemetria camião](./media/iot-accelerators-remote-monitoring-monitor/dashboardselecttelemetry.png)

<!-- 05/01 - this features appears to have been removed
To pause the live telemetry display, choose **Flowing**. To re-enable the live display, choose **Pause**:

![Pause and restart telemetry display](./media/iot-accelerators-remote-monitoring-monitor/dashboardtelemetrypause.png)-->

## <a name="use-the-map"></a>Utilize o mapa

O mapa apresenta informações sobre os trucks simuladas selecionado pelo filtro de atual. Pode ampliem e desloquem o mapa para visualizar localizações detalhadamente mais ou menos. Os ícones de dispositivo no mapa indicam qualquer **alertas** ou **avisos** que estão ativos para o dispositivo. Um resumo do número de **alertas** e **avisos** apresenta à esquerda do mapa.

<!-- 05/01 - cannot select a deice on the map
To view the device details, pan and zoom the map to locate the devices, then click the device on the map. The details include:

* Recent telemetry values
* Methods the device supports
* Device properties

![View device details on the dashboard](./media/iot-accelerators-remote-monitoring-monitor/dashboarddevicedetail.png)-->

## <a name="view-alerts-from-your-devices"></a>Ver alertas dos seus dispositivos

O mapa realça os dispositivos no filtro atual com **alertas** e **avisos**. O **alertas** painel mostra informações detalhadas sobre os alertas mais recentes dos seus dispositivos:

![Ver alertas do sistema no dashboard](./media/iot-accelerators-remote-monitoring-monitor/dashboardsystemalarms.png)

Pode utilizar o **Dashboard** filtro para ajustar o intervalo de tempo para os alertas recentes. Por predefinição, o painel apresenta os alertas de última hora:

![Filtrar os alertas por hora](./media/iot-accelerators-remote-monitoring-monitor/dashboardalarmsfilter.png)

## <a name="view-the-system-kpis"></a>Ver o sistema KPIs

O **Dashboard** página apresenta os KPIs de sistema:

![KPIs de dashboard](./media/iot-accelerators-remote-monitoring-monitor/dashboardkpis.png)

Pode utilizar o **Dashboard** filtro para ajustar o intervalo de tempo para a agregação de KPI. Por predefinição, o painel apresenta KPIs agregados ao longo de última hora.

## <a name="next-steps"></a>Passos Seguintes

Este tutorial mostrou como utilizar o **Dashboard** página para filtrar e monitorizar os trucks simuladas aprovisionados na sua solução de monitorização remota:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Filtrar os dispositivos no dashboard
> * Ver telemetria em tempo real
> * Ver detalhes do dispositivo
> * Ver alertas dos seus dispositivos
> * Ver o sistema KPIs

Agora que aprendeu como monitorizar os seus dispositivos, os passos sugeridos são saber como:

* [Detetar problemas através de regras baseadas em limiares](iot-accelerators-remote-monitoring-automate.md).
* [Gerir e configurar os seus dispositivos](iot-accelerators-remote-monitoring-manage.md).
* [Resolver problemas e resolver problemas de dispositivo](iot-accelerators-remote-monitoring-maintain.md).
* [Testar a sua solução com dispositivos simulados](iot-accelerators-remote-monitoring-test.md).

<!-- Next tutorials in the sequence -->