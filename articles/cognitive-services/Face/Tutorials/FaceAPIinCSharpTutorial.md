---
title: Tutorial de C# da API Face | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Neste tutorial, vai criar uma aplicação Windows simples que utiliza o serviço Face dos Serviços Cognitivos para detetar e enquadrar os rostos numa imagem.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.component: face-api
ms.topic: tutorial
ms.date: 06/29/2018
ms.author: nolachar
ms.openlocfilehash: e4f2192c40f0b650b31ed59642dee89e42eca703
ms.sourcegitcommit: b9786bd755c68d602525f75109bbe6521ee06587
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/18/2018
ms.locfileid: "39125948"
---
# <a name="tutorial-create-a-wpf-app-to-detect-and-frame-faces-in-an-image"></a>Tutorial: criar uma aplicação WPF para detetar e enquadrar rostos numa imagem

Neste tutorial, vai criar uma aplicação WPF (Windows Presentation Framework) que utiliza o serviço Face através da respetiva biblioteca de cliente do .NET. A aplicação deteta os rostos numa imagem, desenha uma moldura em torno de cada rosto e apresenta uma descrição do rosto na barra de estado. O código de exemplo completo está disponível no GitHub em [Detetar e enquadrar os rostos numa imagem do Windows](https://github.com/Azure-Samples/Cognitive-Face-CSharp-sample).

![Captura de ecrã que mostra os rostos detetados, enquadrados com retângulos](../Images/getting-started-cs-detected.png)

Este tutorial mostrar-lhe como:

> [!div class="checklist"]
> - Criar uma aplicação WPF
> - Instalar a biblioteca de cliente do serviço Face
> - Utilizar a biblioteca de cliente para detetar rostos numa imagem
> - Desenhar uma moldura em torno de cada rosto detetado
> - Apresentar uma descrição do rosto na barra de estado

## <a name="prerequisites"></a>Pré-requisitos

- Precisa de uma chave de subscrição para executar o exemplo. Pode obter chaves de subscrição de avaliação gratuita em [Experimente os Serviços Cognitivos](https://azure.microsoft.com/try/cognitive-services/?api=face-api).
- Qualquer edição do [Visual Studio 2015 ou 2017](https://www.visualstudio.com/downloads/). Para o Visual Studio 2017, é necessária a carga de trabalho de desenvolvimento de aplicações de Ambiente de Trabalho .NET. Este tutorial utiliza o Visual Studio 2017 Community Edition.
- O pacote NuGet da biblioteca de cliente [Microsoft.Azure.CognitiveServices.Vision.Face 2.0.0-preview](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.Face/2.0.0-preview). Não é necessário transferir o pacote. Abaixo, são fornecidas as instruções de instalação.

## <a name="create-the-visual-studio-solution"></a>Criar a solução do Visual Studio

Siga estes passos para criar um projeto de aplicação WPF do Windows.

1. Abra o Visual Studio e, no menu **Ficheiro**, clique em **Novo** e em **Projeto**.
   - No Visual Studio 2017, expanda **Instalado**, e em seguida, **Outras Linguagens**. Selecione **Visual C#** e **Aplicação WPF (.NET Framework)**.
   - No Visual Studio 2015, expanda **Instalado**, e em seguida, **Modelos**. Selecione **Visual C#** e **Aplicação WPF**.
1. Dê à aplicação o nome **FaceTutorial** e, em seguida, clique em **OK**.

## <a name="install-the-face-service-client-library"></a>Instalar a biblioteca de cliente do serviço Face

Siga estas instruções para instalar a biblioteca de cliente.

1. No menu **Ferramentas**, selecione **Gestor de Pacotes NuGet** e, em seguida, **Consola do Gestor de Pacotes**.
1. Na **Consola do Gestor de Pacotes**, cole o seguinte e prima **Enter**.

    `Install-Package Microsoft.Azure.CognitiveServices.Vision.Face -Version 2.0.0-preview`

## <a name="add-the-initial-code"></a>Adicionar o código inicial

### <a name="mainwindowxaml"></a>MainWindow.xaml

Abra *MainWindow.xaml* (dica: alterne os painéis com o **ícone de seta para cima/baixo**) e substitua o conteúdo pelo seguinte código. Este código de xaml é utilizado para criar a janela de IU. Tenha em atenção os processadores de eventos `FacePhoto_MouseMove` e `BrowseButton_Click`.

```xml
<Window x:Class="FaceTutorial.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        Title="MainWindow" Height="700" Width="960">
    <Grid x:Name="BackPanel">
        <Image x:Name="FacePhoto" Stretch="Uniform" Margin="0,0,0,50" MouseMove="FacePhoto_MouseMove" />
        <DockPanel DockPanel.Dock="Bottom">
            <Button x:Name="BrowseButton" Width="72" Height="20" VerticalAlignment="Bottom" HorizontalAlignment="Left"
                    Content="Browse..."
                    Click="BrowseButton_Click" />
            <StatusBar VerticalAlignment="Bottom">
                <StatusBarItem>
                    <TextBlock Name="faceDescriptionStatusBar" />
                </StatusBarItem>
            </StatusBar>
        </DockPanel>
    </Grid>
</Window>
```

### <a name="mainwindowxamlcs"></a>MainWindow.xaml.cs

Expanda *MainWindow.xaml*, abra *MainWindow.xaml.cs*e substitua o conteúdo pelo seguinte código. Ignore os sublinhados vermelhos ondulados; irão desaparecer após a primeira compilação.

As duas primeiras linhas importam os espaços de nomes da biblioteca de cliente. Em seguida, o `FaceClient` é criado, passando a chave de subscrição, enquanto a região do Azure é definida no construtor `MainWindow`. Os dois métodos, `BrowseButton_Click` e `FacePhoto_MouseMove`, correspondem aos processadores de eventos declarados em *MainWindow.xaml*.

`BrowseButton_Click` cria um `OpenFileDialog`, que permite ao utilizador selecionar uma imagem jpg. A imagem é lida e apresentada na janela principal. O código restante para `BrowseButton_Click` e o código para `FacePhoto_MouseMove` são inseridos nos passos subsequentes.

```csharp
using Microsoft.Azure.CognitiveServices.Vision.Face;
using Microsoft.Azure.CognitiveServices.Vision.Face.Models;

using System;
using System.Collections.Generic;
using System.IO;
using System.Text;
using System.Threading.Tasks;
using System.Windows;
using System.Windows.Input;
using System.Windows.Media;
using System.Windows.Media.Imaging;

namespace FaceTutorial
{
    public partial class MainWindow : Window
    {
        // Replace <SubscriptionKey> with your valid subscription key.
        // For example, subscriptionKey = "0123456789abcdef0123456789ABCDEF"
        private const string subscriptionKey = "<SubscriptionKey>";

        // Replace or verify the region.
        //
        // You must use the same region as you used to obtain your subscription
        // keys. For example, if you obtained your subscription keys from the
        // westus region, replace "westcentralus" with "westus".
        //
        // NOTE: Free trial subscription keys are generated in the westcentralus
        // region, so if you are using a free trial subscription key, you should
        // not need to change this region.
        private const string baseUri =
            "https://westcentralus.api.cognitive.microsoft.com/face/v1.0";

        private readonly IFaceClient faceClient = new FaceClient(
            new ApiKeyServiceClientCredentials(subscriptionKey),
            new System.Net.Http.DelegatingHandler[] { });

        IList<DetectedFace> faceList;   // The list of detected faces.
        String[] faceDescriptions;      // The list of descriptions for the detected faces.
        double resizeFactor;            // The resize factor for the displayed image.

        public MainWindow()
        {
            InitializeComponent();

            if (Uri.IsWellFormedUriString(baseUri, UriKind.Absolute))
            {
                faceClient.BaseUri = new Uri(baseUri);
            }
            else
            {
                MessageBox.Show(baseUri,
                    "Invalid URI", MessageBoxButton.OK, MessageBoxImage.Error);
                Environment.Exit(0);
            }
        }

        // Displays the image and calls UploadAndDetectFaces.
        private async void BrowseButton_Click(object sender, RoutedEventArgs e)
        {
            // Get the image file to scan from the user.
            var openDlg = new Microsoft.Win32.OpenFileDialog();

            openDlg.Filter = "JPEG Image(*.jpg)|*.jpg";
            bool? result = openDlg.ShowDialog(this);

            // Return if canceled.
            if (!(bool)result)
            {
                return;
            }

            // Display the image file.
            string filePath = openDlg.FileName;

            Uri fileUri = new Uri(filePath);
            BitmapImage bitmapSource = new BitmapImage();

            bitmapSource.BeginInit();
            bitmapSource.CacheOption = BitmapCacheOption.None;
            bitmapSource.UriSource = fileUri;
            bitmapSource.EndInit();

            FacePhoto.Source = bitmapSource;
        }

        // Displays the face description when the mouse is over a face rectangle.
        private void FacePhoto_MouseMove(object sender, MouseEventArgs e)
        {
        }
    }
}
```

### <a name="insert-your-subscription-key-and-verify-or-change-the-region"></a>Inserir a chave de subscrição e validar ou alterar a região

- Localize a seguinte linha em *MainWindow.xaml.cs* e substitua `<Subscription Key>` pela sua chave de subscrição da API Face:

    ```csharp
    private const string subscriptionKey = "<SubscriptionKey>";
    ```

- Localize a seguinte linha em *MainWindow.xaml.cs* e substitua ou valide a região do Azure associada à sua chave de subscrição:

    ```csharp
    private const string baseUri =
        "https://westcentralus.api.cognitive.microsoft.com/face/v1.0";
    ```

    Certifique-se de que a localização é a mesma onde obteve as chaves de subscrição. Se tiver obtido as chaves de subscrição a partir da região **westus**, por exemplo, substitua `Westcentralus` por `Westus`.

    Se tiver recebido as chaves de subscrição através da versão de avaliação gratuita, a região para as suas chaves é **westcentralus**, por isso, não é necessária qualquer alteração.

### <a name="test-the-app"></a>Testar a aplicação

Prima **Iniciar** no menu para testar a sua aplicação. Quando a janela abrir, clique em **Procurar** no canto inferior esquerdo. É apresentada uma caixa de diálogo **Ficheiro Aberto**, onde pode procurar e selecionar uma fotografia que é então apresentada na janela.

![Captura de ecrã que mostra uma imagem não modificada de rostos](../Images/getting-started-cs-ui.png)

## <a name="upload-an-image-to-detect-faces"></a>Carregar uma imagem para detetar rostos

A forma mais simples de detetar rostos é chamar o método `FaceClient.Face.DetectWithStreamAsync`, que encapsula o método API [Detetar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) para carregar a imagem local.

Insira o seguinte método na classe `MainWindow`, abaixo do método `FacePhoto_MouseMove`.

É criada uma lista dos atributos faciais a analisar, e o ficheiro de imagem submetido é lido num `Stream`. Ambos são transmitidos à chamada `DetectWithStreamAsync`.

```csharp
// Uploads the image file and calls DetectWithStreamAsync.
private async Task<IList<DetectedFace>> UploadAndDetectFaces(string imageFilePath)
{
    // The list of Face attributes to return.
    IList<FaceAttributeType> faceAttributes =
        new FaceAttributeType[]
        {
            FaceAttributeType.Gender, FaceAttributeType.Age,
            FaceAttributeType.Smile, FaceAttributeType.Emotion,
            FaceAttributeType.Glasses, FaceAttributeType.Hair
        };

    // Call the Face API.
    try
    {
        using (Stream imageFileStream = File.OpenRead(imageFilePath))
        {
            // The second argument specifies to return the faceId, while
            // the third argument specifies not to return face landmarks.
            IList<DetectedFace> faceList =
                await faceClient.Face.DetectWithStreamAsync(
                    imageFileStream, true, false, faceAttributes);
            return faceList;
        }
    }
    // Catch and display Face API errors.
    catch (APIErrorException f)
    {
        MessageBox.Show(f.Message);
        return new List<DetectedFace>();
    }
    // Catch and display all other errors.
    catch (Exception e)
    {
        MessageBox.Show(e.Message, "Error");
        return new List<DetectedFace>();
    }
}
```

## <a name="draw-rectangles-around-each-face"></a>Desenhar retângulos em torno de cada rosto

Adicione o código para desenhar um retângulo em torno de cada rosto detetado na imagem.

Em *MainWindow.xaml.cs*, adicione o modificador `async` ao método `BrowseButton_Click`.

```csharp
private async void BrowseButton_Click(object sender, RoutedEventArgs e)
```

Insira o seguinte código no final do método `BrowseButton_Click`, após a linha `FacePhoto.Source = bitmapSource`.

A lista de rostos detetados é preenchida pela chamada para `UploadAndDetectFaces`. Em seguida, é desenhado um retângulo em torno de cada rosto, e a imagem modificada é apresentada na janela principal.

```csharp
// Detect any faces in the image.
Title = "Detecting...";
faceList = await UploadAndDetectFaces(filePath);
Title = String.Format(
    "Detection Finished. {0} face(s) detected", faceList.Count);

if (faceList.Count > 0)
{
    // Prepare to draw rectangles around the faces.
    DrawingVisual visual = new DrawingVisual();
    DrawingContext drawingContext = visual.RenderOpen();
    drawingContext.DrawImage(bitmapSource,
        new Rect(0, 0, bitmapSource.Width, bitmapSource.Height));
    double dpi = bitmapSource.DpiX;
    resizeFactor = (dpi > 0) ? 96 / dpi : 1;
    faceDescriptions = new String[faceList.Count];

    for (int i = 0; i < faceList.Count; ++i)
    {
        DetectedFace face = faceList[i];

        // Draw a rectangle on the face.
        drawingContext.DrawRectangle(
            Brushes.Transparent,
            new Pen(Brushes.Red, 2),
            new Rect(
                face.FaceRectangle.Left * resizeFactor,
                face.FaceRectangle.Top * resizeFactor,
                face.FaceRectangle.Width * resizeFactor,
                face.FaceRectangle.Height * resizeFactor
                )
        );

        // Store the face description.
        faceDescriptions[i] = FaceDescription(face);
    }

    drawingContext.Close();

    // Display the image with the rectangle around the face.
    RenderTargetBitmap faceWithRectBitmap = new RenderTargetBitmap(
        (int)(bitmapSource.PixelWidth * resizeFactor),
        (int)(bitmapSource.PixelHeight * resizeFactor),
        96,
        96,
        PixelFormats.Pbgra32);

    faceWithRectBitmap.Render(visual);
    FacePhoto.Source = faceWithRectBitmap;

    // Set the status bar text.
    faceDescriptionStatusBar.Text =
        "Place the mouse pointer over a face to see the face description.";
}
```

## <a name="describe-the-faces-in-the-image"></a>Descrever os rostos na imagem

Anexe o seguinte método à classe `MainWindow`, abaixo do método `UploadAndDetectFaces`.

O método converte os atributos faciais numa cadeia de carateres que descreve a face. A cadeia de carateres é apresentada quando o ponteiro do rato paira sobre o retângulo do rosto.

```csharp
// Creates a string out of the attributes describing the face.
private string FaceDescription(DetectedFace face)
{
    StringBuilder sb = new StringBuilder();

    sb.Append("Face: ");

    // Add the gender, age, and smile.
    sb.Append(face.FaceAttributes.Gender);
    sb.Append(", ");
    sb.Append(face.FaceAttributes.Age);
    sb.Append(", ");
    sb.Append(String.Format("smile {0:F1}%, ", face.FaceAttributes.Smile * 100));

    // Add the emotions. Display all emotions over 10%.
    sb.Append("Emotion: ");
    Emotion emotionScores = face.FaceAttributes.Emotion;
    if (emotionScores.Anger >= 0.1f)
        sb.Append(String.Format("anger {0:F1}%, ", emotionScores.Anger * 100));
    if (emotionScores.Contempt >= 0.1f)
        sb.Append(String.Format("contempt {0:F1}%, ", emotionScores.Contempt * 100));
    if (emotionScores.Disgust >= 0.1f)
        sb.Append(String.Format("disgust {0:F1}%, ", emotionScores.Disgust * 100));
    if (emotionScores.Fear >= 0.1f)
        sb.Append(String.Format("fear {0:F1}%, ", emotionScores.Fear * 100));
    if (emotionScores.Happiness >= 0.1f)
        sb.Append(String.Format("happiness {0:F1}%, ", emotionScores.Happiness * 100));
    if (emotionScores.Neutral >= 0.1f)
        sb.Append(String.Format("neutral {0:F1}%, ", emotionScores.Neutral * 100));
    if (emotionScores.Sadness >= 0.1f)
        sb.Append(String.Format("sadness {0:F1}%, ", emotionScores.Sadness * 100));
    if (emotionScores.Surprise >= 0.1f)
        sb.Append(String.Format("surprise {0:F1}%, ", emotionScores.Surprise * 100));

    // Add glasses.
    sb.Append(face.FaceAttributes.Glasses);
    sb.Append(", ");

    // Add hair.
    sb.Append("Hair: ");

    // Display baldness confidence if over 1%.
    if (face.FaceAttributes.Hair.Bald >= 0.01f)
        sb.Append(String.Format("bald {0:F1}% ", face.FaceAttributes.Hair.Bald * 100));

    // Display all hair color attributes over 10%.
    IList<HairColor> hairColors = face.FaceAttributes.Hair.HairColor;
    foreach (HairColor hairColor in hairColors)
    {
        if (hairColor.Confidence >= 0.1f)
        {
            sb.Append(hairColor.Color.ToString());
            sb.Append(String.Format(" {0:F1}% ", hairColor.Confidence * 100));
        }
    }

    // Return the built string.
    return sb.ToString();
}
```

## <a name="display-the-face-description"></a>Apresentar a descrição de rostos

Substitua o método `FacePhoto_MouseMove` pelo código abaixo.

Este processador de eventos apresenta a cadeia de carateres de descrição do rosto quando o ponteiro do rato paira sobre o retângulo do rosto.

```csharp
private void FacePhoto_MouseMove(object sender, MouseEventArgs e)
{
    // If the REST call has not completed, return.
    if (faceList == null)
        return;

    // Find the mouse position relative to the image.
    Point mouseXY = e.GetPosition(FacePhoto);

    ImageSource imageSource = FacePhoto.Source;
    BitmapSource bitmapSource = (BitmapSource)imageSource;

    // Scale adjustment between the actual size and displayed size.
    var scale = FacePhoto.ActualWidth / (bitmapSource.PixelWidth / resizeFactor);

    // Check if this mouse position is over a face rectangle.
    bool mouseOverFace = false;

    for (int i = 0; i < faceList.Count; ++i)
    {
        FaceRectangle fr = faceList[i].FaceRectangle;
        double left = fr.Left * scale;
        double top = fr.Top * scale;
        double width = fr.Width * scale;
        double height = fr.Height * scale;

        // Display the face description if the mouse is over this face rectangle.
        if (mouseXY.X >= left && mouseXY.X <= left + width &&
            mouseXY.Y >= top  && mouseXY.Y <= top + height)
        {
            faceDescriptionStatusBar.Text = faceDescriptions[i];
            mouseOverFace = true;
            break;
        }
    }

    // String to display when the mouse is not over a face rectangle.
    if (!mouseOverFace)
        faceDescriptionStatusBar.Text =
            "Place the mouse pointer over a face to see the face description.";
}
```

## <a name="run-the-app"></a>Executar a aplicação

Execute a aplicação e procure uma imagem que tenha um rosto. Aguarde alguns segundos para permitir que o serviço Face responda. Depois disso, verá um retângulo vermelho nos rostos da imagem. Ao pairar o rato sobre um retângulo de rosto, a descrição desse rosto é apresentada na barra de estado.

![Captura de ecrã que mostra os rostos detetados, enquadrados com retângulos](../Images/getting-started-cs-detected.png)

## <a name="summary"></a>Resumo

Neste tutorial, aprendeu o processo básico para utilizar a biblioteca de cliente do serviço Face e criou uma aplicação para apresentar e enquadrar os rostos numa imagem.

## <a name="next-steps"></a>Passos seguintes

Saiba mais sobre como detetar e utilizar as particularidades do rosto.

> [!div class="nextstepaction"]
> [Como Detetar Rostos numa Imagem](../Face-API-How-to-Topics/HowtoDetectFacesinImage.md)
