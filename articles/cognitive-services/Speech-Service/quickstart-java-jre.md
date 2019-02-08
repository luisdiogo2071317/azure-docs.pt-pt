---
title: 'Início rápido: Reconhecer a conversão de voz, o Java (Windows, Linux) - serviços de voz'
titleSuffix: Azure Cognitive Services
description: Neste início rápido, irá aprender a criar uma aplicação Java simples que captura e converte voz do utilizador do microfone do seu computador.
services: cognitive-services
author: fmegen
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 12/18/2018
ms.author: fmegen
ms.openlocfilehash: 87360d49892698458a021287d88240d98ba2ee19
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/07/2019
ms.locfileid: "55881513"
---
# <a name="quickstart-recognize-speech-with-the-speech-sdk-for-java"></a>Início rápido: Reconhecer a conversão de voz com o SDK de voz para Java

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Neste artigo, vai criar uma aplicação de consola Java com o [SDK do Serviço de Voz](speech-sdk.md). Converta voz em texto em tempo real a partir do microfone do seu PC. A aplicação baseia-se com o pacote Maven do SDK de voz e o IDE de Java de Eclipse (v4.8) no Windows de 64 bits ou 64 bits Ubuntu Linux 16.04 / 18.04. É executada num ambiente de tempo de execução Java 8 de 64 bits (JRE).

> [!NOTE]
> Para o SDK de Dispositivos de Voz e o dispositivo Roobo, veja [SDK de Dispositivos de Voz](speech-devices-sdk.md).

## <a name="prerequisites"></a>Pré-requisitos

Este início rápido requer:

* Sistema operativo: Windows (64 bits) ou Ubuntu Linux 16.04/18.04 (64-bit)
* [Eclipse Java IDE](https://www.eclipse.org/downloads/)
* [Java 8](https://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html) ou [JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/index.html)
* Uma chave de subscrição do Azure para o serviço de voz. [Obter uma gratuitamente](get-started.md).

Se estiver a executar o Ubuntu 16.04/18.04, certificar-se de que estas dependências estão instaladas antes de iniciar o Eclipse.

```console
sudo apt-get update
sudo apt-get install build-essential libssl1.0.0 libcurl3 libasound2 wget
```

Se estiver executando o Windows (64-bit), certifique-se que instalou o Microsoft Visual C++ Redistributable para a sua plataforma.
* [Baixe o Microsoft Visual C++ Redistributable para Visual Studio 2017](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads)


## <a name="create-and-configure-project"></a>Criar e configurar o projeto

[!INCLUDE [](../../../includes/cognitive-services-speech-service-quickstart-java-create-proj.md)]

## <a name="add-sample-code"></a>Adicionar código de exemplo

1. Para adicionar uma nova classe vazia ao seu projeto Java, selecione **Ficheiro** > **Novo** > **Classe**.

1. Na janela **Nova Classe Java**, introduza **speechsdk.quickstart** no campo **Pacote**, e **Principal** no campo **Nome**.

   ![Captura de ecrã da janela Nova Classe Java](media/sdk/qs-java-jre-06-create-main-java.png)

1. Substitua todo o código em `Main.java` pelo seguinte fragmento:

   [!code-java[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/java-jre/src/speechsdk/quickstart/Main.java#code)]

1. Substitua a cadeia de carateres `YourSubscriptionKey` pela sua chave de subscrição.

1. Substitua a cadeia de carateres `YourServiceRegion` pela [região](regions.md) associada à subscrição (por exemplo, `westus` para a subscrição de avaliação gratuita).

1. Guarde as alterações ao projeto.

## <a name="build-and-run-the-app"></a>Compilar e executar a aplicação

Prima F11 ou selecione **Executar** > **Depurar**.
Os próximos 15 segundos de entrada de voz do microfone serão reconhecidos e registados na janela da consola.

![Captura de ecrã da saída da consola após o reconhecimento bem-sucedido](media/sdk/qs-java-jre-07-console-output.png)

## <a name="next-steps"></a>Passos Seguintes

Exemplos adicionais, tais como a leitura de voz de um arquivo de áudio, estão disponíveis no GitHub.

> [!div class="nextstepaction"]
> [Explore exemplos de Java no GitHub](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Consulte também

- [Quickstart: Translate speech, Java (Windows, Linux)](quickstart-translate-speech-java-jre.md)
- [Personalizar modelos acústicos](how-to-customize-acoustic-models.md)
- [Personalizar modelos de idioma](how-to-customize-language-model.md)
