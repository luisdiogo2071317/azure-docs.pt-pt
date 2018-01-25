---
title: "Simulação de dispositivos na solução de monitorização remota - Azure | Microsoft Docs"
description: "Este tutorial mostra como utilizar o simulador de dispositivo com a solução pré-configurada de monitorização remota."
services: 
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 01/15/2018
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: 0bf1cff4058bfe46b54f3f0b6836ede3e04ed5dd
ms.sourcegitcommit: 28178ca0364e498318e2630f51ba6158e4a09a89
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/24/2018
---
# <a name="test-your-solution-with-simulated-devices"></a>Testar a sua solução com dispositivos simulados

Este tutorial mostra como personalizar o microsserviço do simulador de dispositivo numa solução pré-configurada de monitorização remota. Para mostrar as capacidades do simulador de dispositivo, que este tutorial utiliza dois cenários na aplicação Contoso IoT.

O primeiro cenário, a Contoso pretende testar um novo dispositivo lightbulb inteligente. Para executar os testes, crie um novo dispositivo simulado com as seguintes características:

*Propriedades*

| Nome                     | Valores                      |
| ------------------------ | --------------------------- |
| Cor                    | Em branco, vermelho, azul            |
| Brightness               | 0 e 100                    |
| Estimado vida restantes | Contagem decrescente de 10 000 horas |

*Telemetry*

A tabela seguinte mostra os dados que a lightbulb relatórios para a nuvem como um fluxo de dados:

| Nome   | Valores      |
| ------ | ----------- |
| Estado | "on", "off" |
| Temperatura | Graus F |
| online | TRUE, false |

> [!NOTE]
> O **online** valor de telemetria é obrigatório para tipos de todas as simulada.

*Métodos*

A tabela seguinte mostra as ações que o novo dispositivo suporta:

| Nome        |
| ----------- |
| Comutador no   |
| Desactivar  |

*Estado inicial*

A tabela seguinte mostra o estado inicial do dispositivo:

| Nome                     | Valores |
| ------------------------ | -------|
| Cor inicial            | Branco  |
| Brightness inicial       | 75     |
| Vida restantes inicial   | 10,000 |
| Estado de telemetria inicial | "ativado"   |
| Temperatura telemetria inicial | 200   |

O segundo cenário, adicione um novo tipo de telemetria para Contoso do existente **Chiller** dispositivo.

Este tutorial mostra como utilizar o simulador de dispositivo com a solução pré-configurada de monitorização remota:

Neste tutorial, ficará a saber como:

>[!div class="checklist"]
> * Criar um novo tipo de dispositivo
> * Simular o comportamento de dispositivo personalizada
> * Adicionar um novo tipo de dispositivo para o dashboard
> * Enviar telemetria personalizada a partir de um tipo de dispositivo existente

## <a name="prerequisites"></a>Pré-requisitos

Para seguir este tutorial, tem de:

* Uma instância implementada da solução de monitorização remota na sua subscrição do Azure. Se ainda não implementado a solução de monitorização remota ainda, deve efetuar o [implementar a solução pré-configurada de monitorização remota](iot-suite-remote-monitoring-deploy.md) tutorial.

* O Visual Studio 2017. Se não tiver o Visual Studio 2017, instalado, pode transferir o livre [Visual Studio Community](https://www.visualstudio.com/free-developer-offers/) edição.

* [Explorador da nuvem para Visual Studio 2017](https://marketplace.visualstudio.com/items?itemName=MicrosoftCloudExplorer.CloudExplorerforVS15Preview) extensão do Visual Studio.

* Uma conta no [Docker Hub](https://hub.docker.com/). Pode inscrever gratuitamente começar a utilizar.

* [Git](https://git-scm.com/downloads) instalado no seu computador de secretária.

## <a name="prepare-your-development-environment"></a>Preparar o ambiente de desenvolvimento

Conclua as seguintes tarefas para preparar o ambiente de desenvolvimento para adicionar um novo dispositivo simulado à sua solução de monitorização remota:

### <a name="configure-ssh-access-to-the-solution-virtual-machine-in-azure"></a>Configurar o acesso SSH para a máquina virtual de solução no Azure

Quando criou a sua solução de monitorização remota em [www.azureiotsuite.com](https://www.azureiotsuite.com), escolha um nome de solução. O nome de solução torna-se o nome do grupo de recursos do Azure que contém os recursos implementados vários que a solução utiliza. Os comandos seguintes utilizam um grupo de recursos denominado **Contoso-01**, deve substituir **Contoso-01** com o nome do grupo de recursos.

Os seguintes comandos a utilizar o `az` comando de [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/overview?view=azure-cli-latest). Pode instalar o 2.0 CLI do Azure no seu computador de desenvolvimento ou utilizar o [nuvem Shell](https://docs.microsoft.com/azure/cloud-shell/overview) no [portal do Azure](http://portal.azure.com). O 2.0 CLI do Azure é pré-instaladas na Shell de nuvem.

1. Para verificar o nome do grupo de recursos que contém os recursos de monitorização remotos, execute o seguinte comando:

    ```sh
    az group list | grep "name"
    ```

    Este comando apresenta uma lista de todos os grupos de recursos na sua subscrição. A lista deve incluir um grupo de recursos com o mesmo nome que a sua solução de monitorização remota.

1. Para tornar o recurso do grupo o grupo predefinido para comandos subsequentes, execute o seguinte comando com o nome do grupo de recursos em vez de **Contoso-01**:

    ```sh
    az configure --defaults group=Contoso-01
    ```

1. Para listar os recursos no seu grupo de recursos, execute o seguinte comando:

    ```sh
    az resource list -o table
    ```

    Anote os nomes da sua máquina virtual e o grupo de segurança de rede. Utilize estes valores em passos posteriores.

1. Para ativar o acesso SSH a máquina virtual, execute o seguinte comando com o nome do seu grupo de segurança de rede do passo anterior:

    ```sh
    az network nsg rule create --name SSH --nsg-name your-network-security-group --priority 101 --destination-port-ranges 22 --access Allow --protocol TCP
    ```

    Para ver a lista de regras de entrada para a sua rede, execute o seguinte comando:

    ```sh
    az network nsg rule list --nsg-name Contoso-01-nsg -o table
    ```

1. Para alterar a palavra-passe da máquina virtual para uma palavra-passe souber, execute o seguinte comando. Utilize o nome da máquina virtual que apontou anteriormente e uma palavra-passe da sua preferência:

    ```sh
    az vm user update --name your-vm-name --username azureuser --password your-password
    ```
1. Para localizar o endereço IP da sua máquina virtual, utilize o comando seguinte e anote o endereço IP público:

    ```sh
    az vm list-ip-addresses --name your-vm-name
    ```

1. Agora, pode utilizar o SSH para ligar à máquina virtual. O `ssh` comando é pré-instaladas na Shell de nuvem. Utilize o endereço IP público do passo anterior e, quando lhe for pedido, a palavra-passe que configurou para a máquina virtual:

    ```sh
    ssh azureuser@public-ip-address
    ```

    Tem agora acesso para a shell na máquina virtual que executa os contentores de Docker na solução de monitorização remota. Para ver os contentores em execução, utilize o seguinte comando:

    ```sh
    docker ps
    ```

### <a name="find-the-service-connection-strings"></a>Localizar as cadeias de ligação de serviço

O tutorial, trabalhar com a solução do Visual Studio que estabelece ligação aos serviços de BD do Cosmos e IoT Hub na solução. Os passos seguintes mostram-lhe uma forma de localizar a ligação terá de valores de cadeia:

1. Para localizar a cadeia de ligação de base de dados do Cosmos, execute o seguinte comando na sessão SSH ligado à máquina virtual:

    ```sh
    sudo grep STORAGEADAPTER_DOCUMENTDB /app/env-vars
    ```

    Tome nota da cadeia de ligação. Utilize este valor mais tarde no tutorial.

1. Para localizar a cadeia de ligação do IoT Hub, execute o seguinte comando na sessão SSH ligado à máquina virtual:

    ```sh
    sudo grep IOTHUB_CONNSTRING /app/env-vars
    ```

    Tome nota da cadeia de ligação. Utilize este valor mais tarde no tutorial.

> [!NOTE]
> Também pode encontrar estas cadeias de ligação no portal do Azure ou utilizando o `az` comando.

### <a name="stop-the-device-simulation-service-in-the-virtual-machine"></a>Parar o serviço de simulação de dispositivo na máquina virtual

Quando modifica o serviço de simulação de dispositivos, pode executar localmente para testar as suas alterações. Antes de executar o serviço de simulação de dispositivo localmente, terá de parar a instância em execução na máquina virtual da seguinte forma:

1. Para localizar o **ID de contentor** do **simulação de dispositivo** serviço, execute o comando seguinte numa sessão SSH ligado à máquina virtual:

    ```sh
    docker ps
    ```

    Anote o ID de contentor do **simulação de dispositivo** serviço.

1. Para parar o **simulação de dispositivo** contentor, execute o seguinte comando:

    ```sh
    docker stop container-id-from-previous-step
    ```

### <a name="clone-the-github-repositories"></a>Clonar repositórios do GitHub

Neste tutorial, trabalhar com o **simulação de dispositivo** e **adaptador de armazenamento** projetos do Visual Studio. Pode clonar os repositórios de código de origem a partir do GitHub. Execute este passo no seu computador de desenvolvimento local em que tiver o Visual Studio instalado:

1. Abra uma linha de comandos e navegue para a pasta onde pretende guardar a sua cópia o **simulação de dispositivo** e **adaptador de armazenamento** repositórios do GitHub.

1. Para clonar a versão do .NET do **simulação de dispositivo** repositório, execute o seguinte comando:

    ```cmd
    git clone https://github.com/Azure/device-simulation-dotnet.git
    ```

    O serviço de simulação de dispositivos na solução de monitorização remota permite-lhe efetuar alterações para os tipos de dispositivo simulado incorporadas e criar um novo simulated tipos de dispositivos. Pode utilizar os tipos de dispositivo personalizadas para testar o comportamento da solução de monitorização remota antes de ligar os dispositivos físicos.

1. Para clonar a versão do .NET do **adaptador de armazenamento** repositório, execute o seguinte comando:

    ```cmd
    git clone https://github.com/Azure/storage-adapter.git
    ```

    O serviço de simulação de dispositivo utiliza o serviço de adaptador de armazenamento para ligar ao serviço de base de dados do Cosmos no Azure. A solução de monitorização remota armazena os dados de configuração do dispositivo simulado numa base de dados de base de dados do Cosmos.

### <a name="run-the-storage-adapter-service-locally"></a>Executar o serviço de adaptador de armazenamento localmente

O serviço de simulação de dispositivo utiliza o serviço de adaptador de armazenamento para ligar à base de dados da solução Cosmos DB. Se executar o serviço de simulação de dispositivo localmente, terá também de executar o serviço de adaptador de armazenamento localmente. Os passos seguintes mostram como executar o serviço de adaptador de armazenamento a partir do Visual Studio:

1. No Visual Studio, abra o **pcs-storage-adapter.sln** ficheiro de solução no seu local clone do **adaptador de armazenamento** repositório.

1. No Explorador de soluções, clique com botão direito do **WebService** do projeto, escolha **propriedades**e, em seguida, escolha **depurar**.

1. No **variáveis de ambiente** secção, edite o valor da **PCS\_STORAGEADAPTER\_DOCUMENTDB\_CONNSTRING** variável deve ser a ligação de base de dados do Cosmos cadeia que apontou anteriormente. Em seguida, guarde as alterações.

1. No Explorador de soluções, clique com botão direito do **WebService** do projeto, escolha **depurar**e, em seguida, escolha **iniciar nova instância**.

1. O serviço entrar em execução localmente e abre `http://localhost:9022/v1/status` no seu browser predefinido. Certifique-se de que o **estado** valor é "OK: Alive e bem."

1. Deixe o serviço de adaptador de armazenamento em execução localmente depois de concluir o tutorial.

Tem agora tudo no local e estiver pronto para começar a adicionar um novo tipo de dispositivo simulado à sua solução de monitorização remota.

## <a name="create-a-simulated-device-type"></a>Criar um tipo de dispositivo simulada

É a forma mais fácil de criar um novo tipo de dispositivo no serviço de simulação de dispositivo para copiar e modificar um tipo existente. Os passos seguintes mostram como copiar incorporada **Chiller** dispositivo para criar uma nova **Lightbulb** dispositivo:

1. No Visual Studio, abra o **dispositivo simulation.sln** ficheiro de solução no seu local clone do **simulação de dispositivo** repositório.

1. No Explorador de soluções, clique com botão direito do **SimulationAgent** do projeto, escolha **propriedades**e, em seguida, escolha **depurar**.

1. No **variáveis de ambiente** secção, edite o valor da **PCS\_IOTHUB\_CONNSTRING** variável para ser a cadeia de ligação do IoT Hub que anotou anteriormente. Em seguida, guarde as alterações.

1. No Explorador de soluções, clique com botão direito do **simulação de dispositivo** soluções e escolha **definir projetos de arranque**. Escolha **projeto de arranque único** e selecione **SimulationAgent**. Em seguida, clique em **OK**.

1. Cada tipo de dispositivo tem um ficheiro de modelo JSON e scripts associados no **dados/Services/devicemodels** pasta. No Explorador de soluções, copie o **Chiller** ficheiros para criar o **Lightbulb** ficheiros conforme mostrado na seguinte tabela:

    | Origem                      | Destino                   |
    | --------------------------- | ----------------------------- |
    | chiller-01.json             | lightbulb-01.json             |
    | scripts/chiller-01-state.js | scripts/lightbulb-01-state.js |
    | scripts/reboot-method.js    | scripts/SwitchOn-method.js    |

### <a name="define-the-characteristics-of-the-new-device-type"></a>Definir as características do novo tipo de dispositivo

O **lightbulb 01.json** ficheiro define as características do tipo, tais como a telemetria gera e os métodos que suporta. Os passos seguintes atualização o **lightbulb 01.json** ficheiro para definir o **Lightbulb** dispositivo:

1. No **lightbulb 01.json** de ficheiros, atualize os metadados do dispositivo, conforme mostrado no seguinte fragmento:

    ```json
    "SchemaVersion": "1.0.0",
    "Id": "lightbulb-01",
    "Version": "0.0.1",
    "Name": "Lightbulb",
    "Description": "Smart lightbulb device.",
    "Protocol": "MQTT",
    ```

1. No **lightbulb 01.json** ficheiro, a definição de simulação de atualização, conforme mostrado no seguinte fragmento:

    ```json
    "Simulation": {
      "InitialState": {
        "online": true,
        "temperature": 200.0,
        "temperature_unit": "F",
        "status": "on"
      },
      "Script": {
        "Type": "javascript",
        "Path": "lightbulb-01-state.js",
        "Interval": "00:00:20"
      }
    },
    ```

1. No **lightbulb 01.json** de ficheiros, Atualize as propriedades do tipo de dispositivo, conforme mostrado no seguinte fragmento:

    ```json
    "Properties": {
      "Type": "Lightbulb",
      "Color": "White",
      "Brightness": 75,
      "EstimatedRemainingLife": 10000
    },
    ```

1. No **lightbulb 01.json** de ficheiros, Atualize as definições de telemetria do tipo de dispositivo, conforme mostrado no seguinte fragmento:

    ```json
    "Telemetry": [
      {
        "Interval": "00:00:20",
        "MessageTemplate": "{\"temperature\":${temperature},\"temperature_unit\":\"${temperature_unit}\",\"status\":\"${status}\"}",
        "MessageSchema": {
          "Name": "lightbulb-status;v1",
          "Format": "JSON",
          "Fields": {
            "temperature": "double",
            "temperature_unit": "text",
            "status": "text"
          }
        }
      }
    ],
    ```

1. No **lightbulb 01.json** de ficheiros, atualize os métodos de tipo de dispositivo, conforme mostrado no seguinte fragmento:

    ```json
    "CloudToDeviceMethods": {
      "SwitchOn": {
        "Type": "javascript",
        "Path": "SwitchOn-method.js"
      },
      "SwitchOff": {
        "Type": "javascript",
        "Path": "SwitchOff-method.js"
      }
    }
    ```

1. Guardar o **lightbulb 01.json** ficheiro.

### <a name="simulate-custom-device-behavior"></a>Simular o comportamento de dispositivo personalizada

O **scripts/lightbulb-01-state.js** ficheiro define o comportamento de simulação do **Lightbulb** tipo. Os passos seguintes atualização o **scripts/lightbulb-01-state.js** ficheiro para definir o comportamento do **Lightbulb** dispositivo:

1. Edite a definição de estado no **scripts/lightbulb-01-state.js** conforme mostrado no seguinte fragmento de ficheiros:

    ```js
    // Default state
    var state = {
      online: true,
      temperature: 200.0,
      temperature_unit: "F",
      status: "on"
    };
    ```

1. Adicionar um **inverte** funcionar após a **variar** função com a seguinte definição:

    ```js
    /**
    * Simple formula that sometimes flips the status of the lightbulb
    */
    function flip(value) {
      if (Math.random() < 0.2) {
        return (value == "on") ? "off" : "on"
      }
      return value;
    }
    ```

1. Editar o **principal** função para implementar o comportamento, conforme mostrado no seguinte fragmento:

    ```js
    function main(context, previousState) {

      // Restore the global state before generating the new telemetry, so that
      // the telemetry can apply changes using the previous function state.
      restoreState(previousState);

      state.temperature = vary(200, 5, 150, 250);

      // Make this flip every so often
      state.status = flip(state.status);

      return state;
    }
    ```

1. Guardar o **scripts/lightbulb-01-state.js** ficheiro.

O **SwitchOn/scripts-method.js** ficheiro implementa o **no comutador** método um **Lightbulb** dispositivo. Os passos seguintes atualização o **SwitchOn/scripts-method.js** ficheiro:

1. Edite a definição de estado no **SwitchOn/scripts-method.js** conforme mostrado no seguinte fragmento de ficheiros:

    ```js
    var state = {
       status: "on"
    };
    ```

1. Para mudar no lightbulb, edite o **principal** funciona da seguinte forma:

    ```js
    function main(context, previousState) {
        log("Executing lightbulb Switch On method.");
        state.status = "on";
        updateState(state);
    }
    ```

1. Guardar o **SwitchOn/scripts-method.js** ficheiro.

1. Faça uma cópia do **SwitchOn/scripts-method.js** ficheiro chamado **SwitchOff/scripts-method.js**.

1. Para desactivar a lightbulb, edite o **principal** funcionar o **SwitchOff/scripts-method.js** ficheiro da seguinte forma:

    ```js
    function main(context, previousState) {
        log("Executing lightbulb Switch Off method.");
        state.status = "off";
        updateState(state);
    }
    ```

1. Guardar o **SwitchOff/scripts-method.js** ficheiro.

1. No Explorador de soluções, selecione cada um dos seus novos ficheiros de quatro por sua vez. No **propriedades** janela para cada ficheiro, certifique-se de que **copiar para o diretório de saída** está definido como **copiar se for mais recente**.

### <a name="configure-the-device-simulation-service"></a>Configurar o serviço de simulação de dispositivo

Para limitar o número de dispositivos simulados que se ligam a solução durante os testes, configure o serviço para executar uma única chiller e um dispositivo lightbulb único. Os dados de configuração são armazenados na instância da base de dados do Cosmos no grupo de recursos a solução. Para editar os dados de configuração, utilize o **Cloud Explorer** vista no Visual Studio:

1. Para abrir o **Cloud Explorer** ver no Visual Studio, escolha **vista** e, em seguida, **Cloud Explorer**.

1. Para localizar o documento de configuração de simulação, no **procurar recursos** introduza **simualtions.1**.

1. Faça duplo clique o **simulations.1** documento para abri-lo para edição.

1. O valor para **dados**, localize o **DeviceModels** matriz que se pareça com o seguinte fragmento:

    ```json
    [{\"Id\":\"chiller-01\",\"Count\":1},{\"Id\":\"chiller-02\",\"Count\":1},{\"Id\":\"elevator-01\",\"Count\":1},{\"Id\":\"elevator-02\",\"Count\":1},{\"Id\":\"engine-01\",\"Count\":1},{\"Id\":\"engine-02\",\"Count\":1},{\"Id\":\"prototype-01\",\"Count\":1},{\"Id\":\"prototype-02\",\"Count\":1},{\"Id\":\"truck-01\",\"Count\":1},{\"Id\":\"truck-02\",\"Count\":1}]
    ```

1. Para definir um chiller único e um dispositivo simulado lightbulb único, substitua o **DeviceModels** matriz com o seguinte código:

    ```json
    [{\"Id\":\"chiller-01\",\"Count\":1},{\"Id\":\"lightbulb-01\",\"Count\":1}]
    ```

    Guardar a alteração para o **simulations.1** documento.

> [!NOTE]
> Também pode utilizar o Explorador de dados de base de dados do Cosmos no portal do Azure para editar o **simulations.1** documento.

### <a name="test-the-lightbulb-device-type-locally"></a>Testar o tipo de dispositivo Lightbulb localmente

Agora está pronto para testar o novo tipo de lightbulb simulada, executando o projeto de simulação de dispositivo localmente.

1. No Explorador de soluções, faça duplo clique **SimulationAgent**, escolha **depurar** e, em seguida, escolha **iniciar nova instância**.

1. Para verificar se os dois dispositivos simulados estão ligados ao seu IoT Hub, abra o portal do Azure no seu browser.

1. Navegue para o IoT hub no grupo de recursos que contém a sua solução de monitorização remota.

1. No **monitorização** secção, escolha **métricas**. Em seguida, certifique-se de que o número de **dispositivos ligados** é dois:

    ![Número de dispositivos ligados](media/iot-suite-remote-monitoring-test/connecteddevices.png)

1. No seu browser, navegue para o **Dashboard** para a sua solução de monitorização remota. No painel de telemetria sobre o **Dashboard**, selecione **temperatura**. Apresenta a temperatura para os seus dispositivos simulados duas no gráfico:

    ![Telemetria de temperatura](media/iot-suite-remote-monitoring-test/telemetry.png)

Tem agora a simulação de dispositivo lightbulb executar localmente. O passo seguinte é implementar o código do simulador atualizado para a máquina virtual que executa os micro-serviços de monitorização remotos no Azure.

Antes de continuar, pode parar a depuração de simulação de dispositivo e projetos de adaptador de armazenamento no Visual Studio.

### <a name="deploy-the-updated-simulator-to-the-cloud"></a>Implementar o simulador atualizado para a nuvem

Executam os micro-serviços na solução de monitorização remota nos contentores do docker. Os contentores estão alojados na máquina virtual da solução no Azure. Nesta secção, pode:

* Crie uma nova imagem de docker de simulação de dispositivo.
* Carregar a imagem para o seu repositório de hub de docker.
* Importe a imagem numa máquina virtual da sua solução.

Os seguintes passos assumem que tem um repositório denominado **lightbulb** na sua conta do Hub de Docker.

1. No Visual Studio, no **simulação de dispositivo** do projeto, abra o ficheiro **solution\scripts\docker\build.cmd**.

1. Edite a linha que define o **DOCKER_IMAGE** variável de ambiente para o nome do repositório de Docker Hub:

    ```cmd
    SET DOCKER_IMAGE=your-docker-hub-acccount/lightbulb
    ```

    Guarde a alteração.

1. No Visual Studio, no **simulação de dispositivo** do projeto, abra o ficheiro **solution\scripts\docker\publish.cmd**.

1. Edite a linha que define o **DOCKER_IMAGE** variável de ambiente para o nome do repositório de Docker Hub:

    ```cmd
    SET DOCKER_IMAGE=your-docker-hub-acccount/lightbulb
    ```

    Guarde a alteração.

1. Abra uma linha de comandos como administrador. Em seguida, navegue para a pasta **scripts\docker** no seu clone do **simulação de dispositivo** repositório do GitHub.

1. Para compilar a imagem do docker, execute o seguinte comando:

    ```cmd
    build.cmd
    ```

1. Para iniciar sessão sua conta do Hub de Docker, execute o seguinte comando:

    ```cmd
    docker login
    ```

1. Para carregar a nova imagem para a sua conta do Hub de Docker, execute o seguinte comando:

    ```cmd
    publish.cmd
    ```

1. Para verificar o carregamento, navegue até à [https://hub.docker.com/](https://hub.docker.com/). Localize o **lightbulb** repositório e escolha **detalhes**. Em seguida, escolha **etiquetas**:

    ![Hub de docker](media/iot-suite-remote-monitoring-test/dockerhub.png)

    Os scripts adicionados o **testar** tag de imagem.

1. Utilize o SSH para ligar à máquina virtual da sua solução no Azure. Em seguida, navegue para o **aplicação** pasta e editar o **docker compose.yaml** ficheiro:

    ```sh
    cd /app
    sudo nano docker-compose.yaml
    ```

1. Edite a entrada para o serviço de simulação de dispositivos para utilizar a sua imagem docker:

    ```yaml
    devicesimulation:
      image: {your docker ID}/lightbulb:testing
    ```

    Guarde as alterações.

1. Para reiniciar todos os serviços com as novas definições, execute o seguinte comando:

    ```sh
    sudo ./start.sh
    ```

1. Para verificar o ficheiro de registo do seu novo contentor de simulação de dispositivo, execute o seguinte comando para localizar o ID de contentor:

    ```sh
    docker ps
    ```

    Em seguida, execute o seguinte comando com o ID de contentor:

    ```sh
    docker logs {container ID}
    ```

Agora concluiu os passos para implementar uma versão atualizada do serviço de simulação de dispositivo à sua solução de monitorização remota.

No seu browser, navegue para o **Dashboard** para a sua solução de monitorização remota. No painel de telemetria sobre o **Dashboard**, selecione **temperatura**. Apresenta a temperatura para os seus dispositivos simulados duas no gráfico:

![Telemetria de temperatura](media/iot-suite-remote-monitoring-test/telemetry.png)

No **dispositivos** página, pode aprovisionar as instâncias do seu novo tipo:

![Ver a lista de simulações disponíveis](media/iot-suite-remote-monitoring-test/devicesmodellist.png)

Pode ver a telemetria do dispositivo simulado:

![Vista lightbulb telemetria](media/iot-suite-remote-monitoring-test/devicestelemetry.png)

Pode chamar o **SwitchOn** e **SwitchOff** métodos no seu dispositivo:

![Chamar os métodos de lightbulb](media/iot-suite-remote-monitoring-test/devicesmethods.png)

## <a name="add-a-new-telemetry-type"></a>Adicionar um novo tipo de telemetria

Esta secção descreve como modificar um tipo de dispositivo simulado existente para suportar um novo tipo de telemetria.

### <a name="locate-the-chiller-device-type-files"></a>Localizar os ficheiros de tipo de dispositivo Chiller

Os passos seguintes mostram como encontrar os ficheiros que definem incorporada **Chiller** dispositivo:

1. Se ainda não o tiver feito, utilize o seguinte comando para clonar o **simulação de dispositivo** repositório do GitHub para o computador local:

    ```cmd/sh
    git clone https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet.git
    ```

1. Cada tipo de dispositivo tem um ficheiro de modelo JSON e scripts associados no `data/devicemodels` pasta. Os ficheiros que definem a simulada **Chiller** são do tipo de dispositivo:

    * **data/devicemodels/chiller-01.json**
    * **data/devicemodels/scripts/chiller-01-state.js**

### <a name="specify-the-new-telemetry-type"></a>Especifique o novo tipo de telemetria

Os passos seguintes mostram como adicionar um novo **temperatura interno** escreva para a **Chiller** tipo de dispositivo:

1. Abra o **chiller 01.json** ficheiro.

1. Atualização do **SchemaVersion** valor da seguinte forma:

    ```json
    "SchemaVersion": "1.1.0",
    ```

1. No **InitialState** secção, adicione as follwing duas definições:

    ```json
    "internal_temperature": 65.0,
    "internal_temperature_unit": "F",
    ```

1. No **telemetria** matriz, adicione a seguinte definição:

    ```json
    {
      "Interval": "00:00:05",
      "MessageTemplate": "{\"internal_temperature\":${internal_temperature},\"internal_temperature_unit\":\"${internal_temperature_unit}\"}",
      "MessageSchema": {
        "Name": "chiller-internal-temperature;v1",
        "Format": "JSON",
        "Fields": {
          "temperature": "double",
          "temperature_unit": "text"
        }
      }
    },
    ```

1. Guardar o **chiller 01.json** ficheiro.

1. Abra o **scripts/chiller-01-state.js** ficheiro.

1. Adicione os seguintes campos para o **estado** variável:

    ```js
    internal_temperature: 65.0,
    internal_temperature_unit: "F",
    ```

1. Adicione a seguinte linha para o **principal** função:

    ```js
    state.internal_temperature = vary(65, 2, 15, 125);
    ```

1. Guardar o **scripts/chiller-01-state.js** ficheiro.

### <a name="test-the-chiller-device-type"></a>O tipo de dispositivo Chiller de teste

Para testar a atualização **Chiller** tipo de dispositivo, executado uma cópia local do primeiro o **simulação de dispositivo** serviço para testar o seu tipo de dispositivo funciona conforme esperado. Se tiver testado e debugged localmente o seu tipo de dispositivo atualizado, pode recriar o contentor e voltar a implementar o **simulação de dispositivo** serviço para o Azure.

Quando executa o **simulação de dispositivo** serviço localmente, envia telemetria à sua solução de monitorização remota. No **dispositivos** página, pode aprovisionar instâncias do seu tipo atualizado.

Para testar e depurar as suas alterações localmente, consulte a secção anterior [testar localmente o tipo de dispositivo Lightbulb](#test-the-lightbulb-device-type-locally).

Para implementar o serviço de simulação de dispositivo atualizado para uma máquina virtual a solução no Azure, consulte a secção anterior [implementar o simulador atualizado para a nuvem](#deploy-the-updated-simulator-to-the-cloud).

No **dispositivos** página, pode aprovisionar instâncias do seu tipo atualizada:

![Adicionar chiller atualizado](media/iot-suite-remote-monitoring-test/devicesupdatedchiller.png)

Pode ver o novo **temperatura interna** telemetria do dispositivo simulado.

## <a name="next-steps"></a>Passos Seguintes

Este tutorial mostrou, como para:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Criar um novo tipo de dispositivo
> * Simular o comportamento de dispositivo personalizada
> * Adicionar um novo tipo de dispositivo para o dashboard
> * Enviar telemetria personalizada a partir de um tipo de dispositivo existente

Agora tem aprendeu a personalizar o serviço de simulação de dispositivo. O passo seguinte sugerido é saber como [ligar um dispositivo físico a sua solução de monitorização remota](iot-suite-connecting-devices-node.md).

Para obter mais informações de programador sobre a solução de monitorização remota, consulte:

* [Guia de Referência para Programadores](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide)
* [Guia de Resolução de Problemas de Programadores](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Troubleshooting-Guide)

<!-- Next tutorials in the sequence -->