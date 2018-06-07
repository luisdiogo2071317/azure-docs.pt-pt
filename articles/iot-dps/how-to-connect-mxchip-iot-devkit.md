---
title: Como utilizar o aprovisionamento automático do serviço de aprovisionamento de dispositivos do Azure IoT Hub para registar o DevKit de IoT MXChip com o IoT Hub | Microsoft Docs
description: Como utilizar o aprovisionamento automático do serviço de aprovisionamento de dispositivos do Azure IoT Hub para registar o DevKit de IoT MXChip com o IoT Hub.
author: liydu
ms.author: liydu
ms.date: 04/04/2018
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: jeffya
ms.openlocfilehash: 331e589aadf783fc40ab6efbfb554469c27e48d8
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34629682"
---
# <a name="use-azure-iot-hub-device-provisioning-service-auto-provisioning-to-register-the-mxchip-iot-devkit-with-iot-hub"></a>Utilizar o aprovisionamento automático do serviço de aprovisionamento de dispositivos do Azure IoT Hub para registar o DevKit de IoT MXChip com o IoT Hub

Este artigo descreve como utilizar o serviço de aprovisionamento de dispositivos do Azure IoT Hub [aprovisionamento automático](concepts-auto-provisioning.md), para registar o DevKit de IoT MXChip IoT hub do Azure. Neste tutorial, ficará a saber como:

* Configure o ponto final global do dispositivo aprovisionamento do serviço num dispositivo.
* Utilize um segredo de dispositivo exclusivo (UDS) para gerar um certificado x. 509.
* Inscreva um dispositivo individual.
* Certifique-se de que o dispositivo está registado.

O [MXChip IoT DevKit](https://aka.ms/iot-devkit) é uma placa de Arduino compatível tudo-em-um com periféricos avançados e sensores. Pode desenvolver para o mesmo, utilizando o [extensão do Visual Studio Code para Arduino](https://aka.ms/arduino). O DevKit vem com uma crescer [catálogo projetos](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/) ajudará a suas soluções de Internet das coisas (IoT) de protótipo tirar partido dos serviços do Azure.

## <a name="before-you-begin"></a>Antes de começar

Para concluir os passos neste tutorial, faça primeiro as seguintes tarefas:

* Preparar o seu DevKit seguindo os passos no [AZ3166 de DevKit de IoT estabelecer ligação ao IoT Hub do Azure na nuvem](/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started).
* Atualização do firmware mais recente (1.3.0 ou posterior) com o [DevKit de atualização de firmware](https://microsoft.github.io/azure-iot-developer-kit/docs/firmware-upgrading/) tutorial.
* Criar e ligar um IoT Hub com um dispositivo de aprovisionamento de instância de serviço, seguindo os passos no [configurar o serviço de aprovisionamento de dispositivos IoT Hub com o portal do Azure](/azure/iot-dps/quick-setup-auto-provision).

## <a name="build-and-deploy-auto-provisioning-registration-software-to-the-device"></a>Criar e implementar o software de aprovisionamento automático de registo para o dispositivo

Para ligar o DevKit ao dispositivo de aprovisionamento de instância de serviço que criou:

1. No portal do Azure, selecione o **descrição geral** painel do seu dispositivo aprovisionamento de serviço e tenha em atenção para baixo a **ponto final do dispositivo Global** e **ID âmbito** valores.
  ![DPS Global Endpoint e o âmbito de ID](./media/how-to-connect-mxchip-iot-devkit/dps-global-endpoint.png)

2. Certifique-se de que tem `git` instalado no seu computador e de que é adicionado às variáveis de ambiente acessíveis para a janela de comandos. Consulte [ferramentas de cliente de Git do Software liberdade Conservancy](https://git-scm.com/download/) ter a versão mais recente instalada.

3. Abra uma linha de comandos. Clone o repositório do GitHub para o dispositivo de código de exemplo do serviço de aprovisionamento:
  ```bash
  git clone https://github.com/DevKitExamples/DevKitDPS.git
  ```

4. Abra o Visual Studio Code, ligar a DevKit para o seu computador e, em seguida, abra a pasta que contém o código clonou.

5. Abra **DevKitDPS.ino**. Localizar e substituir `[Global Device Endpoint]` e `[ID Scope]` com valores que apontou apenas para baixo.
  ![Ponto final de DPS](./media/how-to-connect-mxchip-iot-devkit/endpoint.png) pode deixar o **registrationId** em branco. A aplicação gera uma por si com base na versão de firmware e do endereço de MAC. Se pretender personalizar o ID de registo, tem de utilizar apenas alfanuméricos, minúsculas e hífen combinações com um máximo de 128 carateres. Para obter mais informações, consulte [gerir inscrições de dispositivos com o portal do Azure](https://docs.microsoft.com/azure/iot-dps/how-to-manage-enrollments).

6. Utilizar rápida aberta no VS Code (Windows: `Ctrl+P`, macOS: `Cmd+P`) e o tipo *tarefas de carregamento de dispositivo* para criar e carregar o código para o DevKit.

7. Janela de resultados mostra se a tarefa foi concluída com êxito.

## <a name="save-a-unique-device-secret-on-an-stsafe-security-chip"></a>Guardar um segredo de dispositivo exclusivo num chip de segurança STSAFE

Aprovisionamento de automática pode ser configurado num dispositivo baseado do dispositivo [mecanismo de atestado](concepts-security.md#attestation-mechanism). DevKit de IoT MXChip utiliza o [motor de composição de identidade de dispositivo](https://trustedcomputinggroup.org/wp-content/uploads/Foundational-Trust-for-IOT-and-Resource-Constrained-Devices.pdf) do [fidedigna grupo informática](https://trustedcomputinggroup.org). A *segredo de dispositivo exclusivo* (UDS) guardadas em segurança um STSAFE chip no DevKit é utilizado para gerar o dispositivo do exclusivo [certificado x. 509](concepts-security.md#x509-certificates). O certificado é utilizado mais tarde para o processo de inscrição no dispositivo de serviço de fornecimento e durante o registo em tempo de execução.

Um segredo de dispositivo exclusivo típico é uma cadeia de 64 carateres, como mostrado no seguinte exemplo:

```
19e25a259d0c2be03a02d416c05c48ccd0cc7d1743458aae1cb488b074993eae
```

Cada um dos dois carateres é utilizada como o valor hexadecimal o cálculo de segurança. O exemplo anterior UDS é resolvido para: `0x19`, `0xe2`, `0x5a`, `0x25`, `0x9d`, `0x0c`, `0x2b`, `0xe0`, `0x3a`, `0x02`, `0xd4`, `0x16` , `0xc0`, `0x5c`, `0x48`, `0xcc`, `0xd0`, `0xcc`, `0x7d`, `0x17`, `0x43`, `0x45`, `0x8a`, `0xae`, `0x1c`, `0xb4`, `0x88`, `0xb0`, `0x74`, `0x99`, `0x3e`, `0xae`.

Para guardar um segredo de dispositivo exclusivo a DevKit:

1. Abra o monitor de série, utilizando uma ferramenta como o Putty. Consulte [utilizar o modo de configuração](https://microsoft.github.io/azure-iot-developer-kit/docs/use-configuration-mode/) para obter mais detalhes.

2. Com o DevKit ligada ao seu computador, mantenha premida a **A** botão e, em seguida, prima e libertar a **repor** botão para introduzir o modo de configuração. O ecrã mostra o DevKit ID e a configuração.

3. Executar o exemplo UDS cadeia e alterar um ou mais carateres para outros valores entre `0` e `f` para os seus próprios UDS.

4. Na janela monitor de série, escreva *set_dps_uds [your_own_uds_value]* e selecione Enter.
  > [!NOTE]
  > Por exemplo, se definir os seus próprios UDS alterando os dois últimos carateres para `f`, tem de introduzir o comando seguinte: `set_dps_uds 19e25a259d0c2be03a02d416c05c48ccd0cc7d1743458aae1cb488b074993eff`.

5. Sem fechar a janela de monitor de série, prima a **repor** botão a DevKit.

6. Tome nota do **endereço de MAC DevKit** e **versão de Firmware DevKit** valores.
  ![Versão de firmware](./media/how-to-connect-mxchip-iot-devkit/firmware-version.png)

## <a name="generate-an-x509-certificate"></a>Gerar um certificado x. 509

### <a name="windows"></a>Windows

1. Abra o Explorador de ficheiros e vá para a pasta que contém o dispositivo aprovisionamento código de exemplo de serviço que clonou anteriormente. No **.build** pasta, localizar e copiar **DPS.ino.bin** e **DPS.ino.map** na pasta que contém o código.
  ![Ficheiros gerados](./media/how-to-connect-mxchip-iot-devkit/generated-files.png)
  > [!NOTE]
  > Se tiver alterado o `built.path` configuração para Arduino para outra pasta, tem de localizar os ficheiros na pasta que configurou.

2. Colar estes dois ficheiros para o **ferramentas** pasta no mesmo nível com o **.build** pasta.

3. Executar **dps_cert_gen.exe**. Siga as indicações para introduzir o **UDS**, a **endereço MAC** para DevKit e o **versão de firmware** para gerar o certificado x. 509.
  ![Executar dps-certificados-gen.exe](./media/how-to-connect-mxchip-iot-devkit/dps-cert-gen.png)

4. Depois do certificado x. 509 é gerado, um **. pem** certificado é guardado na mesma pasta.

## <a name="create-a-device-enrollment-entry-in-the-device-provisioning-service"></a>Crie uma entrada de inscrição de dispositivo no dispositivo de serviço de fornecimento

1. No portal do Azure, aceda à sua instância do serviço de aprovisionamento de dispositivos. Selecione **gerir inscrições**e, em seguida, selecione o **inscrições individuais** separador. ![Inscrições individuais](./media/how-to-connect-mxchip-iot-devkit/individual-enrollments.png)

2. Selecione **Adicionar**.

3. No painel "Adicionar inscrição":
   - Selecione **x. 509** em **mecanismo**
   - Clique em "Selecionar um ficheiro de" em **ficheiro. pem ou. cer de certificado principal**
   - na caixa de diálogo Abrir ficheiros, navegue para e carregar o **. pem** certificado que acabou de ser gerado
   - Deixe as restantes como a predefinição e clique em **guardar**

   ![Carregar certificado](./media/how-to-connect-mxchip-iot-devkit/upload-cert.png)

## <a name="start-the-devkit"></a>Iniciar o DevKit

1. Abra o VS Code e o monitor de série.

2. Prima a **repor** botão na sua DevKit.

Consulte o início de DevKit o registo com o serviço de aprovisionamento de dispositivos.

![Saída de código de VS](./media/how-to-connect-mxchip-iot-devkit/vscode-output.png)

## <a name="verify-that-the-devkit-is-registered-with-azure-iot-hub"></a>Certifique-se de que o DevKit está registado no IoT Hub do Azure

Depois do dispositivo arranca, as seguintes ações local:

1. O dispositivo envia um pedido de registo para o seu dispositivo de serviço de fornecimento.
2. O serviço de fornecimento de dispositivo envia um desafio de registo para o qual o dispositivo responde.
3. No registo com êxito, o dispositivo de serviço de fornecimento envia o URI de Hub IoT, ID de dispositivo e a chave encriptada novamente para o dispositivo.
4. A aplicação de cliente do IoT Hub no dispositivo se liga ao seu hub.
5. Em ligação com êxito para o hub, pode ver o dispositivo são apresentadas no Explorador de dispositivos do IoT Hub.
  ![Dispositivos registados](./media/how-to-connect-mxchip-iot-devkit/device-registered.png)

## <a name="change-the-device-id"></a>Altere o ID de dispositivo

O ID de dispositivo predefinido registado no IoT Hub do Azure é *AZ3166*. Se pretender modificar o ID, siga as instruções em [personalizar o ID de dispositivo](https://microsoft.github.io/azure-iot-developer-kit/docs/customize-device-id/).

## <a name="problems-and-feedback"></a>Problemas e comentários

Se tiver problemas, consulte o Iot DevKit [perguntas mais frequentes](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/), ou entre em contacto para as seguintes canais de suporte:

* [Gitter.im](http://gitter.im/Microsoft/azure-iot-developer-kit)
* [Stackoverflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, aprendeu a inscrever um dispositivo de forma segura para o dispositivo de serviço de fornecimento, utilizando o motor de composição de identidade de dispositivo, para que o dispositivo pode ser registado automaticamente no IoT Hub do Azure. 

Em resumo, aprendeu como:

> [!div class="checklist"]
> * Configure o ponto final global do dispositivo aprovisionamento do serviço num dispositivo.
> * Utilize um segredo de dispositivo exclusivo para gerar um certificado x. 509.
> * Inscreva um dispositivo individual.
> * Certifique-se de que o dispositivo está registado.

Saiba como [criar e aprovisionar um dispositivo simulado](./quick-create-simulated-device.md).

