---
title: 'Tutorial: Configurar Pingboard para aprovisionamento de utilizadores automática no Azure Active Directory | Microsoft Docs'
description: Saiba como configurar o Azure Active Directory para aprovisionar e anular o aprovisionamento contas de utilizador para Pingboard automaticamente.
services: active-directory
documentationcenter: ''
author: asmalser-msft
writer: asmalser-msft
manager: sakula
ms.assetid: 0b38ee73-168b-42cb-bd8b-9c5e5126d648
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/19/2017
ms.author: asmalser
ms.reviewer: asmalser
ms.openlocfilehash: 4d483f045a8e778953ddb578dd7bb81fdb05fdf2
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35292413"
---
# <a name="tutorial-configure-pingboard-for-automatic-user-provisioning"></a>Tutorial: Configurar Pingboard para aprovisionamento de utilizadores automática

O objetivo deste tutorial é mostrar-lhe os passos que necessários para efetuar para permitir o aprovisionamento e anular o aprovisionamento de contas de utilizador do Azure Active Directory (Azure AD) para Pingboard automática.

## <a name="prerequisites"></a>Pré-requisitos

O cenário descrito neste tutorial assume que já tem os seguintes itens:

*   Um inquilino do Azure AD
*   Um inquilino Pingboard [conta Pro](https://pingboard.com/pricing) 
*   Uma conta de utilizador no Pingboard com permissões de administrador 

> [!NOTE] 
> Depende do Azure AD aprovisionamento integração o [Pingboard API](`https://your_domain.pingboard.com/scim/v2`), que está disponível para a sua conta.

## <a name="assign-users-to-pingboard"></a>Atribuir utilizadores a Pingboard

AD do Azure utiliza um conceito chamado "atribuições de" para determinar quais os utilizadores devem receber acesso às aplicações selecionadas. No contexto de aprovisionamento da conta de utilizador automáticas, apenas os utilizadores atribuídos a uma aplicação no Azure AD estão sincronizados. 

Antes de configurar e ativar o serviço de aprovisionamento, tem de decidir quais os utilizadores no Azure AD tem acesso à sua aplicação Pingboard. Em seguida, pode atribuir estes utilizadores à sua aplicação Pingboard ao seguir as instruções aqui:

[Atribuir um utilizador a uma aplicação empresarial](manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-pingboard"></a>Sugestões importantes para atribuir utilizadores a Pingboard

Recomendamos que atribua um único utilizador do Azure AD para Pingboard para testar a configuração de aprovisionamento. Os utilizadores adicionais podem ser atribuídos mais tarde.

## <a name="configure-user-provisioning-to-pingboard"></a>Configurar o aprovisionamento de utilizadores para Pingboard 

Esta secção orienta-o ao ligar o seu Azure AD para a conta de utilizador Pingboard aprovisionamento API. Também é configurar o serviço de aprovisionamento para criar, atualizar e desativar as contas de utilizador atribuída Pingboard com base em atribuições de utilizador no Azure AD.

> [!TIP]
> Para ativar baseados em SAML-início de sessão único para Pingboard, siga as instruções fornecidas a [portal do Azure](https://portal.azure.com). O início de sessão único a pode ser configurado independentemente aprovisionamento automático, embora estas duas funcionalidades complementam entre si.

### <a name="to-configure-automatic-user-account-provisioning-to-pingboard-in-azure-ad"></a>Para configurar o utilizador automáticas aprovisionamento da conta para Pingboard no Azure AD

1. No [portal do Azure](https://portal.azure.com), navegue para o **do Azure Active Directory** > **aplicações da empresa** > **todas as aplicações** secção.

2. Se já tiver configurado Pingboard para o início de sessão único, procure a instância do Pingboard utilizando o campo de pesquisa. Caso contrário, selecione **adicionar** e procure **Pingboard** na Galeria de aplicações. Selecione **Pingboard** nos resultados de pesquisa e adicione-o à sua lista de aplicações.

3. Selecione a sua instância do Pingboard e, em seguida, selecione o **aprovisionamento** separador.

4. Definir **modo de aprovisionamento** para **automática**.

    ![Aprovisionamento de Pingboard](./media/active-directory-saas-pingboard-provisioning-tutorial/pingboardazureprovisioning.png)
    
5. Sob o **credenciais de administrador** secção, execute os seguintes passos:

    a. No **URL de inquilino**, introduza `https://your_domain.pingboard.com/scim/v2`e substitua "your_domain" com o seu domínio real.

    b. Inicie sessão no [Pingboard](https://pingboard.com/) utilizando a sua conta de administrador.

    c. Selecione **suplementos** > **integrações** > **do Azure Active Directory**.

    d. Vá para o **configurar** separador e selecione **ativar o aprovisionamento de utilizadores do Azure**.

    e. Copie o token no **tokens de portador do OAuth**e introduza-o no **segredo Token**.

6. No portal do Azure, selecione **Testar ligação** para garantir que o Azure AD pode ligar à sua aplicação Pingboard. Se a ligação falhar, certifique-se de que a conta de Pingboard tem permissões de administrador e tente a **Testar ligação** passo novamente.

7. Introduza o endereço de e-mail de uma pessoa ou grupo que pretende receber notificações de erro de aprovisionamento no **correio eletrónico de notificação**. Selecione a caixa de verificação por baixo.

8. Selecione **Guardar**. 

9. Sob o **mapeamentos** secção, selecione **sincronizar utilizadores do Azure Active Directory para Pingboard**.

10. No **mapeamentos de atributos** secção, reveja os atributos de utilizador a ser sincronizadas a partir do Azure AD para Pingboard. Os atributos selecionados como **correspondência** propriedades são utilizadas para corresponder as contas de utilizador no Pingboard para operações de atualização. Selecione **guardar** para consolidar as alterações. Para obter mais informações, consulte [mapeamentos de atributos de aprovisionamento de utilizadores de personalizar](active-directory-saas-customizing-attribute-mappings.md).

11. Para ativar o Azure AD aprovisionamento de serviço para Pingboard, no **definições** secção, altere **estado de aprovisionamento** para **no**.

12. Selecione **guardar** para iniciar a sincronização inicial de utilizadores atribuídos a Pingboard.

A sincronização inicial demora mais para efetuar a sincronizações subsequentes, o que ocorrer aproximadamente a cada 40 minutos, desde que o serviço está em execução. Utilize o **detalhes de sincronização** secção para monitorizar o progresso e siga as ligações para os registos de atividade de aprovisionamento. Os registos descrevem todas as ações efetuadas pelo serviço de aprovisionamento na sua aplicação Pingboard.

Para obter mais informações sobre como ler o Azure AD, os registos de aprovisionamento, consulte [relatórios sobre o aprovisionamento da conta de utilizador automáticas](active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Gerir o aprovisionamento da conta de utilizador para aplicações da empresa](manage-apps/configure-automatic-user-provisioning-portal.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](manage-apps/what-is-single-sign-on.md)
* [Configurar o início de sessão único](active-directory-saas-pingboard-tutorial.md)
