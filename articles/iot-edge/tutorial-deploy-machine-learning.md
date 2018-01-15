---
title: Implementar o Azure Machine Learning com limite de IoT do Azure | Microsoft Docs
description: "Implementar o Azure Machine Learning como um módulo para um dispositivo de limite"
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 12/13/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: a0131fdbbf926d59eae06089cde109649a1433b8
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/12/2018
---
# <a name="deploy-azure-machine-learning-as-an-iot-edge-module---preview"></a>Implementar o Azure Machine Learning como um módulo de limite de IoT – pré-visualização

Pode utilizar módulos de limite de IoT para implementar o código que implementa a lógica de negócio diretamente nos seus dispositivos de limite de IoT. Este tutorial explica-lhe implementar um módulo do Azure Machine Learning que prevê quando um dispositivo falha com base nos dados de sensores no dispositivo simulado contorno de IoT que criou no [implementar o Azure IoT Edge num dispositivo simulado no Windows] [ lnk-tutorial1-win] ou [Linux] [ lnk-tutorial1-lin] tutoriais. 

Neste tutorial, ficará a saber como: 

> [!div class="checklist"]
> * Criar um módulo do Azure Machine Learning
> * Emitir um contentor do módulo para um registo de contentor do Azure
> * Implementar um módulo do Azure Machine Learning para o seu dispositivo de limite de IoT
> * Dados de vista gerada

O módulo do Azure Machine Learning que criar neste tutorial lê os dados de temperatura gerados pelo seu dispositivo e apenas mensagens upstream quando envia para o IoT Hub do Azure prevê a uma falha (denominada uma anomalias). 


## <a name="prerequisites"></a>Pré-requisitos

* O dispositivo de limite de IoT do Azure que criou no início rápido ou primeiro tutorial.
* A cadeia de ligação do IoT Hub para o IoT hub que liga o dispositivo de limite de IoT.
* Uma conta do Azure Machine Learning. Para criar uma conta, siga as instruções em [criar o Azure Machine Learning contas e instalar o Azure Machine Learning Workbench](../machine-learning/preview/quickstart-installation.md#create-azure-machine-learning-accounts). Não é necessário instalar a aplicação do workbench para este tutorial. 
* Módulo Gestão do Azure ML no seu computador. Para configurar o ambiente e criar uma conta, siga as instruções em [a configuração de gestão de modelo](https://docs.microsoft.com/azure/machine-learning/preview/deployment-setup-configuration).

## <a name="create-the-azure-ml-container"></a>Criar o contentor do Azure ML
Nesta secção, transferir os ficheiros de modelo treinado e convertê-los para um contentor do Azure ML.  

No computador a executar o módulo de gestão do Azure ML, transfira e guarde [iot_score.py](https://github.com/Azure/ai-toolkit-iot-edge/blob/master/IoT%20Edge%20anomaly%20detection%20tutorial/iot_score.py) e [model.pkl](https://github.com/Azure/ai-toolkit-iot-edge/blob/master/IoT%20Edge%20anomaly%20detection%20tutorial/model.pkl) partir do Toolkit de IoT do Azure ML no GitHub. Estes ficheiros definem o treinado do machine learning modelo que implementa para o seu dispositivo de limite de Iot. 

Utilize o modelo treinado para criar um contentor que pode ser implementado em dispositivos de limite de IoT.

```cmd
az ml service create realtime --model-file model.pkl -f iot_score.py -n machinelearningmodule -r python
```
O nome do serviço, *machinelearningmodule* neste exemplo, torna-se o nome da imagem de contentor do docker.

### <a name="view-the-container-repository"></a>Visualizar o repositório de contentor

Verifique que a imagem do contentor foi criada e armazenada no repositório de contentor do Azure que está associado ao seu ambiente do machine learning com êxito.

1. No [portal do Azure](https://portal.azure.com), aceda a **todos os serviços** e selecione **os registos do contentor**.
2. Selecione o seu registo. O nome deve começar por **mlcr** e pertence a subscrição que utilizou para configurar a gestão de módulo, a localização e o grupo de recursos.
3. Selecione **chaves de acesso**
4. Copiar o **servidor de início de sessão**, **Username**, e **palavra-passe**.  É necessário para aceder ao registo de dispositivos de limite.
5. Selecione **repositórios**
6. Selecione **machinelearningmodule**
7. Tem agora o caminho de imagem completa do contentor. Tome nota deste caminho de imagem para a secção seguinte. Este deve ter o seguinte aspeto: **.azureacr.io/machinelearningmodule:1 < registry_name >**

## <a name="add-registry-credentials-to-your-edge-device"></a>Adicione as credenciais do registo para o seu dispositivo de limite

Adicione as credenciais para o registo para o tempo de execução do Edge no computador onde está a executar o dispositivo de limite. Este comando permite o acesso de tempo de execução para solicitar o contentor.

Linux:
   ```cmd
   sudo iotedgectl login --address <registry-login-server> --username <registry-username> --password <registry-password> 
   ```

Windows:
   ```cmd
   iotedgectl login --address <registry-login-server> --username <registry-username> --password <registry-password> 
   ```

## <a name="run-the-solution"></a>Executar a solução

1. No [portal do Azure](https://portal.azure.com), navegue até ao seu IoT hub.
1. Aceda a **IoT Edge (pré-visualização)** e selecione o seu dispositivo de limite de IoT.
1. Selecione **definir módulos**.
1. Se anteriormente tiver implementado o módulo de tempSensor para o seu dispositivo de limite de IoT, poderá autopopulate. Se ainda não estiver na sua lista de módulos, adicioná-lo.
    1. Selecione **Adicionar módulo IoT Edge**.
    2. No **nome** campo, introduza `tempSensor`.
    3. No **URI de imagem** campo, introduza `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview`.
    4. Selecione **Guardar**.
1. Adicione a máquina que criou o módulo de aprendizagem.
    1. Selecione **Adicionar módulo IoT Edge**.
    1. No **nome** campo, introduza`machinelearningmodule`
    1. No **imagem** campo, introduza o seu endereço de imagem; por exemplo `<registry_name>.azurecr.io/machinelearningmodule:1`.
    1. Selecione **Guardar**.
1. Volta a **adicionar módulos** passo, selecione **seguinte**.
1. No **especificar rotas** passo, copie o JSON abaixo na caixa de texto. A rota primeiro transportes mensagens do sensor de temperatura para o módulo de aprendizagem máquina através de ponto final "amlInput", que é o ponto final que utilizam todos os módulos do Azure Machine Learning. A segunda rota transportes mensagens do módulo machine learning ao IoT Hub. Esta rota, ' amlOutput"é o ponto final que utilizam todos os módulos do Azure Machine Learning para dados de saída e"$upstream"indica o IoT Hub. 

    ```json
    {
        "routes": {
            "sensorToMachineLearning":"FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/machinelearningmodule/inputs/amlInput\")",
            "machineLearningToIoTHub": "FROM /messages/modules/machinelearningmodule/outputs/amlOutput INTO $upstream"
        }
    }
    ``` 

1. Selecione **seguinte**. 
1. No **rever modelo** passo, selecione **submeter**. 
1. Regressar à página de detalhes do dispositivo e selecionar **atualizar**.  Deverá ver o novo **machinelearningmodule** executar juntamente com o **tempSensor** módulo e os módulos de tempo de execução de limite de IoT.

## <a name="view-generated-data"></a>Dados de vista gerada

Pode ver as mensagens do dispositivo para a nuvem que o dispositivo de limite de IoT envia utilizando a extensão do Toolkit de IoT do Azure para Visual Studio Code. 

1. No Visual Studio Code, selecione **dispositivos do Hub IoT**. 
2. Selecione **...**  , em seguida, selecione **definir a cadeia de ligação do IoT Hub** no menu. 

   ![Dispositivos do Hub IoT mais menu](./media/tutorial-deploy-machine-learning/set-connection.png)

3. Na caixa de texto que se abre no topo da página, introduza a cadeia de ligação de iothubowner para o seu IoT Hub. O dispositivo de limite de IoT deve aparecer na lista de dispositivos do IoT Hub.
4. Selecione **...**  novamente, em seguida, selecione **iniciar a monitorização de mensagem D2C**.
5. Observe as mensagens feitos tempSensor cada cinco segundos, qual machinelearningmodule acrescenta com a respetiva avaliação do Estado de funcionamento do dispositivo. 

   ![Resposta de ML do Azure no corpo da mensagem](./media/tutorial-deploy-machine-learning/ml-output.png)

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, implementou um módulo de limite de IoT com tecnologia do Azure Machine Learning. Pode continuar a sessão em qualquer um dos outros tutoriais para saber mais sobre outras formas de limite de IoT do Azure pode ajudar a que ativar dados em informações empresariais no limite.

> [!div class="nextstepaction"]
> [Implementar uma função do Azure como um módulo](tutorial-deploy-function.md)

<!--Links-->
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md
