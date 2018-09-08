---
title: Raspberry Pi a nuvem (Python) - ligar o Raspberry Pi hub IoT do Azure | Documentos da Microsoft
description: Saiba como configurar e ligar o Raspberry Pi a IoT Hub do Azure para o Raspberry Pi enviar dados para a plataforma de cloud do Azure neste tutorial.
author: rangv
manager: ''
keywords: o Azure iot raspberry pi, hub de iot raspberry pi, enviar dados para a cloud de raspberry pi, raspberry pi com a nuvem
ms.service: iot-hub
services: iot-hub
ms.devlang: python
ms.topic: conceptual
ms.date: 04/11/2018
ms.author: rangv
ms.openlocfilehash: 6e668394bcb647df901dfac72c552475e56f20bf
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/07/2018
ms.locfileid: "44161106"
---
# <a name="connect-raspberry-pi-to-azure-iot-hub-python"></a>Ligar o Raspberry Pi ao Hub IoT do Azure (Python)

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

Neste tutorial, começará aprender as noções básicas do trabalho com o Raspberry Pi com Raspbian. Em seguida, saiba como ligar forma totalmente integrada os seus dispositivos para a cloud, utilizando [IoT Hub do Azure](about-iot-hub.md). Para exemplos do Windows 10 IoT Core, vá para o [Windows Dev Center](http://www.windowsondevices.com/).

Não tem ainda um kit? Tente [simulador online de Raspberry Pi](iot-hub-raspberry-pi-web-simulator-get-started.md). Ou comprar um novo kit [aqui](https://azure.microsoft.com/develop/iot/starter-kits).

## <a name="what-you-do"></a>O que fazer

* Crie um hub IoT.
* Registe um dispositivo para o instalador de plataforma do seu hub IoT.
* Configure o Raspberry Pi.
* Execute uma aplicação de exemplo no instalador de plataforma para enviar dados de sensor ao seu hub IoT.

Ligar o Raspberry Pi para um hub IoT que criou. Em seguida, executar um aplicativo de exemplo no Pi para recolher dados de temperatura e humidade de um sensor BME280. Por fim, que envia os dados de sensor ao seu hub IoT.

## <a name="what-you-learn"></a>O que irá aprender

* Como criar um hub IoT do Azure e obter a cadeia de ligação do novo dispositivo.
* Como ligar o Pi com um sensor BME280.
* Como recolher dados de sensor ao executar um aplicativo de exemplo no Pi.
* Como enviar dados de sensor ao seu hub IoT.

## <a name="what-you-need"></a>Do que precisa

![Do que precisa](media/iot-hub-raspberry-pi-kit-c-get-started/0_starter_kit.jpg)

* O quadro Raspberry Pi 2 ou Raspberry Pi 3.
* Uma subscrição ativa do Azure. Se não tiver uma conta do Azure, [criar uma conta gratuita do Azure](https://azure.microsoft.com/free/) em apenas alguns minutos.
* Um monitor, um USB teclado e mouse que se ligam a Pi.
* Um Mac ou um PC que está a executar o Windows ou Linux.
* Uma ligação à Internet.
* Um 16 GB ou superior do cartão microSD.
* Um USB-adaptador ou microSD cartão SD para gravar a imagem de sistema operativo para o cartão microSD.
* Forneça uma potência de 2 e 5 volt com 6 pés micro um cabo USB.

Os seguintes itens são opcionais:

* Um montado Adafruit BME280 temperatura e humidade pressão sensor.
* Um breadboard.
* Cablagem de jumper 6 F/M.
* Um diffused LED de 10 mm.


> [!NOTE] 
Estes itens são opcionais, uma vez que o suporte de exemplo de código simulated dados de sensor.


[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="set-up-raspberry-pi"></a>Configurar o Raspberry Pi

### <a name="install-the-raspbian-operating-system-for-pi"></a>Instalar o sistema de operativo Raspbian para Pi

Prepare o cartão microSD para a instalação da imagem Raspbian.

1. Baixe Raspbian.
   1. [Transferir Raspbian Jessie com o Desktop](https://www.raspberrypi.org/downloads/raspbian/) (o ficheiro. zip).
   1. Extraia a imagem de Raspbian para uma pasta no seu computador.
1. Instale Raspbian ao cartão microSD.
   1. [Transferir e instalar o utilitário de gravador de cartão Etcher SD](https://etcher.io/).
   1. Execute Etcher e selecione a imagem de Raspbian extraído no passo 1.
   1. Selecione a unidade de cartão microSD. Tenha em atenção que Etcher poderá ter já selecionado da unidade correta.
   1. Clique em Flash para instalar Raspbian ao cartão microSD.
   1. Remova o cartão microSD do computador quando a instalação estiver concluída. É seguro remover o cartão microSD diretamente, pois Etcher automaticamente ejeta ou desmonta o cartão microSD após a conclusão.
   1. Inserir o cartão microSD Pi.

### <a name="enable-ssh-and-i2c"></a>Ativar o SSH e I2C

1. Ligar o instalador de plataforma para o monitor, teclado e mouse, iniciar o instalador de plataforma e, em seguida, inicie sessão no Raspbian utilizando `pi` como o nome de utilizador e `raspberry` como a palavra-passe.
1. Clique no ícone de Raspberry > **preferências** > **Raspberry Pi configuração**.

   ![O menu de preferências de Raspbian](media/iot-hub-raspberry-pi-kit-c-get-started/1_raspbian-preferences-menu.png)

1. Sobre o **Interfaces** separador, defina **I2C** e **SSH** para **ativar**e, em seguida, clique em **OK**. Se não tiver físicos sensores e utilizar dados de sensores simulados, este passo é opcional.

   ![Ativar I2C e SSH no Raspberry Pi](media/iot-hub-raspberry-pi-kit-c-get-started/2_enable-spi-ssh-on-raspberry-pi.png)

> [!NOTE] 
Para ativar o SSH e I2C, pode encontrar mais documentos de referência sobre [raspberrypi.org](https://www.raspberrypi.org/documentation/remote-access/ssh/) e [RASPI-CONFIG](https://www.raspberrypi.org/documentation/configuration/raspi-config.md).

### <a name="connect-the-sensor-to-pi"></a>Ligar o sensor ao instalador de plataforma

Utilize cabos, mas breadboard e jumper para ligar um LED e um BME280 para Pi da seguinte forma. Se não tiver o sensor [ignorar esta secção](#connect-pi-to-the-network).

![A ligação Raspberry Pi e o sensor](media/iot-hub-raspberry-pi-kit-node-get-started/3_raspberry-pi-sensor-connection.png)

O sensor BME280 pode recolher dados de temperatura e humidade. E o LED será blink se houver uma comunicação entre o dispositivo e a cloud. 

Para pins de sensor, utilize a seguinte ligação:

| Iniciar (Sensor & LED)     | Fim (quadro)            | Cor do cabo   |
| -----------------------  | ---------------------- | ------------: |
| VDD (Pin 5g)             | 3.3V PWR (Pin 1)       | Cabo branco   |
| GND (Pin 7G)             | GND (Pin 6)            | Cabo castanha   |
| SDI (Pin 10G)            | I2C1 SDA (Pin 3)       | Cabo vermelho     |
| SCK (Pin 8g)             | I2C1 SCL (Pin 5)       | Cor de laranja cabo  |
| LED VDD (Pin 18F)        | GPIO 24 (Pin 18)       | Cabo branco   |
| LED GND (Pin 17F)        | GND (Pin 20)           | Cabo preto   |

Clique para ver [Raspberry Pi 2 e 3 mapeamentos de Pin](https://developer.microsoft.com/windows/iot/docs/pinmappingsrpi) para sua referência.

Depois de se ligar com êxito BME280 para o seu Raspberry Pi, deve ser, como abaixo da imagem.

![Instalador de plataforma ligado e BME280](media/iot-hub-raspberry-pi-kit-node-get-started/4_connected-pi.jpg)

### <a name="connect-pi-to-the-network"></a>Ligar o Pi à rede

Ative o Pi usando o cabo USB do micro e a fonte de alimentação. Utilize o cabo Ethernet para ligar o instalador de plataforma à sua rede com fio ou seguir a [instruções da base do Raspberry Pi](https://www.raspberrypi.org/learning/software-guide/wifi/) para ligar o instalador de plataforma à sua rede sem fio. Depois de seu Pi foi ligado com êxito à rede, terá de tomar nota dos [endereço IP do seu Pi](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-3-network-setup/finding-your-pis-ip-address).

![Ligado à rede com fio](media/iot-hub-raspberry-pi-kit-node-get-started/5_power-on-pi.jpg)

> [!NOTE]
> Certifique-se de que o instalador de plataforma está ligado à mesma rede que seu computador. Por exemplo, se o computador está ligado a uma rede sem fio, enquanto o instalador de plataforma está ligado a uma rede com fio, poderá não ver o endereço IP na saída devdisco.

## <a name="run-a-sample-application-on-pi"></a>Executar uma aplicação de exemplo no Pi

### <a name="install-the-prerequisite-packages"></a>Instalar os pacotes de pré-requisitos

Utilize um dos seguintes clientes SSH do computador anfitrião para ligar ao seu Raspberry Pi.
   
   **Utilizadores do Windows**
   1. Transfira e instale [PuTTY](http://www.putty.org/) para Windows. 
   1. Copie o endereço IP da seção de instalador de plataforma para o nome de anfitrião (ou endereço IP) e selecione SSH como o tipo de ligação.
   
   
   **Mac e os utilizadores do Ubuntu**
   
   Utilize o cliente SSH incorporado no Ubuntu ou macOS. Poderá ter de executar `ssh pi@<ip address of pi>` para ligar o instalador de plataforma através de SSH.
   > [!NOTE] 
   O nome de utilizador predefinido é `pi` , e a palavra-passe é `raspberry`.


### <a name="configure-the-sample-application"></a>Configurar a aplicação de exemplo

1. Clone a aplicação de exemplo ao executar o seguinte comando:

   ```bash
   cd ~
   git clone https://github.com/Azure-Samples/iot-hub-python-raspberrypi-client-app.git
   ```
1. Abra o ficheiro de configuração ao executar os comandos seguintes:

   ```bash
   cd iot-hub-python-raspberrypi-client-app
   nano config.py
   ```

   Existem 5 macros neste ficheiro, pode configurate. A primeira é `MESSAGE_TIMESPAN`, que define o intervalo de tempo (em milissegundos) entre duas mensagens que enviar para a cloud. Segunda `SIMULATED_DATA`, que é um valor booleano para indicar se a utilizar dados de sensores simulados ou não. `I2C_ADDRESS` é o endereço de I2C que o sensor BME280 está ligado. `GPIO_PIN_ADDRESS` o endereço GPIO destina-se a sua LED. A última é `BLINK_TIMESPAN`, que o período de tempo é definido quando o LED está ativado em milissegundos.

   Se **não tem o sensor**, defina o `SIMULATED_DATA` valor a `True` para tornar o aplicativo de exemplo, criar e utilizar dados de sensores simulados.

1. Guardar e sair pressionando controle-O > Enter > controlo-X.

### <a name="build-and-run-the-sample-application"></a>Criar e executar o aplicativo de exemplo

1. Crie a aplicação de exemplo ao executar o comando seguinte. Como os SDKs IoT do Azure para Python são invólucros sobre o SDK de C de dispositivos de IoT do Azure, será necessário compilar as bibliotecas de C se quer ou precisa de gerar as bibliotecas Python a partir do código-fonte.

   ```bash
   sudo chmod u+x setup.sh
   sudo ./setup.sh
   ```
   > [!NOTE] 
   Também pode especificar a versão que pretende ao executar `sudo ./setup.sh [--python-version|-p] [2.7|3.4|3.5]`. Se executar o script sem parâmetro, o script irá detetar automaticamente a versão do python instaladas (sequência de pesquisa 2.7 -> 3.4 -> 3.5). Certifique-se de que a sua versão de Python mantém consistente durante a criação e execução. 
   
   > [!NOTE] 
   Sobre como criar a biblioteca de cliente de Python (iothub_client.so) em dispositivos de Linux que têm menos de 1GB de RAM, poderá ver criar presos no 98% durante a criação iothub_client_python.cpp, conforme mostrado abaixo `[ 98%] Building CXX object python/src/CMakeFiles/iothub_client_python.dir/iothub_client_python.cpp.o`. Caso se depare com este problema, verifique o consumo de memória do dispositivo `free -m command` na outra janela de terminal durante esse período. Se estiver a ficar sem memória ao compilar o arquivo de iothub_client_python.cpp, poderá ter de aumentar temporariamente o espaço de comutação para obter mais memória disponível para compilar com êxito o dispositivo do lado do cliente Python biblioteca do SDK.
   
1. Execute a aplicação de exemplo ao executar o seguinte comando:

   ```bash
   python app.py '<your Azure IoT hub device connection string>'
   ```

   > [!NOTE] 
   Certifique-se copiar-colar a cadeia de ligação do dispositivo para as aspas simples. E se usar o python 3, então pode usar o comando `python3 app.py '<your Azure IoT hub device connection string>'`.


   Deverá ver o resultado seguinte que mostra os dados de sensor e as mensagens que são enviadas ao seu hub IoT.

   ![Saída - dados de sensor enviados do Raspberry Pi ao seu hub IoT](media/iot-hub-raspberry-pi-kit-c-get-started/success.png
)

## <a name="next-steps"></a>Passos Seguintes

Executar uma aplicação de exemplo para recolher dados de sensor e enviá-lo ao seu hub IoT. Para ver as mensagens que seu Raspberry Pi enviado para o seu IoT hub ou enviar mensagens para o seu Raspberry Pi, consulte a [extensão de utilizar o IoT do Azure Toolkit para Visual Studio Code para enviar e receber mensagens entre o seu dispositivo e o IoT Hub](iot-hub-vscode-iot-toolkit-cloud-device-messaging.md).

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
