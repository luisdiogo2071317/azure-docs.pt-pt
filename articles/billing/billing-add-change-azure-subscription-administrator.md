---
title: Adicionar ou alterar funções de subscrição de administrador do Azure | Documentos da Microsoft
description: Descreve como adicionar ou alterar o Coadministrador do Azure, o administrador de serviços e o administrador de conta
services: ''
documentationcenter: ''
author: genlin
manager: jlian
editor: ''
tags: billing
ms.assetid: 13a72d76-e043-4212-bcac-a35f4a27ee26
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/19/2018
ms.author: cwatson
ms.openlocfilehash: d6e99c2d57baa5fc62f3894abc9d04635f81f5aa
ms.sourcegitcommit: 17633e545a3d03018d3a218ae6a3e4338a92450d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/22/2018
ms.locfileid: "49638057"
---
# <a name="add-or-change-azure-subscription-administrators"></a>Adicionar ou alterar os administradores de subscrição do Azure

Para gerir o acesso aos recursos do Azure, tem de ter a função de administrador adequada. Este artigo descreve como adicionar ou alterar a função de administrador para um utilizador ao nível da subscrição.

> [!div class="nextstepaction"]
> [Ajude a melhorar os documentos de faturação do Azure](https://go.microsoft.com/fwlink/p/?linkid=2010091)

## <a name="what-administrator-role-do-i-use"></a>Função de administrador que utilizo?

O Azure tem várias funções diferentes. Para gerir o acesso a recursos, pode usar as funções de administrador de subscrição clássica, como administrador de serviços e coadministrador ou um sistema de autorização mais recente, chamado de controlo de acesso baseado em funções (RBAC). Para garantir um melhor controle e para simplificar a gestão de acesso, recomendamos que utilize o RBAC para todas as necessidades de gestão de acesso. Se possível, recomendamos que reconfigure as políticas de acesso existentes com o RBAC. Para obter mais informações, consulte [o que é o controlo de acesso baseado em funções (RBAC)](../role-based-access-control/overview.md) e [compreender as diferentes funções no Azure](../role-based-access-control/rbac-and-directory-admin-roles.md).

<a name="add-an-admin-for-a-subscription"></a>

## <a name="add-an-rbac-owner-for-a-subscription-in-azure-portal"></a>Adicionar um proprietário do RBAC para obter uma subscrição no portal do Azure 

Para adicionar alguém como administrador para uma subscrição do Azure, atribua-lhe a função [Proprietário](../role-based-access-control/built-in-roles.md#owner) (uma função RBAC) no âmbito da subscrição. A função Proprietário pode gerir os recursos na subscrição que atribuiu e não tem privilégio de acesso a outras subscrições.

1. Visite [ **subscrições** no portal do Azure](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
2. Selecione a subscrição à qual deseja atribuir acesso.
3. Selecione **Controlo de acesso (IAM)** na lista.
4. Selecione **Adicionar**.
   (Se o botão Adicionar estiver em falta, não tem permissão para adicionar permissões.)
5. Na caixa **Função**, selecione **Proprietário**. 
6. Na caixa **Atribuir acesso a**, selecione **Utilizador, grupo ou aplicação do Azure AD**. 
7. Na caixa **Selecionar**, escreva o endereço de e-mail do utilizador que deseja adicionar como Proprietário. Selecione o utilizador e, em seguida, selecione **Guardar**.

    ![Captura de ecrã que mostra a função de proprietário selecionada](./media/billing-add-change-azure-subscription-administrator/add-role.png)

Isso dá ao usuário acesso total a todos os recursos, incluindo o direito de delegar o acesso a outras pessoas. Para dar acesso a um âmbito diferente, como um grupo de recursos, visite o **controlo de acesso (IAM)** painel para esse âmbito.

## <a name="add-or-change-co-administrator"></a>Adicionar ou alterar o coadministrador

Apenas um [Proprietário](../role-based-access-control/built-in-roles.md#owner) pode ser adicionado como Coadministrador. Outros utilizadores com funções, como [Contribuidor](../role-based-access-control/built-in-roles.md#contributor) e [Leitor](../role-based-access-control/built-in-roles.md#reader) não podem ser adicionados como Coadministradores.

> [!TIP]
> Apenas terá de adicionar o proprietário como um coadministrador se o utilizador precisar gerir implementações clássicas do Azure. Recomendamos que utilize o RBAC para todos os outros fins.

1. Se ainda não o tiver feito, adicione a alguém como um proprietário de seguir instruções acima.
2. **Com o botão direito** o utilizador proprietário que acabou de adicionar e, em seguida, selecione **adicionar como coadministrador**. Se não vir a **adicionar como coadministrador** opção, atualize a página ou tente outro navegador de Internet. 

    ![Captura de ecrã que adiciona coadministrador](./media/billing-add-change-azure-subscription-administrator/add-coadmin.png)

    Para remover a permissão de coadministrador **com o botão direito** o utilizador de coadministrador e selecione **remover coadministrador**.

    ![Captura de ecrã que remove o coadministrador](./media/billing-add-change-azure-subscription-administrator/remove-coadmin.png)

### <a name="adding-a-guest-user-as-a-co-administrator"></a>Adicionar um utilizador convidado como um coadministrador

Os utilizadores convidados que foram atribuídos a função de coadministrador poderão ver algumas diferenças em comparação com os utilizadores de membro com a função de coadministrador. Considere o seguinte cenário:

- O utilizador A com uma conta escolar ou profissional do Azure AD é um administrador de serviços para uma subscrição do Azure.
- O utilizador B tem uma conta Microsoft.
- O utilizador A atribui a função de coadministrador ao utilizador B.
- O utilizador B pode fazer quase tudo, mas não consegue registar aplicações ou procurar os utilizadores no diretório do Azure AD.

Poderia esperar que o utilizador B pode gerir tudo. O motivo para essa diferença é que a conta da Microsoft é adicionada à subscrição como um utilizador convidado, em vez de um utilizador de membro. Os utilizadores convidados têm diferentes permissões predefinidas no Azure AD em comparação com os utilizadores membros. Por exemplo, os utilizadores membros podem ler a outros utilizadores no Azure AD e os utilizadores convidados não é possível. Utilizadores membros podem registar novos principais de serviço no Azure AD e os utilizadores convidados não podem. Se um utilizador convidado tem de ser capaz de executar estas tarefas, uma solução possível é específica de atribuir funções de administrador do Azure AD tem do utilizador convidado. Por exemplo, no cenário anterior, pode atribuir a [leitores de diretório](../active-directory/users-groups-roles/directory-assign-admin-roles.md#directory-readers) função ler outros utilizadores e atribuir a [programador da aplicação](../active-directory/users-groups-roles/directory-assign-admin-roles.md#application-developer) função para poder criar principais de serviço. Para obter mais informações sobre o membro e os utilizadores convidados e as respetivas permissões, consulte [quais são as permissões de usuário padrão no Azure Active Directory?](../active-directory/fundamentals/users-default-permissions.md).

Tenha em atenção que o [funções incorporadas para recursos do Azure](../role-based-access-control/built-in-roles.md) são diferentes do que o [funções de administrador do Azure AD](../active-directory/users-groups-roles/directory-assign-admin-roles.md). As funções incorporadas não concedem acesso ao Azure AD. Para obter mais informações, consulte [compreender as diferentes funções](../role-based-access-control/rbac-and-directory-admin-roles.md).

<a name="change-service-administrator-for-a-subscription"></a>

## <a name="change-the-service-administrator-for-an-azure-subscription"></a>Alterar o administrador de serviço para uma subscrição do Azure

Apenas o administrador de conta pode alterar o administrador de serviços para uma subscrição. Por predefinição, quando se inscrever, o administrador de serviços é o mesmo que o administrador de conta. Se o administrador de serviços é alterado para um utilizador diferente, o administrador de conta perde o acesso ao portal do Azure. No entanto, o administrador de conta, pode utilizar Centro de contas sempre para alterar o administrador de serviços para si.

1. Certifique-se de que o seu cenário é suportado, verificando a [limites para alterar os administradores de serviços](#limits).
1. Inicie sessão no [Centro de contas](https://account.windowsazure.com/subscriptions) como administrador de conta.
1. Selecione uma subscrição.
1. No lado direito, selecione **editar detalhes da subscrição**.

    ![Captura de ecrã que mostra o botão de subscrição de edição no Centro de contas](./media/billing-add-change-azure-subscription-administrator/editsub.png)
1. Na **administrador de serviços** , introduza o endereço de e-mail do administrador de serviço novo.

    ![Captura de ecrã que mostra a caixa para alterar o e-mail de administrador de serviço](./media/billing-add-change-azure-subscription-administrator/changeSA.png)

<a name="limits"></a>

### <a name="limitations-for-changing-service-administrators"></a>Limitações para alterar os administradores de serviços

* Cada subscrição está associada um diretório do Azure AD. Para localizar o diretório da subscrição está associada, aceda a [ **subscrições**](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade), em seguida, selecione uma subscrição para ver o diretório.
* Se tiver entrado com uma conta profissional ou escolar, pode adicionar outras contas na sua organização como administrador de serviço. Por exemplo, abby@contoso.com pode adicionar bob@contoso.com como o administrador de serviço, mas não é possível adicionar john@notcontoso.com , a menos que john@notcontoso.com tiver uma presença no diretório de contoso.com. Utilizadores com sessão iniciada com contas de trabalho ou escola podem continuar a adicionar utilizadores de Account Microsoft como administrador de serviços.

  | Método de início de sessão | Adicionar utilizador de Account Microsoft como um administrador de serviços? | Adicionar conta profissional ou escolar na mesma organização como um administrador de serviços? | Adicionar conta profissional ou escolar na outra organização como um administrador de serviços? |
  | --- | --- | --- | --- |
  |  Conta Microsoft |Sim |Não |Não |
  |  Conta escolar ou profissional |Sim |Sim |Não |

## <a name="change-the-account-administrator-for-an-azure-subscription"></a>Alterar o administrador de conta para uma subscrição do Azure

O administrador de conta é o utilizador que inicialmente se inscreveu na subscrição do Azure e é responsável como o proprietário de faturação da subscrição. Para alterar o administrador de conta de uma subscrição, consulte [transferir a propriedade de uma subscrição do Azure para outra conta](billing-subscription-transfer.md).

<a name="check-the-account-administrator-of-the-subscription"></a>

**Não tem a certeza que o administrador de conta é?** Siga estes passos.

1. Visite [ **subscrições** no portal do Azure](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
1. Selecione a subscrição que pretende verificar e, em seguida, procure em **definições**.
1. Selecione **propriedades**. O administrador de conta da subscrição é apresentado na **administrador de conta** caixa.  

## <a name="learn-more-about-resource-access-control-and-active-directory"></a>Saiba mais sobre o controlo de acesso de recursos e o Active Directory

* Para saber mais sobre o RBAC, veja [o que é o controlo de acesso baseado em funções (RBAC)?](../role-based-access-control/overview.md)
* Para saber mais sobre todas as funções no Azure, veja [compreender as diferentes funções no Azure](../role-based-access-control/rbac-and-directory-admin-roles.md).
* Para obter mais informações sobre o Azure Active Directory, consulte [subscrições do Azure como estão associadas com o Azure Active Directory](../active-directory/active-directory-how-subscriptions-associated-directory.md) e [atribuir funções de administrador no Azure Active Directory](../active-directory/users-groups-roles/directory-assign-admin-roles.md).

## <a name="need-help-contact-support"></a>Precisa de ajuda? Contacte o suporte.

Se precisar de ajuda, ainda [contacte o suporte](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para a sua questão resolvidos rapidamente.
