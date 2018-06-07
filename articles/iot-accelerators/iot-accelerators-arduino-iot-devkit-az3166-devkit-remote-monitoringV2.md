---
title: 'IoT DevKit na nuvem: ligar o IoT DevKit AZ3166 para acelerador de solução IoT de monitorização remota | Microsoft Docs'
description: Neste tutorial, saiba como enviar estado de sensores em IoT DevKit AZ3166 para acelerador de solução IoT de monitorização remota para monitorização e visualização.
author: isabelcabezasm
manager: ''
ms.service: iot-accelerators
services: iot-accelerators
ms.devlang: c
ms.topic: conceptual
ms.date: 12/03/2018
ms.author: isacabe
ms.openlocfilehash: 8aa4d660fbf785a4caf99bcdeddc86aa9929c50d
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34627744"
---
# <a name="connect-mxchip-iot-devkit-az3166-to-the-iot-remote-monitoring-solution-accelerator"></a>Ligar MXChip IoT DevKit AZ3166 para o acelerador de solução IoT monitorização remota


[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

Neste tutorial, irá aprender a executar uma aplicação de exemplo no seu DevKit para enviar dados de sensor para o acelerador de solução.

O [MXChip IoT DevKit](https://aka.ms/iot-devkit) é uma placa de compatível de Arduino tudo-em-um com periféricos avançados e sensores. Pode desenvolver para o mesmo utilizando [extensão do Visual Studio Code para Arduino](https://aka.ms/arduino). E é fornecido com um crescer [catálogo projetos](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/) para ajudá-lo soluções de Internet das coisas (IoT) de protótipo tirar partido dos serviços do Microsoft Azure.

## <a name="what-you-need"></a>Do que precisa

Concluir o [guia de introdução](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started) para:

* Tem a sua DevKit ligado ao Wi-Fi
* Preparar o ambiente de desenvolvimento


## <a name="open-the-remotemonitoring-sample"></a>Abra o exemplo de RemoteMonitoring

1. Desligar o DevKit do seu computador, se estiver ligado.

2. Inicie o VS Code.

3. Ligar o DevKit para o seu computador. O VS Code automaticamente Deteta o DevKit e abre-se as seguintes páginas:
  * A página de introdução DevKit.
  * Exemplos de Arduino: Amostras prática para começar com DevKit.

4. Expanda o lado esquerdo **ARDUINO exemplos** secção, navegue para **exemplos de MXCHIP AZ3166 > AzureIoT**e selecione **RemoteMonitoringv2**. É aberta uma nova janela do código de VS com uma pasta do projeto no mesmo.

  ![Abrir projeto de monitorização remota](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-arduino-examples.png)


  > [!NOTE]
  > Se ocorrer fechar o painel, pode abri-lo novamente. Utilize `Ctrl+Shift+P` (macOS: `Cmd+Shift+P`) para abrir a paleta de comandos, escreva **Arduino**e, em seguida, localize e selecione **Arduino: exemplos**.

## <a name="add-a-new-physical-device"></a>Adicionar um novo dispositivo físico

No portal, aceda a **dispositivos** secção e, clique no **+ novo dispositivo** botão. 

![Adicionar um novo dispositivo](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-add-device.png)

O *novo formulário de dispositivo* deve ser preenchidos.
1. Clique em **físico** no *tipo de dispositivo* secção.
2. Definir o seu próprio ID do dispositivo (por exemplo *MXChip* ou *AZ3166*).
3. Escolha **automática gerar chaves** no *chave de autenticação* secção.
4. Clique em *aplicar* botão.

![Adicionar um novo formulário de dispositivo](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-add-new-device-form.png)

Aguarde pela conclusão do portal de aprovisionamento do novo dispositivo.

![Aprovisionamento de um novo dispositivo ](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-add-device-provisioning.png)


Em seguida, a configuração do novo dispositivo, será apresentada.
Copiar o **cadeia de ligação** gerado.

![Cadeia de ligação do dispositivo](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-new-device-connstring.png)


Esta cadeia de ligação será utilizada na secção seguinte.





## <a name="build-and-upload-the-device-code"></a>Criar e carregar o código de dispositivo

Volte para o Visual Studio Code: 

1. Utilize `Ctrl+P` (macOS: `Cmd + P`) e o tipo **ligação de dispositivo de configuração de tarefas**.

  ![Escolha a sua subscrição do Azure e o seu IoT Hub](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/iot-suite-task-config-device-conexion.png)

2. O terminal pede-lhe se pretende utilizar a cadeia de ligação do dispositivo IoT que pretende utilizar. Selecione *criar novos*e cole-o agora.

  ![Cole a cadeia de ligação](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/iot-suite-task-config-device-conexion-choose-iot-hub-press-button-A.png)

3. Por vezes, o terminal pede-lhe para introduzir o modo de configuração. Para tal, mantenha premida botão A, em seguida, push e o botão de reposição de versão e, em seguida, versão botão A. O ecrã apresenta a DevKit ID e a 'Configuração'.

  ![Ecrã de DevKit do dispositivo](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-devkit-screen.png)

  > [!NOTE]
  > A cadeia de ligação deve ser guardada na sua área de transferência se seguiu última secção deste tutorial. Caso contrário, deve aceda ao portal do Azure e procure o IoT Hub do seu grupo de recursos de monitorização remota. Aqui, pode ver o IoT Hub dispositivos ligados e copie a cadeia de ligação do dispositivo.

  ![Procure a cadeia de ligação](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-connection-string-of-a-device.png)


Agora, pode ver o novo dispositivo físico na secção VS Code "Azure os dispositivos do IoT Hub":

![Tenha em atenção o novo dispositivo de Hub IoT](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/iot-suite-new-iot-hub-device.png)

## <a name="test-the-project"></a>Testar o projeto

Quando é executada a aplicação de exemplo, DevKit envia dados de sensores através de Wi-Fi para os Aceleradores de solução IoT. Para ver o resultado, siga estes passos:

1. Aceda ao seu acelerador de solução IoT e clique em **DASHBOARD**.

2. Na consola de acelerador de solução IoT, verá o estado do sensor DevKit. 

![Dados de sensores em Aceleradores de solução IoT](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-dashboard.png)

Se clicar no nome do sensor (AZ3166) é aberto um separador no lado direito do dashboard, onde pode ver o gráfico de sensores MX Chip em tempo real.


## <a name="send-a-c2d-message"></a>Enviar uma mensagem de C2D
V2 de monitorização remota permite-lhe invocar o método remota no dispositivo.
O código de exemplo MX Chip publica três métodos que pode ver a secção de método quando o sensor está selecionado.

![Métodos MX Chip](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-methods.png)

Pode alterar a cor de uma das leds MX Chip utilizando o método "LedColor". Para fazê-lo, selecione a caixa de verificação do dispositivo e clique no botão de agenda. 

![Métodos MX Chip](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-schedule.png)

Escolha o método chamado ChangeColor na lista pendente, onde todos os métodos são apresentados, escrever um nome e aplicaram.

![Pendente MX Chip](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/iot-suite-change-color.png)

Em alguns segundos, o Chip MX físico deve alterar a cor dos RGB guiado (abaixo de um botão)

![Guiado Chip de MX](./media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2/azure-iot-suite-devkit-led.png)


## <a name="change-device-id"></a>ID de dispositivo de alteração

Pode alterar o ID de dispositivo do IoT Hub seguindo [neste guia](https://microsoft.github.io/azure-iot-developer-kit/docs/customize-device-id/).


## <a name="problems-and-feedback"></a>Problemas e comentários

Se tiver problemas, consulte [perguntas mais frequentes](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) ou entrar-nos de que as seguintes canais:

* [Gitter.im](http://gitter.im/Microsoft/azure-iot-developer-kit)
* [Stackoverflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Passos Seguintes

Agora que aprendeu como ligar um dispositivo DevKit ao seu Aceleradores de solução IoT e visualizar os dados de sensor, Eis os passos sugeridos:

* [Descrição geral de Aceleradores de solução IoT](https://docs.microsoft.com/azure/iot-suite/)
* [Ligue um dispositivo de MXChip IoT DevKit para a aplicação Microsoft IoT Central](https://docs.microsoft.com/en-us/microsoft-iot-central/howto-connect-devkit)
