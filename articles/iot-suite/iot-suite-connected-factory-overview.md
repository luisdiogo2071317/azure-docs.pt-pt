---
title: Descrição geral da solução Fábrica Ligada - Azure | Microsoft Docs
description: Uma descrição do acelerador da solução Fábrica Ligada de IoT do Azure.
services: iot-suite
suite: iot-suite
documentationcenter: ''
author: dominicbetts
manager: timlt
editor: ''
ms.assetid: ''
ms.service: iot-suite
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/12/2017
ms.author: dobett
ms.openlocfilehash: c5fe6cf6dff6fd8951a949761739cb12f98834bf
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="get-started-with-the-connected-factory-solution-accelerator"></a>Introdução ao acelerador da solução Fábrica Ligada

Os [aceleradores de soluções][lnk-preconfigured-solutions] de IoT do Azure combinam vários serviços de IoT do Azure para fornecer soluções ponto a ponto que implementam cenários de negócio comuns de IoT. O acelerador da solução *Fábrica Ligada* liga-se e monitoriza os seus dispositivos industriais. Pode utilizá-la para analisar o fluxo de dados dos seus dispositivos e para aumentar a produtividade operacional e a rentabilidade.

Este tutorial mostra-lhe como aprovisionar o acelerador da solução Fábrica Ligada. Também disponibiliza informações sobre as funcionalidades básicas do acelerador da solução. Pode aceder a muitas destas funcionalidades através do *dashboard* da solução implementado como parte do acelerador da solução:

![Dashboard do acelerador da solução Fábrica Ligada][img-cf-home]

Para concluir este tutorial, precisa de uma subscrição ativa do Azure.

> [!NOTE]
> Se não tiver uma conta, pode criar uma de avaliação gratuita em apenas alguns minutos. Para obter mais detalhes, consulte [Azure Free Trial (Avaliação Gratuita do Azure)][lnk_free_trial].

## <a name="provision-the-solution"></a>Aprovisionar a solução

1. Inicie sessão em azureiotsuite.com com as credenciais da sua conta do Azure e clique em “**+**” para criar uma solução.
2. Clique em **Selecionar** no mosaico **Fábrica Ligada**.
3. Introduza um **Nome da solução** para o acelerador da solução Fábrica Ligada.
4. Selecione a **Subscrição** e a **Região** que quer utilizar para aprovisionar a solução.
5. Clique em **Criar Solução** para iniciar o processo de aprovisionamento. Este processo ação demora vários minutos para ser executado.

### <a name="while-you-wait-for-the-provisioning-process-to-complete"></a>Enquanto aguarda pela conclusão do processo de aprovisionamento

1. Clique no mosaico da sua solução com o estado **Aprovisionamento**.
2. Tenha em atenção os **Estados de aprovisionamento** uma vez que os serviços do Azure estão implementados na sua subscrição do Azure.
3. Depois de ter concluído o aprovisionamento, o estado passa para **Pronto**.
4. Clique no mosaico para ver os detalhes da sua solução no painel da direita.

> [!NOTE]
> Se detetar algum problema ao implementar o acelerador da solução, consulte [Permissões no site azureiotsuite.com][lnk-permissions] e as [FAQ sobre Fábrica Ligada](iot-suite-faq-cf.md). Se os problemas persistirem, crie um pedido de serviço no [portal][lnk-portal].

Pretendia ver certos detalhes que não se encontram listados para a sua solução? Sugira funcionalidades no [User Voice](https://feedback.azure.com/forums/321918-azure-iot).

## <a name="scenario-overview"></a>Descrição geral do cenário

Quando implementa o acelerador da solução Fábrica Ligada, este é pré-povoado com recursos que lhe permitem aceder a um cenário industrial comum. Neste cenário, várias fábricas ligadas à solução reportam os valores de dados necessários para calcular a eficiência geral dos equipamentos (OEE, Overall Equipment Efficiency) e os indicadores chave de desempenho (KPI, Key Performance Indicator). As secções seguintes mostram como:

* Monitorizar valores de fábrica, linhas de produção, OEEs de estações e KPIs
* Analisar os dados de telemetria gerados a partir desses dispositivos através do Azure Time Series Insights
* Responder a alarmes para corrigir problemas

Uma funcionalidade chave deste cenário é que pode efetuar todas estas ações remotamente a partir do dashboard da solução. Não precisa de acesso físico aos dispositivos.

## <a name="view-the-solution-dashboard"></a>Ver o dashboard da solução

O dashboard de solução permite-lhe gerir a solução implementada. É uma representação hierárquica de uma configuração de fábrica global. Por exemplo, pode ver OEEs e KPIs, publicar nós novos para telemetria e ver alarmes de ação.

1. Quando o aprovisionamento estiver concluído e o mosaico do acelerador da solução indicar **Pronto**, escolha **Iniciar** para abrir o portal da solução Fábrica Ligada num novo separador.

    ![Iniciar o acelerador da solução][img-launch-solution]

1. Por predefinição, o portal de solução mostra o *dashboard*. Para navegar para outras áreas do portal, utilize o menu do lado esquerdo da página.

    ![Dashboard do acelerador da solução Fábrica Ligada][cf-img-menu]

O dashboard apresenta as seguintes informações:

* Um painel **Localizações de fábricas** que mostra o estado, a localização e a configuração de produção atual da solução. Quando executar a solução pela primeira vez, existem vários dispositivos simulados. A simulação de linha de produção é composta por três servidores OPC UA reais por linha de produção que realizam tarefas simuladas e partilham dados. Para obter mais informações sobre OPC UA, veja as [FAQ sobre a solução Fábrica Ligada](iot-suite-faq-cf.md).
* Um **mapa** que apresenta a localização de cada dispositivo ligado à solução. A solução pode utilizar a API dos Mapas Bing para desenhar informações no mapa. Se a sua subscrição estiver ativada para a API dos Mapas Bing para Empresas, esta funcionalidade é utilizada automaticamente. Se não estiver, veja as [FAQ][lnk-faq] para saber como tornar o mapa dinâmico.
* Um painel **Alarmes** que apresenta alarmes gerados quando um valor de telemetria ou OEE/KPI excede um limiar específico.
* Um painel **Eficiência Geral de Equipamentos** que mostra os valores de OEE de toda a empresa ou da fábrica/linha de produção/estação que está a ver. Este valor é agregado da vista de estação para o nível de empresa. O número de OEE e os elementos que o constituem podem ser ainda mais analisados.
* Um painel **Indicadores Chave de Desempenho** que apresenta o número de unidades produzidas e a energia utilizada por toda a empresa ou pela fábrica/linha de produção/estação que está a ver. Estes valores são agregados de uma vista de estação para o nível de empresa.

## <a name="view-factories"></a>Ver fábricas

O painel *Localizações de Fábricas* mostra-lhe a localização geográfica de todas as fábricas na solução, o respetivo estado e a configuração de produção atual. Na lista de localizações, pode navegar para os outros níveis na hierarquia da solução. As linhas da lista são hiperligações que ligam detalhes das linhas de produção nessa localização. Posteriormente, é possível ver os detalhes da linha de produção e ainda mais pormenorizadamente a vista de nível de estação. Também pode aplicar um filtro à lista.

![Fábricas do acelerador da solução Fábrica Ligada][cf-img-factories]

1. O **painel Fábrica** mostra a lista de fábricas desta solução.

2. A lista de fábricas mostra inicialmente seis fábricas criadas pelo processo de aprovisionamento. Pode adicionar mais dispositivos simulados e físicos à solução.

3. Para ver os detalhes de uma fábrica, clique na linha da lista de fábricas.

4. Para ver os detalhes de uma linha de produção, clique na linha da lista.

5. Para ver os nós OPC UA publicados de uma estação da linha de produção, clique na linha da lista.

6. Para ver os detalhes num nó específico na estação, clique na linha da lista. Esta ação inicia o painel de contexto com as visualizações do Time Series Insights. Clique nestes gráficos para fazer ainda mais análises no ambiente do explorador do Time Series Insights.

## <a name="view-map"></a>Ver mapa

Se a sua subscrição tiver acesso à API dos Mapas Bing, o mapa *Fábricas* mostra-lhe a localização geográfica e o estado de todas as fábricas da solução. Para analisar detalhadamente os detalhes de uma localização, clique nas localizações apresentadas no mapa.

![Mapa do acelerador da solução Fábrica Ligada][cf-img-map]

## <a name="view-alarms"></a>Ver alarmes

O painel **Alarmes** mostra-lhe os alarmes gerados devido a valores reportados ou a valores OEE/KPI calculados que excedam o respetivo limiar configurado. Este painel apresenta alarmes em cada nível da hierarquia, da vista de nível de estação à vista global. Os alarmes contêm uma descrição, a data, a hora, a localização e o número de ocorrências. Pode utilizar os dados do Time Series Insights para obter informações sobre os dados que provocaram o alarme. Os dados do Time Series Insights são visualizados nos alarmes, sempre que aplicável. Se for Administrador, pode tomar medidas predefinidas nos alarmes, como:

* Feche o alarme.
* Reconhecer o alarme.

Opcionalmente, pode tomar medidas mais complexas. Por exemplo, relativamente ao Nó OPC UA de Pressão da Montagem, pode:

* Mostrar informações de suporte numa página Web numa janela do browser nova.
* Mitigar a causa do alarme, ao chamar um método OPC UA no dispositivo.
* Suprimir a disponibilidade das medidas predefinidas.

    ![Alarmes do acelerador da solução Fábrica Ligada][cf-img-alerts]

> [!NOTE]
> Estes alarmes são gerados por regras que estão especificadas num ficheiro de configuração do acelerador da solução. Estas regras podem gerar alarmes quando os valores de OEE ou KPI ou do Nó OPC UA estão a exceder o respetivo limiar configurado.

1. O **painel Alarmes** mostra os alarmes gerados nesta solução.

2. Para ver os detalhes de um alarme, clique no mesmo no painel Alarmes.

3. Para analisar mais aprofundadamente os dados do alarme, clique no gráfico no painel Alarmes para abrir o ambiente do explorador do Time Series Insights.

4. Para resolver o alarme, estão disponíveis várias medidas no painel Alarmes. Escolha a opção adequada para si e clique no botão do comando Executar Ação.

## <a name="view-overall-equipment-efficiency"></a>Ver a eficiência global dos equipamentos

O OEE classifica a eficiência do processo de fabrico através de parâmetros operacionais chave relacionados com a produção. O OEE é uma medida padrão na indústria, que é calculada ao multiplicar a taxa de disponibilidade, a taxa de desempenho e a taxa de qualidade: OEE = disponibilidade x desempenho x qualidade.

![OEE do acelerador da solução Fábrica Ligada][cf-img-oee]

1. Para ver o OEE relativo a cada nível da hierarquia, navegue para a vista específica em causa. O OEE desta vista é apresentado no painel, juntamente com cada elemento que compõe a percentagem de OEE.

2. Para analisar mais aprofundadamente o OEE relativamente a cada nível nos dados da hierarquia, clique na percentagem de OEE, de disponibilidade, de desempenho ou de qualidade. É apresentado um painel de contexto com visualizações baseadas no Time Series Insights que mostra dados da última hora, das últimas 24 horas e dos últimos sete dias.

    ![Visualização de TSI do acelerador da solução Fábrica Ligada][cf-img-tsi-visualization]

3. Para analisar mais aprofundadamente os dados do alarme, clique no gráfico no painel Alarmes. Esta ação abre o ambiente do explorador do Time Series Insights.

    ![Explorador de TSI do acelerador da solução Fábrica Ligada][cf-img-tsi-explorer]

## <a name="view-key-performance-indicators"></a>Ver Indicadores Chave de Desempenho

A solução disponibiliza dois indicadores chave de desempenho, *unidades por hora* e *energia utilizada em kWh*.

![KPI do acelerador da solução Fábrica Ligada][cf-img-kpi]

1. Para ver as unidades por hora ou a energia utilizada relativamente a qualquer nível na hierarquia, navegue para a vista específica em causa. As unidades por hora e a energia utilizada são apresentadas no painel.

2. Para analisar as unidades por hora ou a energia utilizada relativamente a qualquer nível na hierarquia de dados, clique no medidor no painel **Indicadores Chave de Desempenho**. É apresentado um painel de contexto com visualizações baseadas no Time Series Insights que lhe permitem ver dados da última hora, das últimas 24 horas e dos últimos sete dias.

## <a name="scenario-review"></a>Revisão do cenário

Neste cenário, monitorizámos os valores de OEE e KPIs das suas fábricas no dashboard. Em seguida, utilizámos o Time Series Insights para disponibilizar mais informações para ver mais detalhes sobre os dados de telemetria relativos a OEE e KPIs, de modo a ajudar a detetar anomalias. Também utilizou o painel Alarmes para ver problemas com as suas fábricas e utilizou as medidas disponíveis para resolver os alarmes.

## <a name="other-features"></a>Outras funcionalidades

As secções seguintes descrevem algumas funcionalidades adicionais da solução Fábrica Ligada que não estão descritas no cenário anterior.

## <a name="apply-filters"></a>Aplicar filtros

1. Clique no ícone **funil** para ver uma lista dos filtros disponíveis no painel de localizações das fábricas ou de alarmes.

2. É apresentado o painel de filtros.

    ![Filtros do acelerador da solução Fábrica Ligada][cf-img-alert-filter]

3. Escolha o filtro de que precisa. Também é possível introduzir texto livre nos campos do filtro.

4. O filtro é, então, aplicado. O estado do filtro também é mostrado no dashboard, através de um funil que é apresentado nas tabelas de fábricas e de alarmes.

    ![Filtros do acelerador da solução Fábrica Ligada][cf-img-alert-filter-funnel]

    > [!NOTE]
    > Os filtros ativos não afetam os valores de OEE e KPI apresentados; só filtram os conteúdos da lista.

5. Para limpar um filtro, clique no funil e clique em Filtro, no painel de contexto do filtro. É apresentado o texto **Todos** nas tabelas de fábricas e de alarmes.

## <a name="browse-an-opc-ua-server"></a>Procurar servidores OPC UA

Quando implementa o acelerador da solução, está a aprovisionar automaticamente servidores OPC UA simulados, nos quais pode navegar através do browser da solução. Estes servidores são *servidores OPC UA simulados*. Os servidores simulados permitem-lhe testar facilmente o acelerador da solução sem ser necessário implementar servidores físicos reais. Se quiser ligar um servidor OPC UA real à solução, veja o tutorial [Ligar o dispositivo OPC UA ao acelerador da solução Fábrica Ligada][lnk-connect-cf].

1. Clique no **ícone de browser** na barra de navegação do dashboard.

    ![Browser do servidor do acelerador da solução Fábrica Ligada][cf-img-server-browser]

2. Escolha um dos servidores da lista pré-configurada. Esta lista mostra os servidores implementados automaticamente no acelerador da solução.

    ![Seleção de servidores do acelerador da solução Fábrica Ligada][cf-img-server-choice]

3. Clique em **Ligar**; é apresentada uma caixa de diálogo de segurança. Para a simulação, é seguro clicar em **Continuar**.

4. Para expandir um dos nós na árvore de servidores, clique no mesmo. Os nós que estão a publicar telemetria têm uma marca de visto ao lado.

    ![Árvore de servidores do acelerador da solução Fábrica Ligada][cf-img-server-tree]

5. Clique com o botão direito do rato num item para ler, escrever, publicar ou chamar esse nó. As ações à sua disposição dependem das suas permissões e dos atributos do nó. A opção de leitura apresenta um painel de contexto que mostra o valor do nó específico. A opção de escrita apresenta um painel de contexto, onde pode introduzir um valor novo. A opção de chamada apresenta um nó onde pode introduzir os parâmetros da chamada.

## <a name="publish-a-node"></a>Publicar um nó

Quando procura um *servidor OPC UA simulado*, também pode optar por publicar nós novos. Pode analisar a telemetria desses nós na solução. Estes *servidores OPC UA simulados* permitem testar facilmente o acelerador da solução sem ser necessário implementar dispositivos físicos reais.

1. Navegue para um nó na árvore do browser de servidores OPC UA que quer publicar.

2. Clique com o botão direito do nó.

3. Escolha **Publicar**.

    ![Nó de publicações de Fábrica Ligada][cf-img-publish-node]

4. É mostrado um painel de contexto que lhe diz que a publicação foi bem-sucedida. O nó aparece na vista de nível de estação, com uma marca de visto junto ao mesmo.

    ![Publicação pré-configurada de Fábrica Ligada realizada com êxito][cf-img-publish-success]

## <a name="command-and-control"></a>Comando e controlo

A Fábrica Ligada permite-lhe comandar e controlar os seus dispositivos industriais diretamente a partir da cloud. Pode utilizar esta funcionalidade para responder a alarmes gerados pelos dispositivos. Por exemplo, pode enviar comandos da cloud para o dispositivo. Os comandos disponíveis encontram-se no nó **StationCommands**, na árvore do browser de servidores OPC UA. Neste cenário, vai abrir uma válvula de saída de pressão na estação de montagem de uma linha de produção em Munique. Para utilizar a funcionalidade de comando e controlo, tem de ter a função **Administrador** na implementação do acelerador da solução.

1. Navegue para o nó **StationCommands** na árvore do browser de servidores OPC UA.

2. Escolha o comando que pretende utilizar.

3. Clique com o botão direito do nó.

4. Escolha **Chamar**.

    ![Comando de chamada do acelerador da solução Fábrica Ligada][cf-img-call-command]

5. É apresentado um painel de contexto que o informa do método que está prestes a chamar e de eventuais detalhes de parâmetros, se aplicável.

6. Escolha **Chamar**.

    ![Contexto de chamada do acelerador da solução Fábrica Ligada][cf-img-call-context]

7. O painel de contexto é atualizado, para o informar de que a chamada do método foi bem-sucedida. Para verificar se a chamada teve êxito, pode ler o valor do nó de pressão que foi atualizado como resultado da mesma.

    ![Chamada do acelerador da solução Fábrica Ligada realizada com êxito][cf-img-call-success]

## <a name="behind-the-scenes"></a>Nos bastidores

Quando implementa um acelerador da solução, o processo de implementação cria vários recursos na subscrição do Azure que selecionou. Pode ver estes recursos no [Portal][lnk-portal] do Azure. O processo de implementação cria um **grupo de recursos** com um nome baseado no nome que escolheu para o acelerador da solução:

![Acelerador da solução no portal do Azure][img-cf-portal]

Pode ver as definições de cada recurso, selecionando-o na lista de recursos no grupo de recursos.

Também pode ver o código fonte do acelerador da solução. O código fonte do acelerador da solução Fábrica Ligada encontra-se no repositório do GitHub [azure-iot-connected-factory][lnk-cfgithub]:

Quando tiver terminado, pode eliminar o acelerador da solução da sua subscrição do Azure no site [azureiotsuite.com][lnk-azureiotsuite]. Este site permite-lhe eliminar facilmente todos os recursos que foram aprovisionados quando criou o acelerador da solução.

> [!NOTE]
> Para garantir que elimina tudo o que está relacionado com o acelerador da solução, elimine-o no site [azureiotsuite.com][lnk-azureiotsuite]. Não elimine o grupo de recursos no portal.

## <a name="next-steps"></a>Passos Seguintes

Agora que implementou um acelerador da solução funcional, pode ler os artigos seguintes para continuar a introdução aos aceleradores de soluções de IoT:

* [Apresentação do acelerador da solução Fábrica Ligada][lnk-rm-walkthrough]
* [Ligar o dispositivo ao acelerador da solução Fábrica Ligada][lnk-connect-cf]
* [Permissões no site azureiotsuite.com][lnk-permissions]

[img-cf-home]:media/iot-suite-connected-factory-overview/cf-dashboard.png
[img-launch-solution]: media/iot-suite-connected-factory-overview/launch-cf.png
[cf-img-menu]: media/iot-suite-connected-factory-overview/cf-dashboard-menu.png
[cf-img-factories]:media/iot-suite-connected-factory-overview/cf-dashboard-factory.png
[cf-img-map]:media/iot-suite-connected-factory-overview/cf-dashboard-map.png
[cf-img-alerts]:media/iot-suite-connected-factory-overview/cf-dashboard-alerts.png
[cf-img-oee]:media/iot-suite-connected-factory-overview/cf-dashboard-oee.png
[cf-img-kpi]:media/iot-suite-connected-factory-overview/cf-dashboard-kpi.png
[cf-img-tsi-visualization]:media/iot-suite-connected-factory-overview/cf-dashboard-tsi.png
[cf-img-tsi-explorer]:media/iot-suite-connected-factory-overview/tsi-explorer.png
[cf-img-server-browser]: media/iot-suite-connected-factory-overview/cf-dashboard-browser.png
[cf-img-server-choice]: media/iot-suite-connected-factory-overview/cf-select-server.png
[cf-img-server-tree]:media/iot-suite-connected-factory-overview/cf-server-tree.png
[cf-img-publish-node]:media/iot-suite-connected-factory-overview/cf-publish-node1.png
[cf-img-publish-success]:media/iot-suite-connected-factory-overview/cf-publish-success.png
[cf-img-call-command]:media/iot-suite-connected-factory-overview/cf-command-and-control-call.png
[cf-img-call-context]:media/iot-suite-connected-factory-overview/cf-command-and-control-call-button.png
[cf-img-call-success]:media/iot-suite-connected-factory-overview/cf-command-and-control-succeed.png
[img-cf-portal]:media/iot-suite-connected-factory-overview/cf-resource-group.png
[cf-img-alert-filter]:media/iot-suite-connected-factory-overview/cf-filter.png
[cf-img-alert-filter-funnel]:media/iot-suite-connected-factory-overview/cf-filter-funnel.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-preconfigured-solutions]: iot-suite-what-are-solution-accelerators.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com
[lnk-portal]: http://portal.azure.com/
[lnk-cfgithub]: https://github.com/Azure/azure-iot-connected-factory
[lnk-rm-walkthrough]: iot-suite-connected-factory-sample-walkthrough.md
[lnk-connect-cf]: iot-suite-connected-factory-gateway-deployment.md
[lnk-permissions]: iot-suite-v1-permissions.md
[lnk-faq]: iot-suite-v1-faq.md