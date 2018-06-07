---
title: Visualização de dados em tempo real de dados de sensor da IoT Hub do Azure – Power BI | Microsoft Docs
description: Utilize o Power BI para visualizar dados relativos à temperatura e humidade recolhidos a partir do sensor e enviados para o seu hub IoT do Azure.
author: rangv
manager: ''
keywords: visualização de dados em tempo real, visualização de dados em direto, visualização de dados de sensor
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 4/11/2018
ms.author: rangv
ms.openlocfilehash: a3c54fe635fe0f8988c321684a815e9896922587
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34634350"
---
# <a name="visualize-real-time-sensor-data-from-azure-iot-hub-using-power-bi"></a>Visualizar dados de sensores em tempo real do IoT Hub do Azure através do Power BI

![Diagrama de ponto a ponto](media/iot-hub-get-started-e2e-diagram/4.png)


[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

## <a name="what-you-learn"></a>O que irá aprender

Irá aprender a visualizar dados de sensores em tempo real que recebe o seu hub IoT do Azure pelo Power BI. Se pretender tentar visualizar os dados no seu IoT hub com as Web Apps, inicie consulte [Web Apps do Azure Utilize para visualizar dados de sensores em tempo real do IoT Hub do Azure](iot-hub-live-data-visualization-in-web-apps.md).

## <a name="what-you-do"></a>O que fazer

- Prepare o seu IoT hub para acesso a dados através da adição de um grupo de consumidores.
- Criar, configurar e executar uma tarefa de Stream Analytics para transferência de dados a partir do seu IoT hub à sua conta Power BI.
- Criar e publicar um relatório do Power BI para visualizar os dados.

## <a name="what-you-need"></a>Do que precisa

- Tutorial [configurar o seu dispositivo](iot-hub-raspberry-pi-kit-node-get-started.md) concluir que inclui os seguintes requisitos:
  - Uma subscrição ativa do Azure.
  - Um hub IoT do Azure na sua subscrição.
  - Uma aplicação de cliente que envia mensagens para o seu hub IoT do Azure.
- Uma conta do Power BI. ([Experimente o Power BI gratuitamente](https://powerbi.microsoft.com/))

[!INCLUDE [iot-hub-get-started-create-consumer-group](../../includes/iot-hub-get-started-create-consumer-group.md)]

## <a name="create-configure-and-run-a-stream-analytics-job"></a>Criar, configurar e executar uma tarefa de Stream Analytics

### <a name="create-a-stream-analytics-job"></a>Criar uma tarefa do Stream Analytics

1. No [portal do Azure](https://portal.azure.com), clique em **Criar um recurso** > **Internet das Coisas** > **Tarefa do Stream Analytics**.
1. Introduza as seguintes informações para a tarefa.

   **Nome da tarefa**: o nome da tarefa. O nome tem de ser globalmente exclusivo.

   **Grupo de recursos**: utilizar o mesmo grupo de recursos que utiliza o seu IoT hub.

   **Localização**: utilizar a mesma localização como o grupo de recursos.

   **Afixar ao dashboard**: marque esta opção para facilitar o acesso ao hub IoT a partir do dashboard.

   ![Criar uma tarefa de Stream Analytics no Azure](media/iot-hub-live-data-visualization-in-power-bi/2_create-stream-analytics-job-azure.png)

1. Clique em **Criar**.

### <a name="add-an-input-to-the-stream-analytics-job"></a>Adicionar uma entrada à tarefa do Stream Analytics

1. Abra a tarefa de Stream Analytics.
1. Em **Topologia da Tarefa**, clique em **Entradas**.
1. No **entradas** painel, clique em **adicionar**e, em seguida, introduza as seguintes informações:

   **Alias de entrada**: O alias exclusivo para a entrada.

   **Origem**: selecione **IoT hub**.

   **Grupo de consumidores**: selecione o grupo de consumidores que acabou de criar.
1. Clique em **Criar**.

   ![Adicionar uma entrada para uma tarefa de Stream Analytics no Azure](media/iot-hub-live-data-visualization-in-power-bi/3_add-input-to-stream-analytics-job-azure.png)

### <a name="add-an-output-to-the-stream-analytics-job"></a>Adicionar uma saída à tarefa do Stream Analytics

1. Em **Topologia de Tarefas**, clique em **Saídas**.
1. No **saídas** painel, clique em **adicionar**e, em seguida, introduza as seguintes informações:

   **Alias de saída**: o alias exclusivo da saída.

   **Sink**: selecione **Power BI**.
1. Clique em **autorizar**e, em seguida, inicie sessão na sua conta Power BI.
1. Depois de autorização, introduza as seguintes informações:

   **Área de trabalho de grupo**: selecione a sua área de trabalho do grupo de destino.

   **Nome do conjunto de dados**: introduza um nome de conjunto de dados.

   **Nome de tabela**: introduza um nome de tabela.
1. Clique em **Criar**.

   ![Adicionar uma saída para uma tarefa de Stream Analytics no Azure](media/iot-hub-live-data-visualization-in-power-bi/4_add-output-to-stream-analytics-job-azure.png)

### <a name="configure-the-query-of-the-stream-analytics-job"></a>Configurar a consulta da tarefa do Stream Analytics

1. Em **Topologia de Tarefas**, clique em **Consulta**.
1. Substitua `[YourInputAlias]` pelo alias de entrada da tarefa.
1. Substitua `[YourOutputAlias]` pelo alias de saída da tarefa.
1. Clique em **Guardar**.

   ![Adicionar uma consulta para uma tarefa de Stream Analytics no Azure](media/iot-hub-live-data-visualization-in-power-bi/5_add-query-stream-analytics-job-azure.png)

### <a name="run-the-stream-analytics-job"></a>Executar a tarefa do Stream Analytics

Na tarefa do Stream Analytics, clique em **Iniciar** > **Agora** > **Iniciar**. Assim que a tarefa for iniciada com êxito, o estado da tarefa é alterado de **Parado** para **Em execução**.

![Execute uma tarefa de Stream Analytics no Azure](media/iot-hub-live-data-visualization-in-power-bi/6_run-stream-analytics-job-azure.png)

## <a name="create-and-publish-a-power-bi-report-to-visualize-the-data"></a>Criar e publicar um relatório do Power BI para visualizar os dados

1. Certifique-se de que a aplicação de exemplo está em execução no seu dispositivo. Se não, pode consultar os tutoriais em [configurar o seu dispositivo](https://docs.microsoft.com/azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started).
1. Inicie sessão no seu [Power BI](https://powerbi.microsoft.com/en-us/) conta.
1. Vá para a área de trabalho de grupo que definiu quando criou o resultado da tarefa de Stream Analytics.
1. Clique em **conjuntos de dados de transmissão em fluxo**.

   Deverá ver o conjunto de dados que especificou quando criou a saída da tarefa do Stream Analytics.
1. Em **AÇÕES**, clique no primeiro ícone para criar um relatório.

   ![Criar um relatório do Microsoft Power BI](media/iot-hub-live-data-visualization-in-power-bi/7_create-power-bi-report-microsoft.png)

1. Crie um gráfico de linhas para mostrar a temperatura em tempo real ao longo do tempo.
   1. Na página de criação do relatório, adicione um gráfico de linhas.
   1. No painel **Campos**, expanda a tabela que especificou quando criou a saída da tarefa do Stream Analytics.
   1. Arraste **EventEnqueuedUtcTime** para **Eixo** no painel **Visualizações**.
   1. Arraste **temperatura** para **Valores**.

      Acabou de criar um gráfico de linhas. O eixo X apresenta a data e hora no fuso horário UTC. O eixo Y apresenta a temperatura do sensor.

      ![Adicionar um gráfico de linhas de temperatura para um relatório do Microsoft Power BI](media/iot-hub-live-data-visualization-in-power-bi/8_add-line-chart-for-temperature-to-power-bi-report-microsoft.png)

1. Crie outro gráfico de linhas para mostrar a humidade em tempo real ao longo do tempo. Para tal, siga os mesmos passos acima e coloque **EventEnqueuedUtcTime** no eixo x e **humidade** no eixo y.

   ![Adicionar um gráfico de linhas para humidade para um relatório do Microsoft Power BI](media/iot-hub-live-data-visualization-in-power-bi/9_add-line-chart-for-humidity-to-power-bi-report-microsoft.png)

1. Clique em **Guardar** para guardar o relatório.
1. Clique em **ficheiro** > **publicar na web**.
1. Clique em **código de incorporação criar**e, em seguida, clique em **publicar**.

Está a fornecido a ligação do relatório que pode partilhar com qualquer pessoa para acesso de relatório e um fragmento de código para integrar o relatório no seu blogue ou Web site.

![Publicar um relatório do Microsoft Power BI](media/iot-hub-live-data-visualization-in-power-bi/10_publish-power-bi-report-microsoft.png)

A Microsoft também oferece o [aplicações móveis do Power BI](https://powerbi.microsoft.com/en-us/documentation/powerbi-power-bi-apps-for-mobile-devices/) para ver e interagir com os seus relatórios e dashboards do Power BI no seu dispositivo móvel.

## <a name="next-steps"></a>Passos Seguintes

Utilizou com êxito Power BI para visualizar dados de sensores em tempo real a partir do seu hub IoT do Azure.
Há uma maneira alternativa para visualizar dados a partir do IoT Hub do Azure. Consulte [Web Apps do Azure Utilize para visualizar dados de sensores em tempo real do IoT Hub do Azure](iot-hub-live-data-visualization-in-web-apps.md).

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
