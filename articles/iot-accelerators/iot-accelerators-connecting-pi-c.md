---
title: Aprovisionar o Raspberry Pi a monitorização remota com C - Azure | Documentos da Microsoft
description: Descreve como ligar um dispositivo de Raspberry Pi do solution Accelerator monitorização remota usando um aplicativo escrito em C.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 03/14/2018
ms.author: dobett
ms.openlocfilehash: 23e84a8d577bb1c4950de3acd76b0f8528551ae0
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38611446"
---
# <a name="connect-your-raspberry-pi-device-to-the-remote-monitoring-solution-accelerator-c"></a>Ligar o seu dispositivo de Raspberry Pi para o acelerador de solução de monitorização remota (C)

[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

Este tutorial mostra-lhe como ligar um dispositivo físico para o acelerador de solução de monitorização remota. Tal como acontece com aplicativos mais incorporados que são executadas em dispositivos restritos, o código de cliente para a aplicação de dispositivo Raspberry Pi é escrito em C. Neste tutorial, vai criar o aplicativo num Raspberry Pi a executar o SO Raspbian.

### <a name="required-hardware"></a>Hardware necessário

Um computador de secretária que lhe permite ligar remotamente à linha de comandos no Raspberry Pi.

[Microsoft IoT Starter Kit para Raspberry Pi 3](https://azure.microsoft.com/develop/iot/starter-kits/) ou componentes equivalentes. Este tutorial utiliza os seguintes itens do kit:

- Raspberry Pi 3
- Cartão MicroSD (com NOOBS)
- Um cabo USB Mini
- Um cabo Ethernet

### <a name="required-desktop-software"></a>Software de ambiente de trabalho necessárias

Terá de cliente SSH no seu computador desktop que lhe permite aceder remotamente a linha de comandos no Raspberry Pi.

- Windows não incluem um cliente SSH. Recomendamos que utilize [PuTTY](http://www.putty.org/).
- A maioria das distribuições de Linux e Mac OS incluem o utilitário da linha de comandos de SSH. Para obter mais informações, consulte [SSH através de Linux ou Mac OS](https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md).

### <a name="required-raspberry-pi-software"></a>Software necessário de Raspberry Pi

Este artigo pressupõe que instalou a versão mais recente do [Raspbian SO no seu Raspberry Pi](https://www.raspberrypi.org/learning/software-guide/quickstart/).

Os passos seguintes mostram como preparar o seu Raspberry Pi para a criação de uma aplicação de C que liga ao solution accelerator:

1. Ligar ao seu Raspberry Pi com **ssh**. Para obter mais informações, consulte [SSH (Secure Shell)](https://www.raspberrypi.org/documentation/remote-access/ssh/README.md) sobre o [site Raspberry Pi](https://www.raspberrypi.org/).

1. Utilize o seguinte comando para atualizar o seu Raspberry Pi:

    ```sh
    sudo apt-get update
    ```

1. Utilize o seguinte comando para adicionar as ferramentas de desenvolvimento necessárias e bibliotecas para o seu Raspberry Pi:

    ```sh
    sudo apt-get install g++ make cmake gcc git libssl1.0-dev build-essential curl libcurl4-openssl-dev uuid-dev
    ```

1. Utilize os seguintes comandos para transferir, criar e instalar as bibliotecas de cliente do IoT Hub no seu Raspberry Pi:

    ```sh
    cd ~
    git clone --recursive https://github.com/azure/azure-iot-sdk-c.git
    mkdir cmake
    cd cmake
    cmake ..
    make
    sudo make install
    ```

## <a name="create-a-project"></a>Criar um projeto

Conclua os passos seguintes a utilizar o **ssh** ligação para o seu Raspberry Pi:

1. Crie uma pasta chamada `remote_monitoring` na sua pasta raiz no Raspberry Pi. Navegue para esta pasta na sua shell:

    ```sh
    cd ~
    mkdir remote_monitoring
    cd remote_monitoring
    ```

1. Criar os ficheiros de quatro **Main**, **remote_monitoring.c**, **remote_monitoring.h**, e **CMakeLists.txt** no `remote_monitoring` pasta.

1. No editor de texto, abra a **remote_monitoring.c** ficheiro. No Raspberry Pi, pode usar o **nano** ou **vi** editor de texto. Adicione as seguintes instruções `#include`:

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

[!INCLUDE [iot-suite-connecting-code](../../includes/iot-suite-connecting-code.md)]

Guardar a **remote_monitoring.c** de ficheiro e saia do editor.

## <a name="add-code-to-run-the-app"></a>Adicionar código para executar a aplicação

No editor de texto, abra a **remote_monitoring.h** ficheiro. Adicione o seguinte código:

```c
void remote_monitoring_run(void);
```

Guardar a **remote_monitoring.h** de ficheiro e saia do editor.

No editor de texto, abra a **Main** ficheiro. Adicione o seguinte código:

```c
#include "remote_monitoring.h"

int main(void)
{
  remote_monitoring_run();

  return 0;
}
```

Guardar a **Main** de ficheiro e saia do editor.

## <a name="build-and-run-the-application"></a>Compilar e executar a aplicação

Os passos seguintes descrevem como utilizar *CMake* para criar seu aplicativo de cliente.

1. No editor de texto, abra a **CMakeLists.txt** de ficheiros a `remote_monitoring` pasta.

1. Adicione as seguintes instruções para definir como criar a sua aplicação de cliente:

    ```cmake
    macro(compileAsC99)
      if (CMAKE_VERSION VERSION_LESS "3.1")
        if (CMAKE_C_COMPILER_ID STREQUAL "GNU")
          set (CMAKE_C_FLAGS "--std=c99 ${CMAKE_C_FLAGS}")
          set (CMAKE_CXX_FLAGS "--std=c++11 ${CMAKE_CXX_FLAGS}")
        endif()
      else()
        set (CMAKE_C_STANDARD 99)
        set (CMAKE_CXX_STANDARD 11)
      endif()
    endmacro(compileAsC99)

    cmake_minimum_required(VERSION 2.8.11)
    compileAsC99()

    set(AZUREIOT_INC_FOLDER "${CMAKE_SOURCE_DIR}" "/usr/local/include/azureiot")

    include_directories(${AZUREIOT_INC_FOLDER})

    set(sample_application_c_files
        ./remote_monitoring.c
        ./main.c
    )

    set(sample_application_h_files
        ./remote_monitoring.h
    )

    add_executable(sample_app ${sample_application_c_files} ${sample_application_h_files})

    target_link_libraries(sample_app
      serializer
      iothub_client_mqtt_transport
      umqtt
      iothub_client
      aziotsharedutil
      parson
      pthread
      curl
      ssl
      crypto
      m
    )
    ```

1. Guardar a **CMakeLists.txt** de ficheiro e saia do editor.

1. Na `remote_monitoring` pasta, crie uma pasta para armazenar o *certifique* ficheiros que gera de CMake. Em seguida, execute o **cmake** e **tornar** comandos da seguinte forma:

    ```sh
    mkdir cmake
    cd cmake
    cmake ../
    make
    ```

1. Execute a aplicação de cliente e enviar telemetria ao IoT Hub:

    ```sh
    ./sample_app
    ```

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]
