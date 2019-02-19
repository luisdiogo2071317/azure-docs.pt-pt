---
title: Como utilizar o controlo de mapas de Android no Azure Maps | Documentos da Microsoft
description: Utilize o controlo de mapas de Android no Azure Maps.
author: walsehgal
ms.author: v-musehg
ms.date: 02/12/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 57cc585d621c71872a4b7658c74f581c8998b245
ms.sourcegitcommit: fcb674cc4e43ac5e4583e0098d06af7b398bd9a9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/18/2019
ms.locfileid: "56341084"
---
# <a name="how-to-use-azure-maps-android-sdk"></a>Como utilizar o SDK Android do Azure Maps

O SDK do Azure Maps Android é uma biblioteca de mapas de vetor para Android. Este artigo irá guiá-lo pelo processo de instalação do SDK Android do Azure Maps, para carregar um mapa e colocar um pin no mesmo.

## <a name="prerequisites-to-get-started"></a>Pré-requisitos para começar a utilizar

### <a name="create-an-azure-maps-account"></a>Criar uma conta do Azure Maps 

Para seguir os passos neste guia, tem primeiro de ver [gerir conta e chaves](how-to-manage-account-keys.md) para criar e gerir a sua subscrição de conta com S1 escalão de preço.

### <a name="download-android-studio"></a>Transferir o Android studio

Pode baixar [Android Studio](https://developer.android.com/studio/) gratuitamente a partir do Google. Para instalar o SDK Android do Azure Maps, terá primeiro de transferir o Android Studio e criar um projeto com uma atividade vazia.

## <a name="create-a-project-in-android-studio"></a>Criar um projeto no Android Studio

Terá de criar um novo projeto com uma atividade vazia. Siga os passos abaixo para criar um novo projeto do Android Studio:

1. Sob *escolha o seu projeto*, verificar "Telefone e Tablet" como fator de forma que seu aplicativo será executado em.
2. Clique em *atividade vazia* sob fator forma e clique em **próxima**.
3. Sob *configurar o seu projeto*, selecione `API 21: Android 5.0.0 (Lollipop)` como o SDK mínimo. Esta é a versão mais baixa suportada pelo SDK Android do Azure Maps.
4. Aceite a predefinição `Activity Name` e `Layout Name` e clique em **concluir**

Ver [documentação do Android Studio](https://developer.android.com/studio/intro/) para obter mais ajuda para instalar o Android Studio e criando um novo projeto.

![Criar um novo projeto](./media/how-to-use-android-map-control-library/form-factor-android.png)

## <a name="set-up-a-virtual-device"></a>Configurar um dispositivo virtual

Android Studio permite-lhe configurar um dispositivo virtual Android no seu computador. Que pode ajudar a testar seu aplicativo enquanto programa. Para configurar um dispositivo virtual, clique no ícone Manager do dispositivo Virtual Android (AVD) no canto superior direito do ecrã do seu projeto. Em seguida, clique nas **criar dispositivo Virtual** botão. Também pode obter para o Gestor de via **ferramentas > Android > Gestor de AVD** na barra de ferramentas. Partir do **telemóveis** categoria, selecione **Nexus 5 X** e clique em **seguinte**.

Saiba mais sobre como configurar um AVD no [documentação do Android Studio](https://developer.android.com/studio/run/managing-avds).

![Emulador do Android](./media/how-to-use-android-map-control-library/android-emulator.png)

## <a name="install-azure-maps-android-sdk"></a>Instalar o SDK do Android do Azure Maps

Antes de avançarmos para criar a sua aplicação, siga os passos abaixo para instalar o SDK Android do Azure Maps. 

1. Adicione o seguinte para o **todos os projetos**, repositórios de bloco no seu **gradle** ficheiro.

    ```
    maven {
            url "https://atlas.microsoft.com/sdk/android"
    }
    ```

2. Atualização de seus **gradle** e adicione o seguinte:

    1. Adicione o seguinte para o bloco de Android:

        ```
        compileOptions {
            sourceCompatibility JavaVersion.VERSION_1_8
            targetCompatibility JavaVersion.VERSION_1_8
        }
        ```
    2. Atualize seu bloco de dependências e adicione o seguinte:

        ```
        implementation "com.microsoft.azure.maps:mapcontrol:0.1"
        ```

3. Configurar permissões ao adicionar o seguinte para sua **androidmanifest. XML**

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <manifest>
        ...
        <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
        ...
    </manifest>
    ```

4. Editar **res > layout > ctivity_main**, por isso, ele se parece com o XML abaixo:
    
    ```XML
    <?xml version="1.0" encoding="utf-8"?>
    <FrameLayout
        xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        >

        <com.microsoft.azure.maps.mapcontrol.MapControl
            android:id="@+id/mapcontrol"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            app:mapcontrol_cameraTargetLat="47.64"
            app:mapcontrol_cameraTargetLng="-122.33"
            app:mapcontrol_cameraZoom="12"
            />

    </FrameLayout>
    ```

5. Editar **mainactivity. Java** para criar uma classe de atividade de vista do mapa. Depois de editar deve ser semelhante a classe abaixo:

    ```java
    package com.example.myapplication;

    import android.support.v7.app.AppCompatActivity;
    import android.os.Bundle;
    import com.microsoft.azure.maps.mapcontrol.AzureMaps;
    import com.microsoft.azure.maps.mapcontrol.MapControl;
    import com.microsoft.azure.maps.mapcontrol.layer.SymbolLayer;
    import com.microsoft.azure.maps.mapcontrol.options.MapStyle;
    import com.microsoft.azure.maps.mapcontrol.source.DataSource;

    public class MainActivity extends AppCompatActivity {
        
        static {
            AzureMaps.setSubscriptionKey("{subscription-key}");
        }

        MapControl mapControl;

        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);

            mapControl = findViewById(R.id.mapcontrol);

            mapControl.onCreate(savedInstanceState);

        }

        @Override
        public void onResume() {
            super.onResume();
            mapControl.onResume();
        }

        @Override
        public void onPause() {
            super.onPause();
            mapControl.onPause();
        }

        @Override
        public void onStop() {
            super.onStop();
            mapControl.onStop();
        }

        @Override
        public void onLowMemory() {
            super.onLowMemory();
            mapControl.onLowMemory();
        }

        @Override
        protected void onDestroy() {
            super.onDestroy();
            mapControl.onDestroy();
        }

        @Override
        protected void onSaveInstanceState(Bundle outState) {
            super.onSaveInstanceState(outState);
            mapControl.onSaveInstanceState(outState);
        }

    }

    ```

## <a name="import-classes"></a>Importar Classes

Depois de concluir os passos acima, provavelmente receberá avisos do Android Studio em alguns do texto no código. Para resolver estes avisos, importar as classes que está a ser referenciadas no `MainActivity.java`.

Pode importar automaticamente essas classes, premindo `Alt` + `Enter`(`Option` + `Return` no Mac). 

Clique nas **executar "Aplicação"** botão (ou `Control` + `R` num Mac) para criar seu aplicativo.

![Clique em executar](./media/how-to-use-android-map-control-library/run-app.png)

Irá demorar alguns segundos para android studio para criar a aplicação. Após concluir a compilação pode testar a aplicação no dispositivo emulado Android. Verá um mapa como o mostrado abaixo.

![Mapa de Android](./media/how-to-use-android-map-control-library/android-map.png)

## <a name="add-a-marker-to-the-map"></a>Adicionar um marcador ao mapa

Para adicionar um marcador no seu mapa, adicione `mapView.getMapAsync()` funcionar para o `MainActivity.java`. O último `MainActivity.java` deve ter um aspeto semelhante ao seguinte:

```java
package com.example.myapplication;

import android.app.Activity;
import android.os.Bundle;
import com.mapbox.geojson.Feature;
import com.mapbox.geojson.Point;
import com.microsoft.azure.maps.mapcontrol.AzureMaps;
import com.microsoft.azure.maps.mapcontrol.MapControl;
import com.microsoft.azure.maps.mapcontrol.layer.SymbolLayer;
import com.microsoft.azure.maps.mapcontrol.source.DataSource;
import static com.microsoft.azure.maps.mapcontrol.options.SymbolLayerOptions.iconImage;
public class MainActivity extends AppCompatActivity {
    
    static{
            AzureMaps.setSubscriptionKey("{subscription-key}");
        }

    MapControl mapControl;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        mapControl = findViewById(R.id.mapcontrol);

        mapControl.onCreate(savedInstanceState);

        mapControl.getMapAsync(map -> {
            DataSource dataSource = new DataSource();
            dataSource.add(Feature.fromGeometry(Point.fromLngLat(-122.33, 47.64)));

            SymbolLayer symbolLayer = new SymbolLayer(dataSource);
            symbolLayer.setOptions(iconImage("my-icon"));

            map.images.add("my-icon", R.drawable.mapcontrol_marker_red);
            map.sources.add(dataSource);
            map.layers.add(symbolLayer);
        });
    }

    @Override
    public void onStart() {
        super.onStart();
        mapControl.onStart();
    }

    @Override
    public void onResume() {
        super.onResume();
        mapControl.onResume();
    }

    @Override
    public void onPause() {
        super.onPause();
        mapControl.onPause();
    }

    @Override
    public void onStop() {
        super.onStop();
        mapControl.onStop();
    }

    @Override
    public void onLowMemory() {
        super.onLowMemory();
        mapControl.onLowMemory();
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        mapControl.onDestroy();
    }

    @Override
    protected void onSaveInstanceState(Bundle outState) {
        super.onSaveInstanceState(outState);
        mapControl.onSaveInstanceState(outState);
    }
}
```

Volte a executar a aplicação e deverá ver um marcador no mapa, como o mostrado abaixo.

![Pin de mapa de Android](./media/how-to-use-android-map-control-library/android-map-pin.png)