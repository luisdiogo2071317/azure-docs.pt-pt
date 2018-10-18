---
title: Realizar uma análise da causa raiz num alerta – Azure | Microsoft Docs
description: Neste tutorial, irá aprender a realizar uma análise da causa raiz num alerta com o Azure Time Series Insights.
author: aditidugar
ms.author: adugar
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 09/11/2018
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: 8258c8f34b4b9a1b216d9d497dcdf7d3b8db1373
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/19/2018
ms.locfileid: "46369516"
---
# <a name="tutorial-conduct-a-root-cause-analysis-on-an-alert"></a>Tutorial: Realizar uma análise da causa raiz num alerta

Neste tutorial, irá aprender a utilizar o acelerador de soluções da Monitorização Remota para diagnosticar a causa raiz de um alerta. Verá que um alerta foi acionado no dashboard da solução de Monitorização Remota e, em seguida, utilizará o explorador do Azure Time Series Insights para investigar a causa raiz.

O tutorial utiliza dois dispositivos simulados de camiões de entregas que enviam telemetria de localização, altitude, velocidade e de temperatura da carga. Os camiões são geridos por uma organização chamada Contoso e estão ligados ao acelerador de soluções de Monitorização Remota. Enquanto operador da Contoso, tem de perceber por que razão um dos seus camiões (delivery-truck-02) registou um alerta de temperatura baixa.

Neste tutorial:

>[!div class="checklist"]
> * Filtrar os dispositivos no dashboard
> * Ver telemetria em tempo real
> * Explorar dados no explorador do Time Series Insights
> * Realizar uma análise da causa raiz
> * Criar uma nova regra com base no que aprendeu

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

[!INCLUDE [iot-accelerators-tutorial-prereqs](../../includes/iot-accelerators-tutorial-prereqs.md)]

## <a name="choose-the-devices-to-display"></a>Selecionar os dispositivos a apresentar

Para selecionar os dispositivos ligados que são apresentados na página **Dashboard**, utilize os filtros. Para apresentar apenas os dispositivos de **Camião**, escolha o filtro **Camiões** incorporado na lista pendente de filtros:

[![Filtrar camiões no dashboard](./media/iot-accelerators-remote-monitoring-root-cause-analysis/filter-trucks-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/filter-trucks-expanded.png#lightbox)

Quando aplicar um filtro, escolha apenas os dispositivos que correspondem às condições de filtro apresentadas no painel de telemetria na página **Dashboard**. Pode ver que existem dois camiões ligados ao acelerador de soluções, incluindo o **truck-02**.

## <a name="view-real-time-telemetry"></a>Ver telemetria em tempo real

O acelerador de soluções rastreia a telemetria em tempo real no gráfico na página **Dashboard**. Por predefinição, o gráfico mostra a telemetria de altitude, que varia ao longo do tempo:

[![Gráfico de telemetria da altitude do camião](./media/iot-accelerators-remote-monitoring-root-cause-analysis/trucks-moving-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/trucks-moving-expanded.png#lightbox)

Para ver a telemetria de temperatura dos camiões, clique em **Temperatura** no **Painel de Telemetria**. Pode ver como a temperatura dos dois camiões variou durante os últimos 15 minutos. Também pode ver que foi acionado um alerta de temperatura baixa para o delivery-truck-02 no painel de alertas.

[![Dashboard de RM com alerta de temperatura baixa](./media/iot-accelerators-remote-monitoring-root-cause-analysis/low-temp-alert-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/low-temp-alert-expanded.png#lightbox)

## <a name="explore-the-data"></a>Explorar os dados

Para identificar a causa do alarme de temperatura baixa, abra os dados telemétricos do camião de entregas no explorador do Time Series Insights. Clique em qualquer uma das ligações **Explorar no Time Series Insights** no dashboard:

[![Dashboard de RM com as ligações do TSI destacadas](./media/iot-accelerators-remote-monitoring-root-cause-analysis/explore-tsi-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/explore-tsi-expanded.png#lightbox)

Quando inicia o explorador, vê uma lista de todos os seus dispositivos:

[![Vista inicial do Explorador do TSI](./media/iot-accelerators-remote-monitoring-root-cause-analysis/initial-tsi-view-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/initial-tsi-view-expanded.png#lightbox)

Filtre os dispositivos ao escrever **delivery-truck** na caixa do filtro e selecione **temperatura** como a **Medida** no painel esquerdo:

[![Temperatura do camião no Explorador do TSI](./media/iot-accelerators-remote-monitoring-root-cause-analysis/filter-tsi-temp-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/filter-tsi-temp-expanded.png#lightbox)

Verá a mesma vista que viu no dashboard da Monitorização Remota e poderá ampliar o intervalo de tempo em que o alerta foi acionado:

[![Ampliação no Explorador do TSI](./media/iot-accelerators-remote-monitoring-root-cause-analysis/tsi-zoom-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/tsi-zoom-expanded.png#lightbox)

Também pode adicionar outros fluxos de telemetria provenientes dos camiões. Clique no botão **Adicionar** no canto superior esquerdo. É apresentado um novo painel:

[![Explorador do TSI com novo painel](./media/iot-accelerators-remote-monitoring-root-cause-analysis/tsi-add-pane-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/tsi-add-pane-expanded.png#lightbox)

No novo painel, mude o nome da nova etiqueta para **Dispositivos**, para que corresponda à anterior. Selecione **altitude** como a **Medida** e **iothub-connection-device-id** como o valor **Dividir Por** para adicionar a telemetria de altitude à sua vista:

[![Explorador do TSI com temperatura e altitude](./media/iot-accelerators-remote-monitoring-root-cause-analysis/tsi-add-altitude-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/tsi-add-altitude-expanded.png#lightbox)

## <a name="diagnose-the-alert"></a>Diagnosticar o alerta

Quando examina os fluxos na vista atual, vê que os perfis de altitude dos dois camiões são muito diferentes. Além disso, a queda de temperatura no **delivery-truck-02** ocorre quando o camião atinge uma altitude elevada. Esta informação é inesperada, porque foi atribuída a mesma rota aos dois camiões.

Para confirmar a suspeita de que os camiões estão a fazer percursos diferentes, adicione outro painel ao painel lateral com o botão **Adicionar**. No novo painel, mude o nome da nova etiqueta para **Dispositivos**, para que corresponda à anterior. Selecione **longitude** como a **Medida** e **iothub-connection-device-id** como o valor **Dividir Por** para adicionar a telemetria de longitude à sua vista. Pode ver que os camiões fizeram percursos diferentes ao observar a diferença entre os fluxos de **longitude**:

[![Explorador do TSI com temperatura, altitude e longitude](./media/iot-accelerators-remote-monitoring-root-cause-analysis/tsi-add-longitude-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/tsi-add-longitude-expanded.png#lightbox)

## <a name="create-a-new-rule"></a>Criar uma nova regra

Embora, normalmente, as rotas dos camiões sejam otimizadas com antecedência, fatores como os padrões de tráfego, o clima e outros eventos imprevisíveis podem causar atrasos e deixar decisões de última hora quanto à rota a seguir a cargo do bom senso dos condutores. No entanto, uma vez que a temperatura dos bens que o veículo transporta é fundamental, deve criar uma regra adicional na solução de Monitorização Remota para se certificar de que recebe um aviso caso a altitude média ao longo de um intervalo de 1 minuto ultrapasse os 100 metros:

[![Separador de regras da Monitorização Remota para definir uma regra de altitude](./media/iot-accelerators-remote-monitoring-root-cause-analysis/new-rule-altitude-inline.png)](./media/iot-accelerators-remote-monitoring-root-cause-analysis/new-rule-altitude-expanded.png#lightbox)

Para saber como criar e editar regras, consulte o tutorial anterior sobre como [detetar problemas de dispositivo](iot-accelerators-remote-monitoring-automate.md).

[!INCLUDE [iot-accelerators-tutorial-cleanup](../../includes/iot-accelerators-tutorial-cleanup.md)]

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, aprendeu a utilizar o explorador do Time Series Insights com o acelerador de soluções da Monitorização Remota para diagnosticar a causa raiz de um alerta. Para saber como utilizar o acelerador de soluções para identificar e corrigir problemas com os seus dispositivos ligados, avance para o próximo tutorial.

> [!div class="nextstepaction"]
> [Utilizar alertas de dispositivos para identificar e corrigir problemas com os dispositivos ligados à sua solução de monitorização](iot-accelerators-remote-monitoring-maintain.md)
