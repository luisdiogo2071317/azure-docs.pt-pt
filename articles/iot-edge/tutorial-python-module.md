---
title: Módulo Azure IoT Edge Python | Microsoft Docs
description: Criar um módulo do IoT Edge com código Python e implementá-lo num dispositivo Edge
author: shizn
manager: ''
ms.author: xshi
ms.date: 03/18/2018
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 88d772306cb9e67216b380aa885284ebedc77b5f
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34632113"
---
# <a name="develop-and-deploy-a-python-iot-edge-module-to-your-simulated-device---preview"></a>Desenvolver e implementar um módulo do IoT Edge do Python no seu dispositivo simulado – pré-visualização

Pode utilizar os módulos do IoT Edge para implementar código que aplica a sua lógica de negócio diretamente nos seus dispositivos IoT Edge. Este tutorial explica-lhe como criar e implementar um módulo do IoT Edge que filtra dados de sensores. Irá utilizar o dispositivo IoT Edge simulado que criou na Implementação do Azure IoT Edge num dispositivo simulado em tutoriais do [Windows][lnk-tutorial1-win] ou do [Linux][lnk-tutorial1-lin]. Neste tutorial, ficará a saber como:    

> [!div class="checklist"]
> * Utilizar o Visual Studio Code para criar um módulo do IoT Edge com Python
> * Utilizar o Visual Studio Code e o Docker para criar uma imagem do Docker e publicá-la no seu registo 
> * Implementar o módulo no seu dispositivo IoT Edge
> * Ver os dados gerados


O módulo do IoT Edge que criou neste tutorial filtra os dados de temperatura gerados pelo seu dispositivo. Envia apenas mensagens de origem caso a temperatura seja superior a um limiar especificado. Este tipo de análise no Edge é útil para reduzir a quantidade de dados comunicados e armazenados na cloud. 

> [!IMPORTANT]
> Atualmente, o módulo do Python só pode ser executado em contentores Linux amd64; não pode ser executado em contentores Windows ou contentores baseados em ARM. 

## <a name="prerequisites"></a>Pré-requisitos

* O dispositivo Azure IoT Edge que criou no início rápido ou no primeiro tutorial.
* A cadeia de ligação da chave primária do dispositivo IoT Edge.  
* [Visual Studio Code](https://code.visualstudio.com/). 
* [Extensão Azure IoT Edge para Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge). 
* [Extensão do Python para Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-python.python). 
* [Docker](https://docs.docker.com/engine/installation/) no mesmo computador que tenha o Visual Studio Code. A Community Edition (CE) é suficiente para este tutorial. 
* [Python](https://www.python.org/downloads/).
* [Pip](https://pip.pypa.io/en/stable/installing/#installation) para instalar pacotes do Python (tipicamente incluído com a instalação Python).

## <a name="create-a-container-registry"></a>Criar um registo de contentores
Neste tutorial, vai utilizar a extensão Azure IoT Edge para VS Code para criar um módulo e criar uma **imagem de contentor** a partir dos ficheiros. Em seguida, vai enviar essa imagem para um **registo** que armazena e gere as suas imagens. Por fim, vai implementar a imagem a partir do registo para ser executada no seu dispositivo IoT Edge.  

Pode utilizar qualquer registo compatível com o Docker neste tutorial. O [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) e o [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags) são dois populares serviços de registo do Docker que estão disponíveis na cloud. Este tutorial utiliza o Azure Container Registry. 

1. No [Portal do Azure](https://portal.azure.com), selecione **Criar um recurso** > **Contentores** > **Azure Container Registry**.
2. Dê um nome ao registo, escolha uma subscrição, escolha um grupo de recursos e defina o SKU como **Básico**. 
3. Selecione **Criar**.
4. Assim que o registo de contentores for criado, navegue para o mesmo e selecione **Chaves de acesso**. 
5. Mude **Utilizador administrador** para **Ativar**.
6. Copie os valores de **Servidor de início de sessão**, **Nome de utilizador** e **Palavra-passe**. Vai utilizá-los mais à frente no tutorial. 

## <a name="create-an-iot-edge-module-project"></a>Criar um projeto do módulo do IoT Edge
Os passos seguintes mostram-lhe como criar um módulo do Python do IoT Edge Python com o Visual Studio Code e a extensão Azure IoT Edge.
1. No Visual Studio Code, selecione **Vista** > **Terminal Integrado** para abrir o terminal integrado do VS Code.
2. No terminal integrado, introduza o seguinte comando para instalar (ou atualizar) **cookiecutter** (sugerimos que o faça num ambiente virtual ou como uma instalação pelo utilizador como mostrado abaixo):

    ```cmd/sh
    pip install --upgrade --user cookiecutter
    ```

3. Crie um projeto para o módulo novo. O comando seguinte cria a pasta do projeto, **FilterModule**, com o repositório do contentor. Se estiver a utilizar um registo do Azure Container Registry, o parâmetro de `image_repository` deverá ter o formato `<your container registry name>.azurecr.io/filtermodule`. Introduza o seguinte comando na pasta de trabalho atual:

    ```cmd/sh
    cookiecutter --no-input https://github.com/Azure/cookiecutter-azure-iot-edge-module module_name=FilterModule image_repository=<your container registry address>/filtermodule
    ```
 
4. Selecione **Ficheiro** > **Abrir Pasta**.
5. Navegue para a pasta **FilterModule** e clique em **Selecionar Pasta** para abrir o projeto no VS Code.
6. No explorador do VS Code, clique em **main.py** para o abrir.
7. Na parte superior do espaço de nomes **FilterModule**, importe a biblioteca `json`:

    ```python
    import json
    ```

8. Adicione `TEMPERATURE_THRESHOLD`, `RECEIVE_CALLBACKS` e `TWIN_CALLBACKS` aos contadores globais. O limiar de temperatura define o valor que a temperatura medida tem de exceder para os dados serem enviados para o Hub IoT.

    ```python
    TEMPERATURE_THRESHOLD = 25
    TWIN_CALLBACKS = RECEIVE_CALLBACKS = 0
    ```

9. Atualize a função `receive_message_callback` com o conteúdo abaixo.

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
        print("    Data: <<<{}>>> & Size={:d}".format(message_text, size))
        map_properties = message.properties()
        key_value_pair = map_properties.get_internals()
        print("    Properties: {}".format(key_value_pair))
        RECEIVE_CALLBACKS += 1
        print("    Total calls received: {:d}".format(RECEIVE_CALLBACKS))
        data = json.loads(message_text)
        if "machine" in data and "temperature" in data["machine"] and data["machine"]["temperature"] > TEMPERATURE_THRESHOLD:
            map_properties.add("MessageType", "Alert")
            print("Machine temperature {} exceeds threshold {}".format(data["machine"]["temperature"], TEMPERATURE_THRESHOLD))
        hubManager.forward_event_to_output("output1", message, 0)
        return IoTHubMessageDispositionResult.ACCEPTED
    ```

10. Adicione uma nova função `device_twin_callback`. Esta função será invocada quando as propriedades pretendidas forem atualizadas.

    ```python
    # device_twin_callback is invoked when twin's desired properties are updated.
    def device_twin_callback(update_state, payload, user_context):
        global TWIN_CALLBACKS
        global TEMPERATURE_THRESHOLD
        print("\nTwin callback called with:\nupdateStatus = {}\npayload = {}\ncontext = {}".format(update_state, payload, user_context))
        data = json.loads(payload)
        if "desired" in data and "TemperatureThreshold" in data["desired"]:
            TEMPERATURE_THRESHOLD = data["desired"]["TemperatureThreshold"]
        if "TemperatureThreshold" in data:
            TEMPERATURE_THRESHOLD = data["TemperatureThreshold"]
        TWIN_CALLBACKS += 1
        print("Total calls confirmed: {:d}\n".format(TWIN_CALLBACKS))
    ```

11. Na classe `HubManager`, adicione uma nova linha ao método `__init__` para inicializar a função `device_twin_callback` que adicionou.

    ```python
    # sets the callback when a twin's desired properties are updated.
    self.client.set_device_twin_callback(device_twin_callback, self)
    ```


12. Guarde este ficheiro.

## <a name="create-a-docker-image-and-publish-it-to-your-registry"></a>Criar uma imagem do Docker e publicá-la no seu registo

1. Introduza o comando seguinte no terminal integrado do VS Code e inicie sessão no Docker. 
     
   ```csh/sh
   docker login -u <username> -p <password> <Login server>
   ```
        
   Utilize o nome do utilizador, palavra-passe e servidor de início de sessão que copiou do Azure Container Registry aquando da respetiva criação.

2. No explorador do VS Code, clique com botão direito do rato no ficheiro **module.json** e clique em **Compilar e Emitir imagem de Docker do módulo IoT Edge**. Na caixa pendente pop-up na parte superior da janela do VS Code, selecione a plataforma do contentor, por exemplo, **amd64** para o contentor Linux. O VS Code contentoriza `main.py` e as dependências necessárias e envia-o para o registo de contentor especificado. Pode demorar vários minutos na primeira vez que criar a imagem.

3. Pode obter o endereço da imagem de contentor completo com a etiqueta no terminal integrado do VS Code. Para obter mais informações sobre a definição de compilação e emissão, pode ver o ficheiro `module.json`.

## <a name="add-registry-credentials-to-edge-runtime"></a>Adicionar as credenciais do registo ao runtime do Edge
Adicione as credenciais do registo ao runtime do Edge no computador onde está a executar o dispositivo Edge. Estas credenciais oferecem ao runtime acesso para extrair o contentor. 

- No Windows, execute o seguinte comando:
    
    ```cmd/sh
    iotedgectl login --address <your container registry address> --username <username> --password <password> 
    ```

- No Linux, execute o seguinte comando:
    
    ```cmd/sh
    sudo iotedgectl login --address <your container registry address> --username <username> --password <password> 
    ```

## <a name="run-the-solution"></a>Executar a solução

1. No [Portal do Azure](https://portal.azure.com), navegue para o seu hub IoT.
2. Aceda a **IoT Edge (pré-visualização)** e selecione o seu dispositivo IoT Edge.
3. Selecione **Definir Módulos**. 
2. Verifique se o módulo **tempSensor** é preenchido automaticamente. Se não for, utilize os seguintes passos para adicioná-lo:
    1. Selecione **Adicionar módulo do IoT Edge**.
    2. No campo **Nome**, introduza `tempSensor`.
    3. No campo **URI da Imagem**, introduza `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview`.
    4. Deixe as outras definições inalteradas e clique em **Guardar**.
9. Adicione o módulo **filterModule** que criou nas secções anteriores. 
    1. Selecione **Adicionar módulo do IoT Edge**.
    2. No campo **Nome**, introduza `filterModule`.
    3. No campo **URI da Imagem**, introduza o endereço da sua imagem, como, por exemplo, `<your container registry address>/filtermodule:0.0.1-amd64`. O endereço completo da imagem está disponível na secção anterior.
    4. Assinale a caixa **Ativar**, para que possa editar o módulo duplo. 
    5. Substitua o JSON na caixa de texto para o módulo duplo pelo seguinte JSON: 

        ```json
        {
           "properties.desired":{
              "TemperatureThreshold":25
           }
        }
        ```
 
    6. Clique em **Guardar**.
10. Clique em **Seguinte**.
11. No passo **Especificar Rotas**, copie o JSON abaixo para a caixa de texto. Os módulos publicam todas as mensagens no runtime do Edge. As regras declarativas no runtime definem onde as mensagens fluem. Neste tutorial, vai precisar de duas rotas. A primeira rota transporta mensagens do sensor de temperatura para o módulo de filtro através do ponto final "input1", que é o ponto final que configurou no processador **FilterMessages**. A segunda rota transporta mensagens do módulo de filtro para o Hub IoT. Nesta rota, `upstream` é um destino especial que diz ao Hub Edge para enviar mensagens para o Hub IoT. 

    ```json
    {
       "routes":{
          "sensorToFilter":"FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/filterModule/inputs/input1\")",
          "filterToIoTHub":"FROM /messages/modules/filterModule/outputs/output1 INTO $upstream"
       }
    }
    ```

4. Clique em **Seguinte**.
5. No passo **Rever Modelo**, clique em **Submeter**. 
6. Regresse à página de detalhes do dispositivo IoT Edge e clique em **Atualizar**. Deverá ver o novo **filtermodule** em execução, juntamente com o módulo **tempSensor** e o **runtime do IoT Edge**. 

## <a name="view-generated-data"></a>Ver os dados gerados

Para monitorizar as mensagens do dispositivo para a cloud enviadas a partir do dispositivo IoT Edge para o hub IoT:
1. Configure a extensão Toolkit do Azure IoT com a cadeia de ligação do seu hub IoT: 
    1. Abra o explorador do VS Code ao selecionar **Vista** > **Explorador**. 
    3. No explorador, clique em **DISPOSITIVOS HUB IOT** e clique em **...**. Clique em **Definir Cadeia de Ligação do Hub IoT** e introduza a cadeia de ligação para o hub IoT que o seu dispositivo do IoT Edge liga na janela de pop-up. 

        Para encontrar a cadeia de ligação, clique no mosaico do hub IoT no portal do Azure e, em seguida, clique em **Políticas de acesso partilhado**. Em **Políticas de acesso partilhado**, clique na política **iothubowner** e copie a cadeia de ligação do Hub IoT na janela **iothubowner**.   

1. Para monitorizar os dados que chegam ao hub IoT, selecione **Vista** > **Palete de Comandos** e procure o comando de menu **IoT: Começar a monitorizar mensagens D2C**. 
2. Para parar a monitorização de dados, utilize o comando de menu **IoT: Parar a monitorização de mensagens D2C**. 

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, criou uma função do módulo do IoT Edge que contém código para filtrar dados não processados gerados pelo seu dispositivo IoT Edge. Pode continuar para qualquer um dos seguintes tutoriais para saber mais sobre outras formas em que o Azure IoT Edge o pode ajudar a transformar os dados em informações empresariais no Edge.

> [!div class="nextstepaction"]
> [Implementar a Função do Azure como um módulo](tutorial-deploy-function.md)
> [Implementar o Azure Stream Analytics como um módulo](tutorial-deploy-stream-analytics.md)


<!-- Links -->
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md

<!-- Images -->
[1]: ./media/tutorial-csharp-module/programcs.png
[2]: ./media/tutorial-csharp-module/build-module.png
[3]: ./media/tutorial-csharp-module/docker-os.png
