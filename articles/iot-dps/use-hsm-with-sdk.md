---
title: Procedimento do Azure - Como utilizar um Módulo de Segurança de Hardware diferente com o SDK do Cliente do Serviço Aprovisionamento de Dispositivos no Azure
description: Procedimento do Azure - Como utilizar um Módulo de Segurança de Hardware diferente com o SDK do Cliente do Serviço Aprovisionamento de Dispositivos no Azure
services: iot-dps
keywords: ''
author: yzhong94
ms.author: yizhon
ms.date: 03/28/2018
ms.topic: hero-article
ms.service: iot-dps
documentationcenter: ''
manager: ''
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 0d392f4a8d935cb37b6f4cfcd69826de58b33880
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/30/2018
---
# <a name="how-to-use-different-hardware-security-modules-with-device-provisioning-service-client-sdk-for-c"></a>Como utilizar diferentes Módulos de Segurança de Hardware com o SDK do Cliente do Serviço Aprovisionamento de Dispositivos para C

Este artigo mostra-lhe como utilizar diferentes [Módulos de Segurança de Hardware (HSM)](https://azure.microsoft.com/blog/azure-iot-supports-new-security-hardware-to-strengthen-iot-security/) com o SDK do Cliente do Serviço Aprovisionamento de Dispositivos para C. Pode utilizar um dispositivo físico ou um simulador. O serviço de aprovisionamento suporta a autenticação para dois tipos de mecanismos de atestação: X**.**509 e Trusted Platform Module (TPM).

## <a name="prerequisites"></a>Pré-requisitos

Prepare o seu ambiente de desenvolvimento de acordo com a secção denominada “Preparar o ambiente de desenvolvimento” do guia [Criar e aprovisionar um dispositivo simulado](./quick-create-simulated-device.md).

### <a name="choose-a-hardware-security-module"></a>Escolher um Módulo de Hardware de Segurança

Como fabricante de dispositivos, em primeiro lugar, tem de escolher Módulos de Segurança de Hardware (ou HSMs) que se baseiem num dos tipos suportados. Atualmente, o [SDK do cliente do Serviço Aprovisionamento de Dispositivo para C](https://github.com/Azure/azure-iot-sdk-c/tree/master/provisioning_client) tem suporte para os HSMs seguintes: 

- [Trusted Platform Module (TPM)](https://en.wikipedia.org/wiki/Trusted_Platform_Module): o TPM é uma norma estabelecida para a maioria das plataformas de dispositivos compatíveis com Windows, bem como para alguns dispositivos compatíveis com Linux/Ubuntu. Como fabricante de dispositivos, pode escolher este HSM se tiver algum destes SOs nos seus dispositivos e quiser utilizar uma norma estabelecida para os HSM. Com os chips de TPM, só pode inscrever cada dispositivo individualmente no Serviço Aprovisionamento de Dispositivos. Para fins de desenvolvimento, pode utilizar o simulador TPM no seu computador de desenvolvimento Windows ou Linux.

- [X.509](https://cryptography.io/en/latest/x509/): os HSMs baseados em X.509 são chips relativamente mais recentes. A Microsoft também está atualmente a trabalhar em chips RIoT ou DICE, os quais implementam os certificados X.509. Com os chips X509, pode inscrever dispositivos em massa no portal. Também suporta determinados SOs não Windows, como o embedOS. Para fins de desenvolvimento, o SDK do Cliente do Serviço Aprovisionamento de Dispositivos suportam simuladores de dispositivos X.509. 

Para obter mais informações, veja [IoT Hub Device Provisioning Service security concepts](concepts-security.md) (Conceitos de segurança do Serviço Aprovisionamento de Dispositivos no Hub IoT). 

## <a name="enable-authentication-for-supported-hsms"></a>Ativar a autenticação para HSMs suportados

Antes de o dispositivo físico ou o simulador poderem ser inscritos no portal do Azure, o modo de autenticação (X**.**509 ou TPM) tem de estar ativado naqueles. Primeiro, navegue para a pasta de raiz de azure-iot-sdk-c. Depois, execute o comando especificado, consoante o modo de autenticação que escolher:

### <a name="use-x509-with-simulator"></a>Utilizar X**.**509 com o simulador

O serviço de aprovisionamento inclui um emulador de Device Identity Composition Engine (DICE) que gera um certificado X**.**509 para autenticar o dispositivo. Para ativar a autenticação X**.**509, execute o comando seguinte: 

```
cmake -Ddps_auth_type=x509 ..
```

Estão disponíveis informações sobre o hardware com DICE [aqui](https://azure.microsoft.com/blog/azure-iot-supports-new-security-hardware-to-strengthen-iot-security/).

### <a name="use-x509-with-hardware"></a>Utilizar X**.** 509 com hardware

O serviço de aprovisionamento pode ser utilizado com X**.**509 noutro hardware. Para estabelecer a ligação, é necessário uma interface entre o hardware e o SDK. Entre em contacto com o fabricante do seu HSM para obter informações sobre a interface.

### <a name="use-tpm"></a>Utilizar TPM

O serviço de aprovisionamento pode ligar-se a chips de TPM de hardware Windows e Linux com um Token SAS. Para ativar a autenticação TPM, execute o comando seguinte:

```
cmake -Ddps_auth_type=tpm ..
```

### <a name="use-tpm-with-simulator"></a>Utilizar TPM com o simulador

Se não tiver um dispositivo com chips de TPM, pode utilizar um simulador para fins de desenvolvimento no SO Windows. Para ativar a autenticação TPM e executar o simulador TPM, execute o comando seguinte:

```
cmake -Ddps_auth_type=tpm_simulator ..
```

## <a name="build-the-sdk"></a>Criar o SDK
Crie o SDK antes de criar a inscrição dos dispositivos.

### <a name="linux"></a>Linux
- Para criar o SDK no Linux:
    ```
    cd azure-iot-sdk-c
    mkdir cmake
    cd cmake
    cmake ..
    cmake --build .  # append '-- -j <n>' to run <n> jobs in parallel
    ```
- Para criar os binários de Depuração, adicione a opção CMake correspondente ao comando de geração de projeto, por exemplo:
    ```
    cmake -DCMAKE_BUILD_TYPE=Debug ..
    ```

- Existem muitas [opções de configuração CMake](https://cmake.org/cmake/help/v3.6/manual/cmake.1.html) disponíveis para criar o SDK. Por exemplo, pode desativar uma das pilhas de protocolo disponíveis através da adição de um argumento para o comando de geração de projeto CMake:
    ```
    cmake -Duse_amqp=OFF ..
    ```
- Também pode criar e executar o teste da unidade:
    ```
    cmake -Drun_unittests=ON ..
    cmake --build .
    ctest -C "debug" -V
    ```

### <a name="windows"></a>Windows
- Para criar o SDK no Windows, siga os passos seguintes para gerar ficheiros de projeto:
    - Abra uma “Linha de Comandos de Programador” para o VS2015
    - Execute os seguintes comandos de CMake a partir da raiz do repositório:
      ```
      cd azure-iot-sdk-c
      mkdir cmake
      cd cmake
      cmake -G "Visual Studio 14 2015" ..
      ```
    Este comando cria bibliotecas x86. Para criar para x64, modifique o argumento do gerador de cmake: 
    ```
    cmake .. -G "Visual Studio 14 2015 Win64"
    ```

- Se a geração do projeto for concluída com êxito, deverá ver um ficheiro de solução do Visual Studio (.sln) na pasta `cmake`. Para criar o SDK:
    - Abra **cmake\azure_iot_sdks.sln** no Visual Studio e crie-o **OU**
    - Execute o comando seguinte na linha de comandos que utilizou para gerar os ficheiros de projeto:
      ```
      cmake --build . -- /m /p:Configuration=Release
      ```
- Para criar os binários de Depuração, utilize o argumento MSBuild correspondente: 
    ```
    cmake --build . -- /m /p:Configuration=Debug`
    ```
- Existem muitas opções de configuração CMake disponíveis para criar o SDK. Por exemplo, pode desativar uma das pilhas de protocolo disponíveis através da adição de um argumento para o comando de geração de projeto CMake:
    ```
    cmake -G "Visual Studio 14 2015" -Duse_amqp=OFF ..
    ```
- Além disso, pode criar e executar testes de unidade:
    ```
    cmake -G "Visual Studio 14 2015" -Drun_unittests=ON ..
    cmake --build . -- /m /p:Configuration=Debug
    ctest -C "debug" -V
    ```
  
### <a name="libraries-to-include"></a>Bibliotecas a incluir
- Estas bibliotecas devem ser incluídas no seu SDK:
    - O serviço de aprovisionamento: dps_http_transport, dps_client, dps_security_client
    - Segurança do Hub IoT: iothub_security_client

## <a name="create-a-device-enrollment-entry-in-device-provisioning-services"></a>Criar uma entrada de inscrição de dispositivos no Serviço Aprovisionamento de Dispositivos

### <a name="tpm"></a>TPM
Se estiver a utilizar o TPM, siga as instruções em [“Criar e aprovisionar um dispositivo simulado com o Serviço Aprovisionamento de Dispositivos no Hub IoT”](./quick-create-simulated-device.md) para criar uma entrada de inscrição de dispositivos no Serviço Aprovisionamento de Dispositivos e simular o primeiro arranque.

### <a name="x509"></a>X**.**509
1. Para inscrever dispositivos no serviço de aprovisionamento, tem de anotar a Chave de Endossamento e o ID de Registo de cada dispositivo, que são apresentados na Ferramenta de Aprovisionamento fornecida pelo SDK do Cliente. Execute o comando seguinte para imprimir o certificado de AC de raiz (para grupos de inscrição) e o certificado do signatário (para inscrições individuais):
      ```
      ./azure-iot-sdk-c/dps_client/tools/x509_device_provision/x509_device_provision.exe
      ```
2. Inicie sessão no portal do Azure, clique no botão **Todos os recursos**, no menu do lado esquerdo, e abra o serviço DPS.
   - Inscrição Individual X**.**509: no painel de resumo do Serviço Aprovisionamento de Dispositivos, selecione **Gerir inscrições**. Selecione o separador **Inscrições Individuais** e clique no botão **Adicionar**, na parte superior. Selecione **X**.**509** como o *Mecanismo* de atestado de identidades e carregue o certificado do signatário conforme indicado pelo painel. Quando tiver terminado, clique no botão **Guardar**. 
   - Inscrição em Grupo X**.**509: no painel de resumo do Serviço Aprovisionamento de Dispositivos, selecione **Gerir inscrições**. Selecione o separador **Inscrições em Grupo** e clique no botão **Adicionar**, na parte superior. Selecione **X**.**509** como o *Mecanismo* de atestado de identificações, introduza um nome para o grupo e um nome para a certificação e carregue o certificado de CA de raiz, conforme indicado pelo painel. Quando tiver terminado, clique no botão **Guardar**. 

## <a name="enable-authentication-for-custom-tpm-and-x509-devices-optional"></a>Ativar a autenticação em dispositivos TPM e X.509 personalizados (opcional)

> [!NOTE]
> Esta secção só é aplicável a dispositivos que requerem suporte para uma plataforma ou HSM personalizados, que o SDK do Cliente do Serviço Aprovisionamento de Dispositivos para C não suporta atualmente.

Primeiro, tem de desenvolver o repositório e a biblioteca do HSM personalizado:

1. Desenvolva uma biblioteca para aceder ao seu HSM. Este projeto tem de produzir uma biblioteca estática para ser consumida pelo SDK de Aprovisionamento de Dispositivos.

2. Implemente as funções definidas no seguinte ficheiro de cabeçalho, na biblioteca: 

    - Para um TPM personalizado: implemente as funções do HSM personalizado definidas em [API do TPM de HSM](https://github.com/Azure/azure-iot-sdk-c/blob/master/provisioning_client/devdoc/using_custom_hsm.md#hsm-tpm-api).  
    - Para um X.509 personalizado: implemente as funções do HSM personalizado definidas em [API do X509 de HSM](https://github.com/Azure/azure-iot-sdk-c/blob/master/provisioning_client/devdoc/using_custom_hsm.md#hsm-x509-api). 

Quando a biblioteca se compilar sozinha com êxito, terá de integrá-la no SDK do Cliente do Serviço Aprovisionamento de Dispositivos, ao ligar à sua primeira biblioteca. :

1. Indique o repositório do GitHub do HSM personalizado, o caminho da biblioteca e o respetivo nome no comando `cmake` seguinte:
    ```cmd/sh
    cmake -Duse_prov_client:BOOL=ON -Dhsm_custom_lib=<path_and_name_of_library> <PATH_TO_AZURE_IOT_SDK>
    ```
   
2. Abra o ficheiro de solução do Visual Studio compilado para CMake (`\azure-iot-sdk-c\cmake\azure_iot_sdks.sln`) e compile-o. 

    - O processo de compilação compila a biblioteca do SDK.
    - O SDK tenta ligar-se ao HSM personalizado definido no comando `cmake`.

3. Execute a aplicação de exemplo "prov_dev_client_ll_sample" em "Provision_Samples" (em `\azure-iot-sdk-c\cmake\provisioning_client\samples\prov_dev_client_ll_sample`) para confirmar que o HSM está implementado corretamente.

## <a name="connecting-to-iot-hub-after-provisioning"></a>Ligar ao Hub IoT após o aprovisionamento

Após o dispositivo ter sido aprovisionado pelo serviço de aprovisionamento, esta API utiliza o modo de autenticação HSM para se ligar ao Hub IoT: 
  ```
  IOTHUB_CLIENT_LL_HANDLE handle = IoTHubClient_LL_CreateFromDeviceAuth(iothub_uri, device_id, iothub_transport);
  ```

