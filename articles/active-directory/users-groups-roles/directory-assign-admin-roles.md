---
title: Referenciam de funções de administrador no Azure Active Directory | Documentos da Microsoft
description: Uma função de administrador pode adicionar utilizadores, atribuir funções administrativas, repor palavras-passe do utilizador, gerir licenças de utilizador ou gerir domínios.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 09/19/2018
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.openlocfilehash: 04deb1168c8c5c0977d0f20c9307ce10d2d12d35
ms.sourcegitcommit: 06724c499837ba342c81f4d349ec0ce4f2dfd6d6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/19/2018
ms.locfileid: "46466120"
---
# <a name="assigning-administrator-roles-in-azure-active-directory"></a>Atribuir funções de administrador no Azure Active Directory

Utilizar o Azure Active Directory (Azure AD), pode designar administradores separados para servir as funções diferentes. Os administradores podem ser indicados no portal do Azure AD para realizar tarefas como adicionar ou alterar os utilizadores, atribuir funções administrativas, repor palavras-passe do utilizador, gerir licenças de utilizador e gerir nomes de domínio.

## <a name="details-about-the-global-administrator-role"></a>Detalhes sobre a função de administrador global

O administrador global tenha acesso a todas as funcionalidades administrativas. Por predefinição, a pessoa que se inscreve para uma subscrição do Azure é atribuída a função de administrador global do diretório. Apenas os administradores globais podem atribuir outras funções de administrador.

## <a name="assign-or-remove-administrator-roles"></a>Atribuir ou remover funções de administrador

Para saber como atribuir funções administrativas a um utilizador no Azure Active Directory, veja [atribuir um utilizador a funções de administrador no Azure Active Directory](../fundamentals/active-directory-users-assign-role-azure-portal.md).

## <a name="available-roles"></a>Funções disponíveis

As seguintes funções de administrador estão disponíveis:

* **[Administrador da aplicação](#application-administrator)**: os utilizadores nesta função podem criar e gerir todos os aspetos de aplicações empresariais, registos de aplicação e as definições de proxy da aplicação. Esta função também concede a capacidade de dar consentimento a permissões delegadas e permissões de aplicação, excluindo o Microsoft Graph e o Azure AD Graph. Os membros desta função não são adicionados como proprietários durante a criação de novos registos de aplicação ou aplicações da empresa.

* **[Desenvolvedor de aplicativos](#application-developer)**: os utilizadores nesta função podem criar registos de aplicações quando o "Os utilizadores podem registar aplicações" definição está definida como não. Esta função também permite que os membros dar consentimento em seu próprio benefício quando o "Os utilizadores podem dar consentimento a aplicações acedam aos dados da empresa em nome deles" definição está definida como não. Os membros desta função são adicionados como proprietários durante a criação de novos registos de aplicação ou aplicações da empresa.

* **[Administrador de faturação](#billing-administrator)**: efetua compras, gere subscrições, gere pedidos de suporte e monitoriza o estado de funcionamento do serviço.

* **[Administrador da aplicação cloud](#cloud-application-administrator)**: os utilizadores nesta função têm as mesmas permissões que a função de administrador da aplicação, excluindo a capacidade de gerir o proxy de aplicações. Esta função concede a capacidade de criar e gerir todos os aspetos de aplicações empresariais e registos de aplicação. Esta função também concede a capacidade de dar consentimento a permissões delegadas e permissões de aplicação, excluindo o Microsoft Graph e o Azure AD Graph. Os membros desta função não são adicionados como proprietários durante a criação de novos registos de aplicação ou aplicações da empresa.

* **[Administrador de dispositivos na cloud](#cloud-device-administrator)**: os utilizadores nesta função podem ativar, desativar e eliminar dispositivos no Azure AD e ler as chaves do BitLocker do Windows 10 (caso exista) no portal do Azure. A função não concede permissões para gerir todas as propriedades no dispositivo.

* **[Administrador de conformidade](#compliance-administrator)**: os utilizadores com esta função têm permissões de gestão na segurança do Office 365 e do Centro de conformidade e do Centro de administração do Exchange. Mais informações em [funções de administrador sobre o Office 365](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

* **[Administrador de acesso condicional](#conditional-access-administrator)**: os utilizadores com esta função têm a capacidade para gerir as definições de acesso condicional do Azure Active Directory.
  > [!NOTE]
  > Para implementar a política de acesso condicional do Exchange ActiveSync no Azure, o utilizador também tem de ser um Administrador Global.
  
* **[Administradores de dispositivos](#device-administrators)**: esta função está disponível para atribuição apenas como um administrador local adicional na [definições do dispositivo](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/DevicesMenuBlade/DeviceSettings/menuId/). Os utilizadores com esta função tornam-se administradores de computadores locais em todos os dispositivos Windows 10 que estão associados ao Azure Active Directory. Não têm a capacidade de gerir objetos de dispositivos no Azure Active Directory. 

* **[Leitores de diretório](#directory-readers)**: Esta é uma função legada que está a ser atribuída às aplicações que não suportam a [consentimento Framework](../develop/quickstart-v1-integrate-apps-with-azure-ad.md). Não deve ser atribuído a quaisquer utilizadores.

* **[Contas de sincronização de diretórios](#directory-synchronization-accounts)**: não utilize. Esta função é atribuída automaticamente ao serviço Azure AD Connect e não é a finalidade ou suportada para qualquer outro uso.

* **[Gravadores de diretórios](#directory-writers)**: Esta é uma função legada que está a ser atribuída às aplicações que não suportam a [consentimento Framework](../develop/quickstart-v1-integrate-apps-with-azure-ad.md). Não deve ser atribuído a quaisquer utilizadores.

* **[Administrador de serviços do Dynamics 365 / administrador de serviço CRM](#dynamics-365-service-administrator)**: os utilizadores com esta função possuem permissões globais no Microsoft Dynamics 365 Online, quando o serviço está presente, bem como a capacidade de gerir pedidos de suporte e monitorizar o estado de funcionamento do serviço. Mais informações em [utilizar a função de administrador de serviço para gerir o seu inquilino](https://docs.microsoft.com/dynamics365/customer-engagement/admin/use-service-admin-role-manage-tenant).

* **[Administrador de serviços do Exchange](#exchange-service-administrator)**: os utilizadores com esta função possuem permissões globais dentro do Microsoft Exchange Online, quando o serviço está presente. Mais informações em [funções de administrador sobre o Office 365](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

* **[Administrador global / administrador da empresa](#company-administrator)**: os utilizadores com esta função têm acesso a todas as funcionalidades administrativas no Azure Active Directory, bem como serviços que utilizam identidades do Azure Active Directory, como o Exchange Online, SharePoint Online e Skype para empresas Online. A pessoa que se inscreve no inquilino do Azure Active Directory torna-se um administrador global. Apenas os administradores globais podem atribuir outras funções de administrador. Podem existir mais do que um administrador global na sua empresa. Os administradores globais podem redefinir a palavra-passe para qualquer utilizador e a todos os outros administradores.

  > [!NOTE]
  > No Microsoft Graph API, o Azure AD Graph API e o Azure AD PowerShell, esta função é identificada como "Administrador de empresa". É "Administrador Global" no [portal do Azure](https://portal.azure.com).
  >
  >

* **[Autor de convites](#guest-inviter)**: os utilizadores nesta função podem gerir convites de utilizadores do Azure Active Directory B2B quando o **os membros podem convidar** definição de utilizador está definida como não. Obter mais informações sobre a colaboração B2B ao [colaboração sobre o Azure AD B2B](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b). Não inclui quaisquer outras permissões.

* **[Administrador do Information Protection](#information-protection-administrator)**: os utilizadores com esta função têm todas as permissões no serviço do Azure Information Protection. Esta função permite configurar etiquetas para a política do Azure Information Protection, gestão de modelos de proteção e ativar a proteção. Esta função não concede quaisquer permissões no Centro de proteção de identidade, o Privileged Identity Management, o Monitor Office 365 Service Health, ou o segurança do Office 365 e o Centro de conformidade.

* **[Administrador de serviço do Intune](#intune-service-administrator)**: os utilizadores com esta função possuem permissões globais no Microsoft Intune Online, quando o serviço está presente. Além disso, esta função contém a capacidade de gerir utilizadores e dispositivos para associar a política, bem como criar e gerir grupos. Mais informações em [o controlo de administração baseada em funções (RBAC) com o Microsoft Intune](https://docs.microsoft.com/intune/role-based-access-control)

* **[Administrador de licenças](#license-administrator)**: os utilizadores nesta função podem adicionar, remover e atualizar as atribuições de licenças em utilizadores, grupos (com licenciamento baseado em grupo) e gerir a localização de utilização sobre os usuários. A função não concede a capacidade de comprar ou gerir as subscrições, criar ou gerir grupos, ou criar ou gerir utilizadores além da localização de utilização.

* **[Leitor do Centro de mensagens](#message-center-reader)**: os utilizadores nesta função podem monitorizar notificações e atualizações de estado de funcionamento de aviso [Centro de mensagens do Office 365](https://support.office.com/article/Message-center-in-Office-365-38FB3333-BFCC-4340-A37B-DEDA509C2093) para a sua organização em serviços configurados como o Exchange, o Intune e o Microsoft Teams. Os leitores de centro de mensagens recebem resumos de e-mail semanal de postagens, atualizações e podem partilhar publicações do Centro de mensagens no Office 365. No Azure AD, os utilizadores atribuídos a esta função só terão acesso só de leitura nos serviços do Azure AD, como utilizadores e grupos. 

* **[Suporte de escalão 1 de parceiros](#partner-tier1-support)**: não utilize. Esta função foi preterida e será removida do Azure AD no futuro. Esta função é destinada à utilização por um pequeno número de parceiros de revenda da Microsoft e não se destina a utilização geral.

* **[Suporte de escalão 2 de parceiros](#partner-tier2-support)**: não utilize. Esta função foi preterida e será removida do Azure AD no futuro. Esta função é destinada à utilização por um pequeno número de parceiros de revenda da Microsoft e não se destina a utilização geral.

* **[Palavra-passe de administrador / administrador de suporte técnico](#helpdesk-administrator)**: os utilizadores com esta função podem alterar palavras-passe, invalidar os tokens de atualização, gerir pedidos de serviço e monitorizar o estado de funcionamento do serviço. Os administradores de suporte técnico podem alterar as palavras-passe e invalidar os tokens de atualização apenas para utilizadores e outros administradores de suporte técnico. Invalidar um token de atualização força o utilizador iniciar sessão novamente.

  > [!NOTE]
  > No Microsoft Graph API, o Azure AD Graph API e o Azure AD PowerShell, esta função é identificada como "Administrador de suporte técnico". É "Administrador de palavras-passe" na [portal do Azure](https://portal.azure.com/).
  >
  >
  
* **[Administrador de serviço do Power BI](#power-bi-service-administrator)**: os utilizadores com esta função possuem permissões globais dentro do Microsoft Power BI, quando o serviço está presente, bem como a capacidade para gerir pedidos de suporte e monitorizar o estado de funcionamento do serviço. Mais informações em [compreender a função de administrador do Power BI](https://docs.microsoft.com/power-bi/service-admin-role).

* **[Com privilégios administrador de função](#privileged-role-administrator)**: os utilizadores com esta função podem gerir atribuições de funções no Azure Active Directory, assim como no Azure AD Privileged Identity Management. Além disso, esta função permite a gestão de todos os aspetos do Privileged Identity Management.

* **[Leitor de relatórios](#reports-reader)**: os utilizadores com esta função podem ver a utilização de relatórios do pacote de dados e o dashboard de relatórios no Centro de administração do Office 365 e o contexto de adoção no Power BI. Além disso, a função fornece acesso ao início de sessão em relatórios e a atividade no Azure AD e a API de relatórios de dados devolvidos pelo Microsoft Graph. Pode aceder a um utilizador atribuído à função do leitor de relatórios apenas utilização relevante e métricas de adoção. Eles não tem quaisquer permissões de administrador para configurar as definições ou acesso que no produto específico Centro de administração como o Exchange. 

* **[Administrador de segurança](#security-administrator)**: os utilizadores com esta função têm todas as permissões só de leitura de função do leitor de segurança, além da capacidade para gerir a configuração de serviços relacionados com segurança: Azure Active Directory Identity Protection, Azure Information Protection e o Office 365 Centro de segurança e conformidade. Obter mais informações sobre as permissões do Office 365 estão disponíveis em [permissões no Centro de conformidade de segurança do Office 365 e](https://support.office.com/article/Permissions-in-the-Office-365-Security-Compliance-Center-d10608af-7934-490a-818e-e68f17d0e9c1).
  
  | Em | Pode fazer |
  | --- | --- |
  | Identity Protection Center |<ul><li>Todas as permissões da função do leitor de segurança.<li>Além disso, a capacidade de executar todas as operações de IPC, exceto para repor palavras-passe. |
  | Privileged Identity Management |<ul><li>Todas as permissões da função do leitor de segurança.<li>**Não é possível** gerir definições ou associações das funções do Azure AD. |
  | <p>O estado de funcionamento do monitor do Office 365 serviço</p><p>Centro de Segurança e Conformidade do Office 365 |<ul><li>Todas as permissões da função do leitor de segurança.<li>Pode configurar todas as definições no recurso de proteção avançada contra ameaças (proteção contra vírus e software, configuração de URL maliciosa, rastreamento de URL, etc.). |
  
* **[Leitor de segurança](#security-reader)**: os utilizadores com esta função têm acesso global só de leitura, incluindo todas as informações no Azure Active Directory, Identity Protection, Privileged Identity Management, bem como a capacidade de leitura do Azure Active Directory relatórios de início de sessão e registos de auditoria. A função também concede permissão só de leitura no Centro de conformidade e segurança do Office 365. Obter mais informações sobre as permissões do Office 365 estão disponíveis em [permissões no Centro de conformidade de segurança do Office 365 e](https://support.office.com/article/Permissions-in-the-Office-365-Security-Compliance-Center-d10608af-7934-490a-818e-e68f17d0e9c1).

  | Em | Pode fazer |
  | --- | --- |
  | Identity Protection Center |Ler todos os relatórios de segurança e informações das definições de funcionalidades de segurança<ul><li>Anti-spam<li>Encriptação<li>Prevenção de perda de dados<li>Antimalware<li>Proteção avançada contra ameaças<li>Anti-phishing<li>Regras de fluxo de mensagens |
  | Privileged Identity Management |<p>Tem acesso só de leitura para todas as informações apresentadas no Azure AD PIM: as políticas e relatórios para atribuições de funções do Azure AD, revisões de segurança e acesso aos dados de política e relatórios para cenários, além de atribuição de função do Azure AD de leitura no futuro.<p>**Não é possível** inscrever-se no PIM do Azure AD ou fazer alterações ao mesmo. No portal do PIM ou através do PowerShell, alguém nesta função pode ativar as funções adicionais (por exemplo, Administrador Global ou administrador com função privilegiada), se o utilizador é um candidato para eles. |
  | <p>O estado de funcionamento do monitor do Office 365 serviço</p><p>Centro de Segurança e Conformidade do Office 365</p> |<ul><li>Leitura e gerir alertas<li>Políticas de segurança de leitura<li>Ler informações sobre ameaças, deteção de aplicações na Cloud e quarentena em pesquisa e investigar<li>Ler todos os relatórios |

* **[O administrador de suporte do serviço](#service-support-administrator)**: os utilizadores com esta função podem abrir pedidos de suporte com a Microsoft para serviços do Azure e do Office 365 e vistas do dashboard de serviço e a mensagem center no portal do Azure e o portal de administração do Office 365. Mais informações em [funções de administrador sobre o Office 365](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

* **[Administrador de serviços do SharePoint](#sharepoint-service-administrator)**: os utilizadores com esta função possuem permissões globais dentro do Microsoft SharePoint Online, quando o serviço está presente, bem como a capacidade para gerir pedidos de suporte e monitorizar o estado de funcionamento do serviço. Mais informações em [funções de administrador sobre o Office 365](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

* **[Skype para empresas / administrador do serviço Lync](#lync-service-administrator)**: os utilizadores com esta função têm permissões globais no Microsoft Skype para empresas, quando o serviço está presente, bem como gerir os atributos de utilizador do Skype específico no Azure Active Diretório. Além disso, esta função concede a capacidade para gerir pedidos de suporte e monitorizar o estado de funcionamento do serviço e para acessar as Equipes e o Skype para empresas do Centro de administração. A conta também têm de estar licenciada para equipas ou não é possível executar cmdlets do PowerShell de Equipes. Mais informações em [sobre o Skype para a função de administrador de negócios](https://support.office.com/article/about-the-skype-for-business-admin-role-aeb35bda-93fc-49b1-ac2c-c74fbeb737b5) e as equipas de informações de licenciamento em [Skype para empresas e o Microsoft Teams suplemento licenciamento](https://docs.microsoft.com/skypeforbusiness/skype-for-business-and-microsoft-teams-add-on-licensing/skype-for-business-and-microsoft-teams-add-on-licensing)

  > [!NOTE]
  > No Microsoft Graph API, o Azure AD Graph API e o Azure AD PowerShell, esta função é identificada como "Administrador do serviço Lync". É "Skype para empresas administrador de serviços" na [portal do Azure](https://portal.azure.com/).
  >
  >

* **[As equipes de administrador de comunicações](#teams-communications-administrator)**: os utilizadores nesta função podem gerir os aspetos da carga de trabalho do Microsoft Teams relacionadas com a voz e telefonia. Isto inclui as ferramentas de gestão para atribuição de número de telefone, diretivas de voz e de reunião e acesso total para o conjunto de ferramentas de análise de chamada.

* **[As equipes de engenheiro de suporte de comunicações](#teams-communications-support-engineer)**: os utilizadores nesta função podem resolver problemas de comunicação dentro do Microsoft Teams e o Skype para empresas com o usuário chamam ferramentas de solução de problemas no Microsoft Teams & Skype para Centro de administração de negócios. Os utilizadores nesta função podem ver informações de registo de chamadas completa para todos os participantes envolvidos.

* **[As equipes especialistas de suporte de comunicações](#teams-communications-support-specialist)**: os utilizadores nesta função podem resolver problemas de comunicação dentro do Microsoft Teams e o Skype para empresas com o usuário chamam ferramentas de solução de problemas no Microsoft Teams & Skype para Centro de administração de negócios. Os utilizadores nesta função apenas podem ver os detalhes de utilizador na chamada para o usuário específico que tem procurado.

* **[As equipes de administrador de serviços](#teams-service-administrator)**: os utilizadores nesta função podem gerir todos os aspetos da carga de trabalho Microsoft Teams através do Skype e Microsoft Teams para o Centro de administração de negócios e os respectivos módulos do PowerShell. Isto inclui, entre outras áreas, todas as ferramentas de gestão relacionados com a telefonia, mensagens, reuniões e as equipes propriamente ditas. Esta função também concede a capacidade para gerir grupos do Office 365.

* **[Administrador de conta de utilizador](#user-account-administrator)**: os utilizadores com esta função podem criar e gerir todos os aspetos de utilizadores e grupos. Além disso, esta função inclui a capacidade para gerir pedidos de suporte e monitorizar o estado de funcionamento do serviço. Aplicam-se algumas restrições. Por exemplo, esta função não permite a eliminação de um administrador global. Os administradores de conta de utilizador podem alterar as palavras-passe e invalidar os tokens de atualização para os utilizadores, administradores de suporte técnico e outros administradores de conta de utilizador apenas. Invalidar um token de atualização força o utilizador iniciar sessão novamente.

| Pode fazer | Não é possível fazer |
| --- | --- |
| <p>Ver informações de utilizador e da empresa</p><p>Gerir pedidos de suporte do Office</p><p>Alterar palavras-passe para utilizadores, administradores de suporte técnico e outros administradores de conta de utilizador apenas</p><p>Criar e gerir vistas de utilizador</p><p>Criar, editar, eliminar utilizadores e grupos e gerir licenças de utilizador, com limitações. Ele não pode eliminar um administrador global nem criar outros administradores.</p> |<p>Efetuar operações de faturas e compras de produtos do Office</p><p>Gerir domínios</p><p>Gerir as informações da empresa</p><p>Delegar funções administrativas a outras pessoas</p><p>Utilizar a sincronização de diretórios</p><p>Ativar ou desativar a autenticação multifator</p><p>Ver registos de auditoria</p> |

## <a name="deprecated-roles"></a>Funções preteridas

As seguintes funções não devem ser utilizadas. Eles foi despromovida e será removida do Azure AD no futuro.

* Administrador de Licenças AdHoc
* Associação de dispositivo
* Gestores de Dispositivo
* Utilizadores de dispositivos
* Criador de Utilizador Verificado por E-mail
* Administrador da Caixa de Correio
* Associação de Dispositivos da Área de Trabalho

## <a name="detailed-azure-active-directory-permissions"></a>Detalhadas permissões do Active Directory do Azure
As tabelas seguintes descrevem as permissões específicas no Azure Active Directory tendo em conta a cada função. Algumas funções, como o Administrador Global, podem ter permissões adicionais no Microsoft outide de serviços do Azure Active Directory.


### <a name="adhoc-license-administrator"></a>Administrador de Licenças AdHoc
Pode criar e gerir todos os aspetos de registos de aplicações e aplicações empresariais.

  > [!NOTE]
  > Esta função herda permissões adicionais da função leitores de diretório.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| Microsoft.aad.Directory/Domains/default/Read | Ler as propriedades básicas sobre domínios no Azure Active Directory. |
| microsoft.aad.directory/groups/appRoleAssignments/read | Leia a propriedade groups.appRoleAssignments no Azure Active Directory. |
| Microsoft.aad.Directory/Groups/default/Read | Ler as propriedades básicas sobre os grupos no Azure Active Directory. |
| microsoft.aad.directory/groups/memberOf/read | Leia a propriedade memberof no Azure Active Directory. |
| Microsoft.aad.Directory/Groups/members/Read | Leia a propriedade members no Azure Active Directory. |
| Microsoft.aad.Directory/Groups/Owners/Read | Leia a propriedade Owners no Azure Active Directory. |
| Microsoft.aad.Directory/Groups/Settings/Read | Leia a propriedade groups.settings no Azure Active Directory. |
| microsoft.aad.directory/oAuth2PermissionGrants/default/read | Ler as propriedades básicas em oAuth2PermissionGrants no Azure Active Directory. |
| microsoft.aad.directory/oAuth2PermissionGrants/update | Atualize oAuth2PermissionGrants no Azure Active Directory. |
| Microsoft.aad.Directory/Organization/default/Read | Ler as propriedades básicas na organização no Azure Active Directory. |
| microsoft.aad.directory/organization/trustedCAsForPasswordlessAuth/read | Leia a propriedade organization.trustedCAsForPasswordlessAuth no Azure Active Directory. |
| microsoft.aad.directory/users/assignLicense | Gira licenças em utilizadores no Azure Active Directory. |
| microsoft.aad.directory/users/appRoleAssignments/read | Leia a propriedade approleassignments no Azure Active Directory. |
| Microsoft.aad.Directory/Users/Default/Read | Ler as propriedades básicas sobre os utilizadores no Azure Active Directory. |
| microsoft.aad.directory/users/directReports/read | Leia a propriedade directreports no Azure Active Directory. |
| microsoft.aad.directory/users/invitedBy/read | Leia a propriedade invitedby no Azure Active Directory. |
| microsoft.aad.directory/users/invitedUsers/read | Leia a propriedade invitedusers no Azure Active Directory. |
| Microsoft.aad.Directory/Users/Manager/Read | Leia a propriedade Users no Azure Active Directory. |
| microsoft.aad.directory/users/memberOf/read | Leia a propriedade memberof no Azure Active Directory. |
| microsoft.aad.directory/users/oAuth2PermissionGrants/default/read | Leia a propriedade users.oAuth2PermissionGrants no Azure Active Directory. |
| microsoft.aad.directory/users/ownedDevices/read | Leia a propriedade owneddevices no Azure Active Directory. |
| microsoft.aad.directory/users/ownedObjects/read | Leia a propriedade ownedobjects no Azure Active Directory. |
| microsoft.aad.directory/users/registeredDevices/read | Leia a propriedade de registereddevices no Azure Active Directory. |

### <a name="application-administrator"></a>Administrador de Aplicações
Pode criar e gerir todos os aspetos de registos de aplicações e aplicações empresariais.

  > [!NOTE]
  > Esta função herda permissões adicionais da função leitores de diretório.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| Microsoft.aad.Directory/Applications/Audience/Update | Atualize a propriedade applications.audience no Azure Active Directory. |
| Microsoft.aad.Directory/Applications/Authentication/Update | Atualize a propriedade applications.authentication no Azure Active Directory. |
| Microsoft.aad.Directory/Applications/default/Update | Atualize as propriedades básicas em aplicações no Azure Active Directory. |
| Microsoft.aad.Directory/Applications/Create | Crie aplicações no Azure Active Directory. |
| Microsoft.aad.Directory/Applications/credentials/Update | Atualize a propriedade applications.credentials no Azure Active Directory. |
| Microsoft.aad.Directory/Applications/DELETE | Elimine aplicações no Azure Active Directory. |
| Microsoft.aad.Directory/Applications/Owners/Update | Atualize a propriedade Owners no Azure Active Directory. |
| Microsoft.aad.Directory/Applications/Permissions/Update | Atualize a propriedade applications.permissions no Azure Active Directory. |
| Microsoft.aad.Directory/Applications/Policies/Update | Atualize a propriedade applications.policies no Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/create | Crie appRoleAssignments no Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/read | Leia appRoleAssignments no Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/update | Atualize appRoleAssignments no Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/delete | Elimine appRoleAssignments no Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/default/read | Leia a propriedade policies.applicationConfiguration no Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/default/update | Atualize a propriedade policies.applicationConfiguration no Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/create | Crie políticas no Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/delete | Elimine políticas no Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/owners/read | Leia a propriedade policies.applicationConfiguration no Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/owners/update | Atualize a propriedade policies.applicationConfiguration no Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/policyAppliedTo/read | Leia a propriedade policies.applicationConfiguration no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/default/update | Atualize as propriedades básicas no servicePrincipals no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/create | Crie servicePrincipals no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/delete | Elimine servicePrincipals no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/update | Atualize a propriedade Approleassignedto no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/update | Atualize a propriedade Serviceprincipals no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/owners/update | Atualize a propriedade Serviceprincipals no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/policies/update | Atualize a propriedade servicePrincipals.policies no Azure Active Directory. |
| microsoft.aad.directory/users/assignLicense | Gira licenças em utilizadores no Azure Active Directory. |
| microsoft.aad.reports/allEntities/read | Leia Relatórios do Azure AD. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Ler e configurar o Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Criar e gerir pedidos de suporte do Azure. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Leia e configure o Estado de Funcionamento do Serviço Office 365. |
| Microsoft.office365.supportTickets/allEntities/allTasks | Crie e gira pedidos de suporte do Office 365. |

### <a name="application-developer"></a>Programador de Aplicações
Pode criar registos de aplicação, independentemente dos utilizadores pode registar-se a definição de aplicações.

  > [!NOTE]
  > Esta função herda permissões adicionais da função leitores de diretório.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.aad.directory/applications/createAsOwner | Crie aplicações no Azure Active Directory. Criador é adicionado como o primeiro proprietário e o objeto criado conta contra a quota de 250 objetos criados do criador. |
| microsoft.aad.directory/appRoleAssignments/createAsOwner | Crie appRoleAssignments no Azure Active Directory. Criador é adicionado como o primeiro proprietário e o objeto criado conta contra a quota de 250 objetos criados do criador. |
| microsoft.aad.directory/oAuth2PermissionGrants/createAsOwner | Crie oAuth2PermissionGrants no Azure Active Directory. Criador é adicionado como o primeiro proprietário e o objeto criado conta contra a quota de 250 objetos criados do criador. |
| microsoft.aad.directory/servicePrincipals/createAsOwner | Crie servicePrincipals no Azure Active Directory. Criador é adicionado como o primeiro proprietário e o objeto criado conta contra a quota de 250 objetos criados do criador. |

### <a name="billing-administrator"></a>Administrador de Faturação
Pode executar tarefas de faturação comuns, como atualizar as informações de pagamento.

  > [!NOTE]
  > Esta função herda permissões adicionais da função leitores de diretório.
  >
  >

  > [!NOTE]
  > Esta função tem permissões de addditonal fora do Azure Active Directory. Ver Descrição da função acima para obter mais informações.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| Microsoft.aad.Directory/Organization/default/Update | Atualize as propriedades básicas na organização no Azure Active Directory. |
| microsoft.aad.directory/organization/trustedCAsForPasswordlessAuth/update | Atualize a propriedade organization.trustedCAsForPasswordlessAuth no Azure Active Directory. |
| microsoft.azure.accessService/allEntities/allTasks | Gerir todos os aspetos do serviço de acesso do Azure. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Ler e configurar o Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Criar e gerir pedidos de suporte do Azure. |
| microsoft.commerce.billing/allEntities/allTasks | Gerir todos os aspetos da faturação do Office 365. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Leia e configure o Estado de Funcionamento do Serviço Office 365. |
| Microsoft.office365.supportTickets/allEntities/allTasks | Crie e gira pedidos de suporte do Office 365. |

### <a name="cloud-application-administrator"></a>Administrador da Aplicação Cloud
Pode criar e gerir todos os aspetos de registos de aplicações e aplicações empresariais, exceto o Proxy de Aplicações.

  > [!NOTE]
  > Esta função herda permissões adicionais da função leitores de diretório.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| Microsoft.aad.Directory/Applications/Audience/Update | Atualize a propriedade applications.audience no Azure Active Directory. |
| Microsoft.aad.Directory/Applications/Authentication/Update | Atualize a propriedade applications.authentication no Azure Active Directory. |
| Microsoft.aad.Directory/Applications/default/Update | Atualize as propriedades básicas em aplicações no Azure Active Directory. |
| Microsoft.aad.Directory/Applications/Create | Crie aplicações no Azure Active Directory. |
| Microsoft.aad.Directory/Applications/credentials/Update | Atualize a propriedade applications.credentials no Azure Active Directory. |
| Microsoft.aad.Directory/Applications/DELETE | Elimine aplicações no Azure Active Directory. |
| Microsoft.aad.Directory/Applications/Owners/Update | Atualize a propriedade Owners no Azure Active Directory. |
| Microsoft.aad.Directory/Applications/Permissions/Update | Atualize a propriedade applications.permissions no Azure Active Directory. |
| Microsoft.aad.Directory/Applications/Policies/Update | Atualize a propriedade applications.policies no Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/create | Crie appRoleAssignments no Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/update | Atualize appRoleAssignments no Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/delete | Elimine appRoleAssignments no Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/create | Crie políticas no Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/default/read | Leia a propriedade policies.applicationConfiguration no Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/default/update | Atualize a propriedade policies.applicationConfiguration no Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/delete | Elimine políticas no Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/owners/read | Leia a propriedade policies.applicationConfiguration no Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/owners/update | Atualize a propriedade policies.applicationConfiguration no Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/policyAppliedTo/read | Leia a propriedade policies.applicationConfiguration no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/update | Atualize a propriedade Approleassignedto no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/update | Atualize a propriedade Serviceprincipals no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/default/update | Atualize as propriedades básicas no servicePrincipals no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/create | Crie servicePrincipals no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/delete | Elimine servicePrincipals no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/owners/update | Atualize a propriedade Serviceprincipals no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/policies/update | Atualize a propriedade servicePrincipals.policies no Azure Active Directory. |
| microsoft.aad.directory/users/assignLicense | Gira licenças em utilizadores no Azure Active Directory. |
| microsoft.aad.reports/allEntities/read | Leia Relatórios do Azure AD. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Ler e configurar o Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Criar e gerir pedidos de suporte do Azure. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Leia e configure o Estado de Funcionamento do Serviço Office 365. |
| Microsoft.office365.supportTickets/allEntities/allTasks | Crie e gira pedidos de suporte do Office 365. |

### <a name="cloud-device-administrator"></a>Administrador de dispositivos de cloud
Acesso total para gerir dispositivos no Azure AD.

  > [!NOTE]
  > Esta função herda permissões adicionais da função leitores de diretório.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| Microsoft.aad.Directory/Devices/DELETE | Elimine dispositivos no Azure Active Directory. |
| Microsoft.aad.Directory/Devices/disable | Desative dispositivos no Azure Active Directory. |
| Microsoft.aad.Directory/Devices/Enable | Permitir que os dispositivos no Azure Active Directory. |
| microsoft.aad.reports/allEntities/read | Leia Relatórios do Azure AD. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Ler e configurar o Azure Service Health. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Leia e configure o Estado de Funcionamento do Serviço Office 365. |

### <a name="company-administrator"></a>Administrador de Empresa
Pode gerir todos os aspetos dos serviços Microsoft e Azure AD que utilizam identidades do Azure AD.

  > [!NOTE]
  > Esta função herda permissões adicionais da função.
  >
  >

  > [!NOTE]
  > Esta função tem permissões de addditonal fora do Azure Active Directory. Ver Descrição da função acima para obter mais informações.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.aad.directory/administrativeUnits/allProperties/allTasks | Crie e elimine administrativeUnits e leia e atualize todas as propriedades no Azure Active Directory. |
| microsoft.aad.directory/applications/allProperties/allTasks | Criar e eliminar aplicações e leia e atualize todas as propriedades no Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/allProperties/allTasks | Criar e eliminar appRoleAssignments e leia e atualize todas as propriedades no Azure Active Directory. |
| microsoft.aad.directory/contacts/allProperties/allTasks | Crie e elimine contactos e leia e atualize todas as propriedades no Azure Active Directory. |
| microsoft.aad.directory/contracts/allProperties/allTasks | Crie e elimine contratos e leia e atualize todas as propriedades no Azure Active Directory. |
| microsoft.aad.directory/devices/allProperties/allTasks | Criar e eliminar dispositivos e leia e atualize todas as propriedades no Azure Active Directory. |
| microsoft.aad.directory/directoryRoles/allProperties/allTasks | Criar e eliminar directoryRoles e ler e atualizar todas as propriedades no Azure Active Directory. |
| microsoft.aad.directory/directoryRoleTemplates/allProperties/allTasks | Crie e elimine directoryRoleTemplates e leia e atualize todas as propriedades no Azure Active Directory. |
| microsoft.aad.directory/domains/allProperties/allTasks | Crie e elimine domínios e leia e atualize todas as propriedades no Azure Active Directory. |
| microsoft.aad.directory/groups/allProperties/allTasks | Crie e elimine grupos e leia e atualize todas as propriedades no Azure Active Directory. |
| microsoft.aad.directory/groupSettings/allProperties/allTasks | Crie e elimine groupSettings e leia e atualize todas as propriedades no Azure Active Directory. |
| microsoft.aad.directory/groupSettingTemplates/allProperties/allTasks | Crie e elimine groupSettingTemplates e leia e atualize todas as propriedades no Azure Active Directory. |
| microsoft.aad.directory/loginTenantBranding/allProperties/allTasks | Crie e elimine loginTenantBranding e leia e atualize todas as propriedades no Azure Active Directory. |
| microsoft.aad.directory/oAuth2PermissionGrants/allProperties/allTasks | Criar e eliminar oAuth2PermissionGrants e ler e atualizar todas as propriedades no Azure Active Directory. |
| microsoft.aad.directory/organization/allProperties/allTasks | Crie e elimine organização e leia e atualize todas as propriedades no Azure Active Directory. |
| microsoft.aad.directory/policies/allProperties/allTasks | Criar e eliminar políticas e leia e atualize todas as propriedades no Azure Active Directory. |
| microsoft.aad.directory/roleAssignments/allProperties/allTasks | Crie e elimine RoleAssignments fosse e leia e atualize todas as propriedades no Azure Active Directory. |
| microsoft.aad.directory/roleDefinitions/allProperties/allTasks | Crie e elimine roleDefinitions e leia e atualize todas as propriedades no Azure Active Directory. |
| microsoft.aad.directory/scopedRoleMemberships/allProperties/allTasks | Crie e elimine scopedRoleMemberships e leia e atualize todas as propriedades no Azure Active Directory. |
| microsoft.aad.directory/serviceAction/activateService | Pode efetuar a ação de serviço Activateservice no Azure Active Directory |
| microsoft.aad.directory/serviceAction/disableDirectoryFeature | Pode efetuar a ação de serviço Disabledirectoryfeature no Azure Active Directory |
| microsoft.aad.directory/serviceAction/enableDirectoryFeature | Pode efetuar a ação de serviço Enabledirectoryfeature no Azure Active Directory |
| microsoft.aad.directory/serviceAction/getAvailableExtentionProperties | Pode efetuar a ação de serviço Getavailableextentionproperties no Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/allProperties/allTasks | Crie e elimine servicePrincipals e leia e atualize todas as propriedades no Azure Active Directory. |
| microsoft.aad.directory/subscribedSkus/allProperties/allTasks | Crie e elimine subscribedSkus e leia e atualize todas as propriedades no Azure Active Directory. |
| microsoft.aad.directory/users/allProperties/allTasks | Crie e elimine utilizadores e leia e atualize todas as propriedades no Azure Active Directory. |
| microsoft.aad.directorySync/allEntities/allTasks | Execute todas as ações no Azure AD Connect. |
| microsoft.aad.identityProtection/allEntities/allTasks | Crie e elimine todos os recursos e leia e atualize propriedades padrão no microsoft.aad.identityProtection. |
| microsoft.aad.privilegedIdentityManagement/allEntities/read | Ler todos os recursos no microsoft.aad.privilegedIdentityManagement. |
| microsoft.aad.reports/allEntities/allTasks | Leia e configure Relatórios do Azure AD. |
| microsoft.azure.accessService/allEntities/allTasks | Gerir todos os aspetos do serviço de acesso do Azure. |
| microsoft.azure.informationProtection/allEntities/allTasks | Gerir todos os aspetos do Azure Information Protection. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Ler e configurar o Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Criar e gerir pedidos de suporte do Azure. |
| microsoft.commerce.billing/allEntities/allTasks | Gerir todos os aspetos da faturação do Office 365. |
| microsoft.intune/allEntities/allTasks | Gerir todos os aspetos do Intune. |
| Microsoft.office365.complianceManager/allEntities/allTasks | Gerir todos os aspectos do Gestor de conformidade do Office 365 |
| Microsoft.office365.Exchange/allEntities/allTasks | Gerir todos os aspetos do Exchange Online. |
| Microsoft.office365.lockbox/allEntities/allTasks | Gerir todos os aspectos do Cofre de cliente do Office 365 |
| microsoft.powerApps.powerBI/allEntities/allTasks | Gerir todos os aspetos do Power BI. |
| Microsoft.office365.protectionCenter/allEntities/allTasks | Gerir todos os aspetos do Centro de proteção do Office 365. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Leia e configure o Estado de Funcionamento do Serviço Office 365. |
| Microsoft.office365.SharePoint/allEntities/allTasks | Crie e elimine todos os recursos e leia e atualize propriedades padrão no microsoft.office365.sharepoint. |
| Microsoft.office365.skypeForBusiness/allEntities/allTasks | Gerir todos os aspetos do Skype para empresas Online. |
| Microsoft.office365.supportTickets/allEntities/allTasks | Crie e gira pedidos de suporte do Office 365. |
| microsoft.powerApps.dynamics365/allEntities/allTasks | Gerir todos os aspetos do Dynamics 365. |

### <a name="compliance-administrator"></a>Administrador de Conformidade
Pode ler e gerir a configuração de conformidade e os relatórios no Azure AD e no Office 365.

  > [!NOTE]
  > Esta função herda permissões adicionais da função leitores de diretório.
  >
  >

  > [!NOTE]
  > Esta função tem permissões de addditonal fora do Azure Active Directory. Ver Descrição da função acima para obter mais informações.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | Gerir todos os aspetos do serviço de acesso do Azure. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Ler e configurar o Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Criar e gerir pedidos de suporte do Azure. |
| Microsoft.office365.complianceManager/allEntities/allTasks | Gerir todos os aspectos do Gestor de conformidade do Office 365 |
| Microsoft.office365.Exchange/allEntities/allTasks | Gerir todos os aspetos do Exchange Online. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Leia e configure o Estado de Funcionamento do Serviço Office 365. |
| Microsoft.office365.SharePoint/allEntities/allTasks | Crie e elimine todos os recursos e leia e atualize propriedades padrão no microsoft.office365.sharepoint. |
| Microsoft.office365.skypeForBusiness/allEntities/allTasks | Gerir todos os aspetos do Skype para empresas Online. |
| Microsoft.office365.supportTickets/allEntities/allTasks | Crie e gira pedidos de suporte do Office 365. |

### <a name="conditional-access-administrator"></a>Administrador de Acesso Condicional
Pode gerir capacidades de acesso condicional.

  > [!NOTE]
  > Esta função herda permissões adicionais da função leitores de diretório.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.aad.directory/policies/conditionalAccess/default/read | Leia a propriedade policies.conditionalAccess no Azure Active Directory. |
| microsoft.aad.directory/policies/conditionalAccess/default/update | Atualize a propriedade policies.conditionalAccess no Azure Active Directory. |
| microsoft.aad.directory/policies/conditionalAccess/create | Crie políticas no Azure Active Directory. |
| microsoft.aad.directory/policies/conditionalAccess/delete | Elimine políticas no Azure Active Directory. |
| microsoft.aad.directory/policies/conditionalAccess/owners/read | Leia a propriedade policies.conditionalAccess no Azure Active Directory. |
| microsoft.aad.directory/policies/conditionalAccess/owners/update | Atualize a propriedade policies.conditionalAccess no Azure Active Directory. |
| microsoft.aad.directory/policies/conditionalAccess/policiesAppliedTo/read | Leia a propriedade policies.conditionalAccess no Azure Active Directory. |

### <a name="crm-service-administrator"></a>Administrador de Serviço CRM
Pode gerir todos os aspetos do produto Dynamics 365.

  > [!NOTE]
  > Esta função herda permissões adicionais da função leitores de diretório.
  >
  >

  > [!NOTE]
  > Esta função tem permissões de addditonal fora do Azure Active Directory. Ver Descrição da função acima para obter mais informações.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | Gerir todos os aspetos do serviço de acesso do Azure. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Ler e configurar o Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Criar e gerir pedidos de suporte do Azure. |
| microsoft.powerApps.dynamics365/allEntities/allTasks | Gerir todos os aspetos do Dynamics 365. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Leia e configure o Estado de Funcionamento do Serviço Office 365. |
| Microsoft.office365.supportTickets/allEntities/allTasks | Crie e gira pedidos de suporte do Office 365. |

### <a name="customer-lockbox-access-approver"></a>Aprovador de Acesso ao Cofre de Cliente
Pode aprovar pedidos de suporte da Microsoft para aceder aos dados organizacionais de clientes.

  > [!NOTE]
  > Esta função herda permissões adicionais da função leitores de diretório.
  >
  >

  > [!NOTE]
  > Esta função tem permissões de addditonal fora do Azure Active Directory. Ver Descrição da função acima para obter mais informações.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | Gerir todos os aspetos do serviço de acesso do Azure. |
| Microsoft.office365.lockbox/allEntities/allTasks | Gerir todos os aspectos do Cofre de cliente do Office 365 |

### <a name="device-administrators"></a>Administradores de Dispositivo
Os membros desta função são adicionados ao grupo Administradores local em dispositivos associados ao AD Azure.

  > [!NOTE]
  > Esta função herda permissões adicionais da função leitores de diretório.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.aad.directory/groupSettings/default/read | Ler as propriedades básicas no groupSettings no Azure Active Directory. |
| microsoft.aad.directory/groupSettingTemplates/default/read | Ler as propriedades básicas no groupSettingTemplates no Azure Active Directory. |

### <a name="device-managers"></a>Gestores de Dispositivo
Pode aprovar pedidos de suporte da Microsoft para aceder aos dados organizacionais de clientes.

  > [!NOTE]
  > Esta função herda permissões adicionais da função leitores de diretório.
  >
  >

  > [!NOTE]
  > Esta função tem permissões de addditonal fora do Azure Active Directory. Ver Descrição da função acima para obter mais informações.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| Microsoft.aad.Directory/Devices/default/Read | Ler as propriedades básicas em dispositivos no Azure Active Directory. |
| Microsoft.aad.Directory/Devices/default/Update | Atualize as propriedades básicas em dispositivos no Azure Active Directory. |
| microsoft.aad.directory/devices/memberOf/read | Leia a propriedade memberof no Azure Active Directory. |
| microsoft.aad.directory/devices/registeredOwners/read | Leia a propriedade devices.registeredOwners no Azure Active Directory. |
| microsoft.aad.directory/devices/registeredOwners/update | Atualize a propriedade devices.registeredOwners no Azure Active Directory. |
| microsoft.aad.directory/devices/registeredUsers/read | Leia a propriedade devices.registeredUsers no Azure Active Directory. |
| microsoft.aad.directory/devices/registeredUsers/update | Atualize a propriedade devices.registeredUsers no Azure Active Directory. |

### <a name="directory-readers"></a>Leitores de Diretório
Pode ler as informações de diretório básicas. Para conceder acesso a aplicações

  > [!NOTE]
  > Esta função herda permissões adicionais da função.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.aad.directory/administrativeUnits/default/read | Ler as propriedades básicas no administrativeUnits no Azure Active Directory. |
| microsoft.aad.directory/administrativeUnits/members/read | Leia a propriedade Administrativeunits no Azure Active Directory. |
O Azure Active Directory. |
| Microsoft.aad.Directory/Applications/default/Read | Ler as propriedades básicas em aplicações no Azure Active Directory. |
| Microsoft.aad.Directory/Applications/Owners/Read | Leia a propriedade Owners no Azure Active Directory. |
| Microsoft.aad.Directory/Contacts/default/Read | Ler as propriedades básicas para contactos no Azure Active Directory. |
| microsoft.aad.directory/contacts/memberOf/read | Leia a propriedade contacts.memberOf no Azure Active Directory. |
| Microsoft.aad.Directory/Contracts/default/Read | Ler as propriedades básicas sobre contratos no Azure Active Directory. |
| Microsoft.aad.Directory/Devices/default/Read | Ler as propriedades básicas em dispositivos no Azure Active Directory. |
| microsoft.aad.directory/devices/memberOf/read | Leia a propriedade memberof no Azure Active Directory. |
| microsoft.aad.directory/devices/registeredOwners/read | Leia a propriedade devices.registeredOwners no Azure Active Directory. |
| microsoft.aad.directory/devices/registeredUsers/read | Leia a propriedade devices.registeredUsers no Azure Active Directory. |
| microsoft.aad.directory/directoryRoles/default/read | Ler as propriedades básicas no directoryRoles no Azure Active Directory. |
| microsoft.aad.directory/directoryRoles/eligibleMembers/read | Leia a propriedade directoryRoles.eligibleMembers no Azure Active Directory. |
| microsoft.aad.directory/directoryRoles/members/read | Leia a propriedade directoryRoles.members no Azure Active Directory. |
| Microsoft.aad.Directory/Domains/default/Read | Ler as propriedades básicas sobre domínios no Azure Active Directory. |
| microsoft.aad.directory/groups/appRoleAssignments/read | Leia a propriedade groups.appRoleAssignments no Azure Active Directory. |
| Microsoft.aad.Directory/Groups/default/Read | Ler as propriedades básicas sobre os grupos no Azure Active Directory. |
| microsoft.aad.directory/groups/memberOf/read | Leia a propriedade memberof no Azure Active Directory. |
| Microsoft.aad.Directory/Groups/members/Read | Leia a propriedade members no Azure Active Directory. |
| Microsoft.aad.Directory/Groups/Owners/Read | Leia a propriedade Owners no Azure Active Directory. |
| Microsoft.aad.Directory/Groups/Settings/Read | Leia a propriedade groups.settings no Azure Active Directory. |
| microsoft.aad.directory/groupSettings/default/read | Ler as propriedades básicas no groupSettings no Azure Active Directory. |
| microsoft.aad.directory/groupSettingTemplates/default/read | Ler as propriedades básicas no groupSettingTemplates no Azure Active Directory. |
| microsoft.aad.directory/oAuth2PermissionGrants/default/read | Ler as propriedades básicas em oAuth2PermissionGrants no Azure Active Directory. |
| Microsoft.aad.Directory/Organization/default/Read | Ler as propriedades básicas na organização no Azure Active Directory. |
| microsoft.aad.directory/organization/trustedCAsForPasswordlessAuth/read | Leia a propriedade organization.trustedCAsForPasswordlessAuth no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/read | Leia a propriedade Approleassignedto no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/read | Leia a propriedade Serviceprincipals no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/default/read | Ler as propriedades básicas no servicePrincipals no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/memberOf/read | Ler a propriedade Serviceprincipals no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/oAuth2PermissionGrants/default/read | Leia a propriedade servicePrincipals.oAuth2PermissionGrants no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/ownedObjects/read | Leia a propriedade Ownedobjects no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/owners/read | Leia a propriedade Serviceprincipals no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/policies/read | Leia a propriedade servicePrincipals.policies no Azure Active Directory. |
| microsoft.aad.directory/subscribedSkus/default/read | Ler as propriedades básicas no subscribedSkus no Azure Active Directory. |
| microsoft.aad.directory/users/appRoleAssignments/read | Leia a propriedade approleassignments no Azure Active Directory. |
| Microsoft.aad.Directory/Users/Default/Read | Ler as propriedades básicas sobre os utilizadores no Azure Active Directory. |
| microsoft.aad.directory/users/directReports/read | Leia a propriedade directreports no Azure Active Directory. |
| microsoft.aad.directory/users/invitedBy/read | Leia a propriedade invitedby no Azure Active Directory. |
| microsoft.aad.directory/users/invitedUsers/read | Leia a propriedade invitedusers no Azure Active Directory. |
| Microsoft.aad.Directory/Users/Manager/Read | Leia a propriedade Users no Azure Active Directory. |
| microsoft.aad.directory/users/memberOf/read | Leia a propriedade memberof no Azure Active Directory. |
| microsoft.aad.directory/users/oAuth2PermissionGrants/default/read | Leia a propriedade users.oAuth2PermissionGrants no Azure Active Directory. |
| microsoft.aad.directory/users/ownedDevices/read | Leia a propriedade owneddevices no Azure Active Directory. |
| microsoft.aad.directory/users/ownedObjects/read | Leia a propriedade ownedobjects no Azure Active Directory. |
| microsoft.aad.directory/users/registeredDevices/read | Leia a propriedade de registereddevices no Azure Active Directory. |

### <a name="directory-synchronization-accounts"></a>Contas de sincronização de diretório
Só é utilizada pelo serviço do Azure AD Connect.

  > [!NOTE]
  > Esta função herda permissões adicionais da função.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.aad.directory/organization/dirSync/update | Atualize a propriedade organization.dirSync no Azure Active Directory. |
| Microsoft.aad.Directory/Policies/Create | Crie políticas no Azure Active Directory. |
| Microsoft.aad.Directory/Policies/DELETE | Elimine políticas no Azure Active Directory. |
| Microsoft.aad.Directory/Policies/default/Read | Ler as propriedades básicas nas políticas no Azure Active Directory. |
| Microsoft.aad.Directory/Policies/default/Update | Atualize as propriedades básicas em políticas no Azure Active Directory. |
| Microsoft.aad.Directory/Policies/Owners/Read | Leia a propriedade policies.owners no Azure Active Directory. |
| Microsoft.aad.Directory/Policies/Owners/Update | Atualize a propriedade policies.owners no Azure Active Directory. |
| microsoft.aad.directory/policies/policiesAppliedTo/read | Leia a propriedade policies.policiesAppliedTo no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/read | Leia a propriedade Approleassignedto no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/update | Atualize a propriedade Approleassignedto no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/read | Leia a propriedade Serviceprincipals no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/update | Atualize a propriedade Serviceprincipals no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/default/read | Ler as propriedades básicas no servicePrincipals no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/default/update | Atualize as propriedades básicas no servicePrincipals no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/create | Crie servicePrincipals no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/memberOf/read | Ler a propriedade Serviceprincipals no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/oAuth2PermissionGrants/default/read | Leia a propriedade servicePrincipals.oAuth2PermissionGrants no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/owners/read | Leia a propriedade Serviceprincipals no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/owners/update | Atualize a propriedade Serviceprincipals no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/ownedObjects/read | Leia a propriedade Ownedobjects no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/policies/read | Leia a propriedade servicePrincipals.policies no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/policies/update | Atualize a propriedade servicePrincipals.policies no Azure Active Directory. |
| microsoft.aad.directorySync/allEntities/allTasks | Execute todas as ações no Azure AD Connect. |

### <a name="directory-writers"></a>Escritores de Diretório
Pode ler e gravar informações de diretório básicas. Para conceder acesso a aplicações

  > [!NOTE]
  > Esta função herda permissões adicionais da função leitores de diretório.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| Microsoft.aad.Directory/Groups/Create | Crie grupos no Azure Active Directory. |
| microsoft.aad.directory/groups/createAsOwner | Crie grupos no Azure Active Directory. Criador é adicionado como o primeiro proprietário e o objeto criado conta contra a quota de 250 objetos criados do criador. |
| microsoft.aad.directory/groups/appRoleAssignments/update | Atualize a propriedade groups.appRoleAssignments no Azure Active Directory. |
| Microsoft.aad.Directory/Groups/default/Update | Atualize as propriedades básicas sobre os grupos no Azure Active Directory. |
| Microsoft.aad.Directory/Groups/members/Update | Atualize a propriedade members no Azure Active Directory. |
| Microsoft.aad.Directory/Groups/Owners/Update | Atualize a propriedade Owners no Azure Active Directory. |
| Microsoft.aad.Directory/Groups/Settings/Update | Atualize a propriedade groups.settings no Azure Active Directory. |
| microsoft.aad.directory/groupSettings/default/update | Atualize as propriedades básicas no groupSettings no Azure Active Directory. |
| microsoft.aad.directory/groupSettings/create | Crie groupSettings no Azure Active Directory. |
| microsoft.aad.directory/groupSettings/delete | Elimine groupSettings no Azure Active Directory. |
| microsoft.aad.directory/users/appRoleAssignments/update | Atualize a propriedade approleassignments no Azure Active Directory. |
| microsoft.aad.directory/users/assignLicense | Gira licenças em utilizadores no Azure Active Directory. |
| Microsoft.aad.Directory/Users/Default/Update | Atualize as propriedades básicas sobre os utilizadores no Azure Active Directory. |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Invalide todos os tokens de atualização de utilizador no Azure Active Directory. |
| Microsoft.aad.Directory/Users/Manager/Update | Atualize a propriedade Users no Azure Active Directory. |
| microsoft.aad.directory/users/userPrincipalName/update | Atualize a propriedade users.userPrincipalName no Azure Active Directory. |

### <a name="exchange-service-administrator"></a>Administrador de Serviço Exchange
Pode gerir todos os aspetos do produto Exchange.

  > [!NOTE]
  > Esta função herda permissões adicionais da função leitores de diretório.
  >
  >

  > [!NOTE]
  > Esta função tem permissões de addditonal fora do Azure Active Directory. Ver Descrição da função acima para obter mais informações.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | Gerir todos os aspetos do serviço de acesso do Azure. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Ler e configurar o Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Criar e gerir pedidos de suporte do Azure. |
| Microsoft.office365.Exchange/allEntities/allTasks | Gerir todos os aspetos do Exchange Online. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Leia e configure o Estado de Funcionamento do Serviço Office 365. |
| Microsoft.office365.supportTickets/allEntities/allTasks | Crie e gira pedidos de suporte do Office 365. |

### <a name="guest"></a>Convidado
Função predefinida para os utilizadores convidados. Pode ler um conjunto limitado de informações do diretório.

  > [!NOTE]
  > Esta função herda permissões adicionais da função.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| Microsoft.aad.Directory/Applications/default/Read | Ler as propriedades básicas em aplicações no Azure Active Directory. |
| Microsoft.aad.Directory/Applications/Owners/Read | Leia a propriedade Owners no Azure Active Directory. |
| Microsoft.aad.Directory/Domains/default/Read | Ler as propriedades básicas sobre domínios no Azure Active Directory. |
| microsoft.aad.directory/groups/appRoleAssignments/read | Leia a propriedade groups.appRoleAssignments no Azure Active Directory. |
| Microsoft.aad.Directory/Groups/default/Read | Ler as propriedades básicas sobre os grupos no Azure Active Directory. |
| microsoft.aad.directory/groups/memberOf/read | Leia a propriedade memberof no Azure Active Directory. |
| Microsoft.aad.Directory/Groups/members/Read | Leia a propriedade members no Azure Active Directory. |
| Microsoft.aad.Directory/Groups/Owners/Read | Leia a propriedade Owners no Azure Active Directory. |
| Microsoft.aad.Directory/Groups/Settings/Read | Leia a propriedade groups.settings no Azure Active Directory. |
| microsoft.aad.directory/organization/basicProfile/read | Leia as informações de perfil de organização básica no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/read | Leia a propriedade Approleassignedto no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/read | Leia a propriedade Serviceprincipals no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/default/read | Ler as propriedades básicas no servicePrincipals no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/memberOf/read | Ler a propriedade Serviceprincipals no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/members/read | Leia a propriedade servicePrincipals.members no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/oAuth2PermissionGrants/default/read | Leia a propriedade servicePrincipals.oAuth2PermissionGrants no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/owners/read | Leia a propriedade Serviceprincipals no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/ownedObjects/read | Leia a propriedade Ownedobjects no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/policies/read | Leia a propriedade servicePrincipals.policies no Azure Active Directory. |
| microsoft.aad.directory/users/basicProfile/read | Leia a propriedade basicprofile no Azure Active Directory. |
| microsoft.aad.directory/users/appRoleAssignments/read | Leia a propriedade approleassignments no Azure Active Directory. |
| Microsoft.aad.Directory/Users/Default/Read | Ler as propriedades básicas sobre os utilizadores no Azure Active Directory. |
| microsoft.aad.directory/users/directReports/read | Leia a propriedade directreports no Azure Active Directory. |
| microsoft.aad.directory/users/eligibleMemberOf/read | Leia a propriedade users.eligibleMemberOf no Azure Active Directory. |
| microsoft.aad.directory/users/invitedBy/read | Leia a propriedade invitedby no Azure Active Directory. |
| microsoft.aad.directory/users/invitedUsers/read | Leia a propriedade invitedusers no Azure Active Directory. |
| Microsoft.aad.Directory/Users/Manager/Read | Leia a propriedade Users no Azure Active Directory. |
| microsoft.aad.directory/users/memberOf/read | Leia a propriedade memberof no Azure Active Directory. |
| microsoft.aad.directory/users/oAuth2PermissionGrants/default/read | Leia a propriedade users.oAuth2PermissionGrants no Azure Active Directory. |
| microsoft.aad.directory/users/ownedDevices/read | Leia a propriedade owneddevices no Azure Active Directory. |
| microsoft.aad.directory/users/ownedObjects/read | Leia a propriedade ownedobjects no Azure Active Directory. |
| Microsoft.aad.Directory/Users/password/Update | Atualize as palavras-passe para todos os utilizadores no Azure Active Directory. Consulte a documentação online para obter mais detalhes. |
| microsoft.aad.directory/users/pendingMemberOf/read | Leia a propriedade users.pendingMemberOf no Azure Active Directory. |
| microsoft.aad.directory/users/registeredDevices/read | Leia a propriedade de registereddevices no Azure Active Directory. |
| microsoft.aad.directory/users/scopedAdministratorOf/read | Leia a propriedade users.scopedAdministratorOf no Azure Active Directory. |

### <a name="guest-inviter"></a>Utilizador Que Convida Convidados
Podem convidar os utilizadores convidados independentemente dos membros podem convidar convidados definição.

  > [!NOTE]
  > Esta função herda permissões adicionais da função.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.aad.directory/users/appRoleAssignments/read | Leia a propriedade approleassignments no Azure Active Directory. |
| Microsoft.aad.Directory/Users/Default/Read | Ler as propriedades básicas sobre os utilizadores no Azure Active Directory. |
| microsoft.aad.directory/users/directReports/read | Leia a propriedade directreports no Azure Active Directory. |
| microsoft.aad.directory/users/invitedBy/read | Leia a propriedade invitedby no Azure Active Directory. |
| microsoft.aad.directory/users/inviteGuest | Convide utilizadores no Azure Active Directory. |
| microsoft.aad.directory/users/invitedUsers/read | Leia a propriedade invitedusers no Azure Active Directory. |
| Microsoft.aad.Directory/Users/Manager/Read | Leia a propriedade Users no Azure Active Directory. |
| microsoft.aad.directory/users/memberOf/read | Leia a propriedade memberof no Azure Active Directory. |
| microsoft.aad.directory/users/oAuth2PermissionGrants/default/read | Leia a propriedade users.oAuth2PermissionGrants no Azure Active Directory. |
| microsoft.aad.directory/users/ownedDevices/read | Leia a propriedade owneddevices no Azure Active Directory. |
| microsoft.aad.directory/users/ownedObjects/read | Leia a propriedade ownedobjects no Azure Active Directory. |
| microsoft.aad.directory/users/registeredDevices/read | Leia a propriedade de registereddevices no Azure Active Directory. |

### <a name="helpdesk-administrator"></a>Administrador de Suporte Técnico
Pode repor palavras-passe para não administradores e Administradores de Suporte Técnico.

  > [!NOTE]
  > Esta função herda permissões adicionais da função leitores de diretório.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Invalide todos os tokens de atualização de utilizador no Azure Active Directory. |
| Microsoft.aad.Directory/Users/password/Update | Atualize as palavras-passe para todos os utilizadores no Azure Active Directory. Consulte a documentação online para obter mais detalhes. |
| microsoft.azure.accessService/allEntities/allTasks | Gerir todos os aspetos do serviço de acesso do Azure. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Ler e configurar o Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Criar e gerir pedidos de suporte do Azure. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Leia e configure o Estado de Funcionamento do Serviço Office 365. |
| Microsoft.office365.supportTickets/allEntities/allTasks | Crie e gira pedidos de suporte do Office 365. |

### <a name="information-protection-administrator"></a>Administrador do Information Protection
Pode gerir todos os aspetos do produto Azure Information Protection.

  > [!NOTE]
  > Esta função herda permissões adicionais da função leitores de diretório.
  >
  >

  > [!NOTE]
  > Esta função tem permissões de addditonal fora do Azure Active Directory. Ver Descrição da função acima para obter mais informações.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.azure.informationProtection/allEntities/allTasks | Gerir todos os aspetos do Azure Information Protection. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Ler e configurar o Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Criar e gerir pedidos de suporte do Azure. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Leia e configure o Estado de Funcionamento do Serviço Office 365. |
| Microsoft.office365.supportTickets/allEntities/allTasks | Crie e gira pedidos de suporte do Office 365. |

### <a name="intune-service-administrator"></a>Administrador de Serviço Intune
Pode gerir todos os aspetos do produto Intune.

  > [!NOTE]
  > Esta função herda permissões adicionais da função leitores de diretório.
  >
  >

  > [!NOTE]
  > Esta função tem permissões de addditonal fora do Azure Active Directory. Ver Descrição da função acima para obter mais informações.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| Microsoft.aad.Directory/Contacts/default/Update | Atualize as propriedades básicas para contactos no Azure Active Directory. |
| Microsoft.aad.Directory/Contacts/Create | Crie contactos no Azure Active Directory. |
| Microsoft.aad.Directory/Contacts/DELETE | Elimine contactos no Azure Active Directory. |
| Microsoft.aad.Directory/Devices/default/Update | Atualize as propriedades básicas em dispositivos no Azure Active Directory. |
| Microsoft.aad.Directory/Devices/Create | Crie dispositivos no Azure Active Directory. |
| Microsoft.aad.Directory/Devices/DELETE | Elimine dispositivos no Azure Active Directory. |
| microsoft.aad.directory/devices/registeredOwners/update | Atualize a propriedade devices.registeredOwners no Azure Active Directory. |
| microsoft.aad.directory/devices/registeredUsers/update | Atualize a propriedade devices.registeredUsers no Azure Active Directory. |
| microsoft.aad.directory/groups/appRoleAssignments/update | Atualize a propriedade groups.appRoleAssignments no Azure Active Directory. |
| Microsoft.aad.Directory/Groups/default/Update | Atualize as propriedades básicas sobre os grupos no Azure Active Directory. |
| Microsoft.aad.Directory/Groups/Create | Crie grupos no Azure Active Directory. |
| microsoft.aad.directory/groups/createAsOwner | Crie grupos no Azure Active Directory. Criador é adicionado como o primeiro proprietário e o objeto criado conta contra a quota de 250 objetos criados do criador. |
| Microsoft.aad.Directory/Groups/DELETE | Elimine grupos no Azure Active Directory. |
| microsoft.aad.directory/groups/hiddenMembers/read | Leia a propriedade groups.hiddenMembers no Azure Active Directory. |
| Microsoft.aad.Directory/Groups/members/Update | Atualize a propriedade members no Azure Active Directory. |
| Microsoft.aad.Directory/Groups/Owners/Update | Atualize a propriedade Owners no Azure Active Directory. |
| Microsoft.aad.Directory/Groups/Restore | Restaure grupos no Azure Active Directory. |
| Microsoft.aad.Directory/Groups/Settings/Update | Atualize a propriedade groups.settings no Azure Active Directory. |
| microsoft.aad.directory/users/appRoleAssignments/update | Atualize a propriedade approleassignments no Azure Active Directory. |
| Microsoft.aad.Directory/Users/Default/Update | Atualize as propriedades básicas sobre os utilizadores no Azure Active Directory. |
| Microsoft.aad.Directory/Users/Manager/Update | Atualize a propriedade Users no Azure Active Directory. |
| microsoft.azure.supportTickets/allEntities/allTasks | Criar e gerir pedidos de suporte do Azure. |
| microsoft.intune/allEntities/allTasks | Gerir todos os aspetos do Intune. |
| Microsoft.office365.supportTickets/allEntities/allTasks | Crie e gira pedidos de suporte do Office 365. |

### <a name="license-administrator"></a>Administrador de Licenças
Pode gerir licenças de produtos em utilizadores e grupos.

  > [!NOTE]
  > Esta função herda permissões adicionais da função leitores de diretório.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.aad.directory/users/assignLicense | Gira licenças em utilizadores no Azure Active Directory. |
| microsoft.aad.directory/users/usageLocation/update | Atualize a propriedade usagelocation no Azure Active Directory. |
| microsoft.azure.accessService/allEntities/allTasks | Gerir todos os aspetos do serviço de acesso do Azure. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Ler e configurar o Azure Service Health. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Leia e configure o Estado de Funcionamento do Serviço Office 365. |

### <a name="lync-service-administrator"></a>Administrador do Serviço Lync
Pode gerir todos os aspetos do produto Skype para Empresas.

  > [!NOTE]
  > Esta função herda permissões adicionais da função leitores de diretório.
  >
  >

  > [!NOTE]
  > Esta função tem permissões de addditonal fora do Azure Active Directory. Ver Descrição da função acima para obter mais informações.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | Gerir todos os aspetos do serviço de acesso do Azure. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Ler e configurar o Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Criar e gerir pedidos de suporte do Azure. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Leia e configure o Estado de Funcionamento do Serviço Office 365. |
| Microsoft.office365.skypeForBusiness/allEntities/allTasks | Gerir todos os aspetos do Skype para empresas Online. |
| Microsoft.office365.supportTickets/allEntities/allTasks | Crie e gira pedidos de suporte do Office 365. |

### <a name="message-center-reader"></a>Leitor do Centro de Mensagens
Pode ler as mensagens e atualizações para a sua organização apenas no Centro de Mensagem do Office 365. 

  > [!NOTE]
  > Esta função herda permissões adicionais da função leitores de diretório.
  >
  >

  > [!NOTE]
  > Esta função tem permissões de addditonal fora do Azure Active Directory. Ver Descrição da função acima para obter mais informações.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.aad.accessmessagecenter/allEntities/allTasks | Criar e eliminar todos os recursos, ler e atualizar as propriedades standard no Centro de Mensagens. |
| microsoft.azure.accessService/allEntities/allTasks | Gerir todos os aspetos do serviço de acesso do Azure. |

### <a name="partner-tier1-support"></a>Parceiro de Suporte de Escalão 1
Não utilize – não se destina a utilização geral.

  > [!NOTE]
  > Esta função herda permissões adicionais da função leitores de diretório.
  >
  >

  > [!NOTE]
  > Esta função tem permissões de addditonal fora do Azure Active Directory. Ver Descrição da função acima para obter mais informações.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| Microsoft.aad.Directory/Contacts/default/Update | Atualize as propriedades básicas para contactos no Azure Active Directory. |
| Microsoft.aad.Directory/Contacts/Create | Crie contactos no Azure Active Directory. |
| Microsoft.aad.Directory/Contacts/DELETE | Elimine contactos no Azure Active Directory. |
| Microsoft.aad.Directory/Groups/Create | Crie grupos no Azure Active Directory. |
| microsoft.aad.directory/groups/createAsOwner | Crie grupos no Azure Active Directory. Criador é adicionado como o primeiro proprietário e o objeto criado conta contra a quota de 250 objetos criados do criador. |
| Microsoft.aad.Directory/Groups/members/Update | Atualize a propriedade members no Azure Active Directory. |
| Microsoft.aad.Directory/Groups/Owners/Update | Atualize a propriedade Owners no Azure Active Directory. |
| microsoft.aad.directory/users/appRoleAssignments/update | Atualize a propriedade approleassignments no Azure Active Directory. |
| microsoft.aad.directory/users/assignLicense | Gira licenças em utilizadores no Azure Active Directory. |
| Microsoft.aad.Directory/Users/Default/Update | Atualize as propriedades básicas sobre os utilizadores no Azure Active Directory. |
| Microsoft.aad.Directory/Users/DELETE | Elimine utilizadores no Azure Active Directory. |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Invalide todos os tokens de atualização de utilizador no Azure Active Directory. |
| Microsoft.aad.Directory/Users/Manager/Update | Atualize a propriedade Users no Azure Active Directory. |
| Microsoft.aad.Directory/Users/password/Update | Atualize as palavras-passe para todos os utilizadores no Azure Active Directory. Consulte a documentação online para obter mais detalhes. |
| Microsoft.aad.Directory/Users/Restore | Restaure utilizadores eliminados no Azure Active Directory. |
| microsoft.aad.directory/users/userPrincipalName/update | Atualize a propriedade users.userPrincipalName no Azure Active Directory. |
| microsoft.azure.accessService/allEntities/allTasks | Gerir todos os aspetos do serviço de acesso do Azure. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Ler e configurar o Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Criar e gerir pedidos de suporte do Azure. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Leia e configure o Estado de Funcionamento do Serviço Office 365. |
| Microsoft.office365.supportTickets/allEntities/allTasks | Crie e gira pedidos de suporte do Office 365. |

### <a name="partner-tier2-support"></a>Parceiro de Suporte de Escalão 2
Não utilize – não se destina a utilização geral.

  > [!NOTE]
  > Esta função herda permissões adicionais da função leitores de diretório.
  >
  >

  > [!NOTE]
  > Esta função tem permissões de addditonal fora do Azure Active Directory. Ver Descrição da função acima para obter mais informações.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| Microsoft.aad.Directory/Contacts/default/Update | Atualize as propriedades básicas para contactos no Azure Active Directory. |
| Microsoft.aad.Directory/Contacts/Create | Crie contactos no Azure Active Directory. |
| Microsoft.aad.Directory/Contacts/DELETE | Elimine contactos no Azure Active Directory. |
| microsoft.aad.directory/domains/allTasks | Crie e elimine domínios e leia e atualize propriedades padrão no Azure Active Directory. |
| Microsoft.aad.Directory/Groups/Create | Crie grupos no Azure Active Directory. |
| Microsoft.aad.Directory/Groups/DELETE | Elimine grupos no Azure Active Directory. |
| Microsoft.aad.Directory/Groups/members/Update | Atualize a propriedade members no Azure Active Directory. |
| Microsoft.aad.Directory/Groups/Restore | Restaure grupos no Azure Active Directory. |
| Microsoft.aad.Directory/Organization/default/Update | Atualize as propriedades básicas na organização no Azure Active Directory. |
| microsoft.aad.directory/organization/trustedCAsForPasswordlessAuth/update | Atualize a propriedade organization.trustedCAsForPasswordlessAuth no Azure Active Directory. |
| microsoft.aad.directory/users/appRoleAssignments/update | Atualize a propriedade approleassignments no Azure Active Directory. |
| microsoft.aad.directory/users/assignLicense | Gira licenças em utilizadores no Azure Active Directory. |
| Microsoft.aad.Directory/Users/Default/Update | Atualize as propriedades básicas sobre os utilizadores no Azure Active Directory. |
| Microsoft.aad.Directory/Users/DELETE | Elimine utilizadores no Azure Active Directory. |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Invalide todos os tokens de atualização de utilizador no Azure Active Directory. |
| Microsoft.aad.Directory/Users/Manager/Update | Atualize a propriedade Users no Azure Active Directory. |
| Microsoft.aad.Directory/Users/password/Update | Atualize as palavras-passe para todos os utilizadores no Azure Active Directory. Consulte a documentação online para obter mais detalhes. |
| Microsoft.aad.Directory/Users/Restore | Restaure utilizadores eliminados no Azure Active Directory. |
| microsoft.aad.directory/users/userPrincipalName/update | Atualize a propriedade users.userPrincipalName no Azure Active Directory. |
| microsoft.azure.accessService/allEntities/allTasks | Gerir todos os aspetos do serviço de acesso do Azure. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Ler e configurar o Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Criar e gerir pedidos de suporte do Azure. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Leia e configure o Estado de Funcionamento do Serviço Office 365. |
| Microsoft.office365.supportTickets/allEntities/allTasks | Crie e gira pedidos de suporte do Office 365. |

### <a name="power-bi-service-administrator"></a>Administrador do Power BI
Pode gerir todos os aspetos do produto Power BI.

  > [!NOTE]
  > Esta função herda permissões adicionais da função leitores de diretório.
  >
  >

  > [!NOTE]
  > Esta função tem permissões de addditonal fora do Azure Active Directory. Ver Descrição da função acima para obter mais informações.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | Gerir todos os aspetos do serviço de acesso do Azure. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Ler e configurar o Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Criar e gerir pedidos de suporte do Azure. |
| microsoft.powerApps.powerBI/allEntities/allTasks | Gerir todos os aspetos do Power BI. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Leia e configure o Estado de Funcionamento do Serviço Office 365. |
| Microsoft.office365.supportTickets/allEntities/allTasks | Crie e gira pedidos de suporte do Office 365. |

### <a name="privileged-role-administrator"></a>Administrador de Função com Privilégios 
Pode gerir atribuições de funções no Azure AD

  > [!NOTE]
  > Esta função herda permissões adicionais da função leitores de diretório.
  >
  >

  > [!NOTE]
  > Esta função tem permissões de addditonal fora do Azure Active Directory. Ver Descrição da função acima para obter mais informações.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.aad.directory/directoryRoles/update | Atualize directoryRoles no Azure Active Directory. |
| microsoft.aad.privilegedIdentityManagement/allEntities/allTasks | Crie e elimine todos os recursos e leia e atualize propriedades padrão no microsoft.aad.privilegedIdentityManagement. |

### <a name="reports-reader"></a>Leitor de Relatórios
Pode ler relatórios de auditoria e inícios de sessão.

  > [!NOTE]
  > Esta função herda permissões adicionais da função leitores de diretório.
  >
  >

  > [!NOTE]
  > Esta função tem permissões de addditonal fora do Azure Active Directory. Ver Descrição da função acima para obter mais informações.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.aad.reports/allEntities/read | Leia Relatórios do Azure AD. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Ler e configurar o Azure Service Health. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Leia e configure o Estado de Funcionamento do Serviço Office 365. |
| Microsoft.office365.usageReports/allEntities/Read | Relatórios de utilização de leitura Office 365. |

### <a name="security-administrator"></a>Administrador de Segurança
Pode ler as informações de segurança e relatórios

  > [!NOTE]
  > Esta função herda permissões adicionais da função leitores de diretório.
  >
  >

  > [!NOTE]
  > Esta função tem permissões de addditonal fora do Azure Active Directory. Ver Descrição da função acima para obter mais informações.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| Microsoft.aad.Directory/Applications/Policies/Update | Atualize a propriedade applications.policies no Azure Active Directory. |
| Microsoft.aad.Directory/Policies/default/Update | Atualize as propriedades básicas em políticas no Azure Active Directory. |
| Microsoft.aad.Directory/Policies/Create | Crie políticas no Azure Active Directory. |
| Microsoft.aad.Directory/Policies/DELETE | Elimine políticas no Azure Active Directory. |
| Microsoft.aad.Directory/Policies/Owners/Update | Atualize a propriedade policies.owners no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/policies/update | Atualize a propriedade servicePrincipals.policies no Azure Active Directory. |
| microsoft.aad.identityProtection/allEntities/read | Ler todos os recursos no microsoft.aad.identityProtection. |
| microsoft.aad.identityProtection/allEntities/update | Atualize todos os recursos no microsoft.aad.identityProtection. |
| microsoft.aad.privilegedIdentityManagement/allEntities/read | Ler todos os recursos no microsoft.aad.privilegedIdentityManagement. |
| microsoft.azure.accessService/allEntities/allTasks | Gerir todos os aspetos do serviço de acesso do Azure. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Ler e configurar o Azure Service Health. |
| Microsoft.office365.protectionCenter/allEntities/Read | Leia todos os aspetos do Centro de Proteção do Office 365. |
| Microsoft.office365.protectionCenter/allEntities/Update | Atualize todos os recursos no microsoft.office365.protectionCenter. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Leia e configure o Estado de Funcionamento do Serviço Office 365. |

### <a name="security-reader"></a>Leitor de Segurança
Pode ler as informações de segurança e os relatórios no Azure AD e no Office 365.

  > [!NOTE]
  > Esta função herda permissões adicionais da função leitores de diretório.
  >
  >

  > [!NOTE]
  > Esta função tem permissões de addditonal fora do Azure Active Directory. Ver Descrição da função acima para obter mais informações.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.aad.identityProtection/allEntities/read | Ler todos os recursos no microsoft.aad.identityProtection. |
| microsoft.aad.privilegedIdentityManagement/allEntities/read | Ler todos os recursos no microsoft.aad.privilegedIdentityManagement. |
| microsoft.azure.accessService/allEntities/allTasks | Gerir todos os aspetos do serviço de acesso do Azure. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Ler e configurar o Azure Service Health. |
| Microsoft.office365.protectionCenter/allEntities/Read | Leia todos os aspetos do Centro de Proteção do Office 365. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Leia e configure o Estado de Funcionamento do Serviço Office 365. |

### <a name="service-support-administrator"></a>Administrador de Assistência Técnica
Consegue ler as informações do Service Health e gerir pedidos de suporte.

  > [!NOTE]
  > Esta função herda permissões adicionais da função leitores de diretório.
  >
  >

  > [!NOTE]
  > Esta função tem permissões de addditonal fora do Azure Active Directory. Ver Descrição da função acima para obter mais informações.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | Gerir todos os aspetos do serviço de acesso do Azure. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Ler e configurar o Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Criar e gerir pedidos de suporte do Azure. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Leia e configure o Estado de Funcionamento do Serviço Office 365. |
| Microsoft.office365.supportTickets/allEntities/allTasks | Crie e gira pedidos de suporte do Office 365. |

### <a name="sharepoint-service-administrator"></a>Administrador de Serviços do SharePoint
Pode gerir todos os aspetos do serviço do SharePoint.

  > [!NOTE]
  > Esta função herda permissões adicionais da função leitores de diretório.
  >
  >

  > [!NOTE]
  > Esta função tem permissões de addditonal fora do Azure Active Directory. Ver Descrição da função acima para obter mais informações.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | Gerir todos os aspetos do serviço de acesso do Azure. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Ler e configurar o Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Criar e gerir pedidos de suporte do Azure. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Leia e configure o Estado de Funcionamento do Serviço Office 365. |
| Microsoft.office365.SharePoint/allEntities/allTasks | Crie e elimine todos os recursos e leia e atualize propriedades padrão no microsoft.office365.sharepoint. |
| Microsoft.office365.supportTickets/allEntities/allTasks | Crie e gira pedidos de suporte do Office 365. |

### <a name="teams-communications-administrator"></a>Administrador de comunicação de equipas
Pode gerir a chamar e os recursos de reuniões do serviço do Microsoft Teams.

  > [!NOTE]
  > Esta função herda permissões adicionais da função leitores de diretório.
  >
  >

  > [!NOTE]
  > Esta função tem permissões de addditonal fora do Azure Active Directory. Ver Descrição da função acima para obter mais informações.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| Microsoft.aad.Directory/Policies/Basic/Read | Ler as propriedades básicas nas políticas no Azure Active Directory. |
| microsoft.azure.accessService/allEntities/allTasks | Gerir todos os aspetos do serviço de acesso do Azure. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Ler e configurar o Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Criar e gerir pedidos de suporte do Azure. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Leia e configure o Estado de Funcionamento do Serviço Office 365. |
| Microsoft.office365.supportTickets/allEntities/allTasks | Crie e gira pedidos de suporte do Office 365. |
| Microsoft.office365.usageReports/allEntities/Read | Relatórios de utilização de leitura Office 365. |

### <a name="teams-communications-support-engineer"></a>Engenheiro de suporte de comunicação de equipas
Pode resolver problemas de comunicação entre as Equipes com ferramentas avançadas.

  > [!NOTE]
  > Esta função herda permissões adicionais da função leitores de diretório.
  >
  >

  > [!NOTE]
  > Esta função tem permissões de addditonal fora do Azure Active Directory. Ver Descrição da função acima para obter mais informações.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| Microsoft.aad.Directory/Policies/Basic/Read | Ler as propriedades básicas nas políticas no Azure Active Directory. |
| microsoft.azure.accessService/allEntities/allTasks | Gerir todos os aspetos do serviço de acesso do Azure. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Ler e configurar o Azure Service Health. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Leia e configure o Estado de Funcionamento do Serviço Office 365. |

### <a name="teams-communications-support-specialist"></a>Especialista de suporte de comunicações de equipes
Pode resolver problemas de comunicação entre as Equipes com as ferramentas básicas.

  > [!NOTE]
  > Esta função herda permissões adicionais da função leitores de diretório.
  >
  >

  > [!NOTE]
  > Esta função tem permissões de addditonal fora do Azure Active Directory. Ver Descrição da função acima para obter mais informações.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| Microsoft.aad.Directory/Policies/Basic/Read | Ler as propriedades básicas nas políticas no Azure Active Directory. |
| microsoft.azure.accessService/allEntities/allTasks | Gerir todos os aspetos do serviço de acesso do Azure. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Ler e configurar o Azure Service Health. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Leia e configure o Estado de Funcionamento do Serviço Office 365. |

### <a name="teams-service-administrator"></a>Administrador de serviços de equipes
Pode gerir o serviço do Microsoft Teams.

  > [!NOTE]
  > Esta função herda permissões adicionais da função leitores de diretório.
  >
  >

  > [!NOTE]
  > Esta função tem permissões de addditonal fora do Azure Active Directory. Ver Descrição da função acima para obter mais informações.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.aad.directory/groups/hiddenMembers/read | Leia a propriedade groups.hiddenMembers no Azure Active Directory. |
| Microsoft.aad.Directory/Policies/Basic/Read | Ler as propriedades básicas nas políticas no Azure Active Directory. |
| microsoft.azure.accessService/allEntities/allTasks | Gerir todos os aspetos do serviço de acesso do Azure. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Ler e configurar o Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Criar e gerir pedidos de suporte do Azure. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Leia e configure o Estado de Funcionamento do Serviço Office 365. |
| Microsoft.office365.supportTickets/allEntities/allTasks | Crie e gira pedidos de suporte do Office 365. |
| Microsoft.office365.usageReports/allEntities/Read | Relatórios de utilização de leitura Office 365. |

### <a name="user-account-administrator"></a>Administrador de Conta de Utilizador
Pode gerir todos os aspetos de utilizadores e grupos

  > [!NOTE]
  > Esta função herda permissões adicionais da função leitores de diretório.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.aad.directory/appRoleAssignments/create | Crie appRoleAssignments no Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/delete | Elimine appRoleAssignments no Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/update | Atualize appRoleAssignments no Azure Active Directory. |
| Microsoft.aad.Directory/Contacts/default/Update | Atualize as propriedades básicas para contactos no Azure Active Directory. |
| Microsoft.aad.Directory/Contacts/Create | Crie contactos no Azure Active Directory. |
| Microsoft.aad.Directory/Contacts/DELETE | Elimine contactos no Azure Active Directory. |
| microsoft.aad.directory/groups/appRoleAssignments/update | Atualize a propriedade groups.appRoleAssignments no Azure Active Directory. |
| Microsoft.aad.Directory/Groups/default/Update | Atualize as propriedades básicas sobre os grupos no Azure Active Directory. |
| Microsoft.aad.Directory/Groups/Create | Crie grupos no Azure Active Directory. |
| microsoft.aad.directory/groups/createAsOwner | Crie grupos no Azure Active Directory. Criador é adicionado como o primeiro proprietário e o objeto criado conta contra a quota de 250 objetos criados do criador. |
| Microsoft.aad.Directory/Groups/DELETE | Elimine grupos no Azure Active Directory. |
| microsoft.aad.directory/groups/hiddenMembers/read | Leia a propriedade groups.hiddenMembers no Azure Active Directory. |
| Microsoft.aad.Directory/Groups/members/Update | Atualize a propriedade members no Azure Active Directory. |
| Microsoft.aad.Directory/Groups/Owners/Update | Atualize a propriedade Owners no Azure Active Directory. |
| Microsoft.aad.Directory/Groups/Restore | Restaure grupos no Azure Active Directory. |
| Microsoft.aad.Directory/Groups/Settings/Update | Atualize a propriedade groups.settings no Azure Active Directory. |
| microsoft.aad.directory/users/appRoleAssignments/update | Atualize a propriedade approleassignments no Azure Active Directory. |
| microsoft.aad.directory/users/assignLicense | Gira licenças em utilizadores no Azure Active Directory. |
| Microsoft.aad.Directory/Users/Default/Update | Atualize as propriedades básicas sobre os utilizadores no Azure Active Directory. |
| Microsoft.aad.Directory/Users/Create | Crie utilizadores no Azure Active Directory. |
| Microsoft.aad.Directory/Users/DELETE | Elimine utilizadores no Azure Active Directory. |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Invalide todos os tokens de atualização de utilizador no Azure Active Directory. |
| Microsoft.aad.Directory/Users/Manager/Update | Atualize a propriedade Users no Azure Active Directory. |
| Microsoft.aad.Directory/Users/password/Update | Atualize as palavras-passe para todos os utilizadores no Azure Active Directory. Consulte a documentação online para obter mais detalhes. |
| Microsoft.aad.Directory/Users/Restore | Restaure utilizadores eliminados no Azure Active Directory. |
| microsoft.aad.directory/users/userPrincipalName/update | Atualize a propriedade users.userPrincipalName no Azure Active Directory. |
| microsoft.azure.accessService/allEntities/allTasks | Gerir todos os aspetos do serviço de acesso do Azure. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Ler e configurar o Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Criar e gerir pedidos de suporte do Azure. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Leia e configure o Estado de Funcionamento do Serviço Office 365. |
| Microsoft.office365.supportTickets/allEntities/allTasks | Crie e gira pedidos de suporte do Office 365. |

### <a name="user"></a>Utilizador
Função predefinida para os utilizadores de membro. Pode ler todos e escrever um conjunto limitado de informações do diretório.

  > [!NOTE]
  > Esta função herda permissões adicionais da função leitores de diretório.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.aad.directory/applications/createAsOwner | Crie aplicações no Azure Active Directory. Criador é adicionado como o primeiro proprietário e o objeto criado conta contra a quota de 250 objetos criados do criador. |
| Microsoft.aad.Directory/Groups/default/Read | Ler as propriedades básicas sobre os grupos no Azure Active Directory. |
| microsoft.aad.directory/groups/createAsOwner | Crie grupos no Azure Active Directory. Criador é adicionado como o primeiro proprietário e o objeto criado conta contra a quota de 250 objetos criados do criador. |
| microsoft.aad.directory/oAuth2PermissionGrants/create | Crie oAuth2PermissionGrants no Azure Active Directory. |
| microsoft.aad.directory/oAuth2PermissionGrants/delete | Elimine oAuth2PermissionGrants no Azure Active Directory. |
| microsoft.aad.directory/oAuth2PermissionGrants/update | Atualize oAuth2PermissionGrants no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/createAsOwner | Crie servicePrincipals no Azure Active Directory. Criador é adicionado como o primeiro proprietário e o objeto criado conta contra a quota de 250 objetos criados do criador. |
| microsoft.aad.directory/users/activateServicePlan | Activateserviceplan utilizadores no Azure Active Directory. |
| microsoft.aad.directory/users/inviteGuest | Convide utilizadores no Azure Active Directory. |
| Microsoft.aad.Directory/Applications/default/Update | Atualize as propriedades básicas em aplicações no Azure Active Directory. |
| Microsoft.aad.Directory/Applications/DELETE | Elimine aplicações no Azure Active Directory. |
| Microsoft.aad.Directory/Applications/Owners/Update | Atualize a propriedade Owners no Azure Active Directory. |
| Microsoft.aad.Directory/Applications/Permissions/Update | Atualize a propriedade applications.permissions no Azure Active Directory. |
| Microsoft.aad.Directory/Applications/Policies/Update | Atualize a propriedade applications.policies no Azure Active Directory. |
| Microsoft.aad.Directory/Applications/Restore | Restaure aplicações no Azure Active Directory. |
| Microsoft.aad.Directory/Devices/disable | Desative dispositivos no Azure Active Directory. |
| microsoft.aad.directory/groups/appRoleAssignments/update | Atualize a propriedade groups.appRoleAssignments no Azure Active Directory. |
| Microsoft.aad.Directory/Groups/default/Update | Atualize as propriedades básicas sobre os grupos no Azure Active Directory. |
| Microsoft.aad.Directory/Groups/DELETE | Elimine grupos no Azure Active Directory. |
| microsoft.aad.directory/groups/dynamicMembershipRule/update | Atualize a propriedade groups.dynamicMembershipRule no Azure Active Directory. |
| Microsoft.aad.Directory/Groups/members/Update | Atualize a propriedade members no Azure Active Directory. |
| Microsoft.aad.Directory/Groups/Owners/Update | Atualize a propriedade Owners no Azure Active Directory. |
| Microsoft.aad.Directory/Groups/Restore | Restaure grupos no Azure Active Directory. |
| Microsoft.aad.Directory/Groups/Settings/Update | Atualize a propriedade groups.settings no Azure Active Directory. |
| Microsoft.aad.Directory/Policies/default/Update | Atualize as propriedades básicas em políticas no Azure Active Directory. |
| Microsoft.aad.Directory/Policies/DELETE | Elimine políticas no Azure Active Directory. |
| Microsoft.aad.Directory/Policies/Owners/Update | Atualize a propriedade policies.owners no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/update | Atualize a propriedade Approleassignedto no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/update | Atualize a propriedade Serviceprincipals no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/default/update | Atualize as propriedades básicas no servicePrincipals no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/delete | Elimine servicePrincipals no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/owners/update | Atualize a propriedade Serviceprincipals no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/policies/update | Atualize a propriedade servicePrincipals.policies no Azure Active Directory. |
| microsoft.aad.directory/users/changePassword | Alterar as palavras-passe para todos os utilizadores no Azure Active Directory. Consulte a documentação online para obter mais detalhes. |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Invalide todos os tokens de atualização de utilizador no Azure Active Directory. |
| microsoft.aad.directory/users/basicProfile/update | Atualize a propriedade basicprofile no Azure Active Directory. |
| Microsoft.aad.Directory/Users/Mobile/Update | Atualize a propriedade users.mobile no Azure Active Directory. |
| microsoft.aad.directory/users/searchableDeviceKey/update | Atualize a propriedade users.searchableDeviceKey no Azure Active Directory. |


## <a name="next-steps"></a>Passos Seguintes

* Para saber mais sobre como atribuir um utilizador como administrador de uma subscrição do Azure, veja [gerir o acesso com RBAC e o portal do Azure](../../role-based-access-control/role-assignments-portal.md)
* Para saber mais sobre como o Microsoft Azure controla o acesso aos recursos, consulte [Noções sobre o acesso aos recursos no Azure](../../role-based-access-control/rbac-and-directory-admin-roles.md)
* Para obter mais informações sobre como o Azure Active Directory está relacionado com a sua subscrição do Azure, veja [Como as subscrições do Azure estão associadas ao Azure Active Directory](../fundamentals/active-directory-how-subscriptions-associated-directory.md)
