---
title: "Tutorial: Configurar Samanage para aprovisionamento de utilizadores automática no Azure Active Directory | Microsoft Docs"
description: Saiba como configurar o Azure Active Directory para aprovisionar e anular o aprovisionamento contas de utilizador para Samanage automaticamente.
services: active-directory
documentationcenter: 
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
ms.author: v-ant
ms.openlocfilehash: 8f11beff2c78386f4c3e8130c8e5d72465b8fe87
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/16/2018
---
# <a name="tutorial-configure-samanage-for-automatic-user-provisioning"></a>Tutorial: Configurar Samanage para aprovisionamento de utilizadores automática

O objetivo deste tutorial é demonstrar os passos para ser efetuada no Samanage e Azure Active Directory (Azure AD) para configurar o Azure AD para aprovisionar automaticamente e anular o aprovisionamento utilizadores e/ou grupos para Samanage.

> [!NOTE]
> Este tutorial descreve um conector desenvolvido com o serviço de fornecimento de utilizador do Azure AD. Para obter detalhes importantes sobre o que faz este serviço, como funciona e perguntas mais frequentes, consulte [automatizar utilizador aprovisionamento e desaprovisionamento para aplicações de SaaS no Azure Active Directory](./active-directory-saas-app-provisioning.md).

## <a name="prerequisites"></a>Pré-requisitos

O cenário descrito neste tutorial assume que já tem o seguinte:

*   Um inquilino do Azure Active Directory
*   Um inquilino Samanage com o [Professional](https://www.samanage.com/pricing/) planear ou melhor ativado 
*   Uma conta de utilizador no Samanage com permissões de administrador 

> [!NOTE]
> O Azure AD aprovisionamento integração depende o [API de REST Samanage](https://www.samanage.com/api/), que está disponível para equipas Samanage no plano de profissionais ou melhor.

## <a name="adding-samanage-from-the-gallery"></a>Adicionar Samanage a partir da Galeria
Antes de configurar Samanage para aprovisionamento com o Azure AD de utilizadores automática, tem de adicionar Samanage da Galeria de aplicações do Azure AD à sua lista de aplicações de SaaS geridas.

**Para adicionar Samanage da Galeria de aplicações do Azure AD, execute os seguintes passos:**

1. No  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em de **do Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais** > **todas as aplicações**.

    ![As secção de aplicações da empresa][2]
    
3. Para adicionar Samanage, clique o **nova aplicação** botão na parte superior da caixa de diálogo.

    ![O novo botão de aplicação][3]

4. Na caixa de pesquisa, escreva **Samanage**.

    ![Aprovisionamento de Samanage](./media/active-directory-saas-samanage-provisioning-tutorial/Samanage4.png)

5. No painel de resultados, selecione **Samanage**e, em seguida, clique em de **adicionar** botão Adicionar Samanage à sua lista de aplicações SaaS.

    ![Aprovisionamento de Samanage](./media/active-directory-saas-samanage-provisioning-tutorial/Samanage6.png)

    ![Aprovisionamento de Samanage](./media/active-directory-saas-samanage-provisioning-tutorial/Samanage5.png)

## <a name="assigning-users-to-samanage"></a>Atribuir utilizadores a Samanage

Azure Active Directory utiliza um conceito chamado "atribuições de" para determinar quais os utilizadores devem receber acesso às aplicações selecionadas. No contexto de aprovisionamento de utilizadores automático, são sincronizados apenas a utilizadores e/ou grupos que tenham sido "atribuídos" a uma aplicação no Azure AD. 

Antes de configurar e ativar o aprovisionamento de utilizadores automática, deve decidir quais os utilizadores e/ou grupos no Azure AD necessitam de aceder a Samanage. Depois de decidir, pode atribuir estes utilizadores e/ou grupos a Samanage ao seguir as instruções aqui:

*   [Atribuir um utilizador ou grupo a uma aplicação empresarial](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-samanage"></a>Sugestões importantes para atribuir utilizadores a Samanage

*   Recomenda-se que um único utilizador do Azure AD é atribuído a Samanage para testar a configuração de aprovisionamento de utilizadores automática. Os utilizadores adicionais e/ou grupos podem ser atribuídos mais tarde.

*   Ao atribuir um utilizador a Samanage, tem de selecionar uma função de específicas da aplicação válida (se disponível) na caixa de diálogo atribuição. Os utilizadores com o **predefinido acesso** função foram excluídos de aprovisionamento.

## <a name="configuring-automatic-user-provisioning-to-samanage"></a>Configurar o aprovisionamento de utilizadores automática para Samanage

Esta secção descreve os passos para configurar o Azure AD aprovisionamento do serviço para criar, atualizar e desativar os utilizadores e/ou grupos no Samanage com base em atribuições de utilizador e/ou grupo no Azure AD.

> [!TIP]
> Também pode optar por ativar baseados em SAML-início de sessão único para Samanage, seguindo as instruções fornecidas a [Samanage único início de sessão tutorial](active-directory-saas-samanage-tutorial.md). O início de sessão único a pode ser configurado independentemente do aprovisionamento de utilizadores automática, apesar destas duas funcionalidades complementar dos entre si. Para obter mais informações, consulte o [Samanage único início de sessão tutorial](active-directory-saas-samanage-tutorial.md).

### <a name="to-configure-automatic-user-provisioning-for-samanage-in-azure-ad"></a>Para configurar o aprovisionamento de utilizadores automática para Samanage no Azure AD:

1. Iniciar sessão para o [portal do Azure](https://portal.azure.com) e navegue para **do Azure Active Directory > aplicações da empresa > todas as aplicações**.

2. Selecione Samanage na sua lista de aplicações SaaS.
 
    ![Aprovisionamento de Samanage](./media/active-directory-saas-samanage-provisioning-tutorial/Samanage7.png)

3. Selecione o **aprovisionamento** separador.
    
    ![Aprovisionamento de Samanage](./media/active-directory-saas-samanage-provisioning-tutorial/Samanage8.png)

4. Definir o **modo de aprovisionamento** para **automática**.

    ![Aprovisionamento de Samanage](./media/active-directory-saas-samanage-provisioning-tutorial/Samanage9.png)

5. Sob o **credenciais de administrador** secção, de entrada a **nome de utilizador de Admin**, e **palavra-passe de administrador** da conta da sua Samanages. Exemplos destes valores são:

    *   No **nome de utilizador de Admin** campo, preencher o nome de utilizador da conta de administrador no seu inquilino Samanage. Exemplo: admin@contoso.com.

    *   No **palavra-passe de administrador** campo, preencher a palavra-passe correspondente ao nome de utilizador de admin especificado anteriormente.

6. Após preencher os campos mostrados no passo 5, clique em **Testar ligação** para garantir que o Azure AD pode ligar ao Samanage. Se a ligação falhar, certifique-se de que a conta de Samanage possui permissões de administrador e tente novamente.

    ![Aprovisionamento de Samanage](./media/active-directory-saas-samanage-provisioning-tutorial/Samanage10.png)

7. No **correio eletrónico de notificação** campo, introduza o endereço de e-mail de uma pessoa ou grupo que deve receber as notificações de erro de aprovisionamento e marque a caixa de verificação **enviar uma notificação por e-mail quando uma falha ocorre**.

    ![Aprovisionamento de Samanage](./media/active-directory-saas-samanage-provisioning-tutorial/Samanage11.png)

8. Clique em **Guardar**.

9. Sob o **mapeamentos** secção, selecione **sincronizar utilizadores do Azure Active Directory para Samanage**.

    ![Aprovisionamento de Samanage](./media/active-directory-saas-samanage-provisioning-tutorial/Samanage12.png)

10. Reveja os atributos de utilizador que são sincronizados a partir do Azure AD e Samanage no **atributo mapeamento** secção. Os atributos selecionados como **correspondência** propriedades são utilizadas para corresponder as contas de utilizador no Samanage para operações de atualização. Selecione o **guardar** botão para consolidar as alterações.

    ![Aprovisionamento de Samanage](./media/active-directory-saas-samanage-provisioning-tutorial/Samanage13.png)

12. Sob o **mapeamentos** secção, selecione **sincronizar Azure grupos do Active Directory para Samanage**.

    ![Aprovisionamento de Samanage](./media/active-directory-saas-samanage-provisioning-tutorial/Samanage14.png)

13. Reveja os atributos de grupo que são sincronizados a partir do Azure AD e Samanage no **atributo mapeamento** secção. Os atributos selecionados como **correspondência** propriedades são utilizadas para correspondam aos grupos de Samanage para operações de atualização. Selecione o **guardar** botão para consolidar as alterações.

    ![Aprovisionamento de Samanage](./media/active-directory-saas-samanage-provisioning-tutorial/Samanage15.png)

14. Para configurar filtros de âmbito, consulte as seguintes instruções fornecidas a [tutorial de filtro Scoping](./active-directory-saas-scoping-filters.md).

15. Para ativar o Azure AD aprovisionamento do serviço para Samanage, altere o **estado de aprovisionamento** para **no** no **definições** secção.

    ![Aprovisionamento de Samanage](./media/active-directory-saas-samanage-provisioning-tutorial/Samanage16.png)

16. Definir utilizadores e/ou grupos que pretende que o aprovisionamento Samanage ao escolher os valores pretendidos no **âmbito** no **definições** secção.

    ![Aprovisionamento de Samanage](./media/active-directory-saas-samanage-provisioning-tutorial/Samanage17.png)

17. Quando estiver pronto para aprovisionar, clique em **guardar**.

    ![Aprovisionamento de Samanage](./media/active-directory-saas-samanage-provisioning-tutorial/Samanage18.png)

Esta operação inicia a sincronização inicial de todos os utilizadores e/ou grupos definidos no **âmbito** no **definições** secção. A sincronização inicial demora mais para efetuar a sincronizações subsequentes, o que ocorrer aproximadamente a cada 40 minutos, desde que o Azure AD aprovisionamento do serviço está em execução. Pode utilizar o **detalhes de sincronização** secção para monitorizar o progresso e siga as ligações para o relatório de atividade, que descreve todas as ações efetuadas pelo Azure AD aprovisionamento do serviço no Samanage de aprovisionamento.

Para obter mais informações sobre como ler o Azure AD, os registos de aprovisionamento, consulte [relatórios sobre o aprovisionamento da conta de utilizador automáticas](./active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Gerir o aprovisionamento da conta de utilizador para aplicações da empresa](active-directory-enterprise-apps-manage-provisioning.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a>Passos Seguintes

* [Saiba como rever os registos e obter relatórios sobre o aprovisionamento de atividade](active-directory-saas-provisioning-reporting.md)

<!--Image references-->
[1]: ./media/active-directory-saas-samanage-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_03.png
