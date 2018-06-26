---
title: Começar com o SDK de dispositivos de reconhecimento de voz | Microsoft Docs
description: Pré-requisitos e instruções para começar a trabalhar com o SDK de dispositivos de reconhecimento de voz.
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.technology: speech
ms.topic: article
ms.date: 05/18/2018
ms.author: v-jerkin
ms.openlocfilehash: 32dd295132e26e46d64b01b194b557cae26575ad
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/25/2018
ms.locfileid: "36750800"
---
# <a name="get-started-with-the-speech-devices-sdk"></a>Começar com o SDK de dispositivos de reconhecimento de voz

Este artigo descreve como configurar o seu PC de desenvolvimento e o kit de desenvolvimento do dispositivo de reconhecimento de voz para desenvolver dispositivos preparados para reconhecimento de voz utilizando o SDK de dispositivos de reconhecimento de voz. Em seguida, irá criar e implementar uma aplicação de exemplo para o dispositivo. 

O código de origem para a aplicação de exemplo está incluído com o SDK de dispositivos de reconhecimento de voz e também [está disponível no GitHub](https://github.com/Azure-Samples/Cognitive-Services-Speech-Devices-SDK).

## <a name="prerequisites"></a>Pré-requisitos

Antes de iniciar o desenvolvimento com o SDK de dispositivos de reconhecimento de voz, recolha as informações e terá de software.

* Obter um kit de desenvolvimento [de Roobo](http://ddk.roobo.com/). Kits estão disponíveis com configurações de matriz microfone linear ou circular; Escolha a correta para as suas necessidades.

    |Configuração do kit de desenvolvimento|Localização de orador|
    |-----------------------------|------------|
    |Circular|Qualquer direção do dispositivo|
    |Linear|À frente do dispositivo|

* Obter a versão mais recente do SDK de dispositivos de reconhecimento de voz, incluindo uma aplicação de exemplo Android, o SDK de dispositivos de reconhecimento de voz [transferir site](https://shares.datatransfer.microsoft.com/). Extrair o ficheiro ZIP para uma pasta local (como `C:\SDSDK`).

* Instalar [Android Studio](https://developer.android.com/studio/) e [Vysor](http://vysor.io/download/) no seu PC.

* Obter um serviço de reconhecimento de voz [chave de subscrição](get-started.md). Pode obter uma avaliação gratuita de 30 dias, ou obter uma chave a partir do dashboard do Azure.

* Se pretender utilizar o reconhecimento de intenção do serviço de reconhecimento de voz, subscrever o [serviço compreensão de idiomas](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/) (LUIS) e [obter uma chave de subscrição](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/azureibizasubscription). 

    O utilizador pode [criar um modelo de LUIS simple](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/) ou utilize o exemplo de modelo de LUIS `LUIS-example.json`, disponível a partir do SDK de dispositivos de reconhecimento de voz [transferir site](https://shares.datatransfer.microsoft.com/). Carregar o ficheiro JSON do modelo para o [LUIS portal](https://www.luis.ai/home) clicando **importação nova aplicação** e escolher o ficheiro JSON.

## <a name="set-up-the-development-kit"></a>Configurar o kit de desenvolvimento

1. Plug-in adaptador de energia o kit de desenvolvimento. Um indicador de energia verde deve apresentado sob o painel superior.

1. Ligar o kit de desenvolvimento para um computador com um cabo USB de mini.

    ![ligar o kit de desenvolvimento](media/speech-devices-sdk/qsg-1.jpg)

1. Incluída adequadamente o kit de desenvolvimento.

    |Configuração do kit de desenvolvimento|Orientação|
    |-----------------------------|------------|
    |Circular|Upright, com microfones enfrentam o limite|
    |Linear|No seu lado, com microfones enfrentam que (mostrado abaixo)|

    ![orientação do kit de desenvolvimento linear](media/speech-devices-sdk/qsg-2.jpg)

1. Instale os certificados e o ficheiro de tabela do word (palavra-chave) de reativação e defina as permissões do dispositivo som. Escreva os comandos seguintes numa janela de comandos.

    > [!NOTE]
    > Estes comandos utilizam a Bridge de depuração do Android, `adb.exe`, que faz parte da instalação do Android Studio. Esta ferramenta pode ser encontrada na `C:\Users\[user name]\AppData\Local\Android\Sdk\platform-tools`. Poderá adicionar este diretório para o caminho para o tornar mais cómodo invocar `adb`. Caso contrário, tem de especificar o caminho completo para a instalação da `adb.exe` em cada comando que invoca `adb`.

    ```
    adb push C:\SDSDK\Android-Sample-Release\scripts\roobo_setup.sh /data/ 
    adb shell
    cd /data/ 
    chmod 777 roobo_setup.sh
    ./roobo_setup.sh
    exit
    ```

    > [!TIP]
    > Mute microfone e orador do seu PC. Desta forma, pode ter a certeza de que está a trabalhar com microfones o kit de desenvolvimento e acidentalmente não aciona o dispositivo de áudio do PC.
    
1.  Inicie Vysor no seu computador.

    ![Vysor](media/speech-devices-sdk/qsg-3.png)

1.  O dispositivo deve estar listado em "Choose um dispositivo". Clique em de **vista** botão junto ao mesmo. 
 
1.  Ligar à rede sem fios clicando **definições**, em seguida, **WLAN**.

    ![Vysor WLAN](media/speech-devices-sdk/qsg-4.png)
 
## <a name="run-a-sample-application"></a>Executar uma aplicação de exemplo

Para executar os testes de Roobo e validar a configuração do kit de desenvolvimento, crie e instalar a aplicação de exemplo.

1.  Inicie o Android Studio.

1.  Escolha esta opção abrir um projeto de Android Studio existente.

    ![Android studio - abrir projeto existente](media/speech-devices-sdk/qsg-5.png)
 
1.  Navegue até à `C:\SDSDK\Android-Sample-Release\example`, em seguida, clique em **OK** para abrir o projeto de exemplo.
 
1.  Adicione a chave de subscrição de reconhecimento de voz para o código de origem. Se pretender experimentar o reconhecimento intenção, adicione também o [serviço compreensão de idiomas](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/) ID da chave de subscrição e a aplicação. 

    As chaves e informações sobre a aplicação fica nas seguintes linhas no ficheiro de origem `MainActivity.java`.

    ```java
    // Subscription
    private static final String SpeechSubscriptionKey = "[your speech key]";
    private static final String SpeechRegion = "westus";
    private static final String LuisSubscriptionKey = "[your LUIS key]";
    private static final String LuisRegion = "westus2.api.cognitive.microsoft.com";
    private static final String LuisAppId = "[your LUIS app id]"
    ```

1. A palavra de reativação predefinido (palavra-chave) é o "Computador".  Se assim o desejar, pode tentar uma das outras fornecido reativação palavras, "Máquina" e "Através do assistente." Os ficheiros de recursos para estas palavras alternativos podem ser encontrados no SDK de dispositivos de reconhecimento de voz na pasta "palavra-chave". Por exemplo, `C:\SDSDK\Android-Sample-Release\keyword\Computer` contém os ficheiros utilizados para "Computador".

    Poderá também [criar uma palavra de reativação personalizado](speech-devices-sdk-create-kws.md).

    Para instalar o word reativação pretendido:
 
    * Criar um `keyword` pasta na pasta \data\ no dispositivo, executando os comandos seguintes numa janela de comando.

        ```
        adb shell
        cd /data
        mkdir keyword
        exit
        ```

    * Copie os ficheiros `kws.table`, `kws_g.fst`, `kws_k.fst`, e `words_kw.txt`) para a pasta de \data\keyword\ do dispositivo. Execute os seguintes comandos numa janela de comando Nã.

        ```
        adb push C:\SDSDK\Android-Sample-Release\keyword\kws.table /data/keyword
        adb push C:\SDSDK\Android-Sample-Release\keyword\Computer\kws_g.fst /data/keyword
        adb push C:\SDSDK\Android-Sample-Release\keyword\Computer\kws_k.fst /data/keyword
        adb push C:\SDSDK\Android-Sample-Release\keyword\Computer\words_kw.txt /data/keyword
        ```
    
    * Estes ficheiros da aplicação de exemplo de referência. Localizar as seguintes linhas no `MainActivity.java`. Certifique-se de que a palavra-chave especificada é aquele que está a utilizar e que o caminho aponta para o `kws.table` ficheiro instalado no dispositivo.
        
        ```java
        private static final String Keyword = "Computer";
        private static final String KeywordModel = "/data/keyword/kws.table";
        ```

        > [!NOTE]
        > No seu próprio código, pode utilizar o `kws.table` ficheiros para criar uma instância do modelo de palavra-chave e iniciar o reconhecimento da seguinte forma.
        >
        > ```java
        > KeywordRecognitionModel km = KeywordRecognitionModel.fromFile(KeywordModel);
        > final Task<?> task = reco.startKeywordRecognitionAsync(km);
        > ```

1.  Atualize as seguintes linhas que contém as definições de geometria de matriz microfone.

    ```java
    private static final String DeviceGeometry = "Circular6+1";
    private static final String SelectedGeometry = "Circular6+1";
    ```

    |Variável|Significado|Valores disponíveis|
    |--------|-------|----------------|
    |`DeviceGeometry`|Configuração do mic físico|`Circular6+1` para o kit de desenvolvimento circular|
    ||| `Linear4` para o kit de desenvolvimento linear|
    |`SelectedGeometry`|Configuração de dinâmicas de software|`Circular6+1` para o kit de desenvolvimento circular mics todas as a utilizar|
    |||`Circular3+1` para o kit de desenvolvimento circular utilizando quatro mics|
    |||`Linear4` para o kit de desenvolvimento linear mics todas as a utilizar|
    |||`Linear2` para o kit de desenvolvimento linear utilizando dois mics|


1.  Criar a aplicação escolhendo **executar "aplicação"** no menu de execução. É apresentada a caixa de diálogo Selecionar destino da implementação. Escolha o seu dispositivo e clique em **OK** para implementar a aplicação no dispositivo.

    ![Selecione o destino da implementação](media/speech-devices-sdk/qsg-7.png)
 
1.  Inicia a aplicação de exemplo do SDK de dispositivos de reconhecimento de voz a apresentar as opções apresentadas aqui.

    ![exemplo de aplicação de dispositivo de reconhecimento de voz](media/speech-devices-sdk/qsg-8.png)

1. Testem à volta!

## <a name="troubleshooting"></a>Resolução de problemas

Se obtiver falhas de certificado ao utilizar o serviço de reconhecimento de voz, certifique-se que o dispositivo tem a correta data e hora.

Para obter mais informações de desenvolvimento, consulte o artigo do Roobo [guia de programação](http://dwn.roo.bo/server_upload/ddk/ROOBO%20Dev%20Kit-User%20Guide.pdf).

Roobo fornece uma ferramenta que capture todos os áudio para flash memória, o que pode ajudar a resolver problemas de áudio. Uma versão da ferramenta é fornecida para cada configuração do kit de desenvolvimento. Escolha o seu dispositivo no [Roobo site](http://ddk.roobo.com/), em seguida, clique em de **ROOBO ferramentas** ligação na parte inferior da página.
