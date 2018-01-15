## <a name="iot-edge"></a>IoT Edge
Limite do Azure IoT permite condicionada por nuvem de implementação de serviços do Azure e o código de solução específicos para dispositivos no local. Dispositivos de IoT limite podem agregar dados de outros dispositivos para realizar a computação e análise antes dos dados são enviados para a nuvem. Para obter mais informações, consulte [Azure IoT Edge](https://docs.microsoft.com/azure/iot-edge/).

## <a name="iot-edge-agent"></a>Agente de limite de IoT
A parte do tempo de execução de limite de IoT responsável pela implementação e módulos de monitorização.

## <a name="iot-edge-device"></a>Dispositivo IoT Edge
Os dispositivos de IoT Edge têm o limite de IoT runtime instalado e sinalizados como "Dispositivo de limite de IoT" nos detalhes do dispositivo. Saiba como [implementar o Azure IoT Edge num dispositivo simulado com Linux - pré-visualizar](https://docs.microsoft.com/azure/iot-edge/tutorial-simulate-device-linux).

## <a name="iot-edge-deployment"></a>Implementação de limite de IoT
Uma implementação de limite de IoT configura um conjunto de destino de dispositivos de limite de IoT a executar um conjunto de limite de IoT módulos. Cada implementação continuamente garante que todos os dispositivos que correspondem a condição de destino estiver a executar o conjunto de módulos especificados, mesmo quando novos dispositivos são criados ou modificados para corresponder a condição de destino. Cada dispositivo de limite de IoT apenas recebe a implementação de prioridade mais alta cuja condição de destino cumpre. Saiba mais sobre [implementação IoT Edge](https://docs.microsoft.com/azure/iot-edge/module-deployment-monitoring).

## <a name="iot-edge-deployment-manifest"></a>Manifesto de implementação de limite de IoT
Um documento Json que contém as informações a serem copiadas twin(s) de módulo de um ou mais dispositivos de IoT Edge para implementar um conjunto de módulos, rotas e módulo associado pretendido propriedades.

## <a name="iot-edge-gateway-device"></a>Dispositivo de gateway de IoT Edge
Um dispositivo de limite de IoT com o dispositivo a jusante. O dispositivo a jusante pode ser IoT Edge ou não dispositivo de limite de IoT.

## <a name="iot-edge-hub"></a>Hub IoT Edge
A parte do tempo de execução de limite de IoT responsável pelo módulo ao módulo comunicações, a montante (para o IoT Hub) e a jusante (na direção oposta ao IoT Hub) comunicações. 

## <a name="iot-edge-leaf-device"></a>Dispositivo de limite de IoT de folha
Um dispositivo de limite de IoT com nenhum dispositivo a jusante. 

## <a name="iot-edge-module"></a>Módulo de limite de IoT
Um módulo de limite de IoT é um contentor de Docker que pode implementar em dispositivos de limite de IoT. Executa uma tarefa específica, como uma mensagem a partir de um dispositivo de ingestão de relacionadas, transformar uma mensagem ou enviar uma mensagem para um hub IoT. Comunica com outros módulos e envia dados para o tempo de execução do limite de IoT. [Compreender os requisitos e ferramentas para o desenvolvimento de módulos de limite de IoT](https://docs.microsoft.com/azure/iot-edge/module-development).

## <a name="iot-edge-module-identity"></a>Identidade do módulo de limite de IoT
Um registo no registo de identidade do módulo do IoT Hub com detalhes sobre as credenciais de segurança e de existência para ser utilizado por um módulo para autenticar com um concentrador de limite ou o IoT Hub.

## <a name="iot-edge-module-image"></a>Imagem de módulo de limite de IoT
A imagem de docker que é utilizada pelo tempo de execução de limite de IoT para instanciar instâncias do módulo.

## <a name="iot-edge-module-twin"></a>Duplo de módulo de limite de IoT
Um documento Json continuada no IoT Hub que armazena as informações de estado de uma instância do módulo.

## <a name="iot-edge-priority"></a>Prioridade de limite de IoT
Quando duas implementações de limite de IoT visam o mesmo dispositivo, a implementação com a prioridade mais alta é aplicada. Se duas implementações têm a mesma prioridade, é aplicada a implementação com a data de criação posterior. Saiba mais sobre [prioridade](https://docs.microsoft.com/azure/iot-edge/module-deployment-monitoring#priority).

## <a name="iot-edge-runtime"></a>Runtime do IoT Edge
Tempo de execução do limite de IoT inclui tudo o que distribui Microsoft para ser instalada num dispositivo de limite de IoT. Faz parte o agente de limite, o hub de limite e o ferramenta Edge CTL.

## <a name="iot-edge-set-modules-to-a-single-device"></a>Limite de IoT definido módulos para um único dispositivo
Uma operação que copia o conteúdo de um manifesto de IoT Edge no dispositivo ' duplo de módulo. A API subjacente é um genérico 'aplicar a configuração do', que demora simplesmente um manifesto de IoT Edge como entrada.

## <a name="iot-edge-target-condition"></a>Condição de destino de limite de IoT
Numa implementação IoT Edge, a condição de destino é qualquer condição booleana em tags de dispositivos duplos para selecionar os dispositivos de destino da implementação, por exemplo, "tag.environment = prod". A condição de destino é avaliada continuamente para incluir quaisquer novos dispositivos que cumprem os requisitos ou remova os dispositivos que já não está a fazer. Saiba mais sobre [condição de destino](https://docs.microsoft.com/azure/iot-edge/module-deployment-monitoring#target-condition)