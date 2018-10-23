---
title: Analisar eventos a partir da configuração do Azure Digital Twins | Microsoft Docs
description: Saiba como visualizar e analisar eventos dos seus espaços do Azure Digital Twins com o Time Series Insights através dos passos neste tutorial.
services: digital-twins
author: dsk-2015
ms.service: digital-twins
ms.topic: tutorial
ms.date: 10/15/2018
ms.author: dkshir
ms.openlocfilehash: 1366cafe5d2c526e86905c108b9c7b865aac8f69
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/15/2018
ms.locfileid: "49323465"
---
# <a name="tutorial-visualize-and-analyze-events-from-your-azure-digital-twins-spaces-using-time-series-insights"></a>Tutorial: Visualizar e analisar eventos dos seus espaços do Azure Digital Twins com o Time Series Insights

Depois de ter implementado a sua instância do Azure Digital Twins, aprovisionado os espaços e implementado a função personalizada para monitorizar condições específicas, pode visualizar os eventos e os dados provenientes dos espaços para ver tendências e anomalias. 

No [primeiro tutorial](tutorial-facilities-setup.md), configurou o gráfico espacial de um edifício imaginário, com uma divisão que inclui sensores de movimento, dióxido de carbono e temperatura. No [segundo tutorial](tutorial-facilities-udf.md), aprovisionou o gráfico e uma função definida pelo utilizador. A função monitoriza estes valores do sensor e aciona notificações para as condições certas, ou seja, quando a sala está vazia e os níveis de temperatura e de dióxido de carbono são normais. Este tutorial mostra-lhe como integrar as notificações e os dados provenientes da configuração do Digital Twins no Azure Time Series Insights. Depois, pode visualizar os valores do sensor ao longo do tempo e procurar tendências como a sala mais utilizada, as horas mais congestionadas do dia, etc. Também pode detetar anomalias, como a sala menos arejada e mais quente ou se uma área do edifício envia valores de temperatura sistematicamente altos, indicando um ar-condicionado avariado.

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Transmitir dados com os Hubs de Eventos
> * Analisar com o Time Series Insights

## <a name="prerequisites"></a>Pré-requisitos

Neste tutorial, parte-se do princípio de que [configurou](tutorial-facilities-setup.md) e [aprovisionou](tutorial-facilities-udf.md) a sua configuração do Azure Digital Twins. Antes de avançar, confirme que tem:
- Uma [conta do Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Uma instância do Digital Twins em execução.
- Os [exemplos do Digital Twins em C#](https://github.com/Azure-Samples/digital-twins-samples-csharp) transferidos e extraídos para o computador de trabalho.
- A [versão 2.1.403 ou superior do .NET Core SDK](https://www.microsoft.com/net/download) no computador de desenvolvimento, para executar o exemplo. Para confirmar se tem a versão certa instalada, execute `dotnet --version`. 


## <a name="stream-data-using-event-hubs"></a>Transmitir dados com os Hubs de Eventos
O serviço [Hubs de Eventos](../event-hubs/event-hubs-about.md) permite-lhe criar um pipeline para transmissão os seus dados. Esta secção mostra-lhe como criar o seu hub de eventos para funcionar como o conector entre as instâncias do Digital Twins e do TSI.

### <a name="create-an-event-hub"></a>Criar um hub de eventos

1. Inicie sessão no [portal do Azure](https://portal.azure.com).

1. No painel de navegação do lado esquerdo, clique em **Criar um recurso**. 

1. Procure e selecione **Hubs de Eventos**. Clique em **Criar**.

1. Introduza um **Nome** para o espaço de nomes dos Hubs de Eventos, escolha o **Escalão de preço** *Standard*, a sua **Subscrição**, o **Grupo de recursos** que utilizou para a instância do Digital Twins e a **Localização**. Clique em **Criar**. 

1. Após a implementação, navegue para o espaço de nomes *deployment* dos Hubs de Eventos e clique no espaço de nomes em **RECURSO**.

    ![Espaço de nomes do hub de eventos](./media/tutorial-facilities-analyze/open-event-hub-ns.png)


1. No painel **Descrição Geral** do espaço de nomes dos Hubs de Eventos, clique no botão **Hub de Eventos**, na parte superior. 
    ![Hub de Eventos](./media/tutorial-facilities-analyze/create-event-hub.png)

1. Introduza um **Nome** para o hub de eventos e clique em **Criar**. Após a implementação, o hub aparecerá no painel **Hubs de Eventos** do espaço de nomes dos Hubs de Eventos com o **ESTADO** *Ativo*. Clique neste hub de eventos para abrir o painel **Descrição Geral** do mesmo.

1. Clique no botão **Grupo de consumidor**, na parte superior, e introduza um nome, como *tsievents*, para o grupo de consumidor. Clique em **Criar**.
    ![Grupo de consumidor do Hub de Eventos](./media/tutorial-facilities-analyze/event-hub-consumer-group.png)

   Depois de criado, o grupo de consumidor aparecerá na lista, na parte inferior do painel **Descrição Geral** do hub de eventos. 

1. Abra o painel **Políticas de acesso partilhado** do hub de eventos e clique no botão **Adicionar**. **Crie** uma política denominada *ManageSend* e confirme que todas as caixas de verificação estão assinaladas. 

    ![Cadeias de ligação do Hub de Eventos](./media/tutorial-facilities-analyze/event-hub-connection-strings.png)

1. Abra a política *ManageSend* criada e copie os valores de **Connection string--primary key** (Cadeia de ligação -- chave primária) e **Connection string--secondary key** (Cadeia de ligação -- chave secundária) para um ficheiro temporário. Vai precisar destes valores para criar um ponto final para o hub de eventos na próxima secção.

### <a name="create-endpoint-for-the-event-hub"></a>Criar ponto final para o hub de eventos

1. Na janela de comandos, confirme que está na pasta **_occupancy-quickstart\src** do exemplo do Digital Twins.

1. Abra o ficheiro **_actions\createEndpoints.yaml_** no editor. Substitua os conteúdos pelo seguinte:

    ```yaml
    - type: EventHub
      eventTypes:
      - SensorChange
      - SpaceChange
      - TopologyOperation
      - UdfCustom
      connectionString: Primary_connection_string_for_your_event_hub
      secondaryConnectionString: Secondary_connection_string_for_your_event_hub
      path: Name_of_your_Event_Hubs_namespace
    - type: EventHub
      eventTypes:
      - DeviceMessage
      connectionString: Primary_connection_string_for_your_event_hub
      secondaryConnectionString: Secondary_connection_string_for_your_event_hub
      path: Name_of_your_Event_Hubs_namespace
    ```

1. Substitua os marcadores de posição `Primary_connection_string_for_your_event_hub` pelo valor de **Connection string--primary key** (Cadeia de ligação -- chave primária) do hub de eventos. Confirme que o formato dessa cadeia de ligação é o seguinte:
```
Endpoint=sb://nameOfYourEventHubNamespace.servicebus.windows.net/;SharedAccessKeyName=ManageSend;SharedAccessKey=yourShareAccessKey1GUID;EntityPath=nameOfYourEventHub
```

1. Substitua os marcadores de posição `Secondary_connection_string_for_your_event_hub` pelo valor de **Connection string--secondary key** (Cadeia de ligação -- chave secundária). Confirme que o formato dessa cadeia de ligação é o seguinte: 
```
Endpoint=sb://nameOfYourEventHubNamespace.servicebus.windows.net/;SharedAccessKeyName=ManageSend;SharedAccessKey=yourShareAccessKey2GUID;EntityPath=nameOfYourEventHub
```

1. Substitua os marcadores de posição `Name_of_your_Event_Hubs_namespace` pelo nome do espaço de nomes do Hub de Eventos.

    > [!IMPORTANT]
    > Introduza todos os valores, sem aspas. Certifique-se de que há, pelo menos, um caráter de espaço a seguir aos dois pontos no ficheiro *YAML*. Também pode validar os conteúdos do ficheiro *YAML* com um validador de YAML online, como [esta ferramenta](https://onlineyamltools.com/validate-yaml).


1. Guarde e feche o ficheiro. Na janela de comandos, execute o comando seguinte e inicie sessão com a sua conta do Azure quando lhe for pedido.

    ```cmd/sh
    dotnet run CreateEndpoints
    ```
   
   São criados dois pontos finais para o seu hub de eventos.

   ![Pontos finais dos Hubs de Eventos](./media/tutorial-facilities-analyze/dotnet-create-endpoints.png)

## <a name="analyze-with-time-series-insights"></a>Analisar com o Time Series Insights

1. No painel de navegação esquerdo do [portal do Azure](https://portal.azure.com), clique em **Criar um recurso** 

1. Procure e selecione um recurso novo do **Time Series Insights**. Clique em **Criar**.

1. Introduza um **Nome** para a instância do Time Series Insights e selecione a sua **Subscrição**. Selecione o **Grupo de recursos** que utilizou para a instância do Digital Twins e a **Localização**. Clique em **Criar**.

    ![Criar TSI](./media/tutorial-facilities-analyze/create-tsi.png)

1. Após a implementação, abra o ambiente do Time Series Insights e abra o respetivo painel **Event Sources** (Origens de Eventos). Clique no botão **Add** (Adicionar), na parte superior, para adicionar um grupo de consumidor.

1. No painel **New event source** (Nova origem de evento), introduza **Name** (Nome) e confirme que os outros valores estão devidamente selecionados. Selecione *ManageSend* como o **nome da política do hub de eventos** e selecione o *grupo de consumidor* que criou na secção anterior como o **grupo de consumidor do hub de eventos**. Clique em **Criar**.

    ![Origem de eventos do TSI](./media/tutorial-facilities-analyze/tsi-event-source.png)

1. Abra o painel **Overview** (Descrição Geral) do ambiente do Time Series Insights e clique no botão **Go to Environment** (Ir Para o Ambiente), na parte superior. Se receber um *aviso de acesso a dados*, abra o painel **Data Access Policies** (Políticas de Acesso a Dados) da instância do TSI, clique em **Add** (Adicionar), selecione **Contributor** (Contribuidor) como a função e selecione o utilizador adequado.

1. O botão **Go to Environment** (Ir Para o Ambiente) abre o [Explorador do Time Series Insights](../time-series-insights/time-series-insights-explorer.md). Se não forem mostrados eventos, navegue para o projeto **_device-connectivity_** do exemplo do Digital Twins e execute `dotnet run` para simular eventos do dispositivo.

1. Depois de serem gerados alguns eventos simulados, regresse ao explorador do Time Series Insights e clique no botão para atualizar, na parte superior. Deverá ver que estão a ser criados gráficos analíticos para os dados do sensor simulados. 

    ![Explorador do TSI](./media/tutorial-facilities-analyze/tsi-explorer.png)

1. No Explorador do Time Series, pode, depois, gerar gráficos e mapas térmicos para diferentes eventos e dados das salas, dos sensores e de outros recursos. No lado esquerdo, clique nas caixas pendentes denominadas **MEASURE** (MEDIR) e **SPLIT BY** (DIVIDIR POR) para criar as suas próprias visualizações. Por exemplo, selecione *Eventos* como **MEASURE** (MEDIDA) e *DigitalTwins-SensorHardwareId* para **SPLIT BY** (DIVIDIR POR), de modo a gerar um mapa térmico para cada um dos seus sensores, conforme a imagem seguinte:

    ![Explorador do TSI](./media/tutorial-facilities-analyze/tsi-explorer-heatmap.png)

## <a name="clean-up-resources"></a>Limpar recursos

Se já não quiser explorar mais o Azure Digital Twins para além deste ponto, não hesite em eliminar os recursos que foram criados no tutorial:

1. No menu do lado esquerdo do [portal do Azure](http://portal.azure.com), clique em **Todos os recursos**, selecione o seu grupo de recursos do Digital Twins e clique em **Eliminar**.
2. Se for necessário, também pode eliminar os exemplos de aplicações no computador de trabalho. 


## <a name="next-steps"></a>Passos seguintes

Avance para o próximo artigo para saber mais sobre os gráficos de inteligência espacial e os modelos de objetos no Azure Digital Twins. 
> [!div class="nextstepaction"]
> [Understanding Digital Twins object models and spatial intelligence graph](concepts-objectmodel-spatialgraph.md) (Compreender os modelos de objetos e o gráfico de inteligência espacial do Digital Twins)

