---
title: Junte-se a um novo dispositivo Windows 10 com o Azure AD durante uma primeira | Documentos da Microsoft
description: Um tópico que explica como os utilizadores podem configurar a associação do Azure AD durante a primeira experiência de execução.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 06a149f7-4aa1-4fb9-a8ec-ac2633b031fb
ms.service: active-directory
ms.component: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 1376f011d056aac33333f6ac31ee2eaadaf3ef4a
ms.sourcegitcommit: 96f498de91984321614f09d796ca88887c4bd2fb
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39415002"
---
# <a name="join-a-new-windows-10-device-with-azure-ad-during-a-first-run"></a>Junte-se a um novo dispositivo Windows 10 com o Azure AD durante uma execução primeiro

Gestão de dispositivos no Azure Active Directory (Azure AD), pode certificar-se de que os utilizadores estão a aceder ao seus recursos de dispositivos que cumprem as normas de segurança e conformidade. Para obter mais informações, consulte a [introdução à gestão de dispositivos no Azure Active Directory](overview.md).

Com o Windows 10, pode associar um novo dispositivo ao Azure AD durante a experiência de primeira execução (FRX).  
Isto permite-lhe distribuir dispositivos compactados aos seus funcionários e estudantes.

Se tiver o Windows 10 Professional ou Windows 10 Enterprise instalada num dispositivo, a experiência padrão é o processo de configuração para dispositivos pertencentes à empresa.

Em que o Windows *experiência de out-of-box*, associação a um domínio do Active Directory (AD) no local não é suportada. Se pretender associar um computador a um domínio do AD durante a configuração, selecione a ligação **configurar o Windows com uma conta local**. Em seguida, pode ingressar no domínio das definições no seu computador.
 


## <a name="before-you-begin"></a>Antes de começar

Para associar um dispositivo Windows 10, o serviço de registo do dispositivo tem de ser configurado para que possa registar dispositivos. Além de ter permissão para associar dispositivos no seu inquilino do Azure AD, tem de ter menos dispositivos registados que o máximo configurado. Para obter mais informações, consulte [configurar definições do dispositivo](device-management-azure-portal.md#configure-device-settings).

Além disso, se o seu inquilino está federado, o fornecedor de identidade tem de suportar o ponto de extremidade WS-Trust e WS-Fed de nome de utilizador/palavra-passe. Isso pode ser a versão 1.3 ou 2005. Este suporte de protocolo é necessário para associar o dispositivo ao Azure AD e inicie sessão no dispositivo com uma palavra-passe.

## <a name="joining-a-device"></a>Ingressar num dispositivo

**Para associar um dispositivo Windows 10 ao Azure AD durante FRX:**


1. Quando ativar o seu dispositivo novo e iniciar o processo de configuração, deverá ver o **obtendo pronto** mensagem. Siga as instruções para configurar o dispositivo.

2. Comece por personalizar seu idioma e região. Em seguida, aceite os termos de licença de Software da Microsoft.
 
    ![Personalizar para a sua região](./media/azuread-joined-devices-frx/01.png)

3. Selecione a rede que pretende utilizar para ligar à Internet.

4. Clique em **este dispositivo pertence à minha organização**. 

    ![Quem é o proprietário deste ecrã PC](./media/azuread-joined-devices-frx/02.png)

5. Introduza as credenciais que foram fornecidas pela sua organização e, em seguida, clique em **iniciar sessão**.

    ![Ecrã de início de sessão](./media/azuread-joined-devices-frx/03.png)

6. Dispositivo localiza um inquilino correspondente no Azure AD. Se estiver num domínio federado, será redirecionado para o servidor de proteger o serviço de Token (STS) no local, por exemplo, Active Directory Federation Services (AD FS).

7. Se for um utilizador num domínio não federadas, introduza as credenciais diretamente na página alojado no AD do Azure. 

8. São-lhe pedido para um desafio de autenticação multifator. 
 
9. O Azure AD verifica se a uma inscrição na gestão de dispositivos móveis é necessária.

10. Windows regista o dispositivo no diretório da organização no Azure AD e inscreve-o na gestão de dispositivos móveis, se aplicável.

11. Se for:
    - Um utilizador gerido, Windows leva-o para a área de trabalho através do processo de início de sessão automático.

    - Um utilizador federado, será direcionado para o ecrã de início de sessão do Windows para introduzir as suas credenciais.

## <a name="verification"></a>Verificação

Para verificar se um dispositivo é associado ao Azure AD, reveja os **acesso profissional ou escolar** caixa de diálogo no seu dispositivo Windows. A caixa de diálogo deve indicar que está ligado ao seu diretório do Azure AD.

![Acesso profissional ou escolar](./media/azuread-joined-devices-frx/13.png)


## <a name="next-steps"></a>Passos Seguintes

- Para obter mais informações, consulte a [introdução à gestão de dispositivos no Azure Active Directory](overview.md).

- Para obter mais informações sobre a gestão de dispositivos no portal do Azure AD, consulte [gestão de dispositivos no portal do Azure](device-management-azure-portal.md).
