---
title: Começar com o SDK de dispositivos de voz
description: Pré-requisitos e instruções para começar a trabalhar com o SDK de dispositivos de voz.
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.technology: speech
ms.topic: article
ms.date: 05/18/2018
ms.author: v-jerkin
ms.openlocfilehash: 463a015b7c01dafc5b30de56b95fa0510ffb98e4
ms.sourcegitcommit: fab878ff9aaf4efb3eaff6b7656184b0bafba13b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/22/2018
ms.locfileid: "42424374"
---
# <a name="get-started-with-the-speech-devices-sdk"></a>Começar com o SDK de dispositivos de voz

Este artigo descreve como configurar o desenvolvimento de PC e o seu kit de desenvolvimento de dispositivo de voz para o desenvolvimento de dispositivos com capacidade de voz com o SDK de dispositivos de voz. Em seguida, irá criar e implementar uma aplicação de exemplo para o dispositivo. 

O código-fonte da aplicação de exemplo é incluído com o SDK de dispositivos de voz e também [está disponível no GitHub](https://github.com/Azure-Samples/Cognitive-Services-Speech-Devices-SDK).

## <a name="prerequisites"></a>Pré-requisitos

Antes de iniciar o desenvolvimento com o SDK de dispositivos de voz, recolha as informações e precisará de software.

* Obter um kit de desenvolvimento [partir Roobo](http://ddk.roobo.com/). Kits estão disponíveis com configurações de matriz de microfone linear ou circular; Escolha o método correto para as suas necessidades.

    |Configuração do kit de desenvolvimento|Localização de orador|
    |-----------------------------|------------|
    |Circular|Qualquer direção do dispositivo|
    |Linear|À frente do dispositivo|

* Obter a versão mais recente do SDK de dispositivos de voz, incluindo uma aplicação de exemplo do Android, o SDK de dispositivos de voz [site de download](https://shares.datatransfer.microsoft.com/). Extraia o ficheiro ZIP para uma pasta local (como `C:\SDSDK`).

* Instale [Android Studio](https://developer.android.com/studio/) e [Vysor](http://vysor.io/download/) no seu PC.

* Obter um serviço de voz [chave de subscrição](get-started.md). Pode obter uma avaliação gratuita de 30 dias ou obter uma chave a partir do dashboard do Azure.

* Se pretender utilizar o reconhecimento da intenção do serviço de voz, subscrever o [serviço de compreensão de idiomas](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/) (LUIS) e [obter uma chave de subscrição](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/azureibizasubscription). 

    Poderá [criar um modelo simples do LUIS](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/) ou utilize o exemplo de modelo do LUIS, `LUIS-example.json`, disponível a partir do SDK de dispositivos de voz [site de download](https://shares.datatransfer.microsoft.com/). Carregar o ficheiro JSON de seu modelo para o [portal de LUIS](https://www.luis.ai/home) ao clicar em **importar a nova aplicação** e escolher o ficheiro JSON.

## <a name="set-up-the-development-kit"></a>Configurar o kit de desenvolvimento

1. Dê um kit de desenvolvimento usando um cabo USB de mini ligada a um PC ou um adptor de energia. Um indicador de power verde deve iluminam sob o quadro superior.

1. Ligar o kit de desenvolvimento para um computador usando um cabo USB de mini segundo.

    ![ligar o kit de desenvolvimento](media/speech-devices-sdk/qsg-1.png)

1. Orientar o seu kit de desenvolvimento adequadamente.

    |Configuração do kit de desenvolvimento|Orientação|
    |-----------------------------|------------|
    |Circular|Upright, com microfones voltada para o limite|
    |Linear|No seu lado, com microfones voltada para (mostrado abaixo)|

    ![orientação do kit de programação linear](media/speech-devices-sdk/qsg-2.png)

1. Instale os certificados e o arquivo de tabela do word (palavra-chave) de reativação e definir as permissões do dispositivo de som. Escreva os seguintes comandos numa janela de comando.

    > [!NOTE]
    > Estes comandos utilizem a ponte de depuração do Android, `adb.exe`, que faz parte da instalação do Android Studio. Essa ferramenta pode ser encontrada no `C:\Users\[user name]\AppData\Local\Android\Sdk\platform-tools`. Pode adicionar esse diretório ao seu caminho para que seja mais conveniente para invocar `adb`. Caso contrário, tem de especificar o caminho completo para a instalação do `adb.exe` em todos os comandos que invoquem `adb`.

    ```
    adb push C:\SDSDK\Android-Sample-Release\scripts\roobo_setup.sh /data/ 
    adb shell
    cd /data/ 
    chmod 777 roobo_setup.sh
    ./roobo_setup.sh
    exit
    ```

    > [!TIP]
    > Mudo microfone e palestrante do seu PC. Dessa forma, pode ter-se de que está a trabalhar com microfones o kit de desenvolvimento, e acidentalmente não aciona o dispositivo com o áudio do PC.
    
1.  Inicie Vysor no seu computador.

    ![Vysor](media/speech-devices-sdk/qsg-3.png)

1.  O dispositivo deve estar listado em "Selecione um dispositivo". Clique nas **vista** botão junto ao mesmo. 
 
1.  Ligar à sua rede sem fio ao clicar no ícone de pasta, em seguida, **configurações**, em seguida, **WLAN**.

    ![Vysor WLAN](media/speech-devices-sdk/qsg-4.png)
 
 > [!NOTE]
 > Se a sua empresa tem políticas sobre os dispositivos de ligação para o sistema de Wi-Fi, terá de obter o endereço Mac e contacte o seu departamento de TI sobre como ligá-lo ao seu sistema de Wi-Fi. Para localizar o endereço Mac do kit de desenvolvimento, clique no ícone de pasta do arquivo na área de trabalho do kit de desenvolvimento, em seguida, **configurações**, procure "Endereço Mac", clique em **endereço Mac** entrar **WLAN avançada** , anote o endereço Mac encontrado em direção à parte inferior. Além disso, algumas empresas podem ter um tempo limite de quanto tempo um dispositivo pode ser ligado aos seus sistemas de Wi-Fi. Poderá ter de expandir o registo do kit de desenvolvimento com o seu sistema de Wi-Fi, após um determinado número de dias.  
 
 
   ![Pasta do ficheiro Vysor](media/speech-devices-sdk/qsg-10.png)
   
   ![Endereço Mac de Vysor](media/speech-devices-sdk/qsg-11.png)
   
   
 > Se quiser anexar um palestrante para o kit de desenvolvimento, pode ligá-la para a linha de áudio horizontalmente. Também deve escolher um palestrante de 3,5 mm de boa qualidade.
 
   ![Vysor áudio](media/speech-devices-sdk/qsg-14.png)
 
## <a name="run-a-sample-application"></a>Executar uma aplicação de exemplo

Para executar os testes de Roobo e validar a configuração do kit de desenvolvimento, criar e instalar a aplicação de exemplo.

1.  Execute o Android Studio.

1.  Opte por abrir um projeto existente do Android Studio.

    ![Android studio - abrir projeto existente](media/speech-devices-sdk/qsg-5.png)
 
1.  Navegue até `C:\SDSDK\Android-Sample-Release\example`, em seguida, clique em **OK** para abrir o projeto de exemplo.
 
1.  Adicione a chave de subscrição de voz ao código-fonte. Se quiser experimentar o reconhecimento da intenção, adicione também seu [serviço de compreensão de idiomas](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/) ID da chave de subscrição e a aplicação. 

    As suas chaves e informações sobre a aplicação vai nas seguintes linhas no arquivo de origem `MainActivity.java`.

    ```java
    // Subscription
    private static final String SpeechSubscriptionKey = "[your speech key]";
    private static final String SpeechRegion = "westus";
    private static final String LuisSubscriptionKey = "[your LUIS key]";
    private static final String LuisRegion = "westus2.api.cognitive.microsoft.com";
    private static final String LuisAppId = "[your LUIS app id]"
    ```

1. A palavra de reativação de predefinição (palavra-chave) é o "Computador".  Se desejar, pode tentar uma das outras fornecido reativação palavras, "Máquina" e "Assistente". Os ficheiros de recursos para essas palavras alternativos podem ser encontrados no SDK de dispositivos de voz na pasta "palavra-chave". Por exemplo, `C:\SDSDK\Android-Sample-Release\keyword\Computer` contém os arquivos usados para "Computador".

    Também poderá [criar uma palavra de reativação personalizado](speech-devices-sdk-create-kws.md).

    Para instalar o word reativação pretendido:
 
    * Criar um `keyword` pasta na pasta \data\ no dispositivo, executando os seguintes comandos na janela de comando.

        ```
        adb shell
        cd /data
        mkdir keyword
        exit
        ```

    * Copie os ficheiros `kws.table`, `kws_g.fst`, `kws_k.fst`, e `words_kw.txt`) para a pasta de \data\keyword\ do dispositivo. Execute os seguintes comandos na janela de comando de s. Se tiver criado uma [word personalizado de reativação](speech-devices-sdk-create-kws.md), o ficheiro de kws.table gerado a partir do web vai estar no mesmo diretório `kws.table`, `kws_g.fst`, `kws_k.fst`, e `words_kw.txt` arquivos são. Utilize adb push C:\SDSDK\Android-Sample-Release\keyword\[wake_word_name]\kws.table/dados/palavra-chave este comando para enviar o ficheiro de kws.table para o kit de desenvolvimento em vez disso.

        ```
        adb push C:\SDSDK\Android-Sample-Release\keyword\kws.table /data/keyword
        adb push C:\SDSDK\Android-Sample-Release\keyword\Computer\kws_g.fst /data/keyword
        adb push C:\SDSDK\Android-Sample-Release\keyword\Computer\kws_k.fst /data/keyword
        adb push C:\SDSDK\Android-Sample-Release\keyword\Computer\words_kw.txt /data/keyword
        ```
    
    * Estes ficheiros no aplicativo de exemplo de referência. Encontrar as seguintes linhas no `MainActivity.java`. Certifique-se de que a palavra-chave especificada é o que está a utilizar e que o caminho aponta para o `kws.table` ficheiro que enviou para o dispositivo.
        
        ```java
        private static final String Keyword = "Computer";
        private static final String KeywordModel = "/data/keyword/kws.table";
        ```

        > [!NOTE]
        > No seu próprio código, pode usar o `kws.table` ficheiro para criar uma instância do modelo de palavra-chave e iniciar o reconhecimento da seguinte forma.
        >
        > ```java
        > KeywordRecognitionModel km = KeywordRecognitionModel.fromFile(KeywordModel);
        > final Task<?> task = reco.startKeywordRecognitionAsync(km);
        > ```

1.  Atualize as seguintes linhas que contém as definições de geometria de matriz de microfone.

    ```java
    private static final String DeviceGeometry = "Circular6+1";
    private static final String SelectedGeometry = "Circular6+1";
    ```

    |Variável|Significado|Valores disponíveis|
    |--------|-------|----------------|
    |`DeviceGeometry`|Configuração de mic físico|`Circular6+1` Kit de desenvolvimento circular|
    ||| `Linear4` para o kit de programação linear|
    |`SelectedGeometry`|Configuração do software do mic|`Circular6+1` Kit de desenvolvimento circular com todos os microfones|
    |||`Circular3+1` Kit de desenvolvimento circular com quatro microfones|
    |||`Linear4` para o kit de programação linear com todos os microfones|
    |||`Linear2` para o kit de programação linear usando duas microfones|


1.  Criar a aplicação ao escolher **executar "aplicação"** no menu de execução. É apresentada a caixa de diálogo Selecionar destino da implementação. Escolha o seu dispositivo e clique em **OK** para implementar a aplicação no dispositivo.

    ![Selecione o destino da implementação](media/speech-devices-sdk/qsg-7.png)
 
1.  O aplicativo de exemplo do SDK de dispositivos de fala é iniciado, exibindo as opções mostradas aqui.

    ![exemplo de aplicação de dispositivo de voz](media/speech-devices-sdk/qsg-8.png)

1. Trabalhe com ele!

## <a name="troubleshooting"></a>Resolução de problemas

Se obtiver falhas de certificado ao utilizar o serviço de voz, certifique-se de que o dispositivo tem a data e hora corretas. Aceda a **configurações**, clique em **data e hora** no sistema, e **selecione fuso de horário** para ser o seu fuso horário atual. Manter **automática data e hora** ON. Quando vir o tempo do kit de desenvolvimento corresponde ao tempo de seu PC, em seguida, saberá o kit de desenvolvimento está ligado à internet. 

 ![Pasta do ficheiro Vysor](media/speech-devices-sdk/qsg-12.png)
 
 ![Pasta do ficheiro Vysor](media/speech-devices-sdk/qsg-13.png)

Para obter mais informações de desenvolvimento, consulte do Roobo [guia de desenvolvimento](http://dwn.roo.bo/server_upload/ddk/ROOBO%20Dev%20Kit-User%20Guide.pdf).

Roobo fornece uma ferramenta que captura todos os áudio Flash memória, que pode ajudar na resolução de problemas de áudio. Uma versão da ferramenta é fornecida para cada configuração de kit de desenvolvimento. Escolha o seu dispositivo no [o site de Roobo](http://ddk.roobo.com/), em seguida, clique no **ROOBO ferramentas** link na parte inferior da página.
