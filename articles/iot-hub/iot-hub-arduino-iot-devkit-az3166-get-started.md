---
title: IoT DevKit com a nuvem – ligar IoT DevKit AZ3166 ao IoT Hub do Azure | Documentos da Microsoft
description: Neste tutorial, saiba como configurar e ligar IoT DevKit AZ3166 hub IoT do Azure, pelo que pode enviar dados para a plataforma de cloud do Azure.
author: rangv
manager: jeffya
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 08/27/2018
ms.author: rangv
ms.openlocfilehash: 55901d6f3bcbf5511b6921939fdcba03972efed3
ms.sourcegitcommit: 5b8d9dc7c50a26d8f085a10c7281683ea2da9c10
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/26/2018
ms.locfileid: "47182846"
---
# <a name="connect-iot-devkit-az3166-to-azure-iot-hub"></a>Ligar a IoT DevKit AZ3166 ao Hub IoT do Azure

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

Pode utilizar o [MXChip IoT DevKit](https://microsoft.github.io/azure-iot-developer-kit/) para desenvolver e soluções de Internet das coisas (IoT) de protótipo que tiram partido dos serviços do Microsoft Azure. Ele inclui um quadro compatível com Arduino com periféricos avançados e sensores, um pacote de quadro de código-fonte aberto e crescente [catálogo de projetos](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/).

## <a name="what-you-do"></a>O que fazer

Ligar o [DevKit](https://microsoft.github.io/azure-iot-developer-kit/) para um hub IoT do Azure que criar, coletar os dados de temperatura e humidade a partir de sensores e enviar os dados para o hub IoT.

Não tem um DevKit ainda? Experimente o [DevKit simulador](https://azure-samples.github.io/iot-devkit-web-simulator/) ou [comprar um DevKit](https://aka.ms/iot-devkit-purchase).

## <a name="what-you-learn"></a>O que irá aprender

* Como ligar a IoT DevKit a um ponto de acesso sem fios e prepare seu ambiente de desenvolvimento.
* Como criar um hub IoT e registar um dispositivo para o MXChip IoT DevKit.
* Como recolher dados de sensor ao executar um aplicativo de exemplo no MXChip IoT DevKit.
* Como enviar os dados de sensor ao seu hub IoT.

## <a name="what-you-need"></a>Do que precisa

* Um quadro de MXChip IoT DevKit com um cabo Micro USB. [Obter agora](https://aka.ms/iot-devkit-purchase).
* Um computador com Windows 10 ou macOS 10.10 +.
* Uma subscrição ativa do Azure. [Ativar uma conta gratuita de 30 dias avaliação Microsoft Azure](https://azureinfo.microsoft.com/us-freetrial.html).
  
## <a name="prepare-your-hardware"></a>Prepare o hardware

Conectar o seguinte hardware para o seu computador:

* DevKit quadro
* Cabo micro USB

![Hardware necessário](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/hardware.jpg)

Para ligar o DevKit ao seu computador, siga estes passos:

1. Ligar o fim USB para o seu computador.

2. Ligar o fim de Micro USB para o DevKit.

3. O LED verde para power confirma que a ligação.

   ![Ligações de hardware](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/connect.jpg)

## <a name="configure-wi-fi"></a>Configurar Wi-Fi

Projetos de IoT confiam na ligação à internet. Utilize as seguintes instruções para configurar o DevKit para ligar ao Wi-Fi.

### <a name="enter-ap-mode"></a>Entrar no modo do Pacífico

Premido o botão B, push e largue o botão de reposição e, em seguida, largue o botão B. Sua DevKit entra em modo AP para a configuração de Wi-Fi. A tela exibe o service set identifier (SSID) da DevKit e o endereço IP de portal de configuração.

![Repor o botão, o botão B e o SSID](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/wifi-ap.jpg)

### <a name="connect-to-devkit-ap"></a>Ligar ao DevKit Pacífico

Agora, utilize outro dispositivo de Wi-Fi ativado (computador ou telemóvel) para ligar ao SSID DevKit (realçado na imagem anterior). Deixe a palavra-passe em branco.

![Botão Ligar e as informações de rede](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/connect-ssid.png)

### <a name="configure-wi-fi-for-the-devkit"></a>Configurar Wi-Fi para o DevKit

Abra o endereço IP mostrado no ecrã DevKit no seu computador ou o browser do telemóvel, selecione a rede Wi-Fi que pretende que o DevKit para ligar a e, em seguida, escreva a palavra-passe. Selecione **Ligar**.

![Caixa de palavra-passe e o botão Ligar](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/wifi-portal.png)

Quando a ligação tiver êxito, reinicia o DevKit dentro de alguns segundos. Em seguida, verá o nome do Wi-Fi e o endereço IP na tela:

![Nome do Wi-Fi e endereço IP](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/wifi-ip.jpg)

> [!NOTE] 
> O endereço IP apresentado na foto não pode corresponder o endereço IP real atribuído e apresentados no ecrã de DevKit. Isso é normal, porque o Wi-Fi utiliza o DHCP para atribuir dinamicamente IPs.

Após a configuração de Wi-Fi, as suas credenciais serão mantidas no dispositivo para essa conexão, mesmo que o dispositivo está desligado. Por exemplo, se configurar o DevKit para Wi-Fi na sua casa e, em seguida, efetuar o DevKit para o office, terá de reconfigurar o modo do Pacífico (começando no passo na secção "Introduza o modo do Pacífico") para ligar o DevKit ao seu Wi-Fi do escritório. 

## <a name="start-using-the-devkit"></a>Começar a utilizar o DevKit

A aplicação predefinida em execução no DevKit verifica a versão mais recente do firmware e exibe alguns dados de diagnóstico de sensor para.

### <a name="upgrade-to-the-latest-firmware"></a>Atualizar para o firmware mais recente

> [!NOTE]
> Desde a versão 1.1, DevKit permite ST-SAFE no carregador de inicialização. Tem de atualizar o firmware, se estiver a executar uma versão anterior à versão 1.1.

Se precisar de uma atualização de firmware, o ecrã mostrará as versões de firmware atual e mais recentes. Para atualizar, siga os [atualizar o firmware](https://microsoft.github.io/azure-iot-developer-kit/docs/firmware-upgrading/) guia.

![Exibição de versões de firmware atual e mais recentes](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/firmware.jpg)

> [!NOTE]
> Este é um esforço de uso individual. Depois que comece a desenvolver o DevKit e carregar seu aplicativo, o firmware mais recente virão com a sua aplicação.

### <a name="test-various-sensors"></a>Testar vários sensores

Prima o botão B para testar os sensores. Continue o pressionamento e a liberação do botão B para percorrer cada sensor.

![Apresentação B do botão e o sensor](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/sensors.jpg)

## <a name="prepare-the-development-environment"></a>Preparar o ambiente de desenvolvimento

### <a name="install-azure-iot-workbench"></a>Instale a Bancada de trabalho de IoT do Azure

Recomendamos [Bancada de trabalho do Azure IoT](https://aka.ms/iot-workbench) extensão para o Visual Studio Code para desenvolver no DevKit.

A Bancada de trabalho de IoT do Azure fornece uma experiência integrada para desenvolver soluções de IoT. Ele ajuda a ambos no desenvolvimento de dispositivo e na cloud com o Azure IoT e outros serviços. Pode ver este vídeo do Channel 9 para ter uma visão geral do que ele faz.

Siga estes passos para preparar o ambiente de desenvolvimento para DevKit:

1. Transfira e instale [Arduino IDE](https://www.arduino.cc/en/Main/Software). Ele fornece a coleção de ferramentas necessária para compilar e carregar o código de Arduino.
    * **Windows**: versão de utilizar o instalador do Windows.
    * **macOS**: Arraste e largue o extraídos **Arduino.app** em `/Applications` pasta.
    * **Ubuntu**: Descompacte-o como em pasta `$HOME/Downloads/arduino-1.8.5`

2. Instale [Visual Studio Code](https://code.visualstudio.com/), um editor de código de origem entre plataformas com programação poderosas ferramentas, como a conclusão de código do IntelliSense e depuração.

3. Procure **Bancada de trabalho do Azure IoT** no mercado de extensão e instalá-lo.
    ![Instalar o Azure IoT Workbench](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/install-workbench.png) juntamente com a Bancada de trabalho de IoT, outras extensões dependentes serão instalados.

4. Open **ficheiro > Preferências > definições** e adicionar o seguinte linhas para configurar Arduino.
    * **Windows**:

    ```json
    "arduino.path": "C:\\Program Files (x86)\\Arduino",
    "arduino.additionalUrls": "https://raw.githubusercontent.com/VSChina/azureiotdevkit_tools/master/package_azureboard_index.json"
    ```

    * **macOS**:

    ```json
    "arduino.path": "/Applications",
    "arduino.additionalUrls": "https://raw.githubusercontent.com/VSChina/azureiotdevkit_tools/master/package_azureboard_index.json"
    ```

    * **Ubuntu**:

    ```json
    "arduino.path": "/home/{username}/Downloads/arduino-1.8.5",
    "arduino.additionalUrls": "https://raw.githubusercontent.com/VSChina/azureiotdevkit_tools/master/package_azureboard_index.json"
    ```

5. Clique em `F1` para abrir a paleta de comandos, tipo e selecione **Arduino: Gestor de tabuleiro**. Procure **AZ3166** e instale a versão mais recente.
    ![Instalar o SDK de DevKit](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/install-sdk.png)

### <a name="install-st-link-drivers"></a>Instalar controladores de ST-ligação

[ST-ligação/V2](http://www.st.com/en/development-tools/st-link-v2.html) é a interface USB que IoT DevKit utiliza para comunicar com a sua máquina de desenvolvimento. Siga os passos de específicas de SO para permitir o acesso de máquina para o seu dispositivo.

* **Windows**: Transfira e instale o controlador USB a partir de [STMicroelectronics site](http://www.st.com/en/development-tools/stsw-link009.html).
* **macOS**: nenhum driver é necessário para macOS.
* **Ubuntu**: execute o seguinte no terminal e fim de sessão e início de sessão para a alteração tenha efeito do grupo:
    ```bash
    # Copy the default rules. This grants permission to the group 'plugdev'
    sudo cp ~/.arduino15/packages/AZ3166/tools/openocd/0.10.0/linux/contrib/60-openocd.rules /etc/udev/rules.d/
    sudo udevadm control --reload-rules

    # Add yourself to the group 'plugdev'
    # Logout and log back in for the group to take effect
    sudo usermod -a -G plugdev $(whoami)
    ```

Agora foram todos configurados com a preparar e configurar o ambiente de desenvolvimento. Vamos criar o exemplo de "Hello World" para IoT: enviar dados de telemetria de temperatura hub IoT do Azure.

## <a name="build-your-first-project"></a>Crie seu primeiro projeto

1. Certificar-se de que é o IoT DevKit **não ligado** para o seu computador. Inicie o VS Code primeiro e, em seguida, ligue o DevKit para o seu computador.

1. Na barra de estado correto na parte inferior, consulte a **MXCHIP AZ3166** é mostrado como quadro selecionado e a porta serial com **STMicroelectronics** é utilizado.
    ![Selecionar o quadro e COM](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/select-board.png)

1. Clique em `F1` para abrir a paleta de comandos, tipo e selecione **Bancada de trabalho de IoT: exemplos**. Em seguida, selecione **IoT DevKit** como quadro.

1. Na página de exemplos da bancada de trabalho de IoT, encontrar **começar** e clique em **exemplo aberto**. Em seguida, seleciona o caminho predefinido para transferir o código de exemplo.
    ![Exemplo aberto](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/open-sample.png)

1. Se não tiver a extensão de Arduino no VS Code instalado, clique nas **instalar** no painel de notificação.
    ![Instalar a extensão de Arduino](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/install-arduino-ext.png)

1. Na janela novo projeto aberto, clique em `F1` para abrir a paleta de comandos, tipo e selecione **Bancada de trabalho de IoT: Cloud**, em seguida, selecione **Azure aprovisionar**. Siga o guia passo a passo para concluir o aprovisionamento do seu IoT Hub do Azure e criar o dispositivo.
    ![Aprovisionamento de cloud](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/cloud-provision.png)

1. Clique em `F1` para abrir a paleta de comandos, tipo e selecione **Bancada de trabalho de IoT: dispositivo**, em seguida, selecione **definições de configuração de dispositivos > selecione o IoT Hub dispositivo Connection String**.

1. No DevKit, mantenha premida **botão A**, push e de versão do **repor** botão e, em seguida, versão **botão A**. Sua DevKit entra em modo de configuração e guarda a cadeia de ligação.
    ![Cadeia de ligação](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/connection-string.png)

1. Clique em `F1` novamente, escreva e selecione **Bancada de trabalho de IoT: dispositivo**, em seguida, selecione **dispositivo carregar**.
    ![Carregamento de Arduino](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/arduino-upload.png)

O DevKit reinicia e começa a execução do código.

> [!NOTE]
> Se houver erros ou falhas, sempre pode recuperar ao executar o comando novamente.

## <a name="test-the-project"></a>Testar o projeto

### <a name="view-the-telemetry-sent-to-azure-iot-hub"></a>Ver a telemetria enviada para o IoT Hub do Azure

Clique no ícone de plug power na barra de status para abrir o Monitor de série: ![Serial monitor](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/serial-monitor.png)

O aplicativo de exemplo está em execução com êxito quando vir os seguintes resultados:

* O Monitor Serial apresenta a mensagem enviada para o IoT Hub.
* O LED no MXChip IoT DevKit é intermitente.

![Saída de Serial monitor](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/result-serial-output.png)

### <a name="view-the-telemetry-received-by-azure-iot-hub"></a>Ver a telemetria recebida pelo IoT Hub do Azure

Pode usar [Kit de ferramentas do Azure IoT](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) para monitorizar mensagens do dispositivo para a nuvem (D2C) no IoT Hub.

1. No Visual Studio Code, procure **Kit de ferramentas do Azure IoT** no mercado de extensão e instalá-lo.

1. Iniciar sessão [portal do Azure](https://portal.azure.com/), encontrar o IoT Hub que criou.
    ![Portal do Azure](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/azure-iot-hub-portal.png)

1. Na **políticas de acesso partilhado** painel, clique nas **iothubowner política**e anote a cadeia de ligação do IoT hub.
    ![Cadeia de ligação do IoT Hub do Azure](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/azure-portal-conn-string.png)

1. No Visual Studio Code, expanda **dispositivos do AZURE IOT HUB** no canto inferior esquerdo, clique em **definir cadeia de ligação do IoT Hub**.
    ![Definir cadeia de ligação do IoT Hub do Azure](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/azure-iot-toolkit-conn-string.png)

1. Clique em **IoT: começar a monitorizar mensagens D2C** no menu de contexto.

1. Na **saída** painel, pode ver as mensagens D2C de entrada para o IoT Hub.
    ![Mensagens D2C](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/azure-iot-toolkit-console.png)

## <a name="problems-and-feedback"></a>Problemas e comentários

Se tiver problemas, pode verificar a existência de uma solução no [IoT DevKit FAQ](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) ou contacte-na partir de [Gitter](https://gitter.im/Microsoft/azure-iot-developer-kit). Pode também dar feedback para deixar um comentário nesta página.

## <a name="next-steps"></a>Passos Seguintes

Ligou com êxito um MXChip IoT DevKit ao IoT hub e enviou os dados de sensor capturada ao seu hub IoT.

[!INCLUDE [iot-hub-get-started-az3166-next-steps](../../includes/iot-hub-get-started-az3166-next-steps.md)]
