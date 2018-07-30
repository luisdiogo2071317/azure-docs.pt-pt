---
title: 'Início rápido: Reconhecer voz em Java no Android com o SDK de voz dos serviços cognitivos'
titleSuffix: Microsoft Cognitive Services
description: Aprender a reconhecer a conversão de voz em Java no Android com o SDK de voz dos serviços cognitivos
services: cognitive-services
author: fmegen
ms.service: cognitive-services
ms.technology: Speech
ms.topic: article
ms.date: 07/16/2018
ms.author: fmegen
ms.openlocfilehash: ad1bf15f198529487dbed92d8f8639704b0c5fc7
ms.sourcegitcommit: 7ad9db3d5f5fd35cfaa9f0735e8c0187b9c32ab1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/27/2018
ms.locfileid: "39325130"
---
# <a name="quickstart-recognize-speech-in-java-on-android-using-the-speech-sdk"></a>Início rápido: Reconhecer voz em Java no Android utilizando o SDK de voz

[!include[Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Neste artigo, aprenderá como criar uma aplicação de Java para Android utilizando o SDK de voz dos serviços cognitivos para transcrição de voz em texto.
A aplicação baseia-se a Microsoft Cognitive Services voz SDK pacote Maven, a versão versão 0.5.0 e o Android Studio 3.1.

> [!NOTE]
> Para obter o SDK de dispositivos de voz e o dispositivo Roobo, aceda a [SDK de dispositivos de voz](speech-devices-sdk.md) página.

## <a name="prerequisites"></a>Pré-requisitos

* Uma chave de subscrição para o serviço de voz. Ver [experimentar gratuitamente o serviço de voz](get-started.md).
* Um PC (Windows, Linux, Mac) com capacidade executar o Android Studio.
* Versão 3.1 da [Android Studio](https://developer.android.com/studio/).
* Um dispositivo Android com base em ARM (API 23: Android 6.0 Marshmallow ou superior) [ativada para o desenvolvimento](https://developer.android.com/studio/debug/dev-options) com um microfone de trabalho.

## <a name="create-an-android-studio-project"></a>Criar um projeto do Android Studio

Inicie o Android Studio e selecione **inicie um novo projeto do Android Studio**.

![](media/sdk/qs-java-android-01-start-new-android-studio-project.png)

Na **criar novo projeto** assistente que vem a cópia de segurança, selecione as seguintes opções:

1. Na **criar projeto Android** ecrã, introduza **guia de início rápido** como **nome da aplicação**, **samples.speech.cognitiveservices.microsoft.com** como **domínio da empresa**e escolha uma localização de projeto. Deixe as caixas de verificação desmarcada e clique em **seguinte**.

   ![](media/sdk/qs-java-android-02-create-android-project.png)

1. Na **dispositivos Android de destino** ecrã, verificação **telemóveis e tablets** como a única opção, escolher **API 23: Android 6.0 (Marshmallow)** na lista suspensa sob e clique em **Seguinte**.

   ![](media/sdk/qs-java-android-03-target-android-devices.png)

1. Na **adicionar uma atividade ao Mobile** ecrã, selecione **atividade vazia** e clique em **seguinte**.

   ![](media/sdk/qs-java-android-04-add-an-activity-to-mobile.png)

1. Na **configurar a atividade** ecrã, utilize **MainActivity** como o nome da atividade e **atividade\_principal** como o nome de Layout. Verifique as duas caixas de seleção e clique em **concluir**.

   ![](media/sdk/qs-java-android-05-configure-activity.png)

Depois de ser executada por um tempo, o seu projeto do Android Studio recentemente criado deve ser exibido.

## <a name="configure-your-project-for-the-speech-sdk"></a>Configurar o seu projeto para o SDK de voz

[!include[License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

A versão atual do SDK de voz dos serviços cognitivos é `0.5.0`.

O SDK de voz para Android é empacotado como uma [AAR (biblioteca do Android)](https://developer.android.com/studio/projects/android-library), que inclui as bibliotecas necessárias, bem como permissões Android necessárias para o utilizar.
Ele está hospedado num repositório Maven em https://csspeechstorage.blob.core.windows.net/maven/.

Descrito abaixo como configurar seu projeto para utilizar o SDK de voz.

Abra a janela de estrutura de projeto sob **arquivo** \> **estrutura do projeto**.
Na janela que surge marca as seguintes alterações (clique em **OK** apenas depois de concluir todos os passos):

1. Selecione **Project**e edite o **repositório de biblioteca predefinida** definições ao acrescentar uma vírgula e o nosso URL do repositório Maven inseridos entre aspas `'https://csspeechstorage.blob.core.windows.net/maven/'`:

  ![](media/sdk/qs-java-android-06-add-maven-repository.png)

1. Ainda no mesmo ecrã, no lado esquerdo, selecione o **App** módulo e na parte superior a **dependências** separador. Em seguida, clique no sinal verde no canto superior direito e selecione **dependência de biblioteca**.

  ![](media/sdk/qs-java-android-07-add-module-dependency.png)

1. Na janela apresentada, introduza o nome e versão do nosso SDK de voz para Android, `com.microsoft.cognitiveservices.speech:client-sdk:0.5.0`, em seguida, clique em **OK**.
   Agora, o SDK de voz deve ser adicionado à lista de dependências conforme mostrado abaixo:

  ![](media/sdk/qs-java-android-08-dependency-added.png)

1. Na parte superior, selecione o **propriedades** separador. Selecione **1.8** ambas para **compatibilidade de origem** e **compatibilidade de destino**.

  ![](media/sdk/qs-java-android-09-dependency-added.png)

1. Por fim, clique em **OK** para fechar a **estrutura do projeto** windows e aplicar todas as atualizações.

## <a name="create-a-minimal-ui"></a>Criar uma interface de Usuário mínimo

Editar o layout de sua atividade principal, `activity_main.xml`.
Por padrão ele deve ser exibido com uma barra de título com o nome da sua aplicação e um TextView que diz "Hello World!".

* Clique no TextView. Alterar o atributo ID no canto superior direito para `hello`.

* Na paleta no canto superior esquerdo de seu `activity_main.xml` janela, arraste um botão para o espaço vazio acima o texto.

* Nos atributos do botão à direita, o valor para o `onClick` atributo, introduza `onSpeechButtonClicked`, que será o nome do nosso manipulador do botão.
  Alterar o atributo ID no canto superior direito para `button`.

* Utilize o ícone de Varinha mágica na parte superior do designer para inferir restrições do layout para.

  ![](media/sdk/qs-java-android-10-infer-layout-constraints.png)

O texto e a versão gráfica da sua interface do Usuário devem agora ser semelhantes ao seguinte:

<table>
<tr>
<td valign="top">
![](media/sdk/qs-java-android-11-gui.png)
</td>
<td valign="top">
[! código xml[](~/samples-cognitive-services-speech-sdk/quickstart/java-android/app/src/main/res/layout/activity_main.xml)]
</td>
</tr>
</table>

## <a name="add-the-sample-code"></a>Adicione o código de exemplo

1. Editar o `MainActivity.java` ficheiro de origem e substitua o seu código com o seguinte (abaixo sua declaração de pacote):

   [!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java-android/app/src/main/java/com/microsoft/cognitiveservices/speech/samples/quickstart/MainActivity.java#code)]

   * O `onCreate` método inclui o código que solicita microfone e permissões da Internet, bem como inicializa o enlace de plataforma nativa. Configurar os enlaces de plataforma nativa é apenas necessário uma vez, ou seja, deve ser feito no início durante a inicialização do aplicativo.
   
   * O método `onSpeechButtonClicked` anteriormente foi configurada como o manipulador de clique de botão. Um pressionamento do botão aciona o reconhecimento de fala real.

1. Substitua a cadeia de caracteres `YourSubscriptionKey` com a sua chave de subscrição.

1. Substitua a cadeia de caracteres `YourServiceRegion` com o [região](regions.md) associados à subscrição (por exemplo, `westus` para a subscrição de avaliação gratuita).

## <a name="build-and-run-the-sample"></a>Criar e executar o exemplo

* Para criar, pressione Ctrl + F9 ou selecione **crie** \> **tornar projeto**.

* Ligar o seu dispositivo Android para o desenvolvimento de PC. Certifique-se de que tem [modo de desenvolvimento e depuração de USB ativada](https://developer.android.com/studio/debug/dev-options).

* Para iniciar a aplicação, prima SHIFT+F10 ou selecione **execute** \> **executar "aplicação"**.

* No windows de destino de implementação que aparece, escolha o seu dispositivo Android.

  ![Inicie a aplicação para depuração](media/sdk/qs-java-android-12-deploy.png)

* A aplicação deve iniciar no seu dispositivo.
  Assim que pressionar o botão, os próximos 15 segundos serão serem reconhecidos e mostrados a interface do Usuário (deve também conseguir ver a resposta na sua janela logcat no Android Studio):

  ![Interface do Usuário após o reconhecimento bem-sucedido](media/sdk/qs-java-android-13-gui-on-device.png)

Nesta captura de ecrã conclui o guia de introdução do Android. O código de exemplo de projeto completo pode ser transferido a partir do repositório de exemplos.

[!include[Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Procure este exemplo no `quickstart/java-android` pasta.

## <a name="next-steps"></a>Passos Seguintes

* [Obtenha os nossos exemplos](speech-sdk.md#get-the-samples)
