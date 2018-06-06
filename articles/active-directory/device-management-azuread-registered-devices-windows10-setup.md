---
title: Configurar dispositivos do Azure Active Directory registado | Microsoft Docs
description: Saiba como configurar dispositivos do Azure Active Directory registado.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.component: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 541f37bb11c604567b356716db282d0e4e2f629b
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34713529"
---
# <a name="set-up-azure-active-directory-registered-windows-10-devices"></a>Configurar o Azure Active Directory registado dispositivos Windows 10

Gestão de dispositivos no Azure Active Directory (Azure AD), pode certificar-se de que os utilizadores acedem aos seus recursos dos dispositivos que cumprem as normas de segurança e conformidade. Para obter mais informações, consulte [introdução à gestão de dispositivos no Azure Active Directory](device-management-introduction.md).

Se pretender ativar o **Bring Your Own Device (BYOD)** cenário, isto é possível através da configuração de dispositivos do Azure AD registado. No Azure AD, pode configurar dispositivos do Azure AD registado para o Windows 10, iOS, Android e macOS. Este artigo fornece os passos relacionados para dispositivos Windows 10. 


## <a name="before-you-begin"></a>Antes de começar

Para registar um dispositivo Windows 10, o serviço de registo do dispositivo tem de ser configurado para ativar o registo de dispositivos. Além disso, tem de ter menos dispositivos registados que o máximo configurado. Para obter mais informações, consulte [configurar definições do dispositivo](device-management-azure-portal.md#configure-device-settings).

## <a name="what-you-should-know"></a>O que deve conhecer

Ao registar um dispositivo, deve tenha em atenção o seguinte:

- Windows regista o dispositivo no diretório da organização no Azure AD

- Poderá ser necessário passar por desafio de autenticação multifator. Este desafio é configurável pelo seu administrador de TI.

- Azure AD verifica se o dispositivo requer a inscrição de gestão de dispositivos móveis e inscreve-la, se aplicável.

- Se for um utilizador gerido, o Windows leva-o no ambiente de trabalho através do automático início de sessão.

- Se um utilizador federado, será direcionado para um ecrã de início de sessão do Windows para introduzir as suas credenciais.


## <a name="registering-a-device"></a>Registar um dispositivo

Esta secção fornece os passos para registar o seu dispositivo Windows 10 para o seu Azure AD. Um dispositivo registado com êxito aparece com um **trabalho conta escolar ou profissional** entrada.

![Registar](./media/device-management-azuread-registered-devices-windows10-setup/08.png)


**Para registar o seu dispositivo Windows 10:**

1. No **iniciar** menu, clique em **definições**.

    ![Definições](./media/device-management-azuread-registered-devices-windows10-setup/01.png)

2. Clique em **contas**.

    ![Contas](./media/device-management-azuread-registered-devices-windows10-setup/02.png)


3. Clique em **acesso ou escola**.

    ![Acesso ou escola](./media/device-management-azuread-registered-devices-windows10-setup/03.png)

4. No **acesso ou escola** caixa de diálogo, clique em **Connect**.

    ![Ligar](./media/device-management-azuread-registered-devices-windows10-setup/04.png)


5. No **configurar uma conta escolar ou profissional** caixa de diálogo, introduza o nome da sua conta (por exemplo, someone@example.com) e, em seguida, clique em **seguinte**.

    ![Ligar](./media/device-management-azuread-registered-devices-windows10-setup/06.png)


6. No **palavra-passe de Enter** caixa de diálogo, introduza a palavra-passe e, em seguida, clique em **seguinte**.

    ![Ligar](./media/device-management-azuread-registered-devices-windows10-setup/05.png)


7. No **está tudo pronto** caixa de diálogo, clique em **feito**.

    ![Ligar](./media/device-management-azuread-registered-devices-windows10-setup/07.png)

## <a name="verification"></a>Verificação

Para verificar se um dispositivo é associado a um Azure AD, pode rever o **acesso ou escola** diálogo no seu dispositivo.

![Registar](./media/device-management-azuread-registered-devices-windows10-setup/08.png)

Em alternativa, pode rever as definições de dispositivo no portal do Azure AD.

![Registar](./media/device-management-azuread-registered-devices-windows10-setup/09.png)





## <a name="next-steps"></a>Passos Seguintes

- Para obter mais informações, consulte o [introdução à gestão de dispositivos no Azure Active Directory](device-management-introduction.md)

- Para obter mais informações sobre a gestão de dispositivos no portal do Azure AD, consulte o [gerir dispositivos através do portal do Azure ](device-management-azure-portal.md).




