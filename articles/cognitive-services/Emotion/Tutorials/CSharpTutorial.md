---
title: 'Tutorial: Reconhecer emoções num rosto numa imagem - API de emoçõesC#'
titlesuffix: Azure Cognitive Services
description: Explore uma aplicação básica do Windows para reconhecer as emoções expressas nos rostos numa imagem.
services: cognitive-services
author: anrothMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: tutorial
ms.date: 01/23/2017
ms.author: anroth
ROBOTS: NOINDEX
ms.openlocfilehash: da605ec4013fb11606f99f3d9a2dcfcfcab00d3b
ms.sourcegitcommit: efcd039e5e3de3149c9de7296c57566e0f88b106
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/10/2018
ms.locfileid: "53163332"
---
# <a name="tutorial-recognize-emotions-on-a-face-in-an-image"></a>Tutorial: Reconheça emoções num rosto numa imagem.

> [!IMPORTANT]
> A API de Emoções vai ser preterida no dia 15 de fevereiro de 2019. A função de reconhecimento de emoções está agora geralmente disponível como parte da [API Face](https://docs.microsoft.com/azure/cognitive-services/face/). 

Explore uma aplicação básica do Windows que utiliza a API de Emoções para reconhecer as emoções expressas nos rostos numa imagem. O exemplo abaixo permite submeter um URL de imagem ou um ficheiro armazenado localmente. Pode utilizar este exemplo de código fonte aberto como um modelo para compilar a sua aplicação para o Windows com a API de Emoções e o WPF (Windows Presentation Foundation), uma parte do .NET Framework.

## <a name="Prerequisites">Pré-requisitos</a>
#### <a name="platform-requirements"></a>Requisitos de plataforma  
O exemplo abaixo foi desenvolvido para o .NET Framework com o [Visual Studio 2015, Community Edition](https://www.visualstudio.com/products/visual-studio-community-vs).  

#### <a name="subscribe-to-emotion-api-and-get-a-subscription-key"></a>Subscrever a API de Emoções e obter uma chave de subscrição  
Antes de criar o exemplo, tem de subscrever a API de Emoções, que faz parte dos Serviços Cognitivos do Azure. Veja [Subscrições](https://azure.microsoft.com/try/cognitive-services/). Ambas as chaves, primária e secundária, podem ser utilizadas neste tutorial. Siga as melhores práticas para manter a chave de API secreta e segura.  

#### <a name="get-the-client-library-and-example"></a>Obter a biblioteca de cliente e o exemplo  
Pode transferir a biblioteca de cliente da API de Emoções através de [SDK](https://www.github.com/microsoft/cognitive-emotion-windows). O ficheiro zip transferido tem de ser extraído para uma pasta à sua escolha, vários utilizadores escolhem a pasta do Visual Studio 2015.
## <a name="Step1">Passo 1: Abra o exemplo</a>
1.  Inicie o Microsoft Visual Studio 2015 e clique em **Ficheiro**, selecione **Abrir**, em seguida, **Projeto/Solução**.
2.  Procure a pasta onde guardou os ficheiros transferidos da API de Emoções. Clique em **Emoções**, **Windows** e, em seguida, na pasta **Sample-WPF**.
3.  Faça duplo clique para abrir o ficheiro de Solução do Visual Studio 2015 (.sln) com o nome **EmotionAPI-WPF-Samples.sln**. A solução irá abrir no Visual Studio.

## <a name="Step2">Passo 2: Criar o exemplo</a>
1. No **Explorador de Soluções**, clique com o botão direito do rato em **Referências** e, em seguida, clique em **Gerir Pacotes NuGet**.

  ![Abrir o Gestor de Pacotes NuGet](../Images/EmotionNuget.png)

2.  A janela **Gestor de Pacotes NuGet** é aberta. Em primeiro lugar, selecione **Procurar** no canto superior esquerdo e, em seguida, na caixa de pesquisa escreva “Newtonsoft.Json”, selecione o pacote **Newtonsoft.Json** e clique em **Instalar**.  

  ![Procurar o pacote NuGet](../Images/EmotionNugetBrowse.png)  

3.  Prima Ctrl+Shift+B ou clique em **Compilar** no menu do friso e, em seguida, selecione **Compilar Solução**.

## <a name="Step3">Passo 3: Executar o exemplo</a>
1.  Depois de a compilação estar concluída, prima **F5** ou clique em **Iniciar** no menu do friso para executar o exemplo.
2.  Localize a janela da API de Emoções com a **caixa de texto** com o texto “**Cole a chave de subscrição aqui para iniciar**”. Cole a chave de subscrição na caixa de texto, conforme mostrado na captura de ecrã abaixo. Pode optar por manter a sua chave de subscrição no seu PC ou portátil ao clicar no botão “Guardar Chave”. Quando pretender eliminar a chave de subscrição do sistema, clique em “Eliminar chave” para removê-la do PC ou computador portátil.

  ![Interface de Funcionalidades de Emoções](../Images/EmotionKey.png)

3.  Em “**Selecionar Cenário**”, clique para utilizar um dos dois cenários, “**Detetar emoções com um fluxo**” ou “**Detetar emoções com um URL**” e siga as instruções no ecrã. A Microsoft recebe as imagens que carregar e pode utilizá-las para melhorar a API de Emoções e os serviços relacionados. Ao submeter uma imagem, certifique-se de que seguiu o nosso [Código de Conduta do Programador](https://azure.microsoft.com/support/legal/developer-code-of-conduct/).
4.  Existem imagens de exemplo para utilização com esta aplicação de exemplo. Pode encontrar estas imagens no [o repositório do GitHub de API de rostos](https://github.com/Microsoft/Cognitive-Face-Windows/tree/master/Data) sob a **dados** pasta. Tenha em atenção que a utilização destas imagens está licenciada ao abrigo do contrato de Utilização Justa, o que significa que são adequadas para utilização em testes deste exemplo, mas não para republicação.

## <a name="Review">Rever e Aprender</a>
Agora que tem uma aplicação em execução, vamos rever como esta aplicação de exemplo se integra nos Serviços Cognitivos da Microsoft. Isto irá permitir que continue a compilar esta aplicação ou que desenvolva a sua própria aplicação através da API de Emoções da Microsoft.

Esta aplicação de exemplo utiliza a Biblioteca de Cliente da API de Emoções, um wrapper de cliente magro em C# para a API de Emoções da Microsoft. Quando criou a aplicação de exemplo, conforme descrito acima, obteve a biblioteca de cliente a partir de um pacote NuGet. Pode rever o código fonte da Biblioteca de Cliente na pasta intitulada “[Biblioteca de Cliente](https://github.com/Microsoft/Cognitive-Emotion-Windows/tree/master/ClientLibrary)” em **Emoções**, **Windows**, **Biblioteca de Cliente**, que faz parte do repositório do ficheiro transferido mencionado acima nos [Pré-requisitos](#Prerequisites).

Pode também descobrir como usar o código da biblioteca de cliente no **Explorador de soluções**: Sob **EmotionAPI WPF_Samples**, expanda **DetectEmotionUsingStreamPage.xaml** para localizar **DetectEmotionUsingStreamPage.xaml.cs**, que é utilizado para navegar para um localmente armazenado o ficheiro ou expanda **DetectEmotionUsingURLPage.xaml** para localizar **DetectEmotionUsingURLPage.xaml.cs**, que é utilizada ao carregar um URL de imagem. Faça duplo clique nos ficheiros. xaml.cs para abri-los numa nova janela do Visual Studio.

Ao rever como a Biblioteca de Cliente de Emoções é utilizada na nossa aplicação de exemplo, vamos analisar dois fragmentos de código de **DetectEmotionUsingStreamPage.xaml.cs** e **DetectEmotionUsingURLPage.xaml.cs**. Cada ficheiro contém comentários ao código que indicam “KEY SAMPLE CODE STARTS HERE” e “KEY SAMPLE CODE ENDS HERE” para ajudá-lo a localizar os fragmentos de código reproduzidos abaixo.

A API de Emoções é capaz de trabalhar com um URL da imagem ou dados de imagem binária (na forma de um fluxo de octeto) como entrada. As duas opções são revistas abaixo. Em ambos os casos, primeiro localiza uma diretiva de utilização, que permite utilizar a Biblioteca de Cliente de Emoções.
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

Este fragmento de código mostra como utilizar a Biblioteca de Cliente para submeter a sua chave de subscrição e um URL de fotografia ao serviço API de Emoções.

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

Abaixo é mostrado como submeter a sua chave de subscrição e uma imagem armazenada localmente à API de Emoções.


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
