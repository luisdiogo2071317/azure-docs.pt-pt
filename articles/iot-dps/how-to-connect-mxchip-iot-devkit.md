---
title: Como utilizar o aprovisionamento automático do serviço de aprovisionamento de dispositivos do Azure IoT Hub para registar o MXChip IoT DevKit com o IoT Hub | Documentos da Microsoft
description: Como utilizar o aprovisionamento automático do serviço de aprovisionamento de dispositivos do Azure IoT Hub para registar o MXChip IoT DevKit com o IoT Hub.
author: liydu
ms.author: liydu
ms.date: 04/04/2018
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: jeffya
ms.openlocfilehash: d8912a5da8c4df2069d8bc53454748b5fb3d5c39
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/20/2018
ms.locfileid: "42056495"
---
# <a name="use-azure-iot-hub-device-provisioning-service-auto-provisioning-to-register-the-mxchip-iot-devkit-with-iot-hub"></a>Utilizar o aprovisionamento automático do serviço de aprovisionamento de dispositivos do Azure IoT Hub para registar o MXChip IoT DevKit com o IoT Hub

Este artigo descreve como utilizar o serviço de aprovisionamento de dispositivos do Azure IoT Hub [aprovisionamento automático](concepts-auto-provisioning.md), para registar o MXChip IoT DevKit com o IoT Hub do Azure. Neste tutorial, ficará a saber como:

* Configure o ponto final global do serviço aprovisionamento de dispositivos num dispositivo.
* Utilize um segredo de dispositivo exclusivo (UDS) para gerar um certificado X.509.
* Inscreva um dispositivo individual.
* Certifique-se de que o dispositivo está registado.

O [MXChip IoT DevKit](https://aka.ms/iot-devkit) é um quadro de compatível com Arduino tudo-em-um com periféricos avançados e sensores. Pode desenvolver para o mesmo ao utilizar o [extensão do Visual Studio Code para Arduino](https://aka.ms/arduino). O DevKit vem com um crescimento [catálogo de projetos](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/) para orientar suas soluções de Internet das coisas (IoT) de protótipo que tiram partido dos serviços do Azure.

## <a name="before-you-begin"></a>Antes de começar

Para concluir os passos neste tutorial, primeiro faça as seguintes tarefas:

* Preparar seu DevKit ao seguir os passos em [ligar IoT DevKit AZ3166 hub do IoT do Azure na cloud](/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started).
* Atualizar para o firmware mais recente (1.3.0 ou posterior) com o [DevKit de atualização de firmware](https://microsoft.github.io/azure-iot-developer-kit/docs/firmware-upgrading/) tutorial.
* Criar e ligar um Hub de IoT com uma instância do serviço aprovisionamento de dispositivos ao seguir os passos em [configurar o serviço de aprovisionamento das dispositivo IoT Hub no portal do Azure](/azure/iot-dps/quick-setup-auto-provision).

## <a name="build-and-deploy-auto-provisioning-registration-software-to-the-device"></a>Criar e implementar o software de registo de aprovisionamento automático para o dispositivo

Para ligar o DevKit para a instância do serviço aprovisionamento de dispositivos que criou:

1. No portal do Azure, selecione o **descrição geral** painel do seu serviço aprovisionamento de dispositivos e anote o **ponto final do dispositivo Global** e **âmbito do ID** valores.
  ![Ponto final Global do serviço e o âmbito do ID de aprovisionamento de dispositivos](./media/how-to-connect-mxchip-iot-devkit/dps-global-endpoint.png)

2. Certifique-se de que tem `git` instalado no seu computador e que é adicionado às variáveis de ambiente acessíveis na janela de comando. Ver [ferramentas de cliente do Git da Software Freedom Conservancy](https://git-scm.com/download/) ter a versão mais recente instalada.

3. Abra uma linha de comandos. Clone o repositório do GitHub para o código de exemplo do serviço de aprovisionamento de dispositivos:
  ```bash
  git clone https://github.com/DevKitExamples/DevKitDPS.git
  ```

4. Abra o Visual Studio Code, ligar o DevKit ao seu computador e, em seguida, abra a pasta que contém o código que clonou.

5. Open **DevKitDPS.ino**. Localizar e substituir `[Global Device Endpoint]` e `[ID Scope]` com os valores que anotou apenas para baixo.
  ![Ponto final do serviço aprovisionamento de dispositivo](./media/how-to-connect-mxchip-iot-devkit/endpoint.png) pode deixar o **registrationId** em branco. O aplicativo gera uma por si com base na versão do endereço e o firmware do MAC. Se desejar personalizar o ID de registo, tem de utilizar apenas minúsculas, de alfanuméricos e hífen combinações com um máximo de 128 carateres. Para obter mais informações, consulte [gerir inscrições de dispositivos com o portal do Azure](https://docs.microsoft.com/azure/iot-dps/how-to-manage-enrollments).

6. Usar a Quick Open no VS Code (Windows: `Ctrl+P`, macOS: `Cmd+P`) e o tipo *carregamento de dispositivo de tarefas* para criar e carregar o código para o DevKit.

7. A janela de saída mostra se a tarefa foi concluída com êxito.

## <a name="save-a-unique-device-secret-on-an-stsafe-security-chip"></a>Guardar um segredo de dispositivo exclusivo num chip de segurança STSAFE

Aprovisionamento automático pode ser configurado num dispositivo com base do dispositivo [mecanismo de atestado](concepts-security.md#attestation-mechanism). O MXChip IoT DevKit utiliza a [mecanismo de composição de identidade do dispositivo](https://trustedcomputinggroup.org/wp-content/uploads/Foundational-Trust-for-IOT-and-Resource-Constrained-Devices.pdf) da [Trusted Computing Group](https://trustedcomputinggroup.org). R *segredo de dispositivo exclusivo* (UDS) salvos numa segurança STSAFE chip no DevKit é usado para gerar o dispositivo do exclusivo [certificado X.509](concepts-security.md#x509-certificates). O certificado é utilizado mais tarde para o processo de inscrição no serviço aprovisionamento de dispositivos e durante o registo em tempo de execução.

Um segredo de dispositivo exclusivo típico é uma cadeia de caracteres de 64, como visto no exemplo a seguir:

```
19e25a259d0c2be03a02d416c05c48ccd0cc7d1743458aae1cb488b074993eae
```

A cadeia de caracteres é dividida em pares de carateres que são utilizados no cálculo de segurança. O exemplo anterior, domínios de Atualização é resolvido para: `0x19`, `0xe2`, `0x5a`, `0x25`, `0x9d`, `0x0c`, `0x2b`, `0xe0`, `0x3a`, `0x02`, `0xd4`, `0x16` , `0xc0`, `0x5c`, `0x48`, `0xcc`, `0xd0`, `0xcc`, `0x7d`, `0x17`, `0x43`, `0x45`, `0x8a`, `0xae`, `0x1c`, `0xb4`, `0x88`, `0xb0`, `0x74`, `0x99`, `0x3e`, `0xae`.

Para guardar um segredo de dispositivo exclusivo no DevKit:

1. Abra o monitor serial usando uma ferramenta como o Putty. Ver [modo de configuração de uso](https://microsoft.github.io/azure-iot-developer-kit/docs/use-configuration-mode/) para obter detalhes.

2. Com o DevKit conectado ao computador, mantenha premida a **uma** botão e, em seguida, prima e liberar o **repor** botão para entrar no modo de configuração. O ecrã mostra a DevKit ID e a configuração.

3. Tire o exemplo de domínios de Atualização de cadeias de caracteres e alterar um ou mais carateres para outros valores entre `0` e `f` para seus próprios domínios de Atualização.

4. Na janela serial monitor, digite *set_dps_uds [your_own_uds_value]* e selecione Enter.
  > [!NOTE]
  > Por exemplo, se definir seus próprios domínios de Atualização, alterando os dois últimos carateres para `f`, tem de introduzir o comando como este: `set_dps_uds 19e25a259d0c2be03a02d416c05c48ccd0cc7d1743458aae1cb488b074993eff`.

5. Sem fechar a janela de serial monitor, prima a **repor** botão o DevKit.

6. Tome nota da **endereço de MAC DevKit** e **versão de Firmware DevKit** valores.
  ![Versão de firmware](./media/how-to-connect-mxchip-iot-devkit/firmware-version.png)

## <a name="generate-an-x509-certificate"></a>Gerar um certificado X.509

Agora precisa gerar um certificado X.609. 

### <a name="windows"></a>Windows

1. Abra o Explorador de ficheiros e vá para a pasta que contém o código de exemplo do serviço aprovisionamento de dispositivos que clonados anteriormente. Na **.build** pastas, localizar e copia **DPS.ino.bin** e **DPS.ino.map**.
  ![Arquivos gerados](./media/how-to-connect-mxchip-iot-devkit/generated-files.png)
  > [!NOTE]
  > Se tiver alterado o `built.path` configuração para Arduino para outra pasta, precisa localizar esses ficheiros na pasta que configurou.

2. Cole esses dois arquivos para o **ferramentas** pasta no mesmo nível com o **.build** pasta.

3. Execute **dps_cert_gen.exe**. Siga as indicações para introduzir sua **domínios de Atualização**, o **endereço MAC** para DevKit e o **versão de firmware** para gerar o certificado X.509.
  ![Execute o dps-cert-gen.exe](./media/how-to-connect-mxchip-iot-devkit/dps-cert-gen.png)

4. Depois do certificado X.509 for gerado, uma **. pem** certificado é guardado na mesma pasta.

## <a name="create-a-device-enrollment-entry-in-the-device-provisioning-service"></a>Criar uma entrada de inscrição de dispositivos no serviço aprovisionamento de dispositivos

1. No portal do Azure, aceda à sua instância do serviço aprovisionamento de dispositivos. Selecione **gerir inscrições**e, em seguida, selecione a **inscrições individuais** separador. ![Inscrições individuais](./media/how-to-connect-mxchip-iot-devkit/individual-enrollments.png)

2. Selecione **Adicionar**.

3. No painel "Adicionar inscrição":

   - Selecione **X.509** sob **mecanismo**.
   - Clique em "Selecione um ficheiro de" em **ficheiro. pem ou. cer de certificado primário**.
   - Na caixa de diálogo arquivo abrir, navegue para e carregar os **. pem** certificado que acabou de gerar.
   - Deixe o resto como padrão e clique em **guardar**.

   ![Carregar certificado](./media/how-to-connect-mxchip-iot-devkit/upload-cert.png)

  > [!NOTE]
  > Se tiver um erro com esta mensagem:
  >
  > `{"message":"BadRequest:{\r\n \"errorCode\": 400004,\r\n \"trackingId\": \"1b82d826-ccb4-4e54-91d3-0b25daee8974\",\r\n \"message\": \"The certificate is not a valid base64 string value\",\r\n \"timestampUtc\": \"2018-05-09T13:52:42.7122256Z\"\r\n}"}`
  >
  > Abra o ficheiro de certificado **. pem** como texto (Abrir com o bloco de notas ou qualquer editor de texto) e eliminar as linhas:
  >
  > `"-----BEGIN CERTIFICATE-----"` e `"-----END CERTIFICATE-----"`.
  >

## <a name="start-the-devkit"></a>Iniciar o DevKit

1. Abra o VS Code e o monitor serial.

2. Prima a **repor** botão no seu DevKit.

Veja o início de DevKit o registo com o seu serviço aprovisionamento de dispositivos.

![Saída de código VS](./media/how-to-connect-mxchip-iot-devkit/vscode-output.png)

## <a name="verify-that-the-devkit-is-registered-with-azure-iot-hub"></a>Certifique-se de que o DevKit está registado no IoT Hub do Azure

Depois do dispositivo for arrancado, são efetuadas as seguintes ações:

1. O dispositivo envia um pedido de registo ao Serviço Aprovisionamento de Dispositivos.
2. O serviço de aprovisionamento de dispositivos envia de volta um desafio de registo para o qual o seu dispositivo vai responder.
3. O registo bem-sucedido, o serviço de aprovisionamento de dispositivos envia o URI do Hub IoT, ID de dispositivo e a chave de encriptação volta para o dispositivo.
4. A aplicação de cliente do IoT Hub no dispositivo liga ao seu hub.
5. Em ligação com êxito para o hub, verá o dispositivo aparecer no do IoT Hub Device Explorer.
  ![Dispositivo registado](./media/how-to-connect-mxchip-iot-devkit/device-registered.png)

## <a name="problems-and-feedback"></a>Problemas e comentários

Se tiver problemas, consulte a Iot DevKit [FAQs](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/), ou entrar em contacto com os seguintes canais de suporte:

* [Gitter.im](http://gitter.im/Microsoft/azure-iot-developer-kit)
* [Stackoverflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, aprendeu a inscrever um dispositivo de forma segura para o serviço de aprovisionamento de dispositivos por meio do mecanismo de composição de identidade de dispositivo, para que o dispositivo pode ser registado automaticamente no IoT Hub do Azure. 

Em resumo, aprendeu como:

> [!div class="checklist"]
> * Configure o ponto final global do serviço aprovisionamento de dispositivos num dispositivo.
> * Utilize um segredo de dispositivo exclusivo para gerar um certificado X.509.
> * Inscreva um dispositivo individual.
> * Certifique-se de que o dispositivo está registado.

Saiba como [criar e aprovisionar um dispositivo simulado](./quick-create-simulated-device.md).

