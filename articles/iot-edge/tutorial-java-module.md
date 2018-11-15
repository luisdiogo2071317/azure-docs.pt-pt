---
title: Tutorial Java do Azure IoT Edge | Microsoft Docs
description: Este tutorial mostra como criar um módulo do IoT Edge com código Java e implementá-lo num dispositivo Edge
services: iot-edge
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 09/21/2018
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc
ms.openlocfilehash: d5201f3b2a0a3548b1f9eaf4a3f6c6b0fe160d18
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/12/2018
ms.locfileid: "51567711"
---
# <a name="tutorial-develop-a-java-iot-edge-module-and-deploy-to-your-simulated-device"></a>Tutorial: Desenvolver um módulo do IoT Edge em Java e implementar no seu dispositivo simulado

Pode utilizar os módulos do Azure IoT Edge para implementar código que aplica a sua lógica de negócio diretamente aos seus dispositivos IoT Edge. Este tutorial explica-lhe como criar e implementar um módulo do IoT Edge que filtra dados de sensores. Vai utilizar o dispositivo IoT Edge simulado que criou na Implementação do Azure IoT Edge num dispositivo simulado em inícios rápidos do [Windows](quickstart.md) ou do [Linux](quickstart-linux.md). Neste tutorial, ficará a saber como:    

> [!div class="checklist"]
> * Usar o Visual Studio Code para criar um módulo Java IoT Edge baseado no pacote de modelos maven Azure IoT Edge e no SDK de dispositivos Java Azure IoT.
> * Utilize o Visual Studio Code e o Docker para criar uma imagem do Docker e publicá-la no seu registo.
> * Implemente o módulo no seu dispositivo IoT Edge.
> * Veja os dados gerados.


O módulo do IoT Edge que criou neste tutorial filtra os dados de temperatura que são gerados pelo seu dispositivo. Envia apenas mensagens de origem caso a temperatura seja superior a um limiar especificado. Este tipo de análise no Edge é útil para reduzir a quantidade de dados que são comunicados e armazenados na cloud. 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="prerequisites"></a>Pré-requisitos

Um dispositivo Azure IoT Edge:

* Pode seguir os passos no início rápido para [Linux](quickstart-linux.md) para utilizar o seu computador de desenvolvimento ou uma máquina virtual como um dispositivo Edge.
* Os módulos Java para o IoT Edge não suportam dispositivos Windows.

Recursos da cloud:

* Um [Hub IoT](../iot-hub/iot-hub-create-through-portal.md) no escalão gratuito ou standard no Azure. 

Recursos de desenvolvimento:

* [Visual Studio Code](https://code.visualstudio.com/). 
* [Pacote de Extensão do Java](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-pack) para o Visual Studio Code.
* [Extensão Azure IoT Edge](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) para Visual Studio Code. 
* [Java SE Development Kit 10](https://aka.ms/azure-jdks) e [defina a variável de ambiente `JAVA_HOME`](https://docs.oracle.com/cd/E19182-01/820-7851/inst_cli_jdk_javahome_t/) para apontar para a sua instalação do JDK.
* [Maven](https://maven.apache.org/)
* [Docker CE](https://docs.docker.com/install/)
   * Se estiver a desenvolver num dispositivo Windows, certifique-se de que o Docker está [configurado para utilizar contentores Linux](https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers). 


## <a name="create-a-container-registry"></a>Criar um registo de contentores
Neste tutorial, vai utilizar a extensão Azure IoT Edge para VS Code para criar um módulo e criar uma **imagem de contentor** a partir dos ficheiros. Em seguida, vai enviar essa imagem para um **registo** que armazena e gere as suas imagens. Por fim, vai implementar a imagem a partir do registo para ser executada no seu dispositivo IoT Edge.  

Pode utilizar qualquer registo compatível com o Docker neste tutorial. O [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) e o [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags) são dois populares serviços de registo do Docker que estão disponíveis na cloud. Este tutorial utiliza o Azure Container Registry. 

1. No [Portal do Azure](https://portal.azure.com), selecione **Criar um recurso** > **Contentores** > **Azure Container Registry**.
2. Dê um nome ao registo, escolha uma subscrição, escolha um grupo de recursos e defina o SKU como **Básico**. 
3. Selecione **Criar**.
4. Depois de o registo de contentores ser criado, navegue para o mesmo e selecione **Chaves de acesso**. 
5. Mude **Utilizador administrador** para **Ativar**.
6. Copie os valores de **Servidor de início de sessão**, **Nome de utilizador** e **Palavra-passe**. Irá utilizar estes valores mais tarde no tutorial para publicar a imagem do Docker no seu registo e, quando adicionar as credenciais de registo ao runtime do Azure IoT Edge. 

## <a name="create-an-iot-edge-module-project"></a>Criar um projeto do módulo do IoT Edge
Os passos seguintes criam um projeto do módulo do IoT Edge baseado no pacote de modelos maven Azure IoT Edge e no SDK de dispositivos Java do Azure IoT com o Visual Studio Code e a extensão Azure IoT Edge.

### <a name="create-a-new-solution"></a>Criar uma nova solução

Crie um modelo de solução Java que pode personalizar com o seu próprio código. 

1. No Visual Studio Code, selecione **Ver** > **Paleta de Comandos** para abrir a paleta de comandos do VS Code. 

2. Na paleta de comandos, introduza e execute o comando **Azure IoT Edge: Nova solução do IoT Edge**. Na paleta de comandos, indique as seguintes informações para criar a sua solução: 

   1. Selecione a pasta onde quer criar a solução. 
   2. Indique um nome para a sua solução ou aceite a predefinição **EdgeSolution**.
   3. Escolha **Módulo Java** como o modelo de módulo. 
   4. Forneça um valor para groupId ou aceite a predefinição **com.edgemodule**.
   5. Substitua o nome do módulo predefinido por **JavaModule**. 
   6. Especifique o registo de contentor do Azure que criou na secção anterior como repositório de imagens para o primeiro módulo. Substitua **localhost:5000** pelo valor do servidor de início de sessão que copiou. A cadeia final é semelhante a \<nome do registo\>.azurecr.io/javamodule.


Se é a primeira vez que cria o módulo Java, poderão ser necessários vários minutos para transferir os pacotes maven. A janela do VS Code carrega a área de trabalho da solução do IoT Edge. A área de trabalho da solução contém cinco componentes de nível superior. Não irá editar a pasta **\.vscode** ou o ficheiro **\.gitignore** neste tutorial. A pasta **modules** contém o código Java para o seu módulo, bem como Dockerfiles para criar o seu módulo como uma imagem de contentor. O ficheiro **\.env** armazena as credenciais do registo de contentor. O ficheiro **deployment.template.json** contém as informações que o runtime do IoT Edge utiliza para implementar módulos num dispositivo. 

Se não tiver especificado um registo de contentor ao criar a sua solução, mas aceitou o valor do localhost:5000 predefinido, não terá um ficheiro \.env. 

### <a name="add-your-registry-credentials"></a>Adicionar as credenciais do registo

O ficheiro de ambiente armazena as credenciais do seu registo de contentor e partilha-as com o runtime do IoT Edge. O runtime precisa destas credenciais para solicitar as imagens privadas para o dispositivo IoT Edge. 

1. No explorador do VS Code, abra o ficheiro .env. 
2. Atualize os campos com os valores **nome de utilizador** e **palavra-passe** que copiou do seu Azure Container Registry. 
3. Guarde este ficheiro. 

### <a name="update-the-module-with-custom-code"></a>Atualizar o módulo com o código personalizado

1. No explorador do VS Code, abra **módulos** > **JavaModule** > **src** > **main** > **java** > **com** > **edgemodule** > **App.java**.

5. Adicione o seguinte código na parte superior do ficheiro para importar novas as novas classes referenciadas.

    ```java
    import java.io.StringReader;
    import java.util.concurrent.atomic.AtomicLong;
    import java.util.HashMap;
    import java.util.Map;
    
    import javax.json.Json;
    import javax.json.JsonObject;
    import javax.json.JsonReader;
    
    import com.microsoft.azure.sdk.iot.device.DeviceTwin.Pair;
    import com.microsoft.azure.sdk.iot.device.DeviceTwin.Property;
    import com.microsoft.azure.sdk.iot.device.DeviceTwin.TwinPropertyCallBack;
    ```

5. Adicione a seguinte definição à classe **App**. Esta variável define o valor que a temperatura medida deve exceder, para que os dados sejam enviados para o hub IoT. 

    ```java
    private static final String TEMP_THRESHOLD = "TemperatureThreshold";
    private static AtomicLong tempThreshold = new AtomicLong(25);
    ```

7. Substitua o método de execução de **MessageCallbackMqtt** pelo seguinte código. Este método é chamado sempre que o módulo recebe uma mensagem MQTT do hub do IoT Edge. Ele filtra as mensagens que comunicam temperaturas inferiores ao limiar de temperatura definido através do módulo duplo.

    ```java
        private int counter = 0;
       @Override
        public IotHubMessageResult execute(Message msg, Object context) {
            this.counter += 1;
 
            String msgString = new String(msg.getBytes(), Message.DEFAULT_IOTHUB_MESSAGE_CHARSET);
            System.out.println(
                   String.format("Received message %d: %s",
                            this.counter, msgString));
            if (context instanceof ModuleClient) {
                try (JsonReader jsonReader = Json.createReader(new StringReader(msgString))) {
                    final JsonObject msgObject = jsonReader.readObject();
                    double temperature = msgObject.getJsonObject("machine").getJsonNumber("temperature").doubleValue();
                    long threshold = App.tempThreshold.get();
                    if (temperature >= threshold) {
                        ModuleClient client = (ModuleClient) context;
                        System.out.println(
                            String.format("Temperature above threshold %d. Sending message: %s",
                            threshold, msgString));
                        client.sendEventAsync(msg, eventCallback, msg, App.OUTPUT_NAME);
                    }
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
            return IotHubMessageResult.COMPLETE;
        }
    ```

8. Adicione as seguintes duas classes internas estáticas à classe **App**. Estas classes recebem atualizações nas propriedades pretendidas do módulo duplo e atualizam a variável **tempThreshold** para corresponder. Todos os módulos têm o seu próprio módulo duplo, que lhe permite configurar o código em execução no interior de um módulo diretamente a partir da cloud.

    ```java
    protected static class DeviceTwinStatusCallBack implements IotHubEventCallback {
        @Override
        public void execute(IotHubStatusCode status, Object context) {
            System.out.println("IoT Hub responded to device twin operation with status " + status.name());
        }
    }
 
    protected static class OnProperty implements TwinPropertyCallBack {
        @Override
        public void TwinPropertyCallBack(Property property, Object context) {
            if (!property.getIsReported()) {
                if (property.getKey().equals(App.TEMP_THRESHOLD)) {
                    try {
                        long threshold = Math.round((double) property.getValue());
                        App.tempThreshold.set(threshold);
                    } catch (Exception e) {
                        System.out.println("Faile to set TemperatureThread with exception");
                        e.printStackTrace();
                    }
                }
            }
        }
    }
    ```

9. Adicione as seguintes linhas ao método **main** após **client.open()** para subscrever as atualizações do módulo duplo.

    ```java
    client.startTwin(new DeviceTwinStatusCallBack(), null, new OnProperty(), null);
    Map<Property, Pair<TwinPropertyCallBack, Object>> onDesiredPropertyChange = new HashMap<Property, Pair<TwinPropertyCallBack, Object>>() {
        {
            put(new Property(App.TEMP_THRESHOLD, null), new Pair<TwinPropertyCallBack, Object>(new OnProperty(), null));
        }
    };
    client.subscribeToTwinDesiredProperties(onDesiredPropertyChange);
    client.getTwin();
    ```

11. Guarde este ficheiro.

## <a name="build-your-iot-edge-solution"></a>Criar a sua solução do IoT Edge

Na secção anterior, criou uma solução do IoT Edge e adicionou código ao **JavaModule** para filtrar mensagens onde a temperatura comunicada da máquina é inferior ao limiar aceitável. Agora, tem de criar a solução como uma imagem de contentor e enviá-la para o registo de contentor. 

1. Introduza o comando seguinte no terminal integrado do Visual Studio Code e inicie sessão no Docker. Em seguida, pode emitir a sua imagem do módulo para o registo de contentor do Azure.
     
   ```csh/sh
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```
   Utilize o nome do utilizador, palavra-passe e servidor de início de sessão que copiou do registo de contentor do Azure na primeira secção. Também pode obter estes valores a partir da secção **Chaves de acesso** do registo no portal do Azure.

2. No explorador do VS Code, abra o ficheiro deployment.template.json na área de trabalho da solução do IoT Edge. Este ficheiro indica ao **$edgeAgent** para implementar dois módulos: **tempSensor** e **JavaModule**. O valor **JavaModule.image** está definido para uma versão Linux amd64 da imagem. Altere a versão de imagem para **arm32v7** se esta for a arquitetura do seu dispositivo IoT Edge. 

   Verifique se o modelo tem o nome do módulo correto e não o nome predefinido **SampleModule** que alterou quando criou a solução IoT Edge.

   Para saber mais sobre os manifestos de implementação, veja [Compreender como os módulos do IoT Edge podem ser utilizados, configurados e reutilizados](module-composition.md).

3. No ficheiro deployment.template.json, a secção **registryCredentials** armazena as credenciais de registo do Docker. Os pares de nome de utilizador e palavra-passe reais são armazenados no ficheiro .env, ignorado pelo git.  

4. Adicione o módulo **JavaModule** ao manifesto da implementação. Insira o seguinte conteúdo JSON na parte inferior da secção **moduleContent**, após o módulo duplo **$edgeHub**: 
    ```json
        "JavaModule": {
            "properties.desired":{
                "TemperatureThreshold":25
            }
        }
    ```

4. Guarde este ficheiro.

5. No explorador do VS Code, clique com o botão direito do rato no ficheiro deployment.template.json e selecione **Criar e Emitir solução do IoT Edge**. 

Quando indicar ao Visual Studio Code para criar a solução, este utiliza primeiro as informações no modelo de implementação e gera um ficheiro deployment.json numa nova pasta com o nome **config**. Em seguida, executa dois comandos no terminal integrado: `docker build` e `docker push`. Estes dois comandos criam o código, colocam a aplicação Java em contentores e enviam-no para o registo de contentor que especificou quando inicializou a solução. 

Pode ver o endereço da imagem de contentor completo com a etiqueta no terminal integrado do VS Code. O endereço da imagem baseia-se nas informações no ficheiro module.json, com o formato \<repositório\>:\<versão\>-\<plataforma\>. Neste tutorial, deve ser semelhante a registryname.azurecr.io/javamodule:0.0.1-amd64.

## <a name="deploy-and-run-the-solution"></a>Implementar e executar a solução

No artigo de início rápido que utilizou para configurar o seu dispositivo IoT Edge, implementou um módulo com o portal do Azure. Também pode implementar módulos com a extensão Toolkit IoT do Azure para o Visual Studio Code. Já tem um manifesto de implementação preparado para o seu cenário, o ficheiro **deployment.json**. Agora tudo o que precisa de fazer é selecionar um dispositivo para receber a implementação.

1. Na paleta de comandos do VS Code, execute o comando **Azure: Iniciar sessão** e siga as instruções para iniciar sessão na sua conta do Azure. Se já iniciou sessão, pode ignorar este passo.

2. Na paleta de comandos do VS Code, execute o **Hub IoT do Azure: Selecionar Hub IoT**. 

3. Escolha a subscrição e o hub IoT que contém o dispositivo do IoT Edge que pretende configurar. 

4. No explorador do VS Code, expanda a secção **Dispositivos do Hub IoT do Azure**. 

5. Carregue com o botão direito do rato no nome do seu dispositivo do IoT Edge e, em seguida, selecione **Criar Implementação para o Dispositivo Único**. 

   ![Criar implementação para dispositivo único](./media/tutorial-java-module/create-deployment.png)

6. Selecione o ficheiro **deployment.json** na pasta **config** e, em seguida, clique em **Selecionar Manifesto de Implementação do Edge**. Não utilize o ficheiro deployment.template.json. 

7. Clique no botão Atualizar. Deverá ver o novo **JavaModule** em execução, juntamente com o módulo **TempSensor**, bem como **$edgeAgent** e **$edgeHub**.  

## <a name="view-generated-data"></a>Ver os dados gerados

Depois de aplicar o manifesto de implementação no seu dispositivo IoT Edge, o runtime do IoT Edge no dispositivo recolhe as novas informações de implementação e começa a ser executado no mesmo. Quaisquer módulos em execução no dispositivo que não estão incluídos no manifesto de implementação são parados. Quaisquer módulos em falta do dispositivo são iniciados. 

Pode ver o estado do seu dispositivo do IoT Edge com a secção **Dispositivos do Hub IoT do Azure** do explorador do Visual Studio Code. Expanda os detalhes do seu dispositivo para ver uma lista de módulos implementados e em execução. 

No próprio dispositivo IoT Edge, pode ver o estado dos seus módulos de implementação com o comando `iotedge list`. Deverá ver quatro módulos: os dois módulos de runtime do IoT Edge, o tempSensor e o módulo personalizado que criou neste tutorial. Pode demorar alguns minutos para que todos os módulos comecem, por isso, execute novamente o comando se não os vir todos inicialmente. 

Para ver as mensagens que são geradas por qualquer módulo, utilize o comando `iotedge logs <module name>`. 

Pode ver as mensagens conforme chegam ao seu hub IoT através do Visual Studio Code. 

1. Para monitorizar os dados que são recebidos pelo hub IoT, selecione as reticências (**...** ) e, em seguida, selecione **Iniciar Monitorização de Mensagens D2C**.
2. Para monitorizar a mensagem D2C para um dispositivo específico, clique com o botão direito do rato na lista e selecione **Iniciar Monitorização de Mensagens D2C**.
3. Para parar a monitorização de dados, execute o comando **Hub IoT do Azure: Parar a monitorização de mensagens D2C** na paleta de comandos. 
4. Para ver ou atualizar o módulo duplo, clique com o botão direito do rato no módulo na lista e selecione **Editar módulo duplo**. Para atualizar o módulo duplo, guarde o ficheiro JSON duplo, clique com o botão direito do rato na área de editor e selecione **Atualizar Módulo Duplo**.
5. Para ver os registos do Docker, instale o [Docker](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker) para o VS Code. Pode encontrar os módulos em execução localmente no explorador do Docker. No menu de contexto, selecione **Mostrar Registos** para ver no terminal integrado.
 
## <a name="clean-up-resources"></a>Limpar recursos 

Se planeia avançar para o próximo artigo recomendado, pode manter os recursos e as configurações que criou e reutilizá-los. Também pode continuar a utilizar o mesmo dispositivo IoT Edge como um dispositivo de teste. 

Caso contrário, pode eliminar as configurações locais e os recursos do Azure que criou neste artigo para evitar custos. 

[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]

[!INCLUDE [iot-edge-clean-up-local-resources](../../includes/iot-edge-clean-up-local-resources.md)]


## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, criou uma função do módulo do IoT Edge com código para filtrar os dados não processados que são gerados pelo seu dispositivo IoT Edge. Pode continuar para os próximos tutoriais para aprender outras formas em que o Azure IoT Edge o pode ajudar a transformar os dados em informações empresariais na periferia.

> [!div class="nextstepaction"]
> [Armazenar dados na periferia com bases de dados do SQL Server](tutorial-store-data-sql-server.md)

