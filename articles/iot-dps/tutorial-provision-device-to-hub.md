---
title: Aprovisionar um dispositivo com o Serviço Aprovisionamento de Dispositivos no Hub IoT do Azure | Microsoft Docs
description: Aprovisionar o seu dispositivo num hub IoT individual com o Serviço Aprovisionamento de Dispositivos no Hub IoT do Azure
services: iot-dps
keywords: ''
author: dsk-2015
ms.author: dkshir
ms.date: 04/12/2018
ms.topic: tutorial
ms.service: iot-dps
documentationcenter: ''
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 9f151a8fbcdc20124467a1db290f6a05f574e4fe
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
# <a name="provision-the-device-to-an-iot-hub-using-the-azure-iot-hub-device-provisioning-service"></a>Aprovisionar o dispositivo num hub IoT com o Serviço Aprovisionamento de Dispositivos no Hub IoT do Azure

No tutorial anterior, aprendeu a configurar um dispositivo para se ligar ao Serviço Aprovisionamento de Dispositivos. Neste tutorial, vai aprender a utilizar este serviço para aprovisionar o seu dispositivo num hub IoT individual através de aprovisionamento automático e **_listas de inscrição_**. Este tutorial mostrar-lhe como:

> [!div class="checklist"]
> * Inscrever o dispositivo
> * Iniciar o dispositivo
> * Verificar se o dispositivo está registado

## <a name="prerequisites"></a>Pré-requisitos

Antes de continuar, confirme que configura o seu dispositivo tal como explicado no tutorial [Setup a device to provision using Azure IoT Hub Device Provisioning Service](./tutorial-set-up-device.md) (Configurar um dispositivo para ser aprovisionado com o Serviço Aprovisionamento de Dispositivos no Hub IoT).

Se não estiver familiarizado com o processo de aprovisionamento automático, reveja [Auto-provisioning concepts](concepts-auto-provisioning.md) (Conceitos de aprovisionamento automático) antes de continuar.

<a id="enrolldevice"></a>
## <a name="enroll-the-device"></a>Inscrever o dispositivo

Este passo envolve a adição dos artefactos de segurança exclusiva do dispositivo ao Serviço de Aprovisionamento de Dispositivos. Esses artefactos de segurança baseiam-se no [Mecanismo de atestado](concepts-device.md#attestation-mechanism) do dispositivo da seguinte forma:

- Para dispositivos baseados em TPM, precisa:
    - Da *Chave de Endossamento* que é exclusiva de cada chip ou simulação de TPM, que é obtida junto do fabricante do chip de TPM.  Leia [Understand TPM Endorsement Key](https://technet.microsoft.com/library/cc770443.aspx) (Compreender a Chave de Endossamento de TPM) para obter mais informações.
    - Do *ID de Registo* que é utilizado para identificar exclusivamente um dispositivo no espaço de nomes/âmbito. Este ID pode ou não ser o mesmo que o ID de dispositivo. O ID é obrigatório para todos os dispositivos. Para os dispositivos baseados em TPM, o ID de registo pode derivar do próprio TPM, como, por exemplo, um hash SHA-256 da Chave de Endossamento de TPM.

    [![Informações de inscrição do TPM no portal](./media/tutorial-provision-device-to-hub/tpm-device-enrollment.png)](./media/tutorial-provision-device-to-hub/tpm-device-enrollment.png#lightbox)  

- Para dispositivos baseados em X.509, precisa:
    - Do [certificado emitido para o chip ou simulação de X.509](https://msdn.microsoft.com/library/windows/desktop/bb540819.aspx), sob a forma de ficheiro *.pem* ou *.cer*. Para inscrição individual, tem de utilizar o *certificado de signatário* por dispositivo para o seu sistema X.509, ao passo que, para grupos de inscrição, tem de utilizar o *certificado de arranque*. 

    [![Adicionar inscrição individual para fins de atestado X.509 no portal](./media/tutorial-provision-device-to-hub/individual-enrollment.png)](./media/tutorial-provision-device-to-hub/individual-enrollment.png#lightbox)

Existem duas formas de inscrever o dispositivo no Serviço Aprovisionamento de Dispositivos:

- **Grupos de inscrição** Representam um grupo de dispositivos que partilham um mecanismo de atestado específico. Recomendamos a utilização de um grupo de inscrição para um grande número de dispositivos que partilhem uma configuração inicial pretendida ou para dispositivos que pertencerão todos ao mesmo inquilino.

    [![Adicionar inscrição de grupo para fins de atestado X.509 no portal](./media/tutorial-provision-device-to-hub/group-enrollment.png)](./media/tutorial-provision-device-to-hub/group-enrollment.png#lightbox)

- **Inscrições Individuais** representam uma entrada para um único dispositivo que pode ser registado com o Serviço Aprovisionamento de Dispositivos. As inscrições individuais podem utilizar certificados x509 ou tokens SAS (num TPM real ou virtual) como mecanismos de atestação. Recomendamos a utilização das inscrições individuais para os dispositivos que precisam de configurações iniciais exclusivas e para os dispositivos que só podem utilizar tokens SAS através de TPM ou de TPM virtual como o mecanismo de atestação. As inscrições individuais podem ter o ID de dispositivo do hub IoT pretendido especificado.

Agora, vai inscrever o dispositivo no Serviço Aprovisionamento de Dispositivos com os artefactos de segurança necessários com base no mecanismo de atestação daquele. 

1. Inicie sessão no portal do Azure, clique no botão **Todos os recursos**, no menu do lado esquerdo, e abra o Serviço Aprovisionamento de Dispositivos.

2. No painel de resumo do Serviço Aprovisionamento de Dispositivos, selecione **Gerir inscrições**. Selecione o separador **Inscrições Individuais** ou o separador **Grupos de Inscrição** de acordo com a configuração do dispositivo. Clique no botão **+ Adicionar**, na parte superior. Selecione **TPM** ou **X.509** como o *Mecanismo* de atestação de identidade e introduza os artefactos de segurança adequados, conforme discutido anteriormente. Pode introduzir um **ID de dispositivo Hub IoT** novo. Quando tiver terminado, clique no botão **Guardar**. 

3. Quando o dispositivo for inscrito com êxito, deverá ver que é apresentado no portal do seguinte modo:

    ![Inscrição de TPM bem-sucedida no portal](./media/tutorial-provision-device-to-hub/tpm-enrollment-success.png)

Após a inscrição, o Serviço Aprovisionamento aguarda que o dispositivo seja arrancado e se ligue ao mesmo num momento posterior. Da primeira vez que o dispositivo for arrancado, a biblioteca do SDK de clientes interage com o seu chip para extrair os artefactos de segurança do dispositivo e verifica o registo no Serviço Aprovisionamento de Dispositivos. 

## <a name="start-the-device"></a>Iniciar o dispositivo

Neste momento, a configuração seguinte está pronta para o registo do dispositivo:

1. O dispositivo ou grupo de dispositivos são inscritos no Serviço Aprovisionamento de Dispositivos e 
2. O dispositivo está pronto com o mecanismo de atestação configurado e acessível através da aplicação mediante a utilização do SDK de cliente do Serviço Aprovisionamento de Dispositivos.

Inicie o dispositivo para permitir que a aplicação cliente iniciar o registo no Serviço Aprovisionamento de Dispositivos.  

## <a name="verify-the-device-is-registered"></a>Verificar se o dispositivo está registado

Após o arranque do dispositivo, devem ocorrer as ações seguintes. Veja a aplicação de exemplo do simulador de TPM [dps_client_sample](https://github.com/Azure/azure-iot-device-auth/blob/master/dps_client/samples/dps_client_sample/dps_client_sample.c) para obter mais detalhes. 

1. O dispositivo envia um pedido de registo ao Serviço Aprovisionamento de Dispositivos.
2. Relativamente aos dispositivos TPM, o Serviço Aprovisionamento de Dispositivos devolve um desafio de registo, ao qual o seu dispositivo vai responder. 
3. Após o registo bem-sucedido, o Serviço Aprovisionamento de Dispositivos reenvia o URI do hub IoT, o ID do dispositivo e a chave de encriptação para o dispositivo. 
4. Em seguida, a aplicação cliente do Hub IoT no dispositivo liga-se ao seu hub. 
5. Quando a ligação ao hub for estabelecida com êxito, deverá ver o dispositivo apresentado no **Device Explorer**do hub IoT. 

    ![Ligação com êxito ao hub no portal](./media/tutorial-provision-device-to-hub/hub-connect-success.png)

## <a name="next-steps"></a>Passos seguintes
Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Inscrever o dispositivo
> * Iniciar o dispositivo
> * Verificar se o dispositivo está registado

Avance para o próximo tutorial para saber como aprovisionar vários dispositivos em hubs com balanceamento de carga. 

> [!div class="nextstepaction"]
> [Aprovisionar dispositivos em vários hubs IoT com balanceamento de carga](./tutorial-provision-multiple-hubs.md)
