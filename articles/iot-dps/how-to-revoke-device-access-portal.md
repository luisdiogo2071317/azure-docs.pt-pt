---
title: Como disenroll um dispositivo a partir do serviço de aprovisionamento de dispositivos do Azure IoT Hub
description: Como disenroll um dispositivo para impedir o aprovisionamento através do serviço de aprovisionamento de dispositivos do Azure IoT Hub
services: iot-dps
keywords: ''
author: bryanla
ms.author: v-jamebr;bryanla
ms.date: 04/05/2018
ms.topic: article
ms.service: iot-dps
documentationcenter: ''
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 50074eaecacf603d2bc6170183fd632b4a1ab2d1
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
# <a name="how-to-disenroll-a-device-from-azure-iot-hub-device-provisioning-service"></a>Como disenroll um dispositivo a partir do serviço de aprovisionamento de dispositivos do Azure IoT Hub

Gestão adequado de credenciais de dispositivo é fundamental para sistemas de perfil de alta como soluções de IoT. Uma melhor prática para esses sistemas é ter um plano limpar como revogar o acesso para dispositivos quando as respetivas credenciais, se um token de assinaturas (SAS) de acesso partilhado ou de um certificado x. 509, podem ficar comprometidas. 

Inscrição no serviço de aprovisionamento de dispositivos permite que um dispositivo ser [aprovisionamento automático](concepts-auto-provisioning.md). Um dispositivo aprovisionado é aquele que foi registado com o IoT Hub, permitindo que recebe a sua inicial [dispositivo duplo](~/articles/iot-hub/iot-hub-devguide-device-twins.md) de estado e começar a dados de telemetria de relatórios. Este artigo descreve como disenroll um dispositivo da sua instância de serviço aprovisionamento, impedir de que está a ser aprovisionado novamente no futuro.

> [!NOTE] 
> Tenha em atenção a política de repetição de dispositivos que se revogar o acesso. Por exemplo, um dispositivo que tem uma política de repetição infinita poderá continuamente tentar registar com o serviço de aprovisionamento. Nessa situação consome recursos do serviço e possivelmente afeta o desempenho.

## <a name="blacklist-devices-by-using-an-individual-enrollment-entry"></a>Dispositivos bloqueados através da utilização de uma entrada de registo individuais

As inscrições individuais aplicam-se a um único dispositivo e podem utilizar certificados x. 509 ou tokens SAS (por um TPM real ou virtual) como o mecanismo de atestado. (Para dispositivos que utilizam os tokens SAS de como os respetivos mecanismo de atestado pode ser aprovisionado apenas através de inscrição individuais.) Na lista de proibições num dispositivo que tenha uma inscrição individuais, pode desativar ou eliminar a entrada de inscrição. 

Para temporariamente lista de proibições o dispositivo, desativando a respetiva entrada de inscrição: 

1. Inicie sessão no Azure portal e selecione **todos os recursos** no menu esquerdo.
2. Na lista de recursos, selecione o serviço de aprovisionamento que pretende que a lista de proibições do seu dispositivo.
3. No seu serviço de aprovisionamento, selecione **gerir inscrições**e, em seguida, selecione o **inscrições individuais** separador.
4. Selecione a entrada de registo para o dispositivo que pretende que a lista de proibições. 
5. Desloque-se na parte inferior e selecione **desativar** no **ativar entrada** mudar e, em seguida, selecione **guardar**.  

   [![Desativar a entrada de registo individuais no portal](./media/how-to-revoke-device-access-portal/disable-individual-enrollment.png)](./media/how-to-revoke-device-access-portal/disable-individual-enrollment.png#lightbox)  

Para permanentemente lista de proibições o dispositivo, eliminando a respetiva entrada de inscrição:

1. Inicie sessão no Azure portal e selecione **todos os recursos** no menu esquerdo.
2. Na lista de recursos, selecione o serviço de aprovisionamento que pretende que a lista de proibições do seu dispositivo.
3. No seu serviço de aprovisionamento, selecione **gerir inscrições**e, em seguida, selecione o **inscrições individuais** separador.
4. Selecione a caixa de verificação junto a entrada de registo para o dispositivo que pretende que a lista de proibições. 
5. Selecione **eliminar** na parte superior da janela e, em seguida, selecione **Sim** para confirmar que pretende remover a inscrição. 

   ![Eliminar a entrada de registo individuais no portal](./media/how-to-revoke-device-access-portal/delete-individual-enrollment.png)
    
Depois de concluir o procedimento, deverá ver a entrada removida da lista de inscrições individuais.  

## <a name="blacklist-an-x509-intermediate-or-root-ca-certificate-by-using-an-enrollment-group"></a>Lista de proibições um x. 509 intermediária ou um certificado de AC de raiz através da utilização de um grupo de inscrição

Certificados x. 509 normalmente são dispostos numa cadeia de certificados de confiança. Se um certificado em qualquer fase numa cadeia ficar comprometido, a confiança é quebrada. O certificado tem de ser blacklisted para impedir que o serviço de aprovisionamento de dispositivos de aprovisionamento dispositivos downstream qualquer cadeia que contém esse certificado. Para obter mais informações sobre certificados x. 509 e como são utilizadas com o serviço de aprovisionamento, consulte [certificados x. 509](./concepts-security.md#x509-certificates). 

Um grupo de inscrição é uma entrada para dispositivos que partilham um mecanismo de atestado comuns de certificados x. 509 assinado por intermédio iguais ou AC de raiz. A entrada de grupo de inscrição está configurada com o certificado x. 509 associado o intermédio ou AC de raiz. A entrada também é configurada com quaisquer valores de configuração, tais como Estado duplo e a ligação do hub IoT, que são partilhados por dispositivos com esse certificado na respetiva cadeia de certificados. Na lista de proibições o certificado, pode desativar ou eliminar o grupo de inscrição.

Para temporariamente lista de proibições o certificado, desativando o grupo de inscrição: 

1. Inicie sessão no Azure portal e selecione **todos os recursos** no menu esquerdo.
2. Na lista de recursos, selecione o serviço de aprovisionamento que pretende que a lista de proibições o certificado de assinatura de.
3. No seu serviço de aprovisionamento, selecione **gerir inscrições**e, em seguida, selecione o **inscrição grupos** separador.
4. Selecione o grupo de inscrição com o certificado que pretende que a lista de proibições.
5. Selecione **desativar** no **ativar entrada** mudar e, em seguida, selecione **guardar**.  

   ![Desativar a entrada de grupo de inscrição no portal](./media/how-to-revoke-device-access-portal/disable-enrollment-group.png)

    
Para permanentemente lista de proibições o certificado ao eliminar o grupo de inscrição:

1. Inicie sessão no Azure portal e selecione **todos os recursos** no menu esquerdo.
2. Na lista de recursos, selecione o serviço de aprovisionamento que pretende que a lista de proibições do seu dispositivo.
3. No seu serviço de aprovisionamento, selecione **gerir inscrições**e, em seguida, selecione o **inscrição grupos** separador.
4. Selecione a caixa de verificação junto ao grupo de inscrição para o certificado que pretende que a lista de proibições. 
5. Selecione **eliminar** na parte superior da janela e, em seguida, selecione **Sim** para confirmar que pretende remover o grupo de inscrição. 

   ![Eliminar a entrada de grupo de inscrição no portal](./media/how-to-revoke-device-access-portal/delete-enrollment-group.png)

Depois de concluir o procedimento, deverá ver a entrada removida da lista de grupos de inscrição.  

> [!NOTE]
> Se eliminar um grupo de inscrição de certificados, os dispositivos que tenham o certificado na respetiva cadeia de certificados ainda poderá inscrever se um grupo de inscrição ativado para o certificado de raiz ou intermediária outra superior a cópia de segurança no respetivo certificado existe a cadeia.

## <a name="blacklist-specific-devices-in-an-enrollment-group"></a>Lista de proibições dispositivos específicos num grupo de inscrição

Dispositivos que implementam o mecanismo de atestado de x. 509 utilizam cadeia de certificados do dispositivo e a chave privada para autenticação. Quando um dispositivo se liga e autentica com o serviço de aprovisionamento de dispositivos, o serviço procura primeiro uma inscrição individuais que corresponda a credenciais do dispositivo. O serviço de pesquisa, em seguida, grupos de inscrição para determinar se o dispositivo pode ser aprovisionado. Se o serviço de encontrar uma inscrição individuais desativada para o dispositivo, impede o dispositivo a ligação. O serviço impede que a ligação, mesmo se existe um grupo de inscrição ativados para um intermédio ou uma AC de raiz na cadeia de certificados do dispositivo. 

Na lista de proibições um dispositivo individual num grupo de inscrição, siga estes passos:

1. Inicie sessão no Azure portal e selecione **todos os recursos** no menu esquerdo.
2. Na lista de recursos, selecione o serviço de aprovisionamento que contém o grupo de inscrição para o dispositivo que pretende que a lista de proibições.
3. No seu serviço de aprovisionamento, selecione **gerir inscrições**e, em seguida, selecione o **inscrições individuais** separador.
4. Selecione o **adicionar** botão na parte superior. 
5. Selecione **x. 509** como o mecanismo de atestado de dispositivo e carregue o certificado de dispositivo. Este é o certificado de entidade final assinado instalado no dispositivo. O dispositivo utiliza-o para gerar certificados para autenticação.
6. Para **ID do dispositivo IoT Hub**, introduza o ID do dispositivo. 
7. Selecione **desativar** no **ativar entrada** mudar e, em seguida, selecione **guardar**. 

    [![Utilize desativada entrada de registo individuais para desativar o dispositivo da inscrição de grupo, no portal](./media/how-to-revoke-device-access-portal/disable-individual-enrollment-in-enrollment-group.png)](./media/how-to-revoke-device-access-portal/disable-individual-enrollment-in-enrollment-group.png#lightbox)

Quando criar com êxito a inscrição, deverá ver o seu dispositivo aparecem no **inscrições individuais** separador.

## <a name="next-steps"></a>Passos Seguintes

Disenrollment também faz parte do processo de desaprovisionamento maior. Desaprovisionamento um dispositivo inclui tanto disenrollment do serviço de aprovisionamento e deregistering a partir do IoT hub. Para saber mais sobre o processo completo, consulte [como desaprovisionar dispositivos que foram auto-aprovisionados anteriormente](how-to-unprovision-devices.md) 

