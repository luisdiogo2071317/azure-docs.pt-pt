---
title: 'Tutorial: Configurar BlueJeans para aprovisionamento de utilizadores automática no Azure Active Directory | Microsoft Docs'
description: Saiba como configurar o Azure Active Directory para aprovisionar e anular o aprovisionamento contas de utilizador para BlueJeans automaticamente.
services: active-directory
documentationcenter: ''
author: zhchia
writer: zhchia
manager: beatrizd-msft
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/01/2018
ms.author: v-ant
ms.openlocfilehash: 522d94e2a6fbd8e7eb32941e2cab59de7b28384c
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/20/2018
---
# <a name="tutorial-configure-bluejeans-for-automatic-user-provisioning"></a>Tutorial: Configurar BlueJeans para aprovisionamento de utilizadores automática

O objetivo deste tutorial é demonstrar os passos para ser efetuada no BlueJeans e Azure Active Directory (Azure AD) para configurar o Azure AD para aprovisionar automaticamente e anular o aprovisionamento utilizadores e/ou grupos para BlueJeans.

> [!NOTE]
> Este tutorial descreve um conector desenvolvido com o serviço de fornecimento de utilizador do Azure AD. Para obter detalhes importantes sobre o que faz este serviço, como funciona e perguntas mais frequentes, consulte [automatizar utilizador aprovisionamento e desaprovisionamento para aplicações de SaaS no Azure Active Directory](./active-directory-saas-app-provisioning.md).

## <a name="prerequisites"></a>Pré-requisitos

O cenário descrito neste tutorial assume que já tem o seguinte:

*   Um inquilino do Azure AD
*   Um inquilino BlueJeans com o [minha empresa](https://www.BlueJeans.com/pricing) planear ou melhor ativado
*   Uma conta de utilizador no BlueJeans com permissões de administrador

> [!NOTE]
> O Azure AD aprovisionamento integração depende o [BlueJeans API](https://BlueJeans.github.io/developer), que está disponível para equipas BlueJeans o plano Standard ou melhor.

## <a name="adding-bluejeans-from-the-gallery"></a>Adicionar BlueJeans a partir da Galeria
Antes de configurar BlueJeans para aprovisionamento de utilizadores automática com o Azure AD, tem de adicionar BlueJeans da Galeria de aplicações do Azure AD à sua lista de aplicações de SaaS geridas.

**Para adicionar BlueJeans da Galeria de aplicações do Azure AD, execute os seguintes passos:**

1. No  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em de **do Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais** > **todas as aplicações**.

    ![As secção de aplicações da empresa][2]
    
3. Para adicionar BlueJeans, clique o **nova aplicação** botão na parte superior da caixa de diálogo.

    ![O novo botão de aplicação][3]

4. Na caixa de pesquisa, escreva **BlueJeans**.

    ![BlueJeans aprovisionamento](./media/active-directory-saas-bluejeans-provisioning-tutorial/BluejeansAppSearch.png)

5. No painel de resultados, selecione **BlueJeans**e, em seguida, clique em de **adicionar** botão Adicionar BlueJeans à sua lista de aplicações SaaS.

    ![BlueJeans aprovisionamento](./media/active-directory-saas-bluejeans-provisioning-tutorial/BluejeansAppSearchResults.png)

    ![BlueJeans aprovisionamento](./media/active-directory-saas-bluejeans-provisioning-tutorial/BluejeansAppCreate.png)
    
## <a name="assigning-users-to-bluejeans"></a>Atribuir utilizadores a BlueJeans

Azure Active Directory utiliza um conceito chamado "atribuições de" para determinar quais os utilizadores devem receber acesso às aplicações selecionadas. No contexto de aprovisionamento de utilizadores automático, são sincronizados apenas a utilizadores e/ou grupos que tenham sido "atribuídos" a uma aplicação no Azure AD.

Antes de configurar e ativar o aprovisionamento de utilizadores automática, deve decidir quais os utilizadores e/ou grupos no Azure AD necessitam de aceder a BlueJeans. Depois de decidir, pode atribuir estes utilizadores e/ou grupos a BlueJeans ao seguir as instruções aqui:

*   [Atribuir um utilizador ou grupo a uma aplicação empresarial](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-bluejeans"></a>Sugestões importantes para atribuir utilizadores a BlueJeans

*   Recomenda-se que um único utilizador do Azure AD é atribuído a BlueJeans para testar a configuração de aprovisionamento de utilizadores automática. Os utilizadores adicionais e/ou grupos podem ser atribuídos mais tarde.

*   Ao atribuir um utilizador a BlueJeans, tem de selecionar uma função de específicas da aplicação válida (se disponível) na caixa de diálogo atribuição. Os utilizadores com o **predefinido acesso** função foram excluídos de aprovisionamento.

## <a name="configuring-automatic-user-provisioning-to-bluejeans"></a>Configurar o aprovisionamento de utilizadores automática para BlueJeans

Esta secção descreve os passos para configurar o Azure AD aprovisionamento do serviço para criar, atualizar e desativar os utilizadores e/ou grupos no BlueJeans com base em atribuições de utilizador e/ou grupo no Azure AD.

> [!TIP]
> Também pode optar por ativar baseados em SAML-início de sessão único para BlueJeans, seguindo as instruções fornecidas a [BlueJeans único início de sessão tutorial](active-directory-saas-bluejeans-tutorial.md). O início de sessão único a pode ser configurado independentemente do aprovisionamento de utilizadores automática, apesar destas duas funcionalidades complementar dos entre si.

### <a name="to-configure-automatic-user-provisioning-for-bluejeans-in-azure-ad"></a>Para configurar o aprovisionamento de utilizadores automática para BlueJeans no Azure AD:

1. Iniciar sessão para o [portal do Azure](https://portal.azure.com) e navegue para **do Azure Active Directory > aplicações da empresa > todas as aplicações**.

2. Selecione BlueJeans na sua lista de aplicações SaaS.
 
    ![BlueJeans aprovisionamento](./media/active-directory-saas-bluejeans-provisioning-tutorial/Bluejeans2.png)

3. Selecione o **aprovisionamento** separador.

    ![BlueJeans aprovisionamento](./media/active-directory-saas-bluejeans-provisioning-tutorial/BluejeansProvisioningTab.png)

4. Definir o **modo de aprovisionamento** para **automática**.

    ![BlueJeans aprovisionamento](./media/active-directory-saas-bluejeans-provisioning-tutorial/Bluejeans1.png)

5. Sob o **credenciais de administrador** secção, introduza o **nome de utilizador de Admin**, e **palavra-passe de administrador** da sua conta BlueJeans. Exemplos destes valores são:

    *   No **nome de utilizador de Admin** campo, preencher o nome de utilizador da conta de administrador no seu inquilino BlueJeans. Exemplo: admin@contoso.com.

    *   No **palavra-passe de administrador** campo, preencher a palavra-passe correspondente ao nome de utilizador administrador.

6. Após preencher os campos mostrados no passo 5, clique em **Testar ligação** para garantir que o Azure AD pode ligar ao BlueJeans. Se a ligação falhar, certifique-se de que a conta de BlueJeans possui permissões de administrador e tente novamente.

    ![BlueJeans aprovisionamento](./media/active-directory-saas-bluejeans-provisioning-tutorial/BluejeansTestConnection.png)

7. No **correio eletrónico de notificação** campo, introduza o endereço de e-mail de uma pessoa ou grupo que deve receber as notificações de erro de aprovisionamento e marque a caixa de verificação - **enviar uma notificação por e-mail quando uma falha ocorre**.

    ![BlueJeans aprovisionamento](./media/active-directory-saas-bluejeans-provisioning-tutorial/BluejeansNotificationEmail.png)

8. Clique em **Guardar**.

9. Sob o **mapeamentos** secção, selecione **sincronizar utilizadores do Azure Active Directory para BlueJeans**.

    ![BlueJeans aprovisionamento](./media/active-directory-saas-bluejeans-provisioning-tutorial/BluejeansMapping.png)

10. Reveja os atributos de utilizador que são sincronizados a partir do Azure AD e BlueJeans no **atributo mapeamento** secção. Os atributos selecionados como **correspondência** propriedades são utilizadas para corresponder as contas de utilizador no BlueJeans para operações de atualização. Selecione o **guardar** botão para consolidar as alterações.

    ![BlueJeans aprovisionamento](./media/active-directory-saas-bluejeans-provisioning-tutorial/BluejeansUserMappingAtrributes.png)

11. Para configurar filtros de âmbito, consulte as seguintes instruções fornecidas a [tutorial de filtro Scoping](./active-directory-saas-scoping-filters.md).

12. Para ativar o Azure AD aprovisionamento do serviço para BlueJeans, altere o **estado de aprovisionamento** para **no** no **definições** secção.

    ![BlueJeans aprovisionamento](./media/active-directory-saas-bluejeans-provisioning-tutorial/BluejeansProvisioningStatus.png)

13. Definir utilizadores e/ou grupos que pretende que o aprovisionamento BlueJeans ao escolher os valores pretendidos no **âmbito** no **definições** secção.

    ![BlueJeans aprovisionamento](./media/active-directory-saas-bluejeans-provisioning-tutorial/UserGroupSelection.png)

14. Quando estiver pronto para aprovisionar, clique em **guardar**.

    ![BlueJeans aprovisionamento](./media/active-directory-saas-bluejeans-provisioning-tutorial/SaveProvisioning.png)

Esta operação inicia a sincronização inicial de todos os utilizadores e/ou grupos definidos no **âmbito** no **definições** secção. A sincronização inicial demora mais para efetuar a sincronizações subsequentes, o que ocorrer aproximadamente a cada 40 minutos, desde que o Azure AD aprovisionamento do serviço está em execução. Pode utilizar o **detalhes de sincronização** secção para monitorizar o progresso e siga as ligações para o relatório de atividade, que descreve todas as ações efetuadas pelo Azure AD aprovisionamento do serviço no BlueJeans de aprovisionamento.

Para obter mais informações sobre como ler o Azure AD, os registos de aprovisionamento, consulte [relatórios sobre o aprovisionamento da conta de utilizador automáticas](./active-directory-saas-provisioning-reporting.md).

## <a name="connector-limitations"></a>Limitações de conector

* Bluejeans não permite a nomes de utilizador que excedem 30 carateres.

## <a name="additional-resources"></a>Recursos adicionais

* [Gerir o aprovisionamento da conta de utilizador para aplicações da empresa](active-directory-enterprise-apps-manage-provisioning.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Passos Seguintes

* [Saiba como rever os registos e obter relatórios sobre o aprovisionamento de atividade](active-directory-saas-provisioning-reporting.md)

<!--Image references-->
[1]: ./media/active-directory-saas-bluejeans-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_03.png
