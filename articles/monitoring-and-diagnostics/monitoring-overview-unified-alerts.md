---
title: Alertas unificadas no Azure Monitor
description: Descrição de alertas unificadas no Azure que permitem-lhe gerir alertas e regras de alertas em serviços do Azure.
author: manishsm-msft
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 06/07/2018
ms.author: mamit
ms.component: alerts
ms.openlocfilehash: 30b2d60868702c6113612668b8e4cf9975aa2c40
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/02/2018
ms.locfileid: "50962202"
---
# <a name="unified-alerts-in-azure-monitor"></a>Alertas unificadas no Azure Monitor

## <a name="overview"></a>Descrição geral

> [!NOTE]
>  Uma nova experiência de alerta unificada que permite-lhe gerir alertas a partir da várias subscrições e apresenta o alerta de Estados e inteligentes grupos está atualmente disponível em pré-visualização pública. Consulte a última seção deste artigo para obter uma descrição desta avançada experiência e o processo para ativá-la.


Este artigo descreve a experiência unificada de alerta no Azure Monitor. O [experiência anterior de alerta](monitoring-overview-alerts.md) está disponível a partir do **alertas (clássico)** opção no menu do Azure Monitor. 

## <a name="features-of-the-unified-alert-experience"></a>Funcionalidades de experiência unificada de alerta

A experiência unificada tem as seguintes vantagens sobre a experiência clássica:

-   **Melhor o sistema de notificação**: [grupos de ação](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-action-groups) são denominados grupos de notificações e ações que podem ser reutilizadas em vários alertas. 
- **Experiência de criação de unificação**: alertas e regras de alerta de métricas, registos e registos de atividades no Azure Monitor, no Log Analytics e Application Insights podem ser geridas num único local. 
- **Vista acionados alertas do Log Analytics no portal do Azure**: alertas do Log Analytics agora podem ser vistas com alertas a partir de outras origens no portal do Azure. Anteriormente, os alertas a partir de outras fontes foram num portal separado.
- **Separação de alertas acionados e as regras de alerta**: regras de alerta agora são distinguidas dos alertas. Uma regra de alerta é a definição de uma condição que aciona um alerta. Um alerta é uma instância de uma acionadas de regra de alerta.
- **Fluxo de trabalho melhor**: O alerta unificado experiência de criação orienta-o ao longo do processo de configuração de uma regra de alerta.
 
Alertas de métricas têm as seguintes melhorias sobre alertas de métricas clássicas:

-   **Melhorada a latência**: alertas de métricas podem ser executados com uma frequência de uma vez por minuto. Alertas de métricas clássicas sempre executam com uma frequência de uma vez a cada 5 minutos. Alertas de registo ainda têm um atraso a mais de um minuto devido ao tempo necessário para ingerir os registos. 
-   **Suporte para métricas multidimensionais**: pode alerta relativamente a métricas dimensionais, o que significa que pode monitorizar uma instância específica da métrica.
-   **Mais controlo sobre condições de métricas**: pode definir regras de alerta mais sofisticadas que suportam os valores máximos, mínimo, médios e total de métricas de monitorização.
-   **Combinados a monitorização de várias métricas**: pode monitorizar métricas de até duas com uma única regra. Um alerta é acionado se ambas as métricas de violações seus respectivos limites para o período de tempo especificado.
-   **Métricas de logs** (pré-visualização pública limitada): algum registo de dados que vão para o Log Analytics podem agora ser extraído e convertido em métricas do Azure Monitor e, em seguida, alertado em tal como outras métricas. 


## <a name="alert-rules"></a>Regras de alerta
A experiência de alertas unificada utiliza os seguintes conceitos para separar as regras de alerta de alertas ao unificar a experiência de criação para diferentes tipos de alerta.

| Item | Definição |
|:---|:---|
| Regra de alerta | Definição da condição para criar um alerta. Uma regra de alerta é composta por um _recurso de destino_, _sinal_, _critérios_, e _lógica_. Uma regra de alerta é ativado somente se ele estiver num _ativada_ estado.
| Recurso de destino | Define os recursos específicos e sinais que estão disponíveis para alertas. Um destino pode ser qualquer recurso do Azure.<br><br>Exemplos: máquina virtual, conta de armazenamento, o conjunto de dimensionamento de máquina virtual, área de trabalho do Log Analytics, recurso do Application Insights |
| Sinal | Origem de dados emitidos pelo recurso de destino. Tipos de sinal suportados são *métrica*, *registo de atividades*, *Application Insights*, e *Log*. |
| Critérios | Combinação de _sinal_ e _lógica_ aplicado num recurso de destino.<br><br>Exemplos: Percentagem da CPU > 70%, tempo de resposta do servidor > 4 ms, contagem de resultados de um registo de consultar > 100 e assim por diante |
| Lógica | Lógica definida pelo utilizador para verificar se o sinal está dentro do esperado/valores ou de intervalo. |
| Ação | Ação a executar quando o alerta é acionado. Várias ações podem ocorrer quando um alerta é acionado. Estes alertas suportam os grupos de ação.<br><br>Exemplos: enviar por email para enviar um e-mail endereço, chamando uma URL de webhook |
| Condição do monitor | Indica se a condição que é criado um alerta de métrica foi resolvida. Regras de alerta de métrica de exemplo uma métrica em particular em intervalos regulares. Se os critérios na regra de alerta são cumpridos, um novo alerta com a, em seguida, é criado com uma condição de "disparado".  Quando a métrica é objeto de amostragem mais uma vez, se os critérios ainda são cumpridos, em seguida, nada acontece.  Se os critérios não for cumprida, em seguida, a condição do alerta é alterada para "resolvido". Da próxima vez que os critérios são cumpridos, é criado outro alerta com uma condição de "disparado". |


## <a name="alert-pages"></a>Páginas de alerta
Alertas unificadas fornecem um único local para ver e gerir todos os seus alertas do Azure. As secções seguintes descrevem as funções de cada página individual da experiência unificada.

### <a name="alerts-overview-page"></a>Página de descrição geral de alertas
O **alertas** página Visão Geral mostra um resumo agregado de todos os alertas acionados e o número total de regras de alerta de ativadas. Alterar as subscrições ou parâmetros de filtro atualiza as agregações e os alertas acionados lista.

 ![Descrição geral dos alertas](./media/monitoring-overview-unified-alerts/alerts-preview-overview2.png) 

### <a name="alert-rules-management"></a>Gestão de regras de alerta
**Regras** é uma única página para gerir todas as regras de alerta nas suas subscrições do Azure. Ele apresenta uma lista de todas as regras de alerta e pode ser ordenado com base nos recursos de destino, grupos de recursos, o nome da regra ou estado. Regras de alerta também podem ser editadas, ativadas ou desativadas a partir desta página.

 ![regras de alertas](./media/monitoring-overview-unified-alerts/alerts-preview-rules.png)


## <a name="create-an-alert-rule"></a>Criar uma regra de alerta
Alertas podem ser criados de forma consistente, independentemente do serviço de monitoramento ou sinalizam tipo. Acionado todos os alertas e detalhes relacionados estão disponíveis na página única.
 
Criar uma nova regra de alerta com os seguintes três passos:
1. Escolha o _destino_ para o alerta.
1. Selecione o _sinal_ partir os sinais disponíveis para o destino.
1. Especifique a _lógica_ para ser aplicadas a dados a partir do sinal.
 
Este processo de criação simplificado já não necessita de saber a origem de monitorização ou sinais que são suportados antes de selecionar um recurso do Azure. A lista de sinais disponíveis automaticamente é filtrada com base no recurso de destino que selecionou, e ele orienta por meio de definir a lógica de regra de alerta.

Pode saber mais sobre como criar regras de alerta no [criar, ver e gerir alertas ao utilizar o Azure Monitor](alert-metric.md).

Alertas estão disponíveis em todo o Azure vários serviços de monitorização. Para obter informações sobre como e quando usar cada um desses serviços, consulte [aplicações de monitorização do Azure e recursos](../azure-monitor/overview.md). A tabela seguinte fornece uma listagem dos tipos de regras de alerta que estão disponíveis em todo o Azure. Também apresenta o que é atualmente suportado pela experiência unificada de alerta.

| **Origem de monitor** | **Tipo de sinal**  | **Descrição** | 
|-------------|----------------|-------------|
| Azure Monitor | Métrica  | Também denominado [alertas quase em tempo real de métricas](monitoring-near-real-time-metric-alerts.md), que suporta a avaliar as condições de métricas com uma frequência de uma vez a uma minuto e permitir para as regras de métrica de várias métricas e multidimensionais. Está disponível numa lista de tipos de recurso suportados [mais recente alertas de métricas para os serviços do Azure no portal do Azure](monitoring-near-real-time-metric-alerts.md#metrics-and-dimensions-supported).<br>[Alertas de métricas clássicas](monitoring-overview-alerts.md) não são suportados na nova experiência de alertas. Pode encontrá-los em alertas (clássico) no portal do Azure. Os alertas clássicos suportam alguns tipos de métricos que ainda não foram movidos para os alertas mais recentes. Para obter uma lista completa, consulte [suportado métricas](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-supported-metrics). |
| Log Analytics | Registos  | Receber notificações ou executar ações automatizadas, quando uma consulta de pesquisa de registo atende a certos critérios. Os alertas no Log Analytics são [que está a ser copiados para a nova experiência](monitoring-alerts-extend.md). R [pré-visualização dos *registos do Log Analytics como métricas* ](monitoring-alerts-extend-tool.md) está disponível. A pré-visualização permite-lhe tirar determinados tipos de registos e convertê-los em métricas, onde pode, em seguida, alertar nos mesmos utilizando a nova experiência de alerta. A pré-visualização é útil se tiver registos não Azure que pretende obter juntamente com métricas de nativas do Azure Monitor. |
| Registos de atividade | Registo de atividades | Contém os registos de todos os criar, atualizar e eliminar ações que foram criadas pelo destino selecionado. |
| Estado de funcionamento do serviço | Registo de atividades  | Não é suportada nos alertas unificadas. Ver [criar alertas do registo de atividade nas notificações do serviço](monitoring-activity-log-alerts-on-service-notifications.md).  |
| Application Insights | Registos  | Contém os registos com os detalhes de desempenho da sua aplicação. Ao utilizar a consulta de análise, pode definir as condições para as ações a executar com base nos dados de aplicativo. |
| Application Insights | Métrica | Não é suportada nos alertas unificadas. Ver [alertas de métricas](../application-insights/app-insights-alerts.md). |
| Application Insights | Testes de disponibilidade da Web | Não é suportada nos alertas unificadas.  Ver [alertas de teste Web](../application-insights/app-insights-monitor-web-app-availability.md). Disponível para qualquer Web site que foi instrumentada para enviar dados para o Application Insights. Receba uma notificação quando a disponibilidade ou a capacidade de resposta de um Web site é inferior expectativas. |

## <a name="enhanced-unified-alerts-public-preview"></a>Alertas unificadas avançadas (pré-visualização pública)

Uma experiência unificada de alertas avançados foi lançada em pré-visualização pública para o Azure Monitor de 1 de Junho de 2018. Esta experiência baseia-se nos benefícios da [unified alertas](#overview), que foram lançado em Março de 2018 e que fornecem a capacidade de gerir e agregar alertas individuais e modificar o estado do alerta. Esta secção descreve os novos recursos e como navegar nas páginas de alerta de novo no portal do Azure.

### <a name="enhanced-unified-alerts"></a>Alertas unificadas aprimoradas

A nova experiência disponibiliza as seguintes funcionalidades que não estão disponíveis na experiência unificada do clássica:

- **Ver alertas em várias subscrições**: agora pode ver e gerir instâncias individuais de alertas em várias subscrições numa única vista.
- **Gerir o estado de alertas**: alertas agora tem um Estado que indica se tiver sido confirmados como fechada.
- **Organizar os alertas com grupos inteligentes**: grupos inteligentes automaticamente agrupar relacionados com alertas para que possa geri-los como um conjunto em vez de individualmente.

### <a name="enable-enhanced-unified-alerts"></a>Ativar alertas de unificada aprimoradas
Ative a nova experiência de alertas unificada ao selecionar a faixa na parte superior da página de alertas. Este processo cria um arquivo de alerta que inclui os últimos 30 dias de alertas acionados em todos os serviços suportados. Depois da nova experiência está ativada, pode mudar e volta entre a experiência de nova e antiga selecionando esta faixa.

> [!NOTE]
>  Poderá demorar alguns minutos para que a nova experiência de ser inicialmente ativado.

![Faixa](media/monitoring-overview-unified-alerts/opt-in-banner.png)

Todas as subscrições que têm acesso a são inscritos ao ativar a nova experiência. Embora a subscrição completa é ativada, apenas os usuários que selecionarem a nova experiência podem vê-lo. Outros utilizadores com acesso à subscrição tem de ativar a experiência em separado.

Ativar a nova experiência de alerta não afeta a configuração de grupos de ação ou notificações nas suas regras de alerta. Ele só altera a maneira que ver e gerir instâncias de acionamento de alertas no portal do Azure.

### <a name="smart-groups"></a>Grupos inteligentes
Grupos inteligentes reduzem o ruído, permitindo-lhe gerir alertas relacionados, como uma única unidade em vez de alertas individuais. Pode ver os detalhes dos grupos inteligentes e definir o estado da mesma forma para como é possível com alertas. Cada alerta é um membro do grupo de smart de apenas um.

Grupos inteligentes são criados automaticamente com o machine learning para combinar os alertas relacionados que representam um único problema. Quando é criado um alerta, o algoritmo adiciona-o para um novo grupo inteligente ou um grupo existente inteligente, com base nas informações como padrões históricos, propriedades semelhantes e estrutura semelhante. 

Atualmente, o algoritmo apenas os alertas do serviço de monitor do mesmo numa subscrição. Grupos inteligentes podem reduzir a até 99% do ruído de alertas por meio desta consolidação. Pode ver o motivo que os alertas foram incluídos num grupo na página de detalhes do grupo inteligente.

O nome de um grupo inteligente é o nome do seu primeiro alerta. Não é possível criar ou mudar o nome de um grupo inteligente.


### <a name="alert-states"></a>Estados de alerta
Alertas unificadas aprimoradas introduzem o conceito de estado de alerta. Pode definir o estado de um alerta para especificar onde se encontra no processo de resolução. Quando é criado um alerta, tem o estado *New*. Pode alterar o estado quando reconhece um alerta e quando fechá-lo. Todas as alterações de estado são armazenadas no histórico do alerta.

São suportados os seguintes Estados de alerta.

| Estado | Descrição |
|:---|:---|
| Novo | O problema apenas foi detetado e ainda não foi revisto. |
| Confirmado | Um administrador tiver revisto o alerta e começou a trabalhar no mesmo. |
| Fechado | O problema foi resolvido. Após um alerta ter sido fechado, pode abri-lo novamente ao alterá-la para outro Estado. |

O estado de um alerta é diferente do que a condição do monitor. Regras de alerta de métrica podem definir um alerta a uma condição de _resolvido_ quando já não é cumprida a condição de erro. Estado de alerta é definido pelo utilizador e é independente da condição do monitor. Embora o sistema pode definir a condição do monitor para "resolver", o estado do alerta não é alterado até que o utilizador o altere.

#### <a name="change-the-state-of-an-alert-or-smart-group"></a>Alterar o estado de um grupo de alerta ou inteligente
Pode alterar o estado de um alerta individual ou gerir vários alertas em conjunto, definindo o estado de um grupo inteligente.

Alterar o estado de um alerta, selecionando **alterar estado de alerta** na exibição de detalhes do alerta. Ou alterar o estado para um grupo inteligente, selecionando **alterar estado do grupo inteligente** na sua exibição de detalhes. Alterar o estado de vários itens em simultâneo, selecionando-os primeiro na vista de lista e, em seguida, selecionando **alterar estado** na parte superior da página. 

Em ambos os casos, selecione um novo Estado no menu pendente. Em seguida, forneça um comentário opcional. Se estiver alterando um único item, tem também uma opção para aplicar as mesmas alterações a todos os alertas no grupo de inteligente.

![Alterar estado](media/monitoring-overview-unified-alerts/change-tate.png)

### <a name="alerts-page"></a>Página de alertas
A página de alertas padrão fornece um resumo dos alertas que são criados dentro de uma janela de tempo específico. Apresenta o total de alertas para cada gravidade com as colunas que identificam o número total de alertas em cada Estado para cada gravidade. Selecione qualquer uma das gravidades para abrir o [todos os alertas](#all-alerts-page) página filtrada por esse gravidade.

![Página de alertas](media/monitoring-overview-unified-alerts/alerts-page.png)

Pode filtrar esta vista ao selecionar os valores nos menus de lista pendente na parte superior da página.

| Coluna | Descrição |
|:---|:---|
| Subscrição | Selecione até cinco subscrições do Azure. Apenas os alertas nas subscrições selecionadas estão incluídos na vista. |
| Grupo de recursos | Selecione um grupo de recursos. Apenas os alertas com destinos no grupo de recursos selecionado estão incluídos na vista. |
| Intervalo de tempo | Apenas os alertas acionados dentro da janela de tempo selecionado estão incluídos na vista. Os valores suportados são a última hora, últimas 24 horas, últimos 7 dias e os últimos 30 dias. |

Selecione os seguintes valores na parte superior da página de alertas para abrir outra página.

| Valor | Descrição |
|:---|:---|
| Alertas totais | O número total de alertas que correspondam aos critérios selecionados. Selecione este valor para abrir a vista de todos os alertas com nenhum filtro. |
| Grupos inteligentes | O número total de grupos inteligentes que foram criadas a partir os alertas que correspondam aos critérios selecionados. Selecione este valor para abrir a lista de grupos inteligente na vista de todos os alertas.
| Regras de alertas totais | O número total de regras de alerta do grupo de recursos e subscrição selecionados. Selecione este valor para abrir a vista de regras filtrado com base na subscrição selecionada e o grupo de recursos.


### <a name="all-alerts-page"></a>Página de todos os alertas 
Ao utilizar a página de todos os alertas, pode ver uma lista de alertas que foram criadas dentro da janela de tempo selecionado. Pode ver uma lista dos alertas individuais ou uma lista dos grupos inteligentes que contêm os alertas. Selecione a faixa na parte superior da página para alternar entre modos de exibição.

![Página de todos os alertas](media/monitoring-overview-unified-alerts/all-alerts-page.png)

Pode filtrar a vista ao selecionar os seguintes valores nos menus de lista pendente na parte superior da página.

| Coluna | Descrição |
|:---|:---|
| Subscrição | Selecione até cinco subscrições do Azure. Apenas os alertas nas subscrições selecionadas estão incluídos na vista. |
| Grupo de recursos | Selecione um grupo de recursos. Apenas os alertas com destinos no grupo de recursos selecionado estão incluídos na vista. |
| Tipo de recurso | Selecione um ou mais tipos de recursos. Apenas os alertas com destinos do tipo selecionado estão incluídos na vista. Esta coluna só está disponível depois de um grupo de recursos foi especificado. |
| Recurso | Selecione um recurso. Apenas alertas com esse recurso como um destino estão incluídas na vista. Esta coluna apenas está disponível após foi especificado um tipo de recurso. |
| Gravidade | Selecione uma gravidade de alerta ou selecione *todos os* para incluir alertas de todas as gravidades. |
| Condição do monitor | Selecione uma condição do monitor ou selecione *todos os* para incluir alertas de condições. |
| Estado de alerta | Selecione um Estado de alerta ou selecione *todos os* para incluir a alertas dos Estados. |
| Monitorizar serviço | Selecione um serviço ou selecione *todos os* para incluir todos os serviços. Apenas os alertas criados por regras que utilizam o serviço como um destino estão incluídos. |
| Intervalo de tempo | Apenas os alertas acionados dentro da janela de tempo selecionado estão incluídos na vista. Os valores suportados são a última hora, últimas 24 horas, últimos 7 dias e os últimos 30 dias. |

Selecione **colunas** na parte superior da página para selecionar as colunas a apresentar. 

### <a name="alert-detail-page"></a>Página de detalhes de alerta
A página de detalhes de alerta é apresentada quando seleciona um alerta. Ele fornece detalhes do alerta e permite-lhe alterar o seu estado.

![Detalhes do alerta](media/monitoring-overview-unified-alerts/alert-detail.png)

A página de detalhes de alerta inclui as secções seguintes.

| Section | Descrição |
|:---|:---|
| Essentials | Apresenta as propriedades e outras informações significativas sobre o alerta. |
| Histórico | Apresenta uma lista de cada ação tomada pelo alerta e todas as alterações efetuadas ao alerta. Isso está limitado a alterações de estado. |
| Grupo inteligente | Informações sobre o grupo inteligente o alerta estão incluídas no. O *contagem de alertas* refere-se ao número de alertas que estão incluídos no grupo de inteligente. Isto inclui os outros alertas no mesmo grupo de inteligente que foram criados nos últimos 30 dias.  Isto é, independentemente do filtro de tempo na página de lista de alertas. Selecione um alerta para ver os seus detalhes. |
| Mais detalhes | Mostra informações contextuais para o alerta, o que é normalmente específico para o tipo de origem que criou o alerta de adicionais. |


### <a name="smart-group-detail-page"></a>Página de detalhes do grupo inteligente
A página de detalhes do grupo inteligente é apresentada ao selecionar um grupo inteligente. Fornece detalhes sobre o grupo inteligente, incluindo o raciocínio que foi utilizado para criar o grupo e permite-lhe alterar o seu estado.
 
![Detalhes do grupo de inteligente](media/monitoring-overview-unified-alerts/smart-group-detail.png)


A página de detalhes do grupo inteligente inclui as secções seguintes.

| Section | Descrição |
|:---|:---|
| Alertas | Lista os alertas individuais que estão incluídos no grupo de inteligente. Selecione um alerta para abrir a página de detalhes de alerta. |
| Histórico | Apresenta uma lista de cada ação tomada pelo grupo de smart e quaisquer alterações efetuadas ao mesmo. Isso está limitado a alterações de estado e as alterações na associação de alerta. |

## <a name="next-steps"></a>Passos Seguintes
- [Saiba como utilizar a nova experiência de alertas para criar, ver e gerir alertas](alert-metric.md)
- [Saiba mais sobre alertas de registo na experiência de alertas](monitor-alerts-unified-log.md)
- [Saiba mais sobre alertas de métricas na experiência de alertas](monitoring-near-real-time-metric-alerts.md)
- [Saiba mais sobre alertas de registo de atividade na experiência de alertas](alert-activity-log.md)
