---
title: Capturar eventos de um espaço do Azure Digital Twins | Microsoft Docs
description: Saiba como receber notificações dos seus espaços mediante a integração do Azure Digital Twins no Logic Apps através dos passos descritos neste tutorial.
services: digital-twins
author: dsk-2015
ms.service: digital-twins
ms.topic: tutorial
ms.date: 10/15/2018
ms.author: dkshir
ms.openlocfilehash: 91dd16938efbd1e24419352f66e3238646a77e8a
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/15/2018
ms.locfileid: "49323433"
---
# <a name="tutorial-receive-notifications-from-your-azure-digital-twins-spaces-using-logic-apps"></a>Tutorial: Utilizar o Logic Apps para receber notificações dos seus espaços do Azure Digital Twins

Depois de implementar a instância do Azure Digital Twins, aprovisionar os espaços e implementado as funções personalizadas para monitorizar condições específicas, pode notificar o seu escritório por e-mail quando as condições monitorizadas ocorrem. 

No [primeiro tutorial](tutorial-facilities-setup.md), configurou o gráfico espacial de um edifício imaginário, com uma divisão que inclui sensores de movimento, dióxido de carbono e temperatura. No [segundo tutorial](tutorial-facilities-udf.md), aprovisionou o gráfico e uma função personalizada chamou a função definida pelo utilizador para monitorizar os valores do sensor e acionar notificações quando a divisão estiver vazia e a temperatura e o dióxido de carbono estiverem num intervalo aceitável. Este tutorial mostra-lhe como pode integrar essas notificações no Azure Logic Apps para enviar e-mails quando uma divisão que cumpra estes requisitos esteja disponível. Os administradores dos escritórios podem utilizar estas informações para ajudar os funcionários a reservar a sala de reuniões mais produtiva. 

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Integrar eventos com o Event Grid
> * Notificar eventos com o Logic Apps
    
## <a name="prerequisites"></a>Pré-requisitos

Neste tutorial, parte-se do princípio de que [configurou](tutorial-facilities-setup.md) e [aprovisionou](tutorial-facilities-udf.md) a sua configuração do Azure Digital Twins. Antes de avançar, confirme que tem:
- Uma [conta do Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Uma instância do Digital Twins em execução.
- Os [exemplos do Digital Twins em C#](https://github.com/Azure-Samples/digital-twins-samples-csharp) transferidos e extraídos para o computador de trabalho.
- A [versão 2.1.403 ou superior do .NET Core SDK](https://www.microsoft.com/net/download) no computador de desenvolvimento, para executar o exemplo. Para confirmar se tem a versão certa instalada, execute `dotnet --version`. 
- Uma conta do Office 365 para enviar e-mails de notificação.

## <a name="integrate-events-with-event-grid"></a>Integrar eventos com o Event Grid 
Nesta secção, vai configurar o [Event Grid](../event-grid/overview.md) para recolher eventos da instância do Digital Twins e redirecioná-los para um [processador de eventos](../event-grid/event-handlers.md), como o Logic Apps.

### <a name="create-event-grid-topic"></a>Criar Tópico do Event Grid
Os [Tópicos do Event Grid](../event-grid/concepts.md#topics) oferecem uma interface para encaminhar os eventos gerados pela função definida pelo utilizador. 

1. Inicie sessão no [portal do Azure](https://portal.azure.com).

1. No painel de navegação do lado esquerdo, clique em **Criar um recurso**. 

1. Procure e selecione **Event Grid Topic** (Tópico do Event Grid). Clique em **Criar**.

1. Introduza **Name** (Nome) do tópico do Event Grid e escolha **Subscrição**. Selecione o **Grupo de recursos** que utilizou ou criou para a instância do Digital Twins e a **Localização**. Clique em **Criar**. 

    ![Criar Tópico do Event Grid](./media/tutorial-facilities-events/create-event-grid-topic.png)

1. Navegue para o tópico do Event Grid a partir do grupo de recursos, clique em **Overview** (Descrição Geral) e copie o valor de **Topic Endpoint** (Ponto Final do Tópico) para um ficheiro temporário. Vai precisar deste URL na secção seguinte. 

1. Clique em **Access keys** (Chaves de acesso) e copie **Key 1** (Chave 1) e **Key 2** (Chave 2) para um ficheiro temporário. Vai precisar destes valores para criar o ponto final na secção seguinte.

    ![Chaves do Event Grid](./media/tutorial-facilities-events/event-grid-keys.png)

### <a name="create-an-endpoint-for-the-event-grid-topic"></a>Criar um ponto final para o Tópico do Event Grid

1. Na janela de comandos, confirme que está na pasta **_occupancy-quickstart\src_** do exemplo do Digital Twins.

1. Abra o ficheiro **_actions\createEndpoints.yaml_** no editor do Visual Studio Code. Verifique se os conteúdos seguintes estão presentes:

    ```yaml
    - type: EventGrid
      eventTypes:
      - SensorChange
      - SpaceChange
      - TopologyOperation
      - UdfCustom
      connectionString: Primary_connection_string_for_your_Event_Grid
      secondaryConnectionString: Secondary_connection_string_for_your_Event_Grid
      path: Event_Grid_Topic_Path
    ```

1. Substitua o marcador de posição `Primary_connection_string_for_your_Event_Grid` pelo valor de **Key1** (Chave 1). 

1. Substitua o marcador de posição `Secondary_connection_string_for_your_Event_Grid` pelo valor de **Key2** (Chave 2).

1. Substitua o marcador de posição `Event_Grid_Topic_Path` pelo caminho do tópico do Event Grid. Para obter este caminho, remova *https://* e os caminhos do recurso à direita do URL do **Topic Endpoint** (Ponto Final do Tópico). Deverá ser semelhante ao formato **NomedoEventGrid.asuaLocalização.eventgrid.azure.net**. 

    > [!IMPORTANT]
    > Introduza todos os valores, sem aspas. Certifique-se de que há, pelo menos, um caráter de espaço a seguir aos dois pontos no ficheiro *YAML*. Também pode validar os conteúdos do ficheiro *YAML* com um validador de YAML online, como [esta ferramenta](https://onlineyamltools.com/validate-yaml).

1. Guarde e feche o ficheiro. Na janela de comandos, execute o comando seguinte e inicie sessão quando lhe for pedido. 

    ```cmd/sh
    dotnet run CreateEndpoints
    ```

   Este comando cria o ponto final do Event Grid. 

   ![Pontos finais do Event Grid](./media/tutorial-facilities-events/dotnet-create-endpoints.png)


## <a name="notify-events-with-logic-app"></a>Notificar eventos com o Logic Apps
O serviço [Azure Logic Apps](../logic-apps/logic-apps-overview.md) permite-lhe criar tarefas automatizadas para eventos recebidos de outros serviços. Nesta secção, vai configurar o Logic Apps para criar notificações de e-mail para os eventos enviados pelos sensores espaciais com a ajuda de um [Tópico do Event Grid](../event-grid/overview.md).

1. No painel de navegação esquerdo do [portal do Azure](https://portal.azure.com), clique em **Criar um recurso**

1. Procure e selecione o recurso **Logic App**. Clique em **Criar**.

1. Introduza um **Nome** para a aplicação lógica e selecione a **Subscrição**, o **Grupo de recursos** e a **Localização**. Clique em **Criar**.

    ![Criar Aplicação Lógica](./media/tutorial-facilities-events/create-logic-app.png)

1. Depois de implementada, abra a Aplicação Lógica e abra o painel **Logic App Designer** (Estruturador de Aplicações Lógicas). 

1. Selecione o acionador **When an Event Grid event occurs** (Quando ocorrer um evento do Event Grid). Quando lhe for pedido, **inicie sessão** no seu inquilino com a conta do Azure. Quando lhe for pedido, confirme **Allow access** (Permitir acesso) ao Event Grid. Clique em **Continue** (Continuar).

1. Na janela **When a resource event occurs (Preview)** (Quando ocorrer um evento de recurso (Pré-visualização)), 
    1. Selecione a **Subscrição** que utilizou para criar o Event Grid anteriormente,

    1. Selecione **Microsoft.EventGrid.Topics** como **Resource Type** (Tipo de Recurso),

    1. Selecione o recurso do Event Grid na caixa pendente **Resource Name** (Nome do Recurso).

    ![Criar Aplicação Lógica](./media/tutorial-facilities-events/logic-app-resource-event.png)

1. Clique no botão **New Step** (Novo Passo).

1. Na janela **Choose an action** (Escolher uma ação),
    1. Procure a expressão *parse json* (analisar json) e selecione a ação **Parse JSON**.

    1. Clique dentro do campo **Content** (Conteúdo) e selecione **Body** (Corpo), na lista **Dynamic content** (Conteúdo dinâmico).

    1. Clique em **Use sample to payload to generate schema** (Utilizar o payload de exemplo para gerar o esquema). Cole o payload JSON seguinte e clique em **Done** (Concluído).

        ```JSON
        {
        "id": "32162f00-a8f1-4d37-aee2-9312aabba0fd",
        "subject": "UdfCustom",
        "data": {
          "TopologyObjectId": "20efd3a8-34cb-4d96-a502-e02bffdabb14",
          "ResourceType": "Space",
          "Payload": "\"Air quality is poor.\"",
          "CorrelationId": "32162f00-a8f1-4d37-aee2-9312aabba0fd"
        },
        "eventType": "UdfCustom",
        "eventTime": "0001-01-01T00:00:00Z",
        "dataVersion": "1.0",
        "metadataVersion": "1",
        "topic": "/subscriptions/a382ee71-b48e-4382-b6be-eec7540cf271/resourceGroups/HOL/providers/Microsoft.EventGrid/topics/DigitalTwinEventGrid"
        }
        ```
    
    Este payload tem valores fictícios. A Aplicação Lógica utiliza este payload de exemplo para gerar um **Esquema**.
    
    ![Analisar JSON da Aplicação Lógica do Event Grid](./media/tutorial-facilities-events/logic-app-parse-json.png)

1. Clique no botão **New Step** (Novo Passo).

1. Na janela **Choose an action** (Escolher uma ação),
    1. Procure e selecione **Condition Control** (Controlo de Condições), na lista **Actions** (Ações). 

    1. Clique na primeira caixa de texto **Choose a value** (Escolher um valor) e selecione **eventType**, na lista **Dynamic content** (Conteúdo dinâmico), na janela **Parse JSON** (Analisar JSON).

    1. Clique na segunda caixa de texto **Choose a value** (Escolher um valor) e escreva *UdfCustom*.

    ![Analisar JSON da Aplicação Lógica do Event Grid](./media/tutorial-facilities-events/logic-app-condition.png)

1. Na janela **If true** (Se verdadeiro),
    1. Clique em **Add an action** (Adicionar uma ação) e selecione *Office 365 Outlook* (Outlook do Office 365).

    1. Na lista **Actions** (Ações), selecione **Send an email** (Enviar e-mail). Clique em **Sign in** (Iniciar sessão) e utilize as credenciais da sua conta de e-mail. Quando lhe for pedido, clique em **Allow access** (Permitir acesso).

    1. Na caixa **To** (Para), introduza o ID do seu e-mail para receber notificações. Em **Subject** (Assunto), introduza o texto *Digital Twins notification for poor air quality in space* (Notificação do Digital Twins para má qualidade do ar na divisão) e selecione **TopologyObjectId**, na lista **Dynamic content** (Conteúdo dinâmico), em **Parse JSON** (Analisar JSON).

    1. Em **Body** (Corpo), na mesma, janela, introduza um texto semelhante a: *Poor air quality detected in a room, and temperature needs to be adjusted* (Má qualidade do ar detetada numa divisão e a temperatura tem de ser ajustada). Pode adaptar esse texto com os elementos da lista **Dynamic content** (Conteúdo dinâmico), conforme mostrado abaixo.

    ![Enviar e-mail da Aplicação Lógica](./media/tutorial-facilities-events/logic-app-send-email.png)

1. Clique no botão **Save** (Guardar), na parte superior do painel **Logic App Designer** (Estruturador de Aplicações Lógicas).

1. Navegue para a pasta **_device-connectivity_** do exemplo do Digital Twin numa janela de comandos e execute `dotnet run` para fazer uma simulação dos dados do sensor.

Ao fim de alguns minutos, deverá começar a receber notificações por e-mail desta Aplicação Lógica. 

   ![Enviar e-mail da Aplicação Lógica](./media/tutorial-facilities-events/logic-app-notification.png)

Para deixar de receber esses e-mails, navegue para a sua **Aplicação Lógica**, no portal, e selecione o painel **Descrição Geral**. Clique em **Disable**.


## <a name="clean-up-resources"></a>Limpar recursos

Se já não quiser explorar mais o Azure Digital Twins para além deste ponto, não hesite em eliminar os recursos que foram criados no tutorial:

1. No menu do lado esquerdo do [portal do Azure](http://portal.azure.com), clique em **Todos os recursos**, selecione o seu grupo de recursos do Digital Twins e clique em **Eliminar**.
2. Se for necessário, também pode eliminar os exemplos de aplicações no computador de trabalho. 


## <a name="next-steps"></a>Passos seguintes

Pode avançar para o próximo tutorial e aprender a visualizar os dados do sensor, a analisar tendências e a detetar anomalias. 
> [!div class="nextstepaction"]
> [Tutorial: Visualize and analyze events from your Azure Digital Twins spaces using Time Series Insights](tutorial-facilities-analyze.md) (Tutorial: Visualizar e analisar eventos dos seus espaços do Azure Digital Twins com o Time Series Insights)

Também pode aprender mais sobre os gráficos de inteligência espacial e os modelos de objetos no Azure Digital Twins. 
> [!div class="nextstepaction"]
> [Understanding Digital Twins object models and spatial intelligence graph](concepts-objectmodel-spatialgraph.md) (Compreender os modelos de objetos e o gráfico de inteligência espacial do Digital Twins)