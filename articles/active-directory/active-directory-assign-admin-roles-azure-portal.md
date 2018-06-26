---
title: Atribuir funções de administrador no Azure Active Directory | Microsoft Docs
description: Uma função de administrador pode adicionar utilizadores, atribuir funções administrativas, repor palavras-passe do utilizador, gerir licenças de utilizador ou gerir os domínios. Um utilizador a quem é atribuído uma função de administrador tem as mesmas permissões em todos os serviços de nuvem para o qual a sua organização tiver subscrito.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 06/07/2018
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.openlocfilehash: 1de2482b7795bbed82874b6eea29f89f1ff52560
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/25/2018
ms.locfileid: "36938457"
---
# <a name="assigning-administrator-roles-in-azure-active-directory"></a>Atribuir funções de administrador no Azure Active Directory

Utilizar o Azure Active Directory (Azure AD), pode designar administradores separados para servir diferentes funções. Os administradores têm acesso a várias funcionalidades no portal do Azure e, consoante a respetiva função, podem criar ou editar utilizadores, atribuir funções administrativas a outras pessoas, repor palavras-passe do utilizador, gerir licenças de utilizador e gerir os domínios, entre outras coisas. Um utilizador a quem é atribuído uma função de administrador terá as mesmas permissões em todos os serviços de nuvem a que a organização tem subscritas, independentemente de atribuir a função no portal do Office 365, ou no portal do Azure ou utilizando o módulo do Azure AD para Windows PowerShell.

## <a name="details-about-the-global-administrator-role"></a>Detalhes sobre a função de Administrador Global
O Administrador Global tem acesso a todas as funcionalidades administrativas. Por predefinição, a pessoa que se inscreve para uma subscrição do Azure é atribuída a função de administrador global para o diretório. Apenas os administradores globais podem atribuir outras funções de administrador.

## <a name="assign-or-remove-administrator-roles"></a>Atribuir ou remover funções de administrador
Para saber como atribuir funções administrativas a um utilizador no Azure Active Directory, consulte [atribuir um utilizador a funções de administrador no Azure Active Directory](fundamentals/active-directory-users-assign-role-azure-portal.md).

## <a name="available-roles"></a>Funções disponíveis
Estão disponíveis as seguintes funções de administrador:

* **Administrador da aplicação**: os utilizadores nesta função podem criar e gerir todos os aspetos de aplicações da empresa, registos de aplicações e definições de proxy da aplicação. Esta função também concede a capacidade de consentimento para permissões de delegado e permissões de aplicações excluindo Microsoft Graph e o Azure AD Graph. Os membros desta função não são adicionados como proprietários ao criar novos registos de aplicação ou aplicações da empresa.

* **Programador de aplicações**: os utilizadores nesta função podem criar registos de aplicações quando o "Os utilizadores podem registar aplicações" definição está definida como não. Esta função também permite que os membros consentimento no seu próprio nome quando a "Os utilizadores podem autorizar aplicações acedem aos dados da empresa em nome daqueles" definição está definida como não. Os membros desta função são adicionados como proprietários ao criar novos registos de aplicação ou aplicações da empresa.

* **Administrador de faturação**: efetua compras, gere subscrições, gere pedidos de suporte e monitoriza o estado de funcionamento do serviço.

* **Administrador da aplicação de nuvem**: os utilizadores nesta função têm as mesmas permissões que a função de administrador da aplicação, excluindo a capacidade de gerir o proxy de aplicações. Esta função concede a capacidade de criar e gerir todos os aspetos de aplicações da empresa e registos de aplicação. Esta função também concede a capacidade de consentimento para permissões de delegado e permissões de aplicações excluindo Microsoft Graph e o Azure AD Graph. Os membros desta função não são adicionados como proprietários ao criar novos registos de aplicação ou aplicações da empresa.

* **Administrador de compatibilidade**: os utilizadores com esta função têm permissões de gestão dentro no Office 365 segurança & do Centro de conformidade e do Centro de administração do Exchange. Obter mais informações em "[sobre as funções de administrador do Office 365](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d)."

* **Administrador de acesso condicional**: os utilizadores com esta função têm a capacidade para gerir as definições de acesso condicional do Azure Active Directory.
  > [!NOTE]
  > Para implementar a política de acesso condicional do Exchange ActiveSync no Azure, o utilizador também tem de ser Administrador Global.
  
* **Administradores de dispositivos**: esta função está disponível para atribuição apenas como um administrador local adicional no [definições do dispositivo](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/DevicesMenuBlade/DeviceSettings/menuId/). Os utilizadores com esta função tornam-se administradores de computadores locais em todos os dispositivos Windows 10 que estão associados ao Azure Active Directory. Não têm a capacidade de gerir objetos de dispositivos no Azure Active Directory.

* **Leitores de diretório**: Esta é uma função de legado que está a ser atribuída a aplicações que não suportam o [consentimento Framework](active-directory-integrating-applications.md). Não deve ser atribuído a qualquer utilizador.

* **Contas de sincronização de diretórios**: não utilize. Esta função é atribuída automaticamente ao serviço do Azure AD Connect e não se destina ou não está suportada para quaisquer outras utilizações.

* **Diretório escritores**: Esta é uma função de legado que está a ser atribuída a aplicações que não suportam o [consentimento Framework](active-directory-integrating-applications.md). Não deve ser atribuído a qualquer utilizador.

* **Administrador de Dynamics 365**: os utilizadores com esta função têm permissões global dentro do Microsoft Dynamics 365, quando o serviço está presente, bem como a capacidade de gerir pedidos de suporte e monitorizar estado de funcionamento do serviço. Obter mais informações em [funções de administrador sobre o Office 365](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

* **Administrador de serviço do Exchange**: os utilizadores com esta função permissões global dentro do Microsoft Exchange Online, quando o serviço está presente. Obter mais informações em [funções de administrador sobre o Office 365](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

* **Administrador global / administrador da empresa / administrador inquilino**: os utilizadores com esta função tem acesso a todas as funcionalidades administrativas no Azure Active Directory, bem como serviços que federar ao Azure Active Directory como o Exchange Online, O SharePoint Online e Skype para empresas Online. A pessoa que se inscreve para o inquilino do Azure Active Directory torna-se um administrador global. Apenas os administradores globais podem atribuir outras funções de administrador. Só pode existir mais do que um administrador global da sua empresa. Administradores globais podem repor a palavra-passe para qualquer utilizador e de todos os outros administradores.

  > [!NOTE]
  > Na Microsoft Graph API, AD Graph API do Azure e Azure AD PowerShell, esta função é identificada como "Administrador da empresa". Faz o "Administrador Global" do [portal do Azure](https://portal.azure.com).
  >
  >

* **Convidados Inviter**: os utilizadores nesta função podem gerir convites para de utilizador do Azure Active Directory B2B convidado quando a definição de utilizador "Membros podem convidar" está definida como não. Mais informações sobre a colaboração B2B em [colaboração sobre o Azure AD B2B](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b). Não inclui quaisquer outras permissões.

* **Administrador de proteção de informações**: os utilizadores com esta função de direitos de utilizador apenas no serviço do Azure Information Protection. Estes não são concedidos direitos de utilizador no Centro de proteção de identidade, Privileged Identity Management, Monitor Office 365 serviço de estado de funcionamento, ou segurança do Office 365 e o Centro de conformidade. Pode configurar as etiquetas para a política do Azure Information Protection, gerir modelos de proteção e ative a proteção.

* **Administrador de serviço do Intune**: os utilizadores com esta função permissões global no Microsoft Intune Online, quando o serviço está presente. Além disso, esta função contém a capacidade de gerir utilizadores e dispositivos para associar a política, bem como criar e gerir grupos.

* **Caixa de correio administrador**: esta função é utilizada apenas como parte do suporte de correio eletrónico Exchange Online para dispositivos RIM Blackberry. Se a sua organização não utilizar o e-mail do Exchange Online em dispositivos RIM Blackberry, não utilize esta função.

* **Leitor do Centro de mensagens**: os utilizadores nesta função podem monitorizar as notificações e atualizações de estado de funcionamento advisory [Centro de mensagens do Office 365](https://support.office.com/article/Message-center-in-Office-365-38FB3333-BFCC-4340-A37B-DEDA509C2093) para a respetiva organização nos serviços configurados como o Exchange, o Intune e Microsoft Equipas. Leitores de centro de mensagens recebem semanal digests de correio eletrónico de mensagens, atualizações e podem partilhar publicações no Centro de mensagens no Office 365. No Azure AD, os utilizadores atribuídos a esta função só terão acesso só de leitura nos serviços do Azure AD como utilizadores e grupos. 

* **Suporte de 1 de camada de parceiros**: não utilize. Esta função foi despromovida e será removida do Azure AD no futuro. Esta função destina-se por um pequeno número de parceiros de revenda Microsoft e não se destina a utilização geral.

* **Suporte de 2 de nível de parceiros**: não utilize. Esta função foi despromovida e será removida do Azure AD no futuro. Esta função destina-se por um pequeno número de parceiros de revenda Microsoft e não se destina a utilização geral.

* **Palavra-passe de administrador / suporte técnico administrador**: os utilizadores com esta função podem alterar as palavras-passe, gerir pedidos de serviço e monitorizar estado de funcionamento do serviço. Os administradores de suporte técnico podem alterar as palavras-passe apenas para os utilizadores e de outros administradores de suporte técnico. 

  > [!NOTE]
  > Na Microsoft Graph API, AD Graph API do Azure e Azure AD PowerShell, esta função é identificada como "Administrador de suporte técnico". É "Palavra-passe de administrador" no [portal do Azure](https://portal.azure.com/).
  >
  >
  
* **Administrador de serviço do Power BI**: os utilizadores com esta função têm permissões global dentro do Microsoft Power BI, quando o serviço está presente, bem como a capacidade de gerir pedidos de suporte e monitorizar estado de funcionamento do serviço. Obter mais informações em [funções de administrador sobre o Office 365](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d?ui=en-US&rs=en-001&ad=US).

* **Com privilégios de administrador com função**: os utilizadores com esta função podem gerir atribuições de funções no Azure Active Directory, bem como no Azure AD Privileged Identity Management. Além disso, esta função permite a gestão de todos os aspetos de Privileged Identity Management.

* **Relatórios de leitor**: os utilizadores com esta função podem ver a utilização de relatórios de dados e o dashboard de relatórios no Centro de administração do Office 365 e o contexto de adoção do pacote no Power BI. Além disso, a função fornece acesso ao início de sessão relatórios e a atividade no Azure AD e os dados devolvidos pelo Microsoft Graph API do relatório. Um utilizador atribuído à função de leitor de relatórios pode aceder apenas utilização relevante e as métricas de adoção. Se não possui permissões de administrador para configurar as definições ou acesso que administrador específico do produto centra-se como o Exchange. 

* **Administrador de segurança**: os utilizadores com esta função têm todas as permissões só de leitura da função de leitor de segurança com a capacidade de gerir a configuração de serviços relacionados com segurança: proteção de identidade de diretório do Azure Active Directory, Azure Proteção de informações, Privileged Identity Management, Office 365 segurança & e Centro de conformidade. Estão disponíveis mais informações sobre as permissões do Office 365 no [permissões Centro de conformidade de segurança do Office 365 &](https://support.office.com/article/Permissions-in-the-Office-365-Security-Compliance-Center-d10608af-7934-490a-818e-e68f17d0e9c1).

* **Leitor de segurança**: os utilizadores com esta função têm acesso só de leitura global, incluindo todas as informações no Azure Active Directory, Identity Protection, Privileged Identity Management, bem como a capacidade de leitura relatórios de início de sessão no Azure Active Directory e registos de auditoria. A função também concede a permissão só de leitura no Office 365 segurança & Centro de conformidade. Estão disponíveis mais informações sobre as permissões do Office 365 no [permissões Centro de conformidade de segurança do Office 365 &](https://support.office.com/article/Permissions-in-the-Office-365-Security-Compliance-Center-d10608af-7934-490a-818e-e68f17d0e9c1).

* **Administrador de suporte do serviço**: os utilizadores com esta função podem abrir os pedidos de suporte com a Microsoft para os serviços do Azure e o Office 365 e vistas de dashboard de serviço e mensagem center no portal do Azure e o portal de administração do Office 365. Obter mais informações em [funções de administrador sobre o Office 365](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

* **Administrador de serviço do SharePoint**: os utilizadores com esta função têm permissões global no Microsoft SharePoint Online, quando o serviço está presente, bem como a capacidade de gerir pedidos de suporte e monitorizar estado de funcionamento do serviço. Obter mais informações em [funções de administrador sobre o Office 365](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

* **Skype para empresas / Lync serviço administrador**: os utilizadores com esta função permissões global no Microsoft Skype para empresas, quando o serviço está presente, bem como gerir os atributos de utilizador do Skype específico no Azure Active Directory. Além disso, esta função concede a capacidade de gerir pedidos de suporte e monitorizar estado de funcionamento do serviço. Obter mais informações em [funções de administrador sobre o Office 365](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

  > [!NOTE]
  > Na Microsoft Graph API, AD Graph API do Azure e Azure AD PowerShell, esta função é identificada como "Administrador do serviço Lync". É "Skype para empresas administrador de serviço" no [portal do Azure](https://portal.azure.com/).
  >
  >

* **Administrador de conta de utilizador**: os utilizadores com esta função podem criar e gerir todos os aspetos de utilizadores e grupos. Além disso, esta função inclui a capacidade de gerir pedidos de suporte e monitores de estado de funcionamento do serviço. Algumas restrições aplicam-se. Por exemplo, esta função não permite a eliminação de um administrador global. Os administradores de conta de utilizador podem alterar as palavras-passe para os utilizadores, administradores de suporte técnico e apenas outros administradores de conta de utilizador.

## <a name="administrator-permissions"></a>Permissões de administrador

### <a name="application-administrator"></a>Administrador de Aplicações

| Pode fazê-lo | Não é possível efetuar |
| --- | --- |
| Ler todas as informações de diretório<br>Criar registos de aplicações<br>Atualizar propriedades de registo de aplicação<br>Obter aplicações da empresa<br>Gerir as permissões de registo de aplicação<br>Eliminar registos de aplicações<br>Gerir aplicações único início de sessão definições da empresa<br>Gerir definições de aprovisionamento de aplicações de enterprise<br>Gerir definições de self-service de aplicações na empresa<br>Gerir definições de permissão de aplicação empresarial<br>Gerir o acesso de aplicação<br>Gerir definições de aprovisionamento<br>Eliminar aplicações da empresa<br>Consentimento em nome de todas as pessoas para todos os pedidos de permissão delegado<br>Consentimento em nome de todas as pessoas para todos os pedidos de permissão de aplicação, exceto o Azure AD Graph ou Microsoft Graph<br>Gerir definições de proxy de aplicações<br>Definições dos serviços de acesso<br>Monitorizar estado de funcionamento do serviço<br>Gerir pedidos de suporte<br>Associação ao grupo de leitura, ocultada | Criar, editar e eliminar grupos<br>Gerir licenças de utilizador<br>Utilizar a sincronização de diretórios<br>Ver relatórios de início de sessão e registos de auditoria | 

### <a name="application-developer"></a>Programador de Aplicações

| Pode fazê-lo | Não é possível efetuar |
| --- | --- |
| Ler todas as informações de diretório<br>Criar registos de aplicações<br>Consentimento em nome próprio | Ver início de sessão e registos de auditoria<br>Associação ao grupo de leitura, ocultada |

### <a name="billing-administrator"></a>Administrador de Faturação

| Pode fazê-lo | Não é possível efetuar |
| --- | --- |
|<p>Ver informações de utilizador e da empresa</p><p>Gerir pedidos de suporte do Office</p><p>Efetuar operações de faturação e compras para produtos do Office</p> |<p>Repor palavras-passe do utilizador</p><p>Criar e gerir vistas de utilizador</p><p>Criar, editar, eliminar utilizadores e grupos e gerir licenças de utilizador</p><p>Gerir domínios</p><p>Gerir as informações da empresa</p><p>Delegar funções administrativas a outras pessoas</p><p>Utilizar a sincronização de diretórios</p><p>Ver registos de auditoria</p> |

### <a name="cloud-application-administrator"></a>Administrador da Aplicação Cloud

| Pode fazê-lo | Não é possível efetuar |
| --- | --- |
| Ler todas as informações de diretório<br>Criar registos de aplicações<br>Atualizar propriedades de registo de aplicação<br>Obter aplicações da empresa<br>Gerir as permissões de registo de aplicação<br>Eliminar registos de aplicações<br>Gerir aplicações único início de sessão definições da empresa<br>Gerir definições de aprovisionamento de aplicações de enterprise<br>Gerir definições de self-service de aplicações na empresa<br>Gerir definições de permissão de aplicação empresarial<br>Gerir o acesso de aplicação<br>Gerir definições de aprovisionamento<br>Eliminar aplicações da empresa<br>Consentimento em nome de todas as pessoas para todos os pedidos de permissão delegado<br>Consentimento em nome de todas as pessoas para todos os pedidos de permissão de aplicação, exceto o Azure AD Graph ou Microsoft Graph<br>Definições dos serviços de acesso<br>Monitorizar estado de funcionamento do serviço<br>Gerir pedidos de suporte<br>Associação ao grupo de leitura, ocultada | Gerir definições de proxy de aplicações<br>Criar, editar e eliminar grupos<br>Gerir licenças de utilizador<br>Utilizar a sincronização de diretórios<br>Ver relatórios de início de sessão e registos de auditoria |

### <a name="conditional-access-administrator"></a>Administrador de Acesso Condicional

| Pode fazê-lo | Não é possível efetuar |
| --- | --- |
|<p>Ver informações de utilizador e da empresa</p><p>Gerir definições de acesso condicional</p> |<p>Repor palavras-passe do utilizador</p><p>Criar e gerir vistas de utilizador</p><p>Criar, editar, eliminar utilizadores e grupos e gerir licenças de utilizador</p><p>Gerir domínios</p><p>Gerir as informações da empresa</p><p>Delegar funções administrativas a outras pessoas</p><p>Utilizar a sincronização de diretórios</p><p>Ver registos de auditoria</p>|

### <a name="global-administrator"></a>Administrador Global
| Pode fazê-lo | Não é possível efetuar |
| --- | --- |
|<p>Ver informações de utilizador e da empresa</p><p>Gerir pedidos de suporte do Office</p><p>Efetuar operações de faturação e compras para produtos do Office</p><p>Repor palavras-passe do utilizador</p><p>Repor palavras-passe dos outros administradores</p> <p>Criar e gerir vistas de utilizador</p><p>Criar, editar, eliminar utilizadores e grupos e gerir licenças de utilizador</p><p>Gerir domínios</p><p>Gerir as informações da empresa</p><p>Delegar funções administrativas a outras pessoas</p><p>Utilizar a sincronização de diretórios</p><p>Ativar ou desativar a autenticação multifator</p><p>Ver registos de auditoria</p> |N/A |

### <a name="password-administrator--helpdesk-administrator"></a>Palavra-passe de administrador / suporte técnico administrador
| Pode fazê-lo | Não é possível efetuar |
| --- | --- |
| <p>Ver informações de utilizador e da empresa</p><p>Gerir pedidos de suporte do Office</p><p>Alterar as palavras-passe para os utilizadores e de outros administradores de suporte técnico apenas</p>|<p>Efetuar operações de faturação e compras para produtos do Office</p><p>Criar e gerir vistas de utilizador</p><p>Criar, editar, eliminar utilizadores e grupos e gerir licenças de utilizador</p><p>Gerir domínios</p><p>Gerir as informações da empresa</p><p>Delegar funções administrativas a outras pessoas</p><p>Utilizar a sincronização de diretórios</p><p>Ver relatórios</p>|

### <a name="information-protection-administrator"></a>Administrador do Information Protection
Em | Pode fazê-lo
-------- | ---------
Azure Information Protection | <li>Configurar as definições e de etiquetas nas políticas de âmbito e global<li>Configurar e gerir modelos de proteção<li>Ativar ou desativar proteção-
 
### <a name="reports-reader"></a>Leitor de Relatórios 
Pode fazê-lo | Não é possível efetuar
------ | ----------
Vista do Azure AD início de sessão relatórios e registos de auditoria<br>Ver informações de utilizador e da empresa<br>Dashboard de utilização do acesso do Office 365 | Criar e gerir vistas de utilizador<br>Criar, editar, eliminar utilizadores e grupos e gerir licenças de utilizador<br>Delegar funções administrativas a outras pessoas<br>Gerir as informações da empresa

### <a name="security-reader"></a>Leitor de Segurança
| Em | Pode fazê-lo |
| --- | --- |
| Identity Protection Center |Ler todos os relatórios de segurança e informações de definições de funcionalidades de segurança<ul><li>Anti spam<li>Encriptação<li>Prevenção de perda de dados<li>Antimalware<li>Proteção avançada contra ameaças<li>Antiphishing<li>Regras de Mailflow |
| Privileged Identity Management |<p>Tem acesso só de leitura para todas as informações anexados no Azure AD PIM: as políticas e relatórios para atribuições de funções do Azure AD, segurança analisa e acesso aos dados de política e relatórios para cenários para além de atribuição de função do Azure AD de leitura no futuro.<p>**Não é possível** inscrever-se no Azure AD PIM ou efetue as alterações. No portal do PIM ou através do PowerShell, alguém que esta função pode ativar funções adicionais (por exemplo, Administrador Global ou com privilégios de função de administrador), se o utilizador é uma candidata para os mesmos. |
| <p>O estado de funcionamento do monitor do Office 365 serviço</p><p>Segurança do Office 365 e o Centro de conformidade</p> |<ul><li>Ler e gerir alertas<li>Políticas de segurança de leitura<li>Ler informações sobre ameaças, o Cloud App Discovery e colocam em quarentena na procura e investigar<li>Ler todos os relatórios |

### <a name="security-administrator"></a>Administrador de Segurança
| Em | Pode fazê-lo |
| --- | --- |
| Identity Protection Center |<ul><li>Todas as permissões da função de leitor de segurança.<li>Além disso, a capacidade de efetuar todas as operações de IPC exceto repor palavras-passe. |
| Privileged Identity Management |<ul><li>Todas as permissões da função de leitor de segurança.<li>**Não é possível** gerir as subscrições de funções do Azure AD ou as definições. |
| <p>O estado de funcionamento do monitor do Office 365 serviço</p><p>Segurança do Office 365 e o Centro de conformidade |<ul><li>Todas as permissões da função de leitor de segurança.<li>Pode configurar todas as definições na funcionalidade Advanced Threat Protection (proteção de software maligno e vírus, maliciosa URL configuração, o rastreio de URL, etc.). |

### <a name="service-administrator"></a>Administrador de Serviços
| Pode fazê-lo | Não é possível efetuar |
| --- | --- |
| <p>Ver informações de utilizador e da empresa</p><p>Gerir pedidos de suporte do Office</p> |<p>Repor palavras-passe do utilizador</p><p>Efetuar operações de faturação e compras para produtos do Office</p><p>Criar e gerir vistas de utilizador</p><p>Criar, editar, eliminar utilizadores e grupos e gerir licenças de utilizador</p><p>Gerir domínios</p><p>Gerir as informações da empresa</p><p>Delegar funções administrativas a outras pessoas</p><p>Utilizar a sincronização de diretórios</p><p>Ver registos de auditoria</p> |

### <a name="user-account-administrator"></a>Administrador de Conta de Utilizador
| Pode fazê-lo | Não é possível efetuar |
| --- | --- |
| <p>Ver informações de utilizador e da empresa</p><p>Gerir pedidos de suporte do Office</p><p>Alterar as palavras-passe para os utilizadores, administradores de suporte técnico e apenas outros administradores de conta de utilizador</p><p>Criar e gerir vistas de utilizador</p><p>Criar, editar, eliminar utilizadores e grupos e gerir licenças de utilizador, com limitações. Seja não pode eliminar um administrador global nem criar outros administradores.</p> |<p>Efetuar operações de faturação e compras para produtos do Office</p><p>Gerir domínios</p><p>Gerir as informações da empresa</p><p>Delegar funções administrativas a outras pessoas</p><p>Utilizar a sincronização de diretórios</p><p>Ativar ou desativar a autenticação multifator</p><p>Ver registos de auditoria</p> |

### <a name="to-add-a-user-as-a-global-administrator"></a>Para adicionar um utilizador como um administrador global

1. Iniciar sessão para o [Centro de administração do Azure Active Directory](https://aad.portal.azure.com) com uma conta que seja um Administrador Global do diretório do inquilino.

   ![Abrir o Centro de administração do azure AD](./media/active-directory-assign-admin-roles-azure-portal/active-directory-admin-center.png)

2. Selecione **utilizadores** > **todos os utilizadores**.

3. Abra a página para o utilizador que pretende designar como Administrador Global.

4. Na barra de comandos, selecione **função de diretório**.

5. Selecione **Adicionar função**.

6. Na página de função de diretório, selecione o **Administrador Global** função e, em seguida, clique em **selecione** para guardar.

## <a name="deprecated-roles"></a>Funções preteridas

As seguintes funções não devem ser utilizadas. Estes são preteridas e serão removidas do Azure AD no futuro.

* Administrador de Licenças AdHoc
* Criador de Utilizador Verificado por E-mail
* Associação de dispositivos
* Gestores de Dispositivo
* Utilizadores de dispositivos
* Associação de Dispositivos da Área de Trabalho

## <a name="next-steps"></a>Passos Seguintes

* Para obter mais informações sobre como alterar os administradores para uma subscrição do Azure, consulte [adicionar ou alterar os administradores da subscrição do Azure](../billing-add-change-azure-subscription-administrator.md)
* Para saber mais sobre como o Microsoft Azure controla o acesso aos recursos, consulte [Noções sobre o acesso aos recursos no Azure](../role-based-access-control/rbac-and-directory-admin-roles.md)
* Para obter mais informações sobre como o Azure Active Directory relacionada com a sua subscrição do Azure, consulte [subscrições do Azure como estão associadas ao Azure Active Directory](fundamentals/active-directory-how-subscriptions-associated-directory.md)
* [Gerir utilizadores](active-directory-create-users.md)
* [Gerir palavras-passe](active-directory-manage-passwords.md)
* [Gerir grupos](fundamentals/active-directory-manage-groups.md)
