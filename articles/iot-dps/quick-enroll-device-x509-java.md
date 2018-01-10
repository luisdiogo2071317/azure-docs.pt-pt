---
title: "Inscrever dispositivos X.509 no Serviço de Aprovisionamento de Dispositivos do Azure com Java | Microsoft Docs"
description: "Manual de Início Rápido do Azure - Inscrever dispositivos X.509 no Serviço de Aprovisionamento de Dispositivos no Hub IoT do Azure com o SDK do serviço Java"
services: iot-dps
keywords: 
author: dsk-2015
ms.author: dkshir
ms.date: 12/20/2017
ms.topic: hero-article
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: f05524f9ec70fea243ab6cb3e87a70e762b0fa48
ms.sourcegitcommit: a648f9d7a502bfbab4cd89c9e25aa03d1a0c412b
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/22/2017
---
# <a name="enroll-x509-devices-to-iot-hub-device-provisioning-service-using-java-service-sdk"></a>Inscrever dispositivos X.509 no Serviço de Aprovisionamento de Dispositivos no Hub IoT com o SDK do serviço Java
> [!div class="op_single_selector"]
> * [Java](quick-enroll-device-x509-java.md)
> * [Node.js](quick-enroll-device-x509-node.md)

Estes passos explicam como inscrever um grupo de dispositivos X.509 simulados através de programação no Serviço de Aprovisionamento de Dispositivos no Hub IoT do Azure, com o [SDK do Serviço Java](https://azure.github.io/azure-iot-sdk-java/service/) com a ajuda de um exemplo de aplicação Java. Embora o SDK do serviço Java funcione em computadores Windows e Linux, este artigo utiliza um computador de desenvolvimento Windows para percorrer o processo de inscrição.

Antes de avançar, certifique-se de que [configura o Serviço de Aprovisionamento de Dispositivos no Hub IoT com o portal do Azure](./quick-setup-auto-provision.md).

<a id="setupdevbox"></a>

## <a name="prepare-the-development-environment"></a>Preparar o ambiente de desenvolvimento 

1. Certifique-se de que tem o [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) instalado no seu computador. 

2. Configure variáveis de ambiente para a sua instalação de Java. A variável `PATH` deve incluir o caminho completo para o diretório *jdk1.8.x\bin*. Se esta for a primeira instalação de Java do seu computador, crie uma nova variável de ambiente com o nome `JAVA_HOME` e aponte-a para o caminho completo para o diretório *jdk1.8.x*. Num computador Windows, este diretório encontra-se, normalmente, na pasta *C:\\Program Files\\Java\\* e pode criar ou editar variáveis de ambiente, procurando **Editar as variáveis de ambiente do sistema** no **Painel de controlo** do seu computador Windows. 

  Para verificar se Java está configurado com êxito no seu computador, execute o seguinte comando na janela de comando:

    ```cmd\sh
    java -version
    ```

3. Transfira e extraia [Maven 3](https://maven.apache.org/download.cgi) no seu computador. 

4. Edite a variável de ambiente `PATH` para apontar para a pasta *apache-maven-3.x.x\\bin* dentro da pasta onde o Maven é extraído. Pode confirmar se o Maven é instalado com êxito ao executar este comando na janela de comando:

    ```cmd\sh
    mvn --version
    ```

5. Certifique-se de que o [git](https://git-scm.com/download/) está instalado no computador e é adicionado à variável de ambiente `PATH`. 


<a id="javasample"></a>

## <a name="download-and-modify-the-java-sample-code"></a>Transferir e modificar o código de exemplo de Java

Esta secção mostra como adicionar os detalhes de aprovisionamento do seu dispositivo X.509 ao código de exemplo. 

1. Abra uma linha de comandos. Clone o repositório do GitHub para o exemplo de código de inscrição de dispositivos com o SDK do serviço Java:
    
    ```cmd\sh
    git clone https://github.com/Azure/azure-iot-sdk-java.git --recursive
    ```

2. No código fonte transferido, navegue até à pasta de exemplo **_azure-iot-sdk-java/provisioning/provisioning-samples/service-enrollment-group-sample_**. Abra o ficheiro **_/src/main/java/samples/com/microsoft/azure/sdk/iot/ServiceEnrollmentGroupSample.java_** num editor à sua escolha e adicione os seguintes detalhes:

    1. Adicione `[Provisioning Connection String]` para o seu serviço de aprovisionamento, a partir do portal da seguinte forma:
        1. Navegue até ao serviço de aprovisionamento no [portal do Azure](https://portal.azure.com). 
        2. Abra as **Políticas de acesso partilhado**e selecione uma política que tenha a permissão *EnrollmentWrite*.
        3. Copie a **Cadeia de ligação da chave primária**. 

            ![Obter a cadeia de ligação de aprovisionamento a partir do portal](./media/quick-enroll-device-x509-java/provisioning-string.png)  

        4. No ficheiro de código de exemplo **_ServiceEnrollmentGroupSample.java_**, substitua a cadeia `[Provisioning Connection String]` pela **Cadeia de ligação da chave primária**.

            ```Java
            private static final String PROVISIONING_CONNECTION_STRING = "[Provisioning Connection String]";
            ```

    2. Adicione o certificado de raiz para o grupo de dispositivos. Se precisar de um certificado de raiz de exemplo, utilize a ferramenta _X.509 certificate generator_ da seguinte forma:
        1. Numa janela de comando, navegue até à pasta **_azure-iot-sdk-java/provisioning/provisioning-tools/provisioning-x509-cert-generator_**.
        2. Compile a ferramenta através da execução do seguinte comando:

                ```cmd\sh
                mvn clean install
                ```

        4. Execute a ferramenta com os comandos seguintes:

                ```cmd\sh
                cd target
                java -jar ./provisioning-x509-cert-generator-{version}-with-deps.jar
                ```

        5. Quando lhe for pedido, pode introduzir opcionalmente um _Nome Comum_ para os certificados.
        6. A ferramenta gera localmente um **Certificado de Cliente**, a **Chave Privada do Certificado de Cliente** e o **Certificado de Raiz**.
        7. Copie o **Certificado de Raiz**, incluindo as linhas **_-----BEGIN CERTIFICATE-----_** e **_-----END CERTIFICATE-----_**. 
        8. Atribua o valor do **Certificado de Raiz** ao parâmetro **PUBLIC_KEY_CERTIFICATE_STRING**, conforme apresentado abaixo:

                ```Java
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

        9. Feche a janela de comando ou introduza **n** quando lhe for pedido um *Código de Verificação*. 
 
    3. Opcionalmente, pode configurar o seu serviço de aprovisionamento através do código de exemplo:
        - Para adicionar esta configuração ao exemplo, siga estes passos:
            1. Navegue até ao hub IoT ligado ao seu serviço de aprovisionamento no [portal do Azure](https://portal.azure.com). Abra o separador **Descrição geral** para o hub e copie o **Hostname**. Atribua este **Hostname** ao parâmetro *IOTHUB_HOST_NAME*.

                ```Java
                private static final String IOTHUB_HOST_NAME = "[Host name].azure-devices.net";
                ```
            2. Atribua um nome amigável ao parâmetro *DEVICE_ID* e mantenha o *PROVISIONING_STATUS* como o valor *ATIVADO* predefinido. 

        - OU, se optar por não configurar o serviço de aprovisionamento, certifique-se de que comenta ou elimina as seguintes instruções no ficheiro _ServiceEnrollmentGroupSample.java_:

            ```Java
            enrollmentGroup.setIotHubHostName(IOTHUB_HOST_NAME);                // Optional parameter.
            enrollmentGroup.setProvisioningStatus(ProvisioningStatus.ENABLED);  // Optional parameter.
            ```

    4. Estude o código de exemplo. Este cria, atualiza, consulta e elimina uma inscrição de grupo para dispositivos X.509. Para verificar a inscrição com êxito no portal, comente temporariamente as seguintes linhas de código no final do ficheiro _ServiceEnrollmentGroupSample.java_:

        ```Java
        // ************************************** Delete info of enrollmentGroup ***************************************
        System.out.println("\nDelete the enrollmentGroup...");
        provisioningServiceClient.deleteEnrollmentGroup(enrollmentGroupId);
        ```

    5. Guarde o ficheiro _ServiceEnrollmentGroupSample.java_. 
 

<a id="runjavasample"></a>

## <a name="build-and-run-sample-group-enrollment"></a>Compilar e executar a inscrição de grupo de exemplo

1. Abra uma janela de comando e navegue até à pasta **_azure-iot-sdk-java/provisioning/provisioning-samples/service-enrollment-group-sample_**.

2. Compile o código de exemplo, utilizando este comando:

    ```cmd\sh
    mvn install -DskipTests
    ```

   Este comando transfere o pacote Maven [`com.microsoft.azure.sdk.iot.provisioning.service`](https://www.mvnrepository.com/artifact/com.microsoft.azure.sdk.iot.provisioning/provisioning-service-client) para o seu computador. Este pacote inclui os binários para o SDK do serviço Java, que o código de exemplo precisa de compilar. Se tiver executado a ferramenta _X.509 certificate generator_ na secção anterior, este pacote já estará transferido no seu computador. 

3. Execute o exemplo, utilizando estes comandos na janela de comando:

    ```cmd\sh
    cd target
    java -jar ./service-enrollment-group-sample-{version}-with-deps.jar
    ```

4. Observe a janela de saída para ver se a inscrição foi executada com êxito.

5. Navegue até ao serviço de aprovisionamento no portal do Azure. Clique em **Gerir inscrições**. Repare que o seu grupo de dispositivos X.509 é apresentado no separador **Grupos de Inscrição**, com um *NOME DE GRUPO* gerado automaticamente. 

    ![Verificar a inscrição do X.509 com êxito no portal](./media/quick-enroll-device-x509-java/verify-x509-enrollment.png)  

## <a name="modifications-to-enroll-a-single-x509-device"></a>Modificações para inscrever um dispositivo X.509 único

Para inscrever um dispositivo X.509 único, modifique o código de exemplo de *inscrição individual* utilizado em [Inscrever o dispositivo TPM no Serviço de Aprovisionamento de Dispositivos no Hub IoT com o SDK do serviço Java](quick-enroll-device-tpm-java.md#javasample) da seguinte forma:

1. Copie o *Nome Comum* do seu certificado de cliente X.509 para a área de transferência. Se pretender utilizar a ferramenta _X.509 certificate generator_ conforme apresentado na [secção do código de exemplo anterior](#javasample), introduza um _Nome Comum_ para o seu certificado ou utilize o **microsoftriotcore** predefinido. Utilize este **Nome Comum** como o valor para a variável *REGISTRATION_ID*. 

    ```Java
    // Use common name of your X.509 client certificate
    private static final String REGISTRATION_ID = "[RegistrationId]";
    ```

2. Mude o nome da variável *TPM_ENDORSEMENT_KEY* para *PUBLIC_KEY_CERTIFICATE_STRING*. Copie o certificado de cliente ou o **Client Cert** da saída da ferramenta _X.509 certificate generator_, como o valor para a variável *PUBLIC_KEY_CERTIFICATE_STRING*. 

    ```Java
    // Rename the variable *TPM_ENDORSEMENT_KEY* as *PUBLIC_KEY_CERTIFICATE_STRING*
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
3. Na função **principal**, substitua a linha `Attestation attestation = new TpmAttestation(TPM_ENDORSEMENT_KEY);` pelo seguinte para utilizar o certificado de cliente X.509:
    ```Java
    Attestation attestation = X509Attestation.createFromClientCertificates(PUBLIC_KEY_CERTIFICATE_STRING);
    ```

4. Guarde, compile e execute o ficheiro de exemplo de *inscrição individual* com os passos descritos na secção [Compilar e executar o código de exemplo para inscrição individual](quick-enroll-device-tpm-java.md#runjavasample).


## <a name="clean-up-resources"></a>Limpar recursos
Se quiser explorar o exemplo de serviço Java, não limpe os recursos criados neste Manual de Início Rápido. Se não planear continuar, utilize os passos seguintes para eliminar todos os recursos criados no Guia Rápido.

1. Feche a janela da saída do exemplo de Java no seu computador.
1. Feche a janela do _X509 Cert Generator_ no seu computador.
1. Navegue até ao seu serviço de Aprovisionamento de Dispositivos no portal do Azure, clique em **Gerir inscrições** e, em seguida, selecione o separador **Grupos de Inscrição**. Selecione o *NOME DE GRUPO* dos dispositivos X.509 que inscreveu com este Manual de Início Rápido e clique no botão **Eliminar** na parte superior do painel.  

## <a name="next-steps"></a>Passos seguintes
Neste Manual de Início Rápido, inscreveu um grupo de dispositivos X.509 no seu serviço de Aprovisionamento de Dispositivos. Para ficar a conhecer aprofundadamente o aprovisionamento de dispositivos, prossiga no tutorial para a configuração do Serviço Aprovisionamento de Dispositivos no portal do Azure. 

> [!div class="nextstepaction"]
> [Azure IoT Hub Device Provisioning Service tutorials](./tutorial-set-up-cloud.md) (Tutoriais do Serviço Aprovisionamento de Dispositivos no Hub IoT do Azure)
