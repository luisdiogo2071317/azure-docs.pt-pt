---
title: Integrar a solução de monitorização remota com o Azure Data Lake Store | Microsoft Docs
description: Saiba como integrar a solução de monitorização remota do Azure Data Lake Store utilizando uma tarefa do Azure Stream Analytics.
+services: ''
+suite: iot-suite
+author: philmea
+manager: timlt
+ms.author: philmea
+ms.date: 04/029/2018
+ms.topic: article
+ms.service: iot-suite
ms.openlocfilehash: 75ad9e9ac79dca4db902d7e72511a384dd769463
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/17/2018
---
# <a name="integrate-the-remote-monitoring-solution-with-azure-data-lake-store"></a>Integrar a solução de monitorização remota com o Azure Data Lake Store

Poderá ter avançadas requisitos de análise para além de que é oferecido na solução de monitorização remota. O Azure Data Lake Store é ideal para esta aplicação porque, pode armazenar dados de grandes abrangente e diversificados de conjuntos de dados, bem como integrar com o Azure Data Lake Analytics para fornecer a análise a pedido.

Este procedimentos, utilizará uma tarefa de Stream Analytics do Azure para dados de fluxo do IoT hub na sua solução de monitorização remota para um Azure Data Lake Store.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este procedimentos, irá precisar do seguinte:

* [Implementar a solução pré-configurada de monitorização remota](../iot-accelerators/iot-accelerators-remote-monitoring-deploy.md).
  * A solução de monitorização remota implementará o IoT hub e a tarefa do Azure Stream Analytics utilizada neste artigo para a sua subscrição do Azure.
* [Implementar um Azure Data Lake Store](/data-lake-store/data-lake-store-get-started-portal.md)
  * O Data Lake Store devem ser implementado para a mesma região que a sua solução de monitorização remota.
  * [Crie uma pasta](/data-lake-store/data-lake-store-get-started-portal.md#createfolder) com o nome "de transmissão em fluxo" na sua conta.

## <a name="create-a-consumer-group"></a>Criar um grupo de consumidores

Crie um grupo de consumidores dedicado no hub IoT da sua solução de monitorização remota. Este será utilizado pela tarefa de Stream Analytics para dados de transmissão em fluxo para o Data Lake Store.

> [!NOTE]
> Grupos de consumidores são utilizados por aplicações para obter dados do IoT Hub do Azure. Deve criar um novo grupo de consumidores para todos os consumidores de cinco saída. Pode criar até 32 grupos de consumidores.

1. Inicie sessão no Portal do Azure.

1. No portal do Azure, clique em de **nuvem Shell** botão.

    ![Ícone de início do portal](media/iot-suite-integrate-data-lake/portal-launch-icon.png)

1. Execute este comando para criar um novo grupo de consumidores:

```azurecli-interactive
az iot hub consumer-group create --hub-name contoso-rm30263 --name streamanalyticsjob --resource-group contoso-rm
```

> [!NOTE]
> Utilize o grupo de recursos e nomes de hub IoT da solução de monitorização remota.

## <a name="create-stream-analytics-job"></a>Criar tarefa do Stream Analytics

Crie uma tarefa do Azure Stream Analytics para transmitir os dados do seu IoT hub para o Azure Data Lake store.

1. Clique em **crie um recurso**, selecione de Internet das coisas no Marketplace e clique em **tarefa do Stream Analytics**.

    ![Nova tarefa de Stream Analytics](media/iot-suite-integrate-data-lake/new-stream-analytics-job.png)

1. Introduza um nome de tarefa e selecione o grupo de recursos e subscrição adequado.

1. Selecione uma localização a próxima ou na mesma região que o Data Lake Store. Aqui vamos utilizar EUA Leste.

1. Certifique-se para deixar o ambiente de alojamento como predefinição **nuvem**.

1. Clique em **Criar**.

    ![Criar tarefa do Stream Analytics](media/iot-suite-integrate-data-lake/create-stream-analytics-job.png)

## <a name="configure-the-stream-analytics-job"></a>Configurar a tarefa de Stream Analytics

1. Vá para o **tarefa do Stream Analytics** no grupo de recursos de solução de monitorização remota.

1. Na página Descrição geral, clique em **entradas**.

    ![Página de descrição geral](media/iot-suite-integrate-data-lake/stream-analytics-overview.png)

1. Clique em **adicionar a entrada de fluxo** e selecione **IoT Hub** da lista pendente.

    ![Adicionar a entrada](media/iot-suite-integrate-data-lake/stream-analytics-add-input.png)

1. No separador de entrada, introduza um alias de entrada de **IoTHub**.

1. Do consumidor grupo pendente, selecione o grupo de consumidores que criou anteriormente. Aqui vamos utilizar **streamanalyticsjob**.

    ![Selecione a entrada](media/iot-suite-integrate-data-lake/stream-analytics-new-input.png)

1. Clique em **Guardar**.

1. Na página Descrição geral, clique em **saídas**.

    ![Adicione o arquivo Data Lake](media/iot-suite-integrate-data-lake/stream-analytics-overview-2.png)

1. Clique em **adicionar** e selecione **Data Lake Store** da lista pendente.

    ![Adicionar a saída](media/iot-suite-integrate-data-lake/stream-analytics-output.png)

1. Num novo separador de saída, introduza um alias de saída de **DataLakeStore**.

1. Selecione a conta de Data Lake Store que criou nos passos anteriores e fornecer a estrutura de pastas para dados de fluxo para o arquivo.

1. No campo de formato de data, introduza **/streaming/ {date} / {time}**. Deixe o formato de data predefinido de DD/MM/DD e o formato de hora do HH.

    ![Forneça a estrutura de pastas](media/iot-suite-integrate-data-lake/stream-analytics-new-output.png)

1. Clique em **autorizar**.

    Terá de autorizar com o Data Lake Store para conceder o acesso de escrita de tarefa do Stream analytics para o sistema de ficheiros.

    ![Autorizar o Stream Analytics para o arquivo Data Lake](media/iot-suite-integrate-data-lake/stream-analytics-out-authorize.png)

    Verá um pop-up e depois de fecha o pop-up autorizar botão estarão esbatido depois de concluída a autorização.

    > [!NOTE]
    > Se vir um erro na janela de pop-up, abra uma nova janela do browser no modo de Incognito e tente novamente.

1. Clique em **Guardar**.

## <a name="edit-the-stream-analytics-query"></a>Editar a consulta do Stream Analytics

O Azure Stream Analytics utiliza uma linguagem de consulta do SQL Server semelhante para especificar uma origem de entrada que fluxos de dados, transformar os dados como pretendida e de saída a uma variedade de destinos de armazenamento ou processamento.

1. No separador de descrição geral, clique em **Editar consulta**.

    ![Editar Consulta](media/iot-suite-integrate-data-lake/stream-analytics-edit-query.png)

1. No editor de consultas, substitua a [YourOutputAlias] e [YourInputAlias] marcadores de posição com valores definidos anteriormente.

    ```sql
    SELECT
        *, System.Timestamp as time
    INTO
        DataLakeStore
    FROM
        IoTHub
    ```

    ![Consulta do Stream Analytics](media/iot-suite-integrate-data-lake/stream-analytics-query.png)

1. Clique em **Guardar**.
1. Clique em **Sim** para aceitar as alterações.

## <a name="start-the-stream-analytics-job"></a>Iniciar a tarefa de Stream Analytics

1. No separador de descrição geral, clique em **iniciar**.

    ![Iniciar a tarefa do Stream Analytics](media/iot-suite-integrate-data-lake/stream-analytics-start.png)

1. No separador de tarefa de início, clique em **personalizada**.

1. Definir a hora personalizada para voltar atrás algumas horas para recolher dados a partir de quando o seu dispositivo foi iniciada a transmissão em fluxo.

1. Clique em **iniciar**.

    ![Escolha data personalizada](media/iot-suite-integrate-data-lake/stream-analytics-start-custom.png)

    Aguarde até que a tarefa entra no Estado, de execução se observar erros poderia ser da sua consulta, certifique-se verificar se a sintaxe está correta.

    ![Tarefa em execução](media/iot-suite-integrate-data-lake/stream-analytics-running.png)

    A tarefa de transmissão em fluxo irá começar a ler dados a partir do seu IoT Hub e armazenar os dados no Data Lake Store. Pode demorar alguns minutos para que os dados iniciar a aparecer no Data Lake Store.

## <a name="explore-the-streaming-data"></a>Explorar os dados de transmissão em fluxo

1. Vá para o Data Lake Store.

1. No separador de descrição geral, clique em **Explorador de dados**.

1. No Explorador de dados, desagregar o **/ de transmissão em fluxo** pasta. Verá pastas criadas com o formato dd/MM/DD/HH.

    ![Explore os dados de transmissão em fluxo](media/iot-suite-integrate-data-lake/data-lake-store-data-explorer.png)

    Irá ver ficheiros json com um ficheiro por hora.

    ![Explore os dados de transmissão em fluxo](media/iot-suite-integrate-data-lake/data-lake-store-file-preview.png)

## <a name="next-steps"></a>Próximos Passos

Azure Data Lake Analytics pode ser utilizado para efetuar análise de macrodados nos seus conjuntos de dados do Data Lake Store. Saber mais sobre o [documentação do Data Lake Analytics](https://docs.microsoft.com/en-us/azure/data-lake-analytics).
