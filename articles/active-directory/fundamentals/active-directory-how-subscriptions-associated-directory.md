---
title: Adicionar uma subscrição do Azure existente ao seu inquilino - Azure Active Directory | Documentos da Microsoft
description: Instruções sobre como adicionar uma subscrição do Azure existente com o seu inquilino do Azure Active Directory.
services: active-directory
author: eross-msft
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: lizross
ms.reviewer: jeffsta
ms.custom: it-pro, seodec18
ms.openlocfilehash: 20c4be3741fa1e728cf59a148beb5adbc87b89c0
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/22/2019
ms.locfileid: "54452034"
---
# <a name="associate-or-add-an-azure-subscription-to-your-azure-active-directory-tenant"></a>Associar ou adicionar uma subscrição do Azure com o seu inquilino do Azure Active Directory
A subscrição do Azure tem uma relação de confiança com o Azure Active Directory (Azure AD), que significa que a subscrição confianças de entidades do Azure AD para autenticar utilizadores, serviços e dispositivos. Várias subscrições podem confiar no mesmo diretório do Azure AD, mas cada subscrição apenas pode confiar num único diretório.

Se a sua subscrição expirar, perderá o acesso a todos os outros recursos associados à subscrição. No entanto, o diretório do Azure AD permanece no Azure, que lhe permite associar e gerir o diretório a utilizar uma subscrição do Azure diferente.

Todos os utilizadores têm um diretório de "raiz" único para autenticação. No entanto, os usuários também podem ser convidados noutros diretórios. Pode ver os dois diretórios raiz e convidados para cada utilizador no Azure AD.

>[!Important]
>Todos os [controlo de acesso baseado em funções (RBAC)](../../role-based-access-control/role-assignments-portal.md) os utilizadores com acesso atribuído, juntamente com todos os administradores da subscrição perdem o acesso após a subscrição alterações no diretório. Além disso, se tiver quaisquer cofres de chaves, será também afetados pela movimentação da subscrição. Para corrigi-lo, terá [alterar o ID de inquilino do Cofre de chaves](../../key-vault/key-vault-subscription-move-fix.md) antes de continuar operações.


## <a name="before-you-begin"></a>Antes de começar
Antes de poder associar ou adicionar a sua subscrição, tem de efetuar as seguintes tarefas:

- Inicie sessão com uma conta que:
    - Tem **proprietário do RBAC** acesso à subscrição.

    - Existe no diretório atual que está associada a subscrição e no novo diretório em que é onde deseja associar a subscrição no futuro. Para obter mais informações sobre como obter acesso a outro diretório, consulte [como os administradores do Azure Active Directory adicionar utilizadores de colaboração B2B?](../b2b/add-users-administrator.md).

- Certifique-se de que não estiver a utilizar uma subscrição de fornecedores de serviços de Cloud do Azure (CSP) (MS-AZR - 0145p, MS - AZR - 0146p, MS - AZR - 159p), uma subscrição do Microsoft Internal (MS-AZR - 0015P) ou uma subscrição Microsoft Imagine (MS-AZR - 0144p).
    
## <a name="to-associate-an-existing-subscription-to-your-azure-ad-directory"></a>Associar uma subscrição existente ao diretório do Azure AD
1. Iniciar sessão e selecione a subscrição que pretende utilizar a partir da [página de subscrições no portal do Azure](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).

2. Selecione **altere o diretório**.

    ![Página de subscrições, com a opção de diretório de alteração realçada](media/active-directory-how-subscriptions-associated-directory/change-directory-button.png)

3. Reveja os avisos que aparecem e, em seguida, selecionam **alteração**.

    ![Alterar a página do diretório, que mostra o diretório para alterar para](media/active-directory-how-subscriptions-associated-directory/edit-directory-ui.png)

    O diretório é alterado para a subscrição e obtém uma mensagem de êxito.

    ![Mensagem de êxito](media/active-directory-how-subscriptions-associated-directory/edit-directory-success.png)    

4. Utilize o alternador de diretório para ir para o seu novo diretório. Poderá demorar até 10 minutos para que tudo seja apresentado corretamente.

    ![Página de alternador de diretório](media/active-directory-how-subscriptions-associated-directory/directory-switcher.png)

Alterar o diretório de subscrição é uma operação de nível de serviço, para que não afeta a propriedade de faturação de subscrição. O administrador de conta pode ainda alterar o administrador de serviço a partir da [Centro de contas](https://account.azure.com/subscriptions). Para eliminar o diretório original, tem de transferir a subscrição de faturação a propriedade para um administrador de conta nova. Para obter mais informações sobre como transferir a propriedade da faturação, veja [Transferir a propriedade de uma subscrição do Azure para outra conta](../../billing/billing-subscription-transfer.md). 

## <a name="next-steps"></a>Passos Seguintes

- Para criar um novo do Azure AD de inquilino, consulte [acesso do Azure Active Directory para criar um novo inquilino](active-directory-access-create-new-tenant.md)

- Para saber mais sobre como o Microsoft Azure controla o acesso aos recursos, consulte [Noções sobre o acesso aos recursos no Azure](../../role-based-access-control/rbac-and-directory-admin-roles.md)

- Para saber mais sobre como atribuir funções no Azure AD, veja [como atribuir funções de diretório para os utilizadores com o Azure Active Directory](active-directory-users-assign-role-azure-portal.md)
