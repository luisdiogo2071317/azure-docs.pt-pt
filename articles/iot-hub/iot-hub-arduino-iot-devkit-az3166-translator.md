---
title: Criar um tradutor de IoT DevKit com as funções do Azure e os serviços cognitivos | Documentos da Microsoft
description: Utilizar o microfone num IoT DevKit para receber uma mensagem de voz e, em seguida, utilizar os serviços cognitivos do Azure para processá-lo para o texto traduzido em inglês
author: liydu
manager: jeffya
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 02/28/2018
ms.author: liydu
ms.openlocfilehash: cd67e612dd020ba600e33ac8baf77bc094d8afd3
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/20/2018
ms.locfileid: "42061038"
---
# <a name="use-iot-devkit-az3166-with-azure-functions-and-cognitive-services-to-make-a-language-translator"></a>Utilizar o IoT DevKit AZ3166 com as funções do Azure e os serviços cognitivos para tornar um tradutor lingüístico

Neste artigo, saiba como tornar a IoT DevKit como um tradutor lingüístico usando [serviços cognitivos do Azure](https://azure.microsoft.com/services/cognitive-services/). Regista a voz e traduz-se aparecem no ecrã DevKit de texto em inglês.

O [MXChip IoT DevKit](https://aka.ms/iot-devkit) é uma área de compatível de Arduino tudo-em-um com periféricos avançados e sensores. Pode desenvolver para ele usando [extensão do Visual Studio Code para Arduino](https://aka.ms/arduino). E ele vem com um crescimento [catálogo de projetos](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/) para orientá-lo através de soluções de Internet das coisas (IoT) de protótipo que tiram partido dos serviços do Microsoft Azure.

## <a name="what-you-need"></a>Do que precisa

Concluir o [guia de introdução](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started) para:

* Sua DevKit tiver ligado ao Wi-Fi
* Preparar o ambiente de desenvolvimento

Uma subscrição ativa do Azure. Se não tiver um, pode registrar-se através de um dos seguintes dois métodos:

* Ativar um [conta de Microsoft Azure avaliação de 30 dias gratuita](https://azure.microsoft.com/free/)
* Afirmação sua [crédito do Azure](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) se for assinante do MSDN ou o Visual Studio

## <a name="open-the-project-folder"></a>Abra a pasta de projeto

Em primeiro lugar, abra a pasta de projeto. 

### <a name="start-vs-code"></a>Inicie o VS Code

- Certifique-se de que sua DevKit está ligado ao seu PC.

- Inicie o VS Code.

- Ligar o DevKit ao seu computador.

### <a name="open-the-arduino-examples-folder"></a>Abra a pasta de exemplos de Arduino

Expanda o lado esquerdo **exemplos de ARDUINO > exemplos para MXCHIP AZ3166 > AzureIoT**e selecione **DevKitTranslator**. É aberta uma nova janela do VS Code, exibindo a pasta do projeto. Se não pode ver os exemplos para a secção de MXCHIP AZ3166, certifique-se de que o dispositivo está ligado corretamente e reinicie o VS Code.  

![Exemplos de IoT DevKit](media/iot-hub-arduino-iot-devkit-az3166-translator/vscode_examples.png)

Também é possível abrir o exemplo da paleta de comandos. Uso `Ctrl+Shift+P` (macOS: `Cmd+Shift+P`) para abrir a paleta de comandos, escreva **Arduino**e, em seguida, localize e selecione **Arduino: exemplos**.

## <a name="provision-azure-services"></a>Aprovisionar serviços do Azure

Na janela da solução, escreva `Ctrl+P` (macOS: `Cmd+P`) e introduza `task cloud-provision`.

No código de VS terminal, uma linha de comandos interativa guiá-lo para aprovisionar todos os serviços do Azure necessários:

![Tarefa de aprovisionamento de cloud](media/iot-hub-arduino-iot-devkit-az3166-translator/cloud-provision.png)

## <a name="deploy-the-azure-function"></a>Implementar a função do Azure

Uso `Ctrl+P` (macOS: `Cmd+P`) para executar `task cloud-deploy` para implementar o código de funções do Azure. Este processo normalmente demora 2 a 5 minutos a concluir.

![Tarefa de implementação de nuvem](media/iot-hub-arduino-iot-devkit-az3166-translator/cloud-deploy.png)

Depois da função do Azure, implementar com êxito, preencha o ficheiro de azure_config.h com o nome da aplicação. Pode navegar até [portal do Azure](https://portal.azure.com/) encontrá-lo:

![Localizar o nome de aplicação de função do Azure](media/iot-hub-arduino-iot-devkit-az3166-translator/azure-function.png)

> [!NOTE]
> Verifique se a função do Azure não funcionar corretamente, o [página de "erro de complicação para a função do Azure" nas perguntas frequentes IoT DevKit](https://microsoft.github.io/azure-iot-developer-kit/docs/faq#compilation-error-for-azure-function) para resolvê-lo.

## <a name="build-and-upload-the-device-code"></a>Criar e carregar o código de dispositivo

1. Uso `Ctrl+P` (macOS: `Cmd+P`) para executar `task config-device-connection`.

2. Terminal irá pedir-lhe se pretende utilizar a cadeia de ligação que obtém de `task cloud-provision` passo. Também pode introduzir a sua própria cadeia de ligação do dispositivo ao selecionar **"Criar nova..."**

3. Terminal pede-lhe para introduzir o modo de configuração. Para fazer isso, premido o botão A, em seguida, push e largue o botão de reposição. A tela exibe a DevKit ID e a 'Configuração'.

   ![Verificação e o carregamento do esboço de Arduino](media/iot-hub-arduino-iot-devkit-az3166-translator/config-device-connection.png)

4. Após `task config-device-connection` terminado, clique `F1` para carregar os comandos de código VS e selecione `Arduino: Upload`, em seguida, o VS Code inicia a verificação e carregar o Arduino criar esboços.

   ![Verificação e o carregamento do esboço de Arduino](media/iot-hub-arduino-iot-devkit-az3166-translator/arduino-upload.png)

O DevKit reinicia e começa a execução do código.

## <a name="test-the-project"></a>Testar o projeto

Após a inicialização de aplicação, siga as instruções no ecrã DevKit. O idioma de origem do padrão é chinês.

Para selecionar outro idioma para tradução:

1. Prima A de botão para entrar no modo de configuração.

2. Prima o botão B de se deslocar para todos com suporte de idiomas de origem.

3. Pressione um botão para confirmar a sua escolha de idioma de origem.

4. Prima e mantenha premido o botão B ao mesmo tempo falando, em seguida, B de botão para iniciar a tradução de versão.

5. O texto traduzido em inglês mostra no ecrã.

![Desloque-se para selecionar idioma](media/iot-hub-arduino-iot-devkit-az3166-translator/select-language.jpg)

![Resultado de tradução](media/iot-hub-arduino-iot-devkit-az3166-translator/translation-result.jpg)

No ecrã do resultado da conversão, pode:

- Prima botões A e B, desloque-se e selecione o idioma de origem.

- Prima o botão de B para falar. Para enviar a voz e obter o texto de tradução, largue o botão de B.

## <a name="how-it-works"></a>Como funciona

![mini-Solution-Voice-to-tweet-Diagram](media/iot-hub-arduino-iot-devkit-az3166-translator/diagram.png)

Os registos de esboço de Arduino sua voz, em seguida, publica um HTTP do pedido para acionar uma função do Azure. A função do Azure chama o tradutor de voz do serviço cognitivo API para fazer a tradução. Depois da função do Azure obtém o texto de tradução, envia uma mensagem de (cloud-para-dispositivo) C2D no dispositivo. Em seguida, a tradução é apresentada no ecrã.

## <a name="problems-and-feedback"></a>Problemas e comentários

Se tiver problemas, consulte a [IoT DevKit FAQ](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) ou contacte-nos com os seguintes canais:

* [Gitter.im](http://gitter.im/Microsoft/azure-iot-developer-kit)
* [Stackoverflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Próximos Passos

Aprendeu como utilizar o IoT DevKit como um tradutor ao utilizar as funções do Azure e os serviços cognitivos. Este procedimento, ficou a saber como:

> [!div class="checklist"]
> * Utilizar tarefas do Visual Studio Code para automatizar as provisões de cloud
> * Configurar a cadeia de ligação do dispositivo IoT do Azure
> * Implementar a função do Azure
> * Testar a tradução de mensagem de voz

Avance para os outros tutoriais para saber mais:

> [!div class="nextstepaction"]
> [Ligar a IoT DevKit AZ3166 ao solution accelerator do monitoramento remoto do Azure IoT](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring)
