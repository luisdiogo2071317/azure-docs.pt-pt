---
title: Conversor de IoT DevKit utilizando a função do Azure e serviços cognitivos | Microsoft Docs
description: Utilizar microfone IoT DevKit para receber a mensagem de voz e serviços cognitivos do Azure para o processamento de texto traduzidas em inglês
author: liydu
manager: jeffya
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 02/28/2018
ms.author: liydu
ms.openlocfilehash: ba2325272552a13d6e464797b1fb523415393100
ms.sourcegitcommit: e34afd967d66aea62e34d912a040c4622a737acb
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/26/2018
ms.locfileid: "36945943"
---
# <a name="use-iot-devkit-az3166-with-azure-function-and-cognitive-services-to-make-a-language-translator"></a>Utilize o IoT DevKit AZ3166 com a função do Azure e serviços cognitivos para tornar um conversor de idioma

Neste artigo, irá aprender a tornar IoT DevKit como um conversor de idioma utilizando [serviços cognitivos Azure](https://azure.microsoft.com/services/cognitive-services/). Regista a sua voz e converte-lo para inglês texto apresentado no ecrã DevKit.

O [MXChip IoT DevKit](https://aka.ms/iot-devkit) é uma placa de compatível de Arduino tudo-em-um com periféricos avançados e sensores. Pode desenvolver para o mesmo utilizando [extensão do Visual Studio Code para Arduino](https://aka.ms/arduino). E é fornecido com um crescer [catálogo projetos](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/) para ajudá-lo soluções de Internet das coisas (IoT) de protótipo tirar partido dos serviços do Microsoft Azure.

## <a name="what-you-need"></a>Do que precisa

Concluir o [guia de introdução](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started) para:

* Tem a sua DevKit ligado ao Wi-Fi
* Preparar o ambiente de desenvolvimento

Uma subscrição ativa do Azure. Se não tiver uma, pode registar através de um destes dois métodos:

* Ativar um [conta de Microsoft Azure avaliação de 30 dias gratuita](https://azure.microsoft.com/free/)
* Afirmação sua [crédito do Azure](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) se forem subscritor MSDN ou o Visual Studio

## <a name="step-1-open-the-project-folder"></a>Passo 1. Abra a pasta do projeto

### <a name="a-start-vs-code"></a>A. Iniciar o VS Code

- Certifique-se a que sua DevKit está ligada ao seu PC.
- Iniciar o VS Code
- Ligar o DevKit para o seu computador.

### <a name="b-open-the-arduino-examples-folder"></a>B. Abra a pasta de Arduino exemplos

Expanda o lado esquerdo **ARDUINO exemplos > exemplos de MXCHIP AZ3166 > AzureIoT**e selecione **DevKitTranslator**. É aberta uma nova janela do código de VS com a pasta do projeto DEVKITTRANSLATOR no mesmo.  

> [!NOTE]
> Se não é possível ver os exemplos para a secção de MXCHIP AZ3166, certifique-se de que o dispositivo está ligado corretamente e reinicie o Visual Studio Code.  

![Exemplos de IoT DevKit](media/iot-hub-arduino-iot-devkit-az3166-translator/vscode_examples.png)

> [!NOTE]
> Pode também abrir o exemplo de paleta de comando. Utilize `Ctrl+Shift+P` (macOS: `Cmd+Shift+P`) para abrir a paleta de comandos, escreva **Arduino**e, em seguida, localize e selecione **Arduino: exemplos**.

## <a name="step-2-provision-azure-services"></a>Passo 2. Aprovisionar os serviços do Azure

Na janela de solução, escreva `Ctrl+P` (macOS: `Cmd+P`) e introduza `task cloud-provision`.

Com o código de VS terminal, uma linha de comandos interativa irá ajudá-lo para aprovisionar todos os serviços do Azure necessários:

![Tarefa de aprovisionamento de nuvem](media/iot-hub-arduino-iot-devkit-az3166-translator/cloud-provision.png)

## <a name="step-3-deploy-azure-functions"></a>Passo 3. Implementar as Funções do Azure

Utilize `Ctrl+P` (macOS: `Cmd+P`) para executar `task cloud-deploy` para implementar o código de funções do Azure. Este processo normalmente demora 2 a 5 minutos a concluir:

![Tarefa de implementação de nuvem](media/iot-hub-arduino-iot-devkit-az3166-translator/cloud-deploy.png)

Depois de função do Azure implementa com êxito, preencha o ficheiro de azure_config.h com o nome de aplicação de função. Pode navegar para [portal do Azure](https://portal.azure.com/) a encontrá-lo:

![Localizar o nome de aplicação de função do Azure](media/iot-hub-arduino-iot-devkit-az3166-translator/azure-function.png)

> [!NOTE]
> Se a função do Azure não funciona corretamente, selecionar esta opção [perguntas mais frequentes](https://microsoft.github.io/azure-iot-developer-kit/docs/faq#compilation-error-for-azure-function) secção resolvê-lo.

## <a name="step-4-build-and-upload-the-device-code"></a>Passo 4. Criar e carregar o código de dispositivo

1. Utilize `Ctrl+P` (macOS: `Cmd+P`) para executar `task config-device-connection`.

2. O terminal irá pedir-lhe se pretende utilizar a cadeia de ligação que obtém a partir do `task cloud-provision` passo. Também pode introduzir a sua própria cadeia de ligação do dispositivo ao selecionar **'Criar novo …'**

3. O terminal pede-lhe para introduzir o modo de configuração. Para tal, mantenha premida botão A, em seguida, push e no botão de reposição de versão. O ecrã apresenta a DevKit ID e a 'Configuração'.
  ![Verificação e de carregamento do sketch Arduino](media/iot-hub-arduino-iot-devkit-az3166-translator/config-device-connection.png)

4. Depois de `task config-device-connection` terminar, clique em `F1` para carregar comandos VS Code e selecione `Arduino: Upload`, em seguida, o VS Code inicia a verificar e carregar o Arduino sketch: ![verificação e de carregamento do sketch Arduino](media/iot-hub-arduino-iot-devkit-az3166-translator/arduino-upload.png)

O DevKit reinicia e começa a executar o código.

## <a name="test-the-project"></a>Testar o projeto

Após a inicialização de aplicação, siga as instruções no ecrã DevKit. O idioma de origem predefinido é chinês.

Para selecionar outro idioma da tradução:

1. Prima o botão A introduzir o modo de configuração.
2. Clique no botão B se deslocar para todas as linguagens de origem.
3. Prima um botão para confirmar a sua preferência de idioma da origem.
4. Prima e mantenha botão B durante a fala e, em seguida, botão B para iniciar a tradução de versão.
5. O texto traduzido mostra inglês no ecrã.

![Desloque-se para selecionar o idioma](media/iot-hub-arduino-iot-devkit-az3166-translator/select-language.jpg)

![Resultado da conversão](media/iot-hub-arduino-iot-devkit-az3166-translator/translation-result.jpg)

No ecrã de resultado da conversão, pode:

- Prima o botão A e B, desloque-se e selecione o idioma de origem.
- Clique no botão B para falar, para enviar a voz e obter o texto de tradução de versão

## <a name="how-it-works"></a>Como funciona

![mini-Solution-Voice-to-tweet-Diagram](media/iot-hub-arduino-iot-devkit-az3166-translator/diagram.png)

Os registos de sketch Arduino sua voz mensagens, em seguida, um pedido HTTP para o acionador das funções do Azure. As funções do Azure chama o conversor de reconhecimento de voz de serviço cognitivos API para efetuar a conversão. Depois das funções do Azure obtém o texto de tradução, envia uma mensagem de C2D para o dispositivo. Em seguida, a conversão é apresentada no ecrã.

## <a name="change-device-id"></a>ID de dispositivo de alteração

O ID de dispositivo predefinido registado no IoT Hub do Azure é **AZ3166**. Se pretender modificá-lo, siga [instruções aqui](https://microsoft.github.io/azure-iot-developer-kit/docs/customize-device-id/).

## <a name="problems-and-feedback"></a>Problemas e comentários

Se tiver problemas, consulte [perguntas mais frequentes](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) ou entrar-nos de que as seguintes canais:

* [Gitter.im](http://gitter.im/Microsoft/azure-iot-developer-kit)
* [Stackoverflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Próximos Passos

Agora que efetuar o IoT DevKit como um conversor utilizando a função do Azure e serviços cognitivos. Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Utilizar tarefas do Visual Studio Code para automatizar Aprovisiona de nuvem
> * Configurar a cadeia de ligação do dispositivo IoT do Azure
> * Implementar a função do Azure
> * Testar a tradução de mensagem de voz

Avançar para os outros tutoriais para saber mais:

> [!div class="nextstepaction"]
> [Ligar IoT DevKit AZ3166 acelerador de solução de monitorização remota do Azure IoT](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring)
