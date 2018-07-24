---
title: Intel Edison para cloud (node. js) - ligar o Intel Edison hub IoT do Azure | Documentos da Microsoft
description: Saiba como configurar e ligar Intel Edison hub do IoT do Azure para Intel Edison enviar dados para a plataforma de cloud do Azure neste tutorial.
author: rangv
manager: ''
keywords: o Azure iot intel edison, intel hub de iot edison, intel edison enviar dados para a cloud, intel edison com a nuvem
ms.service: iot-hub
services: iot-hub
ms.devlang: nodejs
ms.topic: conceptual
ms.date: 04/11/2018
ms.author: rangv
ms.openlocfilehash: 5adb6e6119856879e52836a8d3a797a7b40c6fe8
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/23/2018
ms.locfileid: "39214435"
---
# <a name="connect-intel-edison-to-azure-iot-hub-nodejs"></a>Ligar a Intel Edison ao Hub IoT do Azure (node. js)

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

Neste tutorial, começará aprender as noções básicas do trabalho com o Intel Edison. Em seguida, saiba como ligar forma totalmente integrada os seus dispositivos para a cloud, utilizando [IoT Hub do Azure](about-iot-hub.md).

Não tem ainda um kit? Iniciar [aqui](https://azure.microsoft.com/develop/iot/starter-kits)

## <a name="what-you-do"></a>O que fazer

* Intel Edison e Grove módulos de configuração.
* Crie um hub IoT.
* Inscreva-se um dispositivo Edison do seu hub IoT.
* Execute um aplicativo de exemplo no Edison para enviar dados de sensor ao seu hub IoT.

Ligue-se Intel Edison para um hub IoT que criou. Em seguida, executar um aplicativo de exemplo no Edison para recolher dados de temperatura e humidade de um sensor de temperatura Grove. Por fim, que envia os dados de sensor ao seu hub IoT.

## <a name="what-you-learn"></a>O que irá aprender

* Como criar um hub IoT do Azure e obter a cadeia de ligação do novo dispositivo.
* Como ligar Edison com um sensor de temperatura Grove.
* Como recolher dados de sensor ao executar um aplicativo de exemplo no Edison.
* Como enviar dados de sensor ao seu hub IoT.

## <a name="what-you-need"></a>Do que precisa

![Do que precisa](media/iot-hub-intel-edison-kit-node-get-started/0_kit.png)

* O quadro de Intel Edison
* Área de expansão de Arduino
* Uma subscrição ativa do Azure. Se não tiver uma conta do Azure, [criar uma conta gratuita do Azure](https://azure.microsoft.com/free/) em apenas alguns minutos.
* Um Mac ou um PC que está a executar o Windows ou Linux.
* Uma ligação à Internet.
* Um B Micro a cabo USB de um tipo
* Uma fonte de energia atual de direct (DC). Sua fonte de alimentação deve ser classificado da seguinte forma:
  - 7-15 DC
  - Pelo menos 1500mA
  - O pin center/interno deve ser o Polo positivo da fonte de alimentação

Os seguintes itens são opcionais:

* Grove escudo Base V2
* Grove - Sensor de temperatura
* Cabo Grove
* Qualquer barras spacer ou screws incluídos no empacotamento, incluindo dois screws para aperte o módulo do quadro de expansão e quatro conjuntos de screws e spacers plástica.

> [!NOTE] 
Estes itens são opcionais, uma vez que o suporte de exemplo de código simulated dados de sensor.

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="setup-intel-edison"></a>Configurar o Intel Edison

### <a name="assemble-your-board"></a>Montar seu quadro

Esta secção contém passos para ligar o seu módulo Intel® Edison à sua área de expansão.

1. Coloca o módulo de Intel® Edison dentro do contorno branco no seu painel de expansão, reunindo os buracos no módulo com os screws no bloco de expansão.

2. Prima a tecla no módulo imediatamente abaixo as palavras `What will you make?` até se sentir muito simples.

   ![montar o quadro 2](media/iot-hub-intel-edison-kit-node-get-started/1_assemble_board2.jpg)

3. Utilize os dois nuts hexadecimais (incluídos no pacote) para proteger o módulo para o quadro de expansão.

   ![montar o quadro 3](media/iot-hub-intel-edison-kit-node-get-started/2_assemble_board3.jpg)

4. Inserir um screw em um dos buracos quatro canto no bloco de expansão. Rodar e reforçar a um dos spacers plástico em brancas para o screw.

   ![montar o quadro 4](media/iot-hub-intel-edison-kit-node-get-started/3_assemble_board4.jpg)

5. Repita para outros spacers de três canto.

   ![montar o quadro 5](media/iot-hub-intel-edison-kit-node-get-started/4_assemble_board5.jpg)

Agora seu quadro é reunido.

   ![quadro montado](media/iot-hub-intel-edison-kit-node-get-started/5_assembled_board.jpg)

### <a name="connect-the-grove-base-shield-and-the-temperature-sensor"></a>Ligar o escudo da Base de Grove e o sensor de temperatura

1. Coloque o escudo da Base de Grove logon em seu quadro. Certifique-se de que todos os pins são fortemente ligados seu quadro.
   
   ![Escudo Grove de Base](media/iot-hub-intel-edison-kit-node-get-started/6_grove_base_sheild.jpg)

2. Utilize o cabo de Grove para ligar o sensor de temperatura Grove para o escudo da Base de Grove **A0** porta.

   ![Ligar ao sensor de temperatura](media/iot-hub-intel-edison-kit-node-get-started/7_temperature_sensor.jpg)

   ![Ligação EDISON e sensor](media/iot-hub-intel-edison-kit-node-get-started/16_edion_sensor.png)

Agora, o sensor está pronta.

### <a name="power-up-edison"></a>Dê um Edison

1. Plug-in a fonte de alimentação.

   ![Plug-in de alimentação](media/iot-hub-intel-edison-kit-node-get-started/8_plug_power.jpg)

2. Um LED verde (rotulado como DS1 no quadro de expansão Arduino *) deve iluminam e mantenha-se iluminado.

3. Aguarde um minuto para que o quadro a finalização da inicialização cópia de segurança.

   > [!NOTE]
   > Se não tiver uma fonte de energia do controlador de domínio, ainda será possível ligar o quadro por meio de uma porta USB. Consulte `Connect Edison to your computer` secção para obter detalhes. Capacitar seu quadro dessa forma poderão resultar num comportamento imprevisível da sua parte do Conselho, especialmente quando através de Wi-Fi ou a originar motores.

### <a name="connect-edison-to-your-computer"></a>Ligar Edison para o seu computador

1. Ativar/desativar pendente microswitch para as duas portas USB micro, para que Edison está no modo de dispositivo. Para as diferenças entre o modo do dispositivo e o modo de anfitrião, consulte [aqui](https://software.intel.com/en-us/node/628233#usb-device-mode-vs-usb-host-mode).

   ![Alternar para baixo a microswitch](media/iot-hub-intel-edison-kit-node-get-started/9_toggle_down_microswitch.jpg)

2. Conecte o cabo USB micro porta USB micro superior.

   ![Porta USB micro superior](media/iot-hub-intel-edison-kit-node-get-started/10_top_usbport.jpg)

3. Conecte o outro extremo do cabo USB seu computador.

   ![Computador USB](media/iot-hub-intel-edison-kit-node-get-started/11_computer_usb.jpg)

4. Saberá que o seu quadro totalmente é inicializado quando o computador monta uma nova unidade (muito semelhante ao inserir um cartão SD no seu computador).

## <a name="download-and-run-the-configuration-tool"></a>Transfira e execute a ferramenta de configuração
Obter a ferramenta de configuração mais recente do [esta ligação](https://software.intel.com/en-us/iot/hardware/edison/downloads) listado sob o `Installers` cabeçalho. Executar a ferramenta e siga na tela instruções, clicar em seguinte quando necessário

### <a name="flash-firmware"></a>Firmware de Flash
1. Sobre o `Set up options` página, clique em `Flash Firmware`.
2. Selecione a imagem do Flash em seu quadro efetuando um dos seguintes procedimentos:
   - Para transferir e flash seu quadro com a imagem de firmware mais recente disponível da Intel, selecione `Download the latest image version xxxx`.
   - Flash seu quadro com uma imagem já tiver guardado no seu computador, selecione `Select the local image`. Procure e selecione a imagem que pretende Flash para seu quadro.
3. A ferramenta de configuração irá tentar flash seu quadro. Todo o processo piscantes pode demorar até 10 minutos.

### <a name="set-password"></a>Definir palavra-passe
1. Sobre o `Set up options` página, clique em `Enable Security`.
2. Pode definir um nome personalizado para seu quadro Intel® Edison. Isto é opcional.
3. Escreva uma palavra-passe para o quadro, em seguida, clique em `Set password`.
4. Marca para baixo a palavra-passe, que é utilizada mais tarde.

### <a name="connect-wi-fi"></a>Estabelecer a ligação Wi-Fi
1. Sobre o `Set up options` página, clique em `Connect Wi-Fi`. Aguarde até um minuto, como o seu computador verifica a existência de redes Wi-Fi disponíveis.
2. Do `Detected Networks` pendente, selecione a sua rede.
3. Do `Security` pendente, selecione o tipo de segurança da rede.
4. Forneça as suas informações de início de sessão e palavra-passe, em seguida, clique em `Configure Wi-Fi`.
5. Marca para baixo o endereço IP, o que é utilizado mais tarde.

> [!NOTE]
> Certifique-se de que Edison está ligado à mesma rede que seu computador. O computador liga-se ao seu Edison através do endereço IP.

   ![Ligar ao sensor de temperatura](media/iot-hub-intel-edison-kit-node-get-started/12_configuration_tool.png)

Parabéns! Configurou Edison com êxito.

## <a name="run-a-sample-application-on-intel-edison"></a>Executar uma aplicação de exemplo no Intel Edison

### <a name="prepare-the-azure-iot-device-sdk"></a>Preparar o Azure IoT Device SDK

1. Utilize um dos seguintes clientes SSH do computador anfitrião para ligar ao seu Intel Edison. O endereço IP é a partir da ferramenta de configuração e a palavra-passe é aquela que tiver configurado essa ferramenta.
    - [PuTTY](http://www.putty.org/) para Windows.
    - O cliente SSH incorporado no Ubuntu ou macOS.

2. Clone a aplicação de cliente de exemplo para o seu dispositivo. 
   
   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-node-intel-edison-client-app
   ```

3. Em seguida, navegue para a pasta de repositório para executar o seguinte comando para instalar todos os pacotes, poderá demorar serval minutos a concluir.
   
   ```bash
   cd iot-hub-node-intel-edison-client-app
   npm install
   ```


### <a name="configure-and-run-the-sample-application"></a>Configurar e executar o aplicativo de exemplo

1. Abra o ficheiro de configuração ao executar os comandos seguintes:

   ```bash
   nano config.json
   ```

   ![Ficheiro de configuração](media/iot-hub-intel-edison-kit-node-get-started/13_configure_file.png)

   Existem dois macros neste ficheiro, pode configurate. A primeira é `INTERVAL`, que define o intervalo de tempo entre duas mensagens que enviar para a cloud. Segunda `simulatedData`, que é um valor booleano para indicar se a utilizar dados de sensores simulados ou não.

   Se **não tem o sensor**, defina o `simulatedData` valor a `true` para tornar o aplicativo de exemplo, criar e utilizar dados de sensores simulados.

1. Guardar e sair pressionando controle-O > Enter > controlo-X.


1. Execute a aplicação de exemplo ao executar o seguinte comando:

   ```bash
   sudo node index.js '<your Azure IoT hub device connection string>'
   ```

   > [!NOTE] 
   Certifique-se copiar-colar a cadeia de ligação do dispositivo para as aspas simples.

Deverá ver o resultado seguinte que mostra os dados de sensor e as mensagens que são enviadas ao seu hub IoT.

![Saída - dados de sensor enviados do Intel Edison ao seu hub IoT](media/iot-hub-intel-edison-kit-node-get-started/15_message_sent.png)

## <a name="next-steps"></a>Passos Seguintes

Executar uma aplicação de exemplo para recolher dados de sensor e enviá-lo ao seu hub IoT.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
