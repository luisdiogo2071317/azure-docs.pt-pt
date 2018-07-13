---
title: M0 para a cloud – ligar Feather M0 Wi-Fi para o IoT Hub do Azure | Documentos da Microsoft
description: Saiba como configurar e ligar Adafruit Feather M0 Wi-Fi para o IoT Hub do Azure para enviar dados para a plataforma de cloud do Azure neste tutorial.
author: rangv
manager: nasing
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 04/11/2018
ms.author: rangv
ms.openlocfilehash: 2a6899bbd294a16dee3a767e976a92deaa00f0e2
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38676697"
---
# <a name="connect-adafruit-feather-m0-wifi-to-azure-iot-hub-in-the-cloud"></a>Ligar o Adafruit Feather M0 Wi-Fi para o IoT Hub do Azure na cloud
[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

![Ligação entre um BME280 Feather M0 Wi-Fi e IoT Hub](media/iot-hub-adafruit-feather-m0-wifi-get-started/1_connection-m0-feather-m0-iot-hub.png)

Neste tutorial, começará aprender as noções básicas de trabalhar com a sua área de Arduino. Em seguida, saiba como ligar forma totalmente integrada os seus dispositivos para a cloud, utilizando [IoT Hub do Azure](iot-hub-what-is-iot-hub.md).

## <a name="what-you-do"></a>O que fazer

Ligar o Adafruit Feather M0 Wi-Fi para um hub IoT que criou. Em seguida, execute um aplicativo de exemplo no M0 Wi-Fi para recolher os dados de temperatura e humidade a partir de um BME280. Por fim, que envia os dados de sensor ao seu hub IoT.


## <a name="what-you-learn"></a>O que irá aprender

* Como criar um hub IoT e registar um dispositivo para Feather M0 Wi-Fi
* Como ligar o Feather M0 Wi-Fi com o sensor e o seu computador
* Como recolher dados de sensor ao executar um aplicativo de exemplo no Feather M0 Wi-Fi
* Como enviar os dados de sensor ao seu hub IoT

## <a name="what-you-need"></a>Do que precisa

![Partes necessários para o tutorial](media/iot-hub-adafruit-feather-m0-wifi-get-started/2_parts-needed-for-the-tutorial.png)

Para concluir esta operação, tem as seguintes partes do seu Kit de iniciante do Feather M0 Wi-Fi:

* O quadro de Feather M0 Wi-Fi
* Uma USB Micro a cabo USB de um tipo

Também precisa do seguinte para o seu ambiente de desenvolvimento:

* Uma subscrição ativa do Azure. Se não tiver uma conta do Azure, [criar uma conta gratuita do Azure](https://azure.microsoft.com/free/) em apenas alguns minutos.
* Um Mac ou um PC com Windows ou Ubuntu.
* Uma rede sem fio Feather M0 Wi-Fi ligar a.
* Uma ligação à Internet para transferir a ferramenta de configuração.
* [Arduino IDE](https://www.arduino.cc/en/main/software) versão 1.6.8 ou posterior. Versões anteriores não funcionam com a biblioteca do IoT Hub do Azure.

Se não tiver um sensor, os seguintes itens são opcionais. Tem também a opção de utilizar dados de sensores simulados:

* Um sensor de temperatura e humidade BME280
* Um breadboard
* Cablagem de jumper M/M

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="connect-feather-m0-wifi-with-the-sensor-and-your-computer"></a>Ligar o Feather M0 Wi-Fi com o sensor e o seu computador
Nesta secção, vai ligar os sensores a seu quadro. Em seguida, conectar no seu dispositivo para o seu computador para uso posterior.

### <a name="connect-a-dht22-temperature-and-humidity-sensor-to-feather-m0-wifi"></a>Ligar um sensor de temperatura e humidade DHT22 a Feather M0 Wi-Fi

Utilize cabos, mas breadboard e jumper para fazer a conexão. Se não tiver um sensor, ignore esta secção porque pode usar os dados de sensores simulados em vez disso.

![Referência de ligações](media/iot-hub-adafruit-feather-m0-wifi-get-started/3_connections_on_breadboard.png)


Para pins de sensor, utilize a seguinte ligação:


| Iniciar (sensor)           | Fim (quadro)            | Cor do cabo   |
| -----------------------  | ---------------------- | ------------: |
| VDD (Pin 27A)            | 3 v (3A de Pin)            | Cabo vermelho     |
| GND (Pin 29A)            | GND (6A de Pin)           | Cabo preto   |
| SCK (Pin 30A)            | SCK (Pin 12A)          | Cabo amarelo  |
| SDO (Pin 31A)            | MI (Pin 14A)           | Cabo branco   |
| SDI (Pin 32A)            | M0 (Pin 13A)           | Cabo azul    |
| CS (Pin 33A)             | GPIO 5 (Pin 15J)       | Cor de laranja cabo  |

Para obter mais informações, consulte [Adafruit BME280 humidade + pressão Barometric + abertas do Sensor de temperatura](https://learn.adafruit.com/adafruit-bme280-humidity-barometric-pressure-temperature-sensor-breakout/wiring-and-test?view=all) e [Adafruit Feather M0 Wi-Fi pinouts](https://learn.adafruit.com/adafruit-feather-m0-wifi-atwinc1500/pinouts).



Agora o Feather M0 Wi-Fi devem estar ligado com um sensor de trabalho.

![Conecte DHT22 Feather Huzzah](media/iot-hub-adafruit-feather-m0-wifi-get-started/4_connect-bme280-feather-m0-wifi.png)

### <a name="connect-feather-m0-wifi-to-your-computer"></a>Ligar o Feather M0 Wi-Fi para o seu computador

Utilize a USB Micro a cabo USB de um tipo para ligar o Feather M0 Wi-Fi para o seu computador, conforme mostrado:

![Ligar o Feather Huzzah para o seu computador](media/iot-hub-adafruit-feather-m0-wifi-get-started/5_connect-feather-m0-wifi-computer.png)

### <a name="add-serial-port-permissions-ubuntu-only"></a>Adicionar permissões de porta serial (apenas Ubuntu)

Se usar o Ubuntu, certifique-se de que tem as permissões para operar no USB porta de Feather M0 Wi-Fi. Para adicionar permissões de porta serial, siga estes passos:


1. No terminal, execute os seguintes comandos:

   ```bash
   ls -l /dev/ttyUSB*
   ls -l /dev/ttyACM*
   ```

   Obtém uma os seguintes resultados:

   * crw-rw---1 raiz uucp xxxxxxxx
   * crw-rw---1 raiz dialout xxxxxxxx

   Na saída, tenha em atenção que `uucp` ou `dialout` é o nome do grupo proprietário da porta USB.

2. Para adicionar o utilizador ao grupo, execute o seguinte comando:

   ```bash
   sudo usermod -a -G <group-owner-name> <username>
   ```

   No passo anterior, que obteve o nome de proprietário do grupo `<group-owner-name>`. É o nome de utilizador do Ubuntu `<username>`.

3. Para que a alteração apareça, termine a sessão Ubuntu e, em seguida, inicie sessão novamente.

## <a name="collect-sensor-data-and-send-it-to-your-iot-hub"></a>Recolher dados de sensor e enviá-lo ao seu hub IoT

Nesta secção, implementar e executar uma aplicação de exemplo no Feather M0 Wi-Fi. O aplicativo de exemplo faz piscar LED no Feather M0 Wi-Fi. Em seguida, envia os dados de temperatura e humidade recolhidos a partir do sensor BME280 ao seu hub IoT.

### <a name="get-the-sample-application-from-github-and-prepare-the-arduino-ide"></a>Obter a aplicação de exemplo a partir do GitHub e preparar o Arduino IDE

O aplicativo de exemplo está alojado no GitHub. Clone o repositório de exemplo que contém a aplicação de exemplo do GitHub. Para clonar o repositório de exemplo, siga estes passos:

1. Abra um prompt de comando ou uma janela de terminal.

2. Ir para uma pasta onde pretende que o aplicativo de exemplo para serem armazenados.
3. Execute o seguinte comando:

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-Feather-M0-WiFi-client-app.git
   ```

### <a name="install-the-package-for-feather-m0-wifi-in-the-arduino-ide"></a>Instalar o pacote para Feather M0 Wi-Fi no Arduino IDE

1. Abra a pasta onde o aplicativo de exemplo é armazenado.

2. Abra o ficheiro de app.ino na pasta de aplicação no Arduino IDE.

   ![Abra a aplicação de exemplo no Arduino IDE](media/iot-hub-adafruit-feather-m0-wifi-get-started/6_arduino-ide-open-sample-app.png)


1. Clique em **arquivo** > **preferências** (Windows/Linux) ou **Arduino** > **preferências** (Mac) e copie e Cole a ligação abaixo para o **URLs de Gestor de quadros adicionais** opção nas preferências Arduino IDE.
   
   ```
   https://adafruit.github.io/arduino-board-index/package_adafruit_index.json
   ```

1. Clique em **ferramentas** > **quadro** > **quadros Manager**e, em seguida, instale o `Arduino SAMD Boards` versão `1.6.2` ou posterior. 

1. Em seguida, na janela da mesma, instale `Adafruit SAMD Boards` pacote para adicionar as definições de ficheiro do quadro.

   ![O pacote de esp8266 está instalado](media/iot-hub-adafruit-feather-m0-wifi-get-started/7_arduino-ide-package-url.png)

4. Clique em **ferramentas** > **quadro** > **Adafruit M0 Wi-Fi**.

5. Instale controladores (para Windows apenas). Quando conectar Feather M0 Wi-Fi, poderá ter de instalar um driver. Clique em [a ligação de transferência na página Web](https://github.com/adafruit/Adafruit_Windows_Drivers/releases/download/1.1/adafruit_drivers.exe) para transferir o instalador de driver. Siga os passos para instalar os drivers que desejar.

### <a name="install-necessary-libraries"></a>Instalar as bibliotecas necessárias

1. No Arduino IDE, clique em **esboço** > **biblioteca incluem** > **gerir bibliotecas**.

2. Pesquisa para a biblioteca seguinte nomes individualmente. Para cada biblioteca que encontrar, clique em **instalar**:

   * `RTCZero`
   * `NTPClient`
   * `AzureIoTHub`
   * `AzureIoTUtility`
   * `AzureIoTProtocol_HTTP`
   * `ArduinoJson`
   * `Adafruit BME280 Library`
   * `Adafruit Unified Sensor`

3. Instalar manualmente `Adafruit_WINC1500`. Aceda a [este Web site](https://github.com/adafruit/Adafruit_WINC1500) e clique em **clonar ou transferir** > **transferir ZIP**. Em seguida, no seu IDE Arduino, aceda a **esboço** > **biblioteca incluem** > **adicionar. zip biblioteca** e adicione o ficheiro zip.

### <a name="use-the-sample-application-if-you-dont-have-a-real-bme280-sensor"></a>Utilizar a aplicação de exemplo, se não tiver um sensor BME280 real

Se não tiver um sensor BME280 real, a aplicação de exemplo pode simular dados de temperatura e humidade. Para configurar a aplicação de exemplo para utilizar dados simulados, siga estes passos:

1. Abra o `config.h` de ficheiros a `app` pasta.

2. Localize a seguinte linha de código e altere o valor de `false` para `true`:

   ```c
   define SIMULATED_DATA true
   ```
   ![Configurar a aplicação de exemplo para utilizar dados simulados](media/iot-hub-adafruit-feather-m0-wifi-get-started/8_arduino-ide-configure-app-use-simulated-data.png)

3. Guarde o ficheiro com `Control-s`.

### <a name="deploy-the-sample-application-to-feather-m0-wifi"></a>Implementar a aplicação de exemplo para Feather M0 Wi-Fi

1. No Arduino IDE, clique em **ferramenta** > **porta**e, em seguida, clique na porta de série para Feather M0 Wi-Fi.

2. Clique em **esboço** > **carregar** para criar e implementar a aplicação de exemplo para Feather M0 Wi-Fi.

### <a name="enter-your-credentials"></a>Introduza as suas credenciais

Depois do carregamento for concluída com êxito, siga estes passos para introduzir as suas credenciais:

1. No Arduino IDE, clique em **ferramentas** > **Serial Monitor**.

2. No canto inferior direito da janela serial monitor, selecione **sem fim da linha** na lista suspensa à esquerda.
3. Selecione **bauds 115200** na lista pendente à direita.
4. Na caixa de entrada na parte superior, introduza as seguintes informações se lhe for pedido para fornecê-lo e clique em **enviar**:

   * SSID de Wi-Fi
   * Palavra-passe do Wi-Fi
   * Cadeia de ligação do dispositivo

> [!Note]
> As informações de credenciais são armazenadas no EEPROM de Feather M0 Wi-Fi. Se clicar no botão de reposição no quadro Feather M0 Wi-Fi, o aplicativo de exemplo pergunta se deseja apagar as informações. Introduza `Y` apagar as informações. É solicitado a fornecer as informações de uma segunda vez.

### <a name="verify-that-the-sample-application-is-running-successfully"></a>Certifique-se de que o aplicativo de exemplo está em execução com êxito

Se vir o seguinte resultado da janela de serial monitor e o LED piscando no Feather M0 Wi-Fi, o aplicativo de exemplo está em execução com êxito:

![Saída final no Arduino IDE](media/iot-hub-adafruit-feather-m0-wifi-get-started/9_arduino-ide-final-output.png)

## <a name="next-steps"></a>Passos Seguintes

Com êxito ligado Feather M0 Wi-Fi para o seu hub IoT e enviar os dados de sensor capturada ao seu hub IoT. 

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

