---
title: IoT DevKit com a nuvem – ligar IoT DevKit AZ3166 a acelerador de solução de IoT de monitorização remota | Documentos da Microsoft
description: Neste tutorial, saiba como enviar o status de sensores no IoT DevKit AZ3166 a IoT de monitorização remota solution accelerator para monitorização e de visualização.
author: isabelcabezasm
manager: ''
ms.service: iot-accelerators
services: iot-accelerators
ms.devlang: c
ms.topic: conceptual
ms.date: 12/03/2018
ms.author: isacabe
ms.openlocfilehash: 92ce85a3cc94702468a13348d3a41667498c68f5
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/29/2018
ms.locfileid: "43187396"
---
# <a name="connect-mxchip-iot-devkit-az3166-to-the-iot-remote-monitoring-solution-accelerator"></a>Ligar o MXChip IoT DevKit AZ3166 para o acelerador de solução de monitorização remota do IoT


[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

Neste tutorial, saiba como executar uma aplicação de exemplo no seu DevKit para enviar dados de sensor para do solution accelerator.

O [MXChip IoT DevKit](https://aka.ms/iot-devkit) é uma área de compatível de Arduino tudo-em-um com periféricos avançados e sensores. Pode desenvolver para ele usando [extensão do Visual Studio Code para Arduino](https://aka.ms/arduino). E ele vem com um crescimento [catálogo de projetos](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/) para orientá-lo a soluções de Internet das coisas (IoT) de protótipo que tiram partido dos serviços do Microsoft Azure.

## <a name="what-you-need"></a>Do que precisa

Concluir o [guia de introdução](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started) para:

* Sua DevKit tiver ligado ao Wi-Fi
* Preparar o ambiente de desenvolvimento


## <a name="open-the-remotemonitoring-sample"></a>Abra o exemplo de RemoteMonitoring

1. Desligar o DevKit do seu computador, se estiver ligado.

2. Inicie o VS Code.

3. Ligar o DevKit ao seu computador. O VS Code automaticamente Deteta sua DevKit e abre-se as seguintes páginas:
  * A página de introdução de DevKit.
  * Exemplos de Arduino: Exemplos práticos para começar com DevKit.

4. Expanda o lado esquerdo **exemplos de ARDUINO** secção, navegue até **exemplos para MXCHIP AZ3166 > AzureIoT**e selecione **RemoteMonitoringv2**. Abre uma nova janela do VS Code com uma pasta de projeto.

  ![Abrir projeto de monitorização remota](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-arduino-examples.png)


  > [!NOTE]
  > Se fechar o painel, pode abri-lo novamente. Uso `Ctrl+Shift+P` (macOS: `Cmd+Shift+P`) para abrir a paleta de comandos, escreva **Arduino**e, em seguida, localize e selecione **Arduino: exemplos**.

## <a name="add-a-new-physical-device"></a>Adicionar um novo dispositivo físico

No portal, aceda a **dispositivos** secção e, clique nas **+ novo dispositivo** botão. 

![Adicionar um novo dispositivo](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-add-device.png)

O *novo formulário de dispositivo* devem ser preenchidos.
1. Clique em **físico** no *tipo de dispositivo* secção.
2. Definir seu próprio ID do dispositivo (por exemplo *MXChip* ou *AZ3166*).
3. Escolher **chaves de geração automática** no *chave de autenticação* secção.
4. Clique em *aplicar* botão.

![Adicionar um novo formulário de dispositivo](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-add-new-device-form.png)

Aguarde até que o portal, termine o aprovisionamento do novo dispositivo.

![Provisionamento de um novo dispositivo ](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-add-device-provisioning.png)


Em seguida, a configuração do novo dispositivo, será apresentado.
Copiar o **cadeia de ligação** gerado.

![Cadeia de ligação do dispositivo](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-new-device-connstring.png)


Esta cadeia de ligação será utilizada na secção seguinte.





## <a name="build-and-upload-the-device-code"></a>Criar e carregar o código de dispositivo

Volte para o Visual Studio Code: 

1. Uso `Ctrl+P` (macOS: `Cmd + P`) e o tipo **ligação de dispositivos de configuração de tarefas**.

  ![Escolha a sua subscrição do Azure e o IoT Hub](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/iot-suite-task-config-device-conexion.png)

2. Terminal pede-lhe se pretende utilizar cadeia de ligação do dispositivo de IoT pretende utilizar. Selecione *criar um novo*e cole-o agora.

  ![Cole a cadeia de ligação](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/iot-suite-task-config-device-conexion-choose-iot-hub-press-button-A.png)

3. Por vezes, o terminal pede-lhe para introduzir o modo de configuração. Para fazer isso, premido o botão A, em seguida, push e largue o botão de reposição e, em seguida, largue o botão do r. A tela exibe a DevKit ID e a 'Configuração'.

  ![Ecrã do dispositivo DevKit](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-devkit-screen.png)

  > [!NOTE]
  > A cadeia de ligação deverá ser guardada em sua área de transferência se seguiu a última seção deste tutorial. Caso contrário, deve ir para o portal do Azure e procure o IoT Hub do seu grupo de recursos de monitorização remota. Lá, pode ver o IoT Hub dispositivos ligados e copie a cadeia de ligação do dispositivo.

  ![Procure a cadeia de ligação](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-connection-string-of-a-device.png)


Agora, pode ver o novo dispositivo físico na seção de código de VS "Dispositivos de Hub do IoT do Azure":

![Tenha em atenção a novos dispositivos no Hub IoT](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/iot-suite-new-iot-hub-device.png)

## <a name="test-the-project"></a>Testar o projeto

Quando a aplicação de exemplo é executado, DevKit envia dados de sensores por Wi-Fi para seus Aceleradores de solução de IoT. Para ver o resultado, siga estes passos:

1. Aceda ao seu acelerador de solução de IoT e clique em **DASHBOARD**.

2. Na consola de acelerador de solução de IoT, verá o estado de sensor DevKit. 

![Dados de sensores em Aceleradores de solução de IoT](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-dashboard.png)

Se clicar no nome do sensor (AZ3166) é aberto um separador no lado direito do dashboard, onde pode ver o gráfico de sensores de MX Chip em tempo real.


## <a name="send-a-c2d-message"></a>Enviar uma mensagem de C2D
V2 de monitorização remota permite-lhe invocar o método remoto no dispositivo.
O código de exemplo do MX Chip publica três métodos que pode ver a secção de método quando o sensor está selecionado.

![Métodos MX Chip](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-methods.png)

Pode alterar a cor de um dos leds de MX Chip usando o método "LedColor". Para se fazer isso, selecione a caixa de verificação do dispositivo e clique no botão de agenda. 

![Métodos MX Chip](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-schedule.png)

Escolha o método chamado ChangeColor na lista pendente, onde todos os métodos são apresentados, escrever um nome e aplicam-se.

![Lista pendente MX Chip](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/iot-suite-change-color.png)

Em vários segundos, o seu Chip MX físico deve alterar a cor de RGB levado (abaixo de um botão)

![Ministrados por Chip de MX](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-devkit-led.png)

## <a name="problems-and-feedback"></a>Problemas e comentários

Se tiver problemas, consulte [FAQs](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) ou contacte-nos de que os canais seguintes:

* [Gitter.im](http://gitter.im/Microsoft/azure-iot-developer-kit)
* [Stackoverflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Passos Seguintes

Agora que sabe como ligar um dispositivo de DevKit para seus Aceleradores de solução de IoT e visualizar os dados de sensor, aqui estão os passos sugeridos seguintes:

* [Descrição geral de Aceleradores de solução de IoT](https://docs.microsoft.com/azure/iot-suite/)
* [Ligar um dispositivo de MXChip IoT DevKit a sua aplicação Microsoft IoT Central](https://docs.microsoft.com/microsoft-iot-central/howto-connect-devkit)
* [Kit de programação de IoT](https://microsoft.github.io/azure-iot-developer-kit/)
