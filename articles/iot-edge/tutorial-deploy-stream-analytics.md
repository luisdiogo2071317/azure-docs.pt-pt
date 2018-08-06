---
title: Tutorial – Implementar tarefas ASA para dispositivos Azure IoT Edge | Microsoft Docs
description: Neste tutorial, vai implementar o Azure Stream Analytics como um módulo num dispositivo IoT Edge
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/25/2018
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: f2ef53ee53eb2e95d84fc11f3190f62d0e3c2455
ms.sourcegitcommit: 96f498de91984321614f09d796ca88887c4bd2fb
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39413880"
---
# <a name="tutorial-deploy-azure-stream-analytics-as-an-iot-edge-module-preview"></a>Tutorial: Implementar o Azure Stream Analytics como um módulo do IoT Edge (pré-visualização)

Muitas soluções de IoT utilizam os serviços de análise para obter informações sobre os dados à medida que são recebidos na cloud a partir dos dispositivos IoT. Com o Azure IoT Edge, pode utilizar a lógica do [Azure Stream Analytics] [azure-stream] e movê-la para o dispositivo propriamente dito. Ao processar os fluxos de telemetria na periferia, pode reduzir a quantidade de dados carregados, bem como o tempo que demora a reagir a informações acionáveis.

O Azure IoT Edge e o Azure Stream Analytics estão integrados para que possa criar uma tarefa do Azure Stream Analytics no portal do Azure e, em seguida, implementá-la como um módulo do IoT Edge sem código adicional.  

O Azure Stream Analytics fornece uma sintaxe de consulta altamente estruturada para análise de dados na cloud e em dispositivos IoT Edge. Para obter mais informações sobre o Azure Stream Analytics no IoT Edge, veja a [documentação do Azure Stream Analytics](../stream-analytics/stream-analytics-edge.md).

O módulo do Stream Analytics neste tutorial calcula a temperatura média num período de 30 segundos graduais. Quando esta média atingir 70, o módulo envia e alerta para o dispositivo tomar medidas. Neste caso, essa ação é repor o sensor de temperatura simulada. Num ambiente de produção, pode utilizar esta funcionalidade para encerrar uma máquina ou tomar medidas preventivas quando a temperatura atingir níveis perigosos. 

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Crie uma tarefa do Azure Stream Analytics para processar dados na periferia.
> * Ligue a nova tarefa do Azure Stream Analytics a outros módulos do IoT Edge.
> * Implemente a tarefa do Azure Stream Analytics num dispositivo IoT Edge a partir do portal do Azure.

>[!NOTE]
>Os módulos do Azure Stream Analytics para IoT Edge estão em [pré-visualização pública](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Pré-requisitos

Um dispositivo Azure IoT Edge:

* Pode seguir os passos no início rápido para [dispositivos Linux](quickstart-linux.md) ou [Windows](quickstart.md) para utilizar o seu computador de desenvolvimento ou uma máquina virtual como um dispositivo Edge.
* O módulo do Azure Machine Learning não suporta processadores ARM.

Recursos da cloud:

* Um [Hub IoT](../iot-hub/iot-hub-create-through-portal.md) no escalão standard no Azure. 


## <a name="create-an-azure-stream-analytics-job"></a>Criar uma tarefa do Azure Stream Analytics

Nesta secção, vai criar uma tarefa do Azure Stream Analytics para utilizar os dados do seu hub IoT, consultar os dados de telemetria enviados do seu dispositivo e, em seguida, reencaminhar os resultados para um contentor de armazenamento de Blobs do Azure. Para obter mais informações, veja a secção "Descrição geral" da [documentação do Stream Analytics][azure-stream]. 

### <a name="create-a-storage-account"></a>Criar uma conta de armazenamento

É necessária uma conta de Armazenamento do Azure para as tarefas do Azure Stream Analytics, para agir como um ponto final do resultado da tarefa. O exemplo nesta secção utiliza o tipo de armazenamento de Blobs. Para obter mais informações, veja a secção "Blobs" da [documentação do Armazenamento do Azure][azure-storage].

1. No portal do Azure, aceda a **Criar um recurso**, introduza **Conta de armazenamento** na caixa de pesquisa e, em seguida, selecione **Conta de armazenamento do Azure - blob, ficheiro, tabela, fila**.

2. No painel **Criar conta de armazenamento**, introduza um nome para a conta de armazenamento, selecione a mesma localização onde está armazenado o hub IoT, selecione o mesmo grupo de recursos do hub IoT e, em seguida, selecione **Criar**. Anote o nome para utilização posterior.

    ![Criar uma conta de armazenamento][1]


### <a name="create-a-stream-analytics-job"></a>Criar uma tarefa do Stream Analytics

1. No portal do Azure, aceda a **Criar um recurso** > **Internet das Coisas** e, em seguida, selecione **Tarefa do Stream Analytics**.

2. No painel **Nova Tarefa do Stream Analytics** efetue os seguintes passos:

   1. Na caixa **Nome da tarefa**, escreva um nome de tarefa.
   
   2. Utilize o mesmo **Grupo de recursos** e **Localização** do hub IoT. 

      > [!NOTE]
      > Atualmente, as tarefas do Azure Stream Analytics no IoT Edge não são suportadas na região E.U.A. Oeste 2. 

   3. Em **Ambiente de alojamento**, selecione **Edge**.
    
3. Selecione **Criar**.

4. Na tarefa criada, em **Topologia da tarefa**, abra **Entradas**.

   ![Entrada do Azure Stream Analytics](./media/tutorial-deploy-stream-analytics/asa_input.png)

5. Selecione **Adicionar entrada de fluxo** e, em seguida, selecione **Hub do Edge**.

6. No painel **Nova entrada**, introduza **temperatura** como alias de entrada. 

7. Selecione **Guardar**.

8. Em **Topologia da Tarefa**, abra **Saídas**.

   ![Saída do Azure Stream Analytics](./media/tutorial-deploy-stream-analytics/asa_output.png)

9. Selecione **Adicionar** e, em seguida, selecione **Hub do Edge**.

10. No painel **Nova saída**, introduza **alerta** como alias de saída. 

11. Selecione **Guardar**.

12. Em **Topologia da Tarefa**, selecione **Consulta** e, em seguida, substitua o texto predefinido pela seguinte consulta, que cria um alerta se a temperatura média da máquina num período de 30 segundos atingir 70 graus:

    ```sql
    SELECT  
        'reset' AS command 
    INTO 
       alert 
    FROM 
       temperature TIMESTAMP BY timeCreated 
    GROUP BY TumblingWindow(second,30) 
    HAVING Avg(machine.temperature) > 70
    ```

13. Selecione **Guardar**.

14. Em **Configurar**, selecione **Definições do IoT Edge**.

15. Selecione a **Conta de armazenamento** no menu pendente.

16. Para o campo **Contentor**, selecione **Criar novo** e forneça um nome para o contentor de armazenamento. 

17. Selecione **Guardar**. 


## <a name="deploy-the-job"></a>Implementar a tarefa

Está agora pronto para implementar a tarefa do Azure Stream Analytics no seu dispositivo IoT Edge.

1. No portal do Azure, no hub IoT, aceda a **IoT Edge** e, em seguida, abra a página de detalhes do dispositivo IoT Edge.

2. Selecione **Definir módulos**.  

   Se já implementou o módulo tempSensor neste dispositivo, este poderá ser automaticamente populado. Se não for o caso, adicione o módulo com os seguintes passos:

   1. Clique em **Adicionar** e selecione **Módulo do IoT Edge**.
   2. Para o nome, escreva **tempSensor**.
   3. Para o URI da imagem, introduza **mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0**. 
   4. Deixe as outras definições inalteradas.
   5. Selecione **Guardar**.

3. Adicione a tarefa do Azure Stream Analytics Edge com os seguintes passos:

   1. Clique em **Adicionar** e selecione **Módulo do Azure Stream Analytics**.
   2. Selecione a sua subscrição e a tarefa do Azure Stream Analytics Edge que criou. 
   3. Selecione **Guardar**.

4. Selecione **Seguinte**.

5. Substitua o valor predefinido em **Rotas** pelo código seguinte. Atualize _{moduleName}_ com o nome do módulo do Azure Stream Analytics. O módulo deve ter o mesmo nome da tarefa a partir da qual foi criado. 

    ```json
    {
        "routes": {
            "telemetryToCloud": "FROM /messages/modules/tempSensor/* INTO $upstream",
            "alertsToCloud": "FROM /messages/modules/{moduleName}/* INTO $upstream",
            "alertsToReset": "FROM /messages/modules/{moduleName}/* INTO BrokeredEndpoint(\"/modules/tempSensor/inputs/control\")",
            "telemetryToAsa": "FROM /messages/modules/tempSensor/* INTO BrokeredEndpoint(\"/modules/{moduleName}/inputs/temperature\")"
        }
    }
    ```

6. Selecione **Seguinte**.

7. No passo **Rever Implementação**, selecione **Submeter**.

8. Regresse à página de detalhes do dispositivo e, em seguida, selecione **Atualizar**.  

    Deverá ver o novo módulo do Stream Analytics em execução, juntamente com o módulo do agente do IoT Edge e o hub do IoT Edge.

    ![Saída do módulo][7]

## <a name="view-data"></a>Ver dados

Agora, pode aceder ao dispositivo IoT Edge para verificar a interação entre o módulo do Azure Stream Analytics e o módulo tempSensor.

1. Verifique se todos os módulos estão em execução no Docker:

   ```cmd/sh
   iotedge list  
   ```
<!--
   ![Docker output][8]
-->
2. Veja todos os dados de métricas e registos de sistema. Utilize o nome do módulo do Stream Analytics:

   ```cmd/sh
   iotedge logs -f {moduleName}  
   ```

Deverá conseguir ver a temperatura da máquina a aumentar gradualmente até atingir 70 graus durante 30 segundos. Em seguida, o módulo do Stream Analytics aciona uma reposição e a temperatura da máquina baixa novamente para 21. 

   ![Registo do Docker][9]

## <a name="clean-up-resources"></a>Limpar recursos 

<!--[!INCLUDE [iot-edge-quickstarts-clean-up-resources](../../includes/iot-edge-quickstarts-clean-up-resources.md)] -->

Se avançar para o próximo artigo recomendado, pode manter os recursos e as configurações que já criou e reutilizá-los.

Caso contrário, pode eliminar as configurações locais e os recursos do Azure criados neste artigo para evitar custos. 

> [!IMPORTANT]
> A eliminação de recursos do Azure e do grupo de recursos é irreversível. Após a eliminação, o grupo de recursos e todos os recursos nele contidos são eliminados permanentemente. Confirme que não elimina acidentalmente o grupo de recursos ou recursos errados. Se tiver criado o Hub IoT no interior de um grupo de recursos existente que contenha recursos que pretende manter, elimine apenas o recurso do Hub IoT, em vez de eliminar o grupo de recursos.
>

Para eliminar apenas o Hub IoT, execute o seguinte comando com o nome do hub e o nome do grupo de recursos:

```azurecli-interactive
az iot hub delete --name MyIoTHub --resource-group TestResources
```


Para eliminar todo o grupo de recursos por nome:

1. Inicie sessão no [Portal do Azure](https://portal.azure.com) e clique em **Grupos de recursos**.

2. Na caixa de texto **Filtrar por nome...**, escreva o nome do grupo de recursos que contém o seu Hub IoT. 

3. À direita do seu grupo de recursos na lista de resultados, clique em **...** e em **Eliminar grupo de recursos**.

<!--
   ![Delete](./media/iot-edge-quickstarts-clean-up-resources/iot-edge-delete-resource-group.png)
-->
4. É-lhe pedido que confirme a eliminação do grupo de recursos. Escreva o nome do grupo de recursos novamente para confirmar e, em seguida, clique em **Eliminar**. Após alguns instantes, o grupo de recursos e todos os recursos contidos no mesmo são eliminados.

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, configurou uma tarefa do Azure Stream Analytics para analisar dados a partir do dispositivo IoT Edge. Em seguida, carregou este módulo do Azure Stream Analytics no dispositivo do IoT Edge para processar e reagir ao aumento da temperatura localmente, bem como enviar o fluxo de dados agregados para a cloud. Para ver de que forma o Azure IoT Edge pode criar mais soluções para a sua empresa, avance para os outros tutoriais.

> [!div class="nextstepaction"] 
> [Implementar o modelo do Azure Machine Learning como um módulo][lnk-ml-tutorial]

<!-- Images. -->
[1]: ./media/tutorial-deploy-stream-analytics/storage.png
[4]: ./media/tutorial-deploy-stream-analytics/add_device.png
[5]: ./media/tutorial-deploy-stream-analytics/asa_job.png
[6]: ./media/tutorial-deploy-stream-analytics/set_module.png
[7]: ./media/tutorial-deploy-stream-analytics/module_output2.png
[8]: ./media/tutorial-deploy-stream-analytics/docker_output.png
[9]: ./media/tutorial-deploy-stream-analytics/docker_log.png
[10]: ./media/tutorial-deploy-stream-analytics/storage_settings.png
[11]: ./media/tutorial-deploy-stream-analytics/temp_module.png


<!-- Links -->
[lnk-what-is-iot-edge]: what-is-iot-edge.md
[lnk-module-dev]: module-development.md
[iot-hub-get-started-create-hub]: ../../includes/iot-hub-get-started-create-hub.md
[azure-iot]: https://docs.microsoft.com/azure/iot-hub/
[azure-storage]: https://docs.microsoft.com/azure/storage/
[azure-stream]: https://docs.microsoft.com/azure/stream-analytics/
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-quickstart-win]: quickstart.md
[lnk-quickstart-lin]: quickstart-linux.md
[lnk-module-tutorial]: tutorial-csharp-module.md
[lnk-ml-tutorial]: tutorial-deploy-machine-learning.md

