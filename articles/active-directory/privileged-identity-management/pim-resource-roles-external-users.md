---
title: Convidar utilizadores externos e atribuir funções de recursos do Azure no PIM | Documentos da Microsoft
description: Saiba como convidar utilizadores externos e atribuir funções de recursos do Azure no Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: pim
ms.date: 11/29/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 561fccd3e0baee481937954cc65601576a4fd60f
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/01/2018
ms.locfileid: "52730250"
---
# <a name="invite-external-users-and-assign-azure-resource-roles-in-pim"></a>Convidar utilizadores externos e atribuir funções de recursos do Azure no PIM

Azure Directory de atividade (Azure AD) empresa-empresa (B2B) é um conjunto de recursos no Azure AD que permite que as organizações colaboram com utilizadores externos e os fornecedores que utilizam qualquer conta. Ao combiná-B2B com o Azure AD Privileged Identity Management (PIM), pode continuar a aplicar seus requisitos de governação e conformidade a utilizadores externos. Por exemplo, pode utilizar estas funcionalidades PIM para recursos do Azure com utilizadores externos:

- Atribuir acesso a recursos específicos do Azure
- Ativar o acesso just-in-time
- Especifique a data de duração e de fim da atribuição
- Exigir a MFA a ativação ou atribuição ativa
- Executar revisões de acesso
- Utilizar alertas e registos de auditoria

Este artigo descreve como convidar um utilizador externo para o seu diretório e gerir o acesso aos recursos do Azure com o PIM.

## <a name="when-would-you-invite-external-users"></a>Quando convidar utilizadores externos?

Seguem-se alguns cenários de exemplo quando pode convidar utilizadores externos ao seu diretório:

- Permitir que um fornecedor externo Self-empregado que tem apenas uma conta de e-mail para aceder aos recursos do Azure para um projeto.
- Permitir que um parceiro externo numa grande organização que utiliza serviços de Federação do Active Directory no local para aceder à sua aplicação de despesas.
- Permitir engenheiros de suporte não na sua organização (por exemplo, o suporte da Microsoft) para aceder temporariamente o seu recurso do Azure para resolver problemas.

## <a name="how-does-external-collaboration-using-b2b-work"></a>Como funciona a colaboração externa através do trabalho de B2B?

Quando utiliza o B2B, pode convidar um utilizador externo para o seu diretório. O utilizador externo parece estar no seu diretório, mas o utilizador não tem quaisquer credenciais associados a ele. Sempre que um utilizador externo tem de ser autenticados, tem de ser autenticados no respetivo diretório raiz e não no seu diretório. Isso significa que se o utilizador externo já não tem acesso ao respetivo diretório raiz, automaticamente perderem o acesso ao seu diretório. Por exemplo, se o utilizador externo deixa a sua organização, eles automaticamente perderem o acesso a quaisquer recursos partilhados com eles no seu diretório sem ter de fazer nada. Para obter mais informações sobre B2B, consulte [o que é o acesso do utilizador convidado no Azure Active Directory B2B?](../b2b/what-is-b2b.md).

![B2B e o utilizador externo](./media/pim-resource-roles-external-users/b2b-external-user.png)

## <a name="check-external-collaboration-settings"></a>Verifique as definições de colaboração externa

Para certificar-se de que pode convidar utilizadores externos no seu diretório, deve verificar as definições de colaboração externa.

1. Inicie sessão no [portal do Azure](https://portal.azure.com/).

1. Clique em **do Azure Active Directory** > **definições de utilizador**.

1. Clique em **gerir definições de colaboração externa**.

    ![Definições de colaboração externa](./media/pim-resource-roles-external-users/external-collaboration-settings.png)

1. Certifique-se de que o **administradores e utilizadores na função de autor podem convidar** parâmetro estiver definido como **Sim**.

## <a name="invite-an-external-user-and-assign-a-role"></a>Convidar um utilizador externo e atribuir uma função

Utilizar o PIM, pode convidar um utilizador externo e torná-los elegível para uma função de recursos do Azure como um utilizador de membro.

1. Inicie sessão no [portal do Azure](https://portal.azure.com/) com um utilizador que seja um membro a [administrador com função privilegiada](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator) ou [administrador de conta de utilizador](../users-groups-roles/directory-assign-admin-roles.md#user-account-administrator) função.

1. Open **do Azure AD Privileged Identity Management**.

1. Clique em **recursos do Azure**.

1. Utilize o **filtro de recursos** para filtrar a lista de recursos geridos.

1. Clique em recursos que pretende gerir, tal como um recurso, grupo de recursos, subscrição ou grupo de gestão.

    Deve definir o âmbito para precisa apenas de que o utilizador externo.

1. Em gerir, clique em **funções** para ver a lista de funções para recursos do Azure.

    ![Funções de recursos do Azure](./media/pim-resource-roles-external-users/resources-roles.png)

1. Clique na função mínima que o usuário precisará.

    ![Função selecionada](./media/pim-resource-roles-external-users/selected-role.png)

1. Na página de função, clique em **Add member** para abrir o painel de atribuição de novo.

1. Clique em **selecione um membro ou um grupo**.

    ![Selecionar um membro ou grupo](./media/pim-resource-roles-external-users/select-member-group.png)

1. Para convidar um utilizador externo, clique em **convidar**.

    ![Fazer um convite](./media/pim-resource-roles-external-users/invite-guest.png)

1. Depois de especificar um utilizador externo, clique em **convidar**.

    O utilizador externo deve ser adicionado como um membro selecionado.

1. Selecione um painel de membro ou grupo, clique em **selecione**.

1. No painel de definições de associação, selecione o tipo de atribuição e a duração.

    ![Definições de associação](./media/pim-resource-roles-external-users/membership-settings.png)

1. Para concluir a atribuição, clique em **feito** e, em seguida **Add**.

    A atribuição de função de utilizador externo irá aparecer na sua lista de função.

    ![Atribuição de função de utilizador externo](./media/pim-resource-roles-external-users/role-assignment.png)

## <a name="activate-role-as-an-external-user"></a>Ativar função como um utilizador externo

Como um utilizador externo, primeiro tem de aceitar o convite para o diretório do Azure AD e, possivelmente, ativar a sua função.

1. Abra a mensagem de e-mail com o seu convite de diretório. O e-mail terá um aspeto semelhante ao seguinte.

    ![Convite por e-mail](./media/pim-resource-roles-external-users/email-invite.png)

1. Clique nas **começar** ligação no e-mail.

1. Depois de rever as permissões, clique em **Accept**.

    ![Analisar permissões](./media/pim-resource-roles-external-users/invite-accept.png)

1. Poderá ser-lhe pedido para aceitar os termos de utilização e especificar se pretende manter sessão iniciada.

    É aberto o portal do Azure. Se estiver apenas qualificado para uma função, não terá acesso aos recursos.

1. Para ativar sua função, abra o e-mail com a ativar a ligação de função. O e-mail terá um aspeto semelhante ao seguinte.

    ![Convite por e-mail](./media/pim-resource-roles-external-users/email-role-assignment.png)

1. Clique em **ativar função** para abrir as funções elegíveis no PIM.

    ![As minhas funções - elegíveis](./media/pim-resource-roles-external-users/my-roles-eligible.png)

1. Clique em ação, o **Activate** ligação.

    Consoante as definições de função, precisará especificar algumas informações para ativar a função.

1. Depois de especificar as definições para a função, clique em **Activate** para ativar a função.

    ![Ativar função](./media/pim-resource-roles-external-users/activate-role.png)

    A menos que o administrador é necessária para aprovar o pedido, deve ter acesso aos recursos especificados.

## <a name="view-activity-for-an-external-user"></a>Ver a atividade para um utilizador externo

Tal como um utilizador de membro, pode ver os registos de auditoria para controlar o que os utilizadores externos estão fazendo.

1. Como administrador, abra o PIM e selecione o recurso que foi partilhado.

1. Clique em **auditoria de recurso** para ver a atividade para esse recurso. O código a seguir mostra um exemplo da atividade para um grupo de recursos.

    ![Auditoria de recurso](./media/pim-resource-roles-external-users/audit-resource.png)

1. Para ver a atividade para o utilizador externo, clique em **do Azure Active Directory** > **utilizadores** > utilizador externo.

1. Clique em **registos de auditoria** para ver os registos de auditoria para o diretório. Se necessário, pode especificar filtros.

    ![Auditoria de diretórios](./media/pim-resource-roles-external-users/audit-directory.png)

## <a name="next-steps"></a>Passos Seguintes

- [Atribuir funções de diretório do Azure AD no PIM](pim-how-to-add-role-to-user.md)
- [O que é o acesso do utilizador convidado no Azure Active Directory B2B?](../b2b/what-is-b2b.md)
