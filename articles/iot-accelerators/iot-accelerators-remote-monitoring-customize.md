---
title: Personalizar a monitorização remota solução IU – Azure | Microsoft Docs
description: Este artigo fornece informações sobre como pode aceder ao código fonte o acelerador de solução de monitorização remota da IU e efetuar algumas personalizações.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 01/17/2018
ms.topic: conceptual
ms.openlocfilehash: 6e791051fb82197a770bff05f636159c638e3b9a
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34627856"
---
# <a name="customize-the-remote-monitoring-solution-accelerator"></a>Personalizar o acelerador de solução de monitorização remota

Este artigo fornece informações sobre como pode aceder ao código fonte e personalizar o acelerador de solução de monitorização remota IU. Descreve o artigo:

## <a name="prepare-a-local-development-environment-for-the-ui"></a>Preparar o ambiente de desenvolvimento local para a IU

Código de IU acelerador monitorização remota de solução é implementado utilizando a estrutura de React.js. Pode encontrar o código de origem no [azure-iot-pcs-remote-monitoring-webui](https://github.com/Azure/azure-iot-pcs-remote-monitoring-webui) repositório do GitHub.

Para efetuar alterações para a IU, pode executar uma cópia do mesmo localmente. A cópia local liga-se para uma instância implementada da solução para efetuar ações como obter telemetria.

Os passos seguintes descrevem o processo para configurar um ambiente local para o desenvolvimento de IU:

1. Implementar um **básico** instância do acelerador de solução utilizando o **pcs** CLI. Anote o nome da sua implementação e as credenciais fornecidas para a máquina virtual. Para obter mais informações, consulte [implementar utilizando a CLI](iot-accelerators-remote-monitoring-deploy-cli.md).

1. Utilizar o portal do Azure ou o [az CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) para ativar o acesso SSH para a máquina virtual que aloja os micro-serviços na sua solução. Por exemplo:

    ```sh
    az network nsg rule update --name SSH --nsg-name {your solution name}-nsg --resource-group {your solution name} --access Allow
    ```

1. Utilizar o portal do Azure ou o [az CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) para localizar o nome e endereço IP público da sua máquina virtual. Por exemplo:

    ```sh
    az resource list --resource-group {your solution name} -o table
    az vm list-ip-addresses --name {your vm name from previous command} --resource-group {your solution name} -o table
    ```

1. Utilizar o SSH para ligar à máquina virtual utilizando o endereço IP do passo anterior e as credenciais que forneceu quando executou **pcs** para implementar a solução.

1. Para permitir o UX local estabelecer a ligação, execute os seguintes comandos shell de deteção na máquina virtual:

    ```sh
    cd /app
    sudo ./start.sh --unsafe
    ```

1. Depois de confirmar o comando for concluído e inicia o web site, pode desligar da máquina virtual.

1. Na sua cópia local do [azure-iot-pcs-remote-monitoring-webui](https://github.com/Azure/azure-iot-pcs-remote-monitoring-webui) repositório, edite o **.env** ficheiro para adicionar o URL da sua solução implementada:

    ```config
    NODE_PATH = src/
    REACT_APP_BASE_SERVICE_URL=https://{your solution name}.azurewebsites.net/
    ```

1. Numa linha de comandos na sua cópia local do `azure-iot-pcs-remote-monitoring-webui` pasta, execute os seguintes comandos para instalar as bibliotecas necessárias e executar localmente a IU:

    ```cmd/sh
    npm install
    npm start
    ```

1. O comando anterior é executada a IU localmente em http://localhost:3000/dashboard. Pode editar o código enquanto o site está em execução e vê-lo a atualizar dinamicamente.

## <a name="customize-the-layout"></a>Personalizar o esquema

Cada página na solução de monitorização remota é composta por um conjunto de controlos, referida como *painéis* no código fonte. Por exemplo, o **Dashboard** página é constituída por cinco painéis: Descrição geral, mapa, alarmes, telemetria e KPIs. Pode encontrar o código de origem que define cada página e respetivos painéis no [pcs-remota-monitorização-webui](https://github.com/Azure/pcs-remote-monitoring-webui) repositório do GitHub. Por exemplo, o código que define o **Dashboard** página, a esquema e os painéis na página está localizado no [src/componentes/páginas/dashboard](https://github.com/Azure/pcs-remote-monitoring-webui/tree/master/src/components/pages/dashboard) pasta.

Porque os painéis gerir as seus próprios esquema e o dimensionamento, pode facilmente modificar o esquema de uma página. Por exemplo, as seguintes alterações ao **PageContent** elemento o `src/components/pages/dashboard/dashboard.js` ficheiro trocar as posições de painéis de mapa e telemetria e alterar as larguras relativas do mapa e painéis KPI:

```nodejs
<PageContent className="dashboard-container" key="page-content">
  <Grid>
    <Cell className="col-1 devices-overview-cell">
      <OverviewPanel
        openWarningCount={openWarningCount}
        openCriticalCount={openCriticalCount}
        onlineDeviceCount={onlineDeviceCount}
        offlineDeviceCount={offlineDeviceCount}
        isPending={kpisIsPending || devicesIsPending}
        error={devicesError || kpisError}
        t={t} />
    </Cell>
    <Cell className="col-5">
      <TelemetryPanel
        telemetry={telemetry}
        isPending={telemetryIsPending}
        error={telemetryError}
        colors={chartColorObjects}
        t={t} />
    </Cell>
    <Cell className="col-3">
      <CustAlarmsPanel
        alarms={currentActiveAlarmsWithName}
        isPending={kpisIsPending || rulesIsPending}
        error={rulesError || kpisError}
        t={t} />
    </Cell>
    <Cell className="col-4">
    <PanelErrorBoundary msg={t('dashboard.panels.map.runtimeError')}>
        <MapPanel
          azureMapsKey={azureMapsKey}
          devices={devices}
          devicesInAlarm={devicesInAlarm}
          mapKeyIsPending={azureMapsKeyIsPending}
          isPending={devicesIsPending || kpisIsPending}
          error={azureMapsKeyError || devicesError || kpisError}
          t={t} />
      </PanelErrorBoundary>
    </Cell>
    <Cell className="col-6">
      <KpisPanel
        topAlarms={topAlarmsWithName}
        alarmsPerDeviceId={alarmsPerDeviceType}
        criticalAlarmsChange={criticalAlarmsChange}
        warningAlarmsChange={warningAlarmsChange}
        isPending={kpisIsPending || rulesIsPending || devicesIsPending}
        error={devicesError || rulesError || kpisError}
        colors={chartColorObjects}
        t={t} />
    </Cell>
  </Grid>
</PageContent>
```

![Esquema do painel de alteração](./media/iot-accelerators-remote-monitoring-customize/layout.png)

> [!NOTE]
> O mapa não está configurado na implementação local.

Também pode adicionar várias instâncias do mesmo painel ou várias versões se lhe [duplicado e personalizar um painel](#duplicate-and-customize-an-existing-control). O exemplo seguinte mostra como adicionar duas instâncias do painel de telemetria ao editar o `src/components/pages/dashboard/dashboard.js` ficheiro:

```nodejs
<PageContent className="dashboard-container" key="page-content">
  <Grid>
    <Cell className="col-1 devices-overview-cell">
      <OverviewPanel
        openWarningCount={openWarningCount}
        openCriticalCount={openCriticalCount}
        onlineDeviceCount={onlineDeviceCount}
        offlineDeviceCount={offlineDeviceCount}
        isPending={kpisIsPending || devicesIsPending}
        error={devicesError || kpisError}
        t={t} />
    </Cell>
    <Cell className="col-3">
      <TelemetryPanel
        telemetry={telemetry}
        isPending={telemetryIsPending}
        error={telemetryError}
        colors={chartColorObjects}
        t={t} />
    </Cell>
    <Cell className="col-3">
      <TelemetryPanel
        telemetry={telemetry}
        isPending={telemetryIsPending}
        error={telemetryError}
        colors={chartColorObjects}
        t={t} />
    </Cell>
    <Cell className="col-2">
      <CustAlarmsPanel
        alarms={currentActiveAlarmsWithName}
        isPending={kpisIsPending || rulesIsPending}
        error={rulesError || kpisError}
        t={t} />
    </Cell>
    <Cell className="col-4">
    <PanelErrorBoundary msg={t('dashboard.panels.map.runtimeError')}>
        <MapPanel
          azureMapsKey={azureMapsKey}
          devices={devices}
          devicesInAlarm={devicesInAlarm}
          mapKeyIsPending={azureMapsKeyIsPending}
          isPending={devicesIsPending || kpisIsPending}
          error={azureMapsKeyError || devicesError || kpisError}
          t={t} />
      </PanelErrorBoundary>
    </Cell>
    <Cell className="col-6">
      <KpisPanel
        topAlarms={topAlarmsWithName}
        alarmsPerDeviceId={alarmsPerDeviceType}
        criticalAlarmsChange={criticalAlarmsChange}
        warningAlarmsChange={warningAlarmsChange}
        isPending={kpisIsPending || rulesIsPending || devicesIsPending}
        error={devicesError || rulesError || kpisError}
        colors={chartColorObjects}
        t={t} />
    </Cell>
  </Grid>
</PageContent>
```

Em seguida, pode ver telemetria diferente em cada painel:

![Vários painéis de telemetria](./media/iot-accelerators-remote-monitoring-customize/multiple-telemetry.png)

> [!NOTE]
> O mapa não está configurado na implementação local.

## <a name="duplicate-and-customize-an-existing-control"></a>Duplicar e personalizar um controlo existente

Os passos seguintes descrevem como utilizar o **alarmes** painel como um exemplo de como duplicar um painel existente, modificá-lo e utilizar a versão modificada:

1. Na sua cópia local do repositório, faça uma cópia do **alarmes** pasta o `src/components/pages/dashboard/panels` pasta. Nome da nova cópia **cust_alarms**.

1. No **alarmsPanel.js** ficheiros o **cust_alarms** pasta, edite o nome da classe ser **CustAlarmsPanel**:

    ```nodejs
    export class CustAlarmsPanel extends Component {
    ```

1. Adicione a seguinte linha para o `src/components/pages/dashboard/panels/index.js` ficheiro:

    ```nodejs
    export * from './cust_alarms';
    ```

1. Substitua `AlarmsPanel` com `CustAlarmsPanel` no `src/components/pages/dashboard/dashboard.js` ficheiro:

    ```nodejs
    import {
      OverviewPanel,
      CustAlarmsPanel,
      TelemetryPanel,
      KpisPanel,
      MapPanel,
      transformTelemetryResponse,
      chartColors
    } from './panels';

    ...

    <Cell className="col-3">
      <CustAlarmsPanel
        alarms={currentActiveAlarmsWithName}
        isPending={kpisIsPending || rulesIsPending}
        error={rulesError || kpisError}
        t={t} />
    </Cell>
    ```

Tem agora substituída original **alarmes** painel com uma cópia chamada **CustAlarms**. Esta cópia é idêntica ao original. Agora pode modificar a cópia. Por exemplo, para alterar a coluna de ordenação no **alarmes** painel:

1. Abra o ficheiro `src/components/pages/dashboard/panels/cust_alarms/alarmsPanel.js`.

1. Modificar as definições da coluna, conforme mostrado no seguinte fragmento de código:

    ```nodejs
    this.columnDefs = [
      rulesColumnDefs.severity,
      {
        headerName: 'rules.grid.count',
        field: 'count'
      },
      {
        ...rulesColumnDefs.ruleName,
        minWidth: 200
      },
      rulesColumnDefs.explore
    ];
    ```

A seguinte captura de ecrã mostra a nova versão do **alarmes** painel:

![Painel de alarmes atualizado](./media/iot-accelerators-remote-monitoring-customize/reorder-columns.png)

## <a name="customize-the-telemetry-chart"></a>Personalizar o gráfico de telemetria

O gráfico de telemetria sobre o **Dashboard** página é definida pelos ficheiros no `src/components/pages/dashboard/panels/telemtry` pasta. A IU obtém a telemetria do solução de back-end no `src/services/telemetryService.js` ficheiro. Os passos seguintes mostram como alterar o período de tempo apresentado no gráfico de telemetria de 15 minutos para 5 minutos:

1. No `src/services/telemetryService.js` ficheiro, localize a função chamada **getTelemetryByDeviceIdP15M**. Faça uma cópia desta função e modificar a cópia da seguinte forma:

    ```nodejs
    static getTelemetryByDeviceIdP5M(devices = []) {
      return TelemetryService.getTelemetryByMessages({
        from: 'NOW-PT5M',
        to: 'NOW',
        order: 'desc',
        devices
      });
    }
    ```

1. Para utilizar esta nova função para preencher o gráfico de telemetria, abra o `src/components/pages/dashboard/dashboard.js` ficheiro. Localize a linha que inicializa o fluxo de telemetria e modificá-lo da seguinte forma:

    ```node.js
    const getTelemetryStream = ({ deviceIds = [] }) => TelemetryService.getTelemetryByDeviceIdP5M(deviceIds)
    ```

O gráfico de telemetria agora mostra os cinco minutos de dados de telemetria:

![Gráfico de telemetria que mostra um dia](./media/iot-accelerators-remote-monitoring-customize/telemetry-period.png)

## <a name="add-a-new-kpi"></a>Adicionar um novo KPI

O **Dashboard** página apresenta os KPIs o **sistema KPIs** painel. Estes KPIs são calculadas no `src/components/pages/dashboard/dashboard.js` ficheiro. Os KPIs são compostos pelo `src/components/pages/dashboard/panels/kpis/kpisPanel.js` ficheiro. Os passos seguintes descrevem como calcular e compor um novo valor KPI no **Dashboard** página. O exemplo apresentado é adicionar uma nova alteração de percentagem aviso alarmes KPI:

1. Abra o ficheiro `src/components/pages/dashboard/dashboard.js`. Modificar o **initialState** objeto para incluir um **warningAlarmsChange** propriedade da seguinte forma:

    ```nodejs
    const initialState = {
      ...

      // Kpis data
      currentActiveAlarms: [],
      topAlarms: [],
      alarmsPerDeviceId: {},
      criticalAlarmsChange: 0,
      warningAlarmsChange: 0,
      kpisIsPending: true,
      kpisError: null,

      ...
    };
    ```

1. Modificar o **currentAlarmsStats** objeto para incluir **totalWarningCount** como uma propriedade:

    ```nodejs
    return {
      openWarningCount: (acc.openWarningCount || 0) + (isWarning && isOpen ? 1 : 0),
      openCriticalCount: (acc.openCriticalCount || 0) + (isCritical && isOpen ? 1 : 0),
      totalWarningCount: (acc.totalWarningCount || 0) + (isWarning ? 1 : 0),
      totalCriticalCount: (acc.totalCriticalCount || 0) + (isCritical ? 1 : 0),
      alarmsPerDeviceId: updatedAlarmsPerDeviceId
    };
    ```

1. Calcule o KPI de novo. Localize o cálculo para a contagem de alarmes críticos. Duplicar o código e modificar a cópia da seguinte forma:

    ```nodejs
    // ================== Warning Alarms Count - START
    const currentWarningAlarms = currentAlarmsStats.totalWarningCount;
    const previousWarningAlarms = previousAlarms.reduce(
      (cnt, { severity }) => severity === 'warning' ? cnt + 1 : cnt,
      0
    );
    const warningAlarmsChange = ((currentWarningAlarms - previousWarningAlarms) / currentWarningAlarms * 100).toFixed(2);
    // ================== Warning Alarms Count - END
    ```

1. Inclua a nova **warningAlarmsChange** KPI no fluxo de KPI:

    ```nodejs
    return ({
      kpisIsPending: false,

      // Kpis data
      currentActiveAlarms,
      topAlarms,
      criticalAlarmsChange,
      warningAlarmsChange,
      alarmsPerDeviceId: currentAlarmsStats.alarmsPerDeviceId,

      ...
    });

1. Include the new **warningAlarmsChange** KPI in the state data used to render the UI:

    ```nodejs
    const {
      ...

      currentActiveAlarms,
      topAlarms,
      alarmsPerDeviceId,
      criticalAlarmsChange,
      warningAlarmsChange,
      kpisIsPending,
      kpisError,

      ...
    } = this.state;
    ```

1. Atualize os dados transmitidos para o painel de KPI:

    ```node.js
    <KpisPanel
      topAlarms={topAlarmsWithName}
      alarmsPerDeviceId={alarmsPerDeviceType}
      criticalAlarmsChange={criticalAlarmsChange}
      warningAlarmsChange={warningAlarmsChange}
      isPending={kpisIsPending || rulesIsPending || devicesIsPending}
      error={devicesError || rulesError || kpisError}
      colors={chartColorObjects}
      t={t} />
    ```

Terminar agora as alterações a `src/components/pages/dashboard/dashboard.js` ficheiro. Os passos seguintes descrevem as alterações para tornar no `src/components/pages/dashboard/panels/kpis/kpisPanel.js` ficheiro para apresentar o novo KPI:

1. Modifique a seguinte linha de código para obter o novo valor KPI da seguinte forma:

    ```nodejs
    const { t, isPending, criticalAlarmsChange, warningAlarmsChange, error } = this.props;
    ```

1. Modifique a marcação para apresentar o novo valor KPI da seguinte forma:

    ```nodejs
    <div className="kpi-cell">
      <div className="kpi-header">{t('dashboard.panels.kpis.criticalAlarms')}</div>
      <div className="critical-alarms">
        {
          criticalAlarmsChange !== 0 &&
            <div className="kpi-percentage-container">
              <div className="kpi-value">{ criticalAlarmsChange }</div>
              <div className="kpi-percentage-sign">%</div>
            </div>
        }
      </div>
      <div className="kpi-header">{t('Warning alarms')}</div>
      <div className="critical-alarms">
        {
          warningAlarmsChange !== 0 &&
            <div className="kpi-percentage-container">
              <div className="kpi-value">{ warningAlarmsChange }</div>
              <div className="kpi-percentage-sign">%</div>
            </div>
        }
      </div>
    </div>
    ```

O **Dashboard** página apresenta agora o novo valor KPI:

![Aviso KPI](./media/iot-accelerators-remote-monitoring-customize/new-kpi.png)

## <a name="customize-the-map"></a>Personalizar o mapa

Consulte o [Personalizar mapa](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide#upgrade-map-key-to-see-devices-on-a-dynamic-map) página no GitHub para obter detalhes sobre os componentes de mapa na solução.

<!--
### Connect an external visualization tool

See the [Connect an external visualization tool](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/) page in GitHub for details of how to connect an external visualization tool.

-->

## <a name="other-customization-options"></a>Outras opções de personalização

Para modificar ainda mais a camada de apresentação e visualizações na solução de monitorização remota, pode editar o código. Repositórios do GitHub relevantes são:

* [O configuração de microsserviço para soluções de IoT do Azure (.NET)](https://github.com/Azure/pcs-ui-config-dotnet/)
* [O microsserviço de configuração para soluções de IoT do Azure (Java)](https://github.com/Azure/pcs-ui-config-java/)
* [Remoto do Azure IoT PCS monitorização IU da Web](https://github.com/Azure/pcs-remote-monitoring-webui)

## <a name="next-steps"></a>Passos Seguintes

Neste artigo, aprendeu sobre os recursos disponíveis para o ajudar a personalizar a IU da web no acelerador de solução de monitorização remota.

Para obter mais informações concetuais sobre o acelerador de solução de monitorização remota, consulte [arquitetura de monitorização remota](iot-accelerators-remote-monitoring-sample-walkthrough.md)

Para obter mais informações sobre como personalizar a solução de monitorização remota, consulte [personalizar e volte a implementar um microsserviço](iot-accelerators-microservices-example.md)
<!-- Next tutorials in the sequence -->