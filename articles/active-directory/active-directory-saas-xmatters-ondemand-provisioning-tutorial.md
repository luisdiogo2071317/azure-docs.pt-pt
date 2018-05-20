---
title: 'Tutorial: Configurar xMatters OnDemand para o aprovisionamento de utilizador automáticas com o Azure Active Directory | Microsoft Docs'
description: Saiba como configurar o Azure Active Directory para aprovisionar e anular o aprovisionamento contas de utilizador para xMatters OnDemand automaticamente.
services: active-directory
documentationcenter: ''
author: zhchia
writer: zhchia
manager: beatrizd-msft
ms.assetid: na
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2018
ms.author: v-ant-msft
ms.openlocfilehash: e0b945a99766ee52cb357f54d7135fbbdf1fada2
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/20/2018
---
# <a name="tutorial-configure-xmatters-ondemand-for-automatic-user-provisioning"></a>Tutorial: Configurar xMatters OnDemand para o aprovisionamento de utilizador automáticas

O objetivo deste tutorial é demonstrar os passos para ser efetuada no xMatters OnDemand e o Azure Active Directory (Azure AD) para configurar o Azure AD para aprovisionar automaticamente e anular o aprovisionamento utilizadores e/ou grupos para xMatters OnDemand.

> [!NOTE]
> Este tutorial descreve um conector desenvolvido com o serviço de fornecimento de utilizador do Azure AD. Para obter detalhes importantes sobre o que faz este serviço, como funciona e perguntas mais frequentes, consulte [automatizar utilizador aprovisionamento e desaprovisionamento para aplicações de SaaS no Azure Active Directory](./active-directory-saas-app-provisioning.md).

## <a name="prerequisites"></a>Pré-requisitos

O cenário descrito neste tutorial assume que já tem o seguinte:

*   Um inquilino do Azure AD
*   Um inquilino de OnDemand xMatters com o [Starter](https://www.xmatters.com/pricing) planear ou melhor ativado 
*   Uma conta de utilizador no xMatters OnDemand com permissões de administrador

> [!NOTE]
> O Azure AD aprovisionamento integração depende o [xMatters OnDemand API](https://help.xmatters.com/xmAPI), que está disponível para xMatters OnDemand equipas no plano de arranque ou um melhor.

## <a name="adding-xmatters-ondemand-from-the-gallery"></a>Adicionar xMatters OnDemand a partir da Galeria

Antes de configurar xMatters OnDemand para o utilizador automática de aprovisionamento com o Azure AD, tem de adicionar xMatters OnDemand da Galeria de aplicações do Azure AD à sua lista de aplicações de SaaS geridas.

**Para adicionar xMatters OnDemand da Galeria de aplicações do Azure AD, execute os seguintes passos:**

1. No  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em de **do Azure Active Directory** ícone.

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais** > **todas as aplicações**.

    ![As secção de aplicações da empresa][2]
    
3. Para adicionar xMatters OnDemand, clique o **nova aplicação** botão na parte superior da caixa de diálogo.

    ![O novo botão de aplicação][3]

4. Na caixa de pesquisa, escreva **xMatters OnDemand**.

    ![xMatters OnDemand aprovisionamento](./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/AppSearch.png)

5. No painel de resultados, selecione **xMatters OnDemand**e, em seguida, clique em de **adicionar** botão Adicionar xMatters OnDemand à sua lista de aplicações SaaS.

    ![xMatters OnDemand aprovisionamento](./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/AppSearchResults.png)

    ![xMatters OnDemand aprovisionamento](./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/AppCreation.png)

## <a name="assigning-users-to-xmatters-ondemand"></a>Atribuir utilizadores a xMatters OnDemand

Azure Active Directory utiliza um conceito chamado "atribuições de" para determinar quais os utilizadores devem receber acesso às aplicações selecionadas. No contexto de aprovisionamento de utilizadores automático, são sincronizados apenas a utilizadores e/ou grupos que tenham sido "atribuídos" a uma aplicação no Azure AD.

Antes de configurar e ativar o aprovisionamento de utilizadores automática, deve decidir quais os utilizadores e/ou grupos no Azure AD necessitam de aceder a xMatters OnDemand. Depois de decidir, pode atribuir estes utilizadores e/ou grupos a xMatters OnDemand ao seguir as instruções aqui:

*   [Atribuir um utilizador ou grupo a uma aplicação empresarial](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-xmatters-ondemand"></a>Sugestões importantes para atribuir utilizadores a xMatters OnDemand

*   Recomenda-se que um único utilizador do Azure AD é atribuído a xMatters OnDemand para testar a configuração de aprovisionamento de utilizadores automática. Os utilizadores adicionais e/ou grupos podem ser atribuídos mais tarde.

*   Ao atribuir um utilizador a xMatters OnDemand, tem de selecionar uma função de específicas da aplicação válida (se disponível) na caixa de diálogo atribuição. Os utilizadores com o **predefinido acesso** função foram excluídos de aprovisionamento.

## <a name="configuring-automatic-user-provisioning-to-xmatters-ondemand"></a>Configurar o aprovisionamento de utilizadores automática para xMatters OnDemand 

Esta secção descreve os passos para configurar o Azure AD aprovisionamento do serviço para criar, atualizar e desative os utilizadores e/ou grupos no xMatters que ondemand com base no utilizador e/ou atribuições de grupo no Azure AD.

> [!TIP]
> Também pode optar por ativar baseados em SAML-início de sessão único para xMatters OnDemand, seguindo as instruções fornecidas a [xMatters OnDemand-início de sessão único tutorial](active-directory-saas-xmatters-ondemand-tutorial.md). O início de sessão único a pode ser configurado independentemente do aprovisionamento de utilizadores automática, apesar destas duas funcionalidades complementar dos entre si.

### <a name="to-configure-automatic-user-provisioning-for-xmatters-ondemand-in-azure-ad"></a>Para configurar o aprovisionamento de utilizadores automática para xMatters OnDemand no Azure AD:

1. Iniciar sessão para o [portal do Azure](https://portal.azure.com) e navegue para **do Azure Active Directory > aplicações da empresa > todas as aplicações**.

2. Selecione xMatters OnDemand na sua lista de aplicações SaaS.
 
    ![xMatters OnDemand aprovisionamento](./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/ApplicationInstanceSearch.png)

3. Selecione o **aprovisionamento** separador.
    
    ![xMatters OnDemand aprovisionamento](./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/ProvisioningTab.png)

4. Definir o **modo de aprovisionamento** para **automática**.

    ![xMatters OnDemand aprovisionamento](./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/ProvisioningCredentials.png)

5. Sob o **credenciais de administrador** secção, de entrada a **nome de utilizador de Admin**, **palavra-passe de administrador**, e **domínio** do seu xMatters OnDemand conta.

    *   No **nome de utilizador de Admin** campo, preencher o nome de utilizador da conta de administrador no seu inquilino de OnDemand xMatters. Exemplo: admin@contoso.com.

    *   No **palavra-passe de administrador** campo, preencher a palavra-passe correspondente ao nome de utilizador administrador.

    *   No **domínio** campo, preencher o subdomínio do seu inquilino de OnDemand xMatters.
    Exemplo: para uma conta com um URL de inquilino de https://my-tenant.xmatters.com, seria o subdomínio **meu inquilino**.

6. Após preencher os campos mostrados no passo 5, clique em **Testar ligação** para garantir que o Azure AD pode ligar ao xMatters OnDemand. Se a ligação falhar, certifique-se de que o xMatters OnDemand conta tem permissões de administrador e tente novamente.

    ![xMatters OnDemand aprovisionamento](./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/TestConnection.png)
    
7. No **correio eletrónico de notificação** campo, introduza o endereço de e-mail de uma pessoa ou grupo que deve receber as notificações de erro de aprovisionamento e marque a caixa de verificação - **enviar uma notificação por e-mail quando uma falha ocorre**.

    ![xMatters OnDemand aprovisionamento](./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/EmailNotification.png)

8. Clique em **Guardar**.

9. Sob o **mapeamentos** secção, selecione **sincronizar utilizadores do Azure Active Directory para xMatters OnDemand**.

    ![xMatters OnDemand aprovisionamento](./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/SyncUsers.png)

10. Reveja os atributos de utilizador que são sincronizados a partir do Azure AD e xMatters OnDemand no **atributo mapeamento** secção. Os atributos selecionados como **correspondência** propriedades são utilizadas para corresponder as contas de utilizador no xMatters OnDemand para operações de atualização. Selecione o **guardar** botão para consolidar as alterações.

    ![xMatters OnDemand aprovisionamento](./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/UserAttributes.png)

11. Sob o **mapeamentos** secção, selecione **sincronizar Azure grupos do Active Directory para xMatters OnDemand**.

    ![xMatters OnDemand aprovisionamento](./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/SyncGroups.png)

12. Reveja os atributos de grupo que são sincronizados a partir do Azure AD e xMatters OnDemand no **atributo mapeamento** secção. Os atributos selecionados como **correspondência** propriedades são utilizadas para fazer corresponder os grupos no xMatters OnDemand para operações de atualização. Selecione o **guardar** botão para consolidar as alterações.

    ![xMatters OnDemand aprovisionamento](./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/GroupAttributes.png)

13. Para configurar filtros de âmbito, consulte as seguintes instruções fornecidas a [tutorial de filtro Scoping](./active-directory-saas-scoping-filters.md).

14. Para ativar o Azure AD aprovisionamento do serviço para xMatters OnDemand, altere o **estado de aprovisionamento** para **no** no **definições** secção.

    ![xMatters OnDemand aprovisionamento](./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/ProvisioningStatus.png)

15. Definir utilizadores e/ou grupos que pretende que o aprovisionamento xMatters OnDemand ao escolher os valores pretendidos no **âmbito** no **definições** secção.

    ![xMatters OnDemand aprovisionamento](./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/ScopeSelection.png)

16. Quando estiver pronto para aprovisionar, clique em **guardar**.

    ![xMatters OnDemand aprovisionamento](./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/SaveProvisioning.png)

Esta operação inicia a sincronização inicial de todos os utilizadores e/ou grupos definidos no **âmbito** no **definições** secção. A sincronização inicial demora mais para efetuar a sincronizações subsequentes, o que ocorrer aproximadamente a cada 40 minutos, desde que o Azure AD aprovisionamento do serviço está em execução. Pode utilizar o **detalhes de sincronização** secção para monitorizar o progresso e siga as ligações de aprovisionamento do relatório de atividade, que descreve todas as ações efetuadas pelo Azure AD aprovisionamento do serviço no xMatters OnDemand.

Para obter mais informações sobre como ler o Azure AD, os registos de aprovisionamento, consulte [relatórios sobre o aprovisionamento da conta de utilizador automáticas](./active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Gerir o aprovisionamento da conta de utilizador para aplicações da empresa](active-directory-enterprise-apps-manage-provisioning.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Passos Seguintes

* [Saiba como rever os registos e obter relatórios sobre o aprovisionamento de atividade](active-directory-saas-provisioning-reporting.md)

<!--Image references-->
[1]: ./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-xmatters-ondemand-provisioning-tutorial/tutorial_general_03.png
