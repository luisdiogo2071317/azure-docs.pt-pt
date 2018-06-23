---
title: Tutorial de emoções API c# | Microsoft Docs
description: Explore uma aplicação básica do Windows que utiliza a API de emoções serviços cognitivos para reconhecer os emotions expressados pelo faces numa imagem.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: article
ms.date: 01/23/2017
ms.author: anroth
ms.openlocfilehash: f015e5720f65d0951a77de76ce8882a6dcdc1c3b
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352460"
---
# <a name="emotion-api-in-c35-tutorial"></a>Emoções API no C&#35; Tutorial

> [!IMPORTANT]
> Vídeo de API de pré-visualização vai terminar em 30th de Outubro de 2017. Experimentar o novo [pré-visualização do vídeo indexador API](https://azure.microsoft.com/services/cognitive-services/video-indexer/) para extrair facilmente insights de vídeos e a melhorar as experiências de deteção de conteúdos, tais como resultados de pesquisa, através da deteção palavras ditas, faces, carateres e emotions. [Saiba mais](https://docs.microsoft.com/azure/cognitive-services/video-indexer/video-indexer-overview).

Explore uma aplicação básica do Windows que utiliza a API de emoções para reconhecer os emotions expressados pelo faces numa imagem. O abaixo exemplo permite submeter um URL de imagem ou um ficheiro armazenado localmente. Pode utilizar este exemplo de código aberto como um modelo para criar a sua própria aplicação para Windows utilizando a API de emoções e WPF (Windows Presentation Foundation), uma parte do .NET Framework.

## <a name="Prerequisites">Pré-requisitos</a>
#### <a name="platform-requirements"></a>Requisitos de plataforma  
O exemplo abaixo foram desenvolvidas para o .NET Framework utilizando [Visual Studio 2015, edição Community](https://www.visualstudio.com/products/visual-studio-community-vs).  

#### <a name="subscribe-to-emotion-api-and-get-a-subscription-key"></a>Subscrever emoções API e obter uma chave de subscrição  
Antes de criar o exemplo, tem de subscrever emoções API que faz parte dos serviços cognitivos da Microsoft. Consulte [subscrições](https://azure.microsoft.com/try/cognitive-services/). Tanto a chave primária e secundária pode ser utilizada neste tutorial. Certifique-se a seguir as melhores práticas para manter o segredo de chave de API e seguro.  

#### <a name="get-the-client-library-and-example"></a>Instalar o cliente de biblioteca e de exemplo  
Poderá transferi-lo a biblioteca de cliente de API de emoções através de [SDK](https://www.github.com/microsoft/cognitive-emotion-windows). O ficheiro zip transferido tem de ser extraídos para uma pasta à sua escolha, número de utilizadores que escolha a pasta do Visual Studio 2015.
## <a name="Step1">Passo 1: Abra o exemplo</a>
1.  Iniciar o Microsoft Visual Studio 2015 e clique em **ficheiro**, selecione **abra**, em seguida, **projeto/solução**.
2.  Procure a pasta onde guardou os ficheiros transferidos da API de emoções. Clique em **emoções**, em seguida, **Windows**e, em seguida, o **exemplo WPF** pasta.
3.  Faça duplo clique para abrir o ficheiro de solução de 2015 do Visual Studio (. sln) com o nome **EmotionAPI-WPF-Samples.sln**. Esta ação irá abrir a solução no Visual Studio.

## <a name="Step2">Passo 2: Criar o exemplo</a>
1. No **Explorador de soluções**, clique com o botão direito **referências** e selecione **gerir pacotes NuGet**.

  ![Gestor de pacotes Nuget aberta](../Images/EmotionNuget.png)

2.  O **Gestor de pacotes NuGet** é aberta a janela. Comece por selecionar **procurar** no canto superior esquerdo, em seguida, no tipo de caixa de pesquisa "Newtonsoft", selecione o **newtonsoft** do pacote e clique em **instalar**.  

  ![Navegue para o pacote NuGet](../Images/EmotionNugetBrowse.png)  

3.  Prima Ctrl + Shift + B, ou clique em **criar** no menu do Friso, em seguida, selecione **compilar solução**.

## <a name="Step3">Passo 3: Executar o exemplo</a>
1.  Depois de concluída a compilação, prima **F5** ou clique em **iniciar** no menu do friso para executar o exemplo.
2.  Localize a janela de emoções API com o **caixa de texto** ler "**colar a chave de subscrição aqui para começar a**". Cole a chave de subscrição na caixa de texto, conforme mostrado no abaixo captura de ecrã. Pode optar por manter a sua chave de subscrição no seu PC ou portátil ao clicar no botão "Guardar chave". Quando pretender eliminar a chave de subscrição do sistema, clique em "Eliminar chave" para removê-lo a partir do seu PC ou portátil.
  
  ![Interface de funcionalidade de emoções](../Images/EmotionKey.png)

3.  Em "**selecionar cenário**"Clique aqui para utilizar qualquer um dos dois cenários,"**detetar emoções utilizando uma sequência**"ou"**detetar emoções utilizando um URL**", em seguida, siga as instruções o ecrã. Microsoft recebe as imagens de carregar e pode utilizá-los para melhorar a API de emoções e serviços relacionados. Ao submeter uma imagem, confirme que seguiu nosso [programador código de conduta](https://azure.microsoft.com/support/legal/developer-code-of-conduct/).
4.  Existem imagens de exemplo para ser utilizada com esta aplicação de exemplo. Pode encontrar estas imagens no [o repositório do Github da API de rostos em](https://github.com/Microsoft/Cognitive-Face-Windows/tree/master/Data) sob o **dados** pasta. Tenha em atenção que a utilização destas imagens está licenciada nos utilizar justa contrato, que significa que estejam a utilizar para este exemplo de teste, mas não para republishing.

## <a name="Review">Reveja e saiba mais</a>
Agora que tem uma aplicação em execução, informe-nos analise como esta aplicação de exemplo integra-se com os serviços cognitivos da Microsoft. Isto facilitará continuar o edifício para esta aplicação ou desenvolver a sua própria aplicação através da API de emoções da Microsoft. 

Esta aplicação de exemplo permite utilizar a biblioteca de cliente de API de emoções, um dinâmico c# cliente wrapper para a API de emoções Microsoft. Quando criou a aplicação de exemplo, conforme descrito acima, obteve a biblioteca de clientes de um pacote NuGet. Pode rever o código de origem do cliente de biblioteca na pasta intitulada "[biblioteca de clientes](https://github.com/Microsoft/Cognitive-Emotion-Windows/tree/master/ClientLibrary)" em **emoções**, **Windows**, **biblioteca de clientes** , que é parte do repositório ficheiro transferido mencionado na [pré-requisitos](#Prerequisites).
 
Também pode saber como utilizar o código de biblioteca de clientes no **Explorador de soluções**: em **EmotionAPI WPF_Samples**, expanda **DetectEmotionUsingStreamPage.xaml** para localizar **DetectEmotionUsingStreamPage.xaml.cs**, que é utilizado para navegar para um ficheiro armazenado localmente ou expanda **DetectEmotionUsingURLPage.xaml** encontrar  **DetectEmotionUsingURLPage.xaml.cs**, que é utilizada quando carregar um URL de imagem. Faça duplo clique em de. xaml.cs ficheiros para os abrir no windows novo no Visual Studio. 

Rever a forma como a biblioteca de clientes de emoções obtém utilizada na nossa aplicação de exemplo, vamos ver dois fragmentos de código do **DetectEmotionUsingStreamPage.xaml.cs** e **DetectEmotionUsingURLPage.xaml.cs**. Cada ficheiro contém a comentários de código que indica "Chave de exemplo código INICIA aqui" e "Chave de exemplo código TERMINA aqui" para o ajudar a localizar o código de fragmentos reproduzidos abaixo.

A API de emoções é capaz de trabalhar com um URL da imagem ou dados de imagem binário (sob a forma de um fluxo de octeto) como entrada. As duas opções são revistas abaixo. Em ambos os casos, encontrará primeiro um utilizando diretiva, que permite-lhe utilizar a biblioteca de clientes de emoções. 
```csharp

            // ----------------------------------------------------------------------- 
            // KEY SAMPLE CODE STARTS HERE 
            // Use the following namespace for EmotionServiceClient 
            // ----------------------------------------------------------------------- 
            using Microsoft.ProjectOxford.Emotion; 
            using Microsoft.ProjectOxford.Emotion.Contract; 
            // ----------------------------------------------------------------------- 
            // KEY SAMPLE CODE ENDS HERE 
            // ----------------------------------------------------------------------- 
```
#### <a name="detectemotionusingurlpagexamlcs"></a>DetectEmotionUsingURLPage.xaml.cs 

Este fragmento de código mostra como utilizar a biblioteca de cliente para submeter a sua chave de subscrição e um URL de fotografias para o serviço de API de emoções. 

```csharp

            // -----------------------------------------------------------------------
            // KEY SAMPLE CODE STARTS HERE
            // -----------------------------------------------------------------------

            window.Log("EmotionServiceClient is created");

            //
            // Create Project Oxford Emotion API Service client
            //
            EmotionServiceClient emotionServiceClient = new EmotionServiceClient(subscriptionKey);

            window.Log("Calling EmotionServiceClient.RecognizeAsync()...");
            try
            {
                //
                // Detect the emotions in the URL
                //
                Emotion[] emotionResult = await emotionServiceClient.RecognizeAsync(url);
                return emotionResult;
            }
            catch (Exception exception)
            {
                window.Log("Detection failed. Please make sure that you have the right subscription key and proper URL to detect.");
                window.Log(exception.ToString());
                return null;
            }
            // -----------------------------------------------------------------------
            // KEY SAMPLE CODE ENDS HERE
            // -----------------------------------------------------------------------
```
#### <a name="detectemotionusingstreampagexamlcs"></a>DetectEmotionUsingStreamPage.xaml.cs 

Abaixo é como submeter a sua chave de subscrição e uma imagem para a API de emoções armazenada localmente. 


```csharp


            // -----------------------------------------------------------------------
            // KEY SAMPLE CODE STARTS HERE
            // -----------------------------------------------------------------------

            //
            // Create Project Oxford Emotion API Service client
            //
            EmotionServiceClient emotionServiceClient = new EmotionServiceClient(subscriptionKey);

            window.Log("Calling EmotionServiceClient.RecognizeAsync()...");
            try
            {
                Emotion[] emotionResult;
                using (Stream imageFileStream = File.OpenRead(imageFilePath))
                {
                    //
                    // Detect the emotions in the URL
                    //
                    emotionResult = await emotionServiceClient.RecognizeAsync(imageFileStream);
                    return emotionResult;
                }
            }
            catch (Exception exception)
            {
                window.Log(exception.ToString());
                return null;
            }
            // -----------------------------------------------------------------------
            // KEY SAMPLE CODE ENDS HERE
            // -----------------------------------------------------------------------
```
<!--
## <a name="Related">Related Topics</a>
[Emotion API Overview](.)
-->
