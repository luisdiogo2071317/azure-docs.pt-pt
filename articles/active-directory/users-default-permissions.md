---
title: Comparar as permissões de utilizador predefinidas no Azure Active Directory | Microsoft Docs
description: Comparar o membro, o convidado, o proprietário da aplicação e permissões de proprietário do grupo
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 01/29/2018
ms.author: curtand
ms.reviewer: vincesm
ms.openlocfilehash: a9bf9748de5f390f95b8b672e0cf77afa5c49581
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="default-user-permissions-in-azure-active-directory"></a>Permissões de utilizador predefinidas no Azure Active Directory

No Azure Active Directory (Azure AD), todos os utilizadores recebem um conjunto de permissões predefinidas. Acesso de um utilizador é composta pelo tipo de utilizador, os respetivos [associações de função](https://docs.microsoft.com/azure/active-directory/active-directory-users-assign-role-azure-portal)e a respetiva propriedade dos objetos individuais. Este artigo descreve as permissões predefinidas e contém uma comparação entre as predefinições do utilizador membro e de convidado.

## <a name="member-and-guest-users"></a>Utilizadores de membro e o convidado
O conjunto de permissões predefinidas recebido depende se o utilizador for um membro nativo do inquilino (utilizador membro) ou se o utilizador for um convidado de colaboração B2B (utilizador convidado). Para obter mais informações sobre a colaboração B2B, consulte [o que é a colaboração B2B do Azure AD?](active-directory-b2b-what-is-azure-ad-b2b.md) para obter mais informações sobre os utilizadores convidados). 
* Utilizadores de membro podem registar aplicações, gerir os seus próprios número de fotografias e telemóvel do perfil, alterar as suas próprias palavras-passe e convidar B2B convidados. Além disso, os utilizadores podem ler todas as informações de diretório (com algumas exceções). 
* Os utilizadores de convidado do Azure AD B2B tem restringido a permissões de diretório. Por exemplo, os utilizadores convidados não podem procurar informações do inquilino para além das suas próprias informações de perfil. No entanto, um utilizador convidado pode obter informações sobre como outro utilizador, fornecendo o nome do Principal de utilizador ou o objectId. Um convidado não é possível ver as informações sobre outros objetos de inquilino, tais como grupos e aplicações.

As permissões predefinidas para os convidados são restritivas por predefinição. Os convidados podem ser adicionados a funções de administrador, conceder-lhes leitura total e permissões de escrita contidas na função de. Há uma restrição adicional disponível, a capacidade dos convidados convidar outros convidados. Definição **convidados podem convidar** para **não** impede que os convidados inviting outros convidados. Consulte [delegar convites para colaboração B2B](active-directory-b2b-delegate-invitations.md) para saber como. Para conceder a utilizadores convidados as mesmas permissões como utilizadores de membro por predefinição, defina **permissões de utilizadores convidados estão limitadas** para **não**. Esta definição concede aos membros de todas as permissões de utilizador para os utilizadores convidados por predefinição, bem como permitir que os convidados a ser adicionado ao funções administrativas.

## <a name="compare-member-and-guest-default-permissions"></a>Comparar as permissões predefinidas de membro e o convidado

**Área** | **Permissões de utilizador do membro** | **Permissões de utilizador convidado**
------------ | --------- | ----------
Utilizadores e contactos | Ler todas as propriedades públicas de utilizadores e contactos<br>Convidar convidados<br>Alterar a própria palavra-passe<br>Gerir o número de telemóvel próprio<br>Gerir fotografia própria<br>Invalidar própria atualização de tokens | Ler as suas próprias propriedades<br>Ler o nome a apresentar, e-mail, nome de início de sessão, fotografias, nome principal de utilizador e propriedades de tipo de utilizador de outros utilizadores e contactos<br>Alterar a própria palavra-passe
Grupos   | Criar grupos de segurança<br>Criar grupos do Office 365<br>Ler todas as propriedades dos grupos<br>Ler as associações de grupo não oculto<br>Leitura oculto associações a grupos do Office 365 para associados a um grupo<br>Gerir propriedades, a propriedade e a associação dos grupos pertencentes à empresa<br>Adicionar convidados em grupos pertencentes à empresa<br>Gerir definições de associação dinâmica<br>Eliminar grupos pertencentes à empresa<br>Restauro pertencentes a grupos do Office 365 | Ler todas as propriedades dos grupos<br>Ler as associações de grupo não oculto<br>Ler ocultas associações a grupos do Office 365 para grupos associados<br>Gerir grupos pertencentes à empresa<br>Adicionar convidados em grupos pertencentes à empresa (se for permitida)<br>Eliminar grupos pertencentes à empresa<br>Restauro pertencentes a grupos do Office 365           
Aplicações | Registar (criar) nova aplicação<br>Leitura de propriedades de registado e aplicações empresariais<br>Gerir propriedades da aplicação, atribuições e as credenciais para aplicações pertencentes à empresa<br>Criar ou eliminar aplicação palavra-passe do utilizador<br>Eliminar aplicações pertencentes à empresa<br>Restore aplicações pertencentes à empresa | Leitura de propriedades de registado e aplicações empresariais<br>Gerir propriedades da aplicação, atribuições e as credenciais para aplicações pertencentes à empresa<br>Eliminar aplicações pertencentes à empresa<br>Restore aplicações pertencentes à empresa
Dispositivos | Ler todas as propriedades dos dispositivos<br>Gerir todas as propriedades dos dispositivos pertencentes à empresa<br> | Sem permissões<br>Eliminar dispositivos pertencentes à empresa<br>
Diretório | Ler todas as informações da empresa<br>Ler todos os domínios<br>Ler todos os contratos de parceiros | Nome a apresentar de leitura e a verificar a domínios
Funções e âmbitos | Ler todas as associações e funções administrativas<br>Ler todas as propriedades e associação a unidades administrativas | Sem permissões              
Subscrições | Ler todas as subscrições<br>Ativar membro do plano de serviço | Sem permissões
Políticas | Ler todas as propriedades de políticas<br>Gerir todas as propriedades da política de propriedade | Sem permissões

## <a name="to-restrict-the-default-permissions-for-member-users"></a>Para restringir as permissões predefinidas para os utilizadores de membro


Predefinição permissões para o membro de utilizadores podem ser restringidos das seguintes formas. Para obter mais informações, consulte [consentimento no Azure Active Directory, aplicações e permissões](active-directory-apps-permissions-consent.md).

Permissão | Explicação da definição
---------- | ------------
Capacidade de criar grupos de segurança | Definir esta opção para não impede que os utilizadores criar grupos de segurança. Os administradores globais e de administradores de conta de utilizador podem, ainda assim, criar grupos de segurança. Consulte [cmdlets do Azure Active Directory para configurar definições de grupo](active-directory-accessmanagement-groups-settings-cmdlets.md) para saber como.
Capacidade de criar grupos do Office 365 | Definir esta opção para não impede que os utilizadores criar grupos do Office 365. Definir esta opção para algumas permite um conjunto selecione de que os utilizadores criem grupos do Office 365. Os administradores globais e de administradores de conta de utilizador continuarão a poder criar grupos do Office 365. Consulte [cmdlets do Azure Active Directory para configurar definições de grupo](active-directory-accessmanagement-groups-settings-cmdlets.md) para saber como.
Capacidade para autorizar aplicações | Definir esta opção para não impede que os utilizadores consenting para aplicações. Os administradores globais continuarão a poder consentimento para aplicações. Para obter mais informações, consulte [consentimento no Azure Active Directory, aplicações e permissões](active-directory-apps-permissions-consent.md).
Capacidade de adicionar a Galeria de aplicações ao painel de acesso | Definir esta opção para não impede que os utilizadores adicionem Galeria de aplicações para as respetivas painel de acesso.
Capacidade para se registar (criar) aplicações | Definir esta opção para não impede que não administradores criar aplicações. Os administradores globais continuarão a poder criar aplicações. Para obter mais informações, consulte [consentimento no Azure Active Directory, aplicações e permissões](active-directory-apps-permissions-consent.md).
Os administradores e utilizadores na função de inviter de convidados podem convidar os convidados | Definir esta opção para não impede que todos os utilizadores convidados convidando. Consulte a configurar as permissões predefinidas para os utilizadores de membro. Para obter mais informações, consulte [consentimento no Azure Active Directory, aplicações e permissões](active-directory-apps-permissions-consent.md).
Os membros podem convidar os convidados | Definir este como não impede que os utilizadores convidados convidando. Global de administradores, administradores de conta de utilizador e convidado Inviters continuarão a poder convidar convidados. Para obter mais informações, consulte [consentimento no Azure Active Directory, aplicações e permissões](active-directory-apps-permissions-consent.md).
Restringir o acesso ao portal de administração do Azure AD | Definir esta opção para não impede que os utilizadores aceder ao portal do Azure Active Directory.
Capacidade de leitura a outros utilizadores | Esta definição só está disponível no PowerShell. Definir este como $false impede que todos os não-administradores ler contato de utilizador do diretório. Isto não o impede leitura contato de utilizador em outros serviços Microsoft como o Exchange Online. Esta definição destina-se para circunstâncias especiais e definição que isto para $false não é recomendado.

## <a name="object-ownership"></a>Propriedade do objeto

### <a name="application-registration-owner-permissions"></a>Permissões de proprietário de registo de aplicação
Quando um utilizador se regista uma aplicação, são adicionados automaticamente como um proprietário para a aplicação. Como um proprietário, podem gerir os metadados da aplicação, como o nome e permissões de pedidos de aplicação. Também podem gerir a configuração de inquilino específico da aplicação, tais como as atribuições de configuração e de utilizador do SSO. Um proprietário também pode adicionar ou remover outros proprietários. Ao contrário dos administradores globais, os proprietários só podem gerir aplicações possuem. Para atribuir um proprietário de registo de aplicação, consulte [registo de aplicações do Azure Active Directory](active-directory-app-registration.md).

<!-- ### Enterprise application owner permissions

When a user adds a new enterprise application, they are automatically added as an owner for the tenant-specific configuration of the application. As an owner, they can manage the tenant-specific configuration of the application, such as the SSO configuration, provisioning, and user assignments. An owner can also add or remove other owners. Unlike Global Administrators, owners can manage only the applications they own. <!--To assign an enterprise application owner, see *Assigning Owners for an Application*.-->

### <a name="group-owner-permissions"></a>Permissões de proprietário do grupo

Quando um utilizador cria um grupo, eles são adicionados automaticamente como um proprietário para esse grupo. Como um proprietário, estes podem gerir propriedades do grupo, como o nome, bem como gerir a associação. Um proprietário também pode adicionar ou remover outros proprietários. Ao contrário dos administradores globais e administradores de conta de utilizador, os proprietários só podem gerir grupos seus. Para atribuir um proprietário do grupo, consulte [Gerir proprietários de um grupo](active-directory-accessmanagement-managing-group-owners.md).

## <a name="next-steps"></a>Passos Seguintes

* Para saber mais sobre como alterar os administradores para uma subscrição do Azure, consulte [Como adicionar ou alterar funções de administrador do Azure](../billing-add-change-azure-subscription-administrator.md)
* Para saber mais sobre como o Microsoft Azure controla o acesso aos recursos, consulte [Noções sobre o acesso aos recursos no Azure](../role-based-access-control/rbac-and-directory-admin-roles.md)
* Para obter mais informações sobre como o Azure Active Directory relacionada com a sua subscrição do Azure, consulte [subscrições do Azure como estão associadas ao Azure Active Directory](active-directory-how-subscriptions-associated-directory.md)
* [Gerir utilizadores](active-directory-create-users.md)
