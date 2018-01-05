---
title: "Adicionar ou alterar funções de subscrição de administrador do Azure | Microsoft Docs"
description: "Descreve como adicionar ou alterar o Coadministrador do Azure, o administrador de serviço e o administrador de conta"
services: 
documentationcenter: 
author: genlin
manager: jlian
editor: 
tags: billing
ms.assetid: 13a72d76-e043-4212-bcac-a35f4a27ee26
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 01/04/2018
ms.author: genli
ms.openlocfilehash: dc09f29fec78d408e1560bfa0a943f16ab50c760
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/04/2018
---
# <a name="add-or-change-azure-subscription-administrators"></a>Adicionar ou alterar os administradores da subscrição do Azure

Os administradores da subscrição clássico do Azure e Azure [controlo de acesso baseado em funções (RBAC)](../active-directory/role-based-access-control-what-is.md) são dois sistemas para gerir o acesso aos recursos do Azure:

* As funções de administrador de subscrição clássica oferecem gestão de acesso básico e incluem a conta de administrador, administrador de serviços e Coadministradores.
    * Quando se inscreve para uma nova subscrição do Azure, a sua conta está definido como o administrador de conta e o administrador de serviços por predefinição.
    * Coadministradores podem ser adicionadas após o início de sessão de cópia de segurança.
* RBAC é um sistema mais recente que oferece a gestão de acesso detalhada com várias funções incorporadas, flexibilidade de âmbito e funções personalizadas.
    * No entanto, os utilizadores apenas com funções do RBAC e não existem funções de administrador clássico da subscrição não é possível gerir implementações clássicas do Azure.

Para garantir a melhorar o controlo e para simplificar a gestão de acesso, recomendamos que utilize o RBAC para todas as necessidades de gestão de acesso. Se for possível, recomendamos que reconfigurar a utilizar o RBAC de políticas de acesso existentes. 

<a name="add-an-admin-for-a-subscription"></a>

## <a name="add-an-rbac-owner-admin-for-a-subscription-in-azure-portal"></a>Adicionar um administrador de proprietário do RBAC para uma subscrição no portal do Azure 

Para adicionar um utilizador como um administrador para a administração do serviço de subscrição do Azure, conceda-lhes uma função de proprietário do RBAC para a subscrição. A função de proprietário pode gerir os recursos na subscrição que atribuídos e não tem privilégios de acesso a outras subscrições.

1. Visite [ **subscrições** no portal do Azure](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
2. Selecione a subscrição que pretende conceder acesso.
3. Selecione **(IAM) do controlo de acesso** no menu.
4. No **função** caixa, selecione **proprietário**. 
5. No **atribuir acesso** caixa, selecione **utilizador do Azure AD, grupo ou aplicação**. 
6. No **selecione** caixa, escreva o endereço de e-mail do utilizador que pretende adicionar como proprietário. Selecione o utilizador e, em seguida, selecione **guardar**.

    ![Captura de ecrã que mostra a função de proprietário selecionada](./media/billing-add-change-azure-subscription-administrator/add-role.png)

Isto proporciona ao utilizador de acesso completo a todos os recursos, incluindo o direito para delegar o acesso a outras pessoas. Para conceder acesso a um âmbito diferente, como um grupo de recursos, visite o menu IAM para esse âmbito. 

## <a name="add-or-change-co-administrator"></a>Adicionar ou alterar o coadministrador

Apenas um proprietário pode ser adicionado como coadministrador. Não não possível adicionar outros utilizadores com funções, tais como o leitor e contribuinte como coadministradores.

> [!TIP]
> Só tem de adicionar a conta de "Proprietário" como coadministrador, se o utilizador precisar gerir implementações clássicas do Azure. Recomendamos que utilize o RBAC para todos os outros fins.

1. Se ainda não o fez, adicione alguém como um proprietário seguir instruções de acima.
2. **Clique com botão direito** o utilizador proprietário que acabou de adicionar e, em seguida, selecione **adicionar como coadministrador**. Se não vir o **adicionar como coadministrador** opção, atualize a página ou tente outro browser da Internet. 

    ![Captura de ecrã que adiciona o coadministrador](./media/billing-add-change-azure-subscription-administrator/add-coadmin.png)

    Para remover a permissão de coadministrador, **com o botão direito** o utilizador de "Coadministrador" e, em seguida, selecione **remover coadministrador**.

    ![Captura de ecrã que remove o coadministrador](./media/billing-add-change-azure-subscription-administrator/remove-coadmin.png)

<a name="change-service-administrator-for-a-subscription"></a>

## <a name="change-the-service-administrator-for-an-azure-subscription"></a>Alterar o administrador de serviço para uma subscrição do Azure

Apenas o administrador da conta pode alterar o administrador de serviço para uma subscrição. Por predefinição, quando se inscreve, o administrador de serviço é o mesmo que o administrador da conta. Se o administrador de serviço é alterado para um utilizador diferente, o administrador da conta perder o acesso ao portal do Azure. No entanto, o administrador da conta, pode utilizar Centro de contas sempre para alterar o administrador de serviço para si próprios.

1. Certifique-se de que o seu cenário é suportado ao verificar o [limites para alterar os administradores de serviço](#limits).
1. Inicie sessão no [Centro de contas](https://account.windowsazure.com/subscriptions) como o administrador da conta.
1. Selecione uma subscrição.
1. No lado direito, selecione **editar detalhes da subscrição**.

    ![Captura de ecrã que mostra o botão de subscrição de edição no Centro de contas](./media/billing-add-change-azure-subscription-administrator/editsub.png)
1. No **administrador de serviço** box, introduza o endereço de e-mail do administrador de serviço novo.

    ![Captura de ecrã que mostra a caixa para alterar o e-mail do Admin de serviço](./media/billing-add-change-azure-subscription-administrator/changeSA.png)

<a name="limits"></a>

### <a name="limitations-for-changing-service-administrators"></a>Limitações para alterar os administradores de serviço

* Cada subscrição está associada um diretório do Azure AD. Para localizar o diretório a subscrição está associada, aceda a [ **subscrições**](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade), em seguida, selecione uma subscrição para ver o diretório.
* Se tem sessão iniciada com uma conta escolar ou profissional, pode adicionar outras contas na sua organização como administrador de serviço. Por exemplo, abby@contoso.com pode adicionar bob@contoso.com como administrador de serviço, mas não é possível adicionar john@notcontoso.com , a menos que john@notcontoso.com tem presença no diretório contoso.com. Utilizadores com sessão iniciada sessão em contas escolar ou profissional podem continuar a adicionar utilizadores de Account Microsoft como o administrador de serviço.

  | Método de início de sessão | Adicionar utilizador Account Microsoft como SA? | Adicionar conta escolar ou profissional na mesma organização como SA? | Adicionar conta escolar ou profissional numa organização diferente como SA? |
  | --- | --- | --- | --- |
  |  Conta Microsoft |Sim |Não |Não |
  |  Conta escolar ou profissional |Sim |Sim |Não |

## <a name="change-the-account-administrator-for-an-azure-subscription"></a>Alterar o administrador da conta para uma subscrição do Azure

O administrador de conta é o utilizador que inicialmente inscreveu da subscrição do Azure e é responsável como o proprietário de faturação da subscrição. Para alterar o administrador da conta de uma subscrição, consulte [transferir a propriedade de uma subscrição do Azure para outra conta](billing-subscription-transfer.md).

<a name="check-the-account-administrator-of-the-subscription"></a>

**Não tem a certeza que é o administrador da conta?** Siga estes passos.

1. Visite [ **subscrições** no portal do Azure](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
1. Selecione a subscrição que pretende verificar e, em seguida, procure em **definições**.
1. Selecione **propriedades**. O administrador da conta da subscrição é apresentado no **administrador da conta** caixa.  

## <a name="types-of-classic-subscription-admins"></a>Tipos de administradores da subscrição clássica

 Conta de administrador, administrador de serviço e coadministrador são os três tipos de funções de administrador de subscrição clássica no Azure. A conta que é utilizada para se inscrever no Azure é automaticamente definida como o administrador de conta e o administrador de serviço. Em seguida, podem ser adicionados Coadministradores adicionais. A tabela seguinte descreve as diferenças exatas entre estas três funções administrativas. 

> [!TIP]
> Para melhorar o controlo e gestão de acesso detalhado, recomendamos que utilize baseada em funções do Azure controlo de acesso (RBAC), que permite aos utilizadores a adicionar a várias funções. Para obter mais informações, consulte [controlo de acesso baseado em função do Active Directory do Azure](../active-directory/role-based-access-control-what-is.md).

| Administrador de subscrição clássica | Limite | Descrição |
| --- | --- | --- |
| Administrador de conta (AA) |1 por conta do Azure |Este é o utilizador que inscreveu da subscrição do Azure e está autorizado a aceder a [Centro de contas](https://account.azure.com/Subscriptions) e realizar várias tarefas de gestão. Estes incluem a capacidade de criar novas subscrições, cancelar subscrições, altere a faturação de uma subscrição e alterar o administrador de serviço. Concecionais, o administrador de conta é o proprietário de faturação da subscrição. No RBAC, o administrador da conta não está atribuído uma função.|
| Administrador de serviço (SA) |1 por subscrição do Azure |Esta função está autorizada a gerir serviços no [portal do Azure](https://portal.azure.com). Por predefinição, para uma nova subscrição, o administrador da conta também é o administrador de serviço. No RBAC, a função de proprietário é atribuída para o administrador de serviço no âmbito de subscrição.|
| Coadministrador (AC) |200 por subscrição |Esta função tem os mesmos privilégios de acesso do Administrador de Serviços, mas não pode alterar a associação de subscrições a diretórios do Azure. No RBAC, a função de proprietário é atribuída o Coadministrador no âmbito de subscrição.|

## <a name="learn-more-about-resource-access-control-and-active-directory"></a>Saiba mais sobre o controlo de acesso de recursos e do Active Directory

* Para obter mais informações sobre como o acesso a recursos é controlado no Microsoft Azure, consulte o artigo [compreender o acesso a recursos no Azure](../active-directory/active-directory-understanding-resource-access.md).
* Para mais informações sobre o Azure Active Directory, consulte [subscrições do Azure como estão associadas ao Azure Active Directory](../active-directory/active-directory-how-subscriptions-associated-directory.md) e [atribuir funções de administrador no Azure Active Directory](../active-directory/active-directory-assign-admin-roles-azure-portal.md).

## <a name="need-help-contact-support"></a>Precisa de ajuda? Contacte o suporte.

Se ainda precisar de ajuda, [contacte o suporte](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para obter o seu problema resolvido rapidamente.
