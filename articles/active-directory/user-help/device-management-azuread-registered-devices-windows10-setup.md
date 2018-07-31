---
title: Configuração de dispositivos do Azure Active Directory registado | Documentos da Microsoft
description: Saiba como configurar dispositivos do Azure Active Directory registado.
services: active-directory
author: eross-msft
manager: mtillman
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.component: user-help
ms.workload: identity
ms.topic: conceptual
ms.date: 01/15/2018
ms.author: lizross
ms.reviewer: jairoc
ms.openlocfilehash: bf763b08f234145cb81d2cded170348f96e5e478
ms.sourcegitcommit: 30fd606162804fe8ceaccbca057a6d3f8c4dd56d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/30/2018
ms.locfileid: "39343346"
---
# <a name="set-up-azure-active-directory-registered-windows-10-devices"></a>Dispositivos Windows 10 registados do conjunto de cópia de segurança do Azure Active Directory

Gestão de dispositivos no Azure Active Directory (Azure AD), pode certificar-se de que os utilizadores estão a aceder ao seus recursos de dispositivos que cumprem as normas de segurança e conformidade. Para obter mais informações, consulte [introdução à gestão de dispositivos no Azure Active Directory](../device-management-introduction.md).

Se pretender ativar a **Bring Your Own Device (BYOD)** cenário, isso pode ser feito através da configuração de dispositivos do Azure AD registado. No Azure AD, pode configurar dispositivos do Azure AD registado para Windows 10, iOS, Android e macOS. Este artigo fornece os passos relacionados para dispositivos Windows 10. 


## <a name="before-you-begin"></a>Antes de começar

Para registar um dispositivo Windows 10, o serviço de registo do dispositivo tem de ser configurado para que possa registar dispositivos. Além disso, tem de ter menos dispositivos registados que o máximo configurado. Para obter mais informações, consulte [configurar definições do dispositivo](../device-management-azure-portal.md#configure-device-settings).

## <a name="what-you-should-know"></a>O que deve saber

Ao registar um dispositivo, deve ter o seguinte em mente:

- Windows regista o dispositivo no diretório da organização no Azure AD

- Poderá ser necessário passar pelo desafio de autenticação multifator. Esse desafio é configurável pelo seu administrador de TI.

- O Azure AD verifica se o dispositivo requer a inscrição de gestão de dispositivos móveis e inscreve-lo, se aplicável.

- Se for um utilizador gerido, o Windows leva-o para a área de trabalho por meio do automático início de sessão.

- Se for um utilizador federado, será direcionado para um ecrã de início de sessão do Windows para introduzir as suas credenciais.


## <a name="registering-a-device"></a>Registar um dispositivo

Esta secção fornece os passos para registar o seu dispositivo Windows 10 com o Azure AD. Um dispositivo registado com êxito, aparece com um **trabalho conta escolar ou profissional** entrada.

![Registar](./media/device-management-azuread-registered-devices-windows10-setup/08.png)


**Para registar o seu dispositivo Windows 10:**

1. Na **começar** menu, clique em **definições**.

    ![Definições](./media/device-management-azuread-registered-devices-windows10-setup/01.png)

2. Clique em **contas**.

    ![Contas](./media/device-management-azuread-registered-devices-windows10-setup/02.png)


3. Clique em **acesso profissional ou escolar**.

    ![Acesso profissional ou escolar](./media/device-management-azuread-registered-devices-windows10-setup/03.png)

4. Sobre o **acesso profissional ou escolar** caixa de diálogo, clique em **Connect**.

    ![Ligar](./media/device-management-azuread-registered-devices-windows10-setup/04.png)


5. Sobre o **configurar uma conta escolar ou profissional** caixa de diálogo, introduza o nome da sua conta (por exemplo, someone@example.com) e, em seguida, clique em **seguinte**.

    ![Ligar](./media/device-management-azuread-registered-devices-windows10-setup/06.png)


6. Sobre o **introduzir palavra-passe** caixa de diálogo, introduza a palavra-passe e, em seguida, clique em **próxima**.

    ![Ligar](./media/device-management-azuread-registered-devices-windows10-setup/05.png)


7. Sobre o **tudo pronto** caixa de diálogo, clique em **feito**.

    ![Ligar](./media/device-management-azuread-registered-devices-windows10-setup/07.png)

## <a name="verification"></a>Verificação

Para verificar se um dispositivo estiver associado a um Azure AD, pode rever o **acesso profissional ou escolar** caixa de diálogo no seu dispositivo.

![Registar](./media/device-management-azuread-registered-devices-windows10-setup/08.png)

Em alternativa, pode rever as definições de dispositivos no portal do Azure AD.

![Registar](./media/device-management-azuread-registered-devices-windows10-setup/09.png)





## <a name="next-steps"></a>Passos Seguintes

- Para obter mais informações, consulte o [introdução à gestão de dispositivos no Azure Active Directory](../device-management-introduction.md)

- Para obter mais informações sobre a gestão de dispositivos no portal do Azure AD, consulte a [gestão de dispositivos no portal do Azure ](../device-management-azure-portal.md).




