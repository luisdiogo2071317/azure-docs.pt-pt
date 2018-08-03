---
title: Criar, ver e gerir alertas utilizando o Azure Monitor
description: Utilize a nova experiência unificada de alertas do Azure para criar, ver e gerir a métrica e regras de alerta de registo num único local.
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 02/05/2018
ms.author: vinagara
ms.component: alerts
ms.openlocfilehash: 77aa6805094d44d16eea9ac671316c9702423e6c
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39433407"
---
# <a name="create-view-and-manage-alerts-using-azure-monitor"></a>Criar, ver e gerir alertas ao utilizar o Azure Monitor  

## <a name="overview"></a>Descrição geral
Este artigo mostra-lhe como configurar alertas com a nova interface de alertas no portal do Azure. Definição de uma regra de alerta está nas três partes:
- Destino: Recursos do Azure específica, que está a monitorizar
- Critérios: Condição específica ou a lógica que quando visto no sinal, deve acionar a ação
- Ação: Chamada específica enviada para um recetor de uma notificação - enviar um e-mail, SMS, webhook, etc.

Alertas do Azure também fornece uma vista unificada para todas as suas regras de alerta e capacidade de gerenciá-los um único local; incluindo a visualização de todos os alertas não resolvidos. Saiba mais sobre as funcionalidades da [alertas do Azure - descrição geral](monitoring-overview-unified-alerts.md).

Alerta utiliza o termo **alertas de registo** para descrever os alertas em que o sinal é a consulta personalizada com base nos [do Log Analytics](../log-analytics/log-analytics-tutorial-viewdata.md) ou [Application Insights](../application-insights/app-insights-analytics.md). O [nova capacidade de alerta de métrica](monitoring-near-real-time-metric-alerts.md) fornece a capacidade de alerta sobre [métricas multidimensionais](monitoring-metric-charts.md) para recursos específicos do Azure. Os alertas para esse recurso podem utilizar filtros adicionais em dimensões criando **alertas de métrica de acordo com**.


> [!NOTE]
> Enquanto os alertas do Azure oferece uma experiência de nova e aprimorada para a criação de alertas no Azure. A atual [alertas (clássico)](monitoring-overview-alerts.md) experiência permanece utilizável.
>

Próxima detalhada é um guia passo a passo para utilizar os alertas do Azure.

## <a name="create-an-alert-rule-with-the-azure-portal"></a>Criar uma regra de alerta com o Portal do Azure
1. Na [portal](https://portal.azure.com/), selecione **Monitor** e, na secção MONITOR - escolher **alertas**.  
    ![Monitorização](./media/monitor-alerts-unified/AlertsPreviewMenu.png)

1. Selecione o **nova regra de alerta** botão para criar um novo alerta no Azure.
    ![Adicionar alerta](./media/monitor-alerts-unified/AlertsPreviewOption.png)

1. A secção de criar o alerta é mostrada com três partes consiste em: *definir a condição de alerta*, *definir detalhes do alerta*, e *Definir grupo de ação*.

    ![Criar regra](./media/monitor-alerts-unified/AlertsPreviewAdd.png)

1.  Definir a condição de alerta com o **selecionar recurso** ligação e especificando o destino, ao selecionar um recurso. Filtro ao selecionar o * subscrição, * tipo de recurso e, finalmente, selecionando necessários *recursos*.

    >[!NOTE]

    > Antes de prosseguir, verifique se os sinais disponíveis para o recurso selecionado.

    ![Selecionar o recurso](./media/monitor-alerts-unified/Alert-SelectResource.png)

 A interface de utilizador permite-lhe criar vários tipos de alertas num único local. Com base no tipo de alerta pretendida escolha a etapa seguinte como:

    - Para **alertas de métrica**: Siga os passos 5 a 7; em seguida, avance diretamente para o passo 11
    - Para **alertas de registo**, avance para o passo 8.

    > [!NOTE]

    > Alertas de registo de atividade são também suportadas, mas estão em pré-visualização. [Saiba mais](monitoring-activity-log-alerts-new-experience.md).

1. *Alertas de métricas*: Certifique-se **tipo de recurso** está selecionada com o tipo de sinal como **métrica**, em seguida, uma vez adequado **recurso** é escolhido clique  *Feito* botão para retornar ao criar alerta. Em seguida utilize o **adicionar critérios** botão para escolher o sinal específico da lista de opções de sinal, serviço de monitoramento e tipo listado - que estão disponíveis para o recurso selecionado anteriormente.

    ![Selecionar um recurso](./media/monitor-alerts-unified/AlertsPreviewResourceSelection.png)

    > [!NOTE]

    >  Todos os [perto alertas em tempo real](monitoring-near-real-time-metric-alerts.md) com capacidade de recursos é listada com o serviço de monitor **plataforma** e sinalizar tipo como **métrica**

1. *Alertas de métricas*: assim que o sinal é selecionado, a lógica para alertar pode ser declarada. Para referência, o histórico de dados de sinal é mostrado com opção de ajustar a janela de tempo utilizando **Mostrar histórico**, variando dos últimos seis horas para a última semana. Com a visualização no local, **Alert Logic** podem ser selecionados a partir das opções apresentadas da condição, a agregação e, finalmente, limiar. Como pré-visualização da lógica de fornecida, a condição é mostrada na visualização, juntamente com o histórico de sinal, para indicar quando seria ter acionado o alerta. Por fim especifique durante o período de tempo, o alerta deve ser para a condição especificada por escolher o **período** opção juntamente com a frequência com que o alerta deve executar selecionando **frequência**.

    ![Configurar lógica de sinal para métrica](./media/monitor-alerts-unified/AlertsPreviewCriteria.png)

1. *Alertas de métricas*: sinal se é uma métrica, em seguida, janela de alerta pode ser filtrada com vários pontos de dados ou as dimensões para o recurso do Azure disse. 

    a. Escolha uma duração do **Mostrar histórico** lista pendente para visualizar um outro período de tempo. Pode escolher as dimensões para as métricas suportadas filtrar numa série de tempo; a escolha de dimensões é opcional e up-to cinco dimensões podem ser utilizadas. 

    b. **Lógica de alerta** podem ser selecionados a partir das opções apresentadas de *condição*, * agregação, e *limiar*. Como pré-visualização da lógica de fornecida, a condição é mostrada na visualização, juntamente com o histórico de sinal, para indicar quando o alerta seria que foram acionado no passado. 

    c. Para especificar a duração de tempo, escolha **período** juntamente com a frequência com que o alerta deve ser executado ao selecionar **frequência**.

    ![Configurar lógica de sinal para métricas multidimensionais](./media/monitor-alerts-unified/AlertsPreviewCriteriaMultiDim.png)

1. *Alertas de registo*: Certifique-se **tipo de recurso** é uma origem de análise, como *do Log Analytics* ou *Application Insights* e sinalizar tipo como **registo** , em seguida, uma vez adequado **recurso** está selecionado, clique em *feito*. Em seguida utilize o **adicionar critérios** botão para ver a lista de opções de sinal disponíveis para o recurso e a partir da lista de sinal **pesquisa de registos personalizado** opção para escolhida iniciar o monitor de serviço, como *registo Análise* ou *Application Insights*.

   ![Selecione um recurso - pesquisa de registos personalizado](./media/monitor-alerts-unified/AlertsPreviewResourceSelectionLog.png)

   > [!NOTE]

   > Alertas de listas, podem importar consulta do analytics como tipo de sinal - **Log (consulta guardada)**, como mostra acima ilustração. Para que os utilizadores possam aperfeiçoa sua consulta do Analytics e, em seguida, guarde-as para utilização futura nos alertas - mais detalhes sobre como utilizar a guardar consulta disponível em [utilizando a pesquisa de registos no log analytics](../log-analytics/log-analytics-log-searches.md) ou [consulta partilhada no application insights análise](../log-analytics/log-analytics-overview.md). 

1.  *Alertas de registo*: depois de selecionada, a consulta para alertas pode ser indicada na **consulta de pesquisa** campo; se a sintaxe de consulta está incorreta o campo apresenta o erro em vermelho. Se a sintaxe de consulta está correta – para referência, histórico de dados da consulta declarado é mostrado como um gráfico com a opção para ajustar a janela de tempo da última seis horas da semana passada.

 ![Configurar a regra de alerta](./media/monitor-alerts-unified/AlertsPreviewAlertLog.png)

 > [!NOTE]

    > Visualização de dados históricos pode ser apresentada apenas se os resultados da consulta tem os detalhes de tempo. Se a sua consulta resulta em dados resumidos ou valores de coluna de específicos - mesmo é mostrado como um desenho singular.

    >  Para o tipo de medida da métrica de alertas de registo com o Application insights, pode especificar qual variável específico para agrupar os dados utilizando o **agregada no** opção; conforme ilustrado na abaixo:

    ![Agregar na opção](./media/monitor-alerts-unified/aggregate-on.png)

1.  *Alertas de registo*: com a visualização no local, **Alert Logic** podem ser selecionados a partir das opções apresentadas da condição, a agregação e, finalmente, limiar. Por fim especifique na lógica de tempo para avaliar a condição especificada, utilizando **período** opção. Juntamente com a frequência com que o alerta deve executar selecionando **frequência**.
Para **alertas de registo** alertas podem ser baseados em:
   - *Número de registos*: é criado um alerta se a contagem de registos devolvidos pela consulta for maior ou menor que o valor fornecido.
   - *Medida da métrica*: é criado um alerta se cada *agregar valor* nos resultados excede o valor de limiar fornecido e é *agrupados por* escolhido o valor. O número de falhas de um alerta é o número de vezes que o limite é excedido no período de tempo escolhido. Pode especificar o Total de falhas para qualquer combinação de violações de entre o conjunto de resultados ou falhas consecutivas para exigir que as violações devem ocorrer no amostras consecutivas. Saiba mais sobre [alertas de registo e os tipos de](monitor-alerts-unified-log.md).

    > [!TIP]
    > Atualmente em alertas - alertas de pesquisa de registo podem demorar personalizados *período* e *frequência* valor no minuto (s). Valores podem diferir dos 5 minutos e 1440 minutos (ou seja) 24 horas. Portanto, se pretender que o período de alerta para ser três horas, por exemplo, convertê-lo em minutos - 180 minutos, antes de utilização

1. Como o segundo passo, defina um nome para o alerta no **nome da regra de alerta** campo juntamente com um **Descrição** com detalhes sobre as especificações para o alerta e **gravidade** partir das opções fornecidas. Esses detalhes são reutilizados em todos os e-mails de alerta, notificações ou push feita pelo Azure Monitor. Além disso, o utilizador pode optar por ativar imediatamente a regra de alerta após a criação, ativando adequadamente **ativar regra após a criação** opção.

    Para **alertas de registo** apenas, algumas funcionalidades adicionais estão disponível nos detalhes do alerta:

    - **Suprimir alertas**: Quando ativar a supressão para a regra de alerta, as ações da regra estão desativadas para um comprimento de definidos de tempo depois de criar um novo alerta. A regra ainda está em execução e cria registos de alerta, desde que os critérios são cumpridos. Permitindo que tempo para corrigir o problema sem executar ações duplicadas.

        ![Suprimir alertas para alertas de registo](./media/monitor-alerts-unified/AlertsPreviewSuppress.png)

        > [!TIP]
        > Especifique um valor alerta suppress superior à frequência do alerta para garantir a notificações são paradas sem sobreposição

1. Como a terceira e última etapa, especifique se houver **grupo de ação** tem de ser acionada para a regra de alerta quando for cumprida a condição do alerta. Pode escolher qualquer grupo de ação existente com o alerta ou crie um novo grupo de ação. Em conformidade com selecionado o grupo de ação, quando o alerta é o acionador do Azure será: enviar email(s), Enviar SMS(s), chamar o webhook (s), remediar através de Runbooks do Azure, push para a ferramenta ITSM, entre outras. Saiba mais sobre [grupos de ação](monitoring-action-groups.md).

    Para **alertas de registo** algumas funcionalidades adicionais estão disponíveis para substituir as ações padrão:

    - **Notificação por e-mail**: substitui *assunto do e-mail* no e-mail, enviado por grupo de ação; se uma ou mais ações de e-mail do grupo de ação disse. Não é possível modificar o corpo da mensagem e este campo é **não** para endereço de e-mail.
    - **Incluir payload Json personalizado**: substitui o JSON utilizado por grupos de ação do webhook, se uma ou mais ações de webhook do grupo de ação disse. Utilizador pode especificar o formato do JSON a ser utilizado para todos os webhooks configurados no grupo de ação associado; Para obter mais informações sobre formatos de webhook, veja [ação do webhook para alertas de registo](monitor-alerts-unified-log-webhook.md). Opção de Webhook de teste é fornecida para verificar o formato e o processamento de destino com o JSON de exemplo e esta opção, como indicado destina-se apenas **teste** fins.

        ![Substituições de ação para alertas de registo](./media/monitor-alerts-unified/AlertsPreviewOverrideLog.png)

        > [!NOTE]
        > Para **testar Webhook** a opção de trabalhar, o ponto final deve suportar [Cross Origin Resource Sharing (CORS)](https://www.w3.org/TR/cors/) e utilizador pode utilizar o proxy CORS para resolver problemas de "Nenhum cabeçalho Access-Control-Allow-Origin"

1. Se todos os campos são válidos e com escala verde a **criar regra de alerta** botão pode ser clicado e é criado um alerta no Azure Monitor - alertas. Todos os alertas podem ser visualizados a partir os alertas do Dashboard.

    ![Criação de regras](./media/monitor-alerts-unified/AlertsPreviewCreate.png)

    Em poucos minutos, o alerta está ativo e aciona conforme descrito anteriormente.

## <a name="view-your-alerts-in-azure-portal"></a>Ver os alertas no Portal do Azure

1. Na [portal](https://portal.azure.com/), selecione **Monitor** e, na secção MONITOR - escolher **alertas**.  

1. O **Dashboard alertas** é apresentado - na qual todos os alertas do Azure são unificados e apresentados num quadro singular ![Dashboard de alerta](./media/monitoring-alerts-unified-usage/alerts-preview-overview.png)
1. Da parte superior esquerda para a direita, o Dashboard mostra rapidamente, o seguinte - que pode ser clicado para ver uma lista detalhada:
    - *Acionados alertas*: O número de alertas atualmente, que tem cumpridos lógica e na disparado Estado
    - *Total de regras de alerta*: O número de regras de alerta criadas e em subtexto, o número que estão atualmente ativadas 
    

        > [!NOTE]
        > Para garantir que o dashboard consistente com detalhes sobre todos os alertas acionados, incluindo alertas de registo para o application insights e o log analytics; [Avançado unificado o alerta (pré-visualização)](monitoring-overview-unified-alerts.md#enhanced-unified-alerts-public-preview) deve ser utilizado
  
  
1. Uma lista de todos os alertas acionados é mostrada que o usuário pode clicar para ver os detalhes
1. Auxílio em particular a localizar os alertas; é possível usar as opções de menu pendente na parte superior para filtragem específica *subscrição, grupo de recursos e/ou recurso*. Mais para qualquer não resolvido do alerta, uma utilização a *alerta de filtro* opção para localizar para fornecido palavra-chave - alertas correspondentes específicos com *nome, alerta critérios, grupo de recursos e o recurso de destino*

## <a name="managing-your-alerts-in-azure-portal"></a>Gerir os seus alertas no Portal do Azure
1. Na [portal](https://portal.azure.com/), selecione **Monitor** e, na secção MONITOR - escolher **alertas**.  
1. Selecione o **gerir regras** botão na barra superior, para navegar para a secção de gestão de regra - onde estão apresentadas os todas as regras de alerta criadas; incluindo alertas que foram desativadas.
1. Para localizar de regras de alerta específicas, um pode utilizar os filtros de lista pendente na parte superior, que permite às regras de alerta assim, agreguem específicas *subscrição, grupos de recursos e/ou recurso*. Em alternativa sobre como utilizar a pesquisa painel acima da lista de regra de alerta marcado *filtrar alertas*, um pode fornecer a palavra-chave, que é comparado com *nome do alerta, condição e recurso de destino*; para mostrar apenas regras de correspondência.
   ![Alerta de gerir as regras](./media/monitoring-alerts-unified-usage/alerts-preview-rules.png)
1. Para ver ou modificar a regra de alerta específica, clique no respetivo nome seria exibida como um link clicável.
1. Alerta definida é mostrada - na estrutura da fase de três: grupo de 3) ação de detalhe do alerta de 1) condição 2) o alerta. **Critérios de destino** pode ser clicado para modificar a lógica de alerta ou uma nova critérios podem ser adicionados depois de utilizar o ícone de reciclagem para eliminar a lógica anterior. Da mesma forma, na secção de detalhes do alerta - **Descrição** e **gravidade** pode ser modificado. E o grupo de ação pode ser alterado ou um novo pode ser moldado para criar uma ligação para o alerta utilizando o **novo grupo de ação** botão.

   ![Modificar a regra de alerta](./media/monitor-alerts-unified/AlertsPreviewEdit.png)

1. Com o painel superior, alterações do alerta podem ser incluindo reflexiva: **salvar** para consolidar quaisquer alterações efetuadas às alerta, **descartar** para voltar atrás sem consolidar as alterações feitas para o alerta, **desativar**  para desativar o alerta - o após o qual ele já não é executada ou aciona qualquer ação. E por fim, **eliminar** para remover permanentemente a regra de alerta toda a partir do Azure.

## <a name="next-steps"></a>Passos Seguintes

- Saiba mais sobre o novo [perto de alertas de métricas em tempo real](monitoring-near-real-time-metric-alerts.md)
- Obter um [descrição geral da coleção de métricas](insights-how-to-customize-monitoring.md) para se certificar de que o seu serviço está disponível e reativo.
- Saiba mais sobre [alertas de registo nos alertas do Azure](monitor-alerts-unified-log.md)
- [Saiba mais sobre alertas de registo de atividade na experiência de alertas (pré-visualização)](monitoring-activity-log-alerts-new-experience.md)
