---
title: Como configurar dispositivos associados ao Azure Active Directory híbrido | Microsoft Docs
description: Saiba como configurar dispositivos associados ao Azure Active Directory híbrido.
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
ms.topic: tutorial
ms.date: 08/25/2018
ms.author: markvi
ms.reviewer: sandeo
ms.openlocfilehash: 2f020bdf79811c959e07d753231fc133fe597861
ms.sourcegitcommit: 67abaa44871ab98770b22b29d899ff2f396bdae3
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/08/2018
ms.locfileid: "48855188"
---
# <a name="tutorial-configure-hybrid-azure-active-directory-join-for-federated-domains"></a>Tutorial: Configurar a associação do Azure Active Directory híbrido para domínios federados

Da forma semelhante ao utilizador, o dispositivo está a tornar-se outra identidade que deve proteger e também utilizar para proteger os seus recursos em qualquer momento e em qualquer lugar. Pode concretizar este objetivo ao trazer as identidades dos seus dispositivos para o Azure AD através de um dos seguintes métodos:

- Associação ao Azure AD
- Associação ao Azure AD Híbrido
- Registo do Azure AD

Ao trazer os seus dispositivos para o Azure AD, maximiza a produtividade dos seus utilizadores através do início de sessão único (SSO) nos seus recursos na cloud e no local. Ao mesmo tempo, pode proteger o acesso aos seus recursos na cloud e no local com o [acesso condicional](../active-directory-conditional-access-azure-portal.md).

Neste tutorial, saiba como configurar a associação ao Azure AD híbrido para dispositivos federados com o ADFS.

> [!div class="checklist"]
> * Configurar a associação ao Azure AD híbrido
> * Permitir dispositivos de nível inferior do Windows
> * Verificar o registo
> * Resolução de problemas


## <a name="prerequisites"></a>Pré-requisitos

Este tutorial parte do princípio de que está familiarizado com:

-  [Introduction to device management in Azure Active Directory](../device-management-introduction.md) (Introdução à gestão de dispositivos no Azure Active Directory)

-  [Como planear a sua implementação associada híbrida do Azure Active Directory](hybrid-azuread-join-plan.md)

-  [Como controlar a associação híbrida do Azure AD dos seus dispositivos](hybrid-azuread-join-control.md)


Para configurar o cenário neste tutorial, precisa do:

- Windows Server 2012 R2 com o AD FS

- [Azure AD Connect](https://www.microsoft.com/download/details.aspx?id=47594) versão 1.1.819.0 ou superior. 
 

A partir da versão 1.1.819.0, o Azure AD Connect fornece um assistente para configurar a associação do Azure AD híbrido. O assistente permite-lhe simplificar significativamente o processo de configuração. O assistente relacionado:

- Configura os pontos de ligação de serviço (SCP) para o registo de dispositivos

- Faz uma cópia segurança da fidedignidade da entidade confiadora do Azure AD existente

- Atualiza as regras de afirmação na confiança do Azure AD

Os passos de configuração neste artigo baseiam-se neste assistente. Se tiver uma versão mais antiga do Azure AD Connect instalada, tem de atualizá-lo para a versão 1.1.819 ou superior. Se a instalação da versão mais recente do Azure AD Connect não é uma opção para si, veja [como configurar manualmente o registo de dispositivos](../device-management-hybrid-azuread-joined-devices-setup.md).

A associação do Azure AD híbrido requer que os dispositivos tenham acesso aos seguintes recursos da Microsoft a partir da rede da sua organização:  

- https://enterpriseregistration.windows.net
- https://login.microsoftonline.com
- https://device.login.microsoftonline.com
- O STS da sua organização (domínios federados)
- https://autologon.microsoftazuread-sso.com (Se estiver a utilizar ou planear utilizar o SSO totalmente integrado)

A partir do Windows 10 1803, se a associação instantânea ao Microsoft Azure AD Híbrido falhar num domínio federado como o AD FS, estaremos dependentes do Azure AD Connect para sincronizar o objeto de computador no Microsoft Azure AD que, em seguida, é utilizado para concluir o registo de dispositivos para a associação ao Microsoft Azure AD Híbrido.

Se a sua organização precisar de acesso à Internet através de um proxy de saída, a partir do Windows 10 1709, poderá [configurar as definições de proxy no computador através de um objeto de política de grupo (GPO)](https://blogs.technet.microsoft.com/netgeeks/2018/06/19/winhttp-proxy-settings-deployed-by-gpo/). Se o computador estiver a executar uma versão anterior ao Windows 10 1709, terá de implementar o Web Proxy Auto-Discovery (WPAD) para permitir que os computadores com Windows 10 façam o registo de dispositivos com o Microsoft Azure AD. 

Se sua organização precisar de acesso à Internet através de um proxy de saída autenticado, tem de se certificar de que os seus computadores com Windows 10 podem ser autenticados com êxito no proxy de saída. Uma vez que os computadores com Windows 10 executam o registo de dispositivos utilizando o contexto da máquina, é necessário configurar a autenticação de proxy de saída utilizando o contexto da máquina. Consulte o seu fornecedor de proxy de saída sobre os requisitos de configuração. 


## <a name="configure-hybrid-azure-ad-join"></a>Configurar a associação ao Azure AD híbrido

Para configurar uma associação doi Azure AD híbrido com o Azure AD Connect, precisa das:

- Credenciais de um administrador global para o inquilino do Azure AD.  

- As credenciais de administrador da empresa para cada uma das florestas.

- Credenciais do seu administrador do AD FS. 


**Para configurar uma associação do Azure AD híbrido com o Azure AD Connect:**

1. Inicie o Azure AD Connect e, em seguida, clique em **Configurar**.

    ![Bem-vindo](./media/hybrid-azuread-join-federated-domains/11.png)

2. Na página **Tarefas adicionais**, selecione **Configurar opções do dispositivo** e clique em **Seguinte**. 

    ![Tarefas adicionais](./media/hybrid-azuread-join-federated-domains/12.png)

3. Na página **Descrição geral**, clique em **Seguinte**. 

    ![Descrição geral](./media/hybrid-azuread-join-federated-domains/13.png)

4. Na página **Ligar ao Azure AD**, introduza as credenciais de um administrador global para o inquilino do Azure AD e clique em **Seguinte**.   

    ![Ligar ao Azure AD](./media/hybrid-azuread-join-federated-domains/14.png)

5. Na página **Opções do dispositivo**, selecione **Configurar a associação ao Azure AD Híbrido** e, em seguida, clique em **Seguinte**. 

    ![Opções do dispositivo](./media/hybrid-azuread-join-federated-domains/15.png)

6. Na página **SCP**, execute os passos seguintes e clique em **Seguinte**: 

    ![SCP](./media/hybrid-azuread-join-federated-domains/16.png)

    a. Selecione a floresta.

    b. Selecione o serviço de autenticação. Tem de selecionar o servidor AD FS, a menos que a sua organização tenha exclusivamente clientes do Windows 10.

    c. Clique em **Adicionar** para introduzir as credenciais de administrador da empresa.


7. Na página **Sistemas operativos dos dispositivos**, selecione os sistemas operativos utilizados pelos dispositivos no seu ambiente do Active Directory e, em seguida, clique em **Seguinte**. 

    ![Sistema operativo do dispositivo](./media/hybrid-azuread-join-federated-domains/17.png)

8. Na página **Configuração de federação**, introduza as credenciais do administrador do AD FS e clique em **Seguinte**. 

    ![Configuração de federação](./media/hybrid-azuread-join-federated-domains/18.png)

9. Na página **Preparado para configurar**, clique em **Configurar**. 

    ![Preparado para configurar](./media/hybrid-azuread-join-federated-domains/19.png)

10. Na página **Configuração completa**, clique em **Sair**. 

    ![Configuração completa](./media/hybrid-azuread-join-federated-domains/20.png)




## <a name="enable-windows-down-level-devices"></a>Permitir dispositivos de nível inferior do Windows

Se alguns dos seus dispositivos associados a um domínio são dispositivos de nível inferior do Windows, terá de:

- Atualizar as definições do dispositivo
 
- Configurar as definições de intranet local para o registo de dispositivos


### <a name="update-device-settings"></a>Atualizar as definições do dispositivo 

Para registar dispositivos de nível inferior do Windows, terá de se certificar de que as definições do dispositivo para permitir que os utilizadores registem dispositivos no Azure AD estão configuradas. No portal do Azure, pode encontrar esta definição em:

`Home > [Name of your tenant] > Devices - Device settings`  


    
A seguinte política tem de ser definida como **Todos**: **os utilizadores podem registar os seus dispositivos com o Azure AD**

![Registar dispositivos](./media/hybrid-azuread-join-federated-domains/23.png)


### <a name="configure-the-local-intranet-settings-for-device-registration"></a>Configurar as definições de intranet local para o registo de dispositivos

Para concluir com êxito a associação ao Azure AD híbrido dos seus dispositivos de nível inferior do Windows e para evitar avisos de certificado quando os dispositivos são autenticados no Azure AD, pode aplicar uma política aos seus dispositivos associados a um domínio para adicionar os seguintes URLs à zona de intranet local no Internet Explorer:

- `https://device.login.microsoftonline.com`

- `https://device.login.microsoftonline.com`

- O Serviço de Tokens de Segurança (STS - domínios federados) da sua organização

- `https://autologon.microsoftazuread-sso.com` (para SSO totalmente integrado).

Além disso, tem de ativar **Permitir atualizações à barra de estado por meio de script** na zona de intranet local do utilizador.



## <a name="verify-the-registration"></a>Verificar o registo

Para verificar o estado de registo do dispositivo no seu inquilino do Azure, pode utilizar o cmdlet **[Get-MsolDevice](https://docs.microsoft.com/powershell/msonline/v1/get-msoldevice)** no **[módulo PowerShell do Azure Active Directory](/powershell/azure/install-msonlinev1?view=azureadps-2.0)**.

Ao utilizar o cmdlet **Get-MSolDevice** para verificar os detalhes do serviço:

- Tem de existir um objeto com um **id de dispositivo** que corresponda ao ID do cliente Windows.
- O valor de **DeviceTrustType** tem de ser **Associados a um domínio**. Isto é equivalente ao estado **Associado ao Azure AD híbrido** na página Dispositivos no portal do Azure AD.
- O valor de **Ativado** tem de ser **Verdadeiro** para dispositivos que são utilizados no acesso condicional. 


**Para verificar os detalhes do serviço:**

1. Abra o **Windows PowerShell** como administrador.

2. Escreva `Connect-MsolService` para ligar ao seu inquilino do Azure.  

3. Digite `get-msoldevice -deviceId <deviceId>`.

6. Certifique-se de que **Ativado** está definido como **Verdadeiro**.





## <a name="troubleshoot-your-implementation"></a>Resolver problemas relacionados com a implementação

Se estiver a ter problemas a completar a associação ao Azure AD híbrido dos dispositivos Windows de domínio associado, veja:

- [Troubleshooting Hybrid Azure AD join for Windows current devices](troubleshoot-hybrid-join-windows-current.md) (Resolver problemas com a associação híbrida ao Azure AD para dispositivos Windows atuais)
- [Troubleshooting Hybrid Azure AD join for Windows down-level devices](troubleshoot-hybrid-join-windows-legacy.md) (Resolver problemas com a associação híbrida ao Azure AD para dispositivos Windows de nível inferior)



## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"]
> [Configurar a associação ao Azure Active Directory híbrido de domínios federados](hybrid-azuread-join-managed-domains.md)
> [Configurar a associação ao Azure Active Directory híbrido manualmente](hybrid-azuread-join-manual-steps.md)




<!--Image references-->
[1]: ./media/active-directory-conditional-access-automatic-device-registration-setup/12.png
