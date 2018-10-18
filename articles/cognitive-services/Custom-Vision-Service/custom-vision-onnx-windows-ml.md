---
title: 'Tutorial: Utilizar um modelo ONNX com o Windows ML – Serviço de Visão Personalizada'
titlesuffix: Azure Cognitive Services
description: Saiba como criar uma aplicação UWP do Windows que utiliza um modelo ONNX exportado dos Serviços Cognitivos do Azure.
services: cognitive-services
author: larryfr
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: tutorial
ms.date: 06/19/2018
ms.author: larryfr
ms.openlocfilehash: 3a9e9bc92ce38c4bb8d6d83c8017fa223342e7d2
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/19/2018
ms.locfileid: "46365609"
---
# <a name="tutorial-use-an-onnx-model-from-custom-vision-with-windows-ml-preview"></a>Tutorial: Utilizar um modelo ONNX da Visão Personalizada com o Windows ML (pré-visualização)

Saiba como utilizar um modelo ONNX exportado do Serviço de Visão Personalizada com o Windows ML (pré-visualização).

As informações neste documento mostram como utilizar um ficheiro ONNX exportado do Serviço de Visão Personalizada com o Windows ML. É fornecida uma aplicação UWP do Windows de exemplo. É incluído no exemplo um modelo preparado capaz de reconhecer cães e gatos. São também fornecidos passos sobre como pode utilizar o seu próprio modelo com este exemplo.

> [!div class="checklist"]
> * Sobre a aplicação de exemplo
> * Obter o código de exemplo
> * Executar o exemplo
> * Utilizar o seu próprio modelo

## <a name="prerequisites"></a>Pré-requisitos

* Um dispositivo Windows 10 com:

    * Uma câmara.

    * O Visual Studio 2017 versão 15.7 ou posterior com a carga de trabalho de __programação da Plataforma Universal do Windows__ ativada.

    * O modo de Programador ativado. Para obter mais informações, veja o documento [Enable your device for development](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development) (Ativar o seu dispositivo para programação).

* (Opcional) Um ficheiro ONNX exportado do Serviço de Visão Personalizada. Para obter mais informações, veja o documento [Exportar o seu modelo para utilização com dispositivos móveis](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/export-your-model).

    > [!NOTE]
    > Para utilizar o seu próprio modelo, siga os passos na secção [Utilizar o seu próprio modelo](#use-your-own-model).

## <a name="about-the-example-app"></a>Sobre a aplicação de exemplo

A aplicação é uma aplicação UWP do Windows genérica. Esta aplicação utiliza a câmara do seu dispositivo Windows 10 para fornecer imagens ao modelo. As etiquetas e as pontuações devolvidas pelo modelo são apresentadas sob a pré-visualização de vídeo.

* À medida que os dados são obtidos através da câmara, a classe [MediaFrameReader](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader) é utilizada para extrair os frames individuais. Os frames são enviados para o modelo para pontuação.

* O modelo devolve as etiquetas com base nas quais foi preparado e um valor float que indica o nível de confiança de que a imagem contém esse item.

### <a name="the-ui"></a>A IU

A IU da aplicação de exemplo é criada com os controlos __CaptureElement__ e __TextBlock__. O controlo CaptureElement apresenta uma pré-visualização do vídeo da câmara e o controlo TextBlock apresenta os resultados devolvidos do modelo. 

### <a name="the-model"></a>O modelo

O modelo (`cat-or-dog.onnx`) fornecido com o exemplo foi criado e preparado com o Serviço de Visão Personalizada dos Serviços Cognitivos. Em seguida, o modelo preparado foi exportado como um modelo ONNX. Para obter mais informações sobre como utilizar este serviço, veja os documentos [Como criar um classificador com a Visão Personalizada](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/getting-started-build-a-classifier) e [Exportar o seu modelo para utilização com dispositivos móveis](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/export-your-model).

> [!IMPORTANT]
> O modelo fornecido com o exemplo foi preparado com um conjunto pequeno de imagens de cães e gatos. Por isso, poderá não ser o melhor para reconhecer cães e gatos.

### <a name="the-model-class-file"></a>O ficheiro de classes do modelo

Ao adicionar um arquivo ONNX a uma aplicação UWP do Windows, é criado um ficheiro .cs. Este ficheiro tem o mesmo nome que o ficheiro `.onnx` (`cat-or-dog` neste exemplo) e contém as classes utilizadas para trabalhar com o modelo a partir de C#. No entanto, as entidades na classe gerada podem ter nomes como `_x0033_04aa07b_x002D_6c8c_x002D_4641_x002D_93a6_x002D_f3152f8740a1_028da4e3_x002D_9c6e_x002D_480b_x002D_b53c_x002D_c1db13d24d70ModelInput`. Pode mudar o nome destas entradas em segurança (clique com o botão direito do rato e selecione "Mudar o Nome") para um nome amigável.

> [!NOTE]
> O código de exemplo refatorizou os nomes de classe e método gerados para os seguintes:
>
> * `ModelInput`
> * `ModelOutput`
> * `Model`
> * `CreateModel`

### <a name="camera-access"></a>Acesso à câmara

O separador __Capabilities__ no ficheiro `Package.appxmanifest` está configurado para permitir o acesso à câmara Web e ao microfone.

> [!NOTE]
> Embora este exemplo não utilize áudio, foi necessário ativar o microfone antes de aceder à câmara do dispositivo utilizado.

A aplicação tenta aceder à câmara na parte de trás do seu dispositivo, se disponível. A aplicação utiliza a classe [MediaCapture](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture) para iniciar a captura de vídeo da câmara. A classe [MediaFrameReader](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameReader) é utilizada para capturar os frames de vídeo e enviá-los para o modelo.

## <a name="get-the-example-code"></a>Obter o código de exemplo

A aplicação de exemplo está disponível em [https://github.com/Azure-Samples/Custom-Vision-ONNX-UWP](https://github.com/Azure-Samples/Custom-Vision-ONNX-UWP).

## <a name="run-the-example"></a>Executar o exemplo

1. Utilize a tecla `F5` para iniciar a aplicação a partir do Visual Studio. Poderá ser-lhe pedido para ativar o modo de Programador. Para obter mais informações, veja o documento [Enable your device for development](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development) (Ativar o seu dispositivo para programação).

2. Quando lhe for pedido, permita que a aplicação aceda à câmara e ao microfone do seu dispositivo.

3. Aponte a câmara para um cão ou um gato. A pontuação para saber se a imagem contém um cão ou um gato é apresentada sob a pré-visualização na aplicação.

    > [!TIP]
    > Se não tiver um cão ou um gato por perto, pode utilizar uma fotografia de um.

## <a name="use-your-own-model"></a>Utilizar o seu próprio modelo

Para utilizar o seu próprio modelo, execute os seguintes passos:

> [!IMPORTANT]
> Os passos nesta secção mudam o nome do modelo atual (cat-or-dog.cs) e refatorizam os nomes de classe e método do novo modelo. Isto destina-se a evitar conflitos de nomenclatura com o modelo de exemplo.

1. Prepare um modelo com o Serviço de Visão Personalizada. Para obter informações sobre como preparar um modelo, veja o documento [Como criar um classificador com a Visão Personalizada](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/getting-started-build-a-classifier).

2. Exporte o modelo preparado como um modelo ONNX. Para obter informações sobre como exportar um modelo, veja o documento [Exportar o seu modelo para utilização com dispositivos móveis](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/export-your-model).

3. No Explorador de Soluções, clique com botão direito do rato no __cat-or-dog.cs__ e mude o respetivo nome para __cat-or-dog.txt__. Mudar o nome do modelo evita conflitos de nomes com o novo modelo.

    > [!TIP]
    > Também pode utilizar nomes diferentes para os nomes de classe no novo modelo, mas é mais fácil reutilizar os nomes existentes.

4. No Explorador de Soluções, clique com botão direito do rato na entrada __VisionApp__ e selecione __Adicionar__ > __Item Existente...__.

5. Para gerar uma classe para o modelo, selecione o ficheiro ONNX a importar e, em seguida, selecione o botão __Adicionar__. É adicionada ao Explorador de Soluções uma nova classe com o mesmo nome do ficheiro ONNX (mas uma extensão `.cs`).

6. Abra o ficheiro .cs gerado e localize os nomes dos seguintes itens:

    > [!IMPORTANT]
    > Utilize o ficheiro de exemplo `cat-or-dog.txt` como um guia para reconhecer as classes e as funções.

    * A classe que define a entrada do modelo. O nome gerado poderá ser semelhante a `_x0033_04aa07b_x002D_6c8c_x002D_4641_x002D_93a6_x002D_f3152f8740a1_028da4e3_x002D_9c6e_x002D_480b_x002D_b53c_x002D_c1db13d24d70ModelInput`. Mude o nome desta classe para __ModelInput__.
    * A classe que define a saída do modelo. O nome gerado poderá ser semelhante a `_x0033_04aa07b_x002D_6c8c_x002D_4641_x002D_93a6_x002D_f3152f8740a1_028da4e3_x002D_9c6e_x002D_480b_x002D_b53c_x002D_c1db13d24d70ModelOutput`. Mude o nome desta classe para __ModelOutput__.
    * A classe que define o modelo. O nome gerado poderá ser semelhante a `_x0033_04aa07b_x002D_6c8c_x002D_4641_x002D_93a6_x002D_f3152f8740a1_028da4e3_x002D_9c6e_x002D_480b_x002D_b53c_x002D_c1db13d24d70Model`. Mude o nome desta classe para __Model__.
    * O método que cria o modelo. O nome gerado poderá ser semelhante a `Create_x0033_04aa07b_x002D_6c8c_x002D_4641_x002D_93a6_x002D_f3152f8740a1_028da4e3_x002D_9c6e_x002D_480b_x002D_b53c_x002D_c1db13d24d70Model`. Mude o nome deste método para __CreateModel__.

7. No Explorador de Soluções, mova o ficheiro `.onnx` para a pasta __Ativos__. 

8. Para incluir o ficheiro ONNX no pacote de aplicação, selecione o ficheiro `.onnx` e defina __Ação de Compilação__ para __Conteúdo__ nas propriedades.

9. Abra o ficheiro __MainPage.xaml.cs__. Localize a seguinte linha e altere o nome de ficheiro para o novo ficheiro `.onnx`:

    ```csharp
    var file = await StorageFile.GetFileFromApplicationUriAsync(new Uri($"ms-appx:///Assets/cat-or-dog.onnx"));
    ```

    Esta alteração carrega o novo modelo durante a execução.

10. Compile e execute a aplicação. A aplicação utiliza agora o novo modelo para pontuar imagens.

## <a name="next-steps"></a>Passos seguintes

Para descobrir outras formas de exportar e utilizar um modelo de Visão Personalizada, veja os seguintes documentos:

* [Exportar o seu modelo para utilização com dispositivos móveis](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/export-your-model)
* [Utilizar o modelo exportado do Tensorflow numa aplicação Android](https://github.com/Azure-Samples/cognitive-services-android-customvision-sample)
* [Utilizar o modelo exportado do CoreML numa aplicação Swift iOS](https://go.microsoft.com/fwlink/?linkid=857726)
* [Utilizar o modelo exportado do CoreML numa aplicação iOS com Xamarin](https://github.com/xamarin/ios-samples/tree/master/ios11/CoreMLAzureModel)

Para obter mais informações sobre como utilizar modelos ONNX com o Windows ML, veja o documento [Integrate a model into your app with Windows ML](https://docs.microsoft.com/windows/uwp/machine-learning/integrate-model) (Integrar um modelo na sua aplicação com o Windows ML).
