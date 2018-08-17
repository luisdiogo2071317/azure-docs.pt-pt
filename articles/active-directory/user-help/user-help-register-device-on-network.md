---
title: Registar o seu dispositivo pessoal na rede da sua organização - Azure Active Directory | Documentos da Microsoft
description: Saiba como registar o seu dispositivo pessoal na rede da sua organização para que possa aceder a recursos protegidos da sua organização.
services: active-directory
author: eross-msft
manager: mtillman
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.component: user-help
ms.workload: identity
ms.topic: conceptual
ms.date: 08/03/2018
ms.author: lizross
ms.reviewer: jairoc
ms.openlocfilehash: 7126a47bd90168c7d86fe9fcc05fab0a60955063
ms.sourcegitcommit: 1aedb52f221fb2a6e7ad0b0930b4c74db354a569
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/17/2018
ms.locfileid: "40180928"
---
# <a name="register-your-personal-device-on-your-organizations-network"></a>Registar o seu dispositivo pessoal na rede da sua organização
Registe o seu dispositivo pessoal, normalmente, um telefone ou tablet, na rede da sua organização. Depois do dispositivo estiver registado, será capaz de acessar recursos restritos da sua organização.

>[!Note]
>Este artigo utiliza um dispositivo Windows para fins de demonstração, mas também pode registar dispositivos com iOS, Android ou macOS.

## <a name="what-happens-when-you-register-your-device"></a>O que acontece quando registar o seu dispositivo
Enquanto estiver a registar o dispositivo na rede da sua organização, irão ocorrer as seguintes ações:

- Windows regista o dispositivo na rede da sua organização.

- Opcionalmente, com base nas opções da sua organização, poderá ser-lhe pedido para configurar a verificação de dois passos através de um [multi-factor Authentication](multi-factor-authentication-end-user-first-time.md) ou [informações de segurança](user-help-security-info-overview.md).

- Opcionalmente, com base nas opções da sua organização, poderá ser automaticamente inscritos na gestão de dispositivos móveis, como o Microsoft Intune. Para mais informações sobre como inscrever-se no Microsoft Intune, veja [inscrever o seu dispositivo no Intune](https://docs.microsoft.com/intune-user-help/enroll-your-device-in-intune-all).

- Vai passar pelo processo de início de sessão, com o nome de utilizador e palavra-passe para a sua conta Microsoft pessoal.

## <a name="to-register-your-windows-device"></a>Para registar o seu dispositivo Windows

Siga estes passos para registar o seu dispositivo pessoal na sua rede.

1. Open **configurações**e, em seguida, selecione **contas**.

    ![Contas no ecrã de definições](./media/user-help-register-device-on-network/register-device-settings-accounts.png)

2. Selecione **E-Mail e contas**e, em seguida, selecione **associar uma conta Microsoft**.

    ![Ligações de conta de e-mail e contas e adicionar um Microsoft](./media/user-help-register-device-on-network/register-device-email-and-accounts.png)

3. Sobre o **adicionar a sua conta Microsoft** ecrã, escreva o endereço de e-mail da sua conta Microsoft pessoal.

    ![Adicionar o seu ecrã de conta Microsoft, com o e-mail](./media/user-help-register-device-on-network/register-device-add-accounts.png)

4. Sobre o **introduzir palavra-passe** ecrã, escreva a palavra-passe para a sua conta Microsoft pessoal e, em seguida, selecione **iniciar sessão**.

    ![Introduza o ecrã de palavra-passe](./media/user-help-register-device-on-network/register-device-enter-password.png)

5. Conclua o processo de registo, incluindo aprovar o pedido de verificação de identidade (se usar a verificação de dois passos) e configurar o Windows Hello (se necessário).

## <a name="to-make-sure-youre-registered"></a>Para garantir que é registrado
Pode tornar-se de que é registrado ao observar as suas definições.

1. Open **configurações**e, em seguida, selecione **contas**.

    ![Contas no ecrã de definições](./media/user-help-register-device-on-network/register-device-settings-accounts.png)

2. Selecione **E-Mail e contas de**e certifique-se de que consulte a sua conta Microsoft pessoal.

    ![Aceder ao ecrã escolar ou profissional com a conta de contoso ligados](./media/user-help-register-device-on-network/register-device-verify-account.png)

## <a name="next-steps"></a>Passos Seguintes
Depois de registar o seu dispositivo pessoal à rede da sua organização, deverá conseguir aceder à maioria dos seus recursos.

- Se a organização pretende a associar o seu dispositivo de trabalho, consulte [associar o seu dispositivo de trabalho para a rede da sua organização](user-help-join-device-on-network.md).



