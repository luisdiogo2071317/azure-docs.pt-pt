---
title: Simulação do dispositivo na solução de monitorização remota - Azure | Documentos da Microsoft
description: Este tutorial mostra-lhe como utilizar o simulador de dispositivos com o acelerador de solução de monitorização remota.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 01/15/2018
ms.topic: conceptual
ms.openlocfilehash: 8d8835bd97b489a730a040e86748c668963c7196
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/20/2018
ms.locfileid: "39187728"
---
# <a name="create-a-new-simulated-device"></a>Criar um novo dispositivo simulado

Este tutorial mostra-lhe como personalizar os microsserviços de simulador de dispositivos no solution accelerator monitorização remota. Para mostrar as capacidades do simulador de dispositivo, este tutorial utiliza dois cenários na aplicação Contoso IoT.

O vídeo seguinte apresenta uma visão geral das opções para personalizar o simulador de dispositivo dos microsserviços:

>[!VIDEO https://channel9.msdn.com/Shows/Internet-of-Things-Show/How-to-customize-the-Remote-Monitoring-Preconfigured-Solution-for-Azure-IoT/Player]

No primeiro cenário, a Contoso quer testar um novo dispositivo de lâmpada inteligente. Para executar os testes, crie um novo dispositivo simulado com as seguintes características:

*Propriedades*

| Nome                     | Valores                      |
| ------------------------ | --------------------------- |
| Cor                    | White, vermelho, azul            |
| Brilho               | 0 a 100                    |
| Estimado restante da vida | Contagem decrescente de 10.000 horas |

*Telemetria*

A tabela seguinte mostra os dados que na lâmpada relatórios para a cloud como um fluxo de dados:

| Nome   | Valores      |
| ------ | ----------- |
| Estado | "on", "desativado" |
| Temperatura | Graus F |
| Online | VERDADEIRO, FALSO |

> [!NOTE]
> O **online** valor de telemetria é obrigatório para tipos de todos os simulado.

*Métodos*

A tabela seguinte mostra as ações que suporta o novo dispositivo:

| Nome        |
| ----------- |
| Mudar de   |
| Optar por desativar  |

*Estado inicial*

A tabela seguinte mostra o estado inicial do dispositivo:

| Nome                     | Valores |
| ------------------------ | -------|
| Cor inicial            | Branco  |
| Brilho inicial       | 75     |
| Vida restante inicial   | 10,000 |
| Estado inicial de telemetria | "on"   |
| Temperatura de telemetria inicial | 200   |

No segundo cenário, adiciona um novo tipo de telemetria para a Contoso existentes **Chiller** dispositivo.

Este tutorial mostra-lhe como utilizar o simulador de dispositivos com o acelerador de solução de monitorização remota:

Neste tutorial, ficará a saber como:

>[!div class="checklist"]
> * Criar um novo tipo de dispositivo
> * Simular o comportamento de dispositivo personalizado
> * Adicionar um novo tipo de dispositivo ao dashboard
> * Enviar telemetria personalizada a partir de um tipo de dispositivo existente

O vídeo seguinte mostra um passo a passo da ligação de dispositivos simulados e real à solução de monitorização remota:

>[!VIDEO https://channel9.msdn.com/Shows/Internet-of-Things-Show/Part-38-Customizing-Azure-IoT-Suite-solution-and-connect-a-real-device/Player]

## <a name="prerequisites"></a>Pré-requisitos

Para seguir este tutorial, precisa de:

* Uma instância da solução de monitorização remota na sua subscrição do Azure. Se não tiver implementado a solução de monitorização remota, ainda, deverá concluir os [implementar o acelerador de solução de monitorização remota](../iot-accelerators/quickstart-remote-monitoring-deploy.md) tutorial.

* O Visual Studio 2017. Se não tiver o Visual Studio 2017 instalado, pode baixar gratuitamente [Visual Studio Community](https://www.visualstudio.com/free-developer-offers/) edition.

* [Na cloud Explorer para Visual Studio 2017](https://marketplace.visualstudio.com/items?itemName=MicrosoftCloudExplorer.CloudExplorerforVS15Preview) extensão do Visual Studio.

* Uma conta no [Docker Hub](https://hub.docker.com/). Pode inscrever-se gratuitamente para começar a utilizar.

* [Git](https://git-scm.com/downloads) instalado no seu computador desktop.

## <a name="prepare-your-development-environment"></a>Preparar o ambiente de desenvolvimento

Conclua as seguintes tarefas para preparar o ambiente de desenvolvimento para adicionar um novo dispositivo simulado à sua solução de monitorização remota:

### <a name="configure-ssh-access-to-the-solution-virtual-machine-in-azure"></a>Configurar o acesso SSH para a máquina virtual de solução no Azure

Quando criou a sua solução de monitorização remota em [www.azureiotsolutions.com](https://www.azureiotsolutions.com), optou por um nome de solução. O nome da solução torna-se o nome do grupo de recursos do Azure que contém os recursos implementados vários que utiliza a solução. Os comandos seguintes utilizam um grupo de recursos chamado **Contoso-01**, deve substituir **Contoso-01** com o nome do seu grupo de recursos.

Os seguintes comandos a utilizar o `az` comando partir [CLI 2.0 do Azure](https://docs.microsoft.com/cli/azure?view=azure-cli-latest). Pode instalar a CLI 2.0 do Azure no seu computador de desenvolvimento, ou utilizar o [Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) no [portal do Azure](http://portal.azure.com). A CLI 2.0 do Azure está pré-instalado no Cloud Shell.

1. Para verificar o nome do grupo de recursos que contém os recursos de monitorização remota, execute o seguinte comando:

    ```sh
    az group list | grep "name"
    ```

    Este comando apresenta uma lista de todos os grupos de recursos na sua subscrição. A lista deve incluir um grupo de recursos com o mesmo nome que a sua solução de monitorização remota.

1. Para tornar o recurso do grupo o grupo predefinido para comandos subsequentes, execute o seguinte comando com o seu nome de grupo de recursos em vez de **Contoso-01**:

    ```sh
    az configure --defaults group=Contoso-01
    ```

1. Para listar os recursos no grupo de recursos, execute o seguinte comando:

    ```sh
    az resource list -o table
    ```

    Tome nota dos nomes da sua máquina virtual e o seu grupo de segurança de rede. Utilize estes valores em passos posteriores.

1. Para ativar o acesso SSH sua máquina virtual, execute o comando seguinte com o nome do seu grupo de segurança de rede do passo anterior:

    ```sh
    az network nsg rule create --name SSH --nsg-name YOUR-NETWORK-SECURITY-GROUP --priority 101 --destination-port-ranges 22 --access Allow --protocol TCP
    ```

    Para ver a lista de regras de entrada para a sua rede, execute o seguinte comando:

    ```sh
    az network nsg rule list --nsg-name YOUR-NETWORK-SECURITY-GROUP -o table
    ```

1. Para alterar a palavra-passe da máquina virtual para uma palavra-passe, que sabe, execute o seguinte comando. Utilize o nome da máquina virtual que apontou anteriormente e uma palavra-passe da sua preferência:

    ```sh
    az vm user update --name YOUR-VM-NAME --username azureuser --password YOUR-PASSWORD
    ```
1. Para encontrar o endereço IP da sua máquina virtual, utilize o seguinte comando e tome nota do endereço IP público:

    ```sh
    az vm list-ip-addresses --name YOUR-VM-NAME
    ```

1. Agora, pode utilizar o SSH para ligar à máquina virtual. O `ssh` comando está pré-instalado no Cloud Shell. Utilize o endereço IP público do passo anterior e, quando lhe for pedido, a palavra-passe que configurou para a máquina virtual:

    ```sh
    ssh azureuser@public-ip-address
    ```

    Agora tem acesso ao shell na máquina virtual que executa os contentores do Docker na solução de monitorização remota. Para ver os contentores em execução, utilize o seguinte comando:

    ```sh
    docker ps
    ```

### <a name="find-the-service-connection-strings"></a>Localizar as cadeias de ligação de serviço

O tutorial, vai trabalhar com a solução do Visual Studio que se liga a serviços de Cosmos DB e o IoT Hub da solução. Os passos seguintes mostram-lhe uma forma de localizar a ligação tem de valores de cadeia de caracteres:

1. Para localizar a cadeia de ligação do Cosmos DB, execute o comando seguinte na sessão SSH ligada à máquina virtual:

    ```sh
    sudo grep STORAGEADAPTER_DOCUMENTDB /app/env-vars
    ```

    Anote a cadeia de ligação. Vai utilizar este valor mais tarde no tutorial.

1. Para localizar a cadeia de ligação do IoT Hub, execute o comando seguinte na sessão SSH ligada à máquina virtual:

    ```sh
    sudo grep IOTHUB_CONNSTRING /app/env-vars
    ```

    Anote a cadeia de ligação. Vai utilizar este valor mais tarde no tutorial.

> [!NOTE]
> Também pode encontrar essas cadeias de ligação no portal do Azure ou utilizando o `az` comando.

### <a name="stop-the-device-simulation-service-in-the-virtual-machine"></a>Parar o serviço de simulação de dispositivo na máquina virtual

Quando modificar o serviço de simulação de dispositivo, pode executá-la localmente para testar as suas alterações. Antes de executar o serviço de simulação de dispositivo localmente, tem de parar a instância em execução na máquina virtual da seguinte forma:

1. Para localizar os **ID de contentor** da **simulação de dispositivo-dotnet** serviço, execute o seguinte comando na sessão SSH ligada à máquina virtual:

    ```sh
    docker ps
    ```

    Tome nota do ID de contentor a **simulação de dispositivo-dotnet** serviço.

1. Para parar o **simulação de dispositivo-dotnet** contentor, execute o seguinte comando:

    ```sh
    docker stop container-id-from-previous-step
    ```

### <a name="clone-the-github-repositories"></a>Clonar repositórios do GitHub

Neste tutorial, vai trabalhar com o **simulação do dispositivo** e **adaptador de armazenamento** projetos do Visual Studio. Pode clonar os repositórios de código de origem do GitHub. Execute este passo no seu computador de desenvolvimento local em que tiver o Visual Studio instalado:

1. Abra uma linha de comandos e navegue para a pasta onde pretende guardar a sua cópia dos **simulação do dispositivo** e **adaptador de armazenamento** repositórios do GitHub.

1. Para clonar a versão do .NET do **simulação do dispositivo** repositório, execute o seguinte comando:

    ```cmd
    git clone https://github.com/Azure/device-simulation-dotnet.git
    ```

    O serviço de simulação de dispositivo na solução de monitorização remota permite-lhe efetuar alterações aos tipos de dispositivo simulado incorporados e criar novos de tipos de dispositivo simulado. Pode utilizar tipos de dispositivo personalizado para testar o comportamento da solução de monitorização remota, antes de ligar os dispositivos físicos.

1. Para clonar a versão do .NET do **adaptador de armazenamento** repositório, execute o seguinte comando:

    ```cmd
    git clone https://github.com/Azure/pcs-storage-adapter-dotnet.git
    ```

    O serviço de simulação de dispositivo utiliza o serviço de placa de armazenamento para ligar ao serviço no Azure Cosmos DB. A solução de monitorização remota armazena os dados de configuração do dispositivo simulado num banco de dados do Cosmos DB.

### <a name="run-the-storage-adapter-service-locally"></a>Executar o serviço de placa de armazenamento localmente

O serviço de simulação de dispositivo utiliza o serviço de placa de armazenamento para ligar à base de dados do Cosmos DB da solução. Se executar o serviço de simulação de dispositivo localmente, terá também de executar o serviço de placa de armazenamento localmente. Os passos seguintes mostram como executar o serviço de placa de armazenamento a partir do Visual Studio:

1. No Visual Studio, abra a **pcs-storage-adapter.sln** no seu clone local do arquivo da solução a **adaptador de armazenamento** repositório.

1. No Solution Explorer, clique com botão direito a **WebService** do projeto, escolha **propriedades**e, em seguida, escolha **depurar**.

1. Na **variáveis de ambiente** secção, edite o valor da **PCS\_STORAGEADAPTER\_DOCUMENTDB\_CONNSTRING** variável a ser a ligação do Cosmos DB a cadeia de caracteres que apontou anteriormente. Em seguida, guarde as alterações.

1. No Solution Explorer, clique com botão direito a **WebService** do projeto, escolha **depurar**e, em seguida, escolha **iniciar nova instância**.

1. O serviço começa a ser executado localmente e abre `http://localhost:9022/v1/status` no seu browser predefinido. Certifique-se de que o **estado** valor é "OK: ativo e bem."

1. Deixe o serviço de placa de armazenamento em execução localmente até ter concluído o tutorial.

Agora tem tudo no local e está pronto para começar a adicionar um novo tipo de dispositivo simulado à sua solução de monitorização remota.

## <a name="create-a-simulated-device-type"></a>Criar um tipo de dispositivo simulado

A maneira mais fácil para criar um novo tipo de dispositivo no serviço de simulação do dispositivo é copiar e modificar um tipo existente. Os passos seguintes mostram como copiar o incorporado **Chiller** dispositivo para criar uma nova **lâmpada** dispositivo:

1. No Solution Explorer, clique com botão direito a **WebService** do projeto, escolha **propriedades**e, em seguida, escolha **depurar**.

1. Na **variáveis de ambiente** secção, edite o valor da **PCS\_IOTHUB\_CONNSTRING** variável a ser a cadeia de ligação do IoT Hub que anotou anteriormente. Em seguida, guarde as alterações.

1. No Solution Explorer, clique com botão direito a **simulação do dispositivo** solução e escolha **definir projetos de arranque**. Escolher **projeto de arranque único** e selecione **WebService**. Em seguida, clique em **OK**.

1. Cada tipo de dispositivo tem um ficheiro de modelo JSON e scripts associados no **serviços/dados/devicemodels** pasta. No Solution Explorer, copie os **Chiller** ficheiros para criar o **lâmpada** ficheiros conforme mostrado na tabela a seguir:

    | Origem                      | Destino                   |
    | --------------------------- | ----------------------------- |
    | chiller-01.json             | lightbulb-01.json             |
    | scripts/chiller-01-state.js | scripts/lightbulb-01-state.js |
    | scripts/reinício-method.js    | scripts/SwitchOn-method.js    |

### <a name="define-the-characteristics-of-the-new-device-type"></a>Definir as características do novo tipo de dispositivo

O **lâmpada 01.json** arquivo define as características do tipo, como a telemetria gera e os métodos que suporta. Os seguintes passos de atualização do **lâmpada 01.json** ficheiro para definir o **lâmpada** dispositivo:

1. Na **lâmpada 01.json** de ficheiros, atualizar os metadados do dispositivo, conforme mostrado no seguinte fragmento:

    ```json
    "SchemaVersion": "1.0.0",
    "Id": "lightbulb-01",
    "Version": "0.0.1",
    "Name": "Lightbulb",
    "Description": "Smart lightbulb device.",
    "Protocol": "MQTT",
    ```

1. Na **lâmpada 01.json** de ficheiros, atualizar a definição de simulação, conforme mostrado no seguinte fragmento:

    ```json
    "Simulation": {
      "InitialState": {
        "online": true,
        "temperature": 200.0,
        "temperature_unit": "F",
        "status": "on"
      },
      "Interval": "00:00:20",
      "Scripts": [
        {
          "Type": "javascript",
          "Path": "lightbulb-01-state.js"
        }
      ]
    },
    ```

1. Na **lâmpada 01.json** de ficheiros, atualizar as propriedades de tipo de dispositivo, conforme mostrado no seguinte fragmento:

    ```json
    "Properties": {
      "Type": "Lightbulb",
      "Color": "White",
      "Brightness": 75,
      "EstimatedRemainingLife": 10000
    },
    ```

1. Na **lâmpada 01.json** de ficheiros, atualizar as definições de telemetria de tipo de dispositivo, conforme mostrado no seguinte fragmento:

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

1. Na **lâmpada 01.json** de ficheiros, atualizar os métodos de tipo de dispositivo, conforme mostrado no seguinte fragmento:

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

1. Guardar a **lâmpada 01.json** ficheiro.

### <a name="simulate-custom-device-behavior"></a>Simular o comportamento de dispositivo personalizado

O **scripts/lâmpada-01-state.js** arquivo define o comportamento de simulação da **lâmpada** tipo. Os seguintes passos de atualização do **scripts/lâmpada-01-state.js** ficheiro para definir o comportamento da **lâmpada** dispositivo:

1. Editar a definição de estado no **scripts/lâmpada-01-state.js** conforme mostrado no seguinte fragmento do ficheiro:

    ```js
    // Default state
    var state = {
      online: true,
      temperature: 200.0,
      temperature_unit: "F",
      status: "on"
    };
    ```

1. Adicionar uma **inverter** funcionar após a **variar** função com a seguinte definição:

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

1. Editar a **principal** função para implementar o comportamento, conforme mostrado no seguinte fragmento:

    ```js
    function main(context, previousState, previousProperties) {

        // Restore the global device properties and the global state before
        // generating the new telemetry, so that the telemetry can apply changes
        // using the previous function state.
        restoreSimulation(previousState, previousProperties);

        state.temperature = vary(200, 5, 150, 250);

        // Make this flip every so often
        state.status = flip(state.status);

        updateState(state);

        return state;
    }
    ```

1. Guardar a **scripts/lâmpada-01-state.js** ficheiro.

O **scripts/SwitchOn-method.js** implementa de ficheiros a **comutador no** método num **lâmpada** dispositivo. Os seguintes passos de atualização do **scripts/SwitchOn-method.js** ficheiro:

1. Editar a definição de estado no **scripts/SwitchOn-method.js** conforme mostrado no seguinte fragmento do ficheiro:

    ```js
    var state = {
       status: "on"
    };
    ```

1. Alternar a lâmpada, para editar a **principal** funcionar da seguinte forma:

    ```js
    function main(context, previousState) {
        log("Executing lightbulb Switch On method.");
        state.status = "on";
        updateState(state);
    }
    ```

1. Guardar a **scripts/SwitchOn-method.js** ficheiro.

1. Fazer uma cópia da **scripts/SwitchOn-method.js** arquivo chamado **scripts/SwitchOff-method.js**.

1. Para optar por desativar a lâmpada, edite a **principal** funcionar a **scripts/SwitchOff-method.js** ficheiros da seguinte forma:

    ```js
    function main(context, previousState) {
        log("Executing lightbulb Switch Off method.");
        state.status = "off";
        updateState(state);
    }
    ```

1. Guardar a **scripts/SwitchOff-method.js** ficheiro.

1. No Solution Explorer, selecione cada um dos seus quatro novos ficheiros por sua vez. Na **propriedades** janela para cada ficheiro, certifique-se de que **copiar para diretório de saída** está definida como **copiar se for mais recente**.

### <a name="configure-the-device-simulation-service"></a>Configurar o serviço de simulação do dispositivo

Para limitar o número de dispositivos simulados que se ligam à solução durante o teste, configure o serviço para executar uma única chiller e um dispositivo de lâmpada único. Os dados de configuração são armazenados na instância do Cosmos DB, no grupo de recursos da solução. Para editar os dados de configuração, utilize o **Cloud Explorer** vista no Visual Studio:

1. Para abrir o **Cloud Explorer** ver no Visual Studio, escolha **vista** e, em seguida **Cloud Explorer**.

1. Para localizar o documento de configuração de simulação, na **procurar recursos** introduza **simualtions.1**.

1. Clique duas vezes o **simulations.1** documento para abri-lo para edição.

1. O valor para **dados**, localize a **DeviceModels** matriz parecida com o seguinte fragmento:

    ```json
    [{\"Id\":\"chiller-01\",\"Count\":1},{\"Id\":\"chiller-02\",\"Count\":1},{\"Id\":\"elevator-01\",\"Count\":1},{\"Id\":\"elevator-02\",\"Count\":1},{\"Id\":\"engine-01\",\"Count\":1},{\"Id\":\"engine-02\",\"Count\":1},{\"Id\":\"prototype-01\",\"Count\":1},{\"Id\":\"prototype-02\",\"Count\":1},{\"Id\":\"truck-01\",\"Count\":1},{\"Id\":\"truck-02\",\"Count\":1}]
    ```

1. Para definir um chiller único e um dispositivo simulado lâmpada único, substitua a **DeviceModels** matriz com o código a seguir:

    ```json
    [{\"Id\":\"chiller-01\",\"Count\":1},{\"Id\":\"lightbulb-01\",\"Count\":1}]
    ```

    Guardar a alteração para o **simulations.1** documento.

> [!NOTE]
> Também pode utilizar o Explorador de dados do Cosmos DB no portal do Azure para editar a **simulations.1** documento.

### <a name="test-the-lightbulb-device-type-locally"></a>Testar localmente o tipo de dispositivo de lâmpada elétrica

Está agora pronto para testar o seu novo tipo de lâmpada simulados ao executar localmente o projeto de simulação do dispositivo.

1. No Solution Explorer, clique com botão direito **WebService**, escolha **depurar** e, em seguida, escolha **iniciar nova instância**.

1. Para verificar se os dois dispositivos simulados estão ligados ao seu IoT Hub, abra o portal do Azure no seu browser.

1. Navegue para o hub IoT no grupo de recursos que contém a sua solução de monitorização remota.

1. Na **monitorização** secção, escolha **métricas**. Em seguida, certifique-se de que o número de **dispositivos ligados** é dois:

    ![Número de dispositivos ligados](./media/iot-accelerators-remote-monitoring-test/connecteddevices.png)

1. No seu browser, navegue para o **Dashboard** para a sua solução de monitorização remota. No painel de telemetria sobre o **Dashboard**, selecione **temperatura**. Apresenta a temperatura para todos os seus dispositivos simulados no gráfico:

    ![Telemetria de temperatura](./media/iot-accelerators-remote-monitoring-test/telemetry.png)

Tem agora a simulação de dispositivo de lâmpada elétrica em execução localmente. A próxima etapa é implementar o código de simulador atualizado para a máquina virtual que executa os microsserviços de monitorização remota no Azure.

Antes de continuar, pode parar a depuração de simulação do dispositivo e projetos de placa de armazenamento no Visual Studio.

### <a name="deploy-the-updated-simulator-to-the-cloud"></a>Implementar o simulador atualizado para a cloud

Os microsserviços da solução de monitorização remota executam em contentores do docker. Os contentores estão alojados na máquina de virtual da solução no Azure. Nesta secção, pode:

* Crie uma nova imagem do docker de simulação do dispositivo.
* Carregue a imagem para o seu repositório de hub do docker.
* Importe a imagem à máquina virtual a sua solução.

Os passos seguintes partem do princípio de que tem um repositório chamado **lâmpada** na sua conta do Docker Hub.

1. No Visual Studio, no **simulação do dispositivo** do projeto, abra o arquivo **solution\scripts\docker\build.cmd**.

1. Edite a linha que define a **DOCKER_IMAGE** variável de ambiente para o seu nome de repositório do Docker Hub:

    ```cmd
    SET DOCKER_IMAGE=your-docker-hub-acccount/lightbulb
    ```

    Guarde a alteração.

1. No Visual Studio, no **simulação do dispositivo** do projeto, abra o arquivo **solution\scripts\docker\publish.cmd**.

1. Edite a linha que define a **DOCKER_IMAGE** variável de ambiente para o seu nome de repositório do Docker Hub:

    ```cmd
    SET DOCKER_IMAGE=your-docker-hub-acccount/lightbulb
    ```

    Guarde a alteração.

1. Abra uma linha de comandos como administrador. Em seguida, navegue para a pasta **scripts\docker** no seu clone da **simulação do dispositivo** repositório do GitHub.

1. Para criar a imagem do docker, execute o seguinte comando:

    ```cmd
    build.cmd
    ```

1. Para iniciar sessão sua conta do Docker Hub, execute o seguinte comando:

    ```cmd
    docker login
    ```

1. Para carregar a nova imagem à sua conta do Docker Hub, execute o seguinte comando:

    ```cmd
    publish.cmd
    ```

1. Para verificar o carregamento, navegue até [ https://hub.docker.com/ ](https://hub.docker.com/). Localize seu **lâmpada** repositório e escolha **detalhes**. Em seguida, escolha **etiquetas**:

    ![Hub do docker](./media/iot-accelerators-remote-monitoring-test/dockerhub.png)

    Os scripts adicionados a **teste** etiqueta para a imagem.

1. Utilize o SSH para ligar à máquina virtual de sua solução no Azure. Em seguida, navegue para o **aplicação** pasta e editar a **docker-Compose** ficheiro:

    ```sh
    cd /app
    sudo nano docker-compose.yml
    ```

1. Edite a entrada para o serviço de simulação de dispositivo para utilizar a imagem do docker:

    ```yaml
    devicesimulation:
      image: {your docker ID}/lightbulb:testing
    ```

    Guarde as alterações.

1. Para reiniciar todos os serviços com as novas definições, execute o seguinte comando:

    ```sh
    sudo ./start.sh
    ```

1. Para verificar o ficheiro de registo do seu novo contentor de simulação de dispositivo, execute o seguinte comando para encontrar o ID de contentor:

    ```sh
    docker ps
    ```

    Em seguida, execute o comando seguinte com o ID do contentor:

    ```sh
    docker logs {container ID}
    ```

Agora concluiu os passos para implementar uma versão atualizada do serviço de simulação do dispositivo à sua solução de monitorização remota.

No seu browser, navegue para o **Dashboard** para a sua solução de monitorização remota. No painel de telemetria sobre o **Dashboard**, selecione **temperatura**. Apresenta a temperatura para seus dois dispositivos simulados no gráfico:

![Telemetria de temperatura](./media/iot-accelerators-remote-monitoring-test/telemetry.png)

Sobre o **dispositivos** página, pode aprovisionar instâncias do seu novo tipo:

![Ver a lista de simulações disponíveis](./media/iot-accelerators-remote-monitoring-test/devicesmodellist.png)

Pode ver a telemetria do dispositivo simulado:

![Ver a telemetria de lâmpada elétrica](./media/iot-accelerators-remote-monitoring-test/devicestelemetry.png)

Pode chamar o **SwitchOn** e **SwitchOff** métodos no seu dispositivo:

![Chamar os métodos de lâmpada elétrica](./media/iot-accelerators-remote-monitoring-test/devicesmethods.png)

## <a name="add-a-new-telemetry-type"></a>Adicionar um novo tipo de telemetria

Esta secção descreve como modificar um tipo de dispositivo simulado existente para oferecer suporte a um novo tipo de telemetria.

### <a name="locate-the-chiller-device-type-files"></a>Localizar os ficheiros de tipo de dispositivo Chiller

Os passos seguintes mostram como encontrar os ficheiros que definem o incorporado **Chiller** dispositivo:

1. Se ainda não o fez, utilize o seguinte comando para clonar o **simulação de dispositivo-dotnet** repositório do GitHub no seu computador local:

    ```cmd/sh
    git clone https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet.git
    ```

1. Cada tipo de dispositivo tem um ficheiro de modelo JSON e scripts associados no `data/devicemodels` pasta. Os ficheiros que definem o simulado **Chiller** são do tipo de dispositivo:

    * **data/devicemodels/chiller-01.json**
    * **data/devicemodels/scripts/chiller-01-state.js**

### <a name="specify-the-new-telemetry-type"></a>Especifique o novo tipo de telemetria

Os passos seguintes mostram como adicionar um novo **temperatura interna** escreva para o **Chiller** tipo de dispositivo:

1. Abra o **chiller 01.json** ficheiro.

1. Atualização do **SchemaVersion** valor da seguinte forma:

    ```json
    "SchemaVersion": "1.1.0",
    ```

1. Na **InitialState** secção, adicione as definições de follwing dois:

    ```json
    "internal_temperature": 65.0,
    "internal_temperature_unit": "F",
    ```

1. Na **telemetria** matriz, adicione a seguinte definição:

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

1. Guardar a **chiller 01.json** ficheiro.

1. Abra o **scripts/chiller-01-state.js** ficheiro.

1. Adicione os campos seguintes para o **estado** variável:

    ```js
    internal_temperature: 65.0,
    internal_temperature_unit: "F",
    ```

1. Adicione a seguinte linha para o **principal** função:

    ```js
    state.internal_temperature = vary(65, 2, 15, 125);
    ```

1. Guardar a **scripts/chiller-01-state.js** ficheiro.

### <a name="test-the-chiller-device-type"></a>O tipo de dispositivo Chiller de teste

Para testar a atualização **Chiller** tipo de dispositivo, execute uma cópia local do primeiro a **simulação de dispositivo-dotnet** serviço para testar o seu tipo de dispositivo se comporta conforme o esperado. Quando tiver testado e depurado localmente o seu tipo de dispositivo atualizado, pode recriar o contentor e voltar a implementar o **simulação de dispositivo-dotnet** serviço para o Azure.

Quando executa o **simulação de dispositivo-dotnet** serviço localmente, ele envia telemetria à sua solução de monitorização remota. Sobre o **dispositivos** página, pode aprovisionar instâncias do seu tipo atualizados.

Para testar e depurar as suas alterações localmente, consulte a secção anterior [testar localmente o tipo de dispositivo de lâmpada elétrica](#test-the-lightbulb-device-type-locally).

Para implementar o seu serviço de simulação do dispositivo atualizadas para a máquina de virtual da solução no Azure, consulte a secção anterior [implementar o simulador atualizado para a cloud](#deploy-the-updated-simulator-to-the-cloud).

Sobre o **dispositivos** página, pode aprovisionar instâncias do seu tipo atualizada:

![Adicionar chiller atualizado](./media/iot-accelerators-remote-monitoring-test/devicesupdatedchiller.png)

Pode ver a nova **temperatura interna** telemetria do dispositivo simulado.

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, mostramos como para:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Criar um novo tipo de dispositivo
> * Simular o comportamento de dispositivo personalizado
> * Adicionar um novo tipo de dispositivo ao dashboard
> * Enviar telemetria personalizada a partir de um tipo de dispositivo existente

Agora aprendeu a personalizar o serviço de simulação do dispositivo. A próxima etapa sugerida é saber como [ligue-se um dispositivo físico, à sua solução de monitorização remota](iot-accelerators-connecting-devices-node.md).

Para obter mais informações para desenvolvedores sobre a solução de monitorização remota, consulte:

* [Guia de Referência para Programadores](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide)
* [Guia de Resolução de Problemas de Programadores](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Troubleshooting-Guide)

<!-- Next tutorials in the sequence -->
