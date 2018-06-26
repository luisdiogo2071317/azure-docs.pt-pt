---
title: Comparar as permissões de utilizador predefinidas no Azure AD | Microsoft Docs
description: Comparar as permissões de membro, convidado, proprietário da aplicação e proprietário do grupo
services: active-directory
documentationcenter: ''
author: eross-msft
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: overview
ms.date: 01/29/2018
ms.author: lizross
ms.reviewer: vincesm
ms.openlocfilehash: 991feaa61a73d5ac96159fbd6a7dd1fdd420fd3c
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36291417"
---
# <a name="default-user-permissions-in-azure-active-directory"></a>Permissões de utilizador predefinidas no Azure Active Directory

No Azure Active Directory (Azure AD), é concedido a todos os utilizadores um conjunto de permissões predefinidas. O acesso de um utilizador consiste no tipo de utilizador, nas respetivas [associações de funções](https://docs.microsoft.com/azure/active-directory/active-directory-users-assign-role-azure-portal) e na respetiva propriedade dos objetos individuais. Este artigo descreve essas permissões predefinidas e contém uma comparação entre as predefinições do utilizador membro e convidado.

## <a name="member-and-guest-users"></a>Utilizadores membros e convidados
O conjunto de permissões predefinidas recebido depende se o utilizador é um membro nativo do inquilino (utilizador membro) ou se o utilizador é um convidado de colaboração B2B (utilizador convidado). Para obter mais informações sobre a colaboração B2B, veja [O que é a colaboração B2B do Azure AD?](../b2b/what-is-b2b.md) para obter mais informações sobre os utilizadores convidados. 
* Os utilizadores membros podem registar aplicações, gerir a fotografia de perfil e o número de telemóvel, alterar a palavra-passe e convidar convidados B2B. Além disso, os utilizadores podem ler todas as informações de diretório (com algumas exceções). 
* Os utilizadores convidados B2B do Azure AD têm a permissões de diretório restringidas. Por exemplo, os utilizadores convidados não podem procurar informações do inquilino além das suas próprias informações de perfil. No entanto, um utilizador convidado pode obter informações sobre outro utilizador, fornecendo o Nome Principal de Utilizador ou o objectId. Um convidado não consegue ver as informações sobre outros objetos de inquilino, como grupos e aplicações.

As permissões predefinidas para os convidados são restritivas por predefinição. Os convidados podem ser adicionados a funções de administrador, que lhes concedem permissões totais de leitura e escrita contidas na função. Há uma restrição adicional disponível, a capacidade de os convidados convidarem outros convidados. Definir **Os convidados podem convidar** para **Não** impede os convidados de convidarem outros convidados. Veja [Delegar convites para colaboração B2B](../b2b/delegate-invitations.md) para saber como. Para conceder aos utilizadores convidados as mesmas permissões dos utilizadores membros por predefinição, defina **As permissões dos utilizadores convidados são limitadas** para **Não**. Esta definição concede a todos os utilizadores membros permissões para convidar utilizadores por predefinição, bem com permite que sejam adicionados convidados a funções administrativas.

## <a name="compare-member-and-guest-default-permissions"></a>Comparar as permissões predefinidas de membro e convidado

**Área** | **Permissões de utilizador membro** | **Permissões de utilizador convidado**
------------ | --------- | ----------
Utilizadores e contactos | Ler todas as propriedades públicas de utilizadores e contactos<br>Convidar convidados<br>Alterar a própria palavra-passe<br>Gerir o próprio número de telemóvel<br>Gerir a própria fotografia<br>Invalidar os próprios tokens de atualização | Ler as próprias propriedades<br>Ler o nome a apresentar, e-mail, nome de início de sessão, fotografia, nome principal de utilizador e as propriedades de tipo de utilizador de outros utilizadores e contactos<br>Alterar a própria palavra-passe
Grupos | Criar grupos de segurança<br>Criar grupos do Office 365<br>Ler todas as propriedades dos grupos<br>Ler as associações de grupo não oculto<br>Associações de grupo de leitura oculto do Office 365 para grupo associado<br>Gerir propriedades, a propriedade e a associação dos grupos detidos<br>Adicionar convidados a grupos detidos<br>Gerir definições de associação dinâmica<br>Eliminar grupos detidos<br>Restaurar grupos detidos do Office 365 | Ler todas as propriedades dos grupos<br>Ler as associações de grupo não oculto<br>Associações a grupo de leitura oculto do Office 365 para grupos associados<br>Gerir grupos detidos<br>Adicionar convidados a grupos detidos (se permitido)<br>Eliminar grupos detidos<br>Restaurar grupos detidos do Office 365 
Aplicações | Registar (criar) nova aplicação<br>Ler as propriedades de aplicações registadas e empresariais<br>Gerir as propriedades da aplicação, atribuições e credenciais para aplicações detidas<br>Criar ou eliminar palavra-passe de aplicação do utilizador<br>Eliminar aplicações detidas<br>Restaurar aplicações detidas | Ler as propriedades de aplicações registadas e empresariais<br>Gerir as propriedades da aplicação, atribuições e credenciais para aplicações detidas<br>Eliminar aplicações detidas<br>Restaurar aplicações detidas
Dispositivos | Ler todas as propriedades dos dispositivos<br>Gerir todas as propriedades dos dispositivos detidos<br> | Sem permissões<br>Eliminar dispositivos detidos<br>
Diretório | Ler todas as informações da empresa<br>Ler todos os domínios<br>Ler todos os contratos de parceiros | Ler o nome a apresentar e os domínios verificados
Funções e Âmbitos | Ler todas as funções e associações administrativas<br>Ler todas as propriedades e associação de unidades administrativas | Sem permissões 
Subscrições | Ler todas as subscrições<br>Ativar Membro do Plano de Serviço | Sem permissões
Políticas | Ler todas as propriedades das políticas<br>Gerir todas as propriedades da política detida | Sem permissões

## <a name="to-restrict-the-default-permissions-for-member-users"></a>Para restringir as permissões predefinidas para os utilizadores membros

As permissões predefinidas para os utilizadores membros podem ser restringidas das seguintes formas.

Permissão | Explicação da definição
---------- | ------------
Capacidade de criar grupos de segurança | Definir esta opção para Não impede que os utilizadores criem grupos de segurança. Os Administradores Globais e Administradores de Conta de Utilizador podem, ainda assim, criar grupos de segurança. Veja [Cmdlets do Azure Active Directory para configurar definições de grupo](../active-directory-accessmanagement-groups-settings-cmdlets.md) para saber como.
Capacidade de criar grupos do Office 365 | Definir esta opção para Não impede que os utilizadores criem grupos do Office 365. Definir esta opção para Alguns permite que um conjunto selecionado de utilizadores crie grupos do Office 365. Os Administradores Globais e Administradores de Conta de Utilizador conseguirão, ainda assim, criar grupos do Office 365. Veja [Cmdlets do Azure Active Directory para configurar definições de grupo](../active-directory-accessmanagement-groups-settings-cmdlets.md) para saber como.
Restringir o acesso ao portal de administração do Azure AD | Definir esta opção para Não impede que os utilizadores acedam ao portal do Azure Active Directory.
Capacidade de ler outros utilizadores | Esta definição só está disponível no PowerShell. Definir isto como $false impede todos os utilizadores não administradores de lerem as informações de utilizador do diretório. Isto não impede a leitura das informações de utilizador nos outros serviços Microsoft, como o Exchange Online. Esta definição destina-se a circunstâncias especiais e definir isto para $false não é recomendado.

## <a name="object-ownership"></a>Propriedade do objeto

### <a name="application-registration-owner-permissions"></a>Permissões de proprietário de registo de aplicação
Quando um utilizador regista uma aplicação, é adicionado automaticamente como um proprietário da aplicação. Como proprietário, pode gerir os metadados da aplicação, como o nome e as permissões que a aplicação requer. Também pode gerir a configuração específica de inquilino da aplicação, como a configuração do início de sessão único e as atribuições de utilizador. Um proprietário também pode adicionar ou remover outros proprietários. Ao contrário dos Administradores Globais, os proprietários só podem gerir as aplicações que possuem.

<!-- ### Enterprise application owner permissions

When a user adds a new enterprise application, they are automatically added as an owner for the tenant-specific configuration of the application. As an owner, they can manage the tenant-specific configuration of the application, such as the SSO configuration, provisioning, and user assignments. An owner can also add or remove other owners. Unlike Global Administrators, owners can manage only the applications they own. <!--To assign an enterprise application owner, see *Assigning Owners for an Application*.-->

### <a name="group-owner-permissions"></a>Permissões de proprietário do grupo

Quando um utilizador cria um grupo, é adicionado automaticamente como um proprietário desse grupo. Como proprietário, pode gerir propriedades do grupo, como o nome, bem como gerir a associação. Um proprietário também pode adicionar ou remover outros proprietários. Ao contrário dos Administradores Globais e Administradores de Conta de Utilizador, os proprietários só podem gerir os grupos que possuem. Para atribuir um proprietário de grupo, veja [Gerir os proprietários de um grupo](active-directory-accessmanagement-managing-group-owners.md).

## <a name="next-steps"></a>Passos seguintes

* Para saber mais sobre como alterar os administradores para uma subscrição do Azure, consulte [Como adicionar ou alterar funções de administrador do Azure](../../billing/billing-add-change-azure-subscription-administrator.md)
* Para saber mais sobre como o Microsoft Azure controla o acesso aos recursos, consulte [Noções sobre o acesso aos recursos no Azure](../../role-based-access-control/rbac-and-directory-admin-roles.md)
* Para obter mais informações sobre como o Azure Active Directory está relacionado com a sua subscrição do Azure, veja [Como as subscrições do Azure estão associadas ao Azure Active Directory](active-directory-how-subscriptions-associated-directory.md)
* [Gerir utilizadores](add-users-azure-active-directory.md)
