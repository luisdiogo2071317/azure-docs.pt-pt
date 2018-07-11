---
title: Monitorizar os seus dispositivos IoT a partir de uma solução do Azure | Microsoft Docs
description: Neste tutorial irá aprender a monitorizar os seus dispositivos IoT com o acelerador de soluções de Monitorização Remota.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 06/08/2018
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: 5f42ed0fa5362959e5619f2d550ca1ae3711ed65
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37097466"
---
# <a name="tutorial-monitor-your-iot-devices"></a>Tutorial: Monitorizar os seus dispositivos IoT

Neste tutorial, vai utilizar o acelerador de soluções de Monitorização Remota para monitorizar os seus dispositivos IoT ligados. Utilize o dashboard de soluções para ver telemetria, informações do dispositivo, alertas e KPIs.

Para apresentar estas funcionalidades de monitorização, o tutorial utiliza dois dispositivos de camião simulado. Os camiões são geridos por uma organização chamada Contoso e estão ligados ao acelerador de soluções de Monitorização Remota. Enquanto operador da Contoso, tem de monitorizar a localização e o comportamento dos camiões no campo.

Neste tutorial:

>[!div class="checklist"]
> * Filtrar os dispositivos no dashboard
> * Ver telemetria em tempo real
> * Ver detalhes do dispositivo
> * Ver alertas a partir dos seus dispositivos
> * Ver os KPIs do sistema

## <a name="prerequisites"></a>Pré-requisitos

Para seguir este tutorial, precisa de uma instância implementada do acelerador de soluções de Monitorização remota na sua subscrição do Azure.

Se ainda não tiver implementado o acelerador de soluções de Monitorização Remota, deverá concluir o início rápido [Implementar uma solução de monitorização remota baseada na cloud](quickstart-remote-monitoring-deploy.md).

## <a name="choose-the-devices-to-display"></a>Selecionar os dispositivos a apresentar

Para selecionar os dispositivos ligados que são apresentados na página **Dashboard**, utilize os filtros. Para apresentar apenas os dispositivos de **Camião**, escolha o filtro **Camiões** incorporado na lista pendente de filtros:

[![Filtrar camiões no dashboard](./media/iot-accelerators-remote-monitoring-monitor/dashboardtruckfilter-inline.png)](./media/iot-accelerators-remote-monitoring-monitor/dashboardtruckfilter-expanded.png#lightbox)

Quando aplicar um filtro, escolha apenas os dispositivos que correspondem às condições de filtro apresentadas no mapa na página **Dashboard**:

[![São apresentados apenas os camiões no mapa](./media/iot-accelerators-remote-monitoring-monitor/dashboardtruckmap-inline.png)](./media/iot-accelerators-remote-monitoring-monitor/dashboardtruckmap-expanded.png#lightbox)

O filtro também determina quais os dispositivos que pode ver no gráfico **Telemetria**:

[![A telemetria de camião é apresentada no dashboard](./media/iot-accelerators-remote-monitoring-monitor/dashboardtelemetry-inline.png)](./media/iot-accelerators-remote-monitoring-monitor/dashboardtelemetry-expanded.png#lightbox)

Para criar, editar e eliminar filtros, escolha **Gerir grupos de dispositivos**.

## <a name="view-real-time-telemetry"></a>Ver telemetria em tempo real

O acelerador de soluções rastreia a telemetria em tempo real no gráfico na página **Dashboard**. A parte superior do gráfico de telemetria mostra os tipos de telemetria disponíveis para os dispositivos selecionados pelo filtro atual:

[![Tipos de telemetria de camião](./media/iot-accelerators-remote-monitoring-monitor/dashboardtelemetryview-inline.png)](./media/iot-accelerators-remote-monitoring-monitor/dashboardtelemetryview-expanded.png#lightbox)

Para ver a telemetria da temperatura, clique em **Temperatura**:

[![Gráfico da telemetria de temperatura de camião](./media/iot-accelerators-remote-monitoring-monitor/dashboardselecttelemetry-inline.png)](./media/iot-accelerators-remote-monitoring-monitor/dashboardselecttelemetry-expanded.png#lightbox)

## <a name="use-the-map"></a>Utilizar o mapa

O mapa apresenta informações sobre os camiões simulados selecionados pelo filtro atual. Pode ampliar e fazer um deslocamento panorâmico do mapa para apresentar localizações com mais ou menos detalhe. A cor do ícone de um dispositivo no mapa indica se estão ativos **Alertas** ou **Avisos** para o dispositivo. É apresentado um resumo do número de **Alertas** e **Avisos** à esquerda do mapa.

Para ver os detalhes do dispositivo, faça um deslocamento panorâmico e amplie o mapa para localizar o dispositivo e, em seguida, selecione o dispositivo no mapa. Em seguida, clique na etiqueta do dispositivo para abrir o painel **Detalhes do dispositivo**. Os detalhes do dispositivo incluem:

* Valores de telemetria recentes
* Métodos que o dispositivo suporta
* Propriedades do dispositivo

[![Ver detalhes do dispositivo no dashboard](./media/iot-accelerators-remote-monitoring-monitor/dashboarddevicedetail-inline.png)](./media/iot-accelerators-remote-monitoring-monitor/dashboarddevicedetail-expanded.png#lightbox)

## <a name="view-alerts"></a>Ver alertas

O painel **Alertas** mostra informações detalhadas sobre os alertas mais recentes dos seus dispositivos:

[![Ver alertas do dispositivo no dashboard](./media/iot-accelerators-remote-monitoring-monitor/dashboardsystemalarms-inline.png)](./media/iot-accelerators-remote-monitoring-monitor/dashboardsystemalarms-expanded.png#lightbox)

Pode utilizar um filtro para ajustar o intervalo de tempo para os alertas recentes. Por predefinição, o painel apresenta alertas desde a última hora:

[![Filtrar os alertas por hora](./media/iot-accelerators-remote-monitoring-monitor/dashboardalarmsfilter-inline.png)](./media/iot-accelerators-remote-monitoring-monitor/dashboardalarmsfilter-expanded.png#lightbox)

## <a name="view-the-system-kpis"></a>Ver os KPIs do sistema

A página **Dashboard** apresenta KPIs do sistema calculados pelo acelerador de soluções no painel **Análise**:

[![KPIs do Dashboard](./media/iot-accelerators-remote-monitoring-monitor/dashboardkpis-inline.png)](./media/iot-accelerators-remote-monitoring-monitor/dashboardkpis-expanded.png#lightbox)

O dashboard mostra três KPIs para os alertas selecionados pelos filtros atuais do dispositivo e do período de tempo:

* O número de alertas ativos para as regras que acionaram a maioria dos alertas.
* A proporção de alertas por tipo de dispositivo.
* A percentagem de alertas que são alertas críticos.

Os mesmos filtros que definem o período de tempo para os alertas, e controlam os dispositivos que são apresentados, determinam como os KPIs são agregados. Por predefinição, o painel mostra KPIs agregados ao longo da última hora.

## <a name="clean-up-resources"></a>Limpar recursos

Se planear avançar para o próximo tutorial, deixe o acelerador de soluções de Monitorização Remota implementado. Para reduzir os custos de execução do acelerador de soluções enquanto não estiver a utilizá-lo, pode parar os dispositivos simulados no painel de definições:

[![Pausar telemetria](./media/iot-accelerators-remote-monitoring-monitor/togglesimulation-inline.png)](./media/iot-accelerators-remote-monitoring-monitor/togglesimulation-expanded.png#lightbox)

Pode reiniciar os dispositivos simulados quando estiver pronto para iniciar o próximo tutorial.

Se já não precisar do acelerador de soluções, elimine-o na página [Soluções aprovisionadas](https://www.azureiotsolutions.com/Accelerators#dashboard):

![Eliminar solução](media/iot-accelerators-remote-monitoring-monitor/deletesolution.png)

## <a name="next-steps"></a>Passos seguintes

Este tutorial mostrou como utilizar a página **Dashboard** no acelerador de soluções da Monitorização Remota, para filtrar e monitorizar os camiões simulados. Para saber como utilizar o acelerador de soluções para detetar problemas com os seus dispositivos ligados, avance para o próximo tutorial.

> [!div class="nextstepaction"]
> [Detetar problemas dos dispositivos ligados à sua solução de monitorização](iot-accelerators-remote-monitoring-automate.md)