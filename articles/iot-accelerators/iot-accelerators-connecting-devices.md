---
title: Aprovisionar dispositivos do Windows para monitorização remota em C - Azure | Documentos da Microsoft
description: Descreve como ligar um dispositivo para o solution accelerator monitorização remota usando um aplicativo escrito em C em execução no Windows.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 03/14/2018
ms.author: dobett
ms.openlocfilehash: 139daea3e885636b352d4c9a1ba2651a24195b21
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38309875"
---
# <a name="connect-your-device-to-the-remote-monitoring-solution-accelerator-windows"></a>Ligar o seu dispositivo para o acelerador de solução de monitorização remota (Windows)

[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

Este tutorial mostra-lhe como ligar um dispositivo físico para o acelerador de solução de monitorização remota.

## <a name="create-a-c-client-solution-on-windows"></a>Criar uma solução de cliente do C em Windows

Tal como acontece com aplicativos mais incorporados que são executadas em dispositivos restritos, o código de cliente para a aplicação de dispositivo é escrito em C. Neste tutorial, vai criar a aplicação num computador a executar o Windows.

### <a name="create-the-starter-project"></a>Criar o projeto de introdução

Crie um projeto de introdução no Visual Studio 2017 e adicione os pacotes de NuGet de cliente de dispositivo do IoT Hub:

1. No Visual Studio, crie uma aplicação de consola de C com o Visual C++ **aplicação de consola do Windows** modelo. Nomeie o projeto **RMDevice**.

    ![Criar aplicação de consola do Windows do Visual C++](./media/iot-accelerators-connecting-devices/visualstudio01.png)

1. Na **Explorador de soluções**, elimine os ficheiros `stdafx.h`, `targetver.h`, e `stdafx.cpp`.

1. Na **Explorador de soluções**, renomeie o arquivo `RMDevice.cpp` para `RMDevice.c`.

    ![Mostrar Explorador de soluções renomeado RMDevice.c ficheiro](./media/iot-accelerators-connecting-devices/visualstudio02.png)

1. Na **Explorador de soluções**, clique com botão direito a **RMDevice** do projeto e, em seguida, clique em **gerir pacotes NuGet**. Escolher **procurar**, em seguida, procure e instale os seguintes pacotes de NuGet:

    * Microsoft.Azure.IoTHub.Serializer
    * Microsoft.Azure.IoTHub.IoTHubClient
    * Microsoft.Azure.IoTHub.MqttTransport

    ![Gestor de pacotes NuGet mostra os pacotes de Microsoft.Azure.IoTHub instalados](./media/iot-accelerators-connecting-devices/visualstudio03.png)

1. Na **Explorador de soluções**, clique com o botão direito no **RMDevice** do projeto e, em seguida, escolha **propriedades** para abrir o projeto **páginas de propriedades** caixa de diálogo. Para obter detalhes, consulte [propriedades da definição Visual C++ projeto](https://docs.microsoft.com/cpp/ide/working-with-project-properties).

1. Escolha o **C/C++** pasta, em seguida, escolha a **pré-compiladas cabeçalhos** página de propriedades.

1. Definir **pré-compiladas cabeçalho** ao **não utilizar pré-compiladas cabeçalhos**. Em seguida, escolha **aplicar**.

    ![Propriedades do projeto mostram não o uso de cabeçalhos pré-compilado do projeto](./media/iot-accelerators-connecting-devices/visualstudio04.png)

1. Escolha o **Linker** pasta, em seguida, escolha a **entrada** página de propriedades.

1. Adicione `crypt32.lib` para o **dependências adicionais** propriedade. Para guardar o projeto de valores de propriedade, escolha **OK** e, em seguida **OK** novamente.

    ![Propriedades do projeto mostram vinculador incluindo crypt32.lib](./media/iot-accelerators-connecting-devices/visualstudio05.png)

### <a name="add-the-parson-json-library"></a>Adicionar a biblioteca de Parson JSON

Adicionar a biblioteca de Parson JSON para o **RMDevice** do projeto e adicione o necessário `#include` instruções:

1. Numa pasta adequada no seu computador, clone o repositório de Parson GitHub utilizando o seguinte comando:

    ```cmd
    git clone https://github.com/kgabis/parson.git
    ```

1. Copiar o `parson.h` e `parson.c` ficheiros a partir da cópia local do repositório Parson a sua **RMDevice** pasta do projeto.

1. No Visual Studio, clique com botão direito a **RMDevice** do projeto, escolha **Add**e, em seguida, escolha **Item existente**.

1. Na **Adicionar Item existente** caixa de diálogo, selecione a `parson.h` e `parson.c` arquivos no **RMDevice** pasta do projeto. Para adicionar esses dois arquivos para seu projeto, escolha **adicionar**.

    ![Explorador de soluções mostra arquivos parson.h e parson.c](./media/iot-accelerators-connecting-devices/visualstudio06.png)

1. No Visual Studio, abra o `RMDevice.c` ficheiro. Substituir o existente `#include` instruções com o código a seguir:

    ```c
    #include "iothubtransportmqtt.h"
    #include "schemalib.h"
    #include "iothub_client.h"
    #include "serializer_devicetwin.h"
    #include "schemaserializer.h"
    #include "azure_c_shared_utility/threadapi.h"
    #include "azure_c_shared_utility/platform.h"
    #include <string.h>
    ```

    > [!NOTE]
    > Agora pode verificar que o seu projeto tem as dependências corretas, configuradas a criar a solução.

[!INCLUDE [iot-suite-connecting-code](../../includes/iot-suite-connecting-code.md)]

## <a name="build-and-run-the-sample"></a>Criar e executar o exemplo

Adicionar código para invocar a **remoto\_monitoramento\_executar** funcione, em seguida, criar e executar a aplicação de dispositivo:

1. Para invocar a **remoto\_monitorização\_execute** funcionar, substitua o **principal** função com o código a seguir:

    ```c
    int main()
    {
      remote_monitoring_run();
      return 0;
    }
    ```

1. Escolher **crie** e, em seguida **compilar solução** para criar a aplicação de dispositivo.

1. No **Explorador de soluções**, clique com botão direito a **RMDevice** do projeto, escolha **depurar**e, em seguida, selecione **iniciar nova instância** para executar o exemplo . A consola apresenta as mensagens como:

    * A aplicação envia telemetria de exemplo para o solution accelerator.
    * Recebe os valores de propriedades pretendidas definidos no dashboard da solução.
    * Responde a métodos invocados a partir do dashboard da solução.

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]
