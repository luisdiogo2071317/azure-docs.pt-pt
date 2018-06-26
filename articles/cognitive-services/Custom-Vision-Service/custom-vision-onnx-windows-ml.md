---
title: Modelo de visão ONNX personalizado com o Windows ML - serviços cognitivos | Microsoft Docs
description: Saiba como criar uma aplicação de UWP do Windows que utiliza um modelo ONNX exportado a partir de serviços cognitivos.
services: cognitive-services
author: larryfr
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: conceptual
ms.date: 06/19/2018
ms.author: larryfr
ms.openlocfilehash: 0b128ba1800e74c20c09a9c5711c8473f1dd00d0
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/25/2018
ms.locfileid: "36939490"
---
# <a name="tutorial-use-an-onnx-model-from-custom-vision-with-windows-ml-preview"></a>Tutorial: Utilizar um modelo ONNX da visão personalizada com o Windows ML (pré-visualização)

Saiba como utilizar um modelo ONNX exportado a partir do serviço de visão personalizada com o Windows ML (pré-visualização).

As informações neste documento demonstra como utilizar um ficheiro ONNX exportado a partir do serviço de visão personalizada com o Windows ML. Um exemplo de aplicação UWP de Windows é fornecido. Um modelo treinado que pode reconhecer dogs e cats está incluído com o exemplo. Passos também são fornecidos no como pode utilizar o seu próprio modelo com este exemplo.

> [!div class="checklist"]
> * Sobre a aplicação de exemplo
> * Obter o código de exemplo
> * Executar o exemplo
> * Utilizar o seu próprio modelo

## <a name="prerequisites"></a>Pré-requisitos

* Um dispositivo Windows 10 com:

    * Uma câmara.

    * Visual Studio 2017 versão 15.7 ou posterior com o __desenvolvimento de plataforma Universal do Windows__ carga de trabalho ativada.

    * Modo de programador ativado. Para obter mais informações, consulte o [ativar o seu dispositivo para o desenvolvimento](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development) documento.

* (Opcional) Um ficheiro ONNX exportado a partir do serviço de visão personalizada. Para obter mais informações, consulte o [exportar o modelo para utilização com dispositivos móveis](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/export-your-model) documento.

    > [!NOTE]
    > Para utilizar o seu próprio modelo, siga os passos a [utilizar o seu próprio modelo](#use-your-own-model) secção.

## <a name="about-the-example-app"></a>Sobre a aplicação de exemplo

A aplicação é uma aplicação de Windows UWP genérica. Utiliza a câmara no seu dispositivo Windows 10 para fornecer imagens para o modelo. As etiquetas e pontuações devolvidas pelo modelo são apresentadas abaixo a vídeo pré-visualização.

* Como dados é apresentada no apesar da câmara, [MediaFrameReader](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader) é utilizado para extrair frames individuais. As fotogramas são enviadas para o modelo para classificação.

* O modelo devolve as etiquetas que foi preparado no e um valor de vírgula flutuante que indica a certeza de como é que a imagem contém este item.

### <a name="the-ui"></a>A IU

A IU da aplicação de exemplo é criada utilizando __CaptureElement__ e __TextBlock__ controlos. O CaptureElement mostra uma versão de pré-visualização do vídeo da câmara, e o TextBlock os resultados devolvidos do modelo. 

### <a name="the-model"></a>O modelo

O modelo (`cat-or-dog.onnx`) fornecida com o exemplo foi criado e preparada utilizando o serviço de problemas de visão do cognitivos serviços personalizada. O modelo treinado, em seguida, foi exportado como um modelo ONNX. Para obter mais informações sobre como utilizar este serviço, consulte o [como criar um classificador](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/getting-started-build-a-classifier) e [exportar o modelo para utilização com dispositivos móveis](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/export-your-model) documentos.

> [!IMPORTANT]
> O modelo fornecido com o exemplo foi preparado com um pequeno conjunto de imagens preguiçoso e cat. Por isso, não poderá ser melhor o mundo no reconhecer dogs e cats.

### <a name="the-model-class-file"></a>O ficheiro de classe de modelo

Quando adiciona um ficheiro ONNX para uma aplicação de UWP do Windows, cria um ficheiro de CS. Este ficheiro tem o mesmo nome que o `.onnx` ficheiro (`cat-or-dog` neste exemplo) e contém as classes utilizadas para trabalhar com o modelo do c#. No entanto, as entidades na classe gerada podem ter nomes como `_x0033_04aa07b_x002D_6c8c_x002D_4641_x002D_93a6_x002D_f3152f8740a1_028da4e3_x002D_9c6e_x002D_480b_x002D_b53c_x002D_c1db13d24d70ModelInput`. Em segurança pode mudar o nome estas entradas (com o botão direito, mudar o nome) para um nome amigável.

> [!NOTE]
> O código de exemplo tem refatorizado os nomes de classe e método gerados para o seguinte:
>
> * `ModelInput`
> * `ModelOutput`
> * `Model`
> * `CreateModel`

### <a name="camera-access"></a>Acesso de câmara

O __capacidades__ separador o `Package.appxmanifest` ficheiro está configurado para permitir o acesso à câmara Web e microfone.

> [!NOTE]
> Apesar deste exemplo não utiliza áudio, posso tinha que ativar o microfone antes posso foi capaz de aceder a câmara do dispositivo.

A aplicação tenta obter a câmara de cópia de segurança do seu dispositivo esteja disponível. Utiliza o [MediaCapture](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture) classe para iniciar a captura de vídeo da câmara. [MediaFrameReader](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameReader) é utilizada para capturar as vídeos frames e enviá-los para o modelo.

## <a name="get-the-example-code"></a>Obter o código de exemplo

A aplicação de exemplo está disponível em [ https://github.com/Azure-Samples/Custom-Vision-ONNX-UWP ](https://github.com/Azure-Samples/Custom-Vision-ONNX-UWP).

## <a name="run-the-example"></a>Executar o exemplo

1. Utilize o `F5` chave para iniciar a aplicação a partir do Visual Studio. Poderá ser-lhe pedido para ativar o modo de programador. Para obter mais informações, consulte o [ativar o seu dispositivo para o desenvolvimento](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development) documento.

2. Quando lhe for pedido, permitir que a aplicação aceda à câmara e microfone no seu dispositivo.

3. Ponto a câmara num preguiçoso ou cat. A classificação para se a imagem contém um preguiçoso ou cat é apresentada abaixo a pré-visualização na aplicação.

    > [!TIP]
    > Se não tiver um preguiçoso ou cat à mão, pode utilizar uma fotografia de um preguiçoso ou cat.

## <a name="use-your-own-model"></a>Utilizar o seu próprio modelo

Para utilizar o seu próprio modelo, utilize os seguintes passos:

> [!IMPORTANT]
> Os passos nesta secção mudar o nome do modelo atual (cat ou dog.cs) e refatorar os nomes de classe e método do modelo de novo. Isto serve para evitar colisões com o modelo de exemplo de nomenclatura.

1. Preparar um modelo utilizando o serviço de visão personalizada. Para obter informações sobre como preparar um modelo, consulte o [como criar um classificador](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/getting-started-build-a-classifier).

2. Exporte o modelo treinado como um modelo ONNX. Para obter informações sobre como exportar um modelo, consulte o [exportar o modelo para utilização com dispositivos móveis](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/export-your-model) documento.

3. No Explorador de soluções, clique com botão direito do __cat ou dog.cs__ e alterá-lo para __cat ou dog.txt__. Mudar o nome impede colisões de nome com o novo modelo.

    > [!TIP]
    > Também pode utilizar diferentes nomes para os nomes de classe no modelo de novo, mas reutilizar os nomes existentes é mais fácil.

4. No Explorador de soluções, clique com botão direito do __VisionApp__ entrada e, em seguida, selecione __adicionar__ > __item existente...__ .

5. Para gerar uma classe para o modelo, selecione o ficheiro ONNX para importar e, em seguida, selecione o __adicionar__ botão. Uma nova classe com o mesmo nome que o ficheiro ONNX (mas um `.cs` extensão) é adicionado no Explorador de soluções.

6. Abra o ficheiro de CS gerado e localizar os nomes dos seguintes itens:

    > [!IMPORTANT]
    > Utilize o exemplo `cat-or-dog.txt` ficheiro como um guia para reconhecer as classes e funções.

    * A classe que define o modelo de entrada. O nome gerado pode ser semelhante ao `_x0033_04aa07b_x002D_6c8c_x002D_4641_x002D_93a6_x002D_f3152f8740a1_028da4e3_x002D_9c6e_x002D_480b_x002D_b53c_x002D_c1db13d24d70ModelInput`. Mudar o nome esta classe para __ModelInput__.
    * A classe que define o resultado de modelo. O nome gerado pode ser semelhante ao `_x0033_04aa07b_x002D_6c8c_x002D_4641_x002D_93a6_x002D_f3152f8740a1_028da4e3_x002D_9c6e_x002D_480b_x002D_b53c_x002D_c1db13d24d70ModelOutput`. Mudar o nome esta classe para __ModelOutput__.
    * A classe que define o modelo. O nome gerado pode ser semelhante ao `_x0033_04aa07b_x002D_6c8c_x002D_4641_x002D_93a6_x002D_f3152f8740a1_028da4e3_x002D_9c6e_x002D_480b_x002D_b53c_x002D_c1db13d24d70Model`. Mudar o nome esta classe para __modelo__.
    * O método que cria o modelo. O nome gerado pode ser semelhante ao `Create_x0033_04aa07b_x002D_6c8c_x002D_4641_x002D_93a6_x002D_f3152f8740a1_028da4e3_x002D_9c6e_x002D_480b_x002D_b53c_x002D_c1db13d24d70Model`. Mudar o nome este método __CreateModel__.

7. No Explorador de soluções, mova o `.onnx` de ficheiros para o __ativos__ pasta. 

8. Para incluir o ficheiro ONNX no pacote de aplicação, selecione o `.onnx` de ficheiros e defina __ação criar__ para __conteúdo__ nas propriedades.

9. Abra o __MainPage.xaml.cs__ ficheiro. Localize a seguinte linha e altere o nome de ficheiro para o novo `.onnx` ficheiro:

    ```csharp
    var file = await StorageFile.GetFileFromApplicationUriAsync(new Uri($"ms-appx:///Assets/cat-or-dog.onnx"));
    ```

    Esta alteração carrega o novo modelo no tempo de execução.

10. Crie e execute a aplicação. Utiliza agora o novo modelo para imagens de pontuação.

## <a name="next-steps"></a>Passos Seguintes

Para detetar outras formas de exportar e utilizar um modelo de visão personalizada, consulte os seguintes documentos:

* [Exportar o modelo](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/export-your-model)
* [Utilizar o modelo exportado do Tensorflow numa aplicação Android](https://github.com/Azure-Samples/cognitive-services-android-customvision-sample)
* [Utilizar o modelo exportado do CoreML numa aplicação Swift iOS](https://go.microsoft.com/fwlink/?linkid=857726)
* [Utilize exportado CoreML modelo numa aplicação com o Xamarin iOS](https://github.com/xamarin/ios-samples/tree/master/ios11/CoreMLAzureModel)

Para obter mais informações sobre a utilização de modelos ONNX com ML do Windows, consulte o [integrar um modelo de aplicação com o Windows ML](https://docs.microsoft.com/windows/uwp/machine-learning/integrate-model) documento.
