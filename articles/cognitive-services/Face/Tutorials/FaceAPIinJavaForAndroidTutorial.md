---
title: Enfrentam API Java para Android tutorial | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Criar uma aplicação Android simple que utiliza a API de rostos em serviços cognitivos para detetar e humanos de moldura enfrenta numa imagem.
services: cognitive-services
author: SteveMSFT
manager: corncar
ms.service: cognitive-services
ms.component: face-api
ms.topic: article
ms.date: 03/01/2018
ms.author: sbowles
ms.openlocfilehash: 5164a261d482d0cca3842a973d2109b17999bd25
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355189"
---
# <a name="getting-started-with-face-api-in-java-for-android-tutorial"></a>Introdução Android Tutorial de rostos em API em Java

Neste tutorial, irá aprender a criar e desenvolver uma aplicação Android simples que invoca a API de rostos em detetar faces humanos numa imagem. A aplicação apresenta o resultado pelo framing faces que Deteta.

![GettingStartedAndroid](../Images/android_getstarted2.1.PNG)

## <a name="preparation"></a> Preparação

Para utilizar o tutorial, terá dos seguintes pré-requisitos:

- Android Studio e SDK instalado
- Dispositivo Android (opcional para fins de teste).

## <a name="step1"></a>Passo 1: Subscrever para enfrentam API e obter a chave de subscrição

Antes de utilizar qualquer API de letra, deve inscrever-se para subscrever enfrentam API no portal de serviços cognitivos da Microsoft. Consulte [subscrições](https://azure.microsoft.com/try/cognitive-services/). Chave primária e secundária pode ser utilizada neste tutorial.

## <a name="step2"></a>Passo 2: Criar a estrutura da aplicação

Neste passo irá criar um projeto de aplicação Android para implementar a IU básica diretriz cópias de segurança e apresentar uma imagem. Simplesmente, siga as instruções abaixo: 

1. Abra o Android Studio.
2. No menu ficheiro, clique em **novo projeto...**
3. Atribua um nome de aplicação **MyFirstApp**e, em seguida, clique em seguinte. 

    ![GettingStartAndroidNewProject](../Images/AndroidNewProject.png)

4. Escolha a plataforma de destino, conforme necessário e, em seguida, clique em seguinte. 

    ![GettingStartAndroidNewProject2](../Images/AndroidNewProject2.png)

5. Selecione **atividade básico** e, em seguida, clique em seguinte.
6. Nome da atividade da seguinte forma e, em seguida, clique em Concluir. 

    ![GettingStartAndroidNewProject4](../Images/AndroidNewProject4.png)

7. Abra **ctivity_main.XML**, deverá ver o Editor de esquema desta atividade.
8. Ver o ficheiro de texto de origem e, em seguida, edite o esquema de atividade da seguinte forma:

    ```xml
    <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:tools="http://schemas.android.com/tools" android:layout_width="match_parent"
        android:layout_height="match_parent" android:paddingLeft="@dimen/activity_horizontal_margin"
        android:paddingRight="@dimen/activity_horizontal_margin"
        android:paddingTop="@dimen/activity_vertical_margin"
        android:paddingBottom="@dimen/activity_vertical_margin" tools:context=".MainActivity">
     
        <ImageView
            android:layout_width="match_parent"
            android:layout_height="fill_parent"
            android:id="@+id/imageView1"
            android:layout_above="@+id/button1" />
    
        <Button
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="Browse"
            android:id="@+id/button1"
            android:layout_alignParentBottom="true" />
    </RelativeLayout>
    ```  

9. Abra **MainActivity.java** e inserir as diretivas de importação seguintes no início do ficheiro:

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
    ```
      
    Lugar, modifique a classe da seguinte forma:  
    
    ```java
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
                Intent gallIntent = new Intent(Intent.ACTION_GET_CONTENT);
                gallIntent.setType("image/*");
                startActivityForResult(Intent.createChooser(gallIntent, "Select Picture"), PICK_IMAGE);
            }
        });
         
        detectionProgressDialog = new ProgressDialog(this);
    }
    
    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        if (requestCode == PICK_IMAGE && resultCode == RESULT_OK && data != null && data.getData() != null) {
            Uri uri = data.getData();
            try {
                Bitmap bitmap = MediaStore.Images.Media.getBitmap(getContentResolver(), uri);
                ImageView imageView = (ImageView) findViewById(R.id.imageView1);
                imageView.setImageBitmap(bitmap);
                } catch (IOException e) {
                e.printStackTrace();
                }
        }
    }
    ```

Agora a aplicação pode procurar uma fotografia na galeria do e apresente-a na janela semelhante para a imagem abaixo:

![GettingStartAndroidUI](../Images/android_getstarted1.1.PNG)

## <a name="step3"></a>Passo 3: Configurar a biblioteca de cliente de API de rostos em

A API de letra é uma nuvem pedidos de API que possa invocar através de HTTPS. Uma forma mais conveniente de utilizar a API de rostos nas aplicações da plataforma de .NET, uma biblioteca de cliente também é fornecida para encapsular web pedidos. Neste exemplo, utilizamos a biblioteca de clientes para simplificar a nossa trabalho. 

Siga as instruções abaixo para configurar a biblioteca de clientes: 

1. Localize o nível superior **gradle** ficheiro do seu projeto a partir do painel do projeto mostrado no exemplo. Tenha em atenção que existem vários outros **gradle** precisam de ficheiros no seu projeto de árvore e abrir o nível superior **gradle** ficheiro em primeiro lugar.
2. Adicionar **mavenCentral()** para repositórios dos seus projetos. Também pode utilizar jcenter(), que é o repositório de predefinição do Android Studio, uma vez que jcenter() é um superconjunto da mavenCentral().  

```
    allprojects {
        repositories {
            ...
            mavenCentral()
        }
    }
```

3. Abra o **gradle** ficheiros no seu projeto 'aplicação'.
4. Adicione uma dependência para a nossa biblioteca de clientes armazenada no repositório Central Maven:

```
    dependencies {  
        ...  
        implementation 'com.microsoft.projectoxford:face:1.4.3'  
    }
```

5. Abra **MainActivity.java** no seu projeto de "app" e inserir o seguinte importa as diretivas de: 
    
    ```java
    import com.microsoft.projectoxford.face.*;  
    import com.microsoft.projectoxford.face.contract.*;  
    ```
    
   Em seguida, insira o seguinte código na classe:

    ```java
    private FaceServiceClient faceServiceClient = new FaceServiceRestClient("your API endpoint", "<Subscription Key>");
    ```

   Substitua o primeiro parâmetro acima com o ponto final de API que foi atribuído à sua chave no passo 1. Por exemplo:
   
        https://eastus2.api.cognitive.microsoft.com/face/v1.0
   
   Substitua o segundo parâmetro com a chave de subscrição que obteve no passo 1.
   
6. Abra o ficheiro **AndroidManifest.xml** no seu projeto 'aplicação'. Insira o elemento seguinte como elemento subordinado do **manifesto** elemento:  

    ```xml
    <uses-permission android:name="android.permission.INTERNET" />  
    ```

7. Agora, está pronto para chamar a API de rosto da sua aplicação. 

## <a name="step4"></a>Passo 4: Carregar imagens para detetar faces

A forma mais simples para detetar faces é ao chamar o [enfrentam – detetar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) API através do carregamento diretamente o ficheiro de imagem. Quando utilizar a biblioteca de clientes, isto pode ser feito utilizando o método assíncrono **DetectAsync** do **FaceServiceClient** classe. Cada enfrentam devolvido contém um retângulo para indicar a localização, combinada com uma série de atributos de rostos em opcional. Neste exemplo, precisamos apenas de obter a localização de letra. Aqui, é necessário inserir um método para o **MainActivity** classe para a deteção de rostos em: 

```java

    // Detect faces by uploading face images
    // Frame faces after detection
    
    private void detectAndFrame(final Bitmap imageBitmap)
    {
        ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
        imageBitmap.compress(Bitmap.CompressFormat.JPEG, 100, outputStream);
        ByteArrayInputStream inputStream = 
            new ByteArrayInputStream(outputStream.toByteArray());
        AsyncTask<InputStream, String, Face[]> detectTask =
            new AsyncTask<InputStream, String, Face[]>() {
                @Override
                protected Face[] doInBackground(InputStream... params) {
                    try {
                        publishProgress("Detecting...");
                        Face[] result = faceServiceClient.detect(
                                params[0], 
                                true,         // returnFaceId
                                false,        // returnFaceLandmarks
                                null           // returnFaceAttributes: a string like "age, gender"
                /* If you want value of FaceAttributes, try adding 4th argument like below.
                            new FaceServiceClient.FaceAttributeType[] {
                    FaceServiceClient.FaceAttributeType.Age,
                    FaceServiceClient.FaceAttributeType.Gender }
                */              
                        );
                        if (result == null)
                        {
                            publishProgress("Detection Finished. Nothing detected");
                            return null;
                        }
                        publishProgress(
                                String.format("Detection Finished. %d face(s) detected",
                                        result.length));
                        return result;
                    } catch (Exception e) {
                        publishProgress("Detection failed");
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
```

## <a name="step5"></a>Passo 5: Marca enfrenta na imagem

Neste último passo, vamos combinar todos os passos acima em conjunto e marcar os faces detetados com frames na imagem. Primeiro, abra **MainActivity.java** e inserir um método de programa auxiliar para desenhar retângulos: 

```java
    private static Bitmap drawFaceRectanglesOnBitmap(Bitmap originalBitmap, Face[] faces) {
        Bitmap bitmap = originalBitmap.copy(Bitmap.Config.ARGB_8888, true);
        Canvas canvas = new Canvas(bitmap);
        Paint paint = new Paint();
        paint.setAntiAlias(true);
        paint.setStyle(Paint.Style.STROKE);
        paint.setColor(Color.RED);
        int stokeWidth = 2;
        paint.setStrokeWidth(stokeWidth);
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

Agora concluir as partes TODO no **detectAndFrame** método para moldura faces e comunicar estado.

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
        if (result == null) return;
        ImageView imageView = (ImageView)findViewById(R.id.imageView1);
        imageView.setImageBitmap(drawFaceRectanglesOnBitmap(imageBitmap, result));
        imageBitmap.recycle();
    }
```
 
Por fim, adicione uma chamada para o **detectAndFrame** método o **onActivityResult** método, como mostrado abaixo. 

```java
    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        if (requestCode == PICK_IMAGE && resultCode == RESULT_OK && data != null && data.getData() != null) {
            Uri uri = data.getData();
            try {
                Bitmap bitmap = MediaStore.Images.Media.getBitmap(getContentResolver(), uri);
                ImageView imageView = (ImageView) findViewById(R.id.imageView1);
                imageView.setImageBitmap(bitmap);
     
                // This is the new addition.
                // detectAndFrame(bitmap);
     
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
```

Execute esta aplicação e procurar por uma imagem que contém um tipo de letra. Aguarde alguns segundos permitir que a nuvem API para responder. Depois disso, irá obter um resultado semelhante para a imagem abaixo: 

![GettingStartAndroid](../Images/android_getstarted2.1.PNG)

## <a name="summary"></a> Resumo

Neste tutorial, aprendeu o processo básico para utilizar a API de rostos em e criar uma aplicação para apresentar marcas de rostos em imagens. Para obter mais informações sobre a API de letra, consulte os procedimentos e [referência da API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236). 

## <a name="related"></a> Tutoriais relacionados

- [Introdução à API de letra CSharp tutorial](FaceAPIinCSharpTutorial.md)
- [Introdução à API de letra tutorial do Python](FaceAPIinPythonTutorial.md)
