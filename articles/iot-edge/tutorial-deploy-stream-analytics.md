---
title: "Implementação do Azure Stream Analytics com o limite de IoT do Azure | Microsoft Docs"
description: "Implementar o Azure Stream Analytics como um módulo para um dispositivo de limite"
services: iot-edge
keywords: 
author: msebolt
manager: timlt
ms.author: v-masebo
ms.date: 11/28/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: f40fb81fc03e796b906db12bf3bf6904b27b46eb
ms.sourcegitcommit: c4cc4d76932b059f8c2657081577412e8f405478
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/11/2018
---
# <a name="deploy-azure-stream-analytics-as-an-iot-edge-module---preview"></a>Implementar o Azure Stream Analytics como um módulo de limite de IoT – pré-visualização

Dispositivos de IoT podem produzir grandes quantidades de dados. Para reduzir a quantidade de dados carregados ou para eliminar a latência reportadas round-trip das informações acionável sobre, os dados, por vezes, tem analisados ou processados antes de atingir a nuvem.

Limite de IoT do Azure tira partido do serviço do Azure pré-criadas extremidade de IoT módulos para uma implementação rápida. [O Azure Stream Analytics] [ azure-stream] é um desse módulo. Pode criar uma tarefa do Azure Stream Analytics a partir do seu portal e, em seguida, aceda ao portal do Azure IoT Hub para implementá-lo como um módulo de limite de IoT. 

O Azure Stream Analytics fornece uma sintaxe de consulta caixas estruturados para análise de dados na nuvem e no limite de IoT dispositivos. Para obter mais informações sobre o Azure Stream Analytics no limite do IoT, consulte [documentação do Azure Stream Analytics](../stream-analytics/stream-analytics-edge.md).

Este tutorial explica-lhe como criar uma tarefa do Azure Stream Analytics e a respetiva implementação num dispositivo de limite de IoT. Se o fizer, permite-lhe processar um fluxo de telemetria local diretamente no dispositivo e gerar alertas unidade ação imediata no dispositivo. 

O tutorial apresenta dois módulos: 
* Um temperatura simulada sensor módulo (tempSensor) que gera dados de temperatura 20 a 120 graus, aumentadas em 1 a cada cinco segundos. 
* Um módulo de Stream Analytics que repõe a tempSensor quando a média de 30 segundo atinge 70. Num ambiente de produção, poderá utilizar esta funcionalidade para encerradas uma máquina ou tomar medidas preventivas quando a temperatura atinge níveis perigosos. 

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Crie uma tarefa do Azure Stream Analytics para processar dados na extremidade.
> * Ligar a nova tarefa de Stream Analytics do Azure com outros módulos de limite de IoT.
> * Implemente a tarefa de Stream Analytics do Azure para um dispositivo de limite de IoT.

## <a name="prerequisites"></a>Pré-requisitos

* Um IoT hub. 
* O dispositivo que criou e configurado no início rápido ou nos artigos sobre a implementação do Azure IoT Edge num dispositivo simulado [Windows] [ lnk-tutorial1-win] ou no [Linux] [ lnk-tutorial1-lin]. É necessário saber a chave de ligação do dispositivo e o ID de dispositivo. 
* Docker em execução no seu dispositivo de limite de IoT.
    * [Instalar Docker no Windows][lnk-docker-windows].
    * [Instalar Docker no Linux][lnk-docker-linux].
* Python 2.7.x no seu dispositivo de limite de IoT.
    * [Instalar o Python 2.7 no Windows][lnk-python].
    * A maioria das distribuições de Linux, incluindo Ubuntu, já tem o Python 2.7 instalado. Para garantir que o pip está instalado, utilize o seguinte comando: `sudo apt-get install python-pip`.

## <a name="create-an-azure-stream-analytics-job"></a>Criar uma tarefa do Azure Stream Analytics

Nesta secção, vai criar uma tarefa do Azure Stream Analytics para colocar os dados a partir do seu IoT hub, consultar os dados de telemetria enviada do seu dispositivo e, em seguida, reencaminhar os resultados para um contentor de armazenamento de Blobs do Azure. Para obter mais informações, consulte a secção "Descrição geral" da [documentação do Stream Analytics][azure-stream]. 

### <a name="create-a-storage-account"></a>Criar uma conta do Storage

É necessária uma conta de armazenamento do Azure para fornecer um ponto final para ser utilizado como uma saída na sua tarefa do Azure Stream Analytics. O exemplo nesta secção utiliza o tipo de armazenamento de Blobs. Para obter mais informações, consulte a secção "Blobs" o [documentação do Storage do Azure][azure-storage].

1. No portal do Azure, aceda a **crie um recurso**, introduza **conta de armazenamento** na caixa de pesquisa e, em seguida, selecione **conta de armazenamento - BLOBs, ficheiro, tabela, fila**.

2. No **criar conta de armazenamento** painel, introduza um nome para a sua conta de armazenamento, selecione a mesma localização onde está armazenado o seu IoT hub e, em seguida, selecione **criar**. Tenha em atenção o nome para utilização posterior.

    ![Criar uma conta do Storage][1]

3. Vá para a conta de armazenamento que acabou de criar e, em seguida, selecione **procurar blobs**. 

4. Criar um novo contentor para o módulo do Azure Stream Analytics armazenar dados, defina o acesso de nível para **contentor**e, em seguida, selecione **OK**.

    ![definições de armazenamento][10]

### <a name="create-a-stream-analytics-job"></a>Criar uma tarefa do Stream Analytics

1. No portal do Azure, aceda a **crie um recurso** > **Internet das coisas**e, em seguida, selecione **tarefa do Stream Analytics**.

2. No **nova tarefa de Stream Analytics** painel, efetue o seguinte procedimento:

    a. No **nome da tarefa** caixa, escreva um nome de tarefa.
    
    b. Em **ambiente de alojamento**, selecione **Edge**.
    
    c. Os campos restantes, utilize os valores predefinidos.

    > [!NOTE]
    > Atualmente, as tarefas do Azure Stream Analytics IoT Edge não são suportadas na região EUA oeste 2. 

3. Selecione **Criar**.

4. Esta tarefa criada, em **tarefa topologia**, selecione **entradas**e, em seguida, selecione **adicionar**.

5. No **nova entrada** painel, efetue o seguinte procedimento:

    a. No **alias de entrada** box, introduza **temperatura**.
    
    b. No **tipo de origem** caixa, selecione **fluxo de dados**.
    
    c. Os campos restantes, utilize os valores predefinidos.

   ![Entrada de Stream Analytics do Azure](./media/tutorial-deploy-stream-analytics/asa_input.png)

6. Selecione **Criar**.

7. Em **tarefa topologia**, selecione **saídas**e, em seguida, selecione **adicionar**.

8. No **nova saída** painel, efetue o seguinte procedimento:

    a. No **alias de saída** caixa, escreva **alerta**.
    
    b. Os campos restantes, utilize os valores predefinidos. 
    
    c. Selecione **Criar**.

   ![Saída do Azure Stream Analytics](./media/tutorial-deploy-stream-analytics/asa_output.png)


9. Em **tarefa topologia**, selecione **consulta**e, em seguida, substitua o texto predefinido com a seguinte consulta:

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

10. Selecione **Guardar**.

## <a name="deploy-the-job"></a>Implementar a tarefa

Agora está pronto para implementar a tarefa de Stream Analytics do Azure no seu dispositivo de limite de IoT.

1. No portal do Azure, no seu IoT hub, aceda a **IoT Edge (pré-visualização)**e, em seguida, abra a página de detalhes para o seu dispositivo de limite de IoT.

2. Selecione **definir módulos**.  
    Se tiver implementado anteriormente o módulo de tempSensor neste dispositivo, poderá autopopulate. Se não existir, adicione o módulo da seguinte forma:

   a. Selecione **Adicionar módulo IoT Edge**.

   b. Para o nome, escreva **tempSensor**.
    
   c. Para a URI de imagem, introduza **microsoft/azureiotedge-simulated-temperatura-sensor: 1.0-pré-visualização**. 

   d. Deixe as outras definições inalteradas.
   
   e. Selecione **Guardar**.

3. Para adicionar a tarefa do Azure Stream Analytics Edge, selecione **importação do Azure Stream Analytics IoT Edge módulo**.

4. Selecione a sua subscrição e a tarefa de limite do Azure Stream Analytics que criou. 

5. Selecione a sua subscrição e a conta de armazenamento que criou e, em seguida, selecione **guardar**.

    ![módulo de conjunto][6]

6. Copie o nome do seu módulo Azure Stream Analytics. 

    ![módulo de temperatura][11]

7. Para configurar as rotas, selecione **seguinte**.

8. Copie o seguinte código para **rotas**. Substitua _{moduleName}_ com o nome do módulo que copiou:

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

9. Selecione **seguinte**.

10. No **rever modelo** passo, selecione **submeter**.

11. Regressar à página de detalhes do dispositivo e, em seguida, selecione **atualizar**.  
    Deverá ver o novo módulo do Stream Analytics em execução, juntamente com o módulo de agente de IoT Edge e o hub IoT Edge.

    ![saída do módulo][7]

## <a name="view-data"></a>Ver dados

Agora pode aceder ao seu dispositivo de limite de IoT para verificar a interação entre o módulo do Azure Stream Analytics e o módulo de tempSensor.

1. Verifique se todos os módulos em execução no Docker:

   ```cmd/sh
   docker ps  
   ```

   ![saída de docker][8]

2. Ver todos os dados de métricas e registos de sistema. Utilize o nome do módulo do Stream Analytics:

   ```cmd/sh
   docker logs -f {moduleName}  
   ```

Deverá conseguir ver a temperatura da máquina, aumentar gradualmente até atingir graus 70 durante 30 segundos. Em seguida, o módulo de Stream Analytics aciona uma reposição e a temperatura de máquina remove novamente para 21. 

   ![registo de docker][9]


## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, configurou um contentor de armazenamento do Azure e uma tarefa de análise de transmissão em fluxo para analisar os dados a partir do seu dispositivo de limite de IoT. Em seguida, carregar um módulo do Azure Stream Analytics personalizado para mover dados do seu dispositivo, através da sequência de para um blob para transferência. Para ver como o limite de IoT do Azure pode criar soluções mais para a sua empresa, avançar para os outros tutoriais.

> [!div class="nextstepaction"] 
> [Implementar um modelo do Azure Machine Learning como um módulo][lnk-ml-tutorial]

<!-- Images. -->
[1]: ./media/tutorial-deploy-stream-analytics/storage.png
[4]: ./media/tutorial-deploy-stream-analytics/add_device.png
[5]: ./media/tutorial-deploy-stream-analytics/asa_job.png
[6]: ./media/tutorial-deploy-stream-analytics/set_module.png
[7]: ./media/tutorial-deploy-stream-analytics/module_output.png
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
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md
[lnk-module-tutorial]: tutorial-csharp-module.md
[lnk-ml-tutorial]: tutorial-deploy-machine-learning.md

[lnk-docker-windows]: https://docs.docker.com/docker-for-windows/install/ 
[lnk-docker-linux]: https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/
[lnk-python]: https://www.python.org/downloads/
