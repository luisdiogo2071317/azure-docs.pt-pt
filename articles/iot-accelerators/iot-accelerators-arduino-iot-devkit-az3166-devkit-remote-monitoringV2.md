---
title: IoT DevKit com a nuvem – ligar IoT DevKit AZ3166 a acelerador de solução de IoT de monitorização remota | Documentos da Microsoft
description: Neste tutorial, saiba como enviar o status de sensores no IoT DevKit AZ3166 a IoT de monitorização remota solution accelerator para monitorização e de visualização.
author: isabelcabezasm
manager: ''
ms.service: iot-accelerators
services: iot-accelerators
ms.devlang: c
ms.topic: conceptual
ms.date: 05/09/2018
ms.author: isacabe
ms.openlocfilehash: 35ef6ef02e5ae8a4b9231121615f44e0dc00ad15
ms.sourcegitcommit: 5a9be113868c29ec9e81fd3549c54a71db3cec31
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/11/2018
ms.locfileid: "44378742"
---
# <a name="connect-mxchip-iot-devkit-az3166-to-the-iot-remote-monitoring-solution-accelerator"></a>Ligar o MXChip IoT DevKit AZ3166 para o acelerador de solução de monitorização remota do IoT

[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

Neste tutorial, saiba como executar uma aplicação de exemplo no seu DevKit para enviar dados de sensor para do solution accelerator.

O [MXChip IoT DevKit](https://aka.ms/iot-devkit) é uma área de compatível de Arduino tudo-em-um com periféricos avançados e sensores. Pode desenvolver para ele usando [Bancada de trabalho do Azure IoT](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.vscode-iot-workbench) no Visual Studio Code. E ele vem com um crescimento [catálogo de projetos](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/) para orientá-lo a soluções de Internet das coisas (IoT) de protótipo que tiram partido dos serviços do Microsoft Azure.

## <a name="what-you-need"></a>Do que precisa

Avance para o [guia de introdução](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started) e **concluir as secções seguintes apenas**:

* Prepare o hardware
* Configurar Wi-Fi
* Começar a utilizar o DevKit
* Preparar o ambiente de desenvolvimento


## <a name="create-an-azure-iot-remote-monitoring-solution-accelerator"></a>Criar um acelerador de solução de monitorização remota do Azure IoT

O dispositivo de AZ3166 que vai criar neste tutorial envia dados para uma instância do solution accelerator monitorização remota. Se ainda não tiver aprovisionado uma instância na sua conta do Azure, siga os [guia de introdução](https://docs.microsoft.com/azure/iot-accelerators/quickstart-remote-monitoring-deploy) guia para fazer isso.

Quando a implementação da solução de monitorização remota termina, abra o dashboard de solução.

![O dashboard da solução](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/azure-iot-suite-dashboard-info.png)

## <a name="add-a-device-to-the-remote-monitoring-solution"></a>Adicionar um dispositivo à solução de monitorização remota

1. No dashboard, aceda a **dispositivos** secção e clique nas **novo dispositivo**.
   ![Adicionar um novo dispositivo](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/azure-iot-suite-add-device.png)

2. Configurar o dispositivo com as informações abaixo e clique em **aplicar**.
  * Tipo de dispositivo: **físico**
  * ID do dispositivo: **AZ3166**
  * Tipo de autenticação: **chave simétrica**
  * Chave de autenticação: **chaves de geração automática**
  
  ![Adicionar um novo formulário de dispositivo](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/azure-iot-suite-add-new-device-form.png)

3. Depois de criado o novo dispositivo, copie os **chave primária da cadeia de ligação**. Este é o dispositivo que acabou de ser criar no IoT Hub do Azure por baixo.
  
  ![Cadeia de ligação do dispositivo](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/azure-iot-suite-new-device-connstring.png)

## <a name="open-the-remote-monitoring-sample-in-vs-code"></a>Abra o exemplo de monitorização remota no VS Code

1. Certificar-se de que é o IoT DevKit **não ligado** para o seu computador. Inicie o VS Code primeiro e, em seguida, ligue o DevKit para o seu computador.

1. Clique em `F1` para abrir a paleta de comandos, tipo e selecione **Bancada de trabalho de IoT: exemplos**. Em seguida, selecione **IoT DevKit** como quadro.

1. Encontrar **monitorização remota** e clique em **exemplo aberto**. Abre uma nova janela do VS Code com a pasta de projeto.
  ![Bancada de trabalho de IoT, selecione exemplo de monitorização remota](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/iot-workbench-example.png)

## <a name="configure-iot-hub-device-connection-string"></a>Configurar a cadeia de ligação de dispositivos do IoT Hub

1. Mude a IoT DevKit em **modo de configuração**. Para tal:
   * Mantenha premida botão **A**.
   * Push e de versão da **repor** botão.

2. A tela exibe a DevKit ID e a 'Configuração'.
   
  ![Modo de configuração de IoT DevKit](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/devkit-configuration-mode.png)

3. Clique em `F1` para abrir a paleta de comandos, tipo e selecione **Bancada de trabalho de IoT: dispositivo > definições de configuração de dispositivos**.

4. Cole a cadeia de ligação que copiou clique `Enter` para configurá-lo.

## <a name="build-and-upload-the-device-code"></a>Criar e carregar o código de dispositivo

1. Clique em `F1` para abrir a paleta de comandos, tipo e selecione **Bancada de trabalho de IoT: dispositivo > carregar dispositivos**.
  ![Bancada de trabalho de IoT: Dispositivo - > carregar](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/iot-workbench-device-upload.png)

1. VS Code, em seguida, inicia a compilação e carregar o código para o seu DevKit.
  ![Bancada de trabalho de IoT: Dispositivo - > carregado](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/iot-workbench-device-uploaded.png)

O DevKit reinicia e começa a execução do código.

## <a name="test-the-project"></a>Testar o projeto

### <a name="view-the-telemetry-sent-to-remote-monitoring-solution"></a>Ver a telemetria enviada para a solução de monitorização remota

Quando a aplicação de exemplo é executado, DevKit envia dados de sensor por Wi-Fi à sua solução de monitorização remota. Para ver o resultado, siga estes passos:

1. Aceda ao seu dashboard de solução e clique em **dispositivos**.

1. Clique no nome do dispositivo (AZ3166) é aberto um separador no lado direito do dashboard, onde pode ver o estado de sensor em DevKit em tempo real.
  ![Dados de sensores no Azure IoT Suite](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/azure-iot-suite-dashboard.png)

### <a name="send-a-c2d-message"></a>Enviar uma mensagem de C2D

Solução de monitorização remota permite-lhe invocar o método remoto no dispositivo. O código de sxample publica três métodos que pode ver na **método** secção quando o sensor está selecionado.

![Métodos de IoT DevKit](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/azure-iot-suite-methods.png)

Vamos tente alterar a cor de um dos LEDs de DevKit usando o método "LedColor".

1. Selecione o **AZ3166** na lista de dispositivos e clique nas **tarefas**.

  ![Criar uma tarefa](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/azure-iot-suite-job.png)

2. Configurar as tarefas como a seguir e clique em **aplicar**.
  * Selecione tarefa: **executar o método**
  * Nome do método: **LedColor**
  * Nome da tarefa: **ChangeLedColor**
  
  ![Definições de tarefa](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/iot-suite-change-color.png)

Em alguns segundos, sua DevKit deve de alterar a cor do RGB LED (abaixo do botão um).

![Vermelho IoT DevKit por instrutor](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/azure-iot-suite-devkit-led.png)

## <a name="clean-up-resources"></a>Limpar recursos

Se planear avançar para os tutoriais, deixe o acelerador de soluções de Monitorização Remota implementado.

Se já não precisar do solution accelerator, elimine-o partir da página de soluções aprovisionado, selecioná-la e, em seguida, clicando em Eliminar solução:

![Eliminar solução](media/quickstart-remote-monitoring-deploy/deletesolution.png)

## <a name="problems-and-feedback"></a>Problemas e comentários

Se tiver problemas, consulte [perguntas frequentes sobre do IoT DevKit](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) ou contacte-nos com os seguintes canais:

* [Gitter.im](http://gitter.im/Microsoft/azure-iot-developer-kit)
* [Stackoverflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Passos Seguintes

Agora que sabe como ligar um dispositivo de DevKit ao seu acelerador de solução de monitorização remota do Azure IoT e visualizar os dados de sensor, aqui estão os passos sugeridos seguintes:

* [Azure descrição geral do Aceleradores de solução IoT](https://docs.microsoft.com/azure/iot-suite/)
* [Personalizar a IU](../iot-accelerators/iot-accelerators-remote-monitoring-customize.md)
* [Ligar o IoT DevKit à sua aplicação do Azure IoT Central](../iot-central/howto-connect-devkit.md)