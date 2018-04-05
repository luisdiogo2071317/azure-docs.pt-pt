---
title: Receber um e-mail quando a porta está aberta utilizando o serviço do SendGrid e as funções do Azure | Microsoft Docs
description: Monitorize o sensor torção para detetar quando é aberta uma porta e utilizar as funções do Azure para enviar uma notificação por e-mail.
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
ms.date: 03/19/2018
ms.author: liydu
ms.openlocfilehash: e0882a6c87454498d0d1370ee244bfffc137aafb
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/03/2018
---
# <a name="door-monitor"></a>Monitor de porta          

DevKit de IoT MXChip contém um sensor torção incorporado. Neste projeto, pode Deteta a presença ou ausência de um campo magnéticas forte que – neste caso, proveniente de uma pequena. magnet permanente.

## <a name="what-you-learn"></a>O que irá aprender

Este projeto, saber:
- Como utilizar o sensor torção IoT DevKit de localização de MXChip para detetar o movimento de um magnet próximos em.
- Como utilizar o serviço do SendGrid para enviar uma notificação para o seu endereço de correio eletrónico.

> [!NOTE]
> Para uma utilização de prática deste projeto:
> - Monte um magnet na periferia da porta de uma.
> - Monte a DevKit no jamb a porta próximo o magnet. Abrir ou fechar a porta será acionado o sensor, resultando na receber uma notificação de correio eletrónico do evento.

## <a name="what-you-need"></a>Do que precisa

Concluir o [guia de introdução]({{"/docs/get-started/" | absolute_url }}) para:

* Tem a sua DevKit ligado ao Wi-Fi
* Preparar o ambiente de desenvolvimento

Uma subscrição ativa do Azure. Se não tiver uma, pode registar através de um dos seguintes métodos:

* Ativar um [conta de Microsoft Azure avaliação de 30 dias gratuita](https://azureinfo.microsoft.com/us-freetrial.html).
* Afirmação sua [crédito do Azure](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) se for um subscritor MSDN ou o Visual Studio.

## <a name="deploy-sendgrid-service-in-azure"></a>Implementar o serviço do SendGrid no Azure

[SendGrid](https://sendgrid.com/) é uma plataforma de entrega de e-mail baseado na nuvem. Este serviço irá ser utilizado para enviar notificações por e-mail.

> [!NOTE]
> Se já tiver implementado um serviço do SendGrid, pode avançar diretamente para [implementar IoT Hub no Azure](#deploy-iot-hub-in-azure).

### <a name="sendgrid-deployment"></a>Implementação do SendGrid

Para aprovisionar os serviços do Azure, utilize o **implementar no Azure** botão. Este botão ativa uma implementação rápida e fácil dos seus projetos de open source para o Microsoft Azure.

Clique em de **implementar no Azure** no botão abaixo. 

[![Implementar no Azure](https://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FVSChina%2Fdevkit-door-monitor%2Fmaster%2FSendGridDeploy%2Fazuredeploy.json)

Em seguida, consulte a página seguinte.

> [!NOTE]
> Se não vir a página seguinte, poderá ter de iniciar sessão pela primeira vez na sua conta do Azure.

Preencha o formulário de inscrição:

  * **Grupo de recursos**: criar um grupo de recursos para alojar o serviço do SendGrid, ou utilize uma já existente. Consulte [utilizar grupos de recursos para gerir os recursos do Azure](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-portal).

  * **Nome**: O nome do seu serviço do SendGrid. Escolha um nome exclusivo, diferente do outros serviços que possa ter.

  * **Palavra-passe**: O serviço requer uma palavra-passe, que não será para qualquer coisa neste projecto.

  * **E-mail**: SendGrid o serviço irá enviar verificação para este endereço de e-mail.

  > [!NOTE]
  > Verifique o **afixar ao dashboard** opção para facilitar esta aplicação a localizar no futuro.
 
![Implementação do SendGrid](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/sendgrid-deploy.png)

### <a name="sendgrid-api-key-creation"></a>Criação da chave de API do SendGrid

Após a implementação for bem sucedida e, em seguida, clique em de **gerir** botão. Direcionado para a página do SendGrid e precisam de verificar o seu endereço de e-mail.

![Gerir SendGrid](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/sendgrid-manage.png)

Na página do SendGrid, clique em **definições** > **chaves de API** > **criar chave de API**. Entrada de **nome da chave de API** e clique em **Criar & vista**.

![SendGrid criar API pela primeira vez](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/sendgrid-create-api-first.png)

![SendGrid criar API segundo](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/sendgrid-create-api-second.png)

A chave de API é apresentada apenas uma vez. Lembre-se de que copie e guarde-o em segurança, porque é utilizado no próximo passo.

## <a name="deploy-iot-hub-in-azure"></a>Implementar o Hub IoT no Azure

Os passos seguintes irão aprovisionar outras IoT do Azure relacionados com serviços e implementar as funções do Azure para este projeto.

Clique em de **implementar no Azure** no botão abaixo. 

[![Implementar no Azure](https://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FVSChina%2Fdevkit-door-monitor%2Fmaster%2Fazuredeploy.json)

Em seguida, consulte a página seguinte.

> [!NOTE]
> Se não vir a página seguinte, poderá ter de iniciar sessão pela primeira vez na sua conta do Azure.

Preencha o formulário de inscrição:

  * **Grupo de recursos**: criar um grupo de recursos para alojar o serviço do SendGrid, ou utilize uma já existente. Consulte [utilizar grupos de recursos para gerir os recursos do Azure](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-portal).

  * **Nome do Hub IOT**: O nome do IoT hub. Escolha um nome exclusivo, diferente do outros serviços que possa ter.

  * **Sku do IOT Hub**: F1 (limitado por subscrição) é gratuita. Pode ver mais informações sobre preços em [escalão de preço e escala](https://azure.microsoft.com/pricing/details/iot-hub/).

  * **De correio eletrónico**: deve ser o mesmo endereço de correio eletrónico que utilizou quando configurar o serviço do SendGrid.

  > [!NOTE]
  > Verifique o **afixar ao dashboard** opção para facilitar esta aplicação a localizar no futuro.
 
![Implementação de IoTHub](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/iot-hub-deploy.png)

## <a name="build-and-upload-the-code"></a>Criar e carregar o código

### <a name="start-vs-code"></a>Iniciar o VS Code

- Certifique-se do DevKit **não** ligada ao seu computador.
- Inicie o VS Code.
- Ligar o DevKit para o seu computador.

O VS Code irá detetar o DevKit e abrir uma página de introdução automaticamente:

![VSCode](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/vscode-start.png)

> [!NOTE]
> Ao iniciar o VS Code, poderá receber uma mensagem de erro a indicar que não é possível localizar Arduino IDE ou pacote quadro relacionados. Se receber este erro, fechar o VS Code, inicie o IDE Arduino novamente e VS Code devem localizar o caminho de Arduino IDE corretamente.

### <a name="open-arduino-examples-folder"></a>Abrir pasta do Arduino exemplos

Expanda o lado esquerdo **ARDUINO exemplos** secção, navegue para **exemplos de MXCHIP AZ3166 > AzureIoT**e selecione **DoorMonitor**. Esta ação abre uma nova janela do código de VS com uma pasta do projeto no mesmo.

![mini-solution-examples](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/vscode-examples.png)

Se ocorrer fechar o painel, pode abri-lo novamente. Utilize `Ctrl+Shift+P` (macOS: `Cmd+Shift+P`) para abrir a paleta de comandos, escreva **Arduino**e, em seguida, localize e selecione **Arduino: exemplos**.

### <a name="provision-azure-services"></a>Aprovisionar os serviços do Azure

Na janela da solução, execute a tarefa de aprovisionamento de nuvem:
- Tipo `Ctrl+P` (macOS: `Cmd+P`).
- Introduza `task cloud-provision` na caixa de texto fornecidos.

No terminal VS Code, uma linha de comandos interativa orienta-o aprovisionamento de serviços do Azure necessários. Selecione todos os itens mesmos na lista de pedido que aprovisionados anteriormente no [implementar IoT Hub no Azure](#deploy-iot-hub-in-azure).

![Aprovisionamento de nuvem](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/cloud-provision.png)

> [!NOTE]
> Se a página bloqueia o estado de carregamento ao tentar iniciar sessão no Azure, consulte [FAQ](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/#page-hangs-when-log-in-azure) para resolver este problema. 

### <a name="build-and-upload-the-device-code"></a>Criar e carregar o código de dispositivo

#### <a name="windows"></a>Windows

1. Utilize `Ctrl+P` executar `task device-upload`.
2. O terminal pede-lhe para introduzir o modo de configuração. Para tal, mantenha premida botão A, em seguida, push e no botão de reposição de versão. O ecrã mostra o número de identificação DevKit e a palavra *configuração*.

Este procedimento define a cadeia de ligação que é obtida a partir de [serviços do Azure de aprovisionar](#provision-azure-services) passo.

O VS Code, em seguida, inicia a verificar e carregar o Arduino sketch para o DevKit:

![device-upload](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/device-upload.png)

O DevKit reinicia e começa a executar o código.

> [!NOTE]
> Ocasionalmente, poderá receber um "erro: AZ3166: pacote desconhecido" mensagem de erro. Este erro ocorre quando o índice de pacote de placa não está atualizado corretamente. Para resolver este erro, consulte este [FAQ](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/#development).

#### <a name="macos"></a>macOS

1. Colocar o DevKit no modo de configuração: mantenha premida A botão, em seguida, push e versão no botão de reposição. O ecrã apresenta a 'Configuração'.
2. Utilize `Cmd+P` executar `task device-upload`.

Este procedimento define a cadeia de ligação que é obtida a partir de [serviços do Azure de aprovisionar](#provision-azure-services) passo.

O VS Code, em seguida, inicia a verificar e carregar o Arduino sketch para o DevKit:

![device-upload](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/device-upload.png)

O DevKit reinicia e começa a executar o código.

> [!NOTE]
> Ocasionalmente, poderá receber um "erro: AZ3166: pacote desconhecido" mensagem de erro. Este erro ocorre quando o índice de pacote de placa não está atualizado corretamente. Para resolver este erro, consulte este [FAQ](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/#development).

## <a name="test-the-project"></a>Testar o projeto

O programa inicializa primeiro quando a DevKit na presença de um campo magnéticas estável.

Após a inicialização, `Door closed` é apresentado no ecrã. Quando existe uma alteração no campo magnéticas, o estado é alterado para `Door opened`. Cada vez que as alterações de estado de porta, receberá uma notificação por e-mail. (Estas mensagens de correio eletrónico podem demorar até cinco minutos para que sejam recebidas).

![Magnets próximo o sensor: porta fechada](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/test-door-closed.jpg "Magnets próximo o sensor: porta fechada")

![Magnet movidos para fora do sensor: porta aberta](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/test-door-opened.jpg "Magnet movidos para fora do sensor: porta aberta")

## <a name="problems-and-feedback"></a>Problemas e comentários

Se tiver problemas, consulte [perguntas mais frequentes](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) ou estabelecer ligação utilizando as seguintes canais:

* [Gitter.im](http://gitter.im/Microsoft/azure-iot-developer-kit)
* [Stackoverflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Passos Seguintes

Aprendeu como ligar um dispositivo DevKit para o Azure IoT Suite e utilizar o serviço do SendGrid para enviar uma mensagem de e-mail. Eis os passos sugeridos:

* [Descrição geral do Azure IoT Suite](https://docs.microsoft.com/azure/iot-suite/)
* [Ligue um dispositivo de MXChip IoT DevKit para a aplicação Microsoft IoT Central](https://docs.microsoft.com/en-us/microsoft-iot-central/howto-connect-devkit)
