---
title: "Como utilizar MXChip IoT DevKit para ligar ao serviço de aprovisionamento de dispositivos do Azure IoT Hub | Microsoft Docs"
description: "Como utilizar MXChip IoT DevKit para ligar ao serviço de aprovisionamento de dispositivos do Azure IoT Hub"
services: iot-dps
keywords: 
author: liydu
ms.author: liydu
ms.date: 02/20/2018
ms.topic: article
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 77c8a6a5e384d27dca2582cc0fedc2bd23c0592e
ms.sourcegitcommit: 83ea7c4e12fc47b83978a1e9391f8bb808b41f97
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/28/2018
---
# <a name="connect-mxchip-iot-devkit-to-azure-iot-hub-device-provisioning-service"></a>Ligar MXChip IoT DevKit ao serviço de aprovisionamento de dispositivos de IoT Hub do Azure

Este artigo descreve como configurar DevKit para torná-lo a registar automaticamente a utilização do serviço de aprovisionamento de dispositivos do IoT Hub. Neste tutorial, vai aprender a:

* Configurar o ponto final global dos DPS no dispositivo
* Utilizar o segredo de dispositivo exclusivo (UDS) para gerar o certificado x. 509
* Inscrever dispositivos individuais
* Certifique-se de que o dispositivo está registado

O [MXChip IoT DevKit](https://aka.ms/iot-devkit) é uma placa de compatível de Arduino tudo-em-um com periféricos avançados e sensores. Pode desenvolver para o mesmo utilizando [extensão do Visual Studio Code para Arduino](https://aka.ms/arduino). E é fornecido com um crescer [catálogo projetos](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/) para ajudá-lo soluções de Internet das coisas (IoT) de protótipo tirar partido dos serviços do Microsoft Azure.

## <a name="before-you-begin"></a>Antes de começar

Para concluir os passos neste tutorial, precisa do seguinte:

* Preparar o seu DevKit com [guia de introdução](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started).
* Atualização do firmware mais recente (> = 1.3.0) com [atualizar o Firmware](https://microsoft.github.io/azure-iot-developer-kit/docs/firmware-upgrading/) tutorial.
* Criar e ligar o IoT Hub com a instância de serviço de aprovisionamento de dispositivos com [configurar aprovisionamento automático](https://docs.microsoft.com/en-us/azure/iot-dps/quick-setup-auto-provision).

## <a name="set-up-the-device-provisioning-service-configuration-on-the-device"></a>Definir a configuração do serviço de aprovisionamento de dispositivos no dispositivo

Para ativar o DevKit ligar à instância do serviço de aprovisionamento de dispositivos que criou:

1. No portal do Azure, selecione o **descrição geral** do serviço de aprovisionamento de dispositivos e tome nota do **ponto final do dispositivo Global** e **ID âmbito** valor.
  ![DPS Global Endpoint e o âmbito de ID](./media/how-to-connect-mxchip-iot-devkit/dps-global-endpoint.png)

2. Verifique se `git` está instalado no computador e que é adicionado às variáveis de ambiente às quais a janela de comandos pode aceder. Consulte [ferramentas de cliente de Git do Software liberdade Conservancy](https://git-scm.com/download/) ter a versão mais recente instalada.

3. Abra uma linha de comandos. Clone o repositório do GitHub para o código de exemplo DPS:
  ```bash
  git clone https://github.com/DevKitExamples/DevKitDPS.git
  ```

4. Iniciar o VS Code e ligar DevKit ao computador, abra a pasta que contém o código clonou.

5. Abra **DevKitDPS.ino**, localizar e substituir `[Global Device Endpoint]` e `[ID Scope]` com os valores que apenas tenha em atenção para baixo.
  ![Ponto final de DPS](./media/how-to-connect-mxchip-iot-devkit/endpoint.png) pode deixar o **registrationId** como em branco, a aplicação gera um para baseada na versão de firmware e do endereço de MAC. Se pretender personalizá-lo, o ID de registo tem de utilizar alfanuméricos, minúsculas e combinações apenas com o máximo de 128 carateres de hífen longa. Para obter mais informações, consulte [gerir inscrições de dispositivos com o portal do Azure](https://docs.microsoft.com/en-us/azure/iot-dps/how-to-manage-enrollments).

6. Utilize **rápida abra** no VS Code (Windows: `Ctrl+P`, macOS: `Cmd+P`) e o tipo **tarefas de carregamento de dispositivo** para criar e carregar o código para o DevKit.

7. Observe o êxito da tarefa na janela de saída.

## <a name="save-unique-device-secret-on-stsafe-security-chip"></a>Guardar o segredo de dispositivo exclusivo num chip de segurança STSAFE

Serviço de aprovisionamento de dispositivos podem ser configurado no dispositivo com base no respetivo [módulo de segurança de Hardware (HSM)](https://azure.microsoft.com/en-us/blog/azure-iot-supports-new-security-hardware-to-strengthen-iot-security/). Utiliza DevKit [motor de composição de identidade de dispositivo (REPARTIR)](https://trustedcomputinggroup.org/wp-content/uploads/Foundational-Trust-for-IOT-and-Resource-Constrained-Devices.pdf) do [fidedigna informática grupo (TCG)](https://trustedcomputinggroup.org). A **segredo de dispositivo exclusivo (UDS)** guardado em segurança STSAFE chip no DevKit é utilizado para gerar o dispositivo exclusivo [x. 509](https://docs.microsoft.com/en-us/azure/iot-dps/tutorial-set-up-device#select-a-hardware-security-module) certificado. O certificado pode ser utilizado mais tarde para o processo de inscrição no serviço de aprovisionamento de dispositivos.

Típica **segredo de dispositivo exclusivo (UDS)** é uma cadeia de comprimento de 64 carateres. Um exemplo é UDS como abaixo:

```
19e25a259d0c2be03a02d416c05c48ccd0cc7d1743458aae1cb488b074993eae
```

Cada um dos dois carateres é utilizada como valor hexadecimal o cálculo de segurança. Por isso é resolvido para o exemplo acima UDS: "`0x19`, `0xe2`, `0x5a`, `0x25`, `0x9d`, `0x0c`, `0x2b`, `0xe0`, `0x3a`, `0x02`, `0xd4`, `0x16` , `0xc0`, `0x5c`, `0x48`, `0xcc`, `0xd0`, `0xcc`, `0x7d`, `0x17`, `0x43`, `0x45`, `0x8a`, `0xae`, `0x1c`, `0xb4`, `0x88`, `0xb0`, `0x74`, `0x99`, `0x3e`, `0xae`".

Para guardar o segredo de dispositivo exclusivo no DevKit:

1. Abra o monitor de série, utilizando a ferramenta como o Putty, consulte [utilizar o modo de configuração](https://microsoft.github.io/azure-iot-developer-kit/docs/use-configuration-mode/) para obter mais detalhes.

2. Com o DevKit ligada ao computador, mantenha premida botão A, em seguida, push e versão no botão de reposição de introduzir o modo de configuração. O ecrã deve mostrar o ID de DevKit e **'Configuração'**.

3. Colocar a cadeia UDS longo exemplo acima e altere os carateres de um ou muitos para outros valores entre `0` e `f`. Isto é utilizado como o seu próprio UDS.

4. Na janela monitor de série, escreva **set_dps_uds [your_own_uds_value]** e prima a tecla Enter para guardá-lo.
  > [!NOTE]
  > Por exemplo, se definir os seus próprios UDS alterando os dois últimos carateres para `f`, tem de introduzir o comando como **set_dps_uds 19e25a259d0c2be03a02d416c05c48ccd0cc7d1743458aae1cb488b074993eff**.

5. Sem fechar a janela de monitor de série, prima botão de reposição no DevKit.

6. Tome nota **endereço de MAC DevKit** e **versão de Firmware DevKit** valor.
  ![Versão de firmware](./media/how-to-connect-mxchip-iot-devkit/firmware-version.png)

## <a name="generate-x509-certificate"></a>Gerar certificado x. 509

### <a name="windows"></a>Windows

1. Explorador de ficheiros abertos e vá para a pasta contém o DSP código de exemplo clonado, existe um **.build** pasta, localizar e copiar **DPS.ino.bin** e **DPS.ino.map** no mesmo.
  ![Ficheiros gerados](./media/how-to-connect-mxchip-iot-devkit/generated-files.png)
  > [!NOTE]
  > Se tiver alterado o `built.path` configuração Arduino para outra pasta. Tem de localizar os ficheiros na pasta que configurou.

2. Colar estes dois ficheiros para **ferramentas** pasta no mesmo nível com **.build** pasta.

3. Executar **dps_cert_gen.exe**, siga as indicações para introduzir o **UDS**, **endereço MAC** para o DevKit e **versão de firmware** para gerar o certificado x. 509.
  ![Executar dps-certificados-gen.exe](./media/how-to-connect-mxchip-iot-devkit/dps-cert-gen.png)

4. Observar o êxito da geração, um **. pem** certificado é guardado na mesma pasta.

## <a name="create-a-device-enrollment-entry-in-the-device-provisioning-service"></a>Criar uma entrada de inscrição de dispositivos no Serviço de Aprovisionamento de Dispositivos

1. No portal do Azure, navegue até ao seu serviço de aprovisionamento. Clique em **Gerir inscrições**e selecione o separador **Inscrições Individuais**. ![Inscrições individuais](./media/how-to-connect-mxchip-iot-devkit/individual-enrollments.png)

2. Clique em **Adicionar**.

3. No **mecanismo**, escolha **x. 509**.
  ![Carregar o certificado](./media/how-to-connect-mxchip-iot-devkit/upload-cert.png)

4. No **ficheiro. pem ou. cer do certificado**, carregue o **. pem** certificado tiver apenas.

5. Deixe as restantes como a predefinição e clique em **guardar**.

## <a name="start-the-devkit"></a>Iniciar o DevKit

1. Iniciar o VS Code e abra o monitor de série.

2. Prima a **repor** botão na sua DevKit.

Deverá ver o DevKit iniciar o registo com o serviço de aprovisionamento de dispositivos.

![Saída de código de VS](./media/how-to-connect-mxchip-iot-devkit/vscode-output.png)

## <a name="verify-the-devkit-is-registered-on-iot-hub"></a>Certifique-se de que o DevKit está registado no IoT Hub

Uma vez arranques seu dispositivo, as seguintes ações que devem ter lugar:

1. O dispositivo envia um pedido de registo para o serviço de aprovisionamento de dispositivos.
2. O serviço de aprovisionamento de dispositivos envia um desafio de registo para o qual o dispositivo responde.
3. No registo com êxito, o serviço de aprovisionamento de dispositivos envia que o URI do IoT hub, ID de dispositivo e a chave encriptada de volta para o dispositivo.
4. A IoT Hub aplicação cliente no dispositivo, em seguida, liga ao seu hub.
5. Em ligação com êxito para o hub, deverá ver o dispositivo são apresentadas no Explorador de dispositivos do hub IoT.
  ![Dispositivos registados](./media/how-to-connect-mxchip-iot-devkit/device-registered.png)

## <a name="change-device-id"></a>ID de dispositivo de alteração

O ID de dispositivo predefinido registado no IoT Hub do Azure é **AZ3166**. Se pretender modificá-lo, siga [instruções aqui](https://microsoft.github.io/azure-iot-developer-kit/docs/customize-device-id/).

## <a name="problems-and-feedback"></a>Problemas e comentários

Se tiver problemas, consulte [perguntas mais frequentes](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) ou entrar-nos de que as seguintes canais:

* [Gitter.im](http://gitter.im/Microsoft/azure-iot-developer-kit)
* [Stackoverflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Passos Seguintes

Agora que aprendeu preparar DevKit para inscrever um dispositivo de forma segura para DPS utilizando REPARTIR, para que pode registar automaticamente ao IoT Hub com zero touch. Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Configurar o ponto final global dos DPS no dispositivo
> * Utilizar o segredo de dispositivo exclusivo (UDS) para gerar o certificado x. 509
> * Inscrever dispositivos individuais
> * Certifique-se de que o dispositivo está registado

Avançar para os outros tutoriais para saber mais:

> [!div class="nextstepaction"]
> [Criar e aprovisionar um dispositivo simulado](./quick-create-simulated-device.md)

