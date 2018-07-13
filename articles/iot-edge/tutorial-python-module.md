---
title: Tutorial Python do Azure IoT Edge | Microsoft Docs
description: Este tutorial mostra como criar um módulo do IoT Edge com código Python e implementá-lo num dispositivo Edge
services: iot-edge
author: shizn
manager: timlt
ms.author: xshi
ms.date: 06/26/2018
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc
ms.openlocfilehash: 42af2b5ec6b591929f37afebe6546d61b8a3a02a
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38633308"
---
# <a name="tutorial-develop-and-deploy-a-python-iot-edge-module-to-your-simulated-device"></a>Tutorial: Desenvolver e implementar um módulo do IoT Edge do Python no seu dispositivo simulado

Pode utilizar os módulos do IoT Edge para implementar código que aplica a sua lógica de negócio diretamente nos seus dispositivos IoT Edge. Este tutorial explica-lhe como criar e implementar um módulo do IoT Edge que filtra dados de sensores. Vai utilizar o dispositivo IoT Edge simulado que criou na Implementação do Azure IoT Edge num dispositivo simulado em inícios rápidos do [Windows][lnk-quickstart-win] ou do [Linux][lnk-quickstart-lin]. Neste tutorial, ficará a saber como:    

> [!div class="checklist"]
> * Utilizar o Visual Studio Code para criar um módulo do IoT Edge com Python
> * Utilizar o Visual Studio Code e o Docker para criar uma imagem do Docker e publicá-la no seu registo 
> * Implementar o módulo no seu dispositivo IoT Edge
> * Ver os dados gerados


O módulo do IoT Edge que criou neste tutorial filtra os dados de temperatura gerados pelo seu dispositivo. Envia apenas mensagens de origem caso a temperatura seja superior a um limiar especificado. Este tipo de análise no Edge é útil para reduzir a quantidade de dados comunicados e armazenados na cloud. 

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free) antes de começar.


## <a name="prerequisites"></a>Pré-requisitos

* O dispositivo Azure IoT Edge que criou no início rápido para [Linux](quickstart-linux.md).

   >[!Note]
   >Os módulos de Python para o Azure IoT Edge não suportam dispositivos Windows ou ARM. 

* [Visual Studio Code](https://code.visualstudio.com/). 
* [Extensão Azure IoT Edge para Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) 
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

### <a name="create-a-new-solution"></a>Criar uma nova solução

Utilize o pacote do Python **cookiecutter** para criar um modelo de solução do Python que pode ser criado sobre ele. 

1. No Visual Studio Code, selecione **Vista** > **Terminal Integrado** para abrir o terminal integrado do VS Code.

2. No terminal integrado, introduza o seguinte comando para instalar (ou atualizar) **cookiecutter**, que serve para criar o modelo de solução do Edge no VS Code:

    ```cmd/sh
    pip install --upgrade --user cookiecutter
    ```

3. Selecione **Ver** > **Paleta de Comandos** para abrir a paleta de comandos do VS Code. 

4. Na paleta de comandos, escreva e execute o comando **Azure: Sign in** e siga as instruções para iniciar sessão na sua conta do Azure. Se já iniciou sessão, pode ignorar este passo.

5. Na paleta de comandos, escreva e execute o comando **Azure IoT Edge: Nova solução do IoT Edge**. Na paleta de comandos, indique as seguintes informações para criar a sua solução: 

   1. Selecione a pasta onde quer criar a solução. 
   2. Indique um nome para a sua solução ou aceite a predefinição **EdgeSolution**.
   3. Selecione **Módulo Python** como modelo de módulo. 
   4. Nomeie o seu módulo **PythonModule**. 
   5. Especifique o Azure Container Registry que criou na secção anterior como repositório de imagens para o primeiro módulo. Substitua **localhost:5000** pelo valor do servidor de início de sessão que copiou. A cadeia final é semelhante a **\<nome do registo\>.azurecr.io/pythonmodule**.
 
A janela do VS Code carrega a área de trabalho da solução do IoT Edge. Existe uma pasta **módulos**, um ficheiro de modelo do manifesto de implementação **.env**. 

### <a name="add-your-registry-credentials"></a>Adicionar as credenciais do registo

O ficheiro de ambiente armazena as credenciais do seu repositório de contentor e partilha-as com o runtime do IoT Edge. O runtime precisa destas credenciais para solicitar as imagens privadas para o dispositivo IoT Edge. 

1. No explorador do VS Code, abra o ficheiro **.env**. 
2. Atualize os campos com os valores **nome de utilizador** e **palavra-passe** que copiou do seu Azure Container Registry. 
3. Guarde este ficheiro. 

### <a name="update-the-module-with-custom-code"></a>Atualizar o módulo com o código personalizado

Cada modelo possui o código de exemplo incluído, que simula os dados do sensor do módulo **tempSensor** e encaminha-os para o Hub IoT. Nesta secção, irá adicionar o código que expande o pythonModule para analisar as mensagens antes de as enviar. 

1. No explorador do VS Code, abra **módulos** > **PythonModule** > **main.py**.

2. Na parte superior de **main.py**, importe a biblioteca `json`.

    ```python
    import json
    ```

3. Adicione as variáveis `TEMPERATURE_THRESHOLD` e `TWIN_CALLBACKS` aos contadores globais. O limiar de temperatura do computador define o valor que a temperatura medida tem de exceder para os dados serem enviados para o Hub IoT.

    ```python
    TEMPERATURE_THRESHOLD = 25
    TWIN_CALLBACKS = 0
    ```

4. Substitua a função `receive_message_callback` pelo código abaixo:

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

5. Adicione uma nova função designada `module_twin_callback`. Esta função será invocada quando as propriedades pretendidas forem atualizadas.

    ```python
    # module_twin_callback is invoked when twin's desired properties are updated.
    def module_twin_callback(update_state, payload, user_context):
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

6. Na classe `HubManager`, adicione uma nova linha ao método `__init__` para inicializar a função `module_twin_callback` que adicionou.

    ```python
    # sets the callback when a twin's desired properties are updated.
    self.client.set_module_twin_callback(module_twin_callback, self)
    ```

7. Guarde este ficheiro.

## <a name="build-your-iot-edge-solution"></a>Criar a sua solução do IoT Edge

Na secção anterior, criou uma solução do IoT Edge e adicionou código ao PythonModule que filtrará mensagens onde a temperatura comunicada da máquina é inferior ao limiar aceitável. Agora, tem de criar a solução como uma imagem de contentor e enviá-la para o registo de contentor. 

1. Inicie sessão no Docker ao introduzir o comando seguinte no terminal integrado do Visual Studio Code, para que possa enviar a imagem do módulo para o ACR: 
     
   ```csh/sh
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```
   Utilize o nome do utilizador, palavra-passe e servidor de início de sessão que copiou do Azure Container Registry na primeira secção. Também pode obtê-los novamente a partir da secção **Chaves de acesso** do registo no portal do Azure.

2. No explorador do VS Code, abra o ficheiro **deployment.template.json** na área de trabalho da solução do IoT Edge. 

   Este ficheiro informa o `$edgeAgent` para implementar dois módulos: **tempSensor**, que simula dados de dispositivo, e **PythonModule**. O valor `PythonModule.image` está definido para uma versão Linux amd64 da imagem. Para saber mais sobre os manifestos de implementação, veja [Compreender como os módulos do IoT Edge podem ser utilizados, configurados e reutilizados](module-composition.md).

   Este ficheiro também contém as credenciais do registo. No ficheiro de modelo, o seu nome de utilizador e palavra-passe estão preenchidos com marcadores de posição. Ao gerar o manifesto de implementação, os campos são atualizados com os valores que adicionou a **.env**. 

3. Adicione o módulo duplo PythonModule ao manifesto de implementação. Insira o seguinte conteúdo JSON na parte inferior da secção `moduleContent`, após o módulo duplo `$edgeHub`: 
    ```json
        "PythonModule": {
            "properties.desired":{
                "TemperatureThreshold":25
            }
        }
    ```

4. Guarde este ficheiro.

5. No explorador do VS Code, clique com o botão direito do rato no ficheiro **deployment.template.json** e selecione **Criar solução do IoT Edge**. 

Quando indicar ao Visual Studio Code para criar a solução, este utiliza primeiro as informações no modelo de implementação e gera um ficheiro `deployment.json` numa nova pasta **config**. Em seguida, executa dois comandos no terminal integrado: `docker build` e `docker push`. Estes dois comandos criam o código, colocam o código do Python em contentores e enviam-no para o registo de contentor que especificou quando inicializou a solução. 

Pode ver o endereço da imagem de contentor completo com a etiqueta no comando `docker build` que é executado no terminal integrado do VS Code. O endereço da imagem baseia-se nas informações no ficheiro `module.json`, com o formato **\<repositório\>:\<versão\>-\<plataforma\>**. Para este tutorial, deve ser semelhante a **registryname.azurecr.io/pythonmodule:0.0.1-amd64**.

## <a name="deploy-and-run-the-solution"></a>Implementar e executar a solução

Pode utilizar o portal do Azure para implementar o seu módulo do Python para um dispositivo do IoT Edge, tal como fez nos inícios rápidos, mas também pode implementar e monitorizar os módulos a partir do Visual Studio Code. As secções seguintes utilizam a extensão do Azure IoT Edge para o VS Code, que foi listado nos pré-requisitos. Instale-a agora, se ainda não o fez. 

1. Abra a paleta de comandos do VS Code ao selecionar **Ver** > **Paleta de Comandos**.

2. Procure e execute o comando **Azure: Iniciar sessão**. Siga as instruções para iniciar sessão na conta do Azure. 

3. Na paleta de comandos, procure e execute o comando **Hub IoT do Azure: Selecionar Hub IoT**. 

4. Selecione a subscrição que contém o seu hub IoT e, em seguida, selecione o hub IoT ao qual quer aceder.

5. No explorador do VS Code, expanda a secção **Dispositivos do Hub IoT do Azure**. 

6. Carregue com o botão direito do rato no nome do seu dispositivo do IoT Edge e, em seguida, selecione **Criar Implementação para o dispositivo do IoT Edge**. 

7. Navegue para a pasta de solução que contém o PythonModule. Abra a pasta **config** e selecione o ficheiro **deployment.json**. Clique em **Selecionar Manifesto de Implementação do Edge**.

8. Atualize a secção **Dispositivos do Hub IoT do Azure**. Deverá ver o novo **PythonModule** em execução, juntamente com o módulo **TempSensor**, bem como **$edgeAgent** e **$edgeHub**. 

## <a name="view-generated-data"></a>Ver os dados gerados

1. Para monitorizar os dados que chegam ao hub IoT, clique em **...** e selecione **Iniciar Monitorização de Mensagens D2C**.
2. Para monitorizar a mensagem D2C para um dispositivo específico, clique com o botão direito do rato na lista e selecione **Iniciar Monitorização de Mensagens D2C**.
3. Para parar a monitorização de dados, execute o comando **Azure IoT Hub: Stop monitoring D2C message** na paleta de comandos. 
4. Para ver ou atualizar o módulo duplo, clique com o botão direito do rato no módulo na lista e selecione **Editar módulo duplo**. Para atualizar o módulo duplo, guarde o ficheiro JSON duplo e clique com o botão direito do rato na área de editor e selecione **Atualizar Módulo Duplo**.
5. Para ver os registos do Docker, pode instalar o [Docker](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker) para o VS Code e localizar os módulos em execução localmente no explorador do Docker. No menu de contexto, clique em **Mostrar Registos** para ver no terminal integrado. 

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

4. É-lhe pedido que confirme a eliminação do grupo de recursos. Escreva o nome do grupo de recursos novamente para confirmar e, em seguida, clique em **Eliminar**. Após alguns instantes, o grupo de recursos e todos os recursos contidos no mesmo são eliminados.

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, criou uma função do módulo do IoT Edge que contém código para filtrar dados não processados gerados pelo seu dispositivo IoT Edge. Pode continuar para qualquer um dos seguintes tutoriais para saber mais sobre outras formas em que o Azure IoT Edge o pode ajudar a transformar os dados em informações empresariais no Edge.

> [!div class="nextstepaction"]
> [Implementar a Função do Azure como um módulo](tutorial-deploy-function.md)
> [Implementar o Azure Stream Analytics como um módulo](tutorial-deploy-stream-analytics.md)


<!-- Links -->
[lnk-quickstart-win]: quickstart.md
[lnk-quickstart-lin]: quickstart-linux.md

<!-- Images -->
[1]: ./media/tutorial-csharp-module/programcs.png
[2]: ./media/tutorial-csharp-module/build-module.png
[3]: ./media/tutorial-csharp-module/docker-os.png
