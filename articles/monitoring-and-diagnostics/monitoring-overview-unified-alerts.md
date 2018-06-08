---
title: Unified alertas no Monitor do Azure | Microsoft Docs
description: Descrição de alertas unificadas no Azure, que lhe permitem gerir alertas e alertas de regras nos vários serviços do Azure.
author: manishsm-msft
manager: kmadnani1
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: ''
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2018
ms.author: mamit,bwren
ms.custom: ''
ms.openlocfilehash: 699dff42846ee1f9d42980feca55d8a79e2514e3
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/07/2018
ms.locfileid: "34839848"
---
# <a name="unified-alerts-in-azure-monitor"></a>Unificada alertas no Monitor do Azure

## <a name="overview"></a>Descrição geral

> [!NOTE]
>  Uma nova experiência de alerta unificada que permite-lhe gerir alertas a partir da várias subscrições e apresenta o alerta Estados e grupos inteligentes está atualmente disponível na pré-visualização pública. Consulte o [última secção deste artigo](#enhanced-unified-alerts-experience-public-preview) para uma descrição desta experiência de avançada e o processo para ativado.


Este artigo descreve a experiência de alerta unificada no Monitor do Azure. O [experiência alerta anterior](monitoring-overview-alerts.md) está disponível a partir de **alertas (clássica)** opção no menu de Monitor do Azure. 

## <a name="features-of-the-unified-alert-experience"></a>Funcionalidades de experiência de alerta unificada

A experiência de unificada tem as seguintes vantagens a experiência de clássico:

-   **Melhor sistema notificação**: Unified utilizar alertas [grupos ação]( https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-action-groups), que são denominados grupos de ações que podem ser reutilizadas em vários alertas e notificações. 
- **Experiência de criação de Unified** - pode gerir alertas e registos de regras de alerta com base nas métricas, e a atividade iniciar sessão no Monitor do Azure, análise de registos e Application Insights num único local. 
- **Desencadeado de vista de alertas de análise de registos no portal do Azure** -ver alertas a partir de análise de registos com outros alertas a partir de outras origens no portal do Azure. Anteriormente estes foram num portal separado.
- **Separação de alertas de Fired e regras de alerta** -as regras agora são distinguidas de alertas do alerta. Uma regra de alerta é as definições de uma condição que aciona um alerta. Um alerta é uma instância de acionadas uma regra de alerta.
- **Fluxo de trabalho melhor** -o alerta unificado experiência de criação orienta-o processo de configuração de uma regra de alerta.
 
Alertas métricas tem as seguintes melhorias sobre alertas de métricas clássicas:

-   **Melhorado latência**: métricas alertas podem ser executados com uma frequência de cada um minuto. Alertas de métricas clássicas sempre executam com uma frequência de 5 minutos. Alertas de registo ainda têm um atraso mais do que um minuto devido ao tempo é necessário para os registos de inserção. 
-   **Suporte para as métricas multidimensionais**: podem alertá nas métricas dimensional, permitindo-lhe monitorizar uma instância específica da métrica.
-   **Mais controlo sobre condições métricas**: pode definir regras de alertas mais rica que suportam os valores máximo, mínimos, médios e totais de métricas de monitorização.
-   **Combinar a monitorização de várias métricas**: pode monitorizar as métricas de até duas com uma única regra. Um alerta é acionado se ambas as métricas infringir os respetivos limiares para o período de tempo especificado.
-   **Métricas de registos** (limitado pré-visualização pública): algum registo entrem em análise de registos de dados podem agora ser extraído e convertido métricas de Monitor do Azure e, em seguida, alertado em tal como outras métricas. 


## <a name="alert-rules"></a>Regras de alerta
A experiência de alertas unificada utiliza os seguintes conceitos para separar as regras de alerta de alertas ao unificá a experiência de criação em diferentes tipos de alerta.

| Item | Definição |
|:---|:---|
| Regra de alerta | Definição da condição para criar um alerta. É composto por um _recurso de destino_, _sinal_, _critérios_, e _lógica_. Uma regra de alerta só está ativa, se estiver a ser um _ativada_ estado.
| Recurso de Destino | Define os recursos específicos e sinalizar disponível para alertas. Um destino pode ser qualquer recurso do Azure.<br>Exemplos: máquina virtual, conta de armazenamento, o conjunto de dimensionamento de máquina virtual, área de trabalho de análise de registos, recurso do Application Insights |
| Sinal | Origem de dados emitidos pelo recurso de destino. Os tipos de sinal suportados são *métrica*, *registo de atividade*, *Application Insights*, e *registo*. |
| Critérios | Combinação de _sinal_ e _lógica_ aplicada no recurso de destino.<br>Exemplos: Percentagem de CPU > 70%, tempo de resposta do servidor > 4 ms, contagem de resultados de um registo de consultar > 100 etc. |
| Lógica | Lógica definida pelo utilizador para verificar se o sinal está dentro do esperado/valores de intervalo. |
| Ação | Ação a executar quando o alerta é acionado. Várias ações podem ocorrer quando um alerta é acionado. Estes alertas suportam os grupos de ação.<br>Exemplos: relacionada um endereço de e-mail, ao chamar um URL do webhook. |
| Condição do Monitor | Indica se a condição que é criado um alerta de métrico subsequentemente foi resolvida. Regras de alerta de métricas de exemplo uma métrica específica em intervalos regulares. Se os critérios de regra de alerta for cumprida, é criado um novo alerta com uma condição de Fired.  Quando a métrica é amostragem novamente, se os critérios ainda for cumprida, em seguida, nada acontece.  Se os critérios não for cumprida embora, a condição do alerta é alterada como resolvido. Da próxima vez que os critérios forem satisfeitos, em seguida, é criado um alerta outro com uma condição de Fired. |


## <a name="alert-pages"></a>Páginas de alerta
Alertas unificadas fornecem um único local para ver e gerir todos os alertas do Azure. As secções seguintes descrevem as funções de cada página individuais da experiência unificada.

### <a name="alerts-overview-page"></a>Página de descrição geral de alertas
**Alertas** página Descrição Geral mostra um resumo de agregados de todos os alertas fired e o total ativado regras de alertas. Alterar as subscrições ou parâmetros de filtro atualiza as agregações e os alertas desencadeado lista.

 ![Descrição geral dos alertas](./media/monitoring-overview-unified-alerts/alerts-preview-overview2.png) 

### <a name="alert-rules-management"></a>Gestão de regras de alertas
**Regras** é uma única página para gerir todas as regras de alerta nas suas subscrições do Azure. Apresenta uma lista de todas as regras de alerta e podem ser ordenado com base nos recursos de destino, os grupos de recursos, o nome da regra ou estado. Regras de alertas também podem ser editado nd ativadas ou desativadas a partir desta página.

 ![regras de alertas](./media/monitoring-overview-unified-alerts/alerts-preview-rules.png)


## <a name="creating-an-alert-rule"></a>Criar uma regra de alerta
Alertas podem ser criados de forma consistente, independentemente do serviço de monitorização ou assinalam o tipo. Desencadeado todos os alertas e detalhes relacionados estão disponíveis na página único.
 
Criar uma nova regra de alerta com os seguintes três passos:
1. Escolha o _destino_ para o alerta.
1. Selecione o _sinal_ dos sinais disponíveis para o destino.
1. Especifique o _lógica_ a aplicar aos dados do sinal.
 
Este processo de criação simplificado já não exige que o utilizador saber a origem de monitorização ou sinais suportados antes de selecionar um recurso do Azure. A lista de sinais disponíveis são automaticamente filtrado com base no recurso de destino seleccionado e orienta-o de que define a lógica da regra de alerta.

Pode saber mais sobre como criar regras de alertas no [criar, ver e gerir alertas utilizando o Monitor de Azure](monitor-alerts-unified-usage.md).

Alertas estão disponíveis no Azure vários serviços de monitorização. Para obter informações sobre como e quando utilizar cada um destes serviços, consulte [recursos e aplicações de monitorização do Azure](./monitoring-overview.md). A tabela seguinte fornece uma lista dos tipos de regras de alertas disponíveis através do Azure e o que é atualmente suportado pela experiência de alerta unificada.

| **Monitor de origem** | **Tipo de sinal**  | **Descrição** | 
|-------------|----------------|-------------|
| Monitor do Azure | Métrica  | Também denominado [quase em tempo real de alertas métricas](monitoring-near-real-time-metric-alerts.md), suportam a avaliar condições métricas com uma frequência de 1 minuto e permitir regras de métricas de múltiplos métricos e multidimensionais. Uma lista de tipos de recurso suportados está disponível no [alertas métricas mais recentes para serviços do Azure no portal do Azure](monitoring-near-real-time-metric-alerts.md#metrics-and-dimensions-supported).<br>[Alertas de métricas clássicas](monitoring-overview-alerts.md) não são suportados na nova experiência de alertas. Pode encontrá-los em alertas (clássica) no portal do Azure. Os alertas clássicos suportam alguns tipos de métricas que ainda não terem sido movidos para os alertas mais recentes. Para obter uma lista completa, consulte [suportado métricas](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-supported-metrics). |
| Log Analytics | Registos  | Receber notificações ou executar ações automatizadas quando uma consulta de pesquisa de registo cumpra determinados critérios. Os alertas na análise de registos são [que está a ser copiados para a nova experiência](monitoring-alerts-extend.md). A [de pré-visualização de *registos de análise de registos como métricas* ](monitoring-alerts-extend-tool.md) está disponível. A pré-visualização permite-lhe tirar alguns tipos de registos e convertê-los à métrica, onde, em seguida, podem alertá nos mesmos utilizando a nova experiência de alerta. A pré-visualização é útil se tiver registos não do Azure que pretende obter juntamente com nativas métricas de Monitor do Azure. |
| Registos de Atividade | Registo de Atividades | Contém os registos de todas as ações de criação, atualização e eliminação criados de destino selecionado. |
| Service Health | Registo de Atividades  | Não é suportada nos alertas unificadas. Consulte [criam alertas de registo de atividade em notificações de serviço](monitoring-activity-log-alerts-on-service-notifications.md).  |
| Application Insights | Registos  | Contém registos com os detalhes de desempenho da sua aplicação. Utilizar consulta de análise, pode definir as condições de ações a executar com base nos dados de aplicação. |
| Application Insights | Métrica | Não é suportada nos alertas unificadas. Consulte [alertas métricas]. (.. /Application-insights/App-insights-alerts.MD) |
| Application Insights | Testes de disponibilidade da Web | Não é suportada nos alertas unificadas.  Consulte [alertas de teste Web](../application-insights/app-insights-monitor-web-app-availability.md). Está disponível para qualquer Web site instrumentada para enviar dados para o Application Insights. Receba uma notificação quando a disponibilidade ou capacidade de resposta de um Web site é inferior a expetativas. |

## <a name="enhanced-unified-alerts-public-preview"></a>Alertas unificadas avançadas (pré-visualização pública)
> [!NOTE]
>  A funcionalidade nesta secção estará disponível brevemente. -Pode não aparecer na sua versão do portal ainda. 

Uma experiência melhorada alertas unificada foi libertada em pré-visualização pública para o Monitor de Azure 1 de Junho de 2018. Esta experiência baseia-se as vantagens de [unified alertas](#overview) lançadas Março de 2018 e fornece a capacidade de gerir e agregar alertas individuais para além de modificar o estado de alerta. Esta secção descreve as novas funcionalidades e como navegar nas páginas novas alertas no portal do Azure.

### <a name="features-enhanced-unified-alerts"></a>Funcionalidades avançadas unified alertas

A nova experiência fornece as seguintes funcionalidades não estão disponíveis na experiência de unificada clássica:

- **Ver alertas nas subscrições** -agora pode ver e gerir instâncias individuais de alertas em várias subscrições numa única vista.
- **Gerir o estado de alertas** -alertas agora tem um Estado que indica se o foram confirmadas para fechado.
- **Organizar os alertas com grupos inteligente** -inteligente grupos automaticamente agrupar alertas relacionados com o para que possa geri-los como um conjunto em vez de individualmente.

### <a name="enable-enhanced-unified-alerts"></a>Ativar alertas de unificada avançadas
Ative a nova experiência de alerta unificada clicando na faixa na parte superior da página de alertas. Este processo cria um arquivo de alerta que inclui os últimos 30 dias de alertas é desencadeados nos vários serviços suportados. Assim que a nova experiência estiver ativada, pode mudar novamente e estabelecido entre a experiência de antiga e clicando na faixa.

> [!NOTE]
>  Pode demorar alguns minutos para que a nova experiência inicialmente ativado.

![Faixa](media/monitoring-overview-unified-alerts/opt-in-banner.png)

Todas as subscrições que tem acesso a serão inscrito quando ativar a nova experiência. Apesar da subscrição completa é ativada, apenas os utilizadores que selecionou a nova experiência irão conseguir vê-la. Outros utilizadores com acesso à subscrição tem de ativar a experiência em separado.

Ativar a nova experiência de alerta não afeta a configuração de grupos de ação ou notificações nas suas regras de alerta. Apenas as alterações da forma que ver e gerir desencadeadas instâncias dos alertas no portal do Azure.

### <a name="smart-groups"></a>Grupos inteligentes
Grupos inteligentes reduzem o ruído, permitindo-lhe gerir alertas relacionados, como uma única unidade em vez de gerir alertas individuais. Pode ver os detalhes dos grupos inteligentes e definir o estado semelhante a um alerta. Cada alerta é um membro de um e apenas um grupo inteligente.

Grupos inteligentes são criados automaticamente através de machine learning combinar alertas relacionados que representam um único problema. Quando é criado um alerta, o algoritmo adiciona-o para um novo grupo inteligente ou um grupo de inteligente existente com base nas informações como padrões históricos, semelhança das propriedades e semelhança da estrutura. Atualmente, o algoritmo considera apenas alertas do serviço de monitor do mesmo numa subscrição. Grupos inteligentes podem reduzir até 99% contra ruído de alertas através esta consolidação. Pode ver o motivo que os alertas foram incluídos num grupo na página de detalhes do grupo de Smart.

O nome de um grupo inteligente é o nome do seu primeiro alerta. Não é possível criar ou mudar o nome de um grupo inteligente.


### <a name="alert-states"></a>Estados de alerta
Alertas unificadas avançadas introduzem o conceito de estado de alerta. Pode definir o estado de um alerta para especificar onde se encontram no processo de resolução.  Quando é criado um alerta, tem um Estado de *novo*. Pode alterar o estado quando tiver confirmadas um alerta e quando tiver fechado. Quaisquer alterações de estado são armazenadas no histórico do alerta.

São suportados os seguintes Estados de alerta.

| Estado | Descrição |
|:---|:---|
| Novo | Apenas o problema foi detetado e ainda não foi revisto. |
| Confirmado | Um administrador tem de rever o alerta e começou a funcionar no mesmo. |
| Fechado | O problema foi resolvido. Depois de um alerta ter sido fechado, pode abri-lo novamente os meus alterá-lo para outro Estado. |

O estado de um alerta é diferente de condição do monitor. Regras de alerta métricas podem definir um alerta para uma condição de _resolvido_ quando a condição de erro já não for cumprida. Estado do alerta é definido pelo utilizador e é independente da condição do monitor. Apesar do sistema pode definir a condição do monitor para resolver, o estado do alerta não é alterado até que o utilizador altera-lo.

#### <a name="changing-the-state-of-an-alert-or-smart-group"></a>Alteração do Estado de um grupo de alerta ou inteligente
Pode alterar o estado de um alerta individual ou gerir vários alertas em conjunto, definindo o estado de um grupo inteligente.

Alterar o estado de um alerta clicando em **alterar o estado de alerta** na vista de detalhes do alerta ou alterar o estado de um grupo inteligente clicando **alterar o estado do grupo inteligente** na sua vista de detalhes. Pode alterar o estado de vários itens em simultâneo, selecionando-los numa vista de lista e clicando **alteração de estado** na parte superior da página. Em ambos os casos, selecione um novo estado na lista pendente e, opcionalmente, forneça um comentário. Se estiver a alterar um único item, em seguida, tem também uma opção para aplicar as alterações mesmas para todos os alertas no grupo de inteligente.

![Alterar estado](media/monitoring-overview-unified-alerts/change-tate.png)

### <a name="alerts-page"></a>Página de alertas
A página predefinida de alertas fornece um resumo dos alertas que são criadas dentro de uma janela de tempo específico. Apresenta o total de alertas para cada gravidade com colunas que identifica o número total de alertas em cada Estado para cada gravidade. Clique em qualquer um das gravidades para abrir o [todos os alertas](#all-alerts-page) página filtrado por essa gravidade.

![Página de alertas](media/monitoring-overview-unified-alerts/alerts-page.png)

Pode filtrar esta vista selecionando os valores nas dropdowns na parte superior da página.

| Coluna | Descrição |
|:---|:---|
| Subscrição | Selecione até 5 subscrições do Azure. Apenas alertas nas subscrições selecionadas estão incluídas na vista. |
| Grupo de Recursos | Selecione um grupo de recursos única. Apenas alertas com destinos no grupo de recursos selecionado estão incluídas na vista. |
| Intervalo de Tempo | Apenas os alertas desencadeados o período de tempo selecionado serão incluídos na vista. Os valores suportados são decorridos desde a hora, após 24 horas, últimos 7 dias e últimos 30 dias. |

Clique nos seguintes valores no topo da página de alertas para abrir outra página.

| Valor | Descrição |
|:---|:---|
| Alertas Totais | Número total de alertas que correspondem aos critérios seleccionados. Clique neste valor para abrir a vista de todos os alertas com nenhum filtro. |
| Grupos inteligentes | Número total de inteligentes grupos criados a partir de alertas que correspondem aos critérios seleccionados. Clique neste valor para abrir a lista de grupos inteligente na vista de todos os alertas.
| Regras de Alertas Totais | Número total de regras de alertas no grupo de recursos e de subscrição selecionado. Clique neste valor para abrir a vista de regras filtrada nas subscrições selecionadas e o grupo de recursos.


### <a name="all-alerts-page"></a>Página de todos os alertas 
A página de todos os alertas permite-lhe ver uma lista de alertas que foram criadas dentro da janela de tempo selecionado. Pode optar por visualizar uma lista dos alertas individuais ou uma lista dos grupos de inteligentes, que contém os alertas. Clique na faixa na parte superior da página para alternar entre vistas.

![Página de todos os alertas](media/monitoring-overview-unified-alerts/all-alerts-page.png)

Pode filtrar a vista, selecionando os seguintes valores nas dropdowns na parte superior da página.

| Coluna | Descrição |
|:---|:---|
| Subscrição | Selecione até 5 subscrições do Azure. Apenas alertas nas subscrições selecionadas estão incluídas na vista. |
| Grupo de Recursos | Selecione um grupo de recursos única. Apenas alertas com destinos no grupo de recursos selecionado estão incluídas na vista. |
| Tipo de Recurso | Selecione um ou mais tipos de recursos. Apenas alertas com destinos do tipo selecionado estão incluídas na vista. Esta coluna só está disponível depois de um grupo de recursos foi especificado. |
| Recurso | Selecione um recurso. Apenas alertas com esse recursos como um destino estão incluídas na vista. Esta coluna só está disponível uma vez que foi especificado um tipo de recurso. |
| Gravidade | Selecione uma gravidade de alerta ou *todos os* para incluir os alertas de todas as gravidades. |
| Condição do Monitor | Selecione uma condição do monitor ou *todos os* para incluir os alertas de condições. |
| Estado de Alerta | Selecione um alerta de estado ou selecione *todos os* para incluir os alertas de Estados. |
| Monitorizar Serviço | Selecionar um serviço ou selecione *todos os* para incluir todos os serviços. Apenas os alertas criados pelas regras que o serviço a utilizar como um destino estão incluídos. |
| Intervalo de Tempo | Apenas os alertas desencadeados o período de tempo selecionado serão incluídos na vista. Os valores suportados são decorridos desde a hora, após 24 horas, últimos 7 dias e últimos 30 dias. |

Clique em **colunas** na parte superior da página para selecionar as colunas a apresentar. Pode remover quaisquer colunas, exceto para 

### <a name="alert-detail-page"></a>Página de detalhes de alerta
A página de detalhes de alerta é apresentada ao clicar num alerta. Disponibiliza detalhes do alerta e permite-lhe alterar o estado.

![Detalhes de alerta](media/monitoring-overview-unified-alerts/alert-detail.png)

A página de detalhes de alerta inclui as seguintes secções.

| Section | Descrição |
|:---|:---|
| Essentials | Apresenta as propriedades e outras informações importantes sobre o alerta. |
| Histórico | Apresenta uma lista de cada ação tomada pelo alerta e quaisquer alterações efetuadas ao alerta. Isto está limitado a alterações de estado. |
| Grupo de smart | Informações sobre o grupo inteligente o alerta estão incluídas no. O **contagem de alertas** refere-se ao número de alertas incluídas no grupo de inteligente. Isto inclui os outros alertas que estão incluídos no mesmo grupo inteligente mesmo que foram criados nos últimos 30 dias.  Isto é, independentemente do filtro de tempo na página de lista de alertas. Clique num alerta para ver os respetivos detalhes. |
| Obter mais detalhes | Mostra informações contextuais para o alerta que é normalmente específica para o tipo de origem que criou o alerta adicionais. |


### <a name="smart-group-detail-page"></a>Página de detalhes do grupo de smart
A página de detalhes do grupo inteligente é apresentada ao clicar num grupo de smart. Disponibiliza detalhes do grupo inteligente, incluindo raciocínio utilizado para criar o grupo e permite-lhe alterar o estado.
 
![Detalhes do grupo de smart](media/monitoring-overview-unified-alerts/smart-group-detail.png)


A página de detalhes do grupo inteligente inclui as seguintes secções.

| Section | Descrição |
|:---|:---|
| Alertas | Apresenta os alertas individuais que estão incluídos no grupo de inteligente. Clique num alerta para abrir a página de detalhes do alerta. |
| Histórico | Apresenta uma lista de cada ação tomada pelo grupo de inteligente e quaisquer alterações efetuadas ao mesmo. Esta é atualmente limitada a alterações de estado e as alterações na associação de alerta. |

## <a name="next-steps"></a>Passos Seguintes
- [Saiba como utilizar a nova experiência de alertas para criar, ver e gerir alertas](monitor-alerts-unified-usage.md)
- [Saiba mais sobre alertas de registo na experiência de alertas](monitor-alerts-unified-log.md)
- [Saiba mais sobre alertas de métricas de experiência de alertas](monitoring-near-real-time-metric-alerts.md)
- [Saiba mais sobre alertas de registo de atividade numa experiência de alertas](monitoring-activity-log-alerts-new-experience.md)
