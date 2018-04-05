---
title: Obter uma mensagem do Twitter com as funções do Azure | Microsoft Docs
description: Utilize o sensor de movimento para detetar abanar e utilizar as funções do Azure para localizar um tweet aleatório com um hashtag que especificar.
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
ms.date: 03/07/2018
ms.author: liydu
ms.openlocfilehash: a84393c5c53b8f8e4a8b688a462f433b2d611b0e
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/03/2018
---
# <a name="shake-shake-for-a-tweet----retrieve-a-twitter-message-with-azure-functions"></a>Abanar, abanar para um Tweet – obter uma mensagem do Twitter com as funções do Azure!

Este projeto, irá aprender a utilizar o sensor de movimento para acionar um evento com as funções do Azure. A aplicação obtém um tweet aleatório com um #hashtag que configurar na sua sketch Arduino. Apresenta o tweet no ecrã DevKit.

## <a name="what-you-need"></a>Do que precisa

Concluir o [guia de introdução](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started) para:

* O DevKit ligaram Wi-Fi.
* Prepare o ambiente de desenvolvimento.

Uma subscrição ativa do Azure. Se não tiver uma, pode registar através de um dos seguintes métodos:

* Ativar um [conta de Microsoft Azure avaliação de 30 dias gratuita](https://azure.microsoft.com/en-us/free/)
* Afirmação sua [crédito do Azure](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) se forem subscritor MSDN ou o Visual Studio

## <a name="open-the-project-folder"></a>Abra a pasta do projeto

### <a name="start-vs-code"></a>Iniciar o VS Code

- Certifique-se do DevKit **não** ligada ao seu computador.
- Inicie o VS Code.
- Ligar o DevKit para o seu computador.

O VS Code automaticamente localiza o DevKit e apresenta uma página de introdução:

![mini-solution-vscode](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/vscode_start.png)

> [!NOTE]
> Ao iniciar o VS Code, poderá receber uma mensagem de erro Arduino IDE ou pacote quadro relacionados não pode ser encontrado. Se este erro ocorrer, feche o VS Code e inicie a IDE Arduino novamente. O VS Code agora deve localizar o caminho do Arduino IDE corretamente.

### <a name="open-arduino-examples-folder"></a>Abrir pasta do Arduino exemplos

Expanda o lado esquerdo **ARDUINO exemplos** secção, navegue para **exemplos de MXCHIP AZ3166 > AzureIoT**e selecione **ShakeShake**. Abre uma nova janela do código de VS com uma pasta do projeto no mesmo.

![mini-solution-examples](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/vscode_examples.png)

Se ocorrer fechar o painel, pode abri-lo novamente. Utilize `Ctrl+Shift+P` (macOS: `Cmd+Shift+P`) para abrir a paleta de comandos, escreva **Arduino**e, em seguida, localize e selecione **Arduino: exemplos**.

## <a name="provision-azure-services"></a>Aprovisionar os serviços do Azure

Na janela da solução, executar a tarefa `Ctrl+P` (macOS: `Cmd+P`), introduzindo `task cloud-provision`.

No terminal VS Code, uma linha de comandos interativa orienta-o aprovisionamento de serviços do Azure necessários:

![cloud-provision](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/cloud-provision.png)

> [!NOTE]
> Se a página bloqueia o estado de carregamento ao tentar iniciar sessão no Azure, consulte este [FAQ passo] ({{"/docs/faq/#page-hangs-when-log-in-azure" | 
 
## <a name="modify-the-hashtag"></a>Modificar o #hashtag

Abra `ShakeShake.ino` e procure esta linha de código:

```cpp
static const char* iot_event = "{\"topic\":\"iot\"}";
```

Substitua a cadeia `iot` dentro de chavetas com o hashtag preferencial. DevKit mais tarde obtém um tweet aleatório, que inclui o hashtag que especificar neste passo.

## <a name="deploy-azure-functions"></a>Implementar as Funções do Azure

Utilize `Ctrl+P` (macOS: `Cmd+P`) para executar `task cloud-deploy` para começar a implementar o código de funções do Azure:

![cloud-deploy](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/cloud-deploy.png)

> [!NOTE]
> Ocasionalmente, a função do Azure pode não funcionar corretamente. Para resolver este problema quando ocorrer, selecionar esta opção [FAQ passo](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/#compilation-error-for-azure-function).

## <a name="build-and-upload-the-device-code"></a>Criar e carregar o código de dispositivo

### <a name="windows"></a>Windows

1. Utilize `Ctrl+P` executar `task device-upload`.
2. O terminal pede-lhe para introduzir o modo de configuração. Para tal:

   * Mantenha premida A do botão
   * Push e o botão de reposição de versão.

3. O ecrã apresenta a DevKit ID e a 'Configuração'.
4. Isto define a cadeia de ligação que é obtida a partir de `task cloud-provision` passo.
5. O VS Code, em seguida, inicia a verificar e carregar o Arduino sketch para sua DevKit: ![carregamento do dispositivo](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/device-upload.png)
6. O DevKit reinicia e começa a executar o código.

> [!NOTE]
> Poderá receber um "erro: AZ3166: pacote desconhecido" mensagem de erro. Este erro ocorre quando o índice de pacote placa não se encontra atualizado corretamente. Para resolver este problema, verifique [FAQ passo](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/#development).

### <a name="macos"></a>macOS

1. Colocar o DevKit no modo de configuração: mantenha premida A botão, em seguida, push e versão no botão de reposição. O ecrã apresenta a 'Configuração'.
2. Utilize `Cmd+P` executar `task device-upload` para definir a cadeia de ligação que é obtida a partir de `task cloud-provision` passo.
3. O VS Code, em seguida, inicia a verificar e carregar o Arduino sketch para sua DevKit: ![carregamento do dispositivo](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/device-upload.png)
4. O DevKit reinicia e começa a executar o código.

> [!NOTE]
> Poderá receber um "erro: AZ3166: pacote desconhecido" mensagem de erro. Este erro ocorre quando o índice de pacote placa não se encontra atualizado corretamente. Para resolver este problema, verifique [FAQ passo](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/#development).

## <a name="test-the-project"></a>Testar o projeto

Após a inicialização de aplicações e um botão de versão, clique em gently abane o Conselho DevKit. Esta ação obtém um tweet aleatório, que contém o hashtag especificado anteriormente. Dentro de alguns segundos, um tweet apresenta no seu ecrã DevKit:

### <a name="arduino-application-initializing"></a>Arduino inicializar a aplicação...
![A inicializar Arduino-aplicações](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/result-1.png)

### <a name="press-a-to-shake"></a>Prima para abanar...
![Prima-um-para-abanar](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/result-2.png)

### <a name="ready-to-shake"></a>Pronto para abanar...
![Pronto para abanar](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/result-3.png)

### <a name="processing"></a>A processar...
![A processar](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/result-4.png)

### <a name="press-b-to-read"></a>Prima B ler...
![Prima B para leitura](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/result-5.png)

### <a name="display-a-random-tweet"></a>Apresenta um tweet aleatório...
![Display-a-random-tweet](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/result-6.png)

- Clique no botão uma novamente e, em seguida, abanar para um tweet de novo.
- Clique no botão B se deslocar para o resto do tweet.

## <a name="how-it-works"></a>Como funciona

![Diagrama](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/diagram.png)

O sketch Arduino enviará um evento para o IoT Hub do Azure. Este evento é acionado a aplicação das funções do Azure. Aplicação de funções do Azure contém a lógica para ligar à API do Twitter e obter um tweet. -Lo, em seguida, encapsula num wrapper o texto de tweet para um C2D mensagem (nuvem para o dispositivo) e envia-lo novamente para o dispositivo.

## <a name="optional-use-your-own-twitter-bearer-token"></a>Opcional: Utilizar os seus próprios tokens de portador do Twitter

Para fins de teste, este projeto de exemplo utiliza um token de portador do Twitter previamente configurado. No entanto, há um [o limite de velocidade](https://dev.twitter.com/rest/reference/get/search/tweets) para cada conta do Twitter. Se pretender considere utilizar os seus próprios tokens, siga estes passos:

1. Aceda a [portal do programador do Twitter](https://dev.twitter.com/) para registar uma nova aplicação do Twitter.

2. [Obter a chave de consumidor e segredos de consumidor](https://support.yapsody.com/hc/en-us/articles/203068116-How-do-I-get-a-Twitter-Consumer-Key-and-Consumer-Secret-key-) da sua aplicação.

3. Utilize [algumas utilitário](https://gearside.com/nebula/utilities/twitter-bearer-token-generator/) para gerar um token de portador do Twitter destas duas chaves.

4. No [portal do Azure](https://portal.azure.com/){: destino = blank"}, obter o **grupo de recursos** e localizar a função do Azure (tipo: do serviço de aplicações) para o projeto"Abanar, abanar". O nome contém sempre 'abanar...' cadeia.
  ![azure-function](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/azure-function.png)

5. Atualize o código para `run.csx` dentro **funções > shakeshake cs** com o seus próprios token:
  ```csharp
  ...
  string authHeader = "Bearer " + "[your own token]";
  ...
  ```
  ![token do twitter](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/twitter-token.png)

6. Guarde o ficheiro e clique em **executar**.


## <a name="problems-and-feedback"></a>Problemas e comentários

### <a name="the-screen-displays-no-tweets-while-every-step-has-run-successfully"></a>O ecrã apresenta 'Não Tweets' enquanto cada passo foi executada com êxito

Esta condição ocorre normalmente pela primeira vez, implementar e executar o exemplo, uma vez que a aplicação de função requer em qualquer local de alguns segundos para que um minuto para a frio a aplicação. Em alternativa, ao executar o código, existem algumas blips que provocam um reinício da aplicação. Quando esta condição ocorre, a aplicação de dispositivo pode obter um tempo limite para o tweet a obter. Neste caso, tente um ou ambos dos seguintes métodos para resolver este problema:

1. Clique no botão de reposição de DevKit para executar novamente a aplicação de dispositivo.

2. No [portal do Azure](https://portal.azure.com/), localize a aplicação de funções do Azure que criou e reinicie-o: ![reinício de função do azure](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/azure-function-restart.png)

### <a name="feedback"></a>Comentários

Se tiver outros problemas, consulte [perguntas mais frequentes](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) ou contacte-nos de que as seguintes canais:

* [Gitter.im](http://gitter.im/Microsoft/azure-iot-developer-kit)
* [Stackoverflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Passos Seguintes

Agora que aprendeu como ligar um dispositivo de DevKit para o Azure IoT Suite e obter um tweet, Eis os passos sugeridos:

* [Descrição geral do Azure IoT Suite](https://docs.microsoft.com/azure/iot-suite/)