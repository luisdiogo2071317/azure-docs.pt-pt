---
title: Atribuir funções de administrador no Azure Active Directory | Documentos da Microsoft
description: Uma função de administrador pode adicionar utilizadores, atribuir funções administrativas, repor palavras-passe do utilizador, gerir licenças de utilizador ou gerir domínios. Um utilizador a quem seja atribuído uma função de administrador tem as mesmas permissões em todos os serviços de cloud para o qual a sua organização tiver subscrito.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 08/21/2018
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.openlocfilehash: 6c97d7c2f901110421f9fc5d0a1d4468d832c472
ms.sourcegitcommit: 8ebcecb837bbfb989728e4667d74e42f7a3a9352
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/21/2018
ms.locfileid: "42057119"
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

* **[Leitor do Centro de mensagens](#message-center-reader)**: os utilizadores nesta função podem monitorizar notificações e atualizações de estado de funcionamento de aviso [Centro de mensagens do Office 365](https://support.office.com/article/Message-center-in-Office-365-38FB3333-BFCC-4340-A37B-DEDA509C2093) para a sua organização em serviços configurados como o Exchange, o Intune e o Microsoft Teams. Os leitores de centro de mensagens recebem resumos de e-mail semanal de postagens, atualizações e podem partilhar publicações do Centro de mensagens no Office 365. No Azure AD, os utilizadores atribuídos a esta função só terão acesso só de leitura nos serviços do Azure AD, como utilizadores e grupos. 

* **[Suporte de escalão 1 de parceiros](#partner-tier1-support)**: não utilize. Esta função foi preterida e será removida do Azure AD no futuro. Esta função é destinada à utilização por um pequeno número de parceiros de revenda da Microsoft e não se destina a utilização geral.

* **[Suporte de escalão 2 de parceiros](#partner-tier2-support)**: não utilize. Esta função foi preterida e será removida do Azure AD no futuro. Esta função é destinada à utilização por um pequeno número de parceiros de revenda da Microsoft e não se destina a utilização geral.

* **[Palavra-passe de administrador / administrador de suporte técnico](#helpdesk-administrator)**: os utilizadores com esta função podem alterar palavras-passe, gerir pedidos de serviço e monitorizar o estado de funcionamento do serviço. Os administradores de suporte técnico podem alterar as palavras-passe apenas para utilizadores e outros administradores de suporte técnico. 

  > [!NOTE]
  > No Microsoft Graph API, o Azure AD Graph API e o Azure AD PowerShell, esta função é identificada como "Administrador de suporte técnico". É "Administrador de palavras-passe" na [portal do Azure](https://portal.azure.com/).
  >
  >
  
* **[Administrador de serviço do Power BI](#power-bi-service-administrator)**: os utilizadores com esta função possuem permissões globais dentro do Microsoft Power BI, quando o serviço está presente, bem como a capacidade para gerir pedidos de suporte e monitorizar o estado de funcionamento do serviço. Mais informações em [compreender a função de administrador do Power BI](https://docs.microsoft.com/power-bi/service-admin-role).

* **[Com privilégios administrador de função](#privileged-role-administrator)**: os utilizadores com esta função podem gerir atribuições de funções no Azure Active Directory, assim como no Azure AD Privileged Identity Management. Além disso, esta função permite a gestão de todos os aspetos do Privileged Identity Management.

* **[Leitor de relatórios](#reports-reader)**: os utilizadores com esta função podem ver a utilização de relatórios do pacote de dados e o dashboard de relatórios no Centro de administração do Office 365 e o contexto de adoção no Power BI. Além disso, a função fornece acesso ao início de sessão em relatórios e a atividade no Azure AD e a API de relatórios de dados devolvidos pelo Microsoft Graph. Pode aceder a um utilizador atribuído à função do leitor de relatórios apenas utilização relevante e métricas de adoção. Eles não tem quaisquer permissões de administrador para configurar as definições ou acesso que no produto específico Centro de administração como o Exchange. 

* **[Administrador de segurança](#security-administrator)**: os utilizadores com esta função têm todas as permissões só de leitura de função do leitor de segurança, além da capacidade para gerir a configuração de serviços relacionados com segurança: Azure Active Directory Identity Protection, Azure Information Protection, Privileged Identity Management e o Office 365 Centro de segurança e conformidade. Obter mais informações sobre as permissões do Office 365 estão disponíveis em [permissões no Centro de conformidade de segurança do Office 365 e](https://support.office.com/article/Permissions-in-the-Office-365-Security-Compliance-Center-d10608af-7934-490a-818e-e68f17d0e9c1).
  
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

* **[Skype para empresas / administrador do serviço Lync](#lync-service-administrator)**: os utilizadores com esta função têm permissões globais no Microsoft Skype para empresas, quando o serviço está presente, bem como gerir os atributos de utilizador do Skype específico no Azure Active Diretório. Além disso, esta função concede a capacidade para gerir pedidos de suporte e monitorizar o estado de funcionamento do serviço. Mais informações em [sobre o Skype para a função de administrador de empresa](https://support.office.com/en-us/article/about-the-skype-for-business-admin-role-aeb35bda-93fc-49b1-ac2c-c74fbeb737b5).

  > [!NOTE]
  > No Microsoft Graph API, o Azure AD Graph API e o Azure AD PowerShell, esta função é identificada como "Administrador do serviço Lync". É "Skype para empresas administrador de serviços" na [portal do Azure](https://portal.azure.com/).
  >
  >

* **[Administrador de conta de utilizador](#user-account-administrator)**: os utilizadores com esta função podem criar e gerir todos os aspetos de utilizadores e grupos. Além disso, esta função inclui a capacidade para gerir pedidos de suporte e monitorizar o estado de funcionamento do serviço. Aplicam-se algumas restrições. Por exemplo, esta função não permite a eliminação de um administrador global. Os administradores de conta de utilizador podem alterar as palavras-passe para utilizadores, administradores de suporte técnico e outros administradores de conta de utilizador apenas.

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


### <a name="to-add-a-colleague-as-a-global-administrator"></a>Para adicionar um colega como um administrador global

1. Inicie sessão para o [Centro de administração do Azure Active Directory](https://aad.portal.azure.com) com uma conta que seja um administrador global ou administrador com função privilegiada do diretório do inquilino.

   ![Abrir o Centro de administração do azure AD](./media/directory-assign-admin-roles/active-directory-admin-center.png)

2. Selecione **utilizadores**.

3. Localize o utilizador que pretende designar como um administrador global e abrir o painel para esse utilizador.

4. No painel do utilizador, selecione **função de diretório**.
 
5. No painel de função de diretório, selecione o **Administrador Global** função e guarde.

## <a name="detailed-azure-active-directory-permissions"></a>Detalhadas permissões do Active Directory do Azure
As tabelas seguintes descrevem as permissões específicas no Azure Active Directory tendo em conta a cada função. Algumas funções, como o Administrador Global, podem ter permissões adicionais no Microsoft outide de serviços do Azure Active Directory.


### <a name="application-administrator"></a>Administrador de Aplicações
Pode criar e gerir todos os aspetos de registos de aplicações e aplicações empresariais.

  > [!NOTE]
  > Esta função herda permissões adicionais do [função de utilizador](https://docs.microsoft.com/azure/active-directory/users-default-permissions).
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.aad.directory/Application/Create | Criar Aplicações do Azure Active Directory. |
| microsoft.aad.directory/Application/Delete | Eliminar as Aplicações no Azure Active Directory. |
| microsoft.aad.directory/Application/Update | Atualize propriedades padrão em aplicações no Azure Active Directory. |
| microsoft.aad.directory/Application/Update/DefaultPolicy | Atualizar a propriedade Applications.DefaultPolicy no Azure Active Directory. |
| microsoft.aad.directory/Application/Update/Owners | Atualizar a propriedade Applications.Owners no Azure Active Directory. |
| microsoft.aad.directory/AppRoleAssignment/Create | Crie AppRoleAssignments no Azure Active Directory. |
| microsoft.aad.directory/AppRoleAssignment/Delete | Eliminar AppRoleAssignments no Azure Active Directory. |
| microsoft.aad.directory/AppRoleAssignment/Update | Atualize propriedades padrão em AppRoleAssignments no Azure Active Directory. |
| microsoft.aad.directory/Policy/Create | Criar Políticas no Azure Active Directory. |
| microsoft.aad.directory/Policy/Delete | Eliminar Policies no Azure Active Directory. |
| microsoft.aad.directory/Policy/Update | Atualize propriedades standard nas Políticas no Azure Active Directory. |
| microsoft.aad.directory/Policy/Update/Owners | Atualizar a propriedade Policies.Owners no Azure Active Directory. |
| microsoft.aad.directory/ServiceAction/ConsentOnBehalfOfAllNoDirectory | Pode consentir em nome de todos os utilizadores para todos os recursos, exceto o Azure Active Directory (Azure AD Graph e Microsoft Graph). |
| microsoft.aad.directory/ServicePrincipal/Create | Crie ServicePrincipals no Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Delete | Elimine ServicePrincipals no Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Update | Atualize propriedades padrão em ServicePrincipals no Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Update/AppRoleAssignedTo | Atualize a propriedade ServicePrincipals.AppRoleAssignedTo no Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Update/AppRoleAssignments | Atualize a propriedade ServicePrincipals.AppRoleAssignments no Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Update/DefaultPolicy | Atualize a propriedade ServicePrincipals.DefaultPolicy no Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Update/Owners | Atualize a propriedade ServicePrincipals.Owners no Azure Active Directory. |
| microsoft.aad.directory/User/AssignLicense | Gira licenças em Utilizadores no Azure Active Directory. |
| microsoft.aad.reports/AllEntities/Read | Leia Relatórios do Azure AD. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Leia e configure o Estado de Funcionamento do Serviço Office 365. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Crie e gira pedidos de suporte do Office 365. |

### <a name="application-developer"></a>Programador de Aplicações
Pode criar registos de aplicações independentemente do **os utilizadores podem registar aplicações** definição.

  > [!NOTE]
  > Esta função herda permissões adicionais do [função de utilizador](https://docs.microsoft.com/azure/active-directory/users-default-permissions).
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.aad.directory/Application/CreateAsOwner | Criar Aplicações do Azure Active Directory. Criador é adicionado como o primeiro proprietário e o objeto criado conta contra a quota de 250 objetos criados do criador. |
| microsoft.aad.directory/AppRoleAssignment/CreateAsOwner | Crie AppRoleAssignments no Azure Active Directory. Criador é adicionado como o primeiro proprietário e o objeto criado conta contra a quota de 250 objetos criados do criador. |
| microsoft.aad.directory/OAuth2PermissionGrant/CreateAsOwner | Criar OAuth2PermissionGrants no Azure Active Directory. Criador é adicionado como o primeiro proprietário e o objeto criado conta contra a quota de 250 objetos criados do criador. |
| microsoft.aad.directory/ServicePrincipal/CreateAsOwner | Crie ServicePrincipals no Azure Active Directory. Criador é adicionado como o primeiro proprietário e o objeto criado conta contra a quota de 250 objetos criados do criador. |

### <a name="billing-administrator"></a>Administrador de Faturação
Pode executar tarefas de faturação comuns, como atualizar as informações de pagamento.

  > [!NOTE]
  > Esta função herda permissões adicionais do [função de utilizador](https://docs.microsoft.com/azure/active-directory/users-default-permissions).
  >
  >

  > Esta função tem permissões adicionais fora do Azure Active Directory. Consulte a descrição de função acima para obter mais informações.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Leia a propriedade Organizations.TrustedCAsForPasswordlessAuth no Azure Active Directory. |
| microsoft.aad.directory/Organization/Update | Atualize propriedades padrão em organizações no Azure Active Directory. |
| microsoft.aad.directory/Organization/Update/TrustedCAsForPasswordlessAuth | Atualize a propriedade Organizations.TrustedCAsForPasswordlessAuth no Azure Active Directory. |
| microsoft.aad.accessservice/AllEntities/AllActions | Crie e elimine todos os recursos e leia e atualize propriedades padrão no Controlo de Acesso do Azure. |
| microsoft.aad.billing/AllEntities/AllActions | Gerir todos os aspetos da faturação do Office 365. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Leia e configure o Estado de Funcionamento do Serviço Office 365. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Crie e gira pedidos de suporte do Office 365. |

### <a name="cloud-application-administrator"></a>Administrador da Aplicação Cloud
Pode criar e gerir todos os aspetos de registos de aplicações e aplicações empresariais, exceto o Proxy de Aplicações.

  > [!NOTE]
  > Esta função herda permissões adicionais do [função de utilizador](https://docs.microsoft.com/azure/active-directory/users-default-permissions).
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.aad.directory/Application/Create | Criar Aplicações do Azure Active Directory. |
| microsoft.aad.directory/Application/Delete | Eliminar as Aplicações no Azure Active Directory. |
| microsoft.aad.directory/Application/Update | Atualize propriedades padrão em aplicações no Azure Active Directory. |
| microsoft.aad.directory/Application/Update/DefaultPolicy | Atualizar a propriedade Applications.DefaultPolicy no Azure Active Directory. |
| microsoft.aad.directory/Application/Update/Owners | Atualizar a propriedade Applications.Owners no Azure Active Directory. |
| microsoft.aad.directory/AppRoleAssignment/Create | Crie AppRoleAssignments no Azure Active Directory. |
| microsoft.aad.directory/AppRoleAssignment/Delete | Eliminar AppRoleAssignments no Azure Active Directory. |
| microsoft.aad.directory/AppRoleAssignment/Update | Atualize propriedades padrão em AppRoleAssignments no Azure Active Directory. |
| microsoft.aad.directory/Policy/Create | Criar Políticas no Azure Active Directory. |
| microsoft.aad.directory/Policy/Delete | Eliminar Policies no Azure Active Directory. |
| microsoft.aad.directory/Policy/Update | Atualize propriedades standard nas Políticas no Azure Active Directory. |
| microsoft.aad.directory/Policy/Update/Owners | Atualizar a propriedade Policies.Owners no Azure Active Directory. |
| microsoft.aad.directory/ServiceAction/ConsentOnBehalfOfAllNoDirectory | Pode consentir em nome de todos os utilizadores para todos os recursos, exceto o Azure Active Directory (Azure AD Graph e Microsoft Graph). |
| microsoft.aad.directory/ServicePrincipal/Create | Crie ServicePrincipals no Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Delete | Elimine ServicePrincipals no Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Update | Atualize propriedades padrão em ServicePrincipals no Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Update/AppRoleAssignedTo | Atualize a propriedade ServicePrincipals.AppRoleAssignedTo no Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Update/AppRoleAssignments | Atualize a propriedade ServicePrincipals.AppRoleAssignments no Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Update/DefaultPolicy | Atualize a propriedade ServicePrincipals.DefaultPolicy no Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Update/Owners | Atualize a propriedade ServicePrincipals.Owners no Azure Active Directory. |
| microsoft.aad.directory/User/AssignLicense | Gira licenças em Utilizadores no Azure Active Directory. |
| microsoft.aad.reports/AllEntities/Read | Leia Relatórios do Azure AD. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Leia e configure o Estado de Funcionamento do Serviço Office 365. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Crie e gira pedidos de suporte do Office 365. |

### <a name="company-administrator"></a>Administrador de Empresa
Pode gerir todos os aspetos dos serviços Microsoft e Azure AD que utilizam identidades do Azure AD. No Microsoft Graph API, o Azure AD Graph API e o Azure AD PowerShell, esta função é identificada como "Administrador de empresa". É "Administrador Global" no [portal do Azure](https://portal.azure.com).

  > [!NOTE]
  > Esta função tem permissões adicionais fora do Azure Active Directory. Ver Descrição da função acima para obter mais informações.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.aad.directory/AdministrativeUnit/AllActions/AllProperties | Criar e eliminar AdministrativeUnits, ler e atualizar todas as propriedades no Azure Active Directory. |
| microsoft.aad.directory/Application/AllActions/AllProperties | Criar e eliminar as Aplicações, ler e atualizar todas as propriedades no Azure Active Directory. |
| microsoft.aad.directory/AppRoleAssignment/AllActions/AllProperties | Criar e eliminar os AppRoleAssignments, ler e atualizar todas as propriedades no Azure Active Directory. |
| microsoft.aad.directory/CollaborationSpace/AllActions/AllProperties | Criar e eliminar CollaborationSpaces, ler e atualizar todas as propriedades no Azure Active Directory. |
| microsoft.aad.directory/Contact/AllActions/AllProperties | Criar e eliminar Contactos, ler e atualizar todas as propriedades no Azure Active Directory. |
| microsoft.aad.directory/Device/AllActions/AllProperties | Criar e eliminar os Dispositivos, ler e atualizar todas as propriedades no Azure Active Directory. |
| microsoft.aad.directory/DirectoryRole/AllActions/AllProperties | Criar e eliminar DirectoryRoles e ler e atualizar todas as propriedades no Azure Active Directory. |
| microsoft.aad.directory/DirectoryRoleTemplate/AllActions/AllProperties | Criar e eliminar os DirectoryRoleTemplates, ler e atualizar todas as propriedades no Azure Active Directory. |
| microsoft.aad.directory/DirectorySetting/AllActions/AllProperties | Criar e eliminar os DirectorySettings, ler e atualizar todas as propriedades no Azure Active Directory. |
| microsoft.aad.directory/DirectorySettingTemplate/AllActions/AllProperties | Criar e eliminar os DirectorySettingTemplates, ler e atualizar todas as propriedades no Azure Active Directory. |
| microsoft.aad.directory/Domain/AllActions/AllProperties | Crie e elimine Domínios e leia e atualize todas as propriedades no Azure Active Directory. |
| microsoft.aad.directory/Group/AllActions/AllProperties | Criar e eliminar os Grupos, ler e atualizar todas as propriedades no Azure Active Directory. |
| microsoft.aad.directory/LoginTenantBranding/AllActions/AllProperties | Criar e eliminar LoginTenantBrandings e ler e atualizar todas as propriedades no Azure Active Directory. |
| microsoft.aad.directory/OAuth2PermissionGrant/AllActions/AllProperties | Criar e eliminar OAuth2PermissionGrants e ler e atualizar todas as propriedades no Azure Active Directory. |
| microsoft.aad.directory/Policy/AllActions/AllProperties | Criar e eliminar as Políticas, ler e atualizar todas as propriedades no Azure Active Directory. |
| microsoft.aad.directory/ServiceAction/ConsentOnBehalfOfAllWithDirectory | Pode consentir em nome de todos os utilizadores para todos os recursos, incluindo o Azure Active Directory (Azure AD Graph e Microsoft Graph). |
| microsoft.aad.directory/ServicePrincipal/AllActions/AllProperties | Crie e elimine ServicePrincipals e leia e atualize todas as propriedades no Azure Active Directory. |
| microsoft.aad.directory/Organization/AllActions/AllProperties | Crie e elimine as organizações e leia e atualize todas as propriedades no Azure Active Directory. |
| microsoft.aad.directory/User/AllActions/AllProperties | Crie e elimine Utilizadores e leia e atualize todas as propriedades no Azure Active Directory. |
| microsoft.aad.aadconnect/AllEntities/AllActions | Crie e elimine todos os recursos e leia e atualize propriedades padrão no microsoft.aad.aadconnect. |
| microsoft.aad.accessservice/AllEntities/AllActions | Crie e elimine todos os recursos e leia e atualize propriedades padrão no Controlo de Acesso do Azure. |
| microsoft.aad.billing/AllEntities/AllActions | Gerir todos os aspetos da faturação do Office 365. |
| microsoft.aad.compliance/AllEntities/AllActions | Criar e eliminar todos os recursos, ler e atualizar as propriedades standard no Centro de Conformidade. |
| microsoft.aad.directorysync/AllEntities/AllActions | Execute todas as ações no Azure AD Connect. |
| microsoft.aad.lockbox/AllEntities/AllActions | Gerir todos os aspetos do serviço de cofre. |
| microsoft.aad.privilegedrolemanagement/AllEntities/AllActions | Gerir todos os aspetos do serviço de gestão de funções com privilégios. |
| microsoft.aad.reports/AllEntities/AllActions | Leia e configure Relatórios do Azure AD. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Leia e configure o Estado de Funcionamento do Serviço Office 365. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Crie e gira pedidos de suporte do Office 365. |
| microsoft.crm/AllEntities/AllActions | Gerir todos os aspetos do Dynamics 365. |
| microsoft.exchange/AllEntities/AllActions | Gerir todos os aspetos do Exchange Online. |
| microsoft.aad.informationprotection/AllEntities/AllActions | Gerir todos os aspetos do Information Protection. |
| microsoft.intune/AllEntities/AllActions | Gerir todos os aspetos do Intune. |
| microsoft.powerbi/AllEntities/AllActions | Gerir todos os aspetos do Power BI. |
| microsoft.protectioncenter/AllEntities/AllActions | Gira o Centro de Proteção do Office 365. |
| microsoft.sharepoint/AllEntities/AllActions | Gira o SharePoint Online. |
| microsoft.skypeforbusiness/AllEntities/AllActions | Gira o Skype para Empresas Online. |

### <a name="compliance-administrator"></a>Administrador de Conformidade
Pode ler e gerir a configuração de conformidade e os relatórios no Azure AD e no Office 365.

  > [!NOTE]
  > Esta função herda permissões adicionais do [função de utilizador](https://docs.microsoft.com/azure/active-directory/users-default-permissions).
  >
  >

  > [!NOTE]
  > Esta função tem permissões adicionais fora do Azure Active Directory. Ver Descrição da função acima para obter mais informações.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.aad.accessservice/AllEntities/AllActions | Crie e elimine todos os recursos e leia e atualize propriedades padrão no Controlo de Acesso do Azure. |
| microsoft.aad.compliance/AllEntities/AllActions | Criar e eliminar todos os recursos, ler e atualizar as propriedades standard no Centro de Conformidade. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Leia e configure o Estado de Funcionamento do Serviço Office 365. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Crie e gira pedidos de suporte do Office 365. |
| microsoft.exchange/Compliance/AllActions | Gerir a conformidade no Exchange Online. |
| microsoft.sharepoint/Compliance/AllActions | Gerir a conformidade no SharePoint Online. |
| microsoft.skypeforbusiness/Compliance/AllActions | Gerir a conformidade no Skype para empresas Online. |

### <a name="conditional-access-administrator"></a>Administrador de Acesso Condicional
Pode gerir capacidades de acesso condicional.

  > [!NOTE]
  > Esta função herda permissões adicionais do [função de utilizador](https://docs.microsoft.com/azure/active-directory/users-default-permissions).
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.aad.directory/ConditionalAccessPolicy/Create | Crie ConditionalAccessPolicys no Azure Active Directory. |
| microsoft.aad.directory/ConditionalAccessPolicy/Delete | Elimine ConditionalAccessPolicys no Azure Active Directory. |
| microsoft.aad.directory/ConditionalAccessPolicy/Read | Leia propriedades padrão em ConditionalAccessPolicys no Azure Active Directory. |
| microsoft.aad.directory/ConditionalAccessPolicy/Read/Owners | Leia a propriedade ConditionalAccessPolicys.Owners no Azure Active Directory. |
| microsoft.aad.directory/ConditionalAccessPolicy/Read/PolicyAppliedTo | Leia a propriedade ConditionalAccessPolicys.PolicyAppliedTo no Azure Active Directory. |
| microsoft.aad.directory/ConditionalAccessPolicy/Update | Atualize propriedades padrão em ConditionalAccessPolicys no Azure Active Directory. |
| microsoft.aad.directory/ConditionalAccessPolicy/Update/Owners | Atualize a propriedade ConditionalAccessPolicys.Owners no Azure Active Directory. |

### <a name="device-administrators"></a>Administradores de Dispositivo

Os utilizadores com esta função tornam-se administradores de computadores locais em todos os dispositivos Windows 10 que estão associados ao Azure Active Directory. Não têm a capacidade de gerir objetos de dispositivo no Azure Active Directory.

  > [!NOTE]
  > Esta função herda permissões adicionais do [função de utilizador](https://docs.microsoft.com/azure/active-directory/users-default-permissions).
  >
  >

### <a name="directory-readers"></a>Leitores de Diretório
Pode ler as informações de diretório básicas. Para conceder acesso a aplicações.

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.aad.directory/AdministrativeUnit/Read | Ler as propriedades standard em AdministrativeUnits no Azure Active Directory. |
| microsoft.aad.directory/AdministrativeUnit/Read/Members | Leia a propriedade AdministrativeUnits.Members no Azure Active Directory. |
| microsoft.aad.directory/Application/Read | Leia propriedades padrão em aplicações no Azure Active Directory. |
| microsoft.aad.directory/Application/Read/Owners | Leia a propriedade Applications.Owners no Azure Active Directory. |
| microsoft.aad.directory/CollaborationSpace/Read | Ler as propriedades standard em CollaborationSpaces no Azure Active Directory. |
| microsoft.aad.directory/CollaborationSpace/Read/Owners | Ler a propriedade CollaborationSpaces.Owners no Azure Active Directory. |
| microsoft.aad.directory/Contact/Read | Ler as propriedades standard de Contactos no Azure Active Directory. |
| microsoft.aad.directory/Contact/Read/MemberOf | Ler a propriedade Contacts.MemberOf no Azure Active Directory. |
| microsoft.aad.directory/Device/Read | Ler propriedades padrão nos Dispositivos no Azure Active Directory. |
| microsoft.aad.directory/Device/Read/MemberOf | Leia a propriedade Devices.MemberOf no Azure Active Directory. |
| microsoft.aad.directory/Device/Read/RegisteredOwners | Ler a propriedade Devices.RegisteredOwners no Azure Active Directory. |
| microsoft.aad.directory/Device/Read/RegisteredUsers | Ler a propriedade Devices.RegisteredUsers no Azure Active Directory. |
| microsoft.aad.directory/DirectoryRole/Read | Leia propriedades padrão em DirectoryRoles no Azure Active Directory. |
| microsoft.aad.directory/DirectoryRole/Read/EligibleMembers | Ler a propriedade DirectoryRoles.EligibleMembers no Azure Active Directory. |
| microsoft.aad.directory/DirectoryRole/Read/Members | Ler a propriedade de DirectoryRoles.Members no Azure Active Directory. |
| microsoft.aad.directory/DirectorySetting/Read | Leia propriedades padrão nas DirectorySettings no Azure Active Directory. |
| microsoft.aad.directory/DirectorySettingTemplate/Read | Ler as propriedades standard em DirectorySettingTemplates no Azure Active Directory. |
| microsoft.aad.directory/Domain/Read | Leia propriedades padrão em Domínios no Azure Active Directory. |
| microsoft.aad.directory/Group/Read | Leia propriedades padrão nos grupos no Azure Active Directory. |
| microsoft.aad.directory/Group/Read/AppRoleAssignments | Ler a propriedade Groups.AppRoleAssignments no Azure Active Directory. |
| microsoft.aad.directory/Group/Read/MemberOf | Ler a propriedade de Groups.MemberOf no Azure Active Directory. |
| microsoft.aad.directory/Group/Read/Members | Ler a propriedade de Groups.Members no Azure Active Directory. |
| microsoft.aad.directory/Group/Read/Owners | Ler a propriedade Groups.Owners no Azure Active Directory. |
| microsoft.aad.directory/Group/Read/Settings | Ler a propriedade de Groups.Settings no Azure Active Directory. |
| microsoft.aad.directory/OAuth2PermissionGrant/Read | Ler as propriedades standard em OAuth2PermissionGrants no Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Read | Leia propriedades padrão em ServicePrincipals no Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Read/AppRoleAssignedTo | Leia a propriedade ServicePrincipals.AppRoleAssignedTo no Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Read/AppRoleAssignments | Leia a propriedade ServicePrincipals.AppRoleAssignments no Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Read/DefaultPolicy | Leia a propriedade ServicePrincipals.DefaultPolicy no Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Read/MemberOf | Ler a propriedade ServicePrincipals.MemberOf no Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Read/OAuth2PermissionGrants | Leia a propriedade ServicePrincipals.OAuth2PermissionGrants no Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Read/Owners | Leia a propriedade ServicePrincipals.Owners no Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Read/OwnedObjects | Leia a propriedade ServicePrincipals.OwnedObjects no Azure Active Directory. |
| microsoft.aad.directory/Organization/Read | Leia propriedades padrão em organizações no Azure Active Directory. |
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Leia a propriedade Organizations.TrustedCAsForPasswordlessAuth no Azure Active Directory. |
| microsoft.aad.directory/User/Read | Leia propriedades padrão em utilizadores no Azure Active Directory. |
| microsoft.aad.directory/User/Read/AppRoleAssignments | Leia a propriedade Users.AppRoleAssignments no Azure Active Directory. |
| microsoft.aad.directory/User/Read/DirectReports | Leia a propriedade Users.DirectReports no Azure Active Directory. |
| microsoft.aad.directory/User/Read/InvitedBy | Leia a propriedade Users.InvitedBy no Azure Active Directory. |
| microsoft.aad.directory/User/Read/InvitedUsers | Leia a propriedade Users.InvitedUsers no Azure Active Directory. |
| microsoft.aad.directory/User/Read/Manager | Leia a propriedade Users.Manager no Azure Active Directory. |
| microsoft.aad.directory/User/Read/MemberOf | Leia a propriedade Users.MemberOf no Azure Active Directory. |
| microsoft.aad.directory/User/Read/OAuth2PermissionGrants | Leia a propriedade Users.OAuth2PermissionGrants no Azure Active Directory. |
| microsoft.aad.directory/User/Read/OwnedDevices | Leia a propriedade Users.OwnedDevices no Azure Active Directory. |
| microsoft.aad.directory/User/Read/OwnedObjects | Leia a propriedade Users.OwnedObjects no Azure Active Directory. |
| microsoft.aad.directory/User/Read/RegisteredDevices | Leia a propriedade Users.RegisteredDevices no Azure Active Directory. |

### <a name="directory-synchronization-accounts"></a>Contas de sincronização de diretório
Só é utilizada pelo serviço do Azure AD Connect.

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.aad.directory/Policy/Create | Criar Políticas no Azure Active Directory. |
| microsoft.aad.directory/Policy/Delete | Eliminar Policies no Azure Active Directory. |
| microsoft.aad.directory/Policy/Read | Ler as propriedades standard nas Políticas no Azure Active Directory. |
| microsoft.aad.directory/Policy/Read/Owners | Ler a propriedade Policies.Owners no Azure Active Directory. |
| microsoft.aad.directory/Policy/Read/PolicyAppliedTo | Ler a propriedade de Policies.PolicyAppliedTo no Azure Active Directory. |
| microsoft.aad.directory/Policy/Update | Atualize propriedades standard nas Políticas no Azure Active Directory. |
| microsoft.aad.directory/Policy/Update/Owners | Atualizar a propriedade Policies.Owners no Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Create | Crie ServicePrincipals no Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Read | Leia propriedades padrão em ServicePrincipals no Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Read/AppRoleAssignedTo | Leia a propriedade ServicePrincipals.AppRoleAssignedTo no Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Read/AppRoleAssignments | Leia a propriedade ServicePrincipals.AppRoleAssignments no Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Read/DefaultPolicy | Leia a propriedade ServicePrincipals.DefaultPolicy no Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Read/MemberOf | Ler a propriedade ServicePrincipals.MemberOf no Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Read/OAuth2PermissionGrants | Leia a propriedade ServicePrincipals.OAuth2PermissionGrants no Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Read/Owners | Leia a propriedade ServicePrincipals.Owners no Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Read/OwnedObjects | Leia a propriedade ServicePrincipals.OwnedObjects no Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Update | Atualize propriedades padrão em ServicePrincipals no Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Update/AppRoleAssignedTo | Atualize a propriedade ServicePrincipals.AppRoleAssignedTo no Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Update/AppRoleAssignments | Atualize a propriedade ServicePrincipals.AppRoleAssignments no Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Update/DefaultPolicy | Atualize a propriedade ServicePrincipals.DefaultPolicy no Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Update/Owners | Atualize a propriedade ServicePrincipals.Owners no Azure Active Directory. |
| microsoft.aad.directory/Organization/Update/DirSync | Atualize a propriedade Organizations.DirSync no Azure Active Directory. |
| microsoft.aad.directorysync/AllEntities/AllActions | Execute todas as ações no Azure AD Connect. |

### <a name="directory-writer"></a>Escritor de diretório
Pode ler e gravar informações de diretório básicas. Para conceder acesso a aplicações

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.aad.directory/DirectorySetting/Create | Criar DirectorySettings no Azure Active Directory. |
| microsoft.aad.directory/DirectorySetting/Delete | Eliminar DirectorySettings no Azure Active Directory. |
| microsoft.aad.directory/DirectorySetting/Update | Atualizar as propriedades standard em DirectorySettings no Azure Active Directory. |
| microsoft.aad.directory/Group/Create | Criar Grupos no Azure Active Directory. |
| microsoft.aad.directory/Group/CreateAsOwner | Criar Grupos no Azure Active Directory. Criador é adicionado como o primeiro proprietário e o objeto criado conta contra a quota de 250 objetos criados do criador. |
| microsoft.aad.directory/Group/Read | Leia propriedades padrão nos grupos no Azure Active Directory. |
| microsoft.aad.directory/Group/Update | Atualize propriedades padrão em grupos no Azure Active Directory. |
| microsoft.aad.directory/Group/Update/AppRoleAssignments | Atualizar a propriedade Groups.AppRoleAssignments no Azure Active Directory. |
| microsoft.aad.directory/Group/Update/Members | Atualizar a propriedade Groups.Members no Azure Active Directory. |
| microsoft.aad.directory/Group/Update/Owners | Atualize a propriedade Groups.Owners no Azure Active Directory. |
| microsoft.aad.directory/Group/Update/Settings | Atualizar a propriedade Groups.Settings no Azure Active Directory. |
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Leia a propriedade Organizations.TrustedCAsForPasswordlessAuth no Azure Active Directory. |
| microsoft.aad.directory/User/AssignLicense | Gira licenças em Utilizadores no Azure Active Directory. |
| microsoft.aad.directory/User/InvalidateAllRefreshTokens | Invalide todos os tokens de atualização de utilizador no Azure Active Directory. |
| microsoft.aad.directory/User/Update | Atualize propriedades padrão em utilizadores no Azure Active Directory. |
| microsoft.aad.directory/User/Update/AppRoleAssignments | Atualize a propriedade Users.AppRoleAssignments no Azure Active Directory. |
| microsoft.aad.directory/User/Update/Manager | Atualize a propriedade Users.Manager no Azure Active Directory. |

### <a name="dynamics-365-service-administrator"></a>Administrador de serviços do Dynamics 365
Pode gerir todos os aspetos do produto Dynamics 365.

  > [!NOTE]
  > Esta função herda permissões adicionais do [função de utilizador](https://docs.microsoft.com/azure/active-directory/users-default-permissions).
  >
  >

  > [!NOTE]
  > Esta função tem permissões adicionais fora do Azure Active Directory. Ver Descrição da função acima para obter mais informações.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Leia a propriedade Organizations.TrustedCAsForPasswordlessAuth no Azure Active Directory. |
| microsoft.aad.accessservice/AllEntities/AllActions | Crie e elimine todos os recursos e leia e atualize propriedades padrão no Controlo de Acesso do Azure. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Leia e configure o Estado de Funcionamento do Serviço Office 365. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Crie e gira pedidos de suporte do Office 365. |
| microsoft.crm/AllEntities/AllActions | Gerir todos os aspetos do Dynamics 365. |

### <a name="exchange-service-administrator"></a>Administrador de Serviço Exchange
Pode gerir todos os aspetos do produto Exchange.

  > [!NOTE]
  > Esta função herda permissões adicionais do [função de utilizador](https://docs.microsoft.com/azure/active-directory/users-default-permissions).
  >
  >

  > [!NOTE]
  > Esta função tem permissões adicionais fora do Azure Active Directory. Ver Descrição da função acima para obter mais informações.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.aad.accessservice/AllEntities/AllActions | Crie e elimine todos os recursos e leia e atualize propriedades padrão no Controlo de Acesso do Azure. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Crie e gira pedidos de suporte do Office 365. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Leia e configure o Estado de Funcionamento do Serviço Office 365. |
| microsoft.exchange/AllEntities/AllActions | Gerir todos os aspetos do Exchange Online. |

### <a name="guest-inviter"></a>Utilizador Que Convida Convidados
Pode convidar utilizadores convidados independentemente do **os membros podem convidar convidados** definição.

  > [!NOTE]
  > Esta função herda permissões adicionais da função de convidado.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.aad.directory/User/InviteGuest | Convide utilizadores no Azure Active Directory. |
| microsoft.aad.directory/User/Read | Leia propriedades padrão em utilizadores no Azure Active Directory. |
| microsoft.aad.directory/User/Read/AppRoleAssignments | Leia a propriedade Users.AppRoleAssignments no Azure Active Directory. |
| microsoft.aad.directory/User/Read/DirectReports | Leia a propriedade Users.DirectReports no Azure Active Directory. |
| microsoft.aad.directory/User/Read/InvitedBy | Leia a propriedade Users.InvitedBy no Azure Active Directory. |
| microsoft.aad.directory/User/Read/InvitedUsers | Leia a propriedade Users.InvitedUsers no Azure Active Directory. |
| microsoft.aad.directory/User/Read/Manager | Leia a propriedade Users.Manager no Azure Active Directory. |
| microsoft.aad.directory/User/Read/MemberOf | Leia a propriedade Users.MemberOf no Azure Active Directory. |
| microsoft.aad.directory/User/Read/OAuth2PermissionGrants | Leia a propriedade Users.OAuth2PermissionGrants no Azure Active Directory. |
| microsoft.aad.directory/User/Read/OwnedDevices | Leia a propriedade Users.OwnedDevices no Azure Active Directory. |
| microsoft.aad.directory/User/Read/OwnedObjects | Leia a propriedade Users.OwnedObjects no Azure Active Directory. |
| microsoft.aad.directory/User/Read/RegisteredDevices | Leia a propriedade Users.RegisteredDevices no Azure Active Directory. |

### <a name="helpdesk-administrator"></a>Administrador de Suporte Técnico
Pode repor palavras-passe para não administradores e Administradores de Suporte Técnico.

  > [!NOTE]
  > Esta função herda permissões adicionais do [função de utilizador](https://docs.microsoft.com/azure/active-directory/users-default-permissions).
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Leia a propriedade Organizations.TrustedCAsForPasswordlessAuth no Azure Active Directory. |
| microsoft.aad.directory/User/InvalidateAllRefreshTokens | Invalide todos os tokens de atualização de utilizador no Azure Active Directory. |
| microsoft.aad.directory/User/Update/PasswordHelpdeskScope | Atualize as palavras-passe para administradores limitados e outros administradores de suporte técnico no Azure Active Directory. Consulte a documentação online para obter mais detalhes. |
| microsoft.aad.accessservice/AllEntities/AllActions | Crie e elimine todos os recursos e leia e atualize propriedades padrão no Controlo de Acesso do Azure. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Crie e gira pedidos de suporte do Office 365. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Leia e configure o Estado de Funcionamento do Serviço Office 365. |

### <a name="information-protection-administrator"></a>Administrador do Information Protection
Pode gerir todos os aspetos do produto Azure Information Protection.

  > [!NOTE]
  > Esta função herda permissões adicionais do [função de utilizador](https://docs.microsoft.com/azure/active-directory/users-default-permissions).
  >
  >

  > [!NOTE]
  > Esta função tem permissões adicionais fora do Azure Active Directory. Ver Descrição da função acima para obter mais informações.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.aad.directory/Group/Read | Leia propriedades padrão nos grupos no Azure Active Directory. |
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Leia a propriedade Organizations.TrustedCAsForPasswordlessAuth no Azure Active Directory. |
| microsoft.aad.informationprotection/AllEntities/AllActions | Gerir todos os aspetos do Information Protection. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Leia e configure o Estado de Funcionamento do Serviço Office 365. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Crie e gira pedidos de suporte do Office 365. |

### <a name="intune-service-administrator"></a>Administrador de Serviço Intune
Pode gerir todos os aspetos do produto Intune.

  > [!NOTE]
  > Esta função herda permissões adicionais do [função de utilizador](https://docs.microsoft.com/azure/active-directory/users-default-permissions).
  >
  >

  > [!NOTE]
  > Esta função tem permissões adicionais fora do Azure Active Directory. Ver Descrição da função acima para obter mais informações.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.aad.directory/Contact/Create | Criar contactos no Azure Active Directory. |
| microsoft.aad.directory/Contact/Delete | Eliminar Contactos no Azure Active Directory. |
| microsoft.aad.directory/Contact/Update | Atualizar as propriedades standard de Contactos no Azure Active Directory. |
| microsoft.aad.directory/Device/Create | Crie Dispositivos no Azure Active Directory. |
| microsoft.aad.directory/Device/Delete | Elimine Dispositivos no Azure Active Directory. |
| microsoft.aad.directory/Device/Update | Atualize propriedades padrão nos Dispositivos no Azure Active Directory. |
| microsoft.aad.directory/Device/Update/RegisteredOwners | Atualizar a propriedade Devices.RegisteredOwners no Azure Active Directory. |
| microsoft.aad.directory/Device/Update/RegisteredUsers | Atualizar a propriedade Devices.RegisteredUsers no Azure Active Directory. |
| microsoft.aad.directory/Group/Create | Criar Grupos no Azure Active Directory. |
| microsoft.aad.directory/Group/CreateAsOwner | Criar Grupos no Azure Active Directory. Criador é adicionado como o primeiro proprietário e o objeto criado conta contra a quota de 250 objetos criados do criador. |
| microsoft.aad.directory/Group/Delete | Eliminar Grupos no Azure Active Directory. |
| microsoft.aad.directory/Group/Read | Leia propriedades padrão nos grupos no Azure Active Directory. |
| microsoft.aad.directory/Group/Read/HiddenMembers | Ler a propriedade de Groups.HiddenMembers no Azure Active Directory. |
| microsoft.aad.directory/Group/Restore | Restaure Grupos no Azure Active Directory. |
| microsoft.aad.directory/Group/Update | Atualize propriedades padrão em grupos no Azure Active Directory. |
| microsoft.aad.directory/Group/Update/AppRoleAssignments | Atualizar a propriedade Groups.AppRoleAssignments no Azure Active Directory. |
| microsoft.aad.directory/Group/Update/Members | Atualizar a propriedade Groups.Members no Azure Active Directory. |
| microsoft.aad.directory/Group/Update/Owners | Atualize a propriedade Groups.Owners no Azure Active Directory. |
| microsoft.aad.directory/Group/Update/Settings | Atualizar a propriedade Groups.Settings no Azure Active Directory. |
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Leia a propriedade Organizations.TrustedCAsForPasswordlessAuth no Azure Active Directory. |
| microsoft.aad.directory/User/Update | Atualize propriedades padrão em utilizadores no Azure Active Directory. |
| microsoft.aad.directory/User/Update/AppRoleAssignments | Atualize a propriedade Users.AppRoleAssignments no Azure Active Directory. |
| microsoft.aad.directory/User/Update/Manager | Atualize a propriedade Users.Manager no Azure Active Directory. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Crie e gira pedidos de suporte do Office 365. |
| microsoft.intune/AllEntities/AllActions | Gerir todos os aspetos do Intune. |

### <a name="lync-service-administrator"></a>Administrador do Serviço Lync
Pode gerir todos os aspetos do produto Skype para Empresas.

  > [!NOTE]
  > Esta função herda permissões adicionais do [função de utilizador](https://docs.microsoft.com/azure/active-directory/users-default-permissions).
  >
  >

  > [!NOTE]
  > Esta função tem permissões adicionais fora do Azure Active Directory. Ver Descrição da função acima para obter mais informações.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.aad.accessservice/AllEntities/AllActions | Crie e elimine todos os recursos e leia e atualize propriedades padrão no Controlo de Acesso do Azure. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Leia e configure o Estado de Funcionamento do Serviço Office 365. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Crie e gira pedidos de suporte do Office 365. |
| microsoft.skypeforbusiness/AllEntities/AllActions | Gira o Skype para Empresas Online. |

### <a name="message-center-reader"></a>Leitor do Centro de Mensagens
Pode ler as mensagens e atualizações para a sua organização apenas no Centro de Mensagem do Office 365. 

  > [!NOTE]
  > Esta função herda permissões adicionais da função leitores de diretório.
  >
  >

  > [!NOTE]
  > Esta função tem permissões adicionais fora do Azure Active Directory. Ver Descrição da função acima para obter mais informações.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.aad.directory/Group/Read | Leia propriedades padrão nos grupos no Azure Active Directory. |
| microsoft.aad.accessmessagecenter/AllEntities/AllActions | Criar e eliminar todos os recursos, ler e atualizar as propriedades standard no Centro de Mensagens. |
| microsoft.aad.accessservice/AllEntities/AllActions | Crie e elimine todos os recursos e leia e atualize propriedades padrão no Controlo de Acesso do Azure. |

### <a name="partner-tier1-support"></a>Parceiro de Suporte de Escalão 1
Não utilize – não se destina a utilização geral.

  > [!NOTE]
  > Esta função herda permissões adicionais do [função de utilizador](https://docs.microsoft.com/azure/active-directory/users-default-permissions).
  >
  >

  > [!NOTE]
  > Esta função tem permissões adicionais fora do Azure Active Directory. Ver Descrição da função acima para obter mais informações.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.aad.accessservice/AllEntities/AllActions | Crie e elimine todos os recursos e leia e atualize propriedades padrão no Controlo de Acesso do Azure. |
| microsoft.aad.directory/Contact/Create | Criar contactos no Azure Active Directory. |
| microsoft.aad.directory/Contact/Delete | Eliminar Contactos no Azure Active Directory. |
| microsoft.aad.directory/Contact/Update | Atualizar as propriedades standard de Contactos no Azure Active Directory. |
| microsoft.aad.directory/Group/Create | Criar Grupos no Azure Active Directory. |
| microsoft.aad.directory/Group/CreateAsOwner | Criar Grupos no Azure Active Directory. Criador é adicionado como o primeiro proprietário e o objeto criado conta contra a quota de 250 objetos criados do criador. |
| microsoft.aad.directory/Group/Read | Leia propriedades padrão nos grupos no Azure Active Directory. |
| microsoft.aad.directory/Group/Update/Members | Atualizar a propriedade Groups.Members no Azure Active Directory. |
| microsoft.aad.directory/Group/Update/Owners | Atualize a propriedade Groups.Owners no Azure Active Directory. |
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Leia a propriedade Organizations.TrustedCAsForPasswordlessAuth no Azure Active Directory. |
| microsoft.aad.directory/User/AssignLicense | Gira licenças em Utilizadores no Azure Active Directory. |
| microsoft.aad.directory/User/Delete | Elimine Utilizadores no Azure Active Directory. |
| microsoft.aad.directory/User/InvalidateAllRefreshTokens | Invalide todos os tokens de atualização de utilizador no Azure Active Directory. |
| microsoft.aad.directory/User/Restore | Restaure utilizadores eliminados no Azure Active Directory. |
| microsoft.aad.directory/User/Update | Atualize propriedades padrão em utilizadores no Azure Active Directory. |
| microsoft.aad.directory/User/Update/AppRoleAssignments | Atualize a propriedade Users.AppRoleAssignments no Azure Active Directory. |
| microsoft.aad.directory/User/Update/Manager | Atualize a propriedade Users.Manager no Azure Active Directory. |
| microsoft.aad.directory/User/Update/PasswordUserScope | Atualize as palavras-passe para não administradores no Azure Active Directory. Consulte a documentação online para obter mais detalhes. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Leia e configure o Estado de Funcionamento do Serviço Office 365. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Crie e gira pedidos de suporte do Office 365. |

### <a name="partner-tier2-support"></a>Parceiro de Suporte de Escalão 2
Não utilize – não se destina a utilização geral.

  > [!NOTE]
  > Esta função herda permissões adicionais do [função de utilizador](https://docs.microsoft.com/azure/active-directory/users-default-permissions).
  >
  >

  > [!NOTE]
  > Esta função tem permissões adicionais fora do Azure Active Directory. Ver Descrição da função acima para obter mais informações.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.aad.accessservice/AllEntities/AllActions | Crie e elimine todos os recursos e leia e atualize propriedades padrão no Controlo de Acesso do Azure. |
| microsoft.aad.directory/Contact/Create | Criar contactos no Azure Active Directory. |
| microsoft.aad.directory/Contact/Delete | Eliminar Contactos no Azure Active Directory. |
| microsoft.aad.directory/Contact/Update | Atualizar as propriedades standard de Contactos no Azure Active Directory. |
| microsoft.aad.directory/Domain/AllActions | Criar e eliminar os Domínios, ler e atualizar as propriedades standard no Azure Active Directory. |
| microsoft.aad.directory/Group/Create | Criar Grupos no Azure Active Directory. |
| microsoft.aad.directory/Group/Delete | Eliminar Grupos no Azure Active Directory. |
| microsoft.aad.directory/Group/Read | Leia propriedades padrão nos grupos no Azure Active Directory. |
| microsoft.aad.directory/Group/Restore | Restaure Grupos no Azure Active Directory. |
| microsoft.aad.directory/Group/Update/Members | Atualizar a propriedade Groups.Members no Azure Active Directory. |
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Leia a propriedade Organizations.TrustedCAsForPasswordlessAuth no Azure Active Directory. |
| microsoft.aad.directory/Organization/Update | Atualize propriedades padrão em organizações no Azure Active Directory. |
| microsoft.aad.directory/Organization/Update/TrustedCAsForPasswordlessAuth | Atualize a propriedade Organizations.TrustedCAsForPasswordlessAuth no Azure Active Directory. |
| microsoft.aad.directory/User/AssignLicense | Gira licenças em Utilizadores no Azure Active Directory. |
| microsoft.aad.directory/User/Delete | Elimine Utilizadores no Azure Active Directory. |
| microsoft.aad.directory/User/InvalidateAllRefreshTokens | Invalide todos os tokens de atualização de utilizador no Azure Active Directory. |
| microsoft.aad.directory/User/Restore | Restaure utilizadores eliminados no Azure Active Directory. |
| microsoft.aad.directory/User/Update | Atualize propriedades padrão em utilizadores no Azure Active Directory. |
| microsoft.aad.directory/User/Update/AppRoleAssignments | Atualize a propriedade Users.AppRoleAssignments no Azure Active Directory. |
| microsoft.aad.directory/User/Update/Manager | Atualize a propriedade Users.Manager no Azure Active Directory. |
| microsoft.aad.directory/User/Update/Password | Atualize as palavras-passe para todos os utilizadores no Azure Active Directory. Consulte a documentação online para obter mais detalhes. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Leia e configure o Estado de Funcionamento do Serviço Office 365. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Crie e gira pedidos de suporte do Office 365. |

### <a name="power-bi-service-administrator"></a>Administrador do Power BI
Pode gerir todos os aspetos do produto Power BI.

  > [!NOTE]
  > Esta função herda permissões adicionais do [função de utilizador](https://docs.microsoft.com/azure/active-directory/users-default-permissions).
  >
  >

  > [!NOTE]
  > Esta função tem permissões adicionais fora do Azure Active Directory. Ver Descrição da função acima para obter mais informações.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Leia a propriedade Organizations.TrustedCAsForPasswordlessAuth no Azure Active Directory. |
| microsoft.aad.accessservice/AllEntities/AllActions | Crie e elimine todos os recursos e leia e atualize propriedades padrão no Controlo de Acesso do Azure. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Leia e configure o Estado de Funcionamento do Serviço Office 365. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Crie e gira pedidos de suporte do Office 365. |
| microsoft.powerbi/AllEntities/AllActions | Gerir todos os aspetos do Power BI. |

### <a name="privileged-role-administrator"></a>Administrador de Função com Privilégios 
Pode gerir atribuições de funções no Azure AD

  > [!NOTE]
  > Esta função herda permissões adicionais do [função de utilizador](https://docs.microsoft.com/azure/active-directory/users-default-permissions).
  >
  >

  > [!NOTE]
  > Esta função tem permissões adicionais fora do Azure Active Directory. Ver Descrição da função acima para obter mais informações.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.aad.directory/DirectoryRole/Update | Atualize propriedades padrão em DirectoryRoles no Azure Active Directory. |
| microsoft.aad.privilegedrolemanagement/AllEntities/AllActions | Gerir todos os aspetos do serviço de gestão de funções com privilégios. |

### <a name="reports-reader"></a>Leitor de Relatórios
Pode ler relatórios de auditoria e inícios de sessão.

  > [!NOTE]
  > Esta função herda permissões adicionais da função leitores de diretório.
  >
  >

  > [!NOTE]
  > Esta função tem permissões adicionais fora do Azure Active Directory. Ver Descrição da função acima para obter mais informações.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.aad.reports/AllEntities/Read | Leia Relatórios do Azure AD. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Leia e configure o Estado de Funcionamento do Serviço Office 365. |
| Microsoft.office365.usagereports/AllEntities/Read | Relatórios de utilização de leitura Office 365. |

### <a name="security-administrator"></a>Administrador de Segurança
Pode ler as informações de segurança e relatórios

  > [!NOTE]
  > Esta função herda permissões adicionais do [função de utilizador](https://docs.microsoft.com/azure/active-directory/users-default-permissions).
  >
  >

  > [!NOTE]
  > Esta função tem permissões adicionais fora do Azure Active Directory. Ver Descrição da função acima para obter mais informações.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.aad.directory/Application/Update/DefaultPolicy | Atualizar a propriedade Applications.DefaultPolicy no Azure Active Directory. |
| microsoft.aad.directory/Policy/Create | Criar Políticas no Azure Active Directory. |
| microsoft.aad.directory/Policy/Delete | Eliminar Policies no Azure Active Directory. |
| microsoft.aad.directory/Policy/Update | Atualize propriedades standard nas Políticas no Azure Active Directory. |
| microsoft.aad.directory/Policy/Update/Owners | Atualizar a propriedade Policies.Owners no Azure Active Directory. |
| microsoft.aad.directory/ServicePrincipal/Update/DefaultPolicy | Atualize a propriedade ServicePrincipals.DefaultPolicy no Azure Active Directory. |
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Leia a propriedade Organizations.TrustedCAsForPasswordlessAuth no Azure Active Directory. |
| microsoft.aad.accessservice/AllEntities/AllActions | Crie e elimine todos os recursos e leia e atualize propriedades padrão no Controlo de Acesso do Azure. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Leia e configure o Estado de Funcionamento do Serviço Office 365. |
| microsoft.aad.privilegedrolemanagement/AllEntities/Read | Ler todos os aspetos do Privileged Identity Management. |
| microsoft.protectioncenter/AllEntities/Read | Leia todos os aspetos do Centro de Proteção do Office 365. |
| microsoft.protectioncenter/AllEntities/Update | Gira o Centro de Proteção do Office 365. |

### <a name="security-reader"></a>Leitor de Segurança
Pode ler as informações de segurança e os relatórios no Azure AD e no Office 365.

  > [!NOTE]
  > Esta função herda permissões adicionais da função leitores de diretório.
  >
  >

  > [!NOTE]
  > Esta função tem permissões adicionais fora do Azure Active Directory. Ver Descrição da função acima para obter mais informações.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Leia a propriedade Organizations.TrustedCAsForPasswordlessAuth no Azure Active Directory. |
| microsoft.aad.accessservice/AllEntities/AllActions | Crie e elimine todos os recursos e leia e atualize propriedades padrão no Controlo de Acesso do Azure. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Leia e configure o Estado de Funcionamento do Serviço Office 365. |
| microsoft.privilegedidentitymanagement/AllEntities/Read | Ler todos os aspetos do Privileged Identity Management. |
| microsoft.protectioncenter/AllEntities/Read | Leia todos os aspetos do Centro de Proteção do Office 365. |

### <a name="service-support-administrator"></a>Administrador de Assistência Técnica
Consegue ler as informações do Service Health e gerir pedidos de suporte.

  > [!NOTE]
  > Esta função herda permissões adicionais do [função de utilizador](https://docs.microsoft.com/azure/active-directory/users-default-permissions).
  >
  >

  > [!NOTE]
  > Esta função tem permissões adicionais fora do Azure Active Directory. Ver Descrição da função acima para obter mais informações.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Leia a propriedade Organizations.TrustedCAsForPasswordlessAuth no Azure Active Directory. |
| microsoft.aad.accessservice/AllEntities/AllActions | Crie e elimine todos os recursos e leia e atualize propriedades padrão no Controlo de Acesso do Azure. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Crie e gira pedidos de suporte do Office 365. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Leia e configure o Estado de Funcionamento do Serviço Office 365. |

### <a name="sharepoint-service-administrator"></a>Administrador de Serviços do SharePoint
Pode gerir todos os aspetos do serviço do SharePoint.

  > [!NOTE]
  > Esta função herda permissões adicionais do [função de utilizador](https://docs.microsoft.com/azure/active-directory/users-default-permissions).
  >
  >

  > [!NOTE]
  > Esta função tem permissões adicionais fora do Azure Active Directory. Ver Descrição da função acima para obter mais informações.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.aad.accessservice/AllEntities/AllActions | Crie e elimine todos os recursos e leia e atualize propriedades padrão no Controlo de Acesso do Azure. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Leia e configure o Estado de Funcionamento do Serviço Office 365. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Crie e gira pedidos de suporte do Office 365. |
| microsoft.sharepoint/AllEntities/AllActions | Gira o SharePoint Online. |

### <a name="user-account-administrator"></a>Administrador de Conta de Utilizador
Pode gerir todos os aspetos de utilizadores e grupos

  > [!NOTE]
  > Esta função herda permissões adicionais do [função de utilizador](https://docs.microsoft.com/azure/active-directory/users-default-permissions).
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.aad.directory/AppRoleAssignment/Create | Crie AppRoleAssignments no Azure Active Directory. |
| microsoft.aad.directory/AppRoleAssignment/Delete | Eliminar AppRoleAssignments no Azure Active Directory. |
| microsoft.aad.directory/AppRoleAssignment/Update | Atualize propriedades padrão em AppRoleAssignments no Azure Active Directory. |
| microsoft.aad.directory/Contact/Create | Criar contactos no Azure Active Directory. |
| microsoft.aad.directory/Contact/Delete | Eliminar Contactos no Azure Active Directory. |
| microsoft.aad.directory/Contact/Update | Atualizar as propriedades standard de Contactos no Azure Active Directory. |
| microsoft.aad.directory/Group/Create | Criar Grupos no Azure Active Directory. |
| microsoft.aad.directory/Group/CreateAsOwner | Criar Grupos no Azure Active Directory. Criador é adicionado como o primeiro proprietário e o objeto criado conta contra a quota de 250 objetos criados do criador. |
| microsoft.aad.directory/Group/Delete | Eliminar Grupos no Azure Active Directory. |
| microsoft.aad.directory/Group/Read | Leia propriedades padrão nos grupos no Azure Active Directory. |
| microsoft.aad.directory/Group/Read/HiddenMembers | Ler a propriedade de Groups.HiddenMembers no Azure Active Directory. |
| microsoft.aad.directory/Group/Restore | Restaure Grupos no Azure Active Directory. |
| microsoft.aad.directory/Group/Update | Atualize propriedades padrão em grupos no Azure Active Directory. |
| microsoft.aad.directory/Group/Update/AppRoleAssignments | Atualizar a propriedade Groups.AppRoleAssignments no Azure Active Directory. |
| microsoft.aad.directory/Group/Update/Members | Atualizar a propriedade Groups.Members no Azure Active Directory. |
| microsoft.aad.directory/Group/Update/Owners | Atualize a propriedade Groups.Owners no Azure Active Directory. |
| microsoft.aad.directory/Group/Update/Settings | Atualizar a propriedade Groups.Settings no Azure Active Directory. |
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Leia a propriedade Organizations.TrustedCAsForPasswordlessAuth no Azure Active Directory. |
| microsoft.aad.directory/User/AssignLicense | Gira licenças em Utilizadores no Azure Active Directory. |
| microsoft.aad.directory/User/Create | Crie Utilizadores no Azure Active Directory. |
| microsoft.aad.directory/User/Delete | Elimine Utilizadores no Azure Active Directory. |
| microsoft.aad.directory/User/InvalidateAllRefreshTokens | Invalide todos os tokens de atualização de utilizador no Azure Active Directory. |
| microsoft.aad.directory/User/Restore | Restaure utilizadores eliminados no Azure Active Directory. |
| microsoft.aad.directory/User/Update | Atualize propriedades padrão em utilizadores no Azure Active Directory. |
| microsoft.aad.directory/User/Update/AppRoleAssignments | Atualize a propriedade Users.AppRoleAssignments no Azure Active Directory. |
| microsoft.aad.directory/User/Update/Manager | Atualize a propriedade Users.Manager no Azure Active Directory. |
| microsoft.aad.directory/User/Update/PasswordUserAcctAdminScope | Atualize as palavras-passe para administradores limitados, administradores do suporte técnico e outros administradores de conta de utilizador no Azure Active Directory. Consulte a documentação online para obter mais detalhes. |
| microsoft.aad.accessservice/AllEntities/AllActions | Crie e elimine todos os recursos e leia e atualize propriedades padrão no Controlo de Acesso do Azure. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Crie e gira pedidos de suporte do Office 365. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Leia e configure o Estado de Funcionamento do Serviço Office 365. |

## <a name="next-steps"></a>Passos Seguintes

* Para saber mais sobre como alterar os administradores para uma subscrição do Azure, consulte [Como adicionar ou alterar funções de administrador do Azure](../../billing/billing-add-change-azure-subscription-administrator.md)
* Para saber mais sobre como o Microsoft Azure controla o acesso aos recursos, consulte [Noções sobre o acesso aos recursos no Azure](../../role-based-access-control/rbac-and-directory-admin-roles.md)
* Para obter mais informações sobre como o Azure Active Directory está relacionado com a sua subscrição do Azure, veja [Como as subscrições do Azure estão associadas ao Azure Active Directory](../fundamentals/active-directory-how-subscriptions-associated-directory.md)
