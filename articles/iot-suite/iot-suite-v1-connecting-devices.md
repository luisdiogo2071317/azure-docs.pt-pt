---
title: Ligar um dispositivo com o C no Windows | Documentos da Microsoft
description: Descreve como ligar um dispositivo à solução de monitorização remota do Azure IoT Suite pré-configurada com um aplicativo escrito em C em execução no Windows.
services: ''
suite: iot-suite
documentationcenter: na
author: dominicbetts
manager: timlt
editor: ''
ms.assetid: 34e39a58-2434-482c-b3fa-29438a0c05e8
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2017
ms.author: dobett
ms.openlocfilehash: 56c58a31c6b20bd8da7d1442ae75425cb3e20f3d
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/25/2018
ms.locfileid: "47093100"
---
# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-windows"></a>Ligar o dispositivo à solução pré-configurada monitorização remota (Windows)
[!INCLUDE [iot-suite-v1-selector-connecting](../../includes/iot-suite-v1-selector-connecting.md)]

## <a name="create-a-c-sample-solution-on-windows"></a>Criar uma solução de exemplo do C em Windows
Os passos seguintes mostram como criar uma aplicação cliente que comunica com a solução pré-configurada de monitorização remota. Esse aplicativo é escrito em C e criado e executado no Windows.

Criar um projeto de introdução no Visual Studio 2015 ou Visual Studio 2017 e adicione os pacotes de NuGet de cliente de dispositivo do IoT Hub:

1. No Visual Studio, crie uma aplicação de consola de C com o Visual C++ **aplicação de consola Win32** modelo. Nomeie o projeto **RMDevice**.
2. Na **as definições da aplicação** página no **Assistente de aplicação Win32**, certifique-se de que **aplicação de consola** está selecionada e desmarque **Precompiled cabeçalho** e **Security Development Lifecycle (SDL) verifica**.
3. Na **Explorador de soluções**, elimine os ficheiros stdafx, targetver.h e stdafx.cpp.
4. Na **Explorador de soluções**, renomeie o arquivo RMDevice.cpp para RMDevice.c.
5. Na **Explorador de soluções**, clique com o botão direito no **RMDevice** do projeto e, em seguida, clique em **gerir pacotes NuGet**. Clique em **procurar**, em seguida, procure e instale os seguintes pacotes de NuGet:
   
   * Microsoft.Azure.IoTHub.Serializer
   * Microsoft.Azure.IoTHub.IoTHubClient
   * Microsoft.Azure.IoTHub.MqttTransport
6. Na **Explorador de soluções**, clique com o botão direito no **RMDevice** do projeto e, em seguida, clique em **propriedades** para abrir o projeto **páginas de propriedades** caixa de diálogo. Para obter detalhes, consulte [definição Visual C++ Project Properties][lnk-c-project-properties]. 
7. Clique nas **vinculador** pasta, em seguida, clique no **entrada** página de propriedades.
8. Adicione **crypt32.lib** para o **dependências adicionais** propriedade. Clique em **OK** e, em seguida **OK** novamente para guardar o projeto de valores de propriedade.

Adicionar a biblioteca de Parson JSON para o **RMDevice** do projeto e adicione o necessário `#include` instruções:

1. Numa pasta adequada no seu computador, clone o repositório de Parson GitHub utilizando o seguinte comando:

    ```
    git clone https://github.com/kgabis/parson.git
    ```

1. Copiar os arquivos parson.h e parson.c da cópia local do repositório Parson a sua **RMDevice** pasta do projeto.

1. No Visual Studio, clique com botão direito a **RMDevice** do projeto, clique em **Add**e, em seguida, clique em **Item existente**.

1. Na **Adicionar Item existente** caixa de diálogo, selecione o parson.h e parson.c arquivos no **RMDevice** pasta do projeto. Em seguida, clique em **adicionar** para adicionar esses dois arquivos para seu projeto.

1. No Visual Studio, abra o ficheiro de RMDevice.c. Substituir o existente `#include` instruções com o código a seguir:
   
    ```c
    #include "iothubtransportmqtt.h"
    #include "schemalib.h"
    #include "iothub_client.h"
    #include "serializer_devicetwin.h"
    #include "schemaserializer.h"
    #include "azure_c_shared_utility/threadapi.h"
    #include "azure_c_shared_utility/platform.h"
    #include "parson.h"
    ```

    > [!NOTE]
    > Agora pode verificar que o seu projeto tem as dependências corretas configuradas ao criá-lo.

[!INCLUDE [iot-suite-v1-connecting-code](../../includes/iot-suite-v1-connecting-code.md)]

## <a name="build-and-run-the-sample"></a>Criar e executar o exemplo

Adicionar código para invocar a **remoto\_monitoramento\_executar** de função e, em seguida, criar e executar a aplicação de dispositivo.

1. Substitua a **principal** função com o seguinte código para invocar a **remoto\_monitorização\_executar** função:
   
    ```c
    int main()
    {
      remote_monitoring_run();
      return 0;
    }
    ```

1. Clique em **crie** e, em seguida **compilar solução** para criar a aplicação de dispositivo.

1. Na **Explorador de soluções**, clique com botão direito a **RMDevice** do projeto, clique em **depurar**e, em seguida, clique em **iniciar nova instância** para executar o exemplo. A consola apresenta as mensagens que a aplicação envia telemetria de exemplo para a solução pré-configurada, recebe os valores de propriedades pretendidas definidos no dashboard da solução e responde a métodos invocados a partir do dashboard da solução.

[!INCLUDE [iot-suite-v1-visualize-connecting](../../includes/iot-suite-v1-visualize-connecting.md)]

[lnk-c-project-properties]: https://msdn.microsoft.com/library/669zx6zc.aspx
