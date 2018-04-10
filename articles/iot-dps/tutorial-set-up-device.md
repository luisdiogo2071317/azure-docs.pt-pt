---
title: Configurar o dispositivo para o Serviço Aprovisionamento de Dispositivos no Hub IoT
description: Configurar o dispositivo para ser aprovisionado através do Serviço Aprovisionamento de Dispositivos no Hub IoT durante o processo de fabrico de dispositivos
services: iot-dps
keywords: ''
author: dsk-2015
ms.author: dkshir
ms.date: 04/02/2018
ms.topic: tutorial
ms.service: iot-dps
documentationcenter: ''
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: c885e4d5d747d913eaf0b7137b240950e920e7ff
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/03/2018
---
# <a name="set-up-a-device-to-provision-using-the-azure-iot-hub-device-provisioning-service"></a>Configurar um dispositivo para ser aprovisionado com o Serviço Aprovisionamento de Dispositivos no Hub IoT do Azure

No tutorial anterior, aprendeu a configurar o Serviço Aprovisionamento de Dispositivos no Hub IoT do Azure para aprovisionar automaticamente os seus dispositivos no seu hub IoT. Este tutorial mostra-lhe como configurar o seu dispositivo durante o processo de fabrico, permitindo que o mesmo seja aprovisionado automaticamente no hub IoT. O dispositivo é aprovisionado com base no [Mecanismo de atestação](concepts-device.md#attestation-mechanism), após o primeiro arranque e ligação ao serviço de aprovisionamento. Este tutorial aborda os processos para:

> [!div class="checklist"]
> * Compilar o SDK de Cliente do Serviço de Aprovisionamento de Dispositivos para plataformas específicas
> * Extrair os artefactos de segurança
> * Configurar o software de registo de dispositivos

## <a name="prerequisites"></a>Pré-requisitos

Antes de continuar, crie a sua instância do Serviço de Aprovisionamento de Dispositivos e um hub do IoT com as instruções no tutorial [1 - Configurar recursos na cloud](./tutorial-set-up-cloud.md) anterior.

Este tutorial utiliza o [repositório Azure IoT SDKs and libraries for C](https://github.com/Azure/azure-iot-sdk-c) (SDKs e bibliotecas do Azure IoT para C), que contém o SDK de Cliente do Serviço Aprovisionamento de Dispositivos para C. O SDK fornece atualmente suporte para TPM e X.509 para dispositivos em execução em implementações Windows ou Ubuntu. Este tutorial tem por base a utilização de um cliente de desenvolvimento do Windows, que também pressupõe conhecimentos avançados em Visual Studio 2017. 

Se não estiver familiarizado com o processo de aprovisionamento automático, reveja [Auto-provisioning concepts](concepts-auto-provisioning.md) (Conceitos de aprovisionamento automático) antes de continuar. 

## <a name="build-a-platform-specific-version-of-the-sdk"></a>Compilar uma versão do SDK específica para uma plataforma

O SDK de Cliente do Serviço Aprovisionamento de Dispositivos ajuda-o a implementar o software de registo de dispositivos. Contudo, antes de poder utilizá-lo, tem de compilar uma versão do SDK que seja específica da plataforma de cliente de desenvolvimento e do mecanismo de atestação. Neste tutorial, vai criar um SDK que utiliza o Visual Studio 2017 numa plataforma de desenvolvimento Windows para um tipo de atestação suportado:

1. Instale as ferramentas necessárias e clone o repositório do GitHub que contém o SDK de Cliente do Serviço de Aprovisionamento para C:

   a. Certifique-se de que tem o Visual Studio 2015 ou o [Visual Studio 2017](https://www.visualstudio.com/vs/) instalado no seu computador. Tem de ter a carga de trabalho do ["Ambiente de trabalho de desenvolvimento com C++"](https://www.visualstudio.com/vs/support/selecting-workloads-visual-studio-2017/) ativada para a sua instalação do Visual Studio.

   b. Transfira e instale o [sistema de compilação CMake](https://cmake.org/download/). É importante que o Visual Studio com a carga de trabalho "Ambiente de trabalho de desenvolvimento com C++" esteja instalado no seu computador **antes** da instalação de CMake.

   c. Verifique se `git` está instalado no computador e que é adicionado às variáveis de ambiente às quais a janela de comandos pode aceder. Veja [Software Freedom Conservancy's Git client tools](https://git-scm.com/download/) (Ferramentas de Cliente de Git da Software Freedom Conservancy) para obter as mais recentes ferramentas de `git`, incluindo o **Git Bash**, uma shell de Bash de linha de comandos para interagir com o seu repositório do Git local. 

   d. Abra o Git Bash e clone o repositório "Azure IoT SDKs and libraries for C". O comando de clonagem pode demorar vários minutos a concluir, uma vez que também transfere várias submódulos dependentes:
    
   ```cmd/sh
   git clone https://github.com/Azure/azure-iot-sdk-c.git --recursive
   ```

   e. Crie um subdiretório `cmake` novo dentro do subdiretório do repositório recém-criado:

   ```cmd/sh
   mkdir azure-iot-sdk-c/cmake
   ``` 

2. Na linha de comandos do Git Bash, mude para o subdiretório `cmake` do repositório azure-iot-sdk-c.

   ```cmd/sh
   cd azure-iot-sdk-c/cmake
   ```

3. Compile o SDK para a plataforma de desenvolvimento e para um dos mecanismos de atestação suportados através de um dos métodos seguintes (repare também nos dois carateres de ponto final à direita). Após a conclusão, o CMake compila o subdiretório `/cmake` com conteúdo específico do seu dispositivo:
    - Em dispositivos que utilizam um TPM/HSM físico ou um certificado X.509 simulado para a atestação:
        ```cmd/sh
        cmake -Duse_prov_client:BOOL=ON ..
        ```

    - Em dispositivos que utilizam o simulador de TPM para a atestação:
        ```cmd/sh
        cmake -Duse_prov_client:BOOL=ON -Duse_tpm_simulator:BOOL=ON ..
        ```

Está agora pronto para utilizar o SDK para compilar o código de registo do dispositivo. 
 
<a id="extractsecurity"></a> 

## <a name="extract-the-security-artifacts"></a>Extrair os artefactos de segurança 

O passo seguinte é extrair os artefactos de segurança do mecanismo de atestação que o seu dispositivo utiliza. 

### <a name="physical-device"></a>Dispositivo físico 

Se tiver compilado o SDK para utilizar a atestação de um TPM/HSM físico:

- Num dispositivo TPM, tem de determinar a **Chave de Endossamento** associada ao mesmo junto do fabricante do chip do TPM. Pode derivar um **ID de Registo** exclusivo para o seu dispositivo TPM ao encriptar a chave de endossamento.  

- Em dispositivos X.509, tem de obter os certificados emitidos para o seu dispositivo ou dispositivos - certificados de entidade final para inscrição individual de dispositivos ou certificados de raiz para inscrições de dispositivos em grupo. 

### <a name="simulated-device"></a>Dispositivo simulado

Se tiver compilado o SDK para utilizar a atestação de um TPM simulado ou de um certificado X.509:

- Num dispositivo  TPM simulado:
   1. Numa linha de comandos separada, navegue para o subdiretório `azure-iot-sdk-c` e execute o simulador de TPM. O simulador escuta através de um socket nas portas 2321 e 2322. Não feche esta janela de comando; terá de manter este simulador em execução até ao fim do início rápido seguinte. 

      No subdiretório `azure-iot-sdk-c`, execute o seguinte comando para iniciar o simulador:

      ```cmd/sh
      .\provisioning_client\deps\utpm\tools\tpm_simulator\Simulator.exe
      ```

   2. Com o Visual Studio, abra a solução gerada na pasta *cmake* denominada `azure_iot_sdks.sln` e compile-a com o comando “Build solution” no menu “Build” (“Compilar”).

   3. No painel *Explorador de Soluções* do Visual Studio, navegue para a pasta **Aprovisionar\_Ferramentas**. Clique com o botão direito do rato no projeto **tpm_device_provision** e selecione **Configurar como Projeto de Arranque**. 

   4. Execute a solução com um dos comandos "Start" no menu "Debug" (“Depuração”). A janela de saída apresenta o **_ID de Registo_** e a **_Chave de Endossamento_** do simulador de TPM, que são necessários para a inscrição do dispositivo. Copie estes valores para utilização posterior. Pode fechar esta janela (com o ID de Registo e a Chave de Endossamento), mas deixe a janela do simulador de TPM que iniciou no passo 1 em execução.

- Num dispositivo X.509 simulado:
  1. Com o Visual Studio, abra a solução gerada na pasta *cmake* denominada `azure_iot_sdks.sln` e compile-a com o comando “Build solution” no menu “Build” (“Compilar”).

  2. No painel *Explorador de Soluções* do Visual Studio, navegue para a pasta **Aprovisionar\_Ferramentas**. Clique com o botão direito do rato no projeto **dice\_device\_enrollment** e selecione **Set as Startup Project** (Definir como Projeto de Arranque”). 
  
  3. Execute a solução com um dos comandos "Start" no menu "Debug" (“Depuração”). Na janela de saída, introduza **i** para inscrição individual, quando lhe for pedido. A janela de saída apresenta um certificado X.509 gerado localmente para o seu dispositivo simulado. Copie a saída para a área de transferência, começando em *-----BEGIN CERTIFICATE-----* e terminando no primeiro *-----END CERTIFICATE-----*, garantindo que também inclui estas duas linhas. Tenha em atenção que apenas necessita o primeiro certificado na janela de saída.
 
  4. Crie um ficheiro com o nome **_X509testcert.pem_**, abra-o num editor de texto à sua escolha e copie os conteúdos da área de transferência para o mesmo. Guarde o ficheiro, pois vai utilizá-lo mais tarde para a inscrição de dispositivos. Quando o software de registo for executado, utiliza o mesmo certificado durante o aprovisionamento automático.    

Estes artefactos de segurança são necessários durante a inscrição do seu dispositivo no Serviço Aprovisionamento de Dispositivos. O Serviço Aprovisionamento aguarda que o dispositivo seja arrancado e se ligue ao mesmo num momento posterior. Da primeira vez que o dispositivo for arrancado, a lógica do SDK de Cliente interage com o seu chip (ou simulador) para extrair os artefactos de segurança do dispositivo e verifica o registo no Serviço Aprovisionamento de Dispositivos. 

## <a name="create-the-device-registration-software"></a>Configurar o software de registo de dispositivos

O último passo consiste em escrever uma aplicação de registo que utiliza o SDK de Cliente do Serviço Aprovisionamento de Dispositivos para registar o dispositivo no serviço Hub IoT. 

> [!NOTE]
> Neste passo, vamos pressupor que está a ser utilizado um dispositivo simulado, o que é conseguido ao executar uma aplicação de registo de exemplo de SDK a partir da sua estação de trabalho. No entanto, estes mesmos conceitos aplicam-se se estiver a compilar uma aplicação de registo para implementação num dispositivo físico. 

1. No portal do Azure, selecione o painel **Descrição Geral** do seu Serviço Aprovisionamento de Dispositivos e copie o valor **_Âmbito do ID_**. O *ID do Âmbito* é gerado pelo serviço e garante que é exclusivo. É imutável e utilizado para identificar exclusivamente o IDs de registo.

    ![Extrair a informação de ponto final DPS a partir do painel do portal](./media/tutorial-set-up-device/extract-dps-endpoints.png) 

2. No *Explorador de Soluções* do Visual Studio da sua máquina, navegue para a pasta **Aprovisionar\_Exemplos**. Selecione o projeto de exemplo com o nome **prov\_dev\_client\_sample** e abra o ficheiro de origem **prov\_dev\_client\_sample.c**.

3. Atribua o valor _Âmbito de ID_ obtido no passo 1 à variável `id_scope` (ao remover os parênteses de início/`[` e de fim/`]`): 

    ```c
    static const char* global_prov_uri = "global.azure-devices-provisioning.net";
    static const char* id_scope = "[ID Scope]";
    ```

    Para referência, a variável `global_prov_uri`, que permite que a API de registo de cliente do Hub IoT `IoTHubClient_LL_CreateFromDeviceAuth` se ligue à instância do Serviço Aprovisionamento de Dispositivos indicado.

4. Na função **main()**, no mesmo ficheiro, comente/anule o comentário da variável `hsm_type` que corresponde ao mecanismo de atestação que o software de registo de dispositivos está a utilizar (TPM ou X.509): 

    ```c
    hsm_type = SECURE_DEVICE_TYPE_TPM;
    //hsm_type = SECURE_DEVICE_TYPE_X509;
    ```

5. Guarde as alterações e volte a compilar o exemplo **prov\_dev\_client\_sample** ao selecionar “Build solution” no menu “Build”. 

6. Clique com o botão direito do rato no projeto **prov\_dev\_client\_sample**, na pasta **Provision\_Samples** e selecione **Set as Startup Project** (Definir como Projeto de Arranque). NÃO execute ainda a aplicação de exemplo.

> [!IMPORTANT]
> Não execute/inicie ainda o dispositivo! Antes de iniciar o dispositivo, tem de concluir o processo ao inscrevê-lo no Serviço Aprovisionamento de Dispositivos. A secção “Próximos passos” abaixo orienta-o para o artigo seguinte.

### <a name="sdk-apis-used-during-registration-for-reference-only"></a>APIs de SDK utilizadas durante o registo (apenas para referência)

Para referência, o SDK disponibiliza as seguintes APIs para a sua aplicação utilizar durante o registo. Estas APIs ajudam o dispositivo a ligar-se e a registar-se no Serviço Aprovisionamento de Dispositivos quando é arrancado. Por sua vez, o dispositivo recebe as informações necessárias para estabelecer ligação à sua instância do Hub IoT:

```C
// Creates a Provisioning Client for communications with the Device Provisioning Client Service.  
PROV_DEVICE_LL_HANDLE Prov_Device_LL_Create(const char* uri, const char* scope_id, PROV_DEVICE_TRANSPORT_PROVIDER_FUNCTION protocol)

// Disposes of resources allocated by the provisioning Client.
void Prov_Device_LL_Destroy(PROV_DEVICE_LL_HANDLE handle)

// Asynchronous call initiates the registration of a device.
PROV_DEVICE_RESULT Prov_Device_LL_Register_Device(PROV_DEVICE_LL_HANDLE handle, PROV_DEVICE_CLIENT_REGISTER_DEVICE_CALLBACK register_callback, void* user_context, PROV_DEVICE_CLIENT_REGISTER_STATUS_CALLBACK reg_status_cb, void* status_user_ctext)

// Api to be called by user when work (registering device) can be done
void Prov_Device_LL_DoWork(PROV_DEVICE_LL_HANDLE handle)

// API sets a runtime option identified by parameter optionName to a value pointed to by value
PROV_DEVICE_RESULT Prov_Device_LL_SetOption(PROV_DEVICE_LL_HANDLE handle, const char* optionName, const void* value)
```

Poderá também concluir que tem de refinar a aplicação de registo do cliente de Serviço de Aprovisionamento de Dispositivos, ao utilizar um dispositivo simulado primeiro e uma configuração de serviço de teste. Assim que a aplicação estiver a funcionar no ambiente de teste, pode criá-la para o seu dispositivo específico e copiar o ficheiro executável para a imagem do dispositivo. 

## <a name="clean-up-resources"></a>Limpar recursos

Neste momento, os serviços Aprovisionamento de Dispositivos e Hub IoT poderão estar em execução no portal. Se quiser abandonar a configuração de aprovisionamento de dispositivos e/ou retardar a conclusão desta série de tutoriais, recomendamos encerrá-los para evitar incorrer em custos desnecessários.

1. No menu do lado esquerdo do portal do Azure, clique em **Todos os recursos** e selecione o seu Serviço Aprovisionamento de Dispositivos. Na parte superior do painel **Todos os recursos**, clique em **Eliminar**.  
2. No menu do lado esquerdo do portal do Azure, clique em **Todos os recursos** e selecione o seu hub IoT. Na parte superior do painel **Todos os recursos**, clique em **Eliminar**.  

## <a name="next-steps"></a>Passos seguintes
Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Compilar o SDK de Cliente do Serviço de Aprovisionamento de Dispositivos para plataformas específicas
> * Extrair os artefactos de segurança
> * Configurar o software de registo de dispositivos

Avance para o próximo tutorial para saber como aprovisionar o dispositivo no seu hub IoT ao inscrevê-lo no Serviço Aprovisionamento de Dispositivos no Hub IoT do Azure para aprovisionamento automático.

> [!div class="nextstepaction"]
> [Provision the device to your IoT hub](tutorial-provision-device-to-hub.md) (Aprovisionar o dispositivo no seu hub IoT)

