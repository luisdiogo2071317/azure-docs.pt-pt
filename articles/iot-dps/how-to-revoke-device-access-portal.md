---
title: "Como gerir o acesso ao dispositivo para o serviço de aprovisionamento de dispositivos do Azure IoT Hub | Microsoft Docs"
description: "Como revogar o acesso de dispositivo ao seu serviço DPS no Portal do Azure"
services: iot-dps
keywords: 
author: JimacoMS
ms.author: v-jamebr
ms.date: 12/22/2017
ms.topic: article
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 6f1dad648b228163219c8f722eed3897f4ba4d22
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/12/2018
---
# <a name="how-to-revoke-device-access-to-your-provisioning-service-in-the-azure-portal"></a>Como revogar o acesso de dispositivo para o serviço de aprovisionamento no Portal do Azure

Gestão adequado de credenciais de dispositivo é fundamental para sistemas de perfil de alta como soluções de IoT. Uma melhor prática para esses sistemas é ter um plano limpar como revogar o acesso para dispositivos em casos em que as respetivas credenciais, se um token SAS ou um certificado x. 509, podem ficar comprometidas. Este artigo descreve como revogar o acesso de dispositivo, o passo de aprovisionamento.

Para saber mais sobre revogar o acesso de dispositivo a um IoT hub depois do dispositivo tiver sido aprovisionado. consulte [desativar dispositivos](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-identity-registry#disable-devices).

> [!NOTE] 
> Tenha em atenção a política de repetição de dispositivos que se revogar o acesso. Por exemplo, um dispositivo com uma política de repetição infinita pode continuamente tentar registar com o serviço de aprovisionamento, consumir recursos de serviço e, possivelmente, afetar o desempenho.

## <a name="blacklist-devices-with-an-individual-enrollment-entry"></a>Dispositivos bloqueados com uma entrada de registo individuais

As inscrições individuais aplicam-se a um único dispositivo e podem utilizar certificados x. 509 ou tokens SAS (por um TPM real ou virtual) como o mecanismo de atestado. (Para dispositivos que utilizam os tokens SAS de como os respetivos mecanismo de atestado só pode ser aprovisionado através de inscrição individuais.) Na lista de proibições num dispositivo que tenha uma inscrição individuais, pode desativar ou eliminar a entrada de inscrição: 

- Na lista de proibições temporariamente o dispositivo, pode desativar a respetiva entrada de inscrição. 

    1. Inicie sessão no portal do Azure e clique em **todos os recursos** no menu esquerdo.
    2. Clique no serviço de aprovisionamento que pretende criar uma lista de proibições do seu dispositivo na lista de recursos.
    3. No seu serviço de aprovisionamento, clique em **gerir inscrições**, em seguida, selecione **inscrições individuais** separador.
    4. Clique na entrada de registo para o dispositivo que pretende criar uma lista de proibições para abri-lo. 
    5. Clique em **desativar** na parte inferior da entrada de lista de inscrição, em seguida, clique em **guardar**.  

        ![Desativar a entrada de registo individuais no portal](./media/how-to-revoke-device-access-portal/disable-individual-enrollment.png)
    
- Na lista de proibições permanentemente o dispositivo, pode eliminar a entrada de inscrição.

    1. Inicie sessão no portal do Azure e clique em **todos os recursos** no menu esquerdo.
    2. Clique no serviço de aprovisionamento que pretende criar uma lista de proibições do seu dispositivo na lista de recursos.
    3. No seu serviço de aprovisionamento, clique em **gerir inscrições**, em seguida, selecione **inscrições individuais** separador.
    4. Selecione a caixa junto a entrada de registo para o dispositivo que pretende criar uma lista de proibições. 
    5. Clique em **eliminar** na parte superior da janela, em seguida, clique em **Sim** para confirmar que pretende remover a inscrição. 

        ![Eliminar a entrada de registo individuais no portal](./media/how-to-revoke-device-access-portal/delete-individual-enrollment.png)
    
    6. Assim que a ação for concluída, verá a entrada removida da lista de inscrições individuais.  

## <a name="blacklist-an-x509-intermediate-or-root-ca-certificate-using-an-enrollment-group"></a>Lista de proibições um x. 509 intermédio ou utilizar um grupo de inscrição de certificado de AC de raiz

Certificados x. 509 normalmente são dispostos numa cadeia de certificados de confiança. Se um certificado em qualquer fase numa cadeia ficar comprometido, a confiança é quebrada e o certificado tem de ser blacklisted para impedir que os dispositivos para baixo de fluxo em qualquer cadeia que contém o certificado de aprovisionamento, o serviço de aprovisionamento de dispositivos. Para obter mais informações sobre certificados x. 509 e como são utilizadas com o serviço de aprovisionamento, consulte [certificados x. 509](./concepts-security.md#x509-certificates). 

Um grupo de inscrição é uma entrada para dispositivos que partilham um mecanismo de atestado comuns de certificados x. 509 assinado por intermédio iguais ou AC de raiz. A entrada de grupo de inscrição está configurada com o certificado x. 509 associado o intermédio ou AC de raiz, bem como quaisquer valores de configuração, tais como Estado duplo e a ligação do hub IoT, que são partilhados por dispositivos com esse certificado no respetivo certificado cadeia. Na lista de proibições o certificado, pode desativar ou eliminar o grupo de inscrição:

- Na lista de proibições temporariamente o certificado, pode desativar o respetivo grupo de inscrição. 

    1. Inicie sessão no portal do Azure e clique em **todos os recursos** no menu esquerdo.
    2. Clique no serviço de aprovisionamento que pretende criar uma lista de proibições o certificado de assinatura na lista de recursos.
    3. No seu serviço de aprovisionamento, clique em **gerir inscrições**, em seguida, selecione **inscrição grupos** separador.
    4. Clique no grupo de inscrição do certificado que pretende que a lista de proibições para abri-lo.
    5. Clique em **Editar grupo** à parte superior esquerda da entrada de grupo de inscrição.
    6. Para desativar a entrada de registo, selecione **desativar** no **ativar entrada** mudar, em seguida, clique em **guardar**.  

        ![Desativar a entrada de grupo de inscrição no portal](./media/how-to-revoke-device-access-portal/disable-enrollment-group.png)

    
- Na lista de proibições permanentemente o certificado, pode eliminar o grupo de inscrição.

    1. Inicie sessão no portal do Azure e clique em **todos os recursos** no menu esquerdo.
    2. Clique no serviço de aprovisionamento que pretende criar uma lista de proibições do seu dispositivo na lista de recursos.
    3. No seu serviço de aprovisionamento, clique em **gerir inscrições**, em seguida, selecione **inscrição grupos** separador.
    4. Selecione a caixa junto ao certificado que pretende que a lista de proibições o grupo de inscrição. 
    5. Clique em **eliminar** na parte superior da janela, em seguida, clique em **Sim** para confirmar que pretende remover o grupo de inscrição. 

        ![Eliminar a entrada de grupo de inscrição no portal](./media/how-to-revoke-device-access-portal/delete-enrollment-group.png)

    6. Assim que a ação for concluída, verá a entrada removida da lista de grupos de inscrição.  

> [!NOTE]
> Se eliminar um grupo de inscrição de certificados, os dispositivos que tenham esse certificado na respetiva cadeia de certificados ainda poderá inscrever se um grupo de inscrição ativado para o certificado de raiz ou intermediária outra superior a cópia de segurança no respetivo certificado existe a cadeia.

## <a name="blacklist-specific-devices-in-an-enrollment-group"></a>Lista de proibições dispositivos específicos num grupo de inscrição

Dispositivos que implementam o mecanismo de atestado de x. 509 utilizam cadeia de certificados do dispositivo e a chave privada para autenticação. Quando um dispositivo se liga e autentica com o serviço de aprovisionamento de dispositivos, o serviço procura primeiro uma inscrição individuais correspondente credenciais do dispositivo antes de procurar grupos de inscrição para determinar se o dispositivo pode ser aprovisionado. Se o serviço de encontrar uma inscrição individuais desativada para o dispositivo,-impede o dispositivo de ligação, mesmo se existe um grupo de inscrição ativados para um intermédio ou uma AC de raiz na cadeia de certificados do dispositivo. Na lista de proibições um dispositivo individual num grupo de inscrição, siga estes passos:

1. Inicie sessão no portal do Azure e clique em **todos os recursos** no menu da esquerda.
2. Na lista de recursos, clique no serviço de aprovisionamento que contém o grupo de inscrição para o dispositivo que pretende criar uma lista de proibições.
3. No seu serviço de aprovisionamento, clique em **gerir inscrições**, em seguida, selecione **inscrições individuais** separador.
4. Clique em de **adicionar** botão na parte superior. 
5. Selecione **x. 509** como o mecanismo de segurança para o dispositivo e carregar o certificado do dispositivo. Este é o certificado de entidade final assinado instalado no dispositivo, que utiliza para gerar certificados para autenticação.
6. Introduza o **ID do dispositivo IoT Hub** para o dispositivo. 
7. Para desativar a entrada de registo, selecione **desativar** no **ativar entrada** mudar. 
8. Clique em **Guardar**. Criação com êxito a inscrição, deverá ver o seu dispositivo são apresentados no **inscrições individuais** separador. 

    ![Desativar a entrada de registo individuais no portal](./media/how-to-revoke-device-access-portal/disable-individual-enrollment.png)




