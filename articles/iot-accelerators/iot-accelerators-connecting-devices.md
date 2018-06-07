---
title: Aprovisionar dispositivos do Windows para monitorização remota no C - do Azure | Microsoft Docs
description: Descreve como ligar um dispositivo para o acelerador solução de monitorização remota, utilizando uma aplicação de escrita no C em execução no Windows.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 03/14/2018
ms.author: dobett
ms.openlocfilehash: 139daea3e885636b352d4c9a1ba2651a24195b21
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34626960"
---
# <a name="connect-your-device-to-the-remote-monitoring-solution-accelerator-windows"></a>Ligar o seu dispositivo para o acelerador de solução de monitorização remota (Windows)

[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

Este tutorial mostra como ligar um dispositivo físico para o acelerador de solução de monitorização remota.

## <a name="create-a-c-client-solution-on-windows"></a>Criar uma solução de cliente do C no Windows

Tal como acontece com as aplicações mais incorporadas que são executadas em dispositivos restrita, o código de cliente para a aplicação de dispositivo é escrito nos C. Neste tutorial, tem de compilar a aplicação num computador com o Windows.

### <a name="create-the-starter-project"></a>Criar o projeto de arranque

Criar um projeto de arranque no Visual Studio 2017 e adicionar os pacotes de NuGet do cliente de dispositivo IoT Hub:

1. No Visual Studio, crie uma aplicação de consola C utilizando o Visual C++ **aplicação de consola do Windows** modelo. Nomeie o projeto **RMDevice**.

    ![Criar aplicação de consola do Windows do Visual C++](./media/iot-accelerators-connecting-devices/visualstudio01.png)

1. No **Explorador de soluções**, elimine os ficheiros `stdafx.h`, `targetver.h`, e `stdafx.cpp`.

1. No **Explorador de soluções**, mude o nome de ficheiro `RMDevice.cpp` para `RMDevice.c`.

    ![Apresentação do Explorador de soluções mudar o nome do ficheiro de RMDevice.c](./media/iot-accelerators-connecting-devices/visualstudio02.png)

1. No **Explorador de soluções**, clique com botão direito do **RMDevice** projeto e, em seguida, clique em **pacotes NuGet gerir**. Escolha **procurar**, em seguida, procure e instale os seguintes pacotes de NuGet:

    * Microsoft.Azure.IoTHub.Serializer
    * Microsoft.Azure.IoTHub.IoTHubClient
    * Microsoft.Azure.IoTHub.MqttTransport

    ![Gestor de pacotes NuGet mostra pacotes Microsoft.Azure.IoTHub instalados](./media/iot-accelerators-connecting-devices/visualstudio03.png)

1. No **Explorador de soluções**, faça duplo clique no **RMDevice** projeto e, em seguida, escolha **propriedades** para abrir o projeto **páginas de propriedades** caixa de diálogo. Para obter mais informações, consulte [propriedades da definição Visual C++ projeto](https://docs.microsoft.com/cpp/ide/working-with-project-properties).

1. Escolha o **C/C++** pasta, em seguida, escolha o **pré-compilada cabeçalhos** página de propriedades.

1. Definir **pré-compilada cabeçalho** para **não utilizar pré-compilada cabeçalhos**. Em seguida, escolha **aplicar**.

    ![Propriedades do projeto mostram projeto não utilizar pré-compilada cabeçalhos](./media/iot-accelerators-connecting-devices/visualstudio04.png)

1. Escolha o **Linker** pasta, em seguida, escolha o **entrada** página de propriedades.

1. Adicionar `crypt32.lib` para o **dependências adicionais** propriedade. Para guardar o projeto de valores de propriedade, escolha **OK** e, em seguida, **OK** novamente.

    ![Propriedades do projeto mostram Linker incluindo crypt32.lib](./media/iot-accelerators-connecting-devices/visualstudio05.png)

### <a name="add-the-parson-json-library"></a>Adicionar a biblioteca de Parson JSON

Adicionar a biblioteca de Parson JSON para o **RMDevice** projeto e adicione necessários `#include` instruções:

1. Numa pasta adequada no seu computador, clone o repositório do Parson GitHub utilizando o seguinte comando:

    ```cmd
    git clone https://github.com/kgabis/parson.git
    ```

1. Copiar o `parson.h` e `parson.c` ficheiros a partir da cópia local do repositório Parson a sua **RMDevice** pasta do projeto.

1. No Visual Studio, clique com botão direito do **RMDevice** do projeto, escolha **adicionar**e, em seguida, escolha **Item existente**.

1. No **Adicionar Item existente** caixa de diálogo, selecione o `parson.h` e `parson.c` ficheiros no **RMDevice** pasta do projeto. Para adicionar estes dois ficheiros ao seu projeto, escolha **adicionar**.

    ![Explorador de soluções mostra ficheiros parson.h e parson.c](./media/iot-accelerators-connecting-devices/visualstudio06.png)

1. No Visual Studio, abra o `RMDevice.c` ficheiro. Substituir a existente `#include` instruções com o seguinte código:

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
    > Agora pode verificar se o seu projeto tem as dependências corretas configuradas ao compilar a solução.

[!INCLUDE [iot-suite-connecting-code](../../includes/iot-suite-connecting-code.md)]

## <a name="build-and-run-the-sample"></a>Criar e executar o exemplo

Adicionar código para invocar a **remoto\_monitorização\_executar** funcione, em seguida, criar e executar a aplicação de dispositivo:

1. Para invocar a **remoto\_monitorização\_executar** funcionar, substitua o **principal** função com o código seguinte:

    ```c
    int main()
    {
      remote_monitoring_run();
      return 0;
    }
    ```

1. Escolha **criar** e, em seguida, **compilar solução** para criar a aplicação de dispositivo.

1. No **Explorador de soluções**, faça duplo clique o **RMDevice** do projeto, escolha **depurar**e, em seguida, escolha **iniciar nova instância** para executar o exemplo . A consola apresenta mensagens como:

    * A aplicação envia a telemetria de exemplo para o acelerador de solução.
    * Recebe os valores de propriedade pretendido definidos no dashboard de solução.
    * Responde a métodos invocados a partir do dashboard de solução.

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]
