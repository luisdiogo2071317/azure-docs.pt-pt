---
title: Como adicionar uma subscrição existente do Azure ao diretório do Azure AD | Microsoft Docs
description: Como adicionar uma subscrição existente ao diretório do Azure AD
services: active-directory
documentationcenter: ''
author: eross-msft
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: conceptual
ms.date: 12/12/2017
ms.author: lizross
ms.reviewer: jeffsta
ms.custom: oldportal;it-pro;
ms.openlocfilehash: bcae3f51e2245928c8110d06f3514177d57ac883
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37450028"
---
# <a name="how-to-associate-or-add-an-azure-subscription-to-azure-active-directory"></a>Como associar ou adicionar uma subscrição do Azure ao Azure Active Directory

Este artigo inclui informações sobre a relação entre a subscrição do Azure e o Azure Active Directory (Azure AD) e como adicionar uma subscrição existente ao diretório do Azure AD. A sua subscrição do Azure tem uma relação de confiança com o Azure AD, o que significa que confia no diretório para autenticar utilizadores, serviços e dispositivos. Várias subscrições podem confiar no mesmo diretório, mas cada subscrição apenas pode confiar num diretório. 

A relação de confiança entre uma subscrição e um diretório é diferente da relação que ela tem com outros recursos no Azure (sites, bases de dados, e assim sucessivamente). Em caso de expiração de uma subscrição, será também interrompido o acesso aos outros recursos associados à subscrição. No entanto, o diretório do Azure AD permanece no Azure e pode associar uma subscrição diferente a esse diretório e gerir o diretório utilizando a nova subscrição.

Todos os utilizadores têm um diretório raiz único que os autenticam, embora também possam ser convidados noutros diretórios. No Azure AD, pode ver os diretórios dos quais a conta de utilizador é membro ou convidado.

## <a name="before-you-begin"></a>Antes de começar

* Tem de iniciar sessão com a conta que tenha acesso de Proprietário do RBAC para a subscrição.
* Tem de iniciar sessão com uma conta que existe no diretório atual que está associado à subscrição e no diretório ao qual pretende adicionar. Para obter mais informações sobre como obter acesso a outro diretório, veja [Como os administradores do Azure Active Directory adicionam utilizadores de colaboração do B2B?](../b2b/add-users-administrator.md)
* Esta funcionalidade não está disponível para subscrições CSP (MS-AZR-0145P, MS-AZR-0146P, MS-AZR-159P) e Microsoft Imagine (MS-AZR-0144P).

## <a name="to-associate-an-existing-subscription-to-your-azure-ad-directory"></a>Associar uma subscrição existente ao diretório do Azure AD

1. Inicie sessão e selecione uma subscrição a partir da [Página de subscrições no portal do Azure](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
2. Clique em **Alterar o diretório**.

    ![Captura de ecrã que mostra o botão de Alteração de diretório](./media/active-directory-how-subscriptions-associated-directory/edit-directory-button.PNG)
3. Consulte os avisos. Todos os [Controlo de Acesso baseado em Funções (RBAC)](../../role-based-access-control/role-assignments-portal.md) os utilizadores com acesso atribuído ao e todos os administradores da subscrição perdem o acesso quando o diretório da subscrição é alterado.
4. Selecionar um diretório.

    ![Captura de ecrã que mostra a UI de alteração de diretório](./media/active-directory-how-subscriptions-associated-directory/edit-directory-ui.PNG)
5. Clique em **Alterar**.
6. Êxito! Utilize o alternador de diretório para ir para o novo diretório. Poderá demorar até 10 minutos para que tudo seja apresentado corretamente.

    ![Captura de ecrã que mostra a notificação de êxito de alteração de diretório](./media/active-directory-how-subscriptions-associated-directory/edit-directory-success.PNG)

    ![Captura de ecrã que mostra o alternador](./media/active-directory-how-subscriptions-associated-directory/directory-switcher.PNG)


Todos os seus cofres de chaves do Azure também serão afetados pela movimentação da subscrição. Por isso, [altere o ID do inquilino do cofre de chaves](../../key-vault/key-vault-subscription-move-fix.md) antes de retomar as operações.

Alterar o diretório da subscrição é uma operação ao nível do serviço. Não afeta a propriedade de faturação da subscrição e o Administrador de Conta pode ainda alterar o Admin de Serviço utilizando o [Centro de Contas](https://account.azure.com/subscriptions). Se pretender eliminar o diretório original, tem de transferir a propriedade de faturação da subscrição para um Administrador de Conta novo. Para obter mais informações sobre como transferir a propriedade da faturação, veja [Transferir a propriedade de uma subscrição do Azure para outra conta](../../billing/billing-subscription-transfer.md). 

## <a name="next-steps"></a>Passos Seguintes

* Para obter mais informações sobre como criar um novo diretório do Azure AD gratuitamente, veja [Como obter um inquilino do Azure Active Directory](../develop/active-directory-howto-tenant.md)
* Para obter mais informações sobre como transferir a propriedade da faturação de uma subscrição do Azure, veja [Transferir a propriedade de uma subscrição do Azure para outra conta](../../billing/billing-subscription-transfer.md)
* Para saber mais sobre como o Microsoft Azure controla o acesso aos recursos, consulte [Noções sobre o acesso aos recursos no Azure](../../role-based-access-control/rbac-and-directory-admin-roles.md)
* Para obter mais informações sobre como atribuir funções no Azure AD, consulte [Atribuir funções de administrador no Azure Active Directory](../users-groups-roles/directory-assign-admin-roles.md)

<!--Image references-->
[1]: ./media/active-directory-how-subscriptions-associated-directory/WAAD_PassThruAuth.png
[2]: ./media/active-directory-how-subscriptions-associated-directory/WAAD_OrgAccountSubscription.png
[3]: ./media/active-directory-how-subscriptions-associated-directory/WAAD_SignInDisambiguation.PNG
