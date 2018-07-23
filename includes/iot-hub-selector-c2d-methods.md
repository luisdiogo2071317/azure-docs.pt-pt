> [!div class="op_single_selector"]
> * [Node.js](../articles/iot-hub/quickstart-control-device-node.md)
> * [C#](../articles/iot-hub/quickstart-control-device-dotnet.md)
> * [Java](../articles/iot-hub/quickstart-control-device-java.md)
> * [Python](../articles/iot-hub/quickstart-control-device-python.md)

O IoT Hub do Azure é um serviço totalmente gerido que permite fiável e comunicações bidirecionais seguras entre milhões de dispositivos e uma solução de back-end. Tutoriais anteriores ([introdução ao IoT Hub] e [enviar mensagens da Cloud para o dispositivo com o IoT Hub]) ilustrar dispositivo-para-cloud e cloud-para-dispositivo mensagens funcionalidades básicas do IoT Hub. IoT Hub também oferece a capacidade de invocar métodos não duradoura em dispositivos a partir da cloud. Os métodos diretos representam uma interação de solicitação-resposta com um dispositivo semelhante a uma chamada HTTPS, em que forem bem-sucedidos ou falharem imediatamente (após um tempo limite especificado pelo utilizador) permitir que o utilizador saber o estado da chamada. O artigo [invocar um método direto num dispositivo] [ lnk-devguide-methods] descreve os métodos diretos em mais detalhes e oferece orientação sobre quando utilizar métodos diretos em vez de mensagens na cloud para o dispositivo ou as propriedades pretendidas.

[!INCLUDE [iot-hub-basic](iot-hub-basic-whole.md)]

Este tutorial mostrar-lhe como:

* Utilize o portal do Azure para criar um hub IoT e criar uma identidade de dispositivo no IoT hub.
* Crie uma aplicação de dispositivo simulado que tem um método direto, que pode ser chamado pela cloud.
* Crie uma aplicação de consola que chama um método direto na aplicação do dispositivo simulado através do IoT hub.


[lnk-devguide-methods]: ../articles/iot-hub/iot-hub-devguide-direct-methods.md
[lnk-devguide-mqtt]: ../articles/iot-hub/iot-hub-mqtt-support.md

[Enviar mensagens da Cloud para o dispositivo com o IoT Hub]: ../articles/iot-hub/iot-hub-csharp-csharp-c2d.md
[Introdução ao IoT Hub]: ../articles/iot-hub/quickstart-send-telemetry-node.md