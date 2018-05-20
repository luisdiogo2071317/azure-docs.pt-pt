---
title: 'Tutorial: Configurar Clarizen para aprovisionamento de utilizadores automática no Azure Active Directory | Microsoft Docs'
description: Saiba como configurar o Azure Active Directory para aprovisionar e anular o aprovisionamento contas de utilizador para Clarizen automaticamente.
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
ms.date: 01/31/2018
ms.author: v-ant-msft
ms.openlocfilehash: 779fa09781b5ea6e13450d609703bec0c08e38e4
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/20/2018
---
# <a name="tutorial-configure-clarizen-for-automatic-user-provisioning"></a>Tutorial: Configurar Clarizen para aprovisionamento de utilizadores automática

O objetivo deste tutorial é demonstrar os passos para ser efetuada no Clarizen e Azure Active Directory (Azure AD) para configurar o Azure AD para aprovisionar automaticamente e anular o aprovisionamento utilizadores e/ou grupos para Clarizen.

> [!NOTE]
> Este tutorial descreve um conector desenvolvido com o serviço de fornecimento de utilizador do Azure AD. Para obter detalhes importantes sobre o que faz este serviço, como funciona e perguntas mais frequentes, consulte [automatizar utilizador aprovisionamento e desaprovisionamento para aplicações de SaaS no Azure Active Directory](./active-directory-saas-app-provisioning.md).

## <a name="prerequisites"></a>Pré-requisitos

O cenário descrito neste tutorial assume que já tem o seguinte:

*   Um inquilino do Azure AD
*   Um inquilino Clarizen com o [Enterprise Edition](https://www.clarizen.com/product/pricing/) planear ou melhor ativado 
*   Uma conta de utilizador no Clarizen com permissões de administrador

> [!NOTE]
> O Azure AD aprovisionamento integração depende o [Clarizen API](https://api.clarizen.com/v2.0/services/), que está disponível para equipas Clarizen no plano de edição Enterprise ou melhor.

## <a name="adding-clarizen-from-the-gallery"></a>Adicionar Clarizen a partir da Galeria
Antes de configurar Clarizen para aprovisionamento com o Azure AD de utilizadores automática, tem de adicionar Clarizen da Galeria de aplicações do Azure AD à sua lista de aplicações de SaaS geridas.

**Para adicionar Clarizen da Galeria de aplicações do Azure AD, execute os seguintes passos:**

1. No  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em de **do Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais** > **todas as aplicações**.

    ![As secção de aplicações da empresa][2]
    
3. Para adicionar Clarizen, clique o **nova aplicação** botão na parte superior da caixa de diálogo.

    ![O novo botão de aplicação][3]

4. Na caixa de pesquisa, escreva **Clarizen**.

    ![Aprovisionamento de Clarizen](./media/active-directory-saas-clarizen-provisioning-tutorial/AppSearch.png)

5. No painel de resultados, selecione **Clarizen**e, em seguida, clique em de **adicionar** botão Adicionar Clarizen à sua lista de aplicações SaaS.

    ![Aprovisionamento de Clarizen](./media/active-directory-saas-clarizen-provisioning-tutorial/AppSearchResults.png)

    ![Aprovisionamento de Clarizen](./media/active-directory-saas-clarizen-provisioning-tutorial/AppCreation.png)

## <a name="assigning-users-to-clarizen"></a>Atribuir utilizadores a Clarizen

Azure Active Directory utiliza um conceito chamado "atribuições de" para determinar quais os utilizadores devem receber acesso às aplicações selecionadas. No contexto de aprovisionamento de utilizadores automático, são sincronizados apenas a utilizadores e/ou grupos que tenham sido "atribuídos" a uma aplicação no Azure AD. 

Antes de configurar e ativar o aprovisionamento de utilizadores automática, deve decidir quais os utilizadores e/ou grupos no Azure AD necessitam de aceder a Clarizen. Depois de decidir, pode atribuir estes utilizadores e/ou grupos a Clarizen ao seguir as instruções aqui:

*   [Atribuir um utilizador ou grupo a uma aplicação empresarial](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-clarizen"></a>Sugestões importantes para atribuir utilizadores a Clarizen

*   Recomenda-se que um único utilizador do Azure AD é atribuído a Clarizen para testar a configuração de aprovisionamento de utilizadores automática. Os utilizadores adicionais e/ou grupos podem ser atribuídos mais tarde.

*   Ao atribuir um utilizador a Clarizen, tem de selecionar uma função de específicas da aplicação válida (se disponível) na caixa de diálogo atribuição. Os utilizadores com o **predefinido acesso** função foram excluídos de aprovisionamento.

## <a name="configuring-automatic-user-provisioning-to-clarizen"></a>Configurar o aprovisionamento de utilizadores automática para Clarizen 

Esta secção descreve os passos para configurar o Azure AD aprovisionamento do serviço para criar, atualizar e desativar os utilizadores e/ou grupos no Clarizen com base em atribuições de utilizador e/ou grupo no Azure AD.

> [!TIP]
> Também pode optar por ativar baseados em SAML-início de sessão único para Clarizen, seguindo as instruções fornecidas a [Clarizen único início de sessão tutorial](active-directory-saas-clarizen-tutorial.md). O início de sessão único a pode ser configurado independentemente do aprovisionamento de utilizadores automática, apesar destas duas funcionalidades complementar dos entre si.

### <a name="to-configure-automatic-user-provisioning-for-clarizen-in-azure-ad"></a>Para configurar o aprovisionamento de utilizadores automática para Clarizen no Azure AD:

1. Iniciar sessão para o [portal do Azure](https://portal.azure.com) e navegue para **do Azure Active Directory > aplicações da empresa > todas as aplicações**.

2. Selecione Clarizen na sua lista de aplicações SaaS.
 
    ![Aprovisionamento de Clarizen](./media/active-directory-saas-clarizen-provisioning-tutorial/AppInstanceSearch.png)

3. Selecione o **aprovisionamento** separador.
    
    ![Aprovisionamento de Clarizen](./media/active-directory-saas-clarizen-provisioning-tutorial/ProvisioningTab.png)

4. Definir o **modo de aprovisionamento** para **automática**.

    ![Aprovisionamento de Clarizen](./media/active-directory-saas-clarizen-provisioning-tutorial/ProvisioningCredentials.png)

5. Sob o **credenciais de administrador** secção, de entrada a **domínio**, **nome de utilizador de Admin**, e **palavra-passe de administrador** da conta da sua Clarizen. Exemplos destes valores são:

    *   No **nome de utilizador de Admin** campo, preencher o nome de utilizador da conta de administrador no seu inquilino Clarizen. Exemplo: admin@contoso.com.

    *   No **palavra-passe de administrador** campo, preencher a palavra-passe da conta de administrador correspondente ao nome de utilizador administrador.

    *   No **domínio** campo, preencher subdomínio com base no passo 6.
    
6. Obter o **serverLocation** para a sua conta Clarizen com base nos passos mencionados em **autenticação** de [guia de API de Rest do Clarizen](https://success.clarizen.com/hc/en-us/articles/205711828-REST-API-Guide-Version-2). Após obter o serverLocation, utilize o subdomínio do URL conforme realçado abaixo, para preencher o **domínio** campo.

    ![Aprovisionamento de Clarizen](./media/active-directory-saas-clarizen-provisioning-tutorial/ClarizenDomain.png)

7. Após preencher os campos mostrados no passo 5, clique em **Testar ligação** para garantir que o Azure AD pode ligar ao Clarizen. Se a ligação falhar, certifique-se de que a conta de Clarizen possui permissões de administrador e tente novamente.

    ![Aprovisionamento de Clarizen](./media/active-directory-saas-clarizen-provisioning-tutorial/TestConnection.png)
    
8. No **correio eletrónico de notificação** campo, introduza o endereço de e-mail de uma pessoa ou grupo que deve receber as notificações de erro de aprovisionamento e marque a caixa de verificação **enviar uma notificação por e-mail quando uma falha ocorre**.

    ![Aprovisionamento de Clarizen](./media/active-directory-saas-clarizen-provisioning-tutorial/NotificationEmail.png)

9. Clique em **Guardar**.

10. Sob o **mapeamentos** secção, selecione **sincronizar utilizadores do Azure Active Directory para Clarizen**.

    ![Aprovisionamento de Clarizen](./media/active-directory-saas-clarizen-provisioning-tutorial/UserMapping.png)

11. Reveja os atributos de utilizador que são sincronizados a partir do Azure AD e Clarizen no **atributo mapeamento** secção. Os atributos selecionados como **correspondência** propriedades são utilizadas para corresponder as contas de utilizador no Clarizen para operações de atualização. Selecione o **guardar** botão para consolidar as alterações.

    ![Aprovisionamento de Clarizen](./media/active-directory-saas-clarizen-provisioning-tutorial/UserMappingAttributes.png)

12. Sob o **mapeamentos** secção, selecione **sincronizar Azure grupos do Active Directory para Clarizen**.

    ![Aprovisionamento de Clarizen](./media/active-directory-saas-clarizen-provisioning-tutorial/GroupMapping.png)

13. Reveja os atributos de grupo que são sincronizados a partir do Azure AD e Clarizen no **atributo mapeamento** secção. Os atributos selecionados como **correspondência** propriedades são utilizadas para correspondam aos grupos de Clarizen para operações de atualização. Selecione o **guardar** botão para consolidar as alterações.

    ![Aprovisionamento de Clarizen](./media/active-directory-saas-clarizen-provisioning-tutorial/GroupMappingAttributes.png)

14. Para configurar filtros de âmbito, consulte as seguintes instruções fornecidas a [tutorial de filtro Scoping](./active-directory-saas-scoping-filters.md).

15. Para ativar o Azure AD aprovisionamento do serviço para Clarizen, altere o **estado de aprovisionamento** para **no** no **definições** secção.

    ![Aprovisionamento de Clarizen](./media/active-directory-saas-clarizen-provisioning-tutorial/ProvisioningStatus.png)

16. Definir utilizadores e/ou grupos que pretende que o aprovisionamento Clarizen ao escolher os valores pretendidos no **âmbito** no **definições** secção.

    ![Aprovisionamento de Clarizen](./media/active-directory-saas-clarizen-provisioning-tutorial/ScopeSelection.png)

17. Quando estiver pronto para aprovisionar, clique em **guardar**.

    ![Aprovisionamento de Clarizen](./media/active-directory-saas-clarizen-provisioning-tutorial/SaveProvisioning.png)


Esta operação inicia a sincronização inicial de todos os utilizadores e/ou grupos definidos no **âmbito** no **definições** secção. A sincronização inicial demora mais para efetuar a sincronizações subsequentes, o que ocorrer aproximadamente a cada 40 minutos, desde que o Azure AD aprovisionamento do serviço está em execução. Pode utilizar o **detalhes de sincronização** secção para monitorizar o progresso e siga as ligações para o relatório de atividade, que descreve todas as ações efetuadas pelo Azure AD aprovisionamento do serviço no Clarizen de aprovisionamento.

Para obter mais informações sobre como ler o Azure AD, os registos de aprovisionamento, consulte [relatórios sobre o aprovisionamento da conta de utilizador automáticas](./active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Gerir o aprovisionamento da conta de utilizador para aplicações da empresa](active-directory-enterprise-apps-manage-provisioning.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Passos Seguintes

* [Saiba como rever os registos e obter relatórios sobre o aprovisionamento de atividade](active-directory-saas-provisioning-reporting.md)

<!--Image references-->
[1]: ./media/active-directory-saas-clarizen-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_03.png
