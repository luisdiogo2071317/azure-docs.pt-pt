---
title: Tutorial de computador visão API c# | Microsoft Docs
description: Explore uma aplicação básica do Windows que utiliza a API de visão de computador nos serviços cognitivos da Microsoft. Efetuar OCR, criar miniaturas e trabalhar com funcionalidades visual numa imagem.
services: cognitive-services
author: KellyDF
manager: corncar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: article
ms.date: 05/22/2017
ms.author: kefre
ms.openlocfilehash: f2aeb1734f8858ed8b80e5cdf48ef7e558703919
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352892"
---
# <a name="computer-vision-api-c35-tutorial"></a>Computador visão API C&#35; Tutorial

Explore uma aplicação básica do Windows que utiliza a API de visão do computador efetuar optical caráter reconhecimento (OCR), crie smart-recortada miniaturas plus detetar, categorizar tag e descreve visual funcionalidades, incluindo faces, numa imagem. O abaixo exemplo permite submeter um URL de imagem ou um ficheiro armazenado localmente. Pode utilizar este exemplo de código aberto como um modelo para criar a sua própria aplicação para Windows utilizando a API de visão e WPF (Windows Presentation Foundation), uma parte do .NET Framework.

### <a name="prerequisites"></a>Pré-requisitos

#### <a name="platform-requirements"></a>Requisitos de plataforma

O exemplo abaixo foram desenvolvidas para o .NET Framework utilizando [Visual Studio 2015, edição Community](https://www.visualstudio.com/downloads/).

#### <a name="subscribe-to-computer-vision-api-and-get-a-subscription-key"></a>Subscrever a API de visão do computador e obter uma chave de subscrição 

Antes de criar o exemplo, tem de subscrever API de visão do computador que faz parte dos serviços cognitivos da Microsoft (anteriormente Oxford de projeto). Para detalhes de gestão de chaves e da subscrição, consulte [subscrições](https://azure.microsoft.com/try/cognitive-services/). Tanto a chave primária e secundária pode ser utilizada neste tutorial. 

> [!NOTE]
> O tutorial foi concebido para utilizar chaves de subscrição no **westcentralus** região. As chaves de subscrição geradas na utilização de avaliação gratuita visão do computador a **westcentralus** região, para que funcionem corretamente. Se gerou as chaves de subscrição com a sua conta do Azure através de [ https://azure.microsoft.com/ ](https://azure.microsoft.com/), tem de especificar o **westcentralus** região. Chaves geradas fora do **westcentralus** região não funcionará.

#### <a name="get-the-client-library-and-example"></a>Instalar o cliente de biblioteca e de exemplo

Pode clonar a aplicação de exemplo e biblioteca cliente de API de visão do computador para o seu computador através de [SDK](https://www.github.com/microsoft/cognitive-vision-windows). Não transfira-o como um ZIP.

### <a name="Step1">Passo 1: Instalar o exemplo</a>

No seu ambiente de trabalho do GitHub, abra o exemplo-WPF\VisionAPI WPF Samples.sln.

### <a name="Step2">Passo 2: Criar o exemplo</a>

* Prima Ctrl + Shift + B, ou clique em compilação no menu do Friso e, em seguida, selecione compilar solução.

### <a name="Step3">Passo 3: Executar o exemplo</a>

1. Depois de concluída a compilação, prima **F5** ou clique em **iniciar** no menu do friso para executar o exemplo.
2. Localize a janela de interface de utilizador do computador visão API com a caixa de edição de texto ler "Colar a chave de subscrição aqui para iniciar".
Pode optar por manter a sua chave de subscrição no seu PC ou portátil ao clicar no botão "Guardar chave". Quando pretender eliminar a chave de subscrição do sistema, clique em "Eliminar chave" para removê-lo a partir do seu PC ou portátil.

    ![Chave de subscrição de visão](../Images/Vision_UI_Subscription.PNG)

3. Em "Selecionar cenário" clique para utilizar um dos seis cenários, em seguida, siga as instruções no ecrã. Microsoft recebe as imagens de carregar e pode utilizá-los para melhorar a API de visão do computador e serviços relacionados. Ao submeter uma imagem, confirme que seguiu nosso [programador código de conduta](https://azure.microsoft.com/support/legal/developer-code-of-conduct/).

    ![Analisar a Interface de imagem](../Images/Analyze_Image_Example.PNG)

4. Existem imagens de exemplo para ser utilizada com esta aplicação de exemplo. Pode encontrar estas imagens no repositório do Github de Windows de API de letra, além de [pasta dados](https://github.com/Microsoft/Cognitive-Face-Windows/tree/master/Data). Tenha em atenção a utilização destas imagens está licenciada sob contrato [licença imagem](https://github.com/Microsoft/Cognitive-Face-Windows/blob/master/LICENSE-IMAGE.md).

### <a name="Review">Reveja e saiba mais</a>

Agora que tem uma aplicação em execução, informe-nos analise como esta aplicação de exemplo integra-se com a tecnologia de serviços cognitivos. Isto facilitará continuar edifício para esta aplicação ou para desenvolver as suas próprias aplicações através da API de visão de computador do Microsoft.

Esta aplicação de exemplo permite utilizar da biblioteca de cliente de API de visão de computador, um dinâmico c# cliente wrapper para a API de visão de computador do Microsoft. Quando criou a aplicação de exemplo, conforme descrito acima, obteve a biblioteca de clientes de um pacote NuGet. Pode rever o código de origem do cliente de biblioteca na pasta intitulada "**biblioteca de clientes**" em **visão**, **Windows**, **biblioteca de clientes**, que faz parte do repositório ficheiro transferido mencionado na pré-requisitos.

Também pode saber como utilizar o código de biblioteca de clientes no Explorador de soluções: em **VisionAPI WPF_Samples**, expanda **AnalyzePage.xaml** localizar **AnalyzePage.xaml.cs**, que é utilizado para submeter uma imagem para o ponto final de análise de imagem. Faça duplo clique em de. xaml.cs ficheiros para os abrir no windows novo no Visual Studio.

Rever a forma como a biblioteca de clientes de visão obtém utilizada na nossa aplicação de exemplo, vamos ver dois fragmentos de código do **AnalyzePage.xaml.cs**. O ficheiro contém a comentários de código que indica "Chave de exemplo código INICIA aqui" e "Chave de exemplo código TERMINA aqui" para o ajudar a localizar o código de fragmentos reproduzidos abaixo.

O ponto final de analisar é capaz de trabalhar com um URL da imagem ou dados de imagem binário (sob a forma de um fluxo de octeto) como entrada. Em primeiro lugar, encontrará uma utilizando diretiva, que permite-lhe utilizar a biblioteca de clientes de visão.

```
                // ----------------------------------------------------------------------
                // KEY SAMPLE CODE STARTS HERE
                // Use the following namespace for VisionServiceClient 
                // ---------------------------------------------------------------------- 
                using Microsoft.ProjectOxford.Vision; 
                using Microsoft.ProjectOxford.Vision.Contract; 
                // ----------------------------------------------------------------------
                // KEY SAMPLE CODE ENDS HERE 
                // ----------------------------------------------------------------------

```
**UploadAndAnalyzeImage(...)**  Este fragmento de código mostra como utilizar a biblioteca de clientes para submeter a sua chave de subscrição e uma imagem de armazenados a nível local para o ponto final de análise do serviço de API de visão do computador.

```
    private async Task<AnalysisResult> UploadAndAnalyzeImage(string imageFilePath)
    {
        // -----------------------------------------------------------------------
        // KEY SAMPLE CODE STARTS HERE
        // -----------------------------------------------------------------------  
        //
        // Create Project Oxford Computer Vision API Service client
        //
        VisionServiceClient VisionServiceClient = new VisionServiceClient(SubscriptionKey);
        Log("VisionServiceClient is created");
    
        using (Stream imageFileStream = File.OpenRead(imageFilePath))
        {
            //
            // Analyze the image for all visual features
            //
            Log("Calling VisionServiceClient.AnalyzeImageAsync()...");
         VisualFeature[] visualFeatures = new VisualFeature[] { VisualFeature.Adult, VisualFeature.Categories, VisualFeature.Color, VisualFeature.Description, VisualFeature.Faces, VisualFeature.ImageType, VisualFeature.Tags };
            AnalysisResult analysisResult = await VisionServiceClient.AnalyzeImageAsync(imageFileStream, visualFeatures);
            return analysisResult;
        }
    
        // -----------------------------------------------------------------------
        // KEY SAMPLE CODE ENDS HERE
        // -----------------------------------------------------------------------
        }
```
**AnalyzeUrl(...)**  Este fragmento de código mostra como utilizar a biblioteca de clientes para submeter a sua chave de subscrição e um URL de fotografias para o ponto final de análise do serviço de API de visão do computador.

```
    private async Task<AnalysisResult> AnalyzeUrl(string imageUrl)
    {
        // -----------------------------------------------------------------------
        // KEY SAMPLE CODE STARTS HERE
        // -----------------------------------------------------------------------
    
        //
        // Create Project Oxford Computer Vision API Service client
        //
     VisionServiceClient VisionServiceClient = new VisionServiceClient(SubscriptionKey);
        Log("VisionServiceClient is created");
    
        //
        // Analyze the url for all visual features
        //
        Log("Calling VisionServiceClient.AnalyzeImageAsync()...");
        VisualFeature[] visualFeatures = new VisualFeature[] { VisualFeature.Adult, VisualFeature.Categories, VisualFeature.Color, VisualFeature.Description, VisualFeature.Faces, VisualFeature.ImageType, VisualFeature.Tags };
        AnalysisResult analysisResult = await VisionServiceClient.AnalyzeImageAsync(imageUrl, visualFeatures);
     return analysisResult;
    }
        // -----------------------------------------------------------------------
        // KEY SAMPLE CODE ENDS HERE
        // -----------------------------------------------------------------------
```
**Outras páginas e os pontos finais** pode ser visto como interagir com os outros pontos finais expostos pelo serviço de API de visão do computador ao observar as outras páginas na amostra; por exemplo, o ponto final OCR é mostrado como parte do código contido no OCRPage.xaml.cs 

### <a name="Related">Tópicos relacionados</a>
 * [Introdução à API de rostos em](../../Face/Tutorials/FaceAPIinCSharpTutorial.md)
 
 


