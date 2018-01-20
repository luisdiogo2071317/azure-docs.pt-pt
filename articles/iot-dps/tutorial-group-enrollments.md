---
title: "Aprovisionar um dispositivo simulado de x. 509 ao utilizar grupos de Java e a inscrição do Azure IoT Hub | Microsoft Docs"
description: "Tutorial do Azure – criar e aprovisionar um dispositivo simulado de x. 509 utilizando Java dispositivo e o serviço SDK e inscrição grupos para aprovisionamento de serviço do dispositivos do Hub IoT"
services: iot-dps
keywords: 
author: msebolt
ms.author: v-masebo
ms.date: 01/04/2018
ms.topic: tutorial
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: java
ms.custom: mvc
ms.openlocfilehash: 14e5e7613fd5df650625cf8997d569b754ceb689
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/20/2018
---
# <a name="create-and-provision-a-simulated-x509-device-using-java-device-and-service-sdk-and-group-enrollments-for-iot-hub-device-provisioning-service"></a>Criar e aprovisionar um dispositivo simulado de x. 509 utilizando o SDK do serviço e dispositivo de Java e a inscrições de grupo para o serviço de aprovisionamento de dispositivos IoT Hub

Estes passos mostram como simular um dispositivo de x. 509 no computador de desenvolvimento SO Windows em execução e utilizam um código de exemplo para ligar este dispositivo simulado com o serviço de aprovisionamento de dispositivos e o IoT hub com grupos de inscrição. 

Antes de avançar, certifique-se de que executa os passos descritos em [Configurar o Serviço de Aprovisionamento de Dispositivos no Hub IoT com o portal do Azure](./quick-setup-auto-provision.md).


## <a name="prepare-the-environment"></a>Preparar o ambiente 

1. Certifique-se de que tem o [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) instalado no seu computador.

1. Transfira e instale o [Maven](https://maven.apache.org/install.html).

1. Verifique se `git` está instalado no computador e que é adicionado às variáveis de ambiente às quais a janela de comandos pode aceder. Veja as [ferramentas de cliente Git da Software Freedom Conservancy](https://git-scm.com/download/) relativamente à mais recente versão das ferramentas de `git` a instalar, que incluem o **Git Bash**, a aplicação de linha de comandos que pode utilizar para interagir com o seu repositório Git local. 

1. Utilize o seguinte [descrição geral de certificados](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md) para criar os certificados de teste. Para uma visão mais detalhada das criar certificados, consulte [scripts do PowerShell para gerir os certificados x. 509 assinado para AC](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-security-x509-create-certificates).

    > [!NOTE]
    > Este passo necessita de [OpenSSL](https://www.openssl.org/), que pode: ser criado e instalado a partir da origem ou transferido e instalado a partir de um [terceiros 3rd](https://wiki.openssl.org/index.php/Binaries) como [isto](https://sourceforge.net/projects/openssl/). Se já tiver criado o _raiz_, _intermédio_ e _dispositivo_ certificados, pode ignorar este passo.
    >

1. Crie as informações do grupo de inscrição:

    1. Percorrer **passo 1** e **passo 2** para criar o _raiz_ e _intermédio_ certificados.

    1. Inicie sessão no portal do Azure, clique no botão **Todos os recursos**, no menu do lado esquerdo, e abra o seu serviço de aprovisionamento.

        1. No painel de resumo do Serviço de Aprovisionamento de Dispositivos, selecione **Certificados** e clique no botão **Adicionar** na parte superior.

        1. Em **Adicionar Certificado**, introduza as informações seguintes:
            - Introduza um nome exclusivo de certificado.
            - Selecione o  **_RootCA.pem_**  ficheiro que acabou de criar.
            - Quando tiver terminado, clique no botão **Guardar**.

        ![Adicionar certificado](./media/tutorial-group-enrollments/add-certificate.png)

        1. Selecione o certificado que acabou de criar:
            - Clique em **Gerar Código de Verificação**. Copie o código gerado.
            - Percorrer **passo 3**. Introduza o _código de verificação_ ou rato ao colar na janela do PowerShell em execução.  Prima **Enter**.
            - Selecione o recém-criado  **_verifyCert4.pem_**  ficheiro no portal do Azure. Clique em **verificar**.

            ![Validar certificado](./media/tutorial-group-enrollments/validate-certificate.png)

1. Concluir executando **passo 4** e **passo 5** para criar os certificados de dispositivos e recursos de limpeza.

> [!NOTE]
> Ao criar certificados de dispositivo não se esqueça de utilizar apenas alphanumerics minúsculos e hífenes no nome do dispositivo.
>


## <a name="create-a-device-enrollment-entry"></a>Criar uma entrada de inscrição de dispositivos

1. Abra uma linha de comandos. Clone o repositório do GitHub para exemplos de código Java SDK:
    
    ```cmd/sh
    git clone https://github.com/Azure/azure-iot-sdk-java.git --recursive
    ```

1. No código fonte transferido, navegue até à pasta de exemplo **_azure-iot-sdk-java/provisioning/provisioning-samples/service-enrollment-group-sample_**. Abra o ficheiro **_/src/main/java/samples/com/microsoft/azure/sdk/iot/ServiceEnrollmentGroupSample.java_** num editor à sua escolha e adicione os seguintes detalhes:

    1. Adicione `[Provisioning Connection String]` para o seu serviço de aprovisionamento, a partir do portal da seguinte forma:

        1. Navegue até ao serviço de aprovisionamento no [portal do Azure](https://portal.azure.com). 

        1. Abra as **Políticas de acesso partilhado**e selecione uma política que tenha a permissão *EnrollmentWrite*.
    
        1. Copie a **Cadeia de ligação da chave primária**. 

            ![Obter a cadeia de ligação de aprovisionamento a partir do portal](./media/tutorial-group-enrollments/provisioning-string.png)  

        1. No ficheiro de código de exemplo **_ServiceEnrollmentGroupSample.java_**, substitua a cadeia `[Provisioning Connection String]` pela **Cadeia de ligação da chave primária**.

            ```java
            private static final String PROVISIONING_CONNECTION_STRING = "[Provisioning Connection String]";
            ```

    1. Abra o  **_RootCA.pem_**  ficheiro num editor de texto. Atribua o valor do **Certificado de Raiz** ao parâmetro **PUBLIC_KEY_CERTIFICATE_STRING**, conforme apresentado abaixo:

        ```java
        private static final String PUBLIC_KEY_CERTIFICATE_STRING =
                "-----BEGIN CERTIFICATE-----\n" +
                "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "-----END CERTIFICATE-----\n";
        ```
 
    1. Navegue até ao hub IoT ligado ao seu serviço de aprovisionamento no [portal do Azure](https://portal.azure.com). Abra o separador **Descrição geral** para o hub e copie o **Hostname**. Atribua este **Hostname** ao parâmetro *IOTHUB_HOST_NAME*.

        ```java
        private static final String IOTHUB_HOST_NAME = "[Host name].azure-devices.net";
        ```

    1. Estude o código de exemplo. Este cria, atualiza, consulta e elimina uma inscrição de grupo para dispositivos X.509. Para verificar a inscrição com êxito no portal, comente temporariamente as seguintes linhas de código no final do ficheiro _ServiceEnrollmentGroupSample.java_:

        ```java
        // ************************************** Delete info of enrollmentGroup ***************************************
        System.out.println("\nDelete the enrollmentGroup...");
        provisioningServiceClient.deleteEnrollmentGroup(enrollmentGroupId);
        ```

    1. Guarde o ficheiro _ServiceEnrollmentGroupSample.java_. 

1. Abra uma janela de comando e navegue até à pasta **_azure-iot-sdk-java/provisioning/provisioning-samples/service-enrollment-group-sample_**.

1. Compile o código de exemplo, utilizando este comando:

    ```cmd\sh
    mvn install -DskipTests
    ```

1. Execute o exemplo, utilizando estes comandos na janela de comando:

    ```cmd\sh
    cd target
    java -jar ./service-enrollment-group-sample-{version}-with-deps.jar
    ```

1. Observe a janela de saída para ver se a inscrição foi executada com êxito.

    ![inscrição concluída com êxito](./media/tutorial-group-enrollments/enrollment.png) 

1. Navegue até ao serviço de aprovisionamento no portal do Azure. Clique em **Gerir inscrições**. Repare que o seu grupo de dispositivos X.509 é apresentado no separador **Grupos de Inscrição**, com um *NOME DE GRUPO* gerado automaticamente. 


## <a name="simulate-the-device"></a>Simular o dispositivo

1. No painel de resumo do Serviço de Aprovisionamento de Dispositivos, selecione **Descrição geral** e tome nota do _Âmbito do ID_ e do _Ponto Final Global do Serviço de Aprovisionamento_.

    ![Informações de serviço](./media/tutorial-group-enrollments/extract-dps-endpoints.png)

1. Abra uma linha de comandos. Navegue até à pasta do projeto de exemplo.

    ```cmd/sh
    cd azure-iot-sdk-java/provisioning/provisioning-samples/provisioning-X509-sample
    ```

1. Introduza as informações do grupo de inscrição da seguinte forma:

    - Edite `/src/main/java/samples/com/microsoft/azure/sdk/iot/ProvisioningX509Sample.java` para incluir o _Âmbito do ID_ e o _Ponto Final Global do Serviço de Aprovisionamento_ conforme indicado anteriormente. Abra o  **_{deviceName}-public.pem_**  de ficheiros e inclua este valor como o _certificados de cliente_. Abra o  **_{deviceName}-all.pem_**  de ficheiros e copie o texto do _---BEGIN de chave privada---_ para _---END de chave privada---_.  Utilizá-lo como o _chave privada do certificado de cliente_.

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

1. Compile o exemplo. Navegue até à pasta de destino e execute o ficheiro jar criado.

    ```cmd/sh
    mvn clean install
    cd target
    java -jar ./provisioning-x509-sample-{version}-with-deps.jar
    ```

    ![Registo com êxito](./media/tutorial-group-enrollments/registration.png)

1. No portal, navegue para o IoT hub ligado ao seu serviço de aprovisionamento e abra o painel **Explorador de Dispositivos**. Após o aprovisionamento bem-sucedido do dispositivo X.509 simulado no hub, o ID de dispositivo aparece no painel **Explorador de Dispositivos**, com o *ESTADO* **ativado**. Tenha em atenção que poderá ter de clicar no botão **Atualizar**, na parte superior, se já tiver aberto o painel antes de executar a aplicação de dispositivo de exemplo. 

    ![O dispositivo é registado no hub IoT](./media/tutorial-group-enrollments/hub-registration.png) 


## <a name="clean-up-resources"></a>Limpar recursos

Se quiser continuar a trabalhar e a explorar o exemplo de cliente do dispositivo, não limpe os recursos criados neste Guia Rápido. Se não planear continuar, utilize os passos seguintes para eliminar todos os recursos criados no Guia Rápido.

1. Feche a janela da saída do exemplo de dispositivo cliente no seu computador.
1. No menu do lado esquerdo do portal do Azure, clique em **Todos os recursos** e selecione o seu Serviço Aprovisionamento de Dispositivos. Abra o painel **Gerir Inscrições** para o seu serviço e, em seguida, clique no separador **Inscrições Individuais**. Selecione o *ID DE REGISTO* do dispositivo que inscreveu neste Início Rápido e clique no botão **Eliminar** na parte superior. 
1. No menu do lado esquerdo do portal do Azure, clique em **Todos os recursos** e selecione o seu hub IoT. Abra o painel **Dispositivos IoT** do seu hub, selecione o *ID DE DISPOSITIVO* do dispositivo que registou neste Início Rápido e, em seguida, clique no botão **Eliminar** Na parte superior.


## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, já criou um dispositivo simulado de x. 509 no seu computador Windows e aprovisionado ao seu IoT hub com o serviço de aprovisionamento de dispositivos do Azure IoT Hub e grupos de inscrição. Para saber mais sobre o seu dispositivo de x. 509, avance para conceitos do dispositivo. 

> [!div class="nextstepaction"]
> [Conceitos de dispositivo do serviço de aprovisionamento de dispositivos IoT Hub](concepts-device.md)
