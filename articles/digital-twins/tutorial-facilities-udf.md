---
title: Monitorizar um espaço com o Azure Digital Twins | Microsoft Docs
description: Saiba como aprovisionar os seus recursos espaciais e monitorizar as condições de funcionamento com o Azure Digital Twins através dos passos deste tutorial.
services: digital-twins
author: dsk-2015
ms.service: digital-twins
ms.topic: tutorial
ms.date: 10/15/2018
ms.author: dkshir
ms.openlocfilehash: 1e5cb18b4e526cd0a0607f5bc93788fcf07430e1
ms.sourcegitcommit: 6361a3d20ac1b902d22119b640909c3a002185b3
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/17/2018
ms.locfileid: "49364240"
---
# <a name="tutorial-provision-your-building-and-monitor-working-conditions-with-azure-digital-twins"></a>Tutorial: Aprovisionar o edifício e monitorizar as condições de funcionamento com o Azure Digital Twins

Este tutorial demonstra como utilizar o Azure Digital Twins para monitorizar os seus espaços relativamente às condições de temperatura e ao nível de conforto pretendidos. Depois de [configurar o exemplo de edifício](tutorial-facilities-setup.md), pode aprovisioná-lo e executar funções personalizadas nos dados do sensor com os passos deste tutorial.

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Definir as condições a monitorizar
> * Criar uma função definida Pelo utilizador
> * Simular dados do sensor
> * Obter resultados da função definida pelo utilizador

## <a name="prerequisites"></a>Pré-requisitos

Neste tutorial, parte-se do princípio de que [configurou a sua configuração do Azure Digital Twins](tutorial-facilities-setup.md). Antes de avançar, confirme que tem:
- Uma [conta do Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F),
- uma instância do Digital Twins em execução, 
- os [exemplos do Digital Twins em C#](https://github.com/Azure-Samples/digital-twins-samples-csharp) transferidos e extraídos para o computador de trabalho, 
- a [versão 2.1.403 ou superior do SDK de .NET Core](https://www.microsoft.com/net/download) no computador de desenvolvimento, para compilar e executar o exemplo. Para confirmar se tem a versão certa instalada, execute `dotnet --version`. 
- [Visual Studio Code](https://code.visualstudio.com/), para explorar o código de exemplo. 

## <a name="define-conditions-to-monitor"></a>Definir as condições a monitorizar
Pode definir um conjunto de condições específicas a monitorizar nos dados do dispositivo ou do sensor, denominadas **Matchers**. Depois, pode definir funções, que são denominadas *funções definidas pelo utilizador*, que executam lógica personalizada nos dados provenientes dos seus espaços e dispositivos quando as condições especificadas nas correspondências ocorrem. Para obter mais informações, leia [Data Processing and User-Defined Functions](concepts-user-defined-functions.md) (Processamento de Dados e Funções Definidas pelo Utilizador). 

No exemplo de projeto **_occupancy-quickstart_**, abra o ficheiro **_src\actions\provisionSample.yaml_** no Visual Studio Code. Repare na secção que começa com o tipo **matchers**. Cada entrada neste tipo cria um matcher com o **Nome** especificado e que vai monitorizar um sensor do tipo **dataTypeValue**. Observe como está relacionado com o espaço chamado *Focus Room A1*, que tem um nó **dispositivos**, o qual contém alguns **sensores**. Para aprovisionar um matcher que monitorize um desses sensores, o respetivo **dataTypeValue** deve corresponder a **dataType** do sensor. 

Adicione o matcher seguinte abaixo dos matchers já existentes, confirmando que as chaves estão alinhadas e os espaços não são substituídos por tabulações:

```yaml
      - name: Matcher Temperature
        dataTypeValue: Temperature
```

Isto monitoriza o sensor *SAMPLE_SENSOR_TEMPERATURE* que adicionou no [primeiro tutorial](tutorial-facilities-setup.md). Tenha em conta que estas linhas também estão presentes em *provisionSample.yaml* como linhas comentadas. Pode simplesmente anular os comentários; para tal, remova o caráter “#” à frente de cada linha. 

<a id="udf" />

## <a name="create-a-user-defined-function"></a>Criar uma função definida Pelo utilizador
As funções definidas pelo utilizador, ou UDFs, permitem-lhe personalizar o processamento dos dados do sensor. São código JavaScript personalizado que pode ser executado dentro da sua instância do Digital Twins, quando as condições específicas descritas nos matchers ocorrem. Pode criar *matchers* e *funções definidas pelo utilizador* par cada sensor que queira monitorizar. Para obter informações mais detalhadas, leia [Data Processing and User-Defined Functions](concepts-user-defined-functions.md) (Processamento de Dados e Funções Definidas pelo Utilizador). 

No ficheiro *provisionSample.yaml*, procure uma secção que começa com o tipo **userdefinedfunctions**. Esta secção fornece uma função definida pelo utilizador com um determinado **Nome** e que atua na lista de matchers em **matcherNames**. Repare que pode fornecer o seu próprio ficheiro JavaScript para a UDF como o **script**. Veja também a secção com o nome **roleassignments**. Esta secção atribui a função *Administrador do Espaço* à função definida pelo utilizador, o que permite que esta aceda aos eventos provenientes de qualquer um dos espaços aprovisionados. 

1. Configure a UDF para incluir o matcher “temperatura” ao adicionar ou remover o comentário na linha seguinte do nó `matcherNames` do ficheiro *provisionSample.yaml*:

    ```yaml
            - Matcher Temperature
    ```

1. Abra o ficheiro **_src\actions\userDefinedFunctions\availability.js_** no editor. É feita referência a este ficheiro no elemento **script** de *provisionSample.yaml*. A função definida pelo utilizador neste ficheiro procura condições em que não é detetado movimento na sala e em que os níveis de dióxido de carbono são inferiores a 1000 ppm. Modifique o ficheiro JavaScript para monitorizar a temperatura, para além das outras condições. Adicione as linhas de código seguintes para procurar condições em que não é detetado movimento na sala, em que os níveis de dióxido de carbono são inferiores a 1000 ppm e em que a temperatura é inferior a 25,5ºC.

   > [!NOTE]
   > Esta secção modifica o ficheiro *src\actions\userDefinedFunctions\availability.js*, o que lhe permite aprender detalhadamente uma forma de escrever uma função definida pelo utilizador. Contudo, pode optar por utilizar diretamente o ficheiro [src\actions\userDefinedFunctions\availabilityForTutorial.js](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/actions/userDefinedFunctions/availabilityForTutorial.js) na sua configuração, que tem todas as alterações necessárias para este tutorial. Se optar por o utilizar, confirme que utiliza o nome de ficheiro correto para a chave **_script_** em [src\actions\provisionSample.yaml](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/actions/provisionSample.yaml).

    1. Na parte superior do ficheiro, adicione as linhas seguintes para temperatura, abaixo do comentário `// Add your sensor type here`:

        ```JavaScript
            var temperatureType = "Temperature";
            var temperatureThreshold = 78;
        ```
   
    1. Adicione as linhas seguintes a seguir à declaração que define `var motionSensor`, abaixo do comentário `// Add your sensor variable here`:

        ```JavaScript
            var temperatureSensor = otherSensors.find(function(element) {
                return element.DataType === temperatureType;
            });
        ```
    
    1. Adicione a linha seguinte a seguir à declaração que define `var carbonDioxideValue`, abaixo do comentário `// Add your sensor latest value here`:

        ```JavaScript
            var temperatureValue = getFloatValue(temperatureSensor.Value().Value);
        ```
    
    1. Remova as linhas de código seguintes, abaixo do comentário `// Modify this line to monitor your sensor value`: 

        ```JavaScript
            if(carbonDioxideValue === null || motionValue === null) {
                sendNotification(telemetry.SensorId, "Sensor", "Error: Carbon dioxide or motion are null, returning");
                return;
            }
        ```
       
       Substitua-as pelas linhas seguintes:

        ```JavaScript
            if(carbonDioxideValue === null || motionValue === null || temperatureValue === null){
                sendNotification(telemetry.SensorId, "Sensor", "Error: Carbon dioxide, motion, or temperature are null, returning");
                return;
            }
        ```
    
    1. Remova as linhas de código seguintes, abaixo do comentário `// Modify these lines as per your sensor`:

        ```JavaScript
            var availableFresh = "Room is available and air is fresh";
            var noAvailableOrFresh = "Room is not available or air quality is poor";
        ```

       Substitua-as pelas linhas seguintes:

        ```JavaScript
            var alert = "Room with fresh air and comfortable temperature is available.";
            var noAlert = "Either room is occupied, or working conditions are not right.";
        ```
    
    1. Remova o bloco de código *if-else* seguinte, a seguir ao comentário `// Modify this code block for your sensor`:

        ```JavaScript
            // If carbonDioxide less than threshold and no presence in the room => log, notify and set parent space computed value
            if(carbonDioxideValue < carbonDioxideThreshold && !presence) {
                log(`${availableFresh}. Carbon Dioxide: ${carbonDioxideValue}. Presence: ${presence}.`);
                setSpaceValue(parentSpace.Id, spaceAvailFresh, availableFresh);

                // Set up custom notification for air quality
                parentSpace.Notify(JSON.stringify(availableFresh));
            }
            else {
                log(`${noAvailableOrFresh}. Carbon Dioxide: ${carbonDioxideValue}. Presence: ${presence}.`);
                setSpaceValue(parentSpace.Id, spaceAvailFresh, noAvailableOrFresh);

                // Set up custom notification for air quality
                parentSpace.Notify(JSON.stringify(noAvailableOrFresh));
            }
        ```
      
       Substitua-o pelo bloco *if-else* seguinte:

        ```JavaScript
            // If sensor values are within range and room is available
            if(carbonDioxideValue < carbonDioxideThreshold && temperatureValue < temperatureThreshold && !presence) {
                log(`${alert}. Carbon Dioxide: ${carbonDioxideValue}. Temperature: ${temperatureValue}. Presence: ${presence}.`);

                // log, notify and set parent space computed value
                setSpaceValue(parentSpace.Id, spaceAvailFresh, alert);

                // Set up notification for this alert
                parentSpace.Notify(JSON.stringify(alert));
            }
            else {
                log(`${noAlert}. Carbon Dioxide: ${carbonDioxideValue}. Temperature: ${temperatureValue}. Presence: ${presence}.`);
    
                // log, notify and set parent space computed value
                setSpaceValue(parentSpace.Id, spaceAvailFresh, noAlert);
            }
        ```
        
        A UDF modificada procurará uma condição em que uma sala fica disponível e cujos níveis de dióxido de carbono e a temperatura estejam dentro de um limite tolerável. Quando essa condição for satisfeita, gerará uma notificação com a declaração `parentSpace.Notify(JSON.stringigy(alert));`. Definirá o valor do espaço monitorizado independentemente de a condição ser ou não satisfeita, com a mensagem correspondente.
    
    1. Guarde o ficheiro. 
    
1. Abra uma janela de comandos e navegue para a pasta **_occupancy-quickstart\src_**. Execute o comando seguinte para aprovisionar o gráfico de inteligência espacial e a função definida pelo utilizador. 

    ```cmd/sh
    dotnet run ProvisionSample
    ```

   > [!IMPORTANT]
   > Para impedir acessos não autorizados à sua API de Gestão do Digital Twins, tem de iniciar sessão na aplicação **_occupancy-quickstart_** com as credenciais da sua conta do Azure. A aplicação guarda as credenciais durante um curto período, pelo que poderá ter de iniciar sessão sempre que a executar. Na primeira vez que este programa for executado, bem como quando as credenciais guardadas expiram após a execução, a aplicação direciona-o para uma página Início de Sessão e dá-lhe um código de sessão específico para introduzir nessa página. Siga os pedidos para iniciar sessão com a conta do Azure.


1. Quando a conta estiver autenticada, a aplicação começa a criar um exemplo de gráfico espacial, conforme configurado em *provisionSample.yaml*. Aguarde pela conclusão do aprovisionamento; vai demorar alguns minutos. Após a conclusão, observe as mensagens na janela de comandos e veja o gráfico espacial ser criado. Repare que cria um hub IoT no nó raiz ou em `Venue`. 

1. A partir da saída na janela de comandos, copie o valor de `ConnectionString`, na secção `Devices`, para a área de transferência. Vai precisar deste valor para simular a ligação do dispositivo na próxima secção.

    ![Exemplo de aprovisionamento](./media/tutorial-facilities-udf/run-provision-sample.png)

> [!TIP]
> Se receber uma mensagem de erro semelhante a “The I/O operation has been aborted because of either a thread exit or an application request" (“A operação de E/S foi abortada devido a uma saída de thread ou a um pedido da aplicação”) no decurso do aprovisionamento, experimente executar o comando novamente. Este erro pode acontecer se o cliente HTTP tiver expirado devido a um problema de rede.

<a id="simulate" />

## <a name="simulate-sensor-data"></a>Simular dados do sensor
Nesta secção, vai utilizar o projeto denominado *device-connectivity* do exemplo para simular os dados do sensor para detetar movimento, temperatura e dióxido de carbono. O projeto gera valores aleatórios para os sensores e envia-os para o hub IoT mediante a utilização da cadeia de ligação do dispositivo.

1. Numa janela de comandos separada, navegue para o exemplo do Digital Twins e para a pasta **_device-connectivity_**.

1. Execute este comando para confirmar que as dependências do projeto estão corretas:

    ```cmd/sh
    dotnet restore
    ```

1. Abra o ficheiro *appSettings.json* no editor e edite os valores seguintes:
    1. *DeviceConnectionString*: atribua o valor de `ConnectionString` na janela de saída da secção anterior. Certifique-se de que copia esta cadeia na íntegra, dentro das aspas, para que o simulador se ligue corretamente ao hub IoT.

    1. *HardwareId* na matriz *Sensors*: uma vez que está a simular eventos dos sensores aprovisionados na instância do Digital Twins, o ID de hardware e os nomes dos sensores neste ficheiro devem corresponder ao nó `sensors` do ficheiro *provisionSample.yaml*. Adicione uma entrada nova para o sensor de temperatura; o nó **Sensors** em *appSettings.json* deve ser semelhante ao seguinte:

        ```JSON
        "Sensors": [{
          "DataType": "Motion",
          "HardwareId": "SAMPLE_SENSOR_MOTION"
        },{
          "DataType": "CarbonDioxide",
          "HardwareId": "SAMPLE_SENSOR_CARBONDIOXIDE"
        },{
          "DataType": "Temperature",
          "HardwareId": "SAMPLE_SENSOR_TEMPERATURE"
        }]
        ```

1. Execute este comando para começar a simulação dos eventos do dispositivo para temperatura, movimento e dióxido de carbono:

    ```cmd/sh
    dotnet run
    ```

   > [!NOTE] 
   > Uma vez que o exemplo de simulação não comunica diretamente com a sua instância do Digital Twins, não é necessário que se autentique.

## <a name="get-results-of-user-defined-function"></a>Obter resultados da função definida pelo utilizador
A função definida pelo utilizador é executada sempre que a sua instância recebe dados de dispositivos e de sensores. Esta secção consulta a instância do Digital Twins para obter os resultados da função definida pelo utilizador. Quando uma sala estiver disponível, com ar fresco e a temperatura certa, vê-la-á em tempo quase real. 

1. Abra a janela de comandos que utilizou para aprovisionar o exemplo ou uma janela nova e navegue novamente para a pasta **_occupancy-quickstart\src_**. 

1. Execute o comando seguinte e inicie sessão quando lhe for pedido:

    ```cmd/sh
    dotnet run GetAvailableAndFreshSpaces
    ```

A janela de saída mostra como é que a função definida pelo utilizador é executada e intercepta eventos da simulação do dispositivo. 

   ![Executar a UDF](./media/tutorial-facilities-udf/udf-running.png)

Dependendo de a condição monitorizada ter sido ou não satisfeita, a função definida pelo utilizador define o valor do espaço com a mensagem relevante, conforme vimos na [secção acima](#udf), que a função `GetAvailableAndFreshSpaces` imprime na consola. 

## <a name="clean-up-resources"></a>Limpar recursos

Se já não quiser explorar mais o Azure Digital Twins para além deste ponto, não hesite em eliminar os recursos que foram criados no tutorial:

1. No menu do lado esquerdo do [portal do Azure](http://portal.azure.com), clique em **Todos os recursos**, selecione o seu grupo de recursos do Digital Twins e clique em **Eliminar**.
2. Se for necessário, também pode eliminar os exemplos de aplicações no computador de trabalho. 


## <a name="next-steps"></a>Passos seguintes

Agora que já aprovisionou os seus espaços e criou uma estrutura para acionar notificações personalizadas, pode avançar para qualquer um dos tutoriais seguintes. 

> [!div class="nextstepaction"]
> [Tutorial: Receive notifications from your Azure Digital Twins spaces using Logic Apps](tutorial-facilities-events.md) (Tutorial: Utilizar o Logic Apps para receber notificações dos seus espaços do Azure Digital Twins)

Ou

> [!div class="nextstepaction"]
> [Tutorial: Visualize and analyze events from your Azure Digital Twins spaces using Time Series Insights](tutorial-facilities-analyze.md) (Tutorial: Visualizar e analisar eventos dos seus espaços do Azure Digital Twins com o Time Series Insights)
