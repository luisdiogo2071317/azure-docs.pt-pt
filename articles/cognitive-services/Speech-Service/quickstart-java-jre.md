---
title: 'Início rápido: Reconhecer voz em Java (Windows ou Linux)'
titleSuffix: Microsoft Cognitive Services
description: Aprender a reconhecer a conversão de voz em Java (Windows ou Linux)
services: cognitive-services
author: fmegen
ms.service: cognitive-services
ms.technology: Speech
ms.topic: article
ms.date: 08/16/2018
ms.author: fmegen
ms.openlocfilehash: 11aba1ae6f49d6c00fabd928ae3aefedcbea8ed8
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/20/2018
ms.locfileid: "40234353"
---
# <a name="quickstart-recognize-speech-in-java-windows-or-linux"></a>Início rápido: Reconhecer voz em Java (Windows ou Linux)

[!include[Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Este documento descreve como criar um aplicativo de console baseados em Java para o Java Runtime Environment (JRE) que utiliza o SDK de voz.
A aplicação baseia-se o pacote Maven do Microsoft Cognitive Services SDK.
Vamos utilizar o Eclipse como um ambiente (IDE de desenvolvimento integrado).

## <a name="prerequisites"></a>Pré-requisitos

* Uma chave de subscrição para o serviço de voz. Ver [experimentar gratuitamente o serviço de voz](get-started.md).
* Um PC (Windows x64, Ubuntu 16.04 x64) com capacidade executar o Eclipse, com um microfone de trabalho.
* 64-bit JRE/JDK para Java 8.
* 4.8 de versão do [Eclipse](https://www.eclipse.org), versão de 64 bits.
* No Ubuntu 16.04, execute os seguintes comandos para a instalação de pacotes necessários:

  ```sh
  sudo apt-get update
  sudo apt-get install build-essential libssl1.0.0 libcurl3 libasound2 wget
  ```

## <a name="create-and-configure-your-project"></a>Criar e configurar o seu projeto

1. Abra o Eclipse.

1. No iniciador do Eclipse, introduza o nome de um novo diretório para o **área de trabalho** campo.
   Em seguida, clique em **inicie**.

   ![Criar área de trabalho do Eclipse](media/sdk/qs-java-jre-01-create-new-eclipse-workspace.png)

1. Depois de um tempo, a janela principal do IDE do Eclipse aparece.
   Se houver um ecrã de boas-vindas nela, fechá-la.

1. Selecione **arquivo** \> **nova** \> **projeto**.

1. Na **novo projeto** assistente que aparece, selecione **projeto Java**e clique em **seguinte**.

   ![Selecione um assistente](media/sdk/qs-java-jre-02-select-wizard.png)

1. Na janela seguinte, introduza **início rápido** como um projeto dê um nome e escolha **JavaSE 1.8** (ou até) como ambiente de execução.
   Clique em **Concluir**.

   ![Selecione um assistente](media/sdk/qs-java-jre-03-create-java-project.png)

1. Se uma janela intitulada **abrir perspetiva associada?** é exibido, selecione **abrir perspetiva**.

1. Na **Explorador de pacotes**, clique com botão direito a **início rápido** do projeto e selecione **configurar** \> **converter em projeto Maven**.

   ![Converter em projeto Maven](media/sdk/qs-java-jre-04-convert-to-maven-project.png)

1. Na janela de que é apresentado, introduza **com.microsoft.cognitiveservices.speech.samples** como **Id do grupo** e **início rápido** como **Id do artefacto**. Selecione **Concluir**.

   ![Configure o Maven POM](media/sdk/qs-java-jre-05-configure-maven-pom.png)

1. Editar a **pom** ficheiro.

  * No final do ficheiro, antes da tag de fecho `</project>`, crie uma seção de repositórios com uma referência para o repositório Maven para o SDK de voz:

    [!code-xml[POM Repositories](~/samples-cognitive-services-speech-sdk/quickstart/java-jre/pom.xml#repositories)]

  * Além disso, adicione posteriormente uma secção de dependências com o SDK de voz versão 0.6.0 como uma dependência:

    [!code-xml[POM Dependencies](~/samples-cognitive-services-speech-sdk/quickstart/java-jre/pom.xml#dependencies)]

  * Guarde as alterações.

## <a name="add-the-sample-code"></a>Adicione o código de exemplo

1. Selecione **arquivo** \> **New** \> **classe** para adicionar uma nova classe vazia ao seu projeto de Java.

1. Na janela **nova classe de Java** introduza **speechsdk.quickstart** para o **pacote** campo, e **Main** no **nome**  campo.

   ![Criação de uma classe principal](media/sdk/qs-java-jre-06-create-main-java.png)

1. Substitua todo o código `Main.java` pelo seguinte fragmento:

   [!code-java[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/java-jre/src/speechsdk/quickstart/Main.java#code)]

1. Substitua a cadeia de caracteres `YourSubscriptionKey` com a sua chave de subscrição.

1. Substitua a cadeia de caracteres `YourServiceRegion` com o [região](regions.md) associados à subscrição (por exemplo, `westus` para a subscrição de avaliação gratuita).

1. Guarde as alterações ao projeto.

## <a name="build-and-run-the-sample"></a>Criar e executar o exemplo

Prima F11 ou selecione **execute** \> **depurar**.
Próximos 15 segundos de entrada de voz de seu microfone serão reconhecidos e com sessão iniciados na janela da consola.

![Resultado da consola após o reconhecimento bem-sucedido](media/sdk/qs-java-jre-07-console-output.png)

[!include[Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Procure este exemplo no `quickstart/java-jre` pasta.

## <a name="next-steps"></a>Passos Seguintes

* [Obtenha os nossos exemplos](speech-sdk.md#get-the-samples)
