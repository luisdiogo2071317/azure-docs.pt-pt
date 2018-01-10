---
title: Aprovisionar um dispositivo X.509 simulado no Hub IoT do Azure com Java | Microsoft Docs
description: "Manual de Início Rápido do Azure - Criar e aprovisionar um dispositivo X.509 simulado com o SDK de dispositivo Java com o Serviço de Aprovisionamento de Dispositivos no Hub IoT"
services: iot-dps
keywords: 
author: msebolt
ms.author: v-masebo
ms.date: 12/21/2017
ms.topic: quickstart
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: java
ms.custom: mvc
ms.openlocfilehash: 508df6e7c677d87c6b893519c59b02880e16d7ec
ms.sourcegitcommit: a648f9d7a502bfbab4cd89c9e25aa03d1a0c412b
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/22/2017
---
# <a name="create-and-provision-a-simulated-x509-device-using-java-device-sdk-for-iot-hub-device-provisioning-service"></a>Criar e aprovisionar um dispositivo X.509 simulado com o SDK de dispositivo Java com o Serviço de Aprovisionamento de Dispositivos no Hub IoT
> [!div class="op_single_selector"]
> * [C](quick-create-simulated-device-x509.md)
> * [Java](quick-create-simulated-device-x509-java.md)
> * [C#](quick-create-simulated-device-x509-csharp.md)
> * [Python](quick-create-simulated-device-x509-python.md)

Estes passos mostram como simular um dispositivo X.509 no seu computador de desenvolvimento que executa o SO Windows e utilizar um código de exemplo para ligar esse dispositivo ao Serviço Aprovisionamento de Dispositivos e ao seu hub IoT. 

Antes de avançar, certifique-se de que executa os passos descritos em [Configurar o Serviço de Aprovisionamento de Dispositivos no Hub IoT com o portal do Azure](./quick-setup-auto-provision.md).


## <a name="prepare-the-environment"></a>Preparar o ambiente 

1. Certifique-se de que tem o [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) instalado no seu computador.

1. Transfira e instale o [Maven](https://maven.apache.org/install.html).

1. Verifique se `git` está instalado no computador e que é adicionado às variáveis de ambiente às quais a janela de comandos pode aceder. Veja as [ferramentas de cliente Git da Software Freedom Conservancy](https://git-scm.com/download/) relativamente à mais recente versão das ferramentas de `git` a instalar, que incluem o **Git Bash**, a aplicação de linha de comandos que pode utilizar para interagir com o seu repositório Git local. 

1. Abra uma linha de comandos. Clone o repositório do GitHub para exemplo de código de simulação de dispositivo.
    
    ```cmd/sh
    git clone https://github.com/Azure/azure-iot-sdk-java.git --recursive
    ```

1. Navegue até ao projeto de gerador de certificados e compile o projeto. 

    ```cmd/sh
    cd azure-iot-sdk-java/provisioning/provisioning-tools/provisioning-x509-cert-generator
    mvn clean install
    ```

1. Navegue até à pasta de destino e execute o ficheiro jar criado.

    ```cmd/sh
    cd target
    java -jar ./provisioning-x509-cert-generator-{version}-with-deps.jar
    ```

1. Crie as informações de inscrição de qualquer uma das seguintes formas, de acordo com a sua configuração:

    - **Inscrição individual**:

        1. Introduza **N** para _Do you want to input common name_. Copie para a área de transferência a saída de `Client Cert` a partir de *-----BEGIN CERTIFICATE-----* e terminando em *-----END CERTIFICATE-----*.

            ![Gerador de certificados individuais](./media/java-quick-create-simulated-device-x509/individual.png)

        1. Crie um ficheiro com o nome **_X509individual.pem_** no seu computador Windows, abra-o num editor à sua escolha e copie os conteúdos da área de transferência para este ficheiro. Guarde o ficheiro.

        1. Introduza **N** para _Do you want to input Verification Code_ e mantenha a saída do programa aberta para referência mais à frente no Manual de Início Rápido. Tome nota dos valores de _Client Cert_ e _Client Cert Private Key_.
    
    - **Grupos de inscrição**:

        1. Introduza **N** para _Do you want to input common name_. Copie para a área de transferência a saída de `Root Cert` a partir de *-----BEGIN CERTIFICATE-----* e terminando em *-----END CERTIFICATE-----*.

            ![Gerador de certificados de grupo](./media/java-quick-create-simulated-device-x509/group.png)

        1. Crie um ficheiro com o nome **_X509group.pem_** no seu computador Windows, abra-o num editor à sua escolha e copie os conteúdos da área de transferência para este ficheiro. Guarde o ficheiro.

        1. Introduza **Y** para _Do you want to input Verification Code_ e mantenha o programa aberto para utilizar mais à frente no Manual de Início Rápido. Tome nota dos valores de _Certificado de Cliente_, _Chave Privada do Certificado de Cliente_, _Certificado de Signatário_ e _Certificado de Raiz_.


## <a name="create-a-device-enrollment-entry"></a>Criar uma entrada de inscrição de dispositivos

1. Inicie sessão no portal do Azure, clique no botão **Todos os recursos**, no menu do lado esquerdo, e abra o seu serviço de aprovisionamento.

1. Introduza as informações de inscrição de qualquer uma das seguintes formas, de acordo com a sua configuração:

    - **Inscrição individual**: 

        1. No painel de resumo do Serviço Aprovisionamento de Dispositivos, selecione **Gerir inscrições**. Selecione o separador **Inscrições Individuais** e clique no botão **Adicionar**, na parte superior. 

        1. Em **Adicionar entrada da lista de inscrições**, introduza as informações seguintes:
            - Selecione **X.509** como o *Mecanismo* de atestado de identidades.
            - Em *Ficheiro .pem ou .cer de certificado*, selecione o ficheiro de certificado **_X509individual.pem_** criado nos passos anteriores com o widget *Explorador de Ficheiros*.
            - Opcionalmente, pode fornecer as seguintes informações:
                - Selecione um hub IoT ligado ao seu serviço de aprovisionamento.
                - Introduza um ID de dispositivo exclusivo. Certifique-se de que evita dados confidenciais quando der o nome ao seu dispositivo. 
                - Atualize o **estado inicial do dispositivo duplo** com a configuração inicial pretendida para o dispositivo.
            - Quando tiver terminado, clique no botão **Guardar**. 

        ![Introduza as informações de inscrição do dispositivo X.509 no painel do portal](./media/java-quick-create-simulated-device-x509/enter-device-enrollment.png)  

       Se a inscrição for bem-sucedida, o seu dispositivo X.509 aparece como **microsoftriotcore** na coluna *ID de Registo* no separador *Inscrições Individuais*. 

    - **Grupos de inscrição**: 

        1. No painel de resumo do Serviço de Aprovisionamento de Dispositivos, selecione **Certificados** e clique no botão **Adicionar** na parte superior.

        1. Em **Adicionar Certificado**, introduza as informações seguintes:
            - Introduza um nome exclusivo de certificado.
            - Selecione o ficheiro **_X509group.pem_** que criou anteriormente.
            - Quando tiver terminado, clique no botão **Guardar**.

        ![Adicionar certificado](./media/java-quick-create-simulated-device-x509/add-certificate.png)

        1. Selecione o certificado que acabou de criar:
            - Clique em **Gerar Código de Verificação**. Copie o código gerado.
            - Introduza o _código de verificação_ ou clique com o botão direito do rato para colar na janela _provisioning-x509-cert-generator_ em execução.  Prima **Enter**.
            - Copie para a área de transferência a saída de `Verification Cert` a partir de *-----BEGIN CERTIFICATE-----* e terminando em *-----END CERTIFICATE-----*.
            
                ![Gerador de validação](./media/java-quick-create-simulated-device-x509/validation-generator.png)

            - Crie um ficheiro com o nome **_X509validation.pem_** no seu computador Windows, abra-o num editor à sua escolha e copie os conteúdos da área de transferência para este ficheiro. Guarde o ficheiro.
            - Selecione o ficheiro **_X509validation.pem_** no portal do Azure. Clique em **Verificar**.

            ![Validar certificado](./media/java-quick-create-simulated-device-x509/validate-certificate.png)

        1. Selecione **Gerir inscrições**. Selecione o separador **Grupos de Inscrição** e clique no botão **Adicionar** na parte superior.
            - Introduza um nome de grupo exclusivo.
            - Selecione o nome do certificado exclusivo criado anteriormente.
            - Opcionalmente, pode fornecer as seguintes informações:
                - Selecione um hub IoT ligado ao seu serviço de aprovisionamento.
                - Atualize o **estado inicial do dispositivo duplo** com a configuração inicial pretendida para o dispositivo.

        ![Introduzir as informações de inscrição de grupo X.509 no painel do portal](./media/java-quick-create-simulated-device-x509/enter-group-enrollment.png)

        Se a inscrição for bem-sucedida, o grupo de dispositivos X.509 aparece sob a coluna *Nome do Grupo* no separador *Grupos de Inscrição*.


## <a name="simulate-the-device"></a>Simular o dispositivo

1. No painel de resumo do Serviço de Aprovisionamento de Dispositivos, selecione **Descrição geral** e tome nota do _Âmbito do ID_ e do _Ponto Final Global do Serviço de Aprovisionamento_.

    ![Informações de serviço](./media/java-quick-create-simulated-device-x509/extract-dps-endpoints.png)

1. Abra uma linha de comandos. Navegue até à pasta do projeto de exemplo.

    ```cmd/sh
    cd azure-iot-sdk-java/provisioning/provisioning-samples/provisioning-X509-sample
    ```

1. Introduza as informações de inscrição de qualquer uma das seguintes formas, de acordo com a sua configuração:

    - **Inscrição individual**: 

        1. Edite `/src/main/java/samples/com/microsoft/azure/sdk/iot/ProvisioningX509Sample.java` para incluir o _Âmbito do ID_ e o _Ponto Final Global do Serviço de Aprovisionamento_ conforme indicado anteriormente. Inclua também o _Certificado de Cliente_ e a _Chave Privada do Certificado de Cliente_ conforme indicado anteriormente.

            ```java
            private static final String idScope = "[Your ID scope here]";
            private static final String globalEndpoint = "[Your Provisioning Service Global Endpoint here]";
            private static final ProvisioningDeviceClientTransportProtocol PROVISIONING_DEVICE_CLIENT_TRANSPORT_PROTOCOL = ProvisioningDeviceClientTransportProtocol.HTTPS;
            private static final String leafPublicPem = "<Your Public PEM Certificate here>";
            private static final String leafPrivateKey = "<Your Private PEM Key here>";
            ```

            - Utilize o formato seguinte para incluir o certificado e a chave:
            
                ```java
                private static final String leafPublicPem = "-----BEGIN CERTIFICATE-----\n" +
                    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                    "+XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                    "-----END CERTIFICATE-----\n";
                private static final String leafPrivateKey = "-----BEGIN PRIVATE KEY-----\n" +
                    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                    "XXXXXXXXXX\n" +
                    "-----END PRIVATE KEY-----\n";
                ```

    - **Grupos de inscrição**: 

        1. Siga as instruções relativas a **Inscrição individual** acima.

        1. Adicione as seguintes linhas de código ao início da função `main`.
        
            ```java
            String intermediatePem = "<Your Signer Certificate here>";          
            String rootPem = "<Your Root Certificate here>";
                
            signerCertificates.add(intermediatePem);
            signerCertificates.add(root);
            ```
    
            - Utilize o formato seguinte para incluir os seus certificados:
        
                ```java
                String intermediatePem = "-----BEGIN CERTIFICATE-----\n" +
                    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                    "+XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                    "-----END CERTIFICATE-----\n";
                String rootPem = "-----BEGIN CERTIFICATE-----\n" +
                    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                    "+XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                    "-----END CERTIFICATE-----\n";
                ```

1. Compile o exemplo. Navegue até à pasta de destino e execute o ficheiro jar criado.

    ```cmd/sh
    mvn clean install
    cd target
    java -jar ./provisioning-x509-sample-{version}-with-deps.jar
    ```

1. No portal, navegue para o IoT hub ligado ao seu serviço de aprovisionamento e abra o painel **Explorador de Dispositivos**. Após o aprovisionamento bem-sucedido do dispositivo X.509 simulado no hub, o ID de dispositivo aparece no painel **Explorador de Dispositivos**, com o *ESTADO* **ativado**. Tenha em atenção que poderá ter de clicar no botão **Atualizar**, na parte superior, se já tiver aberto o painel antes de executar a aplicação de dispositivo de exemplo. 

    ![O dispositivo é registado no hub IoT](./media/java-quick-create-simulated-device-x509/hub-registration.png) 

> [!NOTE]
> Se tiver alterado o *estado inicial do dispositivo duplo* face ao valor predefinido na entrada de inscrição do seu dispositivo, este pode extrair o estado pretendido do dispositivo duplo a partir do hub e agir em conformidade. Para obter mais informações, veja [Understand and use device twins in IoT Hub](../iot-hub/iot-hub-devguide-device-twins.md) (Compreender e utilizar dispositivos duplos no Hub IoT).
>


## <a name="clean-up-resources"></a>Limpar recursos

Se quiser continuar a trabalhar e a explorar o exemplo de cliente do dispositivo, não limpe os recursos criados neste Guia Rápido. Se não planear continuar, utilize os passos seguintes para eliminar todos os recursos criados no Guia Rápido.

1. Feche a janela da saída do exemplo de dispositivo cliente no seu computador.
1. No menu do lado esquerdo do portal do Azure, clique em **Todos os recursos** e selecione o seu Serviço Aprovisionamento de Dispositivos. Abra o painel **Gerir Inscrições** para o seu serviço e, em seguida, clique no separador **Inscrições Individuais**. Selecione o *ID DE REGISTO* do dispositivo que inscreveu neste Manual de Início Rápido e clique no botão **Eliminar** na parte superior. 
1. No menu do lado esquerdo do portal do Azure, clique em **Todos os recursos** e selecione o seu hub IoT. Abra o painel **Dispositivos IoT** do seu hub, selecione o *ID DE DISPOSITIVO* do dispositivo que registou neste Manual de Início Rápido e, em seguida, clique no botão **Eliminar** na parte superior.


## <a name="next-steps"></a>Passos seguintes

e para o hub IoT ligado ao seu serviçNeste Início Rápido, criou um dispositivo X.509 simulado no seu computador Windows e aprovisionou-o no seu hub IoT com o Serviço Aprovisionamento de Dispositivos no Hub IoT do Azure no portal. Para saber como inscrever o seu dispositivo X.509 programaticamente, continue para o Manual de Início Rápido para inscrição programática de dispositivos X.509. 

> [!div class="nextstepaction"]
> [Manual de Início Rápido do Azure - Inscrever dispositivos X.509 no Serviço de Aprovisionamento de Dispositivos no Hub IoT do Azure](quick-enroll-device-x509-java.md)
