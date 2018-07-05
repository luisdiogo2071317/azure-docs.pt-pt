---
title: Aceleradores de solução de IoT do Azure e Azure Active Directory | Documentos da Microsoft
description: Descreve como aceleradores de solução de IoT do Azure utiliza o Azure Active Directory para gerir as permissões.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 11/10/2017
ms.author: dobett
ms.openlocfilehash: 676d5e553e2929ae09d447141ca315fd1cc448e3
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37450019"
---
# <a name="permissions-on-the-azureiotsolutionscom-site"></a>Permissões no azureiotsolutions.com site

## <a name="what-happens-when-you-sign-in"></a>O que acontece quando inicia sessão

Na primeira vez que iniciar sessão no [azureiotsuite.com][lnk-azureiotsolutions], o site determina os níveis de permissão com base no inquilino do Azure Active Directory (AAD) selecionada atualmente e subscrição do Azure.

1. Em primeiro lugar, para preencher a lista de inquilinos junto ao seu nome de utilizador, o site, descobre do Azure inquilinos do AAD pertencem a. Atualmente, o site apenas pode obter tokens de utilizador para um inquilino ao mesmo tempo. Por conseguinte, quando mudar inquilinos através da lista pendente no canto superior direito, o site iniciar a sua sessão nesse inquilino para obter os tokens nesse inquilino.

2. Em seguida, através do Azure, o site localiza as subscrições às quais o inquilino selecionado está associado. Ver as subscrições disponíveis quando cria um novo solution accelerator.

3. Por fim, o site obtém todos os recursos nas subscrições e grupos de recursos marcados como aceleradores de solução e preenche os mosaicos na home page.

As secções seguintes descrevem as funções que controlam o acesso aos Aceleradores.

## <a name="aad-roles"></a>Funções do AAD

As funções do AAD controlam a capacidade de aprovisionar Aceleradores de soluções, para gerir utilizadores e de alguns serviços do Azure num acelerador de solução.

Pode encontrar mais informações sobre funções de administrador no AAD em [atribuir funções de administrador no Azure AD][lnk-aad-admin]. O artigo atual se concentra a **Administrador Global** e o **utilizador** funções de diretório como o usado pelo solution accelerators.

### <a name="global-administrator"></a>Administrador global

Podem existir vários administradores globais por inquilino do AAD:

* Quando cria um inquilino do AAD, por predefinição, é o administrador global desse inquilino.
* O administrador global pode aprovisionar um Aceleradores de solução de básico e standard (uma implementação básica utiliza uma única máquina de Virtual do Azure).

### <a name="domain-user"></a>Utilizador de domínio

Podem existir vários usuários de domínio por inquilino do AAD:

* Um utilizador de domínio pode aprovisionar um acelerador de solução básica através do [azureiotsolutions.com] [ lnk-azureiotsolutions] site.
* Um utilizador de domínio pode criar um acelerador de solução básica com a CLI.

### <a name="guest-user"></a>Utilizador convidado

Podem existir vários utilizadores convidados por inquilino do AAD. Os utilizadores convidados têm um conjunto limitado de direitos no inquilino do AAD. Como resultado, os utilizadores convidados não podem aprovisionar um acelerador de solução no inquilino do AAD.

Para obter mais informações sobre utilizadores e funções no AAD, consulte os seguintes recursos:

* [Criar utilizadores no Azure AD][lnk-create-edit-users]
* [Atribuir utilizadores a aplicações][lnk-assign-app-roles]

## <a name="azure-subscription-administrator-roles"></a>Funções de administrador de subscrição do Azure

As funções de administrador do Azure controlam a capacidade de mapear uma subscrição do Azure para um inquilino do AAD.

Saiba mais sobre as funções de administrador do Azure no artigo [como adicionar ou alterar o Coadministrador do Azure, o administrador de serviços e o administrador de conta][lnk-admin-roles].

## <a name="faq"></a>FAQ

### <a name="im-a-service-administrator-and-id-like-to-change-the-directory-mapping-between-my-subscription-and-a-specific-aad-tenant-how-do-i-complete-this-task"></a>Sou administrador de serviço e gostaria de alterar o mapeamento de diretório entre a minha subscrição e um inquilino do AAD específico. Como posso concluir esta tarefa?

Consulte [para adicionar uma subscrição existente ao diretório do Azure AD](../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md#to-associate-an-existing-subscription-to-your-azure-ad-directory)

### <a name="i-want-to-change-a-service-administrator-or-co-administrator-when-logged-in-with-an-organizational-account"></a>Quero alterar um administrador de serviços ou Coadministrador, ao iniciar sessão com uma conta institucional

Consulte o artigo de suporte [alterar o administrador de serviços e Coadministrador ao iniciar sessão com uma conta institucional][lnk-service-admins].

### <a name="why-am-i-seeing-this-error-your-account-does-not-have-the-proper-permissions-to-create-a-solution-please-check-with-your-account-administrator-or-try-with-a-different-account"></a>Porque estou a ver este erro? "A sua conta não tem as permissões adequadas para criar uma solução. Consulte o seu administrador de conta ou tente com uma conta diferente."

Observe o diagrama a seguir para obter orientações sobre:

![][img-flowchart]

> [!NOTE]
> Se continuar a ver o erro depois de confirmar que são um administrador global do inquilino do AAD e coadministrador da subscrição, peça ao seu administrador de conta, remova o utilizador e reatribuir as permissões necessárias pela seguinte ordem. Primeiro, adicione o utilizador como administrador global e, em seguida, adicionar o utilizador como um coadministrador da subscrição do Azure. Se os problemas persistirem, contacte [ajuda e suporte][lnk-help-support].

### <a name="why-am-i-seeing-this-error-when-i-have-an-azure-subscription-an-azure-subscription-is-required-to-create-pre-configured-solutions-you-can-create-a-free-trial-account-in-just-a-couple-of-minutes"></a>Porque estou a ver este erro quando tenho uma subscrição do Azure? "Uma subscrição do Azure é necessário para criar soluções pré-configuradas. Pode criar uma conta de avaliação gratuita em apenas alguns minutos."

Se tem a certeza de que tem uma subscrição do Azure, valide o mapeamento do inquilino da sua subscrição e certifique-se de que o inquilino correto está selecionado na lista pendente. Se confirmou que o inquilino pretendido está correto, siga o diagrama anterior e valide o mapeamento da sua subscrição e este inquilino do AAD.

## <a name="next-steps"></a>Passos Seguintes
Para saber mais sobre Aceleradores de solução de IoT, veja como é possível [personalizar um acelerador de soluções][lnk-customize].

[img-flowchart]: media/iot-accelerators-permissions/flowchart.png

[lnk-azureiotsolutions]: https://www.azureiotsolutions.com
[lnk-rm-github-repo]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-pm-github-repo]: https://github.com/Azure/azure-iot-predictive-maintenance
[lnk-cf-github-repo]: https://github.com/Azure/azure-iot-connected-factory
[lnk-aad-admin]:../active-directory/users-groups-roles/directory-assign-admin-roles.md
[lnk-portal]: https://portal.azure.com
[lnk-create-edit-users]:../active-directory/fundamentals/active-directory-users-profile-azure-portal.md
[lnk-assign-app-roles]:../active-directory/manage-apps/assign-user-or-group-access-portal.md
[lnk-service-admins]: https://azure.microsoft.com/support/changing-service-admin-and-co-admin
[lnk-admin-roles]: ../billing/billing-add-change-azure-subscription-administrator.md
[lnk-resource-cs]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/DeviceAdministration/Web/Security/RolePermissions.cs
[lnk-help-support]: https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade
[lnk-customize]: iot-accelerators-remote-monitoring-customize.md
