---
title: Como configurar híbrida do Azure Active Directory dispositivos associados ao | Documentos da Microsoft
description: Saiba como configurar híbrida do Azure Active Directory associados a um dispositivos.
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
ms.date: 07/31/2018
ms.author: markvi
ms.reviewer: sandeo
ms.openlocfilehash: 1aa5c0f259fdb5fce449bc86f9cf293801b4770a
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/31/2018
ms.locfileid: "39369171"
---
# <a name="tutorial-configure-hybrid-azure-active-directory-join-for-federated-domains"></a>Tutorial: Configurar a associação ao Azure Active Directory de híbrido para domínios federados

De forma semelhante a um utilizador, um dispositivo está se tornando outra identidade que pretende proteger e também utilizar para proteger os seus recursos em qualquer momento e local. Pode concretizar este objetivo ao trazer as identidades dos seus dispositivos com o Azure AD através de um dos seguintes métodos:

- Associação do Azure AD
- Associação ao Azure AD Híbrido
- Registo do Azure AD

Ao trazer os seus dispositivos com o Azure AD, maximize a produtividade dos seus utilizadores através do início de sessão único (SSO) todos os seus recursos na cloud e no local. Ao mesmo tempo, pode proteger o acesso aos seus recursos na cloud e no local com o [acesso condicional](../active-directory-conditional-access-azure-portal.md).

Neste tutorial, saiba como configurar a associação do Azure AD híbrido para dispositivos que federado utilizando AD FS.

> [!div class="checklist"]
> * Configurar a associação ao Azure AD híbrido
> * Permitir que os dispositivos de nível inferior do Windows
> * Verifique se o registo
> * Resolução de problemas


## <a name="prerequisites"></a>Pré-requisitos

Este tutorial parte do princípio de que está familiarizado com:

-  [Introdução à gestão de dispositivos no Azure Active Directory](../device-management-introduction.md)

-  [Como planear a sua implementação híbrida do Azure Active Directory](hybrid-azuread-join-plan.md)



Para configurar o cenário neste tutorial, terá de:

- Windows Server 2012 R2 com o AD FS

- [O Azure AD Connect](https://www.microsoft.com/download/details.aspx?id=47594) versão 1.1.819.0 ou superior. 
 

A partir da versão 1.1.819.0, o Azure AD Connect fornece um Assistente para configurar a associação do Azure AD híbrido. O assistente permite-lhe simplificar significativamente o processo de configuração. O assistente relacionado:

- Configura os pontos de ligação de serviço (SCP) para o registo do dispositivo

- Faz backup de sua do Azure AD existente confiança de entidade confiadora

- Atualiza as regras de afirmação a confiança do Azure AD

Os passos de configuração neste artigo baseiam-se este assistente. Se tiver uma versão mais antiga do Azure AD Connect instalado, tem de atualizá-lo para 1.1.819 ou superior. Se instalar a versão mais recente do Azure AD Connect não é uma opção para si, veja [como configurar manualmente o registo de dispositivos](../device-management-hybrid-azuread-joined-devices-setup.md).

Associação do híbrida do Azure AD requer que os dispositivos têm acesso a recursos da Microsoft seguintes na rede da sua organização:  

- https://enterpriseregistration.windows.net
- https://login.microsoftonline.com
- https://device.login.microsoftonline.com
- STS de sua organização (domínios federados)
- https://autologon.microsoftazuread-sso.com (Se estiver a utilizar ou planear utilizar o SSO totalmente integrado)

Se sua organização necessita de acesso à Internet através de um proxy de saída, começando com o Windows 10 1709, pode configurar as definições de proxy no seu computador usando um objeto de política de grupo (GPO). Se o computador esteja executando alguma versão mais antiga que o Windows 10 1709, tem de implementar o Proxy Auto-Discovery WPAD (Web) para permitir que os computadores Windows 10 fazer o registo de dispositivos com o Azure AD. 

Se sua organização necessita de acesso à Internet através de um proxy autenticado de saída, tem de certificar-se de que os seus computadores Windows 10 podem autenticar com êxito para o proxy de saída. Como computadores Windows 10 executarem o registo de dispositivos com o contexto da máquina, é necessário configurar a autenticação de proxy de saída usando o contexto da máquina. Dar seguimento, com o seu fornecedor de proxy de saída, sobre os requisitos de configuração. 


## <a name="configure-hybrid-azure-ad-join"></a>Configurar a associação ao Azure AD híbrido

Para configurar uma associação do Azure AD híbrido com o Azure AD Connect, terá de:

- As credenciais de um administrador global do inquilino do Azure AD.  

- As credenciais de administrador de empresa para cada uma das florestas.

- As credenciais do seu administrador do AD FS. 


**Para configurar uma associação do Azure AD híbrido com o Azure AD Connect:**

1. Inicie o Azure AD Connect e, em seguida, clique em **configurar**.

    ![Bem-vindo](./media/hybrid-azuread-join-federated-domains/11.png)

2. Sobre o **tarefas adicionais** página, selecione **configurar opções do dispositivo**e, em seguida, clique em **seguinte**. 

    ![Tarefas adicionais](./media/hybrid-azuread-join-federated-domains/12.png)

3. Sobre o **descrição geral** página, clique em **próxima**. 

    ![Descrição geral](./media/hybrid-azuread-join-federated-domains/13.png)

4. Sobre o **ligar para o Azure AD** página, introduza as credenciais de administrador global do seu inquilino do Azure AD e, em seguida, clique em **próxima**.   

    ![Ligar ao Azure AD](./media/hybrid-azuread-join-federated-domains/14.png)

5. Sobre o **opções do dispositivo** página, selecione **associação ao configurar híbrido do Azure AD**e, em seguida, clique em **seguinte**. 

    ![Opções do dispositivo](./media/hybrid-azuread-join-federated-domains/15.png)

6. Sobre o **SCP** página, execute os seguintes passos e, em seguida, clique em **próxima**: 

    ![SCP](./media/hybrid-azuread-join-federated-domains/16.png)

    a. Selecione uma floresta.

    b. Selecione o serviço de autenticação.

    c. Clique em **adicionar** para introduzir as credenciais de administrador de empresa.


7. Sobre o **sistemas operativos de dispositivos** página, selecione os sistemas operativos utilizados pelos dispositivos no seu ambiente do Active Directory e, em seguida, clique em **próxima**. 

    ![Sistema operativo do dispositivo](./media/hybrid-azuread-join-federated-domains/17.png)

8. Sobre o **configuração de Federação** página, introduza as credenciais do seu administrador do AD FS e, em seguida, clique em **próxima**. 

    ![Configuração de federação](./media/hybrid-azuread-join-federated-domains/18.png)

9. Sobre o **pronto para configurar** página, clique em **configurar**. 

    ![Preparado para configurar](./media/hybrid-azuread-join-federated-domains/19.png)

10. Sobre o **concluir a configuração** página, clique em **saída**. 

    ![Configuração completa](./media/hybrid-azuread-join-federated-domains/20.png)




## <a name="enable-windows-down-level-devices"></a>Permitir que os dispositivos de nível inferior do Windows

Se alguns dos seus dispositivos associados a um domínio são dispositivos de nível inferior do Windows, terá de:

- Atualizar definições do dispositivo
 
- Configurar as definições de local intranet para o registo de dispositivos


### <a name="update-device-settings"></a>Atualizar definições do dispositivo 

Para registar dispositivos de nível inferior do Windows, terá de certificar-se de que as definições do dispositivo para permitir que os utilizadores registar dispositivos no Azure AD estão definidas. No portal do Azure, pode encontrar esta definição em:

`Home > [Name of your tenant] > Devices - Device settings`  


    
A seguinte política tem de ser definida **todos os**: **os utilizadores podem registar os seus dispositivos com o Azure AD**

![Registar dispositivos](./media/hybrid-azuread-join-federated-domains/23.png)


### <a name="configure-the-local-intranet-settings-for-device-registration"></a>Configurar as definições de local intranet para o registo de dispositivos

Para concluir com êxito híbrido do Azure AD associar dos seus dispositivos de nível inferior do Windows e para evitar avisos de certificado quando os dispositivos serão autenticados autenticarem para o Azure AD, pode enviar por push uma política para os seus dispositivos associados a um domínio para adicionar os seguintes URLs para a Intranet Local zona no Internet Explorer:

- `https://device.login.microsoftonline.com`

- `https://device.login.microsoftonline.com`

- Serviço de Token de segurança (STS - domínios federados) da sua organização

- `https://autologon.microsoftazuread-sso.com` (para SSO totalmente integrado).

Além disso, tem de ativar **permitir atualizações à barra de estado por meio de script** na zona de local intranet do usuário.



## <a name="verify-the-registration"></a>Verifique se o registo

Para verificar o estado de registo do dispositivo no seu inquilino do Azure, pode utilizar o **[Get-MsolDevice](https://docs.microsoft.com/powershell/msonline/v1/get-msoldevice)** cmdlet no  **[módulo do Azure Active Directory PowerShell](/powershell/azure/install-msonlinev1?view=azureadps-2.0)**.

Ao utilizar o **Get-MSolDevice** cmdlet para verificar os detalhes do serviço:

- Um objeto com o **id de dispositivo** que corresponde ao ID do Windows cliente tem de existir.
- O valor para **DeviceTrustType** tem de ser **associados a um domínio**. Isso é equivalente a **Azure AD híbrido associou** estado na página de dispositivos no portal do Azure AD.
- O valor para **Enabled** tem de ser **verdadeiro** para dispositivos que são utilizados no acesso condicional. 


**Para verificar os detalhes do serviço:**

1. Open **Windows PowerShell** como administrador.

2. Tipo de `Connect-MsolService` para ligar ao seu inquilino do Azure.  

3. Digite `get-msoldevice -deviceId <deviceId>`.

6. Certifique-se de que **Enabled** está definida como **verdadeiro**.





## <a name="troubleshoot-your-implementation"></a>Resolver problemas relacionados com a implementação

Se estiver tendo problemas com a conclusão da híbrida associação do Azure AD para o domínio dispositivos associados ao Windows, consulte:

- [Associação ao Azure AD híbrido resolução de problemas para dispositivos atuais do Windows](../device-management-troubleshoot-hybrid-join-windows-current.md)
- [Resolução de problemas de associação do Azure AD híbrido para dispositivos de nível inferior do Windows](../device-management-troubleshoot-hybrid-join-windows-legacy.md)



## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Associação ao Azure Active Directory configurar híbrido para domínios geridos](hybrid-azuread-join-managed-domains.md)
> [configurar manualmente a associação ao Azure Active Directory de híbrido](../device-management-hybrid-azuread-joined-devices-setup.md)




<!--Image references-->
[1]: ./media/active-directory-conditional-access-automatic-device-registration-setup/12.png
