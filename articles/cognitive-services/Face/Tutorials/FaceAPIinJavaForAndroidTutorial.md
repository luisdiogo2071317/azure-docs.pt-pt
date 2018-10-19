---
title: 'Tutorial: Detetar e enquadrar rostos numa imagem – API Face, Java para Android'
titleSuffix: Azure Cognitive Services
description: Neste tutorial, irá criar uma aplicação Android simples que utiliza a API Face para detetar e enquadrar rostos numa imagem.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: face-api
ms.topic: tutorial
ms.date: 07/12/2018
ms.author: pafarley
ms.openlocfilehash: 08344e21d4f425a021bdefe840390ede8b3fb01e
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/16/2018
ms.locfileid: "49342386"
---
# <a name="tutorial-create-an-android-app-to-detect-and-frame-faces-in-an-image"></a>Tutorial: criar uma aplicação Android para detetar e enquadrar rostos numa imagem

Neste tutorial, vai criar uma aplicação Android simples que utiliza a biblioteca de classes de Java do serviço Face para detetar rostos humanos numa imagem. A aplicação mostra uma imagem selecionada, com cada rosto detetado enquadrado por um retângulo. O código de exemplo completo está disponível no GitHub em [Detetar e enquadrar rostos numa imagem em Android](https://github.com/Azure-Samples/cognitive-services-face-android-sample).

![Captura de ecrã do Android de uma fotografia com rostos enquadrados por um retângulo vermelho](../Images/android_getstarted2.1.PNG)

Este tutorial mostrar-lhe como:

> [!div class="checklist"]
> - Criar uma aplicação Android
> - Instalar a biblioteca de cliente do serviço Face
> - Utilizar a biblioteca de cliente para detetar rostos numa imagem
> - Desenhar uma moldura em torno de cada rosto detetado

## <a name="prerequisites"></a>Pré-requisitos

- Precisa de uma chave de subscrição para executar o exemplo. Pode obter chaves de subscrição de avaliação gratuita em [Experimente os Serviços Cognitivos](https://azure.microsoft.com/try/cognitive-services/?api=face-api).
- [Android Studio](https://developer.android.com/studio/) com o mínimo de SDK 22 (requerido pela biblioteca de cliente do Face).
- A biblioteca de cliente do Face [com.microsoft.projectoxford:face:1.4.3](http://search.maven.org/#search%7Cga%7C1%7Cg%3A%22com.microsoft.projectoxford%22) do Maven. Não é necessário transferir o pacote. Abaixo, são fornecidas as instruções de instalação.

## <a name="create-the-project"></a>Criar o projeto

Crie o seu projeto de aplicação Android seguindo estes passos:

1. Abra o Android Studio. Este tutorial utiliza o Android Studio 3.1.
1. Selecione **Iniciar um novo projeto do Android Studio**.
1. No ecrã **Criar Projeto Android**, modifique os campos predefinidos, se necessário, e clique em **Seguinte**.
1. No ecrã **Dispositivos Android de Destino**, utilize o seletor de lista pendente para escolher **API 22** ou superior, e clique em **Seguinte**.
1. Selecione **Atividade Vazia** e clique em **Seguinte**.
1. Desmarque **Retrocompatibilidade** e clique em **Concluir**.

## <a name="create-the-ui-for-selecting-and-displaying-the-image"></a>Criar a IU para selecionar e apresentar a imagem

Abra *activity_main.xml*; deverá ver o Editor do Esquema. Selecione o separador **Texto** e substitua o conteúdo pelo seguinte código.

```xml
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <ImageView
        android:layout_width="match_parent"
        android:layout_height="fill_parent"
        android:id="@+id/imageView1"
        android:layout_above="@+id/button1"
        android:contentDescription="Image with faces to analyze"/>

    <Button
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Browse for face image"
        android:id="@+id/button1"
        android:layout_alignParentBottom="true"/>
</RelativeLayout>
```

Abra *MainActivity.java* e substitua tudo, exceto a primeira instrução `package`, pelo seguinte código.

O código define um processador de eventos no `Button` que inicia uma nova atividade para permitir ao utilizador selecionar uma imagem. Depois de selecionada, a imagem é apresentada no `ImageView`.

```java
import java.io.*;
import android.app.*;
import android.content.*;
import android.net.*;
import android.os.*;
import android.view.*;
import android.graphics.*;
import android.widget.*;
import android.provider.*;

public class MainActivity extends Activity {
    private final int PICK_IMAGE = 1;
    private ProgressDialog detectionProgressDialog;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
            Button button1 = (Button)findViewById(R.id.button1);
            button1.setOnClickListener(new View.OnClickListener() {
                @Override
                public void onClick(View v) {
                Intent intent = new Intent(Intent.ACTION_GET_CONTENT);
                intent.setType("image/*");
                startActivityForResult(Intent.createChooser(
                        intent, "Select Picture"), PICK_IMAGE);
            }
        });

        detectionProgressDialog = new ProgressDialog(this);
    }

    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        if (requestCode == PICK_IMAGE && resultCode == RESULT_OK &&
                data != null && data.getData() != null) {
            Uri uri = data.getData();
            try {
                Bitmap bitmap = MediaStore.Images.Media.getBitmap(
                        getContentResolver(), uri);
                ImageView imageView = (ImageView) findViewById(R.id.imageView1);
                imageView.setImageBitmap(bitmap);

                // Uncomment
                //detectAndFrame(bitmap);
                } catch (IOException e) {
                    e.printStackTrace();
                }
        }
    }
}
```

A aplicação pode agora procurar uma fotografia e apresentá-la na janela, semelhante à imagem abaixo.

![Captura de ecrã do Android de uma fotografia com rostos](../Images/android_getstarted1.1.PNG)

## <a name="configure-the-face-client-library"></a>Configurar a biblioteca de cliente do Face

A API Face é uma API da cloud, que pode chamar com pedidos HTTPS. Este tutorial utiliza a biblioteca de cliente do Face, que encapsula estes pedidos Web para simplificar o seu trabalho.

No painel **Projeto**, utilize o seletor de lista pendente para selecionar **Android**. Expanda **Scripts de Gradle** e abra *build.gradle (Module: app)*.

Adicione uma dependência para a biblioteca de cliente do Face `com.microsoft.projectoxford:face:1.4.3`, conforme mostrado na captura de ecrã abaixo, e clique em **Sincronizar Agora**.

![Captura de ecrã do Android Studio do ficheiro build.gradle da Aplicação](../Images/face-tut-java-gradle.png)

Abra **MainActivity.java** e anexe as seguintes diretivas de importação:

```java
import com.microsoft.projectoxford.face.*;
import com.microsoft.projectoxford.face.contract.*;
```

## <a name="add-the-face-client-library-code"></a>Adicionar o código da biblioteca de cliente do Face

Insira o seguinte código na classe `MainActivity`, acima do método `onCreate`:

```java
private final String apiEndpoint = "<API endpoint>";
private final String subscriptionKey = "<Subscription Key>";

private final FaceServiceClient faceServiceClient =
        new FaceServiceRestClient(apiEndpoint, subscriptionKey);
```

Substitua `<API endpoint>` pelo ponto final da API que foi atribuído à sua chave. As chaves de subscrição de avaliação gratuita são geradas na região **westcentralus**. Portanto, se estiver a utilizar uma chave de subscrição de avaliação gratuita, a instrução seria:

```java
apiEndpoint = "https://westcentralus.api.cognitive.microsoft.com/face/v1.0";
```

Substitua `<Subscription Key>` pela sua chave de subscrição. Por exemplo:

```java
subscriptionKey = "0123456789abcdef0123456789ABCDEF"
```

No painel **Projeto**, expanda **aplicação**, em seguida, **manifestos**, e abra *AndroidManifest.xml*.

Insira o seguinte elemento como um subordinado direto do elemento `manifest`:

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

Compile o seu projeto para verificar a existência de erros. Está agora pronto para chamar o serviço Face.

## <a name="upload-an-image-to-detect-faces"></a>Carregar uma imagem para detetar rostos

A forma mais simples de detetar rostos é chamar o método `FaceServiceClient.detect`. Este método encapsula o método API [Detetar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) e devolve uma matriz de `Face`.

Cada `Face` devolvido inclui um retângulo para indicar a localização, em conjunto com uma série de atributos faciais opcionais. Neste exemplo, apenas as localizações de rostos são necessárias.

Se ocorrer um erro, um `AlertDialog` apresenta o motivo subjacente.

Insira os seguintes métodos na classe `MainActivity`.

```java
// Detect faces by uploading a face image.
// Frame faces after detection.
private void detectAndFrame(final Bitmap imageBitmap) {
    ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
    imageBitmap.compress(Bitmap.CompressFormat.JPEG, 100, outputStream);
    ByteArrayInputStream inputStream =
            new ByteArrayInputStream(outputStream.toByteArray());

    AsyncTask<InputStream, String, Face[]> detectTask =
            new AsyncTask<InputStream, String, Face[]>() {
                String exceptionMessage = "";

                @Override
                protected Face[] doInBackground(InputStream... params) {
                    try {
                        publishProgress("Detecting...");
                        Face[] result = faceServiceClient.detect(
                                params[0],
                                true,         // returnFaceId
                                false,        // returnFaceLandmarks
                                null          // returnFaceAttributes:
                                /* new FaceServiceClient.FaceAttributeType[] {
                                    FaceServiceClient.FaceAttributeType.Age,
                                    FaceServiceClient.FaceAttributeType.Gender }
                                */
                        );
                        if (result == null){
                            publishProgress(
                                    "Detection Finished. Nothing detected");
                            return null;
                        }
                        publishProgress(String.format(
                                "Detection Finished. %d face(s) detected",
                                result.length));
                        return result;
                    } catch (Exception e) {
                        exceptionMessage = String.format(
                                "Detection failed: %s", e.getMessage());
                        return null;
                    }
                }

                @Override
                protected void onPreExecute() {
                    //TODO: show progress dialog
                }
                @Override
                protected void onProgressUpdate(String... progress) {
                    //TODO: update progress
                }
                @Override
                protected void onPostExecute(Face[] result) {
                    //TODO: update face frames
                }
            };

    detectTask.execute(inputStream);
}

private void showError(String message) {
    new AlertDialog.Builder(this)
    .setTitle("Error")
    .setMessage(message)
    .setPositiveButton("OK", new DialogInterface.OnClickListener() {
            public void onClick(DialogInterface dialog, int id) {
        }})
    .create().show();
}
```

## <a name="frame-faces-in-the-image"></a>Enquadrar rostos na imagem

Insira o seguinte método auxiliar na classe `MainActivity`. Esse método desenha um retângulo em torno de cada rosto detetado.

```java
private static Bitmap drawFaceRectanglesOnBitmap(
        Bitmap originalBitmap, Face[] faces) {
    Bitmap bitmap = originalBitmap.copy(Bitmap.Config.ARGB_8888, true);
    Canvas canvas = new Canvas(bitmap);
    Paint paint = new Paint();
    paint.setAntiAlias(true);
    paint.setStyle(Paint.Style.STROKE);
    paint.setColor(Color.RED);
    paint.setStrokeWidth(10);
    if (faces != null) {
        for (Face face : faces) {
            FaceRectangle faceRectangle = face.faceRectangle;
            canvas.drawRect(
                    faceRectangle.left,
                    faceRectangle.top,
                    faceRectangle.left + faceRectangle.width,
                    faceRectangle.top + faceRectangle.height,
                    paint);
        }
    }
    return bitmap;
}
```

Conclua os métodos `AsyncTask`, indicados pelos comentários `TODO`, no método `detectAndFrame`. Em caso de êxito, a imagem selecionada é apresentada com os rostos enquadrados no `ImageView`.

```java
@Override
protected void onPreExecute() {
    detectionProgressDialog.show();
}
@Override
protected void onProgressUpdate(String... progress) {
    detectionProgressDialog.setMessage(progress[0]);
}
@Override
protected void onPostExecute(Face[] result) {
    detectionProgressDialog.dismiss();
    if(!exceptionMessage.equals("")){
        showError(exceptionMessage);
    }
    if (result == null) return;
    ImageView imageView = findViewById(R.id.imageView1);
    imageView.setImageBitmap(
            drawFaceRectanglesOnBitmap(imageBitmap, result));
    imageBitmap.recycle();
}
```

Por fim, no método `onActivityResult`, anule os comentários à chamada para o método `detectAndFrame`, conforme mostrado abaixo.

```java
@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    super.onActivityResult(requestCode, resultCode, data);

    if (requestCode == PICK_IMAGE && resultCode == RESULT_OK &&
                data != null && data.getData() != null) {
        Uri uri = data.getData();
        try {
            Bitmap bitmap = MediaStore.Images.Media.getBitmap(
                    getContentResolver(), uri);
            ImageView imageView = findViewById(R.id.imageView1);
            imageView.setImageBitmap(bitmap);

            // Uncomment
            detectAndFrame(bitmap);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

## <a name="run-the-app"></a>Executar a aplicação

Execute a aplicação e procure uma imagem com um rosto. Aguarde alguns segundos para permitir que o serviço Face responda. Depois disso, obterá um resultado semelhante à imagem abaixo:

![GettingStartAndroid](../Images/android_getstarted2.1.PNG)

## <a name="summary"></a>Resumo

Neste tutorial, aprendeu o processo básico para utilizar o serviço Face e criou uma aplicação para apresentar rostos enquadrados numa imagem.

## <a name="next-steps"></a>Passos seguintes

Saiba mais sobre como detetar e utilizar as particularidades do rosto.

> [!div class="nextstepaction"]
> [Como Detetar Rostos numa Imagem](../Face-API-How-to-Topics/HowtoDetectFacesinImage.md)

Explore as APIs Face utilizadas para detetar rostos e os respetivos atributos, como a posição, o género, a idade, a posição da cabeça, o pelo facial e óculos.

> [!div class="nextstepaction"]
> [Referência da API Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).