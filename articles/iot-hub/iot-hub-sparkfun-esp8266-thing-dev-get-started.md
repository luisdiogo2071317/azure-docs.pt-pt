---
title: ESP8266 para a cloud - ligar Sparkfun ESP8266 Thing Dev hub IoT do Azure | Documentos da Microsoft
description: Saiba como configurar e ligar Sparkfun ESP8266 Thing Dev hub de IoT do Azure para o mesmo para enviar dados para a plataforma de cloud do Azure neste tutorial.
author: rangv
manager: ''
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 04/11/2018
ms.author: rangv
ms.openlocfilehash: 75ff53d5be29af08bb8e9c1b41f61040e5710cf7
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38452520"
---
# <a name="connect-sparkfun-esp8266-thing-dev-to-azure-iot-hub-in-the-cloud"></a>Ligar Sparkfun ESP8266 Thing Dev ao IoT Hub do Azure na cloud

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

![ligação entre DHT22 Thing Dev e IoT Hub](media/iot-hub-sparkfun-thing-dev-get-started/1_connection-hdt22-thing-dev-iot-hub.png)

## <a name="what-you-will-do"></a>O que fará

Ligue-se Sparkfun ESP8266 Thing Dev para um hub IoT, irá criar. Em seguida, execute um aplicativo de exemplo no ESP8266 para recolher dados de temperatura e humidade de um sensor DHT22. Por fim, envie os dados de sensor ao seu hub IoT.

> [!NOTE]
> Se estiver a utilizar outros quadros ESP8266, ainda pode seguir estes passos para ligá-lo ao seu hub IoT. Dependendo do quadro de ESP8266 estiver a utilizar, poderá ter de reconfigurar o `LED_PIN`. Por exemplo, se estiver a utilizar ESP8266 de IA Thinker, pode alterá-lo partir `0` para `2`. Ainda não tem um kit?: clique em [aqui](http://azure.com/iotstarterkits)

## <a name="what-you-will-learn"></a>O que aprenderá

* Como criar um hub IoT e registar um dispositivo para programação de coisa.
* Como ligar Thing Dev com o sensor e o seu computador.
* Como recolher dados de sensor ao executar um aplicativo de exemplo na programação de coisa.
* Como enviar os dados de sensor ao seu hub IoT.

## <a name="what-you-will-need"></a>O que irá precisar

![Partes necessários para o tutorial](media/iot-hub-sparkfun-thing-dev-get-started/2_parts-needed-for-the-tutorial.png)

Para concluir esta operação, tem as seguintes partes do seu Kit de iniciante de programação coisa:

* Quadro Sparkfun ESP8266 Thing Dev.
* Uma USB Micro a cabo USB de um tipo.

Também é necessário o seguinte para o seu ambiente de desenvolvimento:

* Uma subscrição ativa do Azure. Se não tiver uma conta do Azure, [criar uma conta gratuita do Azure](https://azure.microsoft.com/free/) em apenas alguns minutos.
* Mac ou PC com Windows ou Ubuntu.
* Rede sem fio Sparkfun ESP8266 Thing Dev ligar a.
* Ligação à Internet para transferir a ferramenta de configuração.
* [Arduino IDE](https://www.arduino.cc/en/main/software) versão 1.6.8 (ou mais recente), as versões anteriores não irão funcionar com a biblioteca de AzureIoT.

Os seguintes itens são opcionais no caso de não ter um sensor. Tem também a opção de utilizar dados de sensores simulados.

* Sensor de temperatura e humidade um Adafruit DHT22.
* Um breadboard.
* Cablagem de jumper M/M.

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="connect-esp8266-thing-dev-with-the-sensor-and-your-computer"></a>Ligar ESP8266 Thing Dev com o sensor e o seu computador

### <a name="connect-a-dht22-temperature-and-humidity-sensor-to-esp8266-thing-dev"></a>Ligar um sensor de temperatura e humidade DHT22 a ESP8266 Thing Dev

Utilize cabos, mas breadboard e jumper para estabelecer a ligação da seguinte forma. Se não tiver um sensor, ignore esta secção porque pode usar os dados de sensores simulados em vez disso.

![Referência de ligações](media/iot-hub-sparkfun-thing-dev-get-started/15_connections_on_breadboard.png)

Para pins de sensor, utilizamos a ligação seguinte:

| Iniciar (Sensor)           | Fim (quadro)           | Cor do cabo   |
| -----------------------  | ---------------------- | ------------: |
| VDD (Pin 27F)            | 3 v (8A de Pin)           | Cabo vermelho     |
| DADOS (Pin 28F)           | GPIO 2 (9A de Pin)       | Cabo branco    |
| GND (Pin 30F)            | GND (7J de Pin)          | Cabo preto   |


- Para obter mais informações, consulte: [configuração do sensor DHT22](http://cdn.sparkfun.com/datasheets/Sensors/Weather/RHT03.pdf) e [especificação Sparkfun ESP8266 Thing Dev](https://www.sparkfun.com/products/13711)

Agora seu Sparkfun ESP8266 Thing Dev devem estar ligado com um sensor de trabalho.

![ligar dht22 com ESP8266 Thing Dev](media/iot-hub-sparkfun-thing-dev-get-started/8_connect-dht22-thing-dev.png)

### <a name="connect-sparkfun-esp8266-thing-dev-to-your-computer"></a>Ligar Sparkfun ESP8266 Thing Dev para o seu computador

Utilize a USB Micro a cabo USB de um tipo para ligar Sparkfun ESP8266 Thing Dev para o seu computador da seguinte forma.

![ligar feather huzzah para o seu computador](media/iot-hub-sparkfun-thing-dev-get-started/9_connect-thing-dev-computer.png)

### <a name="add-serial-port-permissions--ubuntu-only"></a>Adicionar permissões de porta serial – apenas Ubuntu

Se usar o Ubuntu, certifique-se de que um utilizador normal tem as permissões para operar no USB porta de Sparkfun ESP8266 coisa programação. Para adicionar permissões de porta serial de um utilizador normal, siga estes passos:

1. Execute os seguintes comandos num terminal:

   ```bash
   ls -l /dev/ttyUSB*
   ls -l /dev/ttyACM*
   ```

   Obtém uma os seguintes resultados:

   * crw-rw---1 raiz uucp xxxxxxxx
   * crw-rw---1 raiz dialout xxxxxxxx

   Na saída, tenha em atenção `uucp` ou `dialout` isto é o nome de proprietário de grupo da porta USB.

1. Adicione o utilizador ao grupo executando o seguinte comando:

   ```bash
   sudo usermod -a -G <group-owner-name> <username>
   ```

   `<group-owner-name>` é o nome de proprietário do grupo que obteve no passo anterior. `<username>` é o nome de utilizador do Ubuntu.

1. Termine a Ubuntu e inicie sessão novamente para que a alteração tenha efeito.

## <a name="collect-sensor-data-and-send-it-to-your-iot-hub"></a>Recolher dados de sensor e enviá-lo ao seu hub IoT

Nesta secção, implementar e executar um aplicativo de exemplo na programação. do Sparkfun ESP8266 coisa O aplicativo de exemplo pisca LED no Sparkfun ESP8266 Thing Dev e envia os dados de temperatura e humidade recolhidos a partir do sensor DHT22 ao seu hub IoT.

### <a name="get-the-sample-application-from-github"></a>Obter a aplicação de exemplo a partir do GitHub

O aplicativo de exemplo está alojado no GitHub. Clone o repositório de exemplo que contém a aplicação de exemplo do GitHub. Para clonar o repositório de exemplo, siga estes passos:

1. Abra um prompt de comando ou uma janela de terminal.
1. Ir para uma pasta onde pretende que o aplicativo de exemplo para serem armazenados.
1. Execute o seguinte comando:

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-SparkFun-ThingDev-client-app.git
   ```

Instale o pacote para Sparkfun ESP8266 Thing Dev no IDE de Arduino:

1. Abra a pasta onde o aplicativo de exemplo é armazenado.
1. Abra o ficheiro de app.ino na pasta da aplicação na Arduino IDE.

   ![Abra a aplicação de exemplo no arduino ide](media/iot-hub-sparkfun-thing-dev-get-started/10_arduino-ide-open-sample-app.png)

1. No Arduino IDE, clique em **arquivo** > **preferências**.
1. No **preferências** caixa de diálogo caixa, clique no ícone junto aos **URLs de Gestor de quadros adicionais** caixa de texto.
1. Na janela de pop-up, introduza o seguinte URL e, em seguida, clique em **OK**.

   `http://arduino.esp8266.com/stable/package_esp8266com_index.json`

   ![aponte para um url do pacote no arduino ide](media/iot-hub-sparkfun-thing-dev-get-started/11_arduino-ide-package-url.png)

1. Na **preferência** caixa de diálogo, clique em **OK**.
1. Clique em **ferramentas** > **quadro** > **quadros Manager**e, em seguida, procure esp8266.
   Deve ser instalado ESP8266 com uma versão do 2.2.0 ou posterior.

   ![O pacote de esp8266 está instalado](media/iot-hub-sparkfun-thing-dev-get-started/12_arduino-ide-esp8266-installed.png)

1. Clique em **ferramentas** > **quadro** > **Sparkfun ESP8266 Thing Dev**.

### <a name="install-necessary-libraries"></a>Instalar as bibliotecas necessárias

1. No Arduino IDE, clique em **esboço** > **biblioteca incluem** > **gerir bibliotecas**.
1. Pesquisa para a biblioteca seguinte nomes individualmente. Para cada um a biblioteca é encontrar, clique em **instalar**.
   * `AzureIoTHub`
   * `AzureIoTUtility`
   * `AzureIoTProtocol_MQTT`
   * `ArduinoJson`
   * `DHT sensor library`
   * `Adafruit Unified Sensor`

### <a name="dont-have-a-real-dht22-sensor"></a>Não tem um sensor DHT22 real?

O aplicativo de exemplo pode simular dados de temperatura e humidade no caso de não ter um sensor DHT22 real. Para ativar a aplicação de exemplo para utilizar dados simulados, siga estes passos:

1. Abra o `config.h` de ficheiros a `app` pasta.
1. Localize a seguinte linha de código e altere o valor de `false` para `true`:
   ```c
   define SIMULATED_DATA true
   ```
   ![Configurar a aplicação de exemplo para utilizar dados simulados](media/iot-hub-sparkfun-thing-dev-get-started/13_arduino-ide-configure-app-use-simulated-data.png)
   
1. Poupe com `Control-s`.

### <a name="deploy-the-sample-application-to-sparkfun-esp8266-thing-dev"></a>Implementar a aplicação de exemplo para Sparkfun ESP8266 Thing Dev

1. No Arduino IDE, clique em **ferramenta** > **porta**e, em seguida, clique na porta de série para efeitos de coisa Sparkfun ESP8266
1. Clique em **esboço** > **carregar** para criar e implementar a aplicação de exemplo para a programação. do Sparkfun ESP8266 coisa

> [!Note]
> Se estiver a utilizar o macOS provavelmente poderia ver as seguintes mensagens durante o carregamento. `warning: espcomm_sync failed`,`error: espcomm_open failed`. Abra a janela de ternimal e concluir abaixo das ações para resolver este problema.
> ```bash
> cd /System/Library/Extensions/IOUSBFamily.kext/Contents/PlugIns
> sudo mv AppleUSBFTDI.kext AppleUSBFTDI.disabled
> sudo touch /System/Library/Extensions
> ```

### <a name="enter-your-credentials"></a>Introduza as suas credenciais

Depois do carregamento for concluída com êxito, siga os passos para introduzir as suas credenciais:

1. No Arduino IDE, clique em **ferramentas** > **Serial Monitor**.
1. Na janela serial monitor, tenha em atenção as duas listas suspensas no canto inferior direito.
1. Selecione **sem fim da linha** para a esquerda na lista pendente.
1. Selecione **bauds 115200** para a direita na lista pendente.
1. Na caixa de entrada na parte superior da janela serial monitor, introduza as seguintes informações se for solicitado a fornecê-los e, em seguida, clique em **enviar**.
   * SSID de Wi-Fi
   * Palavra-passe do Wi-Fi
   * Cadeia de ligação do dispositivo

> [!Note]
> As informações de credenciais são armazenadas no EEPROM de Sparkfun ESP8266 coisa programação. Se clicar no botão de reposição no quadro Sparkfun ESP8266 Thing Dev, o aplicativo de exemplo pede-lhe se pretende apagar as informações. Introduza `Y` ter as informações de apagar e é-lhe pedido para fornecer as informações novamente.

### <a name="verify-the-sample-application-is-running-successfully"></a>Certifique-se de que o aplicativo de exemplo está em execução com êxito

Se vir o seguinte resultado da janela de serial monitor e o LED piscando no Sparkfun ESP8266 Thing Dev, o aplicativo de exemplo está em execução com êxito.

![saída final no arduino ide](media/iot-hub-sparkfun-thing-dev-get-started/14_arduino-ide-final-output.png)

## <a name="next-steps"></a>Passos Seguintes

Com êxito ligado um Sparkfun ESP8266 Thing Dev ao seu hub IoT e enviar os dados de sensor capturada ao seu hub IoT. 

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
