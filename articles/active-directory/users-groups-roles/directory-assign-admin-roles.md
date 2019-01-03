---
title: Permissões de função de administrador no Azure Active Directory | Documentos da Microsoft
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
ms.date: 10/26/2018
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.openlocfilehash: 75fe35a22ccae249b734f05bd4adcaf8ddcab9f8
ms.sourcegitcommit: fd488a828465e7acec50e7a134e1c2cab117bee8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/03/2019
ms.locfileid: "53995104"
---
# <a name="administrator-role-permissions-in-azure-active-directory"></a>Permissões de função de administrador no Azure Active Directory

Utilizar o Azure Active Directory (Azure AD), pode designar administradores separados para servir as funções diferentes. Os administradores podem ser indicados no portal do Azure AD para realizar tarefas como adicionar ou alterar os utilizadores, atribuir funções administrativas, repor palavras-passe do utilizador, gerir licenças de utilizador e gerir nomes de domínio.

O Administrador Global tenha acesso a todas as funcionalidades administrativas. Por predefinição, a pessoa que se inscreve para uma subscrição do Azure é atribuída a função de Administrador Global do diretório. Apenas os administradores globais e administradores de função com privilégios, podem delegar funções de administrador.

## <a name="assign-or-remove-administrator-roles"></a>Atribuir ou remover funções de administrador

Para saber como atribuir funções administrativas a um utilizador no Azure Active Directory, veja [modo de exibição e atribuir funções de administrador no Azure Active Directory](directory-manage-roles-portal.md).

## <a name="available-roles"></a>Funções disponíveis

As seguintes funções de administrador estão disponíveis:

* **[Administrador da aplicação](#application-administrator)**: Os utilizadores nesta função podem criar e gerir todos os aspetos de aplicações empresariais, registos de aplicação e as definições de proxy da aplicação. Esta função também concede a capacidade de dar consentimento a permissões delegadas e permissões de aplicação, excluindo o Microsoft Graph e o Azure AD Graph. Os membros desta função não são adicionados como proprietários durante a criação de novos registos de aplicação ou aplicações da empresa.

  <b>Importante</b>: Esta função concede a capacidade de gerir as credenciais do aplicativo. Utilizadores atribuídos esta função podem adicionar as credenciais para uma aplicação e usar as credencias para representar a identidade da aplicação. Se a identidade da aplicação tenha sido concedida acesso ao Azure Active Directory, como a capacidade de criar ou atualizar o utilizador ou outros objetos, um utilizador atribuído a esta função foi execute essas ações quando for representar o aplicativo. Esta capacidade para representar a identidade da aplicação pode ser uma elevação do privilégio ao longo do que o utilizador pode fazer por meio de suas atribuições de funções no Azure AD. É importante compreender que atribuir um utilizador à função de administrador da aplicação fornece a capacidade para representar a identidade de um aplicativo.

* **[Desenvolvedor de aplicativos](#application-developer)**: Os utilizadores nesta função podem criar registos de aplicações quando o "Os utilizadores podem registar aplicações" definição está definida como não. Esta função também permite que os membros dar consentimento em seu próprio benefício quando o "Os utilizadores podem dar consentimento a aplicações acedam aos dados da empresa em nome deles" definição está definida como não. Os membros desta função são adicionados como proprietários durante a criação de novos registos de aplicação ou aplicações da empresa.

* **[Administrador de faturação](#billing-administrator)**: Efetua compras, gere subscrições, gere pedidos de suporte e monitoriza o estado de funcionamento do serviço.

* **[Administrador da aplicação cloud](#cloud-application-administrator)**: Os utilizadores nesta função têm as mesmas permissões que a função de administrador da aplicação, excluindo a capacidade de gerir o proxy de aplicações. Esta função concede a capacidade de criar e gerir todos os aspetos de aplicações empresariais e registos de aplicação. Esta função também concede a capacidade de dar consentimento a permissões delegadas e permissões de aplicação, excluindo o Microsoft Graph e o Azure AD Graph. Os membros desta função não são adicionados como proprietários durante a criação de novos registos de aplicação ou aplicações da empresa.

  <b>Importante</b>: Esta função concede a capacidade de gerir as credenciais do aplicativo. Utilizadores atribuídos esta função podem adicionar as credenciais para uma aplicação e usar as credencias para representar a identidade da aplicação. Se a identidade da aplicação tenha sido concedida acesso ao Azure Active Directory, como a capacidade de criar ou atualizar o utilizador ou outros objetos, um utilizador atribuído a esta função foi execute essas ações quando for representar o aplicativo. Esta capacidade para representar a identidade da aplicação pode ser uma elevação do privilégio ao longo do que o utilizador pode fazer por meio de suas atribuições de funções no Azure AD. É importante compreender que atribuir um utilizador à função de administrador da aplicação Cloud fornece a capacidade para representar a identidade de um aplicativo.

* **[Administrador de dispositivos na cloud](#cloud-device-administrator)**: Os utilizadores nesta função podem ativar, desativar e eliminar dispositivos no Azure AD e ler as chaves do BitLocker do Windows 10 (caso exista) no portal do Azure. A função não concede permissões para gerir todas as propriedades no dispositivo.

* **[Administrador de conformidade](#compliance-administrator)**: Os utilizadores com esta função possuem permissões de gestão no Centro de Segurança e Conformidade do Office 365 e no Centro Administração do Exchange. Mais informações em [funções de administrador sobre o Office 365](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

* **[Administrador de acesso condicional](#conditional-access-administrator)**: Os utilizadores com esta função têm a capacidade de gerir as definições de acesso condicional do Azure Active Directory.
  > [!NOTE]
  > Para implementar a política de acesso condicional do Exchange ActiveSync no Azure, o utilizador também tem de ser um Administrador Global.
  
* **[Administradores de dispositivos](#device-administrators)**: Esta função está disponível para atribuição apenas como um administrador local adicional na [definições do dispositivo](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/DevicesMenuBlade/DeviceSettings/menuId/). Os utilizadores com esta função tornam-se administradores de computadores locais em todos os dispositivos Windows 10 que estão associados ao Azure Active Directory. Não têm a capacidade de gerir objetos de dispositivos no Azure Active Directory. 

* **[Leitores de diretório](#directory-readers)**: Esta é uma função legada que está a ser atribuída às aplicações que não suportam o [consentimento Framework](../develop/quickstart-v1-integrate-apps-with-azure-ad.md). Não deve ser atribuído a quaisquer utilizadores.

* **[Contas de sincronização de diretórios](#directory-synchronization-accounts)**: Não utilize. Esta função é atribuída automaticamente ao serviço Azure AD Connect e não é a finalidade ou suportada para qualquer outro uso.

* **[Gravadores de diretórios](#directory-writers)**: Esta é uma função legada que está a ser atribuída às aplicações que não suportam o [consentimento Framework](../develop/quickstart-v1-integrate-apps-with-azure-ad.md). Não deve ser atribuído a quaisquer utilizadores.

* **[Administrador do Dynamics 365 / administrador de CRM](#dynamics-365-administrator)**: Os utilizadores com esta função possuem permissões globais no Microsoft Dynamics 365 Online, quando o serviço está presente, bem como a capacidade para gerir pedidos de suporte e monitorizar o estado de funcionamento do serviço. Mais informações em [utilizar a função de administrador de serviço para gerir o seu inquilino](https://docs.microsoft.com/dynamics365/customer-engagement/admin/use-service-admin-role-manage-tenant).
  > [!NOTE] 
  > No Microsoft Graph API, o Azure AD Graph API e o Azure AD PowerShell, esta função é identificada como "Administrador de serviço do Dynamics 365". É "Administrador do Dynamics 365" no portal do Azure.

* **[Administrador do Exchange](#exchange-administrator)**: Os utilizadores com esta função têm permissões globais dentro do Microsoft Exchange Online, quando o serviço está presente. bem como a capacidade de criar e gerir todos os grupos do Office 365, gerir pedidos de suporte e monitorizar o estado de funcionamento do serviço. Mais informações em [funções de administrador sobre o Office 365](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).
  > [!NOTE]
  > No Microsoft Graph API, o Azure AD Graph API e o Azure AD PowerShell, esta função é identificada como "administrador de serviço Exchange". É "administrador do Exchange" no portal do Azure.

* **[Administrador global / administrador de empresa](#company-administrator)**: Os utilizadores com esta função têm acesso a todas as funcionalidades administrativas no Azure Active Directory, bem como serviços que utilizam identidades do Azure Active Directory como o Exchange Online, SharePoint Online e Skype para empresas Online. A pessoa que se inscreve no inquilino do Azure Active Directory torna-se um administrador global. Apenas os administradores globais podem atribuir outras funções de administrador. Podem existir mais do que um administrador global na sua empresa. Os administradores globais podem redefinir a palavra-passe para qualquer utilizador e a todos os outros administradores.

  > [!NOTE]
  > No Microsoft Graph API, o Azure AD Graph API e o Azure AD PowerShell, esta função é identificada como "Administrador de empresa". É "Administrador Global" no [portal do Azure](https://portal.azure.com).
  >
  >

* **[Autor de convites](#guest-inviter)**: Os utilizadores nesta função podem gerir convites de utilizadores do Azure Active Directory B2B quando os **os membros podem convidar** definição de utilizador está definida como não. Obter mais informações sobre a colaboração B2B ao [colaboração sobre o Azure AD B2B](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b). Não inclui quaisquer outras permissões.

* **[Administrador do Information Protection](#information-protection-administrator)**: Os utilizadores com esta função têm todas as permissões no serviço do Azure Information Protection. Esta função permite configurar etiquetas para a política do Azure Information Protection, gestão de modelos de proteção e ativar a proteção. Esta função não concede quaisquer permissões no Centro de proteção de identidade, o Privileged Identity Management, o Monitor Office 365 Service Health, ou o segurança do Office 365 e o Centro de conformidade.

* **[Administrador do Intune](#intune-administrator)**: Os utilizadores com esta função possuem permissões globais no Microsoft Intune Online, quando o serviço está presente. Além disso, esta função contém a capacidade de gerir utilizadores e dispositivos para associar a política, bem como criar e gerir grupos. Mais informações em [o controlo de administração baseada em funções (RBAC) com o Microsoft Intune](https://docs.microsoft.com/intune/role-based-access-control)
  > [!NOTE]
  > No Microsoft Graph API, o Azure AD Graph API e o Azure AD PowerShell, esta função é identificada como "administrador do serviço Intune". É "administrador do Intune" no portal do Azure.

* **[Administrador de licenças](#license-administrator)**: Os utilizadores nesta função podem adicionar, remover e atualizar as atribuições de utilizadores, grupos (com licenciamento baseado em grupo) de licença e gerir a localização de utilização sobre os usuários. A função não concede a capacidade de comprar ou gerir as subscrições, criar ou gerir grupos, ou criar ou gerir utilizadores além da localização de utilização.

* **[Leitor do Centro de mensagens](#message-center-reader)**: Os utilizadores nesta função podem monitorizar notificações e atualizações de estado de funcionamento de consultadoria [Centro de mensagens do Office 365](https://support.office.com/article/Message-center-in-Office-365-38FB3333-BFCC-4340-A37B-DEDA509C2093) para a sua organização em serviços configurados como o Exchange, o Intune e o Microsoft Teams. Os leitores de centro de mensagens recebem resumos de e-mail semanal de postagens, atualizações e podem partilhar publicações do Centro de mensagens no Office 365. No Azure AD, os utilizadores atribuídos a esta função só terão acesso só de leitura nos serviços do Azure AD, como utilizadores e grupos. 

* **[Suporte de escalão 1 de parceiros](#partner-tier1-support)**: Não utilize. Esta função foi preterida e será removida do Azure AD no futuro. Esta função é destinada à utilização por um pequeno número de parceiros de revenda da Microsoft e não se destina a utilização geral.

* **[Suporte de escalão 2 de parceiros](#partner-tier2-support)**: Não utilize. Esta função foi preterida e será removida do Azure AD no futuro. Esta função é destinada à utilização por um pequeno número de parceiros de revenda da Microsoft e não se destina a utilização geral.

* **[Palavra-passe de administrador / administrador de suporte técnico](#helpdesk-administrator)**: Os utilizadores com esta função podem alterar palavras-passe, invalidar os tokens de atualização, gerir pedidos de serviço e monitorizar o estado de funcionamento do serviço. Invalidar um token de atualização força o utilizador iniciar sessão novamente. Os administradores de suporte técnico podem repor palavras-passe e invalidar os tokens de atualização de outros utilizadores que são não-administradores ou membros de apenas as seguintes funções:
  * Leitores de Diretório
  * Utilizador Que Convida Convidados
  * Administrador de Suporte Técnico
  * Leitor do Centro de Mensagens
  * Leitor de Relatórios
  
  <b>Importante</b>: Os utilizadores com esta função podem alterar as palavras-passe para as pessoas que podem ter acesso a informações confidenciais ou privadas ou críticos de configuração dentro e fora do Azure Active Directory. Alterar a palavra-passe de um utilizador pode significar que a capacidade de assumir a identidade e permissões do usuário. Por exemplo:
  * Registo de aplicação e aplicação empresarial proprietários, que podem gerir as credenciais de aplicações que possuem. Esses aplicativos podem ter privilegiado permissões no Azure AD e noutro local não é concedido a administradores de suporte técnico. Através deste caminho um administrador de suporte técnico poderá assumir a identidade de um proprietário da aplicação e, em seguida, mais assumir a identidade de um aplicativo com privilégios ao atualizar as credenciais para a aplicação.
  * Proprietários de subscrições do Azure, que poderão ter acesso a informações confidenciais ou privadas ou críticos de configuração no Azure.
  * Grupo de segurança e de grupo do Office 365 proprietários, que podem gerir a associação de grupo. Esses grupos podem conceder acesso a informações confidenciais ou privadas ou críticos de configuração no Azure AD e em outros locais.
  * Os administradores em outros serviços fora do Azure AD, como sistemas de Exchange Online, segurança do Office e Centro de conformidade e de recursos humanos.
  * Os não administradores como executivos, assessores jurídicos e funcionários de recursos humanos que podem ter acesso a informações confidenciais ou privadas.

  > [!NOTE]
  > No Microsoft Graph API, o Azure AD Graph API e o Azure AD PowerShell, esta função é identificada como "Administrador de suporte técnico". É "Administrador de palavras-passe" na [portal do Azure](https://portal.azure.com/).
  >
  
* **[Administrador do Power BI](#power-bi-administrator)**: Os utilizadores com esta função possuem permissões globais dentro do Microsoft Power BI, quando o serviço está presente, bem como a capacidade para gerir pedidos de suporte e monitorizar o estado de funcionamento do serviço. Mais informações em [compreender a função de administrador do Power BI](https://docs.microsoft.com/power-bi/service-admin-role).
  > [!NOTE]
  > No Microsoft Graph API, o Azure AD Graph API e o Azure AD PowerShell, esta função é identificada como "administrador do Power BI". É "Administrador do Power BI" no portal do Azure.

* **[Com privilégios administrador de função](#privileged-role-administrator)**: Os utilizadores com esta função podem gerir atribuições de funções no Azure Active Directory, assim como no Azure AD Privileged Identity Management. Além disso, esta função permite a gestão de todos os aspetos do Privileged Identity Management.

  <b>Importante</b>: Esta função concede a capacidade de gerir a associação em todas as funções do Azure AD, incluindo a função de Administrador Global. Esta função não inclui quaisquer outras capacidades com privilégios no Azure AD, como a criação ou atualização dos utilizadores. No entanto, os utilizadores atribuídos a esta função podem conceder próprios ou outros privilégios adicionais através da atribuição de funções adicionais.

* **[Leitor de relatórios](#reports-reader)**: Os utilizadores com esta função podem ver os dados de relatórios de utilização e o dashboard de relatórios no Centro de administração do Office 365 e o contexto de adoção do pacote no Power BI. Além disso, a função fornece acesso de início de sessão relatórios e a atividade no Azure AD e a API de relatórios de dados devolvidos pelo Microsoft Graph. Pode aceder a um utilizador atribuído à função do leitor de relatórios apenas utilização relevante e métricas de adoção. Eles não tem quaisquer permissões de administrador para configurar as definições ou o acesso que do Centro de administração da específicos de produtos como o Exchange. 

* **[Administrador de segurança](#security-administrator)**: Os utilizadores com esta função têm todas as permissões só de leitura de função do leitor de segurança, além da capacidade para gerir a configuração de serviços relacionados com segurança: O Azure Active Directory Identity Protection, Azure Information Protection e o Office 365 Centro de segurança e conformidade. Obter mais informações sobre as permissões do Office 365 estão disponíveis em [permissões no Centro de conformidade de segurança do Office 365 e](https://support.office.com/article/Permissions-in-the-Office-365-Security-Compliance-Center-d10608af-7934-490a-818e-e68f17d0e9c1).
  
  | Em | Pode fazer |
  | --- | --- |
  | Identity Protection Center |<ul><li>Todas as permissões da função do leitor de segurança.<li>Além disso, a capacidade de executar todas as operações de IPC, exceto para repor palavras-passe. |
  | Privileged Identity Management |<ul><li>Todas as permissões da função do leitor de segurança.<li>**Não é possível** gerir definições ou associações das funções do Azure AD. |
  | <p>O estado de funcionamento do monitor do Office 365 serviço</p><p>Centro de Segurança e Conformidade do Office 365 |<ul><li>Todas as permissões da função do leitor de segurança.<li>Pode configurar todas as definições no recurso de proteção avançada contra ameaças (proteção contra vírus e software, configuração de URL maliciosa, rastreamento de URL, etc.). |
  
* **[Leitor de segurança](#security-reader)**: Os utilizadores com esta função têm acesso global só de leitura, incluindo todas as informações no Azure Active Directory, Identity Protection, Privileged Identity Management, bem como a capacidade de ler relatórios de início de sessão no Azure Active Directory e registos de auditoria. A função também concede permissão só de leitura no Centro de conformidade e segurança do Office 365. Obter mais informações sobre as permissões do Office 365 estão disponíveis em [permissões no Centro de conformidade de segurança do Office 365 e](https://support.office.com/article/Permissions-in-the-Office-365-Security-Compliance-Center-d10608af-7934-490a-818e-e68f17d0e9c1).

  | Em | Pode fazer |
  | --- | --- |
  | Identity Protection Center |Ler todos os relatórios de segurança e informações das definições de funcionalidades de segurança<ul><li>Anti-spam<li>Encriptação<li>Prevenção de perda de dados<li>Antimalware<li>Proteção avançada contra ameaças<li>Anti-phishing<li>Regras de fluxo de mensagens |
  | Privileged Identity Management |<p>Tem acesso só de leitura para todas as informações apresentadas no PIM do Azure AD: As políticas e relatórios para atribuições de funções do Azure AD, o security analisa e acesso aos dados de política e relatórios para cenários, além de atribuição de função do Azure AD de leitura no futuro.<p>**Não é possível** inscrever-se no PIM do Azure AD ou fazer alterações ao mesmo. No portal do PIM ou através do PowerShell, alguém nesta função pode ativar as funções adicionais (por exemplo, Administrador Global ou administrador com função privilegiada), se o utilizador é um candidato para eles. |
  | <p>O estado de funcionamento do monitor do Office 365 serviço</p><p>Centro de Segurança e Conformidade do Office 365</p> |<ul><li>Leitura e gerir alertas<li>Políticas de segurança de leitura<li>Ler informações sobre ameaças, deteção de aplicações na Cloud e quarentena em pesquisa e investigar<li>Ler todos os relatórios |

* **[Administrador de assistência técnica](#service-support-administrator)**: Os utilizadores com esta função podem abrir pedidos de suporte com os serviços Microsoft para o Azure e o Office 365, bem como visualizar o dashboard de serviço e o centro de mensagens no portal do Azure e no portal de administração do Office 365. Mais informações em [funções de administrador sobre o Office 365](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

* **[Administrador do SharePoint](#sharepoint-administrator)**: Os utilizadores com esta função possuem permissões globais dentro do Microsoft SharePoint Online, quando o serviço está presente, bem como a capacidade de criar e gerir todos os grupos do Office 365, gerir pedidos de suporte e monitorizar o estado de funcionamento do serviço. Mais informações em [funções de administrador sobre o Office 365](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).
  > [!NOTE]
  > No Microsoft Graph API, o Azure AD Graph API e o Azure AD PowerShell, esta função é identificada como "administrador do serviço SharePoint." É "administrador do SharePoint" no portal do Azure.

* **[Skype para empresas / administrador Lync](#skype-for-business-administrator)**: Os utilizadores com esta função têm permissões globais no Microsoft Skype para empresas, quando o serviço está presente, bem como gerir os atributos de utilizador do Skype específicos no Azure Active Directory. Além disso, esta função concede a capacidade para gerir pedidos de suporte e monitorizar o estado de funcionamento do serviço e para acessar as Equipes e o Skype para empresas do Centro de administração. A conta também têm de estar licenciada para equipas ou não é possível executar cmdlets do PowerShell de Equipes. Mais informações em [sobre o Skype para a função de administrador de negócios](https://support.office.com/article/about-the-skype-for-business-admin-role-aeb35bda-93fc-49b1-ac2c-c74fbeb737b5) e as equipas de informações de licenciamento em [Skype para empresas e o Microsoft Teams suplemento licenciamento](https://docs.microsoft.com/skypeforbusiness/skype-for-business-and-microsoft-teams-add-on-licensing/skype-for-business-and-microsoft-teams-add-on-licensing)

  > [!NOTE]
  > No Microsoft Graph API, o Azure AD Graph API e o Azure AD PowerShell, esta função é identificada como "Administrador do serviço Lync". É "Administrador de Skype para empresas" no [portal do Azure](https://portal.azure.com/).

* **[As equipes de administrador de comunicações](#teams-communications-administrator)**: Os utilizadores nesta função podem gerir aspectos da carga de trabalho Microsoft Teams relacionadas com a voz e telefonia. Isto inclui as ferramentas de gestão para atribuição de número de telefone, diretivas de voz e de reunião e acesso total para o conjunto de ferramentas de análise de chamada.

* **[As equipes de engenheiro de suporte de comunicações](#teams-communications-support-engineer)**: Os utilizadores nesta função podem resolver problemas de comunicação dentro do Microsoft Teams e o Skype para empresas através da chamada de utilizador ferramentas no Microsoft Teams & Skype para o Centro de administração de empresas de resolução de problemas. Os utilizadores nesta função podem ver informações de registo de chamadas completa para todos os participantes envolvidos.

* **[As equipes especialistas de suporte de comunicações](#teams-communications-support-specialist)**: Os utilizadores nesta função podem resolver problemas de comunicação dentro do Microsoft Teams e o Skype para empresas através da chamada de utilizador ferramentas no Microsoft Teams & Skype para o Centro de administração de empresas de resolução de problemas. Os utilizadores nesta função apenas podem ver os detalhes de utilizador na chamada para o usuário específico que tem procurado.

* **[As equipes de administrador](#teams-administrator)**: Os utilizadores nesta função podem gerir todos os aspetos da carga de trabalho Microsoft Teams através do Skype e Microsoft Teams para o Centro de administração de negócios e os respectivos módulos do PowerShell. Isto inclui, entre outras áreas, todas as ferramentas de gestão relacionados com a telefonia, mensagens, reuniões e as equipes propriamente ditas. Esta função concede, além disso, a capacidade de criar e gerir todos os grupos do Office 365, gerir pedidos de suporte e monitorizar o estado de funcionamento do serviço.
  > [!NOTE]
  > No Microsoft Graph API, o Azure AD Graph API e o Azure AD PowerShell, esta função é identificada como "as Equipes de administrador de serviços". É "Administrador de Equipes" no portal do Azure.

* **[Administrador de conta de utilizador](#user-account-administrator)**: Os utilizadores com esta função podem criar utilizadores e gerir todos os aspetos de utilizadores com algumas restrições (ver abaixo). Além disso, os utilizadores com esta função podem criar e gerir todos os grupos. Esta função também inclui a capacidade de criar e gerir vistas de utilizador, gerir pedidos de suporte e monitorizar o estado de funcionamento do serviço.

  | | |
  | --- | --- |
  |Permissões gerais|<p>Criar utilizadores e grupos</p><p>Criar e gerir vistas de utilizador</p><p>Gerir pedidos de suporte do Office|
  |<p>Em todos os utilizadores, incluindo todos os administradores</p>|<p>Gerir licenças</p><p>Gerir todas as propriedades de utilizador, exceto o nome Principal de utilizador</p>
  |Apenas em utilizadores que são não-administradores ou em qualquer um dos seguintes limitada funções de administrador:<ul><li>Leitores de Diretório<li>Utilizador Que Convida Convidados<li>Administrador de Suporte Técnico<li>Leitor do Centro de Mensagens<li>Leitor de Relatórios<li>Administrador de Conta de Utilizador|<p>Eliminar e restauro</p><p>Desativar e ativar</p><p>Invalidar Tokens de atualização</p><p>Gerir todas as propriedades de utilizador, incluindo o nome Principal de utilizador</p><p>Repor palavra-passe</p><p>Atualizar as chaves de dispositivo (FIDO)</p>
  
  <b>Importante</b>: Os utilizadores com esta função podem alterar as palavras-passe para as pessoas que podem ter acesso a informações confidenciais ou privadas ou críticos de configuração dentro e fora do Azure Active Directory. Alterar a palavra-passe de um utilizador pode significar que a capacidade de assumir a identidade e permissões do usuário. Por exemplo:
  * Registo de aplicação e aplicação empresarial proprietários, que podem gerir as credenciais de aplicações que possuem. Esses aplicativos podem ter privilegiado permissões no Azure AD e noutro local não é concedido a administradores de utilizador. Através deste caminho um administrador do utilizador poderá assumir a identidade de um proprietário da aplicação e, em seguida, mais assumir a identidade de um aplicativo com privilégios ao atualizar as credenciais para a aplicação.
  * Proprietários de subscrições do Azure, que poderão ter acesso a informações confidenciais ou privadas ou críticos de configuração no Azure.
  * Grupo de segurança e de grupo do Office 365 proprietários, que podem gerir a associação de grupo. Esses grupos podem conceder acesso a informações confidenciais ou privadas ou críticos de configuração no Azure AD e em outros locais.
  * Os administradores em outros serviços fora do Azure AD, como sistemas de Exchange Online, segurança do Office e Centro de conformidade e de recursos humanos.
  * Os não administradores como executivos, assessores jurídicos e funcionários de recursos humanos que podem ter acesso a informações confidenciais ou privadas.

## <a name="role-permissions"></a>Permissões de função
As tabelas seguintes descrevem as permissões específicas no Azure Active Directory tendo em conta a cada função. Algumas funções podem ter permissões adicionais nos serviços da Microsoft fora do Azure Active Directory.

### <a name="application-administrator"></a>Administrador de Aplicações
Pode criar e gerir todos os aspetos de registos de aplicações e aplicações empresariais.

| **Ações** | **Descrição** |
| --- | --- |
| Microsoft.aad.Directory/Applications/Audience/Update | Atualizar a propriedade applications.audience no Azure Active Directory. |
| Microsoft.aad.Directory/Applications/Authentication/Update | Atualizar a propriedade applications.authentication no Azure Active Directory. |
| Microsoft.aad.Directory/Applications/Basic/Update | Atualize as propriedades básicas das aplicações no Azure Active Directory. |
| Microsoft.aad.Directory/Applications/Create | Criar aplicações no Azure Active Directory. |
| Microsoft.aad.Directory/Applications/credentials/Update | Atualizar a propriedade applications.credentials no Azure Active Directory. |
| Microsoft.aad.Directory/Applications/DELETE | Eliminar aplicações no Azure Active Directory. |
| Microsoft.aad.Directory/Applications/Owners/Update | Atualizar a propriedade applications.owners no Azure Active Directory. |
| Microsoft.aad.Directory/Applications/Permissions/Update | Atualizar a propriedade applications.permissions no Azure Active Directory. |
| Microsoft.aad.Directory/Applications/Policies/Update | Atualizar a propriedade applications.policies no Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/create | Criar appRoleAssignments no Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/read | Ler appRoleAssignments no Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/update | Atualizar appRoleAssignments no Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/delete | Eliminar appRoleAssignments no Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/basic/read | Ler a propriedade policies.applicationConfiguration no Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/basic/update | Atualizar a propriedade policies.applicationConfiguration no Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/create | Criar políticas no Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/delete | Eliminar políticas no Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/owners/read | Ler a propriedade policies.applicationConfiguration no Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/owners/update | Atualizar a propriedade policies.applicationConfiguration no Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/policyAppliedTo/read | Ler a propriedade policies.applicationConfiguration no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/basic/update | Atualizar propriedades básicas nos servicePrincipals no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/create | Criar servicePrincipals no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/delete | Eliminar servicePrincipals no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/update | Atualizar a propriedade servicePrincipals.appRoleAssignedTo no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/update | Atualizar a propriedade servicePrincipals.appRoleAssignments no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/owners/update | Atualizar a propriedade servicePrincipals.owners no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/policies/update | Atualizar a propriedade servicePrincipals.policies no Azure Active Directory. |
| microsoft.aad.reports/applicationAuditLogs/read | Leia applicationAuditLogs nos relatórios do Azure AD. |
| microsoft.aad.reports/applicationSignInReports/read | Leia applicationSignInReports nos relatórios do Azure AD. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Ler e configurar o Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Criar e gerir pedidos de suporte do Azure. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Leia e configure o Estado de Funcionamento do Serviço Office 365. |
| Microsoft.office365.supportTickets/allEntities/allTasks | Crie e gira pedidos de suporte do Office 365. |

### <a name="application-developer"></a>Programador de Aplicações
Pode criar registos de aplicações independentemente do "os utilizadores podem registar aplicações" definição.

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.aad.directory/applications/createAsOwner | Criar aplicações no Azure Active Directory. Criador é adicionado como o primeiro proprietário e o objeto criado conta contra a quota de 250 objetos criados do criador. |
| microsoft.aad.directory/appRoleAssignments/createAsOwner | Criar appRoleAssignments no Azure Active Directory. Criador é adicionado como o primeiro proprietário e o objeto criado conta contra a quota de 250 objetos criados do criador. |
| microsoft.aad.directory/oAuth2PermissionGrants/createAsOwner | Criar oAuth2PermissionGrants no Azure Active Directory. Criador é adicionado como o primeiro proprietário e o objeto criado conta contra a quota de 250 objetos criados do criador. |
| microsoft.aad.directory/servicePrincipals/createAsOwner | Criar servicePrincipals no Azure Active Directory. Criador é adicionado como o primeiro proprietário e o objeto criado conta contra a quota de 250 objetos criados do criador. |

### <a name="billing-administrator"></a>Administrador de Faturação
Pode executar tarefas de faturação comuns, como atualizar as informações de pagamento.

  > [!NOTE]
  > Esta função tem permissões adicionais fora do Azure Active Directory. Para obter mais informações, consulte a descrição da função acima.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| Microsoft.aad.Directory/Organization/Basic/Update | Atualizar as propriedades básicas na organização no Azure Active Directory. |
| microsoft.aad.directory/organization/trustedCAsForPasswordlessAuth/update | Atualizar a propriedade organization.trustedCAsForPasswordlessAuth no Azure Active Directory. |
| microsoft.azure.accessService/allEntities/allTasks | Gerir todos os aspetos do serviço de Acesso do Azure. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Ler e configurar o Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Criar e gerir pedidos de suporte do Azure. |
| microsoft.commerce.billing/allEntities/allTasks | Gerir todos os aspetos da faturação do Office 365. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Leia e configure o Estado de Funcionamento do Serviço Office 365. |
| Microsoft.office365.supportTickets/allEntities/allTasks | Crie e gira pedidos de suporte do Office 365. |

### <a name="desktop-analytics-administrator"></a>Administrador da Análise de Computadores
Pode aceder e gerir os serviços incluindo o Intune e de ferramentas de gerenciamento de Desktops.

  > [!NOTE]
  > Esta função tem permissões adicionais fora do Azure Active Directory. Para obter mais informações, consulte a descrição da função acima.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | Gerir todos os aspetos do serviço de Acesso do Azure. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Ler e configurar o Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Criar e gerir pedidos de suporte do Azure. |
| Microsoft.office365.desktopAnalytics/allEntities/allTasks | Gerir todos os aspetos da análise de ambiente de trabalho. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Leia e configure o Estado de Funcionamento do Serviço Office 365. |
| Microsoft.office365.supportTickets/allEntities/allTasks | Crie e gira pedidos de suporte do Office 365. |

### <a name="cloud-application-administrator"></a>Administrador da Aplicação Cloud
Pode criar e gerir todos os aspetos de registos de aplicações e aplicações empresariais, exceto o Proxy de Aplicações.

| **Ações** | **Descrição** |
| --- | --- |
| Microsoft.aad.Directory/Applications/Audience/Update | Atualizar a propriedade applications.audience no Azure Active Directory. |
| Microsoft.aad.Directory/Applications/Authentication/Update | Atualizar a propriedade applications.authentication no Azure Active Directory. |
| Microsoft.aad.Directory/Applications/Basic/Update | Atualize as propriedades básicas das aplicações no Azure Active Directory. |
| Microsoft.aad.Directory/Applications/Create | Criar aplicações no Azure Active Directory. |
| Microsoft.aad.Directory/Applications/credentials/Update | Atualizar a propriedade applications.credentials no Azure Active Directory. |
| Microsoft.aad.Directory/Applications/DELETE | Eliminar aplicações no Azure Active Directory. |
| Microsoft.aad.Directory/Applications/Owners/Update | Atualizar a propriedade applications.owners no Azure Active Directory. |
| Microsoft.aad.Directory/Applications/Permissions/Update | Atualizar a propriedade applications.permissions no Azure Active Directory. |
| Microsoft.aad.Directory/Applications/Policies/Update | Atualizar a propriedade applications.policies no Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/create | Criar appRoleAssignments no Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/update | Atualizar appRoleAssignments no Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/delete | Eliminar appRoleAssignments no Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/create | Criar políticas no Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/basic/read | Ler a propriedade policies.applicationConfiguration no Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/basic/update | Atualizar a propriedade policies.applicationConfiguration no Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/delete | Eliminar políticas no Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/owners/read | Ler a propriedade policies.applicationConfiguration no Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/owners/update | Atualizar a propriedade policies.applicationConfiguration no Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/policyAppliedTo/read | Ler a propriedade policies.applicationConfiguration no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/update | Atualizar a propriedade servicePrincipals.appRoleAssignedTo no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/update | Atualizar a propriedade servicePrincipals.appRoleAssignments no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/basic/update | Atualizar propriedades básicas nos servicePrincipals no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/create | Criar servicePrincipals no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/delete | Eliminar servicePrincipals no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/owners/update | Atualizar a propriedade servicePrincipals.owners no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/policies/update | Atualizar a propriedade servicePrincipals.policies no Azure Active Directory. |
| microsoft.aad.reports/applicationAuditLogs/read | Leia applicationAuditLogs nos relatórios do Azure AD. |
| microsoft.aad.reports/applicationSignInReports/read | Leia applicationSignInReports nos relatórios do Azure AD. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Ler e configurar o Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Criar e gerir pedidos de suporte do Azure. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Leia e configure o Estado de Funcionamento do Serviço Office 365. |
| Microsoft.office365.supportTickets/allEntities/allTasks | Crie e gira pedidos de suporte do Office 365. |

### <a name="cloud-device-administrator"></a>Administrador de Dispositivo na Cloud
Acesso total para gerir dispositivos no Azure AD.

| **Ações** | **Descrição** |
| --- | --- |
| Microsoft.aad.Directory/Devices/DELETE | Eliminar dispositivos no Azure Active Directory. |
| Microsoft.aad.Directory/Devices/disable | Desativar dispositivos no Azure Active Directory. |
| Microsoft.aad.Directory/Devices/Enable | Permitir que os dispositivos no Azure Active Directory. |
| microsoft.aad.reports/applicationAuditLogs/read | Leia applicationAuditLogs nos relatórios do Azure AD. |
| microsoft.aad.reports/applicationSignInReports/read | Leia applicationSignInReports nos relatórios do Azure AD. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Ler e configurar o Azure Service Health. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Leia e configure o Estado de Funcionamento do Serviço Office 365. |

### <a name="company-administrator"></a>Administrador de Empresa
Pode gerir todos os aspetos dos serviços Microsoft e Azure AD que utilizam identidades do Azure AD.

  > [!NOTE]
  > Esta função tem permissões adicionais fora do Azure Active Directory. Para obter mais informações, consulte a descrição da função acima.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.aad.directory/administrativeUnits/allProperties/allTasks | Criar e eliminar administrativeUnits e ler e atualizar todas as propriedades no Azure Active Directory. |
| microsoft.aad.directory/applications/allProperties/allTasks | Criar e eliminar aplicações e ler e atualizar todas as propriedades no Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/allProperties/allTasks | Criar e eliminar appRoleAssignments e ler e atualizar todas as propriedades no Azure Active Directory. |
| microsoft.aad.directory/contacts/allProperties/allTasks | Criar e eliminar contactos e ler e atualizar todas as propriedades no Azure Active Directory. |
| microsoft.aad.directory/contracts/allProperties/allTasks | Criar e eliminar contratos e ler e atualizar todas as propriedades no Azure Active Directory. |
| microsoft.aad.directory/devices/allProperties/allTasks | Criar e eliminar dispositivos e ler e atualizar todas as propriedades no Azure Active Directory. |
| microsoft.aad.directory/directoryRoles/allProperties/allTasks | Criar e eliminar directoryRoles e ler e atualizar todas as propriedades no Azure Active Directory. |
| microsoft.aad.directory/directoryRoleTemplates/allProperties/allTasks | Criar e eliminar directoryRoleTemplates e ler e atualizar todas as propriedades no Azure Active Directory. |
| microsoft.aad.directory/domains/allProperties/allTasks | Criar e eliminar domínios e ler e atualizar todas as propriedades no Azure Active Directory. |
| microsoft.aad.directory/groups/allProperties/allTasks | Criar e eliminar grupos e ler e atualizar todas as propriedades no Azure Active Directory. |
| microsoft.aad.directory/groupSettings/allProperties/allTasks | Criar e eliminar groupSettings e ler e atualizar todas as propriedades no Azure Active Directory. |
| microsoft.aad.directory/groupSettingTemplates/allProperties/allTasks | Criar e eliminar groupSettingTemplates e ler e atualizar todas as propriedades no Azure Active Directory. |
| microsoft.aad.directory/loginTenantBranding/allProperties/allTasks | Criar e eliminar loginTenantBranding e ler e atualizar todas as propriedades no Azure Active Directory. |
| microsoft.aad.directory/oAuth2PermissionGrants/allProperties/allTasks | Criar e eliminar oAuth2PermissionGrants e ler e atualizar todas as propriedades no Azure Active Directory. |
| microsoft.aad.directory/organization/allProperties/allTasks | Criar e eliminar a organização e ler e atualizar todas as propriedades no Azure Active Directory. |
| microsoft.aad.directory/policies/allProperties/allTasks | Criar e eliminar políticas e ler e atualizar todas as propriedades no Azure Active Directory. |
| microsoft.aad.directory/roleAssignments/allProperties/allTasks | Criar e eliminar roleAssignments e ler e atualizar todas as propriedades no Azure Active Directory. |
| microsoft.aad.directory/roleDefinitions/allProperties/allTasks | Criar e eliminar roleDefinitions e ler e atualizar todas as propriedades no Azure Active Directory. |
| microsoft.aad.directory/scopedRoleMemberships/allProperties/allTasks | Criar e eliminar scopedRoleMemberships e ler e atualizar todas as propriedades no Azure Active Directory. |
| microsoft.aad.directory/serviceAction/activateService | Pode efetuar a ação de serviço Activateservice no Azure Active Directory |
| microsoft.aad.directory/serviceAction/disableDirectoryFeature | Pode efetuar a ação de serviço Disabledirectoryfeature no Azure Active Directory |
| microsoft.aad.directory/serviceAction/enableDirectoryFeature | Pode efetuar a ação de serviço Enabledirectoryfeature no Azure Active Directory |
| microsoft.aad.directory/serviceAction/getAvailableExtentionProperties | Pode efetuar a ação de serviço Getavailableextentionproperties no Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/allProperties/allTasks | Criar e eliminar servicePrincipals e ler e atualizar todas as propriedades no Azure Active Directory. |
| microsoft.aad.directory/subscribedSkus/allProperties/allTasks | Criar e eliminar subscribedSkus e ler e atualizar todas as propriedades no Azure Active Directory. |
| microsoft.aad.directory/users/allProperties/allTasks | Criar e eliminar utilizadores e ler e atualizar todas as propriedades no Azure Active Directory. |
| microsoft.aad.directorySync/allEntities/allTasks | Execute todas as ações no Azure AD Connect. |
| microsoft.aad.identityProtection/allEntities/allTasks | Criar e eliminar todos os recursos e ler e atualizar propriedades padrão em microsoft.aad.identityProtection. |
| microsoft.aad.privilegedIdentityManagement/allEntities/read | Ler todos os recursos em microsoft.aad.privilegedIdentityManagement. |
| microsoft.aad.reports/applicationAuditLogs/read | Leia applicationAuditLogs nos relatórios do Azure AD. |
| microsoft.aad.reports/applicationSignInReports/read | Leia applicationSignInReports nos relatórios do Azure AD. |
| microsoft.azure.accessService/allEntities/allTasks | Gerir todos os aspetos do serviço de Acesso do Azure. |
| microsoft.azure.informationProtection/allEntities/allTasks | Gerir todos os aspetos do Azure Information Protection. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Ler e configurar o Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Criar e gerir pedidos de suporte do Azure. |
| microsoft.commerce.billing/allEntities/allTasks | Gerir todos os aspetos da faturação do Office 365. |
| microsoft.intune/allEntities/allTasks | Gerir todos os aspetos do Intune. |
| Microsoft.office365.complianceManager/allEntities/allTasks | Gerir todos os aspetos do Gestor de Conformidade do Office 365 |
| Microsoft.office365.Exchange/allEntities/allTasks | Gerir todos os aspetos do Exchange Online. |
| Microsoft.office365.lockbox/allEntities/allTasks | Gerir todos os aspetos do Sistema de Proteção de Dados do Cliente do Office 365 |
| Microsoft.office365.messageCenter/messages/Read | Ler mensagens na microsoft.office365.messageCenter. |
| Microsoft.office365.messageCenter/securityMessages/Read | Leia securityMessages no microsoft.office365.messageCenter. |
| microsoft.powerApps.powerBI/allEntities/allTasks | Gerir todos os aspetos do Power BI. |
| Microsoft.office365.protectionCenter/allEntities/allTasks | Gerir todos os aspetos do Centro de Proteção do Office 365. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Leia e configure o Estado de Funcionamento do Serviço Office 365. |
| Microsoft.office365.SharePoint/allEntities/allTasks | Criar e eliminar todos os recursos e ler e atualizar propriedades padrão em microsoft.office365.sharepoint. |
| Microsoft.office365.skypeForBusiness/allEntities/allTasks | Gerir todos os aspetos do Skype para Empresas Online. |
| Microsoft.office365.supportTickets/allEntities/allTasks | Crie e gira pedidos de suporte do Office 365. |
| microsoft.powerApps.dynamics365/allEntities/allTasks | Gerir todos os aspetos do Dynamics 365. |

### <a name="compliance-administrator"></a>Administrador de Conformidade
Pode ler e gerir a configuração de conformidade e os relatórios no Azure AD e no Office 365.

  > [!NOTE]
  > Esta função tem permissões adicionais fora do Azure Active Directory. Para obter mais informações, consulte a descrição da função acima.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | Gerir todos os aspetos do serviço de Acesso do Azure. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Ler e configurar o Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Criar e gerir pedidos de suporte do Azure. |
| Microsoft.office365.complianceManager/allEntities/allTasks | Gerir todos os aspetos do Gestor de Conformidade do Office 365 |
| Microsoft.office365.Exchange/allEntities/allTasks | Gerir todos os aspetos do Exchange Online. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Leia e configure o Estado de Funcionamento do Serviço Office 365. |
| Microsoft.office365.SharePoint/allEntities/allTasks | Criar e eliminar todos os recursos e ler e atualizar propriedades padrão em microsoft.office365.sharepoint. |
| Microsoft.office365.skypeForBusiness/allEntities/allTasks | Gerir todos os aspetos do Skype para Empresas Online. |
| Microsoft.office365.supportTickets/allEntities/allTasks | Crie e gira pedidos de suporte do Office 365. |

### <a name="conditional-access-administrator"></a>Administrador de Acesso Condicional
Pode gerir capacidades de acesso condicional.

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.aad.directory/policies/conditionalAccess/basic/read | Ler a propriedade policies.conditionalAccess no Azure Active Directory. |
| microsoft.aad.directory/policies/conditionalAccess/basic/update | Atualizar a propriedade policies.conditionalAccess no Azure Active Directory. |
| microsoft.aad.directory/policies/conditionalAccess/create | Criar políticas no Azure Active Directory. |
| microsoft.aad.directory/policies/conditionalAccess/delete | Eliminar políticas no Azure Active Directory. |
| microsoft.aad.directory/policies/conditionalAccess/owners/read | Ler a propriedade policies.conditionalAccess no Azure Active Directory. |
| microsoft.aad.directory/policies/conditionalAccess/owners/update | Atualizar a propriedade policies.conditionalAccess no Azure Active Directory. |
| microsoft.aad.directory/policies/conditionalAccess/policiesAppliedTo/read | Ler a propriedade policies.conditionalAccess no Azure Active Directory. |

### <a name="crm-service-administrator"></a>Administrador de Serviço CRM
Pode gerir todos os aspetos do produto Dynamics 365.

  > [!NOTE]
  > Esta função tem permissões adicionais fora do Azure Active Directory. Para obter mais informações, consulte a descrição da função acima.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | Gerir todos os aspetos do serviço de Acesso do Azure. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Ler e configurar o Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Criar e gerir pedidos de suporte do Azure. |
| microsoft.powerApps.dynamics365/allEntities/allTasks | Gerir todos os aspetos do Dynamics 365. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Leia e configure o Estado de Funcionamento do Serviço Office 365. |
| Microsoft.office365.supportTickets/allEntities/allTasks | Crie e gira pedidos de suporte do Office 365. |

### <a name="customer-lockbox-access-approver"></a>Aprovador de Acesso ao Cofre de Cliente
Pode aprovar pedidos de suporte da Microsoft para aceder aos dados organizacionais do cliente.

  > [!NOTE]
  > Esta função tem permissões adicionais fora do Azure Active Directory. Para obter mais informações, consulte a descrição da função acima.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | Gerir todos os aspetos do serviço de Acesso do Azure. |
| Microsoft.office365.lockbox/allEntities/allTasks | Gerir todos os aspetos do Sistema de Proteção de Dados do Cliente do Office 365 |

### <a name="device-administrators"></a>Administradores de Dispositivo
Os membros desta função são adicionados ao grupo Administradores local em dispositivos associados ao AD Azure.

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.aad.directory/groupSettings/basic/read | Ler as propriedades básicas em groupSettings no Azure Active Directory. |
| microsoft.aad.directory/groupSettingTemplates/basic/read | Ler as propriedades básicas de groupSettingTemplates no Azure Active Directory. |

### <a name="directory-readers"></a>Leitores de Diretório
Pode ler as informações de diretório básicas. Para conceder acesso a aplicações, não se destina aos utilizadores.

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.aad.directory/administrativeUnits/basic/read | Ler as propriedades básicas de administrativeUnits no Azure Active Directory. |
| microsoft.aad.directory/administrativeUnits/members/read | Ler a propriedade AdministrativeUnits.Members no Azure Active Directory. |
| Microsoft.aad.Directory/Applications/Audience/Read | Ler a propriedade applications.audience no Azure Active Directory. |
| Microsoft.aad.Directory/Applications/Authentication/Read | Ler a propriedade Groups.Members no Azure Active Directory. |
| Microsoft.aad.Directory/Applications/Basic/Read | Ler as propriedades básicas em aplicações no Azure Active Directory. |
| Microsoft.aad.Directory/Applications/credentials/Read | Ler a propriedade applications.credentials no Azure Active Directory. |
| Microsoft.aad.Directory/Applications/Owners/Read | Ler a propriedade applications.owners no Azure Active Directory. |
| Microsoft.aad.Directory/Applications/Permissions/Read | Ler a propriedade applications.permissions no Azure Active Directory. |
| Microsoft.aad.Directory/Applications/Policies/Read | Ler a propriedade applications.policies no Azure Active Directory. |
| Microsoft.aad.Directory/Contacts/Basic/Read | Ler as propriedades básicas em contactos no Azure Active Directory. |
| microsoft.aad.directory/contacts/memberOf/read | Ler a propriedade contacts.memberOf no Azure Active Directory. |
| Microsoft.aad.Directory/Contracts/Basic/Read | Ler as propriedades básicas em contratos no Azure Active Directory. |
| Microsoft.aad.Directory/Devices/Basic/Read | Ler as propriedades básicas em dispositivos no Azure Active Directory. |
| microsoft.aad.directory/devices/memberOf/read | Ler a propriedade devices.memberOf no Azure Active Directory. |
| microsoft.aad.directory/devices/registeredOwners/read | Ler a propriedade devices.registeredOwners no Azure Active Directory. |
| microsoft.aad.directory/devices/registeredUsers/read | Ler a propriedade devices.registeredUsers no Azure Active Directory. |
| microsoft.aad.directory/directoryRoles/basic/read | Ler as propriedades básicas em directoryRoles no Azure Active Directory. |
| microsoft.aad.directory/directoryRoles/eligibleMembers/read | Ler a propriedade directoryRoles.eligibleMembers no Azure Active Directory. |
| microsoft.aad.directory/directoryRoles/members/read | Ler a propriedade directoryRoles.members no Azure Active Directory. |
| Microsoft.aad.Directory/Domains/Basic/Read | Ler as propriedades básicas em domínios no Azure Active Directory. |
| microsoft.aad.directory/groups/appRoleAssignments/read | Ler a propriedade groups.appRoleAssignments no Azure Active Directory. |
| Microsoft.aad.Directory/Groups/Basic/Read | Ler as propriedades básicas em grupos no Azure Active Directory. |
| microsoft.aad.directory/groups/memberOf/read | Ler a propriedade groups.memberOf no Azure Active Directory. |
| Microsoft.aad.Directory/Groups/members/Read | Ler a propriedade groups.members no Azure Active Directory. |
| Microsoft.aad.Directory/Groups/Owners/Read | Ler a propriedade groups.owners no Azure Active Directory. |
| Microsoft.aad.Directory/Groups/Settings/Read | Ler a propriedade groups.settings no Azure Active Directory. |
| microsoft.aad.directory/groupSettings/basic/read | Ler as propriedades básicas em groupSettings no Azure Active Directory. |
| microsoft.aad.directory/groupSettingTemplates/basic/read | Ler as propriedades básicas de groupSettingTemplates no Azure Active Directory. |
| microsoft.aad.directory/oAuth2PermissionGrants/basic/read | Ler as propriedades básicas em oAuth2PermissionGrants no Azure Active Directory. |
| Microsoft.aad.Directory/Organization/Basic/Read | Ler as propriedades básicas na organização no Azure Active Directory. |
| microsoft.aad.directory/organization/trustedCAsForPasswordlessAuth/read | Ler a propriedade organization.trustedCAsForPasswordlessAuth no Azure Active Directory. |
| microsoft.aad.directory/roleAssignments/basic/read | Ler as propriedades básicas no RoleAssignments fosse no Azure Active Directory. |
| microsoft.aad.directory/roleDefinitions/basic/read | Ler as propriedades básicas no roleDefinitions no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/read | Leia a propriedade servicePrincipals.appRoleAssignedTo no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/read | Leia a propriedade servicePrincipals.appRoleAssignments no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/basic/read | Ler as propriedades básicas em servicePrincipals no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/memberOf/read | Ler a propriedade servicePrincipals.memberOf no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/oAuth2PermissionGrants/basic/read | Ler a propriedade servicePrincipals.oAuth2PermissionGrants no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/ownedObjects/read | Ler a propriedade servicePrincipals.ownedObjects no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/owners/read | Ler a propriedade servicePrincipals.owners no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/policies/read | Ler a propriedade servicePrincipals.policies no Azure Active Directory. |
| microsoft.aad.directory/subscribedSkus/basic/read | Ler as propriedades básicas em subscribedSkus no Azure Active Directory. |
| microsoft.aad.directory/users/appRoleAssignments/read | Ler a propriedade users.appRoleAssignments no Azure Active Directory. |
| Microsoft.aad.Directory/Users/Basic/Read | Ler as propriedades básicas em utilizadores no Azure Active Directory. |
| microsoft.aad.directory/users/directReports/read | Ler a propriedade users.directReports no Azure Active Directory. |
| microsoft.aad.directory/users/invitedBy/read | Ler a propriedade users.invitedBy no Azure Active Directory. |
| microsoft.aad.directory/users/invitedUsers/read | Ler a propriedade users.invitedUsers no Azure Active Directory. |
| Microsoft.aad.Directory/Users/Manager/Read | Ler a propriedade users.manager no Azure Active Directory. |
| microsoft.aad.directory/users/memberOf/read | Ler a propriedade users.memberOf no Azure Active Directory. |
| microsoft.aad.directory/users/oAuth2PermissionGrants/basic/read | Ler a propriedade users.oAuth2PermissionGrants no Azure Active Directory. |
| microsoft.aad.directory/users/ownedDevices/read | Ler a propriedade users.ownedDevices no Azure Active Directory. |
| microsoft.aad.directory/users/ownedObjects/read | Ler a propriedade users.ownedObjects no Azure Active Directory. |
| microsoft.aad.directory/users/registeredDevices/read | Ler a propriedade users.registeredDevices no Azure Active Directory. |

### <a name="directory-synchronization-accounts"></a>Contas de sincronização de diretório
Só é utilizada pelo serviço do Azure AD Connect.

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.aad.directory/organization/dirSync/update | Atualizar a propriedade organization.dirSync no Azure Active Directory. |
| Microsoft.aad.Directory/Policies/Create | Criar políticas no Azure Active Directory. |
| Microsoft.aad.Directory/Policies/DELETE | Eliminar políticas no Azure Active Directory. |
| Microsoft.aad.Directory/Policies/Basic/Read | Ler as propriedades básicas no Azure Active Directory. |
| Microsoft.aad.Directory/Policies/Basic/Update | Atualizar propriedades básicas em políticas no Azure Active Directory. |
| Microsoft.aad.Directory/Policies/Owners/Read | Ler a propriedade policies.owners no Azure Active Directory. |
| Microsoft.aad.Directory/Policies/Owners/Update | Atualizar a propriedade policies.owners no Azure Active Directory. |
| microsoft.aad.directory/policies/policiesAppliedTo/read | Ler a propriedade policies.policiesAppliedTo no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/read | Leia a propriedade servicePrincipals.appRoleAssignedTo no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/update | Atualizar a propriedade servicePrincipals.appRoleAssignedTo no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/read | Leia a propriedade servicePrincipals.appRoleAssignments no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/update | Atualizar a propriedade servicePrincipals.appRoleAssignments no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/basic/read | Ler as propriedades básicas em servicePrincipals no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/basic/update | Atualizar propriedades básicas nos servicePrincipals no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/create | Criar servicePrincipals no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/memberOf/read | Ler a propriedade servicePrincipals.memberOf no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/oAuth2PermissionGrants/basic/read | Ler a propriedade servicePrincipals.oAuth2PermissionGrants no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/owners/read | Ler a propriedade servicePrincipals.owners no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/owners/update | Atualizar a propriedade servicePrincipals.owners no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/ownedObjects/read | Ler a propriedade servicePrincipals.ownedObjects no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/policies/read | Ler a propriedade servicePrincipals.policies no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/policies/update | Atualizar a propriedade servicePrincipals.policies no Azure Active Directory. |
| microsoft.aad.directorySync/allEntities/allTasks | Execute todas as ações no Azure AD Connect. |

### <a name="directory-writers"></a>Gravadores de Diretórios
Pode ler e gravar informações de diretório básicas. Para conceder acesso a aplicações, não se destina aos utilizadores.

| **Ações** | **Descrição** |
| --- | --- |
| Microsoft.aad.Directory/Groups/Create | Criar grupos no Azure Active Directory. |
| microsoft.aad.directory/groups/createAsOwner | Criar grupos no Azure Active Directory. Criador é adicionado como o primeiro proprietário e o objeto criado conta contra a quota de 250 objetos criados do criador. |
| microsoft.aad.directory/groups/appRoleAssignments/update | Atualizar a propriedade groups.appRoleAssignments no Azure Active Directory. |
| Microsoft.aad.Directory/Groups/Basic/Update | Atualizar as propriedades básicas em grupos no Azure Active Directory. |
| Microsoft.aad.Directory/Groups/members/Update | Atualizar a propriedade groups.members no Azure Active Directory. |
| Microsoft.aad.Directory/Groups/Owners/Update | Atualize a propriedade groups.owners no Azure Active Directory. |
| Microsoft.aad.Directory/Groups/Settings/Update | Atualizar a propriedade groups.settings no Azure Active Directory. |
| microsoft.aad.directory/groupSettings/basic/update | Atualizar as propriedades básicas em groupSettings no Azure Active Directory. |
| microsoft.aad.directory/groupSettings/create | Criar groupSettings no Azure Active Directory. |
| microsoft.aad.directory/groupSettings/delete | Elimine groupSettings no Azure Active Directory. |
| microsoft.aad.directory/users/appRoleAssignments/update | Atualizar a propriedade users.appRoleAssignments no Azure Active Directory. |
| microsoft.aad.directory/users/assignLicense | Gerir licenças em utilizadores no Azure Active Directory. |
| Microsoft.aad.Directory/Users/Basic/Update | Atualizar propriedades básicas em utilizadores no Azure Active Directory. |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Invalide todos os tokens de atualização de utilizador no Azure Active Directory. |
| Microsoft.aad.Directory/Users/Manager/Update | Atualizar a propriedade users.manager no Azure Active Directory. |
| microsoft.aad.directory/users/userPrincipalName/update | Atualizar a propriedade users.userPrincipalName no Azure Active Directory. |

### <a name="exchange-service-administrator"></a>Administrador de Serviço Exchange
Pode gerir todos os aspetos do produto Exchange.

  > [!NOTE]
  > Esta função tem permissões adicionais fora do Azure Active Directory. Para obter mais informações, consulte a descrição da função acima.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | Gerir todos os aspetos do serviço de Acesso do Azure. |
| microsoft.aad.directory/groups/unified/appRoleAssignments/update | Atualize a propriedade groups.unified no Azure Active Directory. |
| Microsoft.aad.Directory/Groups/Unified/Basic/Update | Atualize as propriedades básicas dos grupos do Office 365. |
| Microsoft.aad.Directory/Groups/Unified/Create | Crie grupos do Office 365. |
| Microsoft.aad.Directory/Groups/Unified/DELETE | Elimine grupos do Office 365. |
| Microsoft.aad.Directory/Groups/Unified/members/Update | Atualize associação de grupos do Office 365. |
| Microsoft.aad.Directory/Groups/Unified/Owners/Update | Atualize a propriedade de grupos do Office 365. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Ler e configurar o Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Criar e gerir pedidos de suporte do Azure. |
| Microsoft.office365.Exchange/allEntities/allTasks | Gerir todos os aspetos do Exchange Online. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Leia e configure o Estado de Funcionamento do Serviço Office 365. |
| Microsoft.office365.supportTickets/allEntities/allTasks | Crie e gira pedidos de suporte do Office 365. |

### <a name="guest-inviter"></a>Utilizador Que Convida Convidados
Pode convidar utilizadores convidados independentemente da definição "Os membros podem convidar convidados".

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.aad.directory/users/appRoleAssignments/read | Ler a propriedade users.appRoleAssignments no Azure Active Directory. |
| Microsoft.aad.Directory/Users/Basic/Read | Ler as propriedades básicas em utilizadores no Azure Active Directory. |
| microsoft.aad.directory/users/directReports/read | Ler a propriedade users.directReports no Azure Active Directory. |
| microsoft.aad.directory/users/invitedBy/read | Ler a propriedade users.invitedBy no Azure Active Directory. |
| microsoft.aad.directory/users/inviteGuest | Convide utilizadores no Azure Active Directory. |
| microsoft.aad.directory/users/invitedUsers/read | Ler a propriedade users.invitedUsers no Azure Active Directory. |
| Microsoft.aad.Directory/Users/Manager/Read | Ler a propriedade users.manager no Azure Active Directory. |
| microsoft.aad.directory/users/memberOf/read | Ler a propriedade users.memberOf no Azure Active Directory. |
| microsoft.aad.directory/users/oAuth2PermissionGrants/basic/read | Ler a propriedade users.oAuth2PermissionGrants no Azure Active Directory. |
| microsoft.aad.directory/users/ownedDevices/read | Ler a propriedade users.ownedDevices no Azure Active Directory. |
| microsoft.aad.directory/users/ownedObjects/read | Ler a propriedade users.ownedObjects no Azure Active Directory. |
| microsoft.aad.directory/users/registeredDevices/read | Ler a propriedade users.registeredDevices no Azure Active Directory. |

### <a name="helpdesk-administrator"></a>Administrador de Suporte Técnico
Pode repor palavras-passe para não administradores e Administradores de Suporte Técnico.

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Invalide todos os tokens de atualização de utilizador no Azure Active Directory. |
| Microsoft.aad.Directory/Users/password/Update | Atualize as palavras-passe para todos os utilizadores no Azure Active Directory. Consulte a documentação online para obter mais detalhes. |
| microsoft.azure.accessService/allEntities/allTasks | Gerir todos os aspetos do serviço de Acesso do Azure. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Ler e configurar o Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Criar e gerir pedidos de suporte do Azure. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Leia e configure o Estado de Funcionamento do Serviço Office 365. |
| Microsoft.office365.supportTickets/allEntities/allTasks | Crie e gira pedidos de suporte do Office 365. |

### <a name="information-protection-administrator"></a>Administrador do Information Protection
Pode gerir todos os aspetos do produto Azure Information Protection.

  > [!NOTE]
  > Esta função tem permissões adicionais fora do Azure Active Directory. Para obter mais informações, consulte a descrição da função acima.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.azure.informationProtection/allEntities/allTasks | Gerir todos os aspetos do Azure Information Protection. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Ler e configurar o Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Criar e gerir pedidos de suporte do Azure. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Leia e configure o Estado de Funcionamento do Serviço Office 365. |
| Microsoft.office365.supportTickets/allEntities/allTasks | Crie e gira pedidos de suporte do Office 365. |

### <a name="intune-service-administrator"></a>Administrador de Serviços do Intune
Pode gerir todos os aspetos do produto Intune.

  > [!NOTE]
  > Esta função tem permissões adicionais fora do Azure Active Directory. Para obter mais informações, consulte a descrição da função acima.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| Microsoft.aad.Directory/Contacts/Basic/Update | Atualize as propriedades básicas de contactos no Azure Active Directory. |
| Microsoft.aad.Directory/Contacts/Create | Criar contactos no Azure Active Directory. |
| Microsoft.aad.Directory/Contacts/DELETE | Eliminar contactos no Azure Active Directory. |
| Microsoft.aad.Directory/Devices/Basic/Update | Atualizar as propriedades básicas em dispositivos no Azure Active Directory. |
| Microsoft.aad.Directory/Devices/Create | Criar dispositivos no Azure Active Directory. |
| Microsoft.aad.Directory/Devices/DELETE | Eliminar dispositivos no Azure Active Directory. |
| microsoft.aad.directory/devices/registeredOwners/update | Atualizar a propriedade devices.registeredOwners no Azure Active Directory. |
| microsoft.aad.directory/devices/registeredUsers/update | Atualizar a propriedade devices.registeredUsers no Azure Active Directory. |
| microsoft.aad.directory/groups/appRoleAssignments/update | Atualizar a propriedade groups.appRoleAssignments no Azure Active Directory. |
| Microsoft.aad.Directory/Groups/Basic/Update | Atualizar as propriedades básicas em grupos no Azure Active Directory. |
| Microsoft.aad.Directory/Groups/Create | Criar grupos no Azure Active Directory. |
| microsoft.aad.directory/groups/createAsOwner | Criar grupos no Azure Active Directory. Criador é adicionado como o primeiro proprietário e o objeto criado conta contra a quota de 250 objetos criados do criador. |
| Microsoft.aad.Directory/Groups/DELETE | Eliminar grupos no Azure Active Directory. |
| microsoft.aad.directory/groups/hiddenMembers/read | Ler a propriedade groups.hiddenMembers no Azure Active Directory. |
| Microsoft.aad.Directory/Groups/members/Update | Atualizar a propriedade groups.members no Azure Active Directory. |
| Microsoft.aad.Directory/Groups/Owners/Update | Atualize a propriedade groups.owners no Azure Active Directory. |
| Microsoft.aad.Directory/Groups/Restore | Restaurar grupos no Azure Active Directory. |
| Microsoft.aad.Directory/Groups/Settings/Update | Atualizar a propriedade groups.settings no Azure Active Directory. |
| microsoft.aad.directory/users/appRoleAssignments/update | Atualizar a propriedade users.appRoleAssignments no Azure Active Directory. |
| Microsoft.aad.Directory/Users/Basic/Update | Atualizar propriedades básicas em utilizadores no Azure Active Directory. |
| Microsoft.aad.Directory/Users/Manager/Update | Atualizar a propriedade users.manager no Azure Active Directory. |
| microsoft.azure.supportTickets/allEntities/allTasks | Criar e gerir pedidos de suporte do Azure. |
| microsoft.intune/allEntities/allTasks | Gerir todos os aspetos do Intune. |
| Microsoft.office365.supportTickets/allEntities/allTasks | Crie e gira pedidos de suporte do Office 365. |

### <a name="license-administrator"></a>Administrador de Licenças
Pode gerir licenças de produtos em utilizadores e grupos.

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.aad.directory/users/assignLicense | Gerir licenças em utilizadores no Azure Active Directory. |
| microsoft.aad.directory/users/usageLocation/update | Atualizar a propriedade users.usageLocation no Azure Active Directory. |
| microsoft.azure.accessService/allEntities/allTasks | Gerir todos os aspetos do serviço de Acesso do Azure. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Ler e configurar o Azure Service Health. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Leia e configure o Estado de Funcionamento do Serviço Office 365. |

### <a name="lync-service-administrator"></a>Administrador do Serviço Lync
Pode gerir todos os aspetos do produto Skype para Empresas.

  > [!NOTE]
  > Esta função tem permissões adicionais fora do Azure Active Directory. Para obter mais informações, consulte a descrição da função acima.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | Gerir todos os aspetos do serviço de Acesso do Azure. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Ler e configurar o Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Criar e gerir pedidos de suporte do Azure. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Leia e configure o Estado de Funcionamento do Serviço Office 365. |
| Microsoft.office365.skypeForBusiness/allEntities/allTasks | Gerir todos os aspetos do Skype para Empresas Online. |
| Microsoft.office365.supportTickets/allEntities/allTasks | Crie e gira pedidos de suporte do Office 365. |

### <a name="message-center-reader"></a>Leitor do Centro de Mensagens
Pode ler as mensagens e atualizações para a sua organização apenas no Centro de Mensagem do Office 365. 

  > [!NOTE]
  > Esta função tem permissões adicionais fora do Azure Active Directory. Para obter mais informações, consulte a descrição da função acima.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | Gerir todos os aspetos do serviço de Acesso do Azure. |
| Microsoft.office365.messageCenter/messages/Read | Ler mensagens na microsoft.office365.messageCenter. |

### <a name="partner-tier1-support"></a>Parceiro de Suporte de Escalão 1
Não utilize – não se destina a utilização geral.

  > [!NOTE]
  > Esta função tem permissões adicionais fora do Azure Active Directory. Para obter mais informações, consulte a descrição da função acima.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| Microsoft.aad.Directory/Contacts/Basic/Update | Atualize as propriedades básicas de contactos no Azure Active Directory. |
| Microsoft.aad.Directory/Contacts/Create | Criar contactos no Azure Active Directory. |
| Microsoft.aad.Directory/Contacts/DELETE | Eliminar contactos no Azure Active Directory. |
| Microsoft.aad.Directory/Groups/Create | Criar grupos no Azure Active Directory. |
| microsoft.aad.directory/groups/createAsOwner | Criar grupos no Azure Active Directory. Criador é adicionado como o primeiro proprietário e o objeto criado conta contra a quota de 250 objetos criados do criador. |
| Microsoft.aad.Directory/Groups/members/Update | Atualizar a propriedade groups.members no Azure Active Directory. |
| Microsoft.aad.Directory/Groups/Owners/Update | Atualize a propriedade groups.owners no Azure Active Directory. |
| microsoft.aad.directory/users/appRoleAssignments/update | Atualizar a propriedade users.appRoleAssignments no Azure Active Directory. |
| microsoft.aad.directory/users/assignLicense | Gerir licenças em utilizadores no Azure Active Directory. |
| Microsoft.aad.Directory/Users/Basic/Update | Atualizar propriedades básicas em utilizadores no Azure Active Directory. |
| Microsoft.aad.Directory/Users/DELETE | Eliminar utilizadores no Azure Active Directory. |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Invalide todos os tokens de atualização de utilizador no Azure Active Directory. |
| Microsoft.aad.Directory/Users/Manager/Update | Atualizar a propriedade users.manager no Azure Active Directory. |
| Microsoft.aad.Directory/Users/password/Update | Atualize as palavras-passe para todos os utilizadores no Azure Active Directory. Consulte a documentação online para obter mais detalhes. |
| Microsoft.aad.Directory/Users/Restore | Restaure utilizadores eliminados no Azure Active Directory. |
| microsoft.aad.directory/users/userPrincipalName/update | Atualizar a propriedade users.userPrincipalName no Azure Active Directory. |
| microsoft.azure.accessService/allEntities/allTasks | Gerir todos os aspetos do serviço de Acesso do Azure. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Ler e configurar o Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Criar e gerir pedidos de suporte do Azure. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Leia e configure o Estado de Funcionamento do Serviço Office 365. |
| Microsoft.office365.supportTickets/allEntities/allTasks | Crie e gira pedidos de suporte do Office 365. |

### <a name="partner-tier2-support"></a>Parceiro de Suporte de Escalão 2
Não utilize – não se destina a utilização geral.

  > [!NOTE]
  > Esta função tem permissões adicionais fora do Azure Active Directory. Para obter mais informações, consulte a descrição da função acima.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| Microsoft.aad.Directory/Contacts/Basic/Update | Atualize as propriedades básicas de contactos no Azure Active Directory. |
| Microsoft.aad.Directory/Contacts/Create | Criar contactos no Azure Active Directory. |
| Microsoft.aad.Directory/Contacts/DELETE | Eliminar contactos no Azure Active Directory. |
| microsoft.aad.directory/domains/allTasks | Criar e eliminar domínios e ler e atualizar as propriedades padrão no Azure Active Directory. |
| Microsoft.aad.Directory/Groups/Create | Criar grupos no Azure Active Directory. |
| Microsoft.aad.Directory/Groups/DELETE | Eliminar grupos no Azure Active Directory. |
| Microsoft.aad.Directory/Groups/members/Update | Atualizar a propriedade groups.members no Azure Active Directory. |
| Microsoft.aad.Directory/Groups/Restore | Restaurar grupos no Azure Active Directory. |
| Microsoft.aad.Directory/Organization/Basic/Update | Atualizar as propriedades básicas na organização no Azure Active Directory. |
| microsoft.aad.directory/organization/trustedCAsForPasswordlessAuth/update | Atualizar a propriedade organization.trustedCAsForPasswordlessAuth no Azure Active Directory. |
| microsoft.aad.directory/users/appRoleAssignments/update | Atualizar a propriedade users.appRoleAssignments no Azure Active Directory. |
| microsoft.aad.directory/users/assignLicense | Gerir licenças em utilizadores no Azure Active Directory. |
| Microsoft.aad.Directory/Users/Basic/Update | Atualizar propriedades básicas em utilizadores no Azure Active Directory. |
| Microsoft.aad.Directory/Users/DELETE | Eliminar utilizadores no Azure Active Directory. |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Invalide todos os tokens de atualização de utilizador no Azure Active Directory. |
| Microsoft.aad.Directory/Users/Manager/Update | Atualizar a propriedade users.manager no Azure Active Directory. |
| Microsoft.aad.Directory/Users/password/Update | Atualize as palavras-passe para todos os utilizadores no Azure Active Directory. Consulte a documentação online para obter mais detalhes. |
| Microsoft.aad.Directory/Users/Restore | Restaure utilizadores eliminados no Azure Active Directory. |
| microsoft.aad.directory/users/userPrincipalName/update | Atualizar a propriedade users.userPrincipalName no Azure Active Directory. |
| microsoft.azure.accessService/allEntities/allTasks | Gerir todos os aspetos do serviço de Acesso do Azure. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Ler e configurar o Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Criar e gerir pedidos de suporte do Azure. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Leia e configure o Estado de Funcionamento do Serviço Office 365. |
| Microsoft.office365.supportTickets/allEntities/allTasks | Crie e gira pedidos de suporte do Office 365. |

### <a name="power-bi-service-administrator"></a>Administrador do Power BI
Pode gerir todos os aspetos do produto Power BI.

  > [!NOTE]
  > Esta função tem permissões adicionais fora do Azure Active Directory. Para obter mais informações, consulte a descrição da função acima.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | Gerir todos os aspetos do serviço de Acesso do Azure. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Ler e configurar o Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Criar e gerir pedidos de suporte do Azure. |
| microsoft.powerApps.powerBI/allEntities/allTasks | Gerir todos os aspetos do Power BI. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Leia e configure o Estado de Funcionamento do Serviço Office 365. |
| Microsoft.office365.supportTickets/allEntities/allTasks | Crie e gira pedidos de suporte do Office 365. |

### <a name="privileged-role-administrator"></a>Administrador com Função Privilegiada
Pode gerir atribuições de funções no Azure AD e todos os aspetos do Privileged Identity Management.

  > [!NOTE]
  > Esta função tem permissões adicionais fora do Azure Active Directory. Para obter mais informações, consulte a descrição da função acima.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.aad.directory/directoryRoles/update | Atualizar os directoryRoles no Azure Active Directory. |
| microsoft.aad.privilegedIdentityManagement/allEntities/allTasks | Criar e eliminar todos os recursos e ler e atualizar propriedades padrão em microsoft.aad.privilegedIdentityManagement. |

### <a name="reports-reader"></a>Leitor de Relatórios
Pode ler relatórios de auditoria e inícios de sessão.

  > [!NOTE]
  > Esta função tem permissões adicionais fora do Azure Active Directory. Para obter mais informações, consulte a descrição da função acima.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.aad.reports/applicationAuditLogs/read | Leia applicationAuditLogs nos relatórios do Azure AD. |
| microsoft.aad.reports/applicationSignInReports/read | Leia applicationSignInReports nos relatórios do Azure AD. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Ler e configurar o Azure Service Health. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Leia e configure o Estado de Funcionamento do Serviço Office 365. |
| Microsoft.office365.usageReports/allEntities/Read | Ler relatórios de utilização do Office 365. |

### <a name="security-administrator"></a>Administrador de Segurança
Pode ler as informações de segurança e de relatórios e gerir a configuração no Azure AD e o Office 365.

  > [!NOTE]
  > Esta função tem permissões adicionais fora do Azure Active Directory. Para obter mais informações, consulte a descrição da função acima.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| Microsoft.aad.Directory/Applications/Policies/Update | Atualizar a propriedade applications.policies no Azure Active Directory. |
| Microsoft.aad.Directory/Policies/Basic/Update | Atualizar propriedades básicas em políticas no Azure Active Directory. |
| Microsoft.aad.Directory/Policies/Create | Criar políticas no Azure Active Directory. |
| Microsoft.aad.Directory/Policies/DELETE | Eliminar políticas no Azure Active Directory. |
| Microsoft.aad.Directory/Policies/Owners/Update | Atualizar a propriedade policies.owners no Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/policies/update | Atualizar a propriedade servicePrincipals.policies no Azure Active Directory. |
| microsoft.aad.identityProtection/allEntities/read | Ler todos os recursos no microsoft.aad.identityProtection. |
| microsoft.aad.identityProtection/allEntities/update | Atualizar todos os recursos em microsoft.aad.identityProtection. |
| microsoft.aad.privilegedIdentityManagement/allEntities/read | Ler todos os recursos em microsoft.aad.privilegedIdentityManagement. |
| microsoft.aad.reports/applicationAuditLogs/read | Leia applicationAuditLogs nos relatórios do Azure AD. |
| microsoft.aad.reports/applicationSignInReports/read | Leia applicationSignInReports nos relatórios do Azure AD. |
| microsoft.azure.accessService/allEntities/allTasks | Gerir todos os aspetos do serviço de Acesso do Azure. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Ler e configurar o Azure Service Health. |
| Microsoft.office365.protectionCenter/allEntities/Read | Leia todos os aspetos do Centro de Proteção do Office 365. |
| Microsoft.office365.protectionCenter/allEntities/Update | Atualizar todos os recursos em microsoft.office365.protectionCenter. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Leia e configure o Estado de Funcionamento do Serviço Office 365. |

### <a name="security-reader"></a>Leitor de Segurança
Pode ler as informações de segurança e os relatórios no Azure AD e no Office 365.

  > [!NOTE]
  > Esta função tem permissões adicionais fora do Azure Active Directory. Para obter mais informações, consulte a descrição da função acima.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.aad.identityProtection/allEntities/read | Ler todos os recursos no microsoft.aad.identityProtection. |
| microsoft.aad.privilegedIdentityManagement/allEntities/read | Ler todos os recursos em microsoft.aad.privilegedIdentityManagement. |
| microsoft.aad.reports/applicationAuditLogs/read | Leia applicationAuditLogs nos relatórios do Azure AD. |
| microsoft.aad.reports/applicationSignInReports/read | Leia applicationSignInReports nos relatórios do Azure AD. |
| microsoft.azure.accessService/allEntities/allTasks | Gerir todos os aspetos do serviço de Acesso do Azure. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Ler e configurar o Azure Service Health. |
| Microsoft.office365.protectionCenter/allEntities/Read | Leia todos os aspetos do Centro de Proteção do Office 365. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Leia e configure o Estado de Funcionamento do Serviço Office 365. |

### <a name="service-support-administrator"></a>Administrador de Assistência Técnica
Consegue ler as informações do Service Health e gerir pedidos de suporte.

  > [!NOTE]
  > Esta função tem permissões adicionais fora do Azure Active Directory. Para obter mais informações, consulte a descrição da função acima.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | Gerir todos os aspetos do serviço de Acesso do Azure. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Ler e configurar o Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Criar e gerir pedidos de suporte do Azure. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Leia e configure o Estado de Funcionamento do Serviço Office 365. |
| Microsoft.office365.supportTickets/allEntities/allTasks | Crie e gira pedidos de suporte do Office 365. |

### <a name="sharepoint-service-administrator"></a>Administrador do Serviço SharePoint
Pode gerir todos os aspetos do serviço do SharePoint.

  > [!NOTE]
  > Esta função tem permissões adicionais fora do Azure Active Directory. Para obter mais informações, consulte a descrição da função acima.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | Gerir todos os aspetos do serviço de Acesso do Azure. |
| microsoft.aad.directory/groups/unified/appRoleAssignments/update | Atualize a propriedade groups.unified no Azure Active Directory. |
| Microsoft.aad.Directory/Groups/Unified/Basic/Update | Atualize as propriedades básicas dos grupos do Office 365. |
| Microsoft.aad.Directory/Groups/Unified/Create | Crie grupos do Office 365. |
| Microsoft.aad.Directory/Groups/Unified/DELETE | Elimine grupos do Office 365. |
| Microsoft.aad.Directory/Groups/Unified/members/Update | Atualize associação de grupos do Office 365. |
| Microsoft.aad.Directory/Groups/Unified/Owners/Update | Atualize a propriedade de grupos do Office 365. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Ler e configurar o Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Criar e gerir pedidos de suporte do Azure. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Leia e configure o Estado de Funcionamento do Serviço Office 365. |
| Microsoft.office365.SharePoint/allEntities/allTasks | Criar e eliminar todos os recursos e ler e atualizar propriedades padrão em microsoft.office365.sharepoint. |
| Microsoft.office365.supportTickets/allEntities/allTasks | Crie e gira pedidos de suporte do Office 365. |

### <a name="teams-communications-administrator"></a>Administrador de Comunicações do Teams
Pode gerir funcionalidades de chamadas e reuniões no serviço Microsoft Teams.

  > [!NOTE]
  > Esta função tem permissões adicionais fora do Azure Active Directory. Para obter mais informações, consulte a descrição da função acima.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| Microsoft.aad.Directory/Policies/Basic/Read | Ler as propriedades básicas no Azure Active Directory. |
| microsoft.azure.accessService/allEntities/allTasks | Gerir todos os aspetos do serviço de Acesso do Azure. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Ler e configurar o Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Criar e gerir pedidos de suporte do Azure. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Leia e configure o Estado de Funcionamento do Serviço Office 365. |
| Microsoft.office365.supportTickets/allEntities/allTasks | Crie e gira pedidos de suporte do Office 365. |
| Microsoft.office365.usageReports/allEntities/Read | Ler relatórios de utilização do Office 365. |

### <a name="teams-communications-support-engineer"></a>Engenheiro de Suporte de Comunicações do Teams
Pode resolver problemas de comunicação no Teams com ferramentas avançadas.

  > [!NOTE]
  > Esta função tem permissões adicionais fora do Azure Active Directory. Para obter mais informações, consulte a descrição da função acima.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| Microsoft.aad.Directory/Policies/Basic/Read | Ler as propriedades básicas no Azure Active Directory. |
| microsoft.azure.accessService/allEntities/allTasks | Gerir todos os aspetos do serviço de Acesso do Azure. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Ler e configurar o Azure Service Health. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Leia e configure o Estado de Funcionamento do Serviço Office 365. |

### <a name="teams-communications-support-specialist"></a>Especialista de Suporte de Comunicações do Teams
Pode resolver problemas de comunicação no Teams com ferramentas básicas.

  > [!NOTE]
  > Esta função tem permissões adicionais fora do Azure Active Directory. Para obter mais informações, consulte a descrição da função acima.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| Microsoft.aad.Directory/Policies/Basic/Read | Ler as propriedades básicas no Azure Active Directory. |
| microsoft.azure.accessService/allEntities/allTasks | Gerir todos os aspetos do serviço de Acesso do Azure. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Ler e configurar o Azure Service Health. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Leia e configure o Estado de Funcionamento do Serviço Office 365. |

### <a name="teams-service-administrator"></a>Administrador de Serviço do Teams
Pode gerir o serviço Microsoft Teams.

  > [!NOTE]
  > Esta função tem permissões adicionais fora do Azure Active Directory. Para obter mais informações, consulte a descrição da função acima.
  >
  >

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.aad.directory/groups/hiddenMembers/read | Ler a propriedade groups.hiddenMembers no Azure Active Directory. |
| microsoft.aad.directory/groups/unified/appRoleAssignments/update | Atualize a propriedade groups.unified no Azure Active Directory. |
| Microsoft.aad.Directory/Groups/Unified/Basic/Update | Atualize as propriedades básicas dos grupos do Office 365. |
| Microsoft.aad.Directory/Groups/Unified/Create | Crie grupos do Office 365. |
| Microsoft.aad.Directory/Groups/Unified/DELETE | Elimine grupos do Office 365. |
| Microsoft.aad.Directory/Groups/Unified/members/Update | Atualize associação de grupos do Office 365. |
| Microsoft.aad.Directory/Groups/Unified/Owners/Update | Atualize a propriedade de grupos do Office 365. |
| Microsoft.aad.Directory/Policies/Basic/Read | Ler as propriedades básicas no Azure Active Directory. |
| microsoft.azure.accessService/allEntities/allTasks | Gerir todos os aspetos do serviço de Acesso do Azure. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Ler e configurar o Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Criar e gerir pedidos de suporte do Azure. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Leia e configure o Estado de Funcionamento do Serviço Office 365. |
| Microsoft.office365.supportTickets/allEntities/allTasks | Crie e gira pedidos de suporte do Office 365. |
| Microsoft.office365.usageReports/allEntities/Read | Ler relatórios de utilização do Office 365. |

### <a name="user-account-administrator"></a>Administrador de Conta de Utilizador
Pode gerir todos os aspetos de utilizadores e grupos, incluindo repor palavras-passe para administradores limitados.

| **Ações** | **Descrição** |
| --- | --- |
| microsoft.aad.directory/appRoleAssignments/create | Criar appRoleAssignments no Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/delete | Eliminar appRoleAssignments no Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/update | Atualizar appRoleAssignments no Azure Active Directory. |
| Microsoft.aad.Directory/Contacts/Basic/Update | Atualize as propriedades básicas de contactos no Azure Active Directory. |
| Microsoft.aad.Directory/Contacts/Create | Criar contactos no Azure Active Directory. |
| Microsoft.aad.Directory/Contacts/DELETE | Eliminar contactos no Azure Active Directory. |
| microsoft.aad.directory/groups/appRoleAssignments/update | Atualizar a propriedade groups.appRoleAssignments no Azure Active Directory. |
| Microsoft.aad.Directory/Groups/Basic/Update | Atualizar as propriedades básicas em grupos no Azure Active Directory. |
| Microsoft.aad.Directory/Groups/Create | Criar grupos no Azure Active Directory. |
| microsoft.aad.directory/groups/createAsOwner | Criar grupos no Azure Active Directory. Criador é adicionado como o primeiro proprietário e o objeto criado conta contra a quota de 250 objetos criados do criador. |
| Microsoft.aad.Directory/Groups/DELETE | Eliminar grupos no Azure Active Directory. |
| microsoft.aad.directory/groups/hiddenMembers/read | Ler a propriedade groups.hiddenMembers no Azure Active Directory. |
| Microsoft.aad.Directory/Groups/members/Update | Atualizar a propriedade groups.members no Azure Active Directory. |
| Microsoft.aad.Directory/Groups/Owners/Update | Atualize a propriedade groups.owners no Azure Active Directory. |
| Microsoft.aad.Directory/Groups/Restore | Restaurar grupos no Azure Active Directory. |
| Microsoft.aad.Directory/Groups/Settings/Update | Atualizar a propriedade groups.settings no Azure Active Directory. |
| microsoft.aad.directory/users/appRoleAssignments/update | Atualizar a propriedade users.appRoleAssignments no Azure Active Directory. |
| microsoft.aad.directory/users/assignLicense | Gerir licenças em utilizadores no Azure Active Directory. |
| Microsoft.aad.Directory/Users/Basic/Update | Atualizar propriedades básicas em utilizadores no Azure Active Directory. |
| Microsoft.aad.Directory/Users/Create | Criar utilizadores no Azure Active Directory. |
| Microsoft.aad.Directory/Users/DELETE | Eliminar utilizadores no Azure Active Directory. |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Invalide todos os tokens de atualização de utilizador no Azure Active Directory. |
| Microsoft.aad.Directory/Users/Manager/Update | Atualizar a propriedade users.manager no Azure Active Directory. |
| Microsoft.aad.Directory/Users/password/Update | Atualize as palavras-passe para todos os utilizadores no Azure Active Directory. Consulte a documentação online para obter mais detalhes. |
| Microsoft.aad.Directory/Users/Restore | Restaure utilizadores eliminados no Azure Active Directory. |
| microsoft.aad.directory/users/userPrincipalName/update | Atualizar a propriedade users.userPrincipalName no Azure Active Directory. |
| microsoft.azure.accessService/allEntities/allTasks | Gerir todos os aspetos do serviço de Acesso do Azure. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Ler e configurar o Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Criar e gerir pedidos de suporte do Azure. |
| Microsoft.office365.serviceHealth/allEntities/allTasks | Leia e configure o Estado de Funcionamento do Serviço Office 365. |
| Microsoft.office365.supportTickets/allEntities/allTasks | Crie e gira pedidos de suporte do Office 365. |

## <a name="deprecated-roles"></a>Funções preteridas

As seguintes funções não devem ser utilizadas. Eles foram preteridos e serão removidos do Azure AD no futuro.

* Administrador de Licenças AdHoc
* Associação de dispositivo
* Gestores de Dispositivo
* Utilizadores de dispositivos
* Criador de Utilizador Verificado por E-mail
* Administrador da Caixa de Correio
* Associação de Dispositivos da Área de Trabalho

## <a name="next-steps"></a>Passos Seguintes

* Para saber mais sobre como atribuir um utilizador como administrador de uma subscrição do Azure, veja [gerir o acesso com RBAC e o portal do Azure](../../role-based-access-control/role-assignments-portal.md)
* Para saber mais sobre como o Microsoft Azure controla o acesso aos recursos, consulte [Noções sobre o acesso aos recursos no Azure](../../role-based-access-control/rbac-and-directory-admin-roles.md)
* Para obter mais informações sobre como o Azure Active Directory está relacionado com a sua subscrição do Azure, veja [Como as subscrições do Azure estão associadas ao Azure Active Directory](../fundamentals/active-directory-how-subscriptions-associated-directory.md)
