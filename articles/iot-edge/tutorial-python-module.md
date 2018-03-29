---
title: Módulo de Python de limite de IoT do Azure | Microsoft Docs
description: Criar um módulo de limite de IoT com o código de Python e implementá-la para um dispositivo de limite
services: iot-edge
keywords: ''
author: shizn
manager: timlt
ms.author: xshi
ms.date: 03/18/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: d5bad277e6a54b23f0e3ef7321e82d212ae885d3
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/28/2018
---
# <a name="develop-and-deploy-a-python-iot-edge-module-to-your-simulated-device---preview"></a>Desenvolver e implementar um módulo de Python IoT limite para o seu dispositivo simulado – pré-visualização

Pode utilizar módulos de limite de IoT para implementar o código que implementa a lógica de negócio diretamente nos seus dispositivos de limite de IoT. Este tutorial explica-lhe como criar e implementar um módulo de limite de IoT que filtra os dados de sensores. Irá utilizar o dispositivo simulado contorno de IoT que criou na implementação do Azure IoT limite num dispositivo simulado [Windows] [ lnk-tutorial1-win] ou [Linux] [ lnk-tutorial1-lin]tutoriais. Neste tutorial, ficará a saber como:    

> [!div class="checklist"]
> * Utilize o Visual Studio Code para criar um módulo Python do limite de IoT
> * Utilize o Visual Studio Code e Docker para criar uma imagem de docker e publicá-lo no seu registo 
> * Implementar o módulo para o seu dispositivo de limite de IoT
> * Dados de vista gerada


O módulo de limite de IoT que criou neste tutorial filtra os dados de temperatura gerados pelo seu dispositivo. Só envia mensagens upstream se a temperatura for superior um limiar especificado. Este tipo de análise no limite é útil para reduzir a quantidade de dados comunicados para e armazenados na nuvem. 

> [!IMPORTANT]
> Atualmente o módulo de Python pode apenas ser em execução nos contentores de Linux amd64. Não é possível funcionar em contentores de Windows ou contentores de baseadas em ARM. 

## <a name="prerequisites"></a>Pré-requisitos

* O dispositivo de limite de IoT do Azure que criou no início rápido ou primeiro tutorial.
* A cadeia de ligação da chave primária do dispositivo IoT Edge.  
* [Visual Studio Code](https://code.visualstudio.com/). 
* [Extensão de limite de IoT do Azure para Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge). 
* [Extensão do Python para Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-python.python). 
* [Docker](https://docs.docker.com/engine/installation/) no mesmo computador que tenha o Visual Studio Code. A edição de Comunidade (CE) é suficiente para este tutorial. 
* [Python](https://www.python.org/downloads/).
* [PIP](https://pip.pypa.io/en/stable/installing/#installation) para instalar pacotes de Python.

## <a name="create-a-container-registry"></a>Criar um registo de contentores
Neste tutorial, utilize a extensão de limite de IoT do Azure para o VS Code para construir um módulo e criar um **imagem contentor** partir dos ficheiros. Em seguida, push esta imagem para um **registo** que armazena e gere as imagens. Por fim, implementar a imagem a partir do seu registo para executar no seu dispositivo de limite de IoT.  

Pode utilizar qualquer registo compatível com o Docker para este tutorial. Dois populares Docker registo serviços disponíveis na nuvem são [registo de contentor do Azure](https://docs.microsoft.com/azure/container-registry/) e [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags). Este tutorial utiliza o registo de contentor do Azure. 

1. No [portal do Azure](https://portal.azure.com), selecione **crie um recurso** > **contentores** > **registo de contentor do Azure** .
2. Atribua o seu registo de um nome, escolha uma subscrição, escolha um grupo de recursos e o SKU de conjunto **básico**. 
3. Selecione **Criar**.
4. Assim que for criado o seu registo de contentor, navegue até ao mesmo e selecione **chaves de acesso**. 
5. Ativar/desativar **utilizador de Admin** para **ativar**.
6. Copie os valores para **servidor de início de sessão**, **Username**, e **palavra-passe**. Irá utilizar estes valores mais tarde no tutorial. 

## <a name="create-an-iot-edge-module-project"></a>Criar um projeto do módulo de limite de IoT
Os passos seguintes mostram como criar um módulo de IoT Edge Python com Visual Studio Code e a extensão de limite de IoT do Azure.
1. No Visual Studio Code, selecione **vista** > **integrada Terminal** para abrir o terminal integrado VS Code.
2. No terminal integrado, introduza o seguinte comando para instalar (ou atualizar) as **cookiecutter**:

    ```cmd/sh
    pip install -U cookiecutter
    ```

3. Crie um projeto para o módulo de novo. O comando seguinte cria a pasta do projeto, **FilterModule**, com o seu repositório de contentor. O parâmetro de `image_repository` deve estar no formato `<your container registry name>.azurecr.io/filtermodule` se estiver a utilizar o registo de contentor do Azure. Introduza o seguinte comando na pasta de trabalho atual:

    ```cmd/sh
    cookiecutter --no-input https://github.com/Azure/cookiecutter-azure-iot-edge-module module_name=FilterModule image_repository=<your container registry address>/filtermodule
    ```
 
4. Selecione **ficheiro** > **Abrir pasta**.
5. Navegue para o **FilterModule** pasta e clique em **selecionar pasta** para abrir o projeto no VS Code.
6. No Explorador de VS Code, clique em **main.py** para abri-lo.
7. Na parte superior do **FilterModule** espaço de nomes, importar o `json` biblioteca:

    ```python
    import json
    ```

8. Adicionar o `TEMPERATURE_THRESHOLD` e `TWIN_CALLBACKS` nos contadores global. O limiar de temperatura define o valor pode exceder a temperatura medida para que os dados a ser enviadas ao IoT Hub.

    ```python
    TEMPERATURE_THRESHOLD = 25
    TWIN_CALLBACKS = 0
    ```

9. Atualizar a função `receive_message_callback` com abaixo conteúdo.

    ```python
    # receive_message_callback is invoked when an incoming message arrives on the specified 
    # input queue (in the case of this sample, "input1").  Because this is a filter module, 
    # we will forward this message onto the "output1" queue.
    def receive_message_callback(message, hubManager):
        global RECEIVE_CALLBACKS
        global TEMPERATURE_THRESHOLD
        message_buffer = message.get_bytearray()
        size = len(message_buffer)
        message_text = message_buffer[:size].decode('utf-8')
        print ( "    Data: <<<%s>>> & Size=%d" % (message_text, size) )
        map_properties = message.properties()
        key_value_pair = map_properties.get_internals()
        print ( "    Properties: %s" % key_value_pair )
        RECEIVE_CALLBACKS += 1
        print ( "    Total calls received: %d" % RECEIVE_CALLBACKS )
        data = json.loads(message_text)
        if "machine" in data and "temperature" in data["machine"] and data["machine"]["temperature"] > TEMPERATURE_THRESHOLD:
            map_properties.add("MessageType", "Alert")
            print("Machine temperature %s exceeds threshold %s" % (data["machine"]["temperature"], TEMPERATURE_THRESHOLD))
        hubManager.forward_event_to_output("output1", message, 0)
        return IoTHubMessageDispositionResult.ACCEPTED
    ```

10. Adicionar uma nova função `device_twin_callback`. Esta função será invocada quando as propriedades pretendidas são atualizadas.

    ```python
    # device_twin_callback is invoked when twin's desired properties are updated.
    def device_twin_callback(update_state, payload, user_context):
        global TWIN_CALLBACKS
        global TEMPERATURE_THRESHOLD
        print ( "\nTwin callback called with:\nupdateStatus = %s\npayload = %s\ncontext = %s" % (update_state, payload, user_context) )
        data = json.loads(payload)
        if "desired" in data and "TemperatureThreshold" in data["desired"]:
            TEMPERATURE_THRESHOLD = data["desired"]["TemperatureThreshold"]
        if "TemperatureThreshold" in data:
            TEMPERATURE_THRESHOLD = data["TemperatureThreshold"]
        TWIN_CALLBACKS += 1
        print ( "Total calls confirmed: %d\n" % TWIN_CALLBACKS )
    ```

11. Na classe `HubManager`, adicione uma nova linha para o `__init__` método ao inicializar o `device_twin_callback` acabou de adicionar a função.

    ```python
    # sets the callback when a twin's desired properties are updated.
    self.client.set_device_twin_callback(device_twin_callback, self)
    ```


12. Guarde este ficheiro.

## <a name="create-a-docker-image-and-publish-it-to-your-registry"></a>Criar uma imagem de Docker e publicá-lo no seu registo

1. Iniciar sessão para Docker, introduzindo o comando seguinte no terminal VS Code integrado: 
     
   ```csh/sh
   docker login -u <username> -p <password> <Login server>
   ```
        
   Utilize o nome de utilizador, a palavra-passe e o servidor de início de sessão que copiou do seu registo de contentor do Azure quando o criou.

2. No Explorador de VS Code, clique com botão direito do **module.json** do ficheiro e clique em **compilação e o limite de Push de IoT a imagem do Docker módulo**. Na caixa de lista pendente de pop-up na parte superior da janela VS Code, selecione a sua plataforma de contentor, por exemplo, **amd64** para o contentor de Linux. O VS Code containerize o `main.py` e dependências necessárias, em seguida, enviá-lo para o registo de contentor especificado. Poderá demorar alguns minutos para que a primeira vez compilar a imagem.

3. Pode obter o endereço de imagem de contentor completa com etiqueta com o código de VS integrado terminal. Para obter mais informações sobre a definição de push e de compilação, pode consultar o `module.json` ficheiro.

## <a name="add-registry-credentials-to-edge-runtime"></a>Adicionar credenciais de registo ao tempo de execução do contorno
Adicione as credenciais para o registo para o tempo de execução do Edge no computador onde está a executar o dispositivo de limite. Estas credenciais conceder o acesso de tempo de execução para o contentor de extração. 

- Para o Windows, execute o seguinte comando:
    
    ```cmd/sh
    iotedgectl login --address <your container registry address> --username <username> --password <password> 
    ```

- Para Linux, execute o seguinte comando:
    
    ```cmd/sh
    sudo iotedgectl login --address <your container registry address> --username <username> --password <password> 
    ```

## <a name="run-the-solution"></a>Executar a solução

1. No [portal do Azure](https://portal.azure.com), navegue até ao seu IoT hub.
2. Aceda a **IoT Edge (pré-visualização)** e selecione o seu dispositivo IoT Edge.
3. Selecione **definir módulos**. 
2. Verifique se o **tempSensor** módulo é preenchido automaticamente. Se não for, utilize os seguintes passos para adicioná-lo:
    1. Selecione **Adicionar módulo IoT Edge**.
    2. No **nome** campo, introduza `tempSensor`.
    3. No **URI de imagem** campo, introduza `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview`.
    4. Deixe as outras definições inalteradas e clique em **guardar**.
9. Adicionar o **filterModule** módulo que criou nas secções anteriores. 
    1. Selecione **Adicionar módulo IoT Edge**.
    2. No **nome** campo, introduza `filterModule`.
    3. No **URI de imagem** campo, introduza o seu endereço de imagem; por exemplo `<your container registry address>/filtermodule:0.0.1-amd64`. É possível localizar o endereço de imagem completa da secção anterior.
    4. Verifique o **ativar** caixa para que pode editar o duplo de módulo. 
    5. Substitua o JSON na caixa de texto para o duplo módulo com o seguinte JSON: 

        ```json
        {
           "properties.desired":{
              "TemperatureThreshold":25
           }
        }
        ```
 
    6. Clique em **Guardar**.
10. Clique em **Seguinte**.
11. No **especificar rotas** passo, copie o JSON abaixo na caixa de texto. Módulos publicar todas as mensagens para o tempo de execução do limite. As regras de declarativas em tempo de execução definem onde as mensagens de fluxo. Neste tutorial, precisa de dois rotas. A rota primeiro transportes mensagens do sensor de temperatura para o módulo de filtro através de ponto final "input1", que é o ponto final que configurou com o **FilterMessages** processador. A segunda rota transportes mensagens do módulo de filtro ao IoT Hub. Nesta rota `upstream` é um destino especial que diz ao Hub Edge para enviar mensagens para o IoT Hub. 

    ```json
    {
       "routes":{
          "sensorToFilter":"FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/filterModule/inputs/input1\")",
          "filterToIoTHub":"FROM /messages/modules/filterModule/outputs/output1 INTO $upstream"
       }
    }
    ```

4. Clique em **Seguinte**.
5. No **rever modelo** passo, clique em **submeter**. 
6. Regressar à página de detalhes do dispositivo IoT limite e clique em **atualizar**. Deverá ver o novo **filtermodule** executar juntamente com o **tempSensor** módulo e a **IoT Edge runtime**. 

## <a name="view-generated-data"></a>Dados de vista gerada

Para monitorizar as mensagens da nuvem enviados a partir do seu dispositivo de limite de IoT ao seu IoT hub do dispositivo:
1. Configure a extensão do Toolkit de IoT do Azure com a cadeia de ligação do IoT hub: 
    1. Abra o Explorador de VS Code selecionando **vista** > **Explorer**. 
    3. No Explorador de, clique em **dispositivos do HUB IOT** e, em seguida, clique em **...** . Clique em **definir a cadeia de ligação do IoT Hub** e introduza a cadeia de ligação para o IoT hub que o dispositivo de limite de IoT liga na janela de pop-up. 

        Para localizar a cadeia de ligação, clique no mosaico do hub IoT no portal do Azure e, em seguida, clique em **políticas de acesso partilhado**. No **políticas de acesso partilhado**, clique em de **iothubowner** política e copie a ligação do IoT Hub da cadeia no **iothubowner** janela.   

1. Para monitorizar os dados que chegam ao IoT hub, selecione **vista** > **paleta de comando** e procure o **IoT: iniciar a monitorização de mensagem D2C** comandos de menu. 
2. Para parar a monitorização de dados, utilize o **IoT: parar a monitorização D2C mensagem** comandos de menu. 

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, vai criar um módulo de limite de IoT que contém o código para filtrar dados não processados gerados pelo seu dispositivo de limite de IoT. Pode continuar a sessão em qualquer um dos seguintes tutoriais para saber mais sobre outras formas de limite de IoT do Azure pode ajudar a que ativar dados em informações empresariais no limite.

> [!div class="nextstepaction"]
> [Implementar a função do Azure como um módulo](tutorial-deploy-function.md)
> [implementar o Azure Stream Analytics como um módulo](tutorial-deploy-stream-analytics.md)


<!-- Links -->
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md

<!-- Images -->
[1]: ./media/tutorial-csharp-module/programcs.png
[2]: ./media/tutorial-csharp-module/build-module.png
[3]: ./media/tutorial-csharp-module/docker-os.png
