---
title: Adicionar um submenus para a solução de monitorização remota da interface do Usuário - Azure | Documentos da Microsoft
description: Este artigo mostra-lhe como adicionar um submenus novo numa página da web de acelerador de solução da interface do Usuário da monitorização remota.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 10/05/2018
ms.topic: conceptual
ms.openlocfilehash: 9ba58ca887332d2ea224320951b25031cacbef0d
ms.sourcegitcommit: 4047b262cf2a1441a7ae82f8ac7a80ec148c40c4
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/11/2018
ms.locfileid: "49094609"
---
# <a name="add-a-custom-fly-out-to-the-remote-monitoring-solution-accelerator-web-ui"></a>Adicionar um submenus personalizado para monitorização remota solution accelerator da IU da web

Este artigo mostra-lhe como adicionar um submenus novo numa página web de acelerador de solução da interface do Usuário da monitorização remota. O artigo descreve:

- Como preparar um ambiente de desenvolvimento local.
- Como adicionar um submenus novo a uma página na IU da web.

O exemplo sem perder tempo-limite neste artigo apresenta a página e a grade que o [adicionar uma grade personalizada para monitorização remota solution accelerator da IU da web](iot-accelerators-remote-monitoring-customize-grid.md) procedimento artigo mostra-lhe como adicionar.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir os passos neste guia de procedimentos, terá o seguinte software instalado no seu computador de desenvolvimento local:

- [Git](https://git-scm.com/downloads)
- [Node.js](https://nodejs.org/download/)

## <a name="before-you-start"></a>Antes de começar

Antes de continuar, deve de concluir os passos nos seguintes artigos:

- [Adicionar uma página personalizada para monitorização remota solution accelerator da IU da web](iot-accelerators-remote-monitoring-customize-page.md).
- [Adicionar um serviço personalizado para monitorização remota solution accelerator da IU da web](iot-accelerators-remote-monitoring-customize-service.md)
- [Adicionar uma grade personalizada para monitorização remota solution accelerator da IU da web](iot-accelerators-remote-monitoring-customize-grid.md)

## <a name="add-a-fly-out"></a>Adicionar um submenus

Para adicionar um submenus para a IU da web, terá de adicionar os ficheiros de origem que definem os submenus e modificar alguns ficheiros existentes para tornar a IU da web em consideração o novo componente.

### <a name="add-the-new-files-that-define-the-fly-out"></a>Adicionar os novos ficheiros que definem os submenus

Para começar, o **src/instruções/componentes/páginas/pageWithFlyout/flyouts/exampleFlyout** pasta contém os ficheiros que definem um submenus:

**exampleFlyout.container.js**

[!code-javascript[Example fly-out container](~/remote-monitoring-webui/src/walkthrough/components/pages/pageWithFlyout/flyouts/exampleFlyout/exampleFlyout.container.js?name=flyoutcontainer "Example fly-out container")]

**exampleFlyout.js**

[!code-javascript[Example fly-out](~/remote-monitoring-webui/src/walkthrough/components/pages/pageWithFlyout/flyouts/exampleFlyout/exampleFlyout.js?name=flyout "Example fly-out")]

Copiar o **passo a passo/src/componentes/pageWithFlyout/páginas/flyouts** pasta para o **src/componentes/páginas/exemplo** pasta.

### <a name="add-the-fly-out-to-the-page"></a>Adicionar os submenus à página

Modificar a **src/components/pages/example/basicPage.js** para adicionar os submenus.

Adicione **Btn** para as importações de **componentes/partilhados** e adicione importações para **svgs** e **ExampleFlyoutContainer**:

```js
import {
  AjaxError,
  ContextMenu,
  PageContent,
  RefreshBar,
  Btn
} from 'components/shared';
import { ExampleGrid } from './exampleGrid';
import { svgs } from 'utilities';
import { ExampleFlyoutContainer } from './flyouts/exampleFlyout';
```

Adicionar uma **const** definição para **closedFlyoutState** e adicioná-lo para o estado no construtor:

```js
const closedFlyoutState = { openFlyoutName: undefined };

export class BasicPage extends Component {
  constructor(props) {
    super(props);
    this.state = { contextBtns: null, closedFlyoutState };
  }
```

Adicione as seguintes funções para o **BasicPage** classe:

```js
  closeFlyout = () => this.setState(closedFlyoutState);

  openFlyout = (name) => () => this.setState({ openFlyoutName: name });
```

Adicione as seguintes **const** definições para o **renderizar** função:

```js
    const { openFlyoutName } = this.state;

    const isExampleFlyoutOpen = openFlyoutName === 'example';
```

Adicione um botão para abrir o submenus ao menu de contexto:

```js
      <ContextMenu key="context-menu">
        {this.state.contextBtns}
        <Btn svg={svgs.reconfigure} onClick={this.openFlyout('example')}>{t('walkthrough.pageWithFlyout.open')}</Btn>
      </ContextMenu>,
```

Adicione algum texto e o contentor de submenus para o conteúdo da página:

```js
      <PageContent className="basic-page-container" key="page-content">
        {t('walkthrough.pageWithFlyout.pageBody')}
        { isExampleFlyoutOpen && <ExampleFlyoutContainer onClose={this.closeFlyout} /> }
        <RefreshBar refresh={fetchData} time={lastUpdated} isPending={isPending} t={t} />
        {!!error && <AjaxError t={t} error={error} />}
        {!error && <ExampleGrid {...gridProps} />}
      </PageContent>
```

## <a name="test-the-fly-out"></a>Testar os submenus

Se o web interface do Usuário não está já em execução localmente, execute o seguinte comando na raiz da sua cópia local do repositório:

```cmd/sh
npm start
```

O comando anterior é a interface do Usuário localmente em executado [ http://localhost:3000/dashboard ](http://localhost:3000/dashboard). Navegue para o **exemplo** página e clique em **flutuante aberto**.

## <a name="next-steps"></a>Passos Seguintes

Neste artigo, aprendeu sobre os recursos disponíveis para o ajudar a adicionar ou personalizar páginas na IU da web no solution accelerator monitorização remota.

Agora que definiu uma submenus numa página, a próxima etapa é [adicionar um painel para o dashboard de monitorização remota solution accelerator da IU da web](iot-accelerators-remote-monitoring-customize-panel.md).

Para obter mais informações concetuais sobre o acelerador de solução de monitorização remota, consulte [arquitetura de monitorização remota](iot-accelerators-remote-monitoring-sample-walkthrough.md).
