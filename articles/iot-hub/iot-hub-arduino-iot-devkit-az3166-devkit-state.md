---
title: Utilizar o Azure dispositivos duplos para controlar o LED de utilizador MXChip IoT DevKit | Microsoft Docs
description: Neste tutorial, saiba como monitorizar DevKit Estados e controlar o utilizador LED com dispositivos duplos do IoT Hub do Azure.
services: iot-hub
documentationcenter: ''
author: liydu
manager: timlt
tags: ''
keywords: ''
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/04/2018
ms.author: liydu
ms.openlocfilehash: 3678eb20429d9e5123272499d474fdec2cdab065
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
---
# <a name="mxchip-iot-devkit"></a>MXChip IoT DevKit

Pode utilizar este exemplo, para monitorizar os Estados de informações e sensor MXChip IoT DevKit Wi-Fi e controlar a cor do LED de utilizador utilizando dispositivos duplos do IoT Hub do Azure.

## <a name="what-you-learn"></a>O que irá aprender

- Como monitorizar os Estados de sensor MXChip DevKit de IoT.
- Como utilizar o Azure dispositivos duplos para controlar a cor do DevKit RGB LED.

## <a name="what-you-need"></a>Do que precisa

- Configurar o ambiente de desenvolvimento, seguindo o [guia de introdução](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started).
- A partir da janela de terminal GitBash (ou outra interface de linha de comandos de Git), escreva os seguintes comandos:
    - `git clone https://github.com/DevKitExamples/DevKitState.git`
    - `cd DevKitState`
    - `code .`

## <a name="provision-azure-services"></a>Aprovisionar os serviços do Azure

1. Clique em de **tarefas** menu pendente no Visual Studio Code e selecione **executar tarefa...**   -  **nuvem-provision**.
2. O progresso é apresentado sob o **TERMINAL** separador do **bem-vindo** painel.
3. Quando lhe for pedido com a mensagem *que subscrição que pretende escolher*, selecione uma subscrição.
4. Selecione ou escolha um grupo de recursos. 
 
    > [!NOTE]
    > Se já tiver um IoT Hub gratuito, este passo será ignorado.

5. Quando lhe for pedido com a mensagem *que o IoT hub que pretende escolher*, selecione ou crie um IoT Hub.
6. Algo semelhante ao *aplicação de função: nome de aplicação de função: xxx*, é apresentada. Anote o nome da aplicação de função; será utilizado num passo posterior.
7. Aguarde que a implementação do modelo Azure Resource Manager concluir, que é indicado quando a mensagem *implementação de modelo do Resource Manager: feito* é apresentado.

## <a name="deploy-function-app"></a>Implementar a aplicação de função

1. Clique em de **tarefas** menu pendente no Visual Studio Code e selecione **executar tarefa...**   -  **nuvem-implementar**.
2. Aguarde a carregar o processo de terminar; código de aplicação de função a mensagem *implementa a aplicação de função: feito* é apresentado.

## <a name="configure-iot-hub-device-connection-string-in-devkit"></a>Configurar a cadeia de ligação do dispositivo IoT Hub no DevKit

1. Ligar o seu DevKit de IoT MXChip para o seu computador.
2. Clique em de **tarefas** menu pendente no Visual Studio Code e selecione **executar tarefa...**   -  **ligação de configuração de dispositivo**
3. No MXChip IoT DevKit, prima e botão de suspensão **A**, prima a **repor** botão e, em seguida, versão botão **A** para tornar o DekKit em modo de configuração.
4. Aguarde o processo de configuração de cadeia de ligação seja concluída.

## <a name="upload-arduino-code-to-devkit"></a>Carregar código Arduino para DevKit

Com o seu DevKit de IoT MXChip ligado ao seu computador:
1. Clique em de **tarefas** menu pendente no Visual Studio Code e selecione **executar tarefa de compilação...** O sketch Arduino é compilado e carregada para o DevKit.
2. Quando o sketch foi carregado com êxito, um *compilação & carregar Sketch: êxito* é apresentada a mensagem.

## <a name="monitor-devkit-state-in-browser"></a>Estado do monitor DevKit no Browser

1. Num browser, abra o `DevKitState\web\index.html` ficheiro – que foi criado durante o [o que precisa de](#whatyouneed) passo.
2. É apresentada a página Web seguinte:![Especifique o nome de aplicação de função.](media/iot-hub-arduino-iot-devkit-az3166-devkit-state/devkit-state-function-app-name.png)
1. Introduza o nome da aplicação de função que escrevemos anteriormente para baixo.
2. Clique em de **Connect** botão
3. Dentro de alguns segundos, a página é atualizada e mostra o estado da ligação Wi-Fi a DevKit e o estado de cada um dos sensores a carregar.

## <a name="control-the-devkits-user-led"></a>Controlar utilizador o DevKit LED

1. Clique no gráfico de LED de utilizador na ilustração da página Web.
2. Dentro de alguns segundos, o ecrã é atualizada e mostra o estado atual da cor do utilizador LED.
3. Experimente alterar o valor de cor do LED RGB clicando em várias localizações em controlos de deslize RGB.

## <a name="example-operation"></a>Operação de exemplo

![Procedimento de teste de exemplo](media/iot-hub-arduino-iot-devkit-az3166-devkit-state/devkit-state.gif)

> [!NOTE]
> Pode ver os dados brutos do dispositivo duplo no portal do Azure: IoT Hub -\> dispositivos de IoT -\> *\<dispositivo\>*  - \> dispositivo duplo.

## <a name="next-steps"></a>Passos Seguintes

Aprendeu como:
- Ligue um dispositivo de MXChip IoT DevKit ao seu acelerador de solução de monitorização remota do Azure IoT.
- Utilize a função de duplos de dispositivos do IoT do Azure para detetar e controlar a cor do DevKit RGB LED.

Eis os passos sugeridos:

* [Monitorização remota do IoT solução acelerador descrição geral do Azure](https://docs.microsoft.com/azure/iot-suite/)
* [Ligue um dispositivo de MXChip IoT DevKit à sua aplicação do Azure IoT Central](https://docs.microsoft.com/microsoft-iot-central/howto-connect-devkit)
