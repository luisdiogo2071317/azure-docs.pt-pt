---
title: IoT DevKit com a nuvem – ligar IoT DevKit AZ3166 ao IoT Hub do Azure | Documentos da Microsoft
description: Neste tutorial, saiba como configurar e ligar IoT DevKit AZ3166 hub IoT do Azure, pelo que pode enviar dados para a plataforma de cloud do Azure.
author: rangv
manager: jeffya
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 04/11/2018
ms.author: rangv
ms.openlocfilehash: cf9ee5339c53eb4f9c74f6b5f251a7963555d676
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/10/2018
ms.locfileid: "37928754"
---
# <a name="connect-iot-devkit-az3166-to-azure-iot-hub-in-the-cloud"></a>Ligar a IoT DevKit AZ3166 ao IoT Hub do Azure na cloud

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

Pode utilizar o [MXChip IoT DevKit](https://microsoft.github.io/azure-iot-developer-kit/) para desenvolver e soluções de Internet das coisas (IoT) de protótipo que tiram partido dos serviços do Microsoft Azure. Ele inclui um quadro compatível com Arduino com periféricos avançados e sensores, um pacote de quadro de código-fonte aberto e crescente [catálogo de projetos](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/).

## <a name="what-you-do"></a>O que fazer
Ligar o [DevKit](https://microsoft.github.io/azure-iot-developer-kit/) para um hub IoT do Azure que criar, coletar os dados de temperatura e humidade a partir de sensores e enviar os dados para o hub IoT.

Não tem um DevKit ainda? Tente [DevKit simulador](https://azure-samples.github.io/iot-devkit-web-simulator/) ou [obter uma](https://aka.ms/iot-devkit-purchase).

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

Conectar o hardware para o seu computador.

Precisa esse hardware:

* DevKit quadro
* Cabo micro USB

![Hardware necessário](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/hardware.jpg)

Para ligar o DevKit ao seu computador:

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
> Desde a versão 1.1, DevKit permite ST-SAFE no carregador de inicialização. Tem de atualizar o firmware se estiver a executar em v1.1 para fazê-lo funcionar provavelmente.

Se precisar de uma atualização de firmware, o ecrã mostrará as versões de firmware atual e mais recentes. Para atualizar, siga os [atualizar o firmware](https://microsoft.github.io/azure-iot-developer-kit/docs/firmware-upgrading/) guia.

![Exibição de versões de firmware atual e mais recentes](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/firmware.jpg)

> [!NOTE] 
> Este é um esforço de uso individual. Depois que comece a desenvolver o DevKit e carregar seu aplicativo, o firmware mais recente virão com a sua aplicação.

### <a name="test-various-sensors"></a>Testar vários sensores

Prima o botão B para testar sensores. Continue o pressionamento e a liberação do botão B para percorrer cada sensor.

![Apresentação B do botão e o sensor](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/sensors.jpg)

## <a name="prepare-the-development-environment"></a>Preparar o ambiente de desenvolvimento

Agora está na altura de configurar o ambiente de desenvolvimento: ferramentas e pacotes para a criação de aplicativos de IoT impressionantes. Pode escolher a versão do macOS ou Windows, de acordo com seu sistema operativo.

### <a name="windows"></a>Windows

Recomendamos que utilize o pacote de instalação para preparar o ambiente de desenvolvimento. Se tiver quaisquer problemas, pode seguir a [passos manuais](https://microsoft.github.io/azure-iot-developer-kit/docs/installation/) de realizar.

#### <a name="download-the-latest-package"></a>Transferir o pacote mais recente

O ficheiro. zip que transferiu contém todas as ferramentas necessárias e pacotes para o desenvolvimento de DevKit.

> [!div class="button"]
[Transferência](https://aka.ms/devkit/prod/installpackage/latest)

O ficheiro. zip contém as seguintes ferramentas e pacotes. Se já tiver alguns componentes instalados, o script irá detetar e ignorá-los.

* NODE. js e o Yarn: tempo de execução para o script de configuração e tarefas automatizadas.
* [Azure CLI 2.0 MSI](https://docs.microsoft.com//cli/azure/install-azure-cli#windows): experiência de linha de comandos para várias plataformas para o gerenciamento de recursos do Azure. O MSI contém dependentes Python e pip.
* [Visual Studio Code](https://code.visualstudio.com/) (VS Code): o editor de código simples para o desenvolvimento de DevKit.
* [Extensão do Visual Studio Code para Arduino](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.vscode-arduino): extensão que permite o desenvolvimento de Arduino no Visual Studio Code.
* [Arduino IDE](https://www.arduino.cc/en/Main/Software): ferramenta que a extensão para Arduino depende.
* Pacote de quadro de DevKit: Ferramenta cadeias, bibliotecas e projetos para o DevKit.
* Utilitário de ST-Link: Ferramentas essenciais e drivers.

#### <a name="run-the-installation-script"></a>Execute o script de instalação

No Explorador de ficheiros do Windows, localize o ficheiro. zip e extraí-lo. Encontrar `install.cmd`, faça duplo clique nele e selecione **executar como administrador**.

![Explorador de ficheiros](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/run-admin.png)

Durante a instalação, verá o progresso de cada ferramenta ou pacote.

![Progresso da instalação](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/install.png)

> [!NOTE] 
> Dependendo do seu ambiente, por vezes, obterá Falha ao instalar Arduino IDE. Neste caso, pode tentar [instalar Arduino IDE individualmente](https://microsoft.github.io/azure-iot-developer-kit/docs/installation/#windows) e execute novamente o install.cmd. Caso contrário, siga os [passos manuais](https://microsoft.github.io/azure-iot-developer-kit/docs/installation/#windows) para instalar todas as ferramentas necessárias e pacotes.

#### <a name="install-drivers"></a>Instalar controladores

O VS Code para a extensão de Arduino conta com Arduino IDE. Se esta for a primeira vez que está a instalar o Arduino IDE, lhe for pedido para instalar controladores relevantes:

![introdução-iniciada-driver](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/driver.png)

Instalação deve demorar cerca de 10 minutos, consoante a velocidade de internet. Após a instalação estiver concluída, deverá ver o Visual Studio Code e Arduino IDE atalhos no ambiente de trabalho.

> [!NOTE] 
> Ocasionalmente, quando iniciar o VS Code, lhe for pedido com um erro que não é possível localizar o Arduino IDE ou o pacote de quadro relacionados. Para resolvê-lo, feche o VS Code e reinicie o Arduino IDE. VS código deve, em seguida, localizar o caminho de Arduino IDE corretamente.

### <a name="macos"></a>macOS

Nós o encorajamos a utilizar a experiência de instalação de um clique para preparar o ambiente de desenvolvimento. Se tiver quaisquer problemas, pode seguir a [passos manuais](https://microsoft.github.io/azure-iot-developer-kit/docs/installation/) de realizar.

#### <a name="install-homebrew"></a>Instale o Homebrew

> [!NOTE] 
> Se o ter instalado o Homebrew, pode ignorar este passo.

Siga os [instruções de instalação do Homebrew](https://docs.brew.sh/Installation.html) para instalá-lo.

#### <a name="download-the-latest-package"></a>Transferir o pacote mais recente
O ficheiro. zip que transferiu contém todas as ferramentas necessárias e pacotes para o desenvolvimento de DevKit.

> [!div class="button"]
[Transferência](https://aka.ms/devkit/prod/installpackage/mac/latest)

O ficheiro. zip contém as seguintes ferramentas e pacotes. Se já tiver alguns componentes instalados, o script irá detetar e ignorá-los.

* NODE. js e o Yarn: tempo de execução para o script de configuração e tarefas automatizadas.
* [CLI 2.0 do Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest#a-namemacosinstall-on-macos): experiência de linha de comandos para várias plataformas para o gerenciamento de recursos do Azure.
* [Visual Studio Code](https://code.visualstudio.com/) (VS Code): o editor de código simples para o desenvolvimento de DevKit.
* [Extensão do Visual Studio Code para Arduino](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.vscode-arduino): extensão que permite o desenvolvimento de Arduino no Visual Studio Code.
* [Arduino IDE](https://www.arduino.cc/en/Main/Software): ferramenta que a extensão para Arduino depende.
* Pacote de quadro de DevKit: Ferramenta cadeias, bibliotecas e projetos para o DevKit.
* Utilitário de ST-Link: Ferramentas essenciais e drivers.

#### <a name="run-the-installation-script"></a>Execute o script de instalação

No Finder, localize o. zip e extraí-lo:

![Localizador de macOS](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/mac-finder.png)

Iniciar a aplicação de Terminal, localize a pasta extrair o ficheiro. zip e execute:

```bash
./install.sh
```

![instalação para macOS](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/mac-install-sh.png)

> [!NOTE] 
> Se cumprir o erro de permissão do Homebrew, executar `brew doctor` para corrigir. Verifique [FAQ](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/#homebrew-permission-error-on-macos) para obter mais detalhes.

Agora tem todas as ferramentas necessárias e os pacotes instalados para macOS.


## <a name="open-the-project-folder"></a>Abra a pasta de projeto

### <a name="start-vs-code"></a>Inicie o VS Code

Certifique-se de que seu DevKit não está ligado. Inicie o VS Code pela primeira vez e ligar o DevKit ao seu computador. O VS Code automaticamente localiza o DevKit e abre uma página de introdução:

![Página de introdução](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/vscode_start.png)

> [!NOTE] 
> Ocasionalmente, quando iniciar o VS Code, lhe for pedido com um erro que não é possível localizar o Arduino IDE ou o pacote de quadro relacionados. Feche o VS Code e reinicie o Arduino IDE. VS código deve, em seguida, localizar o caminho de Arduino IDE corretamente.


### <a name="open-the-arduino-examples-folder"></a>Abra a pasta de exemplos de Arduino

Sobre o **exemplos de Arduino** separador, navegue para **exemplos para MXCHIP AZ3166** > **AzureIoT**e selecione **GetStarted**.

![Guia de exemplos de Arduino](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/vscode_start.png)

Se fechar o painel, pode abri-lo novamente. Uso `Ctrl+Shift+P` (macOS: `Cmd+Shift+P`) para abrir a paleta de comandos, escreva **Arduino**e, em seguida, localize e selecione **Arduino: exemplos**.

## <a name="provision-azure-services"></a>Aprovisionar serviços do Azure

Na janela da solução, executar a tarefa através de `Ctrl+P` (macOS: `Cmd+P`) ao introduzir `task cloud-provision`.

No terminal VS Code, uma linha de comandos interativa orienta-o através do aprovisionamento os serviços do Azure necessários:

![Linha de comandos interativa](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/connect-iothub/cloud-provision.png)

## <a name="build-and-upload-the-arduino-sketch"></a>Criar e carregar o esboço de Arduino

### <a name="windows"></a>Windows

1. Uso `Ctrl+P` para executar `task device-upload`.
2. Terminal pede-lhe para introduzir o modo de configuração. Para fazer isso, premido o botão A, em seguida, push e largue o botão de reposição. A tela exibe a DevKit id e a 'Configuração'.

Isso é definir a cadeia de ligação que obtém de `task cloud-provision` passo.

Em seguida, o VS Code inicia a verificar e carregar o esboço de Arduino:

![Verificação e o carregamento do esboço de Arduino](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/connect-iothub/device-upload.png)

O DevKit reinicia e começa a execução do código.

> [!NOTE] 
> Ocasionalmente, obterá o erro "erro: AZ3166: pacote desconhecido". Isto ocorre devido ao quadro do índice do pacote não é atualizado. Verificar isso [passos FAQ](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) para resolvê-lo.

### <a name="macos"></a>macOS

1. Colocar DevKit no modo de configuração: mantenha premida A de botão, em seguida, push e lançamento no botão Redefinir. A tela exibe 'Configuração'.
2. Uso `Cmd+P` para executar `task device-upload`.

Isso é definir a cadeia de ligação que obtém de `task cloud-provision` passo.

Em seguida, o VS Code inicia a verificar e carregar o esboço de Arduino:

![carregamento de dispositivo](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/connect-iothub/device-upload.png)

O DevKit reinicia e começa a execução do código.

> [!NOTE] 
> Ocasionalmente, obterá o erro "erro: AZ3166: pacote desconhecido". Isto ocorre devido ao quadro do índice do pacote não é atualizado. Verificar isso [passos FAQ](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) para resolvê-lo.


## <a name="test-the-project"></a>Testar o projeto

No VS Code, siga estes passos para abrir e configurar o Monitor de série:

1. Clique nas `COM[X]` word na barra de status para definir a porta COM correta com `STMicroelectronics`: ![porta com](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/connect-iothub/com-port.png)

2. Clique power plug ícone na barra de status para abrir o Monitor de série: ![monitor de série](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution//connect-iothub/serial-monitor.png)

3. Na barra de status, clique no número que representa a taxa de transmissão e definido como `115200`: ![velocidade de transmissão.](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/connect-iothub/baud-rate.png)

O aplicativo de exemplo está em execução com êxito quando vir os seguintes resultados:

* O Monitor Serial apresenta as mesmas informações como o conteúdo na captura de ecrã abaixo.
* O RGB LED no MXChip IoT DevKit é intermitente.

![Saída final no VS Code](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/connect-iothub/result-serial-output.png)

## <a name="problems-and-feedback"></a>Problemas e comentários

Se tiver problemas, pode encontrar [FAQs](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/). Pode também dar feedback para deixar um comentário nesta página.

## <a name="next-steps"></a>Passos Seguintes

Ligou com êxito um MXChip IoT DevKit ao IoT hub e enviou os dados de sensor capturada ao seu hub IoT.

Para continuar a introdução ao hub IoT do Azure e explorar outros cenários de IoT, veja:

- [Gerir mensagens de dispositivo na cloud com o iothub-explorer](https://docs.microsoft.com/azure/iot-hub/iot-hub-explorer-cloud-device-messaging)
- [Guardar mensagens do Hub IoT no armazenamento de dados do Azure](https://docs.microsoft.com//azure/iot-hub/iot-hub-store-data-in-azure-table-storage)
- [Utilizar o Power BI para visualizar dados de sensores em tempo real do IoT Hub do Azure](https://docs.microsoft.com//azure/iot-hub/iot-hub-live-data-visualization-in-power-bi)
- [Utilizar aplicações Web para visualizar dados de sensores em tempo real do IoT Hub do Azure](https://docs.microsoft.com//azure/iot-hub/iot-hub-live-data-visualization-in-web-apps)
- [Previsão meteorológica com os dados de sensor do seu hub IoT no Azure Machine Learning](https://docs.microsoft.com/azure/iot-hub/iot-hub-weather-forecast-machine-learning)
- [Gestão de dispositivos com o iothub-explorer](https://docs.microsoft.com/azure/iot-hub/iot-hub-device-management-iothub-explorer)
- [Monitorização remota e notificações com Aplicações Lógicas](https://docs.microsoft.com/azure/iot-hub/iot-hub-monitoring-notifications-with-azure-logic-apps)
