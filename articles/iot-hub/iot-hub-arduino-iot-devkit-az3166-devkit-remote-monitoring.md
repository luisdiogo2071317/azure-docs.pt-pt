---
title: IoT DevKit para a nuvem – ligar IoT MXChip DevKit ao IoT Hub do Azure | Microsoft Docs
description: Neste tutorial, saiba como enviar estado de sensores em IoT DevKit AZ3166 para o acelerador de solução de monitorização remota do Azure IoT.
author: liydu
manager: jeffya
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 02/02/2018
ms.author: liydu
ms.openlocfilehash: 6c5c12ffeacad9a3dd56ac561d9b4fe1a6e67eea
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34631501"
---
# <a name="connect-mxchip-iot-devkit-to-azure-iot-remote-monitoring-solution-accelerator"></a>Ligar MXChip IoT DevKit acelerador de solução de monitorização remota do Azure IoT

Neste tutorial, irá aprender a executar uma aplicação de exemplo no seu DevKit para enviar dados de sensor para o acelerador de solução de monitorização remota do Azure IoT.

O [MXChip IoT DevKit](https://aka.ms/iot-devkit) é uma placa de compatível de Arduino tudo-em-um com periféricos avançados e sensores. Pode desenvolver para o mesmo utilizando [extensão do Visual Studio Code para Arduino](https://aka.ms/arduino). E é fornecido com um crescer [catálogo projetos](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/) para ajudá-lo soluções de Internet das coisas (IoT) de protótipo tirar partido dos serviços do Microsoft Azure.

## <a name="what-you-need"></a>Do que precisa

Concluir o [guia de introdução](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started) para:

* Tem a sua DevKit ligado ao Wi-Fi
* Preparar o ambiente de desenvolvimento

Uma subscrição ativa do Azure. Se não tiver uma, pode registar através de um destes dois métodos:

* Ativar um [conta de Microsoft Azure avaliação de 30 dias gratuita](https://azure.microsoft.com/free/)
* Afirmação sua [crédito do Azure](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) se forem subscritor MSDN ou o Visual Studio

## <a name="create-an-azure-iot-remote-monitoring-solution-accelerator"></a>Criar um acelerador de solução de monitorização remota do Azure IoT

1. Aceda a [site de Aceleradores de solução IoT do Azure](https://www.azureiotsolutions.com/) e clique em **criar uma nova solução**.
  ![Selecione o tipo de acelerador de solução IoT do Azure](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/azure-iot-suite-solution-types.png)
  > [!WARNING]
  > Por predefinição, este exemplo cria um S2 IoT Hub depois de criar um acelerador de solução de monitorização remota do IoT. Se este hub IoT não está a ser utilizado com grande número de dispositivos, recomendamos vivamente que mudá-lo a partir de S2 para S1 e eliminará o acelerador de solução de monitorização remota do IoT IoT Hub relacionados também podem ser eliminado, quando já não precisar dele. 

2. Selecione **monitorização remota**.

3. Introduza um nome de solução, selecione uma subscrição e região e, em seguida, clique em **criar solução**. A solução pode demorar algum tempo a ser aprovisionado.
  ![Criar a solução](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/azure-iot-suite-new-solution.png)

4. O aprovisionamento esteja concluída, clique em **iniciar**. Alguns dispositivos simulados são criados para a solução durante o processo de aprovisionamento. Clique em **dispositivos** para Assista aos vídeos. ![Dashboard](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/azure-iot-suite-new-solution-created.png)
  ![consola](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/azure-iot-suite-console.png)

5. Clique em **adicionar um dispositivo**.

6. Clique em **adicionar novos** para **personalizadas do dispositivo**.
  ![Adicionar novo dispositivo](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/azure-iot-suite-add-new-device.png)

7. Clique em **deixar-me a definir o meu próprio ID do dispositivo**, introduza `AZ3166`e, em seguida, clique em **criar**.
  ![Criar dispositivo com o ID](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/azure-iot-suite-new-device-configuration.png)

8. Tome nota do **o nome de anfitrião do IoT Hub**e clique em **feito**.

## <a name="open-the-remotemonitoring-sample"></a>Abra o exemplo de RemoteMonitoring

1. Desligar o DevKit do seu computador, se estiver ligado.

2. Inicie o VS Code.

3. Ligar o DevKit para o seu computador. O VS Code automaticamente Deteta o DevKit e abre-se as seguintes páginas:
  * A página de introdução DevKit.
  * Exemplos de Arduino: Amostras prática para começar com DevKit.

4. Expanda o lado esquerdo **ARDUINO exemplos** secção, navegue para **exemplos de MXCHIP AZ3166 > AzureIoT**e selecione **RemoteMonitoring**. É aberta uma nova janela do código de VS com uma pasta do projeto no mesmo.
  > [!NOTE]
  > Se ocorrer fechar o painel, pode abri-lo novamente. Utilize `Ctrl+Shift+P` (macOS: `Cmd+Shift+P`) para abrir a paleta de comandos, escreva **Arduino**e, em seguida, localize e selecione **Arduino: exemplos**.

## <a name="provision-required-azure-services"></a>Aprovisionar necessários serviços do Azure

Na janela da solução, executar a tarefa `Ctrl+P` (macOS: `Cmd+P`), introduzindo `task cloud-provision` na caixa de texto fornecidos:

No terminal VS Code, uma linha de comandos interativa orienta-o aprovisionamento de serviços do Azure necessários:

![Aprovisionar os recursos do Azure](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/provision.png)

## <a name="build-and-upload-the-device-code"></a>Criar e carregar o código de dispositivo

1. Utilize `Ctrl+P` (macOS: `Cmd + P`) e o tipo **ligação de dispositivo de configuração de tarefas**.

2. O terminal pede-lhe se pretende utilizar a cadeia de ligação que obtém a partir do `task cloud-provision` passo. Também foi entrada sua própria cadeia de ligação do dispositivo clicando em 'Criar novo …'

3. O terminal pede-lhe para introduzir o modo de configuração. Para tal, mantenha premida botão A, em seguida, push e no botão de reposição de versão. O ecrã apresenta a DevKit ID e a 'Configuração'.
  ![Cadeia de ligação de entrada](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/config-device-connection.png)

4. Depois de `task config-device-connection` terminar, clique em `F1` para carregar comandos VS Code e selecione `Arduino: Upload`, em seguida, o VS Code inicia a verificar e carregar o Arduino sketch: ![verificação e de carregamento do sketch Arduino](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/arduino-upload.png)

O DevKit reinicia e começa a executar o código.

## <a name="test-the-project"></a>Testar o projeto

Quando é executada a aplicação de exemplo, DevKit envia dados de sensores através de Wi-Fi para o acelerador de solução de monitorização remota do Azure IoT. Para ver o resultado, siga estes passos:

1. Aceda ao seu acelerador de solução de monitorização remota do Azure IoT e clique em **DASHBOARD**.

2. Na consola de solução de monitorização remota, verá o estado do sensor DevKit.

![Dados de sensores em acelerador de solução de monitorização remota do Azure IoT](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/sensor-status.png)

## <a name="change-device-id"></a>ID de dispositivo de alteração

Pode alterar o ID de dispositivo do IoT Hub seguindo [neste guia](https://microsoft.github.io/azure-iot-developer-kit/docs/customize-device-id/). E se pretender alterar o codificado **AZ3166** para personalizado ID de dispositivo no código. [Aqui](https://github.com/Microsoft/devkit-sdk/blob/master/AZ3166/src/libraries/AzureIoT/examples/RemoteMonitoring/RemoteMonitoring.ino#L23) é a linha de código que pode modificar.

## <a name="problems-and-feedback"></a>Problemas e comentários

Se tiver problemas, consulte [perguntas mais frequentes](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) ou entrar-nos de que as seguintes canais:

* [Gitter.im](http://gitter.im/Microsoft/azure-iot-developer-kit)
* [Stackoverflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Passos Seguintes

Agora que aprendeu como ligar um dispositivo DevKit ao seu acelerador de solução de monitorização remota do Azure IoT e visualizar os dados de sensor, Eis os passos sugeridos:

* [IoT solução Aceleradores descrição geral do Azure](https://docs.microsoft.com/azure/iot-suite/)
* [Ligue um dispositivo de MXChip IoT DevKit à sua aplicação do Azure IoT Central](https://docs.microsoft.com/microsoft-iot-central/howto-connect-devkit)
