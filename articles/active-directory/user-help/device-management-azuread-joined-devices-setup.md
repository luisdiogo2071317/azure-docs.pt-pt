---
title: Configurar o Azure Active Directory dispositivos associados ao | Documentos da Microsoft
description: Saiba como configurar dispositivos do Azure associados ao Active Directory.
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
ms.openlocfilehash: f2d285735b92c3acd67dc741f344b836e791be04
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39060041"
---
# <a name="set-up-azure-active-directory-joined-devices"></a>Dispositivos associados ao conjunto de cópia de segurança do Azure Active Directory

Gestão de dispositivos no Azure Active Directory (Azure AD), pode certificar-se de que os utilizadores estão a aceder ao seus recursos de dispositivos que cumprem as normas de segurança e conformidade. Para obter mais informações, consulte [introdução à gestão de dispositivos no Azure Active Directory](../device-management-introduction.md).

Se quiser colocar os dispositivos Windows 10 pertencentes no trabalho sob o controle do Azure AD, isso pode ser feito ao configurar dispositivos associados ao Azure AD. Este tópico fornece os passos relacionados. 


## <a name="prerequisites"></a>Pré-requisitos

Para associar um dispositivo Windows 10, o serviço de registo do dispositivo tem de ser configurado para que possa registar dispositivos. Além de ter permissão para associar dispositivos no seu inquilino do Azure AD, tem de ter menos dispositivos registados que o máximo configurado. Para obter mais informações, consulte [configurar definições do dispositivo](../device-management-azure-portal.md#configure-device-settings).



## <a name="what-you-should-know"></a>O que deve saber


- Windows associa o dispositivo no diretório da organização no Azure AD.

- Poderá ser necessário passar pelo desafio de autenticação multifator. Esse desafio é configurável pelo seu administrador de TI.

- O Azure AD verifica se o dispositivo requer a inscrição de gestão de dispositivos móveis e inscreve-lo, se aplicável.

- Se for um utilizador gerido, o Windows leva-o para a área de trabalho por meio do automático início de sessão.

- Se for um utilizador federado, terá de iniciar sessão com as suas credenciais.

- Se forem federadas, o fornecedor de identidade tem de suportar WS-Trust e WS-Fed ponto final de nome de utilizador/palavra-passe. Isso pode ser a versão 1.3 ou 2005. Este suporte de protocolo é necessário para associar o dispositivo ao Azure AD e inicie sessão no dispositivo com uma palavra-passe. 




## <a name="joining-a-device"></a>Ingressar num dispositivo

Esta secção fornece os passos para associar o seu dispositivo Windows 10 com o Azure AD. Um dispositivo associado com êxito ao mostrado como **ligado à \<do Azure AD\>**.

![Ligado](./media/device-management-azuread-joined-devices-setup/13.png)


**Para associar o seu dispositivo Windows 10:**

1. Na **começar** menu, clique em **definições**.

    ![Definições](./media/device-management-azuread-joined-devices-setup/01.png)

2. Clique em **contas**.

    ![Contas](./media/device-management-azuread-joined-devices-setup/02.png)


3. Clique em **acesso profissional ou escolar**.

    ![Acesso profissional ou escolar](./media/device-management-azuread-joined-devices-setup/03.png)

4. Sobre o **acesso profissional ou escolar** caixa de diálogo, clique em **Connect**.

    ![Ligar](./media/device-management-azuread-joined-devices-setup/04.png)


5. Sobre o **configurar uma conta escolar ou profissional** caixa de diálogo, clique em **associar este dispositivo ao Azure Active Directory**.

    ![Ligar](./media/device-management-azuread-joined-devices-setup/08.png)


6. Sobre o **vamos ajudá-lo a entrar** caixa de diálogo, introduza o nome da sua conta (por exemplo, someone@example.com) e, em seguida, clique em **seguinte**.

    ![Vamos ajudá-lo a iniciar sessão](./media/device-management-azuread-joined-devices-setup/10.png)


6. Sobre o **introduzir palavra-passe** caixa de diálogo, introduza a palavra-passe e, em seguida, clique em **iniciar sessão**.

    ![Introduzir palavra-passe](./media/device-management-azuread-joined-devices-setup/05.png)


7. Sobre o **certificar-se de que esta é a sua organização** caixa de diálogo, clique em **associar**.

    ![Certifique-se de que esta é a sua organização](./media/device-management-azuread-joined-devices-setup/11.png)


8. Sobre o **tudo pronto** caixa de diálogo, clique em **feito**.

    ![Está tudo pronto](./media/device-management-azuread-joined-devices-setup/12.png)

## <a name="verification"></a>Verificação

Para verificar se um dispositivo estiver associado a um Azure AD, pode rever o **acesso profissional ou escolar** caixa de diálogo no seu dispositivo.

![Ligado](./media/device-management-azuread-joined-devices-setup/13.png)

Em alternativa, pode executar o seguinte comando: `dsregcmd /status`  
Num dispositivo associado com êxito, **AzureAdJoined** é **Sim**.

![Ligado](./media/device-management-azuread-joined-devices-setup/14.png)

Também pode rever as definições de dispositivos no portal do Azure AD.

![Ligado](./media/device-management-azuread-joined-devices-setup/15.png)

Para obter mais informações, consulte [localizar dispositivos](../device-management-azure-portal.md#locate-devices).


## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações, consulte: 

- O [introdução à gestão de dispositivos no Azure Active Directory](../device-management-introduction.md)
- [Gestão de dispositivos no portal do Azure](../device-management-azure-portal.md)
- 



