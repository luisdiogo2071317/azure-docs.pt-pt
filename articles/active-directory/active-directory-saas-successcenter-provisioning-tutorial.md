---
title: "Tutorial: Configurar pilares OnDemand para o aprovisionamento de utilizador automáticas com o Azure Active Directory | Microsoft Docs"
description: Saiba como configurar o Azure Active Directory para aprovisionar e anular o aprovisionamento contas de utilizador para pilares OnDemand automaticamente.
services: active-directory
documentationcenter: 
author: zhchia
writer: zhchia
manager: beatrizd
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2018
ms.author: v-ant
ms.openlocfilehash: 8aed9bdcdd54f2f0ef3cd81f979635e5716f0b5f
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/05/2018
---
# <a name="tutorial-configure-cornerstone-ondemand-for-automatic-user-provisioning"></a>Tutorial: Configurar pilares OnDemand para o aprovisionamento de utilizador automáticas


O objetivo deste tutorial é demonstrar os passos para ser efetuada no pilares OnDemand e Azure Active Directory (Azure AD) para configurar o Azure AD para aprovisionar e anular o aprovisionamento utilizadores e/ou grupos para pilares OnDemand automaticamente.


> [!NOTE]
> Este tutorial descreve um conector desenvolvido com o serviço de fornecimento de utilizador do Azure AD. Para obter detalhes importantes sobre o que faz este serviço, como funciona e perguntas mais frequentes, consulte [automatizar utilizador aprovisionamento e desaprovisionamento para aplicações de SaaS no Azure Active Directory](./active-directory-saas-app-provisioning.md).

## <a name="prerequisites"></a>Pré-requisitos

O cenário descrito neste tutorial assume que já tem o seguinte:

*   Um inquilino do Azure AD
*   Um inquilino pilares OnDemand
*   Uma conta de utilizador em OnDemand pilares com permissões de administrador


> [!NOTE]
> O Azure AD aprovisionamento integração depende o [pilares OnDemand Webservice](https://help.csod.com/help/csod_0/Content/Resources/Documents/WebServices/CSOD_-_Summary_of_Web_Services_v20151106.pdf), que está disponível para as equipas de pilares OnDemand.

## <a name="adding-cornerstone-ondemand-from-the-gallery"></a>Adicionar OnDemand fundamental na galeria do
Antes de configurar pilares OnDemand para o aprovisionamento de utilizador automáticas com o Azure AD, tem de adicionar OnDemand fundamental da Galeria de aplicações do Azure AD à sua lista de aplicações de SaaS geridas.

**Para adicionar OnDemand fundamental da Galeria de aplicações do Azure AD, execute os seguintes passos:**

1. No  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em de **do Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais** > **todas as aplicações**.

    ![As secção de aplicações da empresa][2]
    
3. Para adicionar pilares OnDemand, clique o **nova aplicação** botão na parte superior da caixa de diálogo.

    ![O novo botão de aplicação][3]

4. Na caixa de pesquisa, escreva **pilares OnDemand**.

    ![Aprovisionamento de OnDemand pilares](./media/active-directory-saas-successcenter-provisioning-tutorial/AppSearch.png)

5. No painel de resultados, selecione **pilares OnDemand**e, em seguida, clique em de **adicionar** botão Adicionar pilares OnDemand à sua lista de aplicações SaaS.

    ![Aprovisionamento de OnDemand pilares](./media/active-directory-saas-successcenter-provisioning-tutorial/AppSearchResults.png)

    ![Aprovisionamento de OnDemand pilares](./media/active-directory-saas-successcenter-provisioning-tutorial/AppCreation.png)

## <a name="assigning-users-to-cornerstone-ondemand"></a>Atribuir utilizadores a pilares OnDemand

Azure Active Directory utiliza um conceito chamado "atribuições de" para determinar quais os utilizadores devem receber acesso às aplicações selecionadas. No contexto de aprovisionamento de utilizadores automático, são sincronizados apenas a utilizadores e/ou grupos que tenham sido "atribuídos" a uma aplicação no Azure AD. 

Antes de configurar e ativar o aprovisionamento de utilizadores automática, deve decidir quais os utilizadores e/ou grupos no Azure AD necessitam de aceder a pilares OnDemand. Depois de decidir, pode atribuir estes utilizadores e/ou grupos a pilares OnDemand ao seguir as instruções aqui:

*   [Atribuir um utilizador ou grupo a uma aplicação empresarial](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-cornerstone-ondemand"></a>Sugestões importantes para atribuir utilizadores a pilares OnDemand

*   Recomenda-se que um único utilizador do Azure AD é atribuído a pilares OnDemand para testar a configuração de aprovisionamento de utilizadores automática. Os utilizadores adicionais e/ou grupos podem ser atribuídos mais tarde.

*   Ao atribuir um utilizador a pilares OnDemand, tem de selecionar uma função de específicas da aplicação válida (se disponível) na caixa de diálogo atribuição. Os utilizadores com o **predefinido acesso** função foram excluídos de aprovisionamento.

## <a name="configuring-automatic-user-provisioning-to-cornerstone-ondemand"></a>Configurar o aprovisionamento de utilizadores automática para pilares OnDemand

Esta secção descreve os passos para configurar o Azure AD aprovisionamento do serviço para criar, atualizar e desativar os utilizadores e/ou grupos no OnDemand pilares com base no utilizador e/ou atribuições de grupo no Azure AD.


### <a name="to-configure-automatic-user-provisioning-for-cornerstone-ondemand-in-azure-ad"></a>Para configurar o aprovisionamento de utilizadores automática para OnDemand fundamental no Azure AD:


1. Iniciar sessão para o [portal do Azure](https://portal.azure.com) e navegue para **do Azure Active Directory > aplicações da empresa > todas as aplicações**.

2. Selecione OnDemand fundamental na sua lista de aplicações SaaS.
 
    ![Aprovisionamento de OnDemand pilares](./media/active-directory-saas-successcenter-provisioning-tutorial/Successcenter2.png)

3. Selecione o **aprovisionamento** separador.

    ![Aprovisionamento de OnDemand pilares](./media/active-directory-saas-successcenter-provisioning-tutorial/ProvisioningTab.png)

4. Definir o **modo de aprovisionamento** para **automática**.

    ![Aprovisionamento de OnDemand pilares](./media/active-directory-saas-successcenter-provisioning-tutorial/ProvisioningCredentials.png)

5. Sob o **credenciais de administrador** secção, de entrada a **nome de utilizador de Admin**, **palavra-passe de administrador**, e **domínio** do seu OnDemand de pilares conta.

    *   No **nome de utilizador de Admin** campo, preencher o nome de utilizador da conta de administrador no seu inquilino de OnDemand pilares. Exemplo: admin.

    *   No **palavra-passe de administrador** campo, preencher a palavra-passe correspondente ao nome de utilizador administrador.

    *   No **domínio** campo, preencher o URL do serviço Web de fundamental. Exemplo: O serviço está localizado em `https://ws-[corpname].csod.com/feed30/clientdataservice.asmx`para Contoso o domínio é `https://ws-contoso.csod.com/feed30/clientdataservice.asmx`.

6. Após preencher os campos mostrados no passo 5, clique em **Testar ligação** para garantir que o Azure AD pode ligar ao pilares OnDemand. Se a ligação falhar, certifique-se de que a conta de pilares OnDemand possui permissões de administrador e tente novamente.

    ![Aprovisionamento de OnDemand pilares](./media/active-directory-saas-successcenter-provisioning-tutorial/TestConnection.png)

7. No **correio eletrónico de notificação** campo, introduza o endereço de e-mail de uma pessoa ou grupo que deve receber as notificações de erro de aprovisionamento e marque a caixa de verificação **enviar uma notificação por e-mail quando uma falha ocorre**.

    ![Aprovisionamento de OnDemand pilares](./media/active-directory-saas-successcenter-provisioning-tutorial/EmailNotification.png)

8. Clique em **Guardar**.

9. Sob o **mapeamentos** secção, selecione **sincronizar do Azure Active Directory que os utilizadores pilares OnDemand**.

    ![Aprovisionamento de OnDemand pilares](./media/active-directory-saas-successcenter-provisioning-tutorial/UserMapping.png)

10. Reveja os atributos de utilizador que são sincronizados a partir do Azure AD e OnDemand fundamental no **atributo mapeamento** secção. Os atributos selecionados como **correspondência** propriedades são utilizadas para corresponder as contas de utilizador em OnDemand fundamental para as operações de atualização. Selecione o **guardar** botão para consolidar as alterações.

    ![Aprovisionamento de OnDemand pilares](./media/active-directory-saas-successcenter-provisioning-tutorial/UserMappingAttributes.png)

11. Para configurar filtros de âmbito, consulte as seguintes instruções fornecidas a [tutorial de filtro Scoping](./active-directory-saas-scoping-filters.md).

12. Para ativar o Azure AD aprovisionamento do serviço para pilares OnDemand, altere o **estado de aprovisionamento** para **no** no **definições** secção.

    ![Aprovisionamento de OnDemand pilares](./media/active-directory-saas-successcenter-provisioning-tutorial/ProvisioningStatus.png)

13. Definir utilizadores e/ou grupos que pretende que o aprovisionamento pilares OnDemand ao escolher os valores pretendidos no **âmbito** no **definições** secção.

    ![Aprovisionamento de OnDemand pilares](./media/active-directory-saas-successcenter-provisioning-tutorial/SyncScope.png)

14. Quando estiver pronto para aprovisionar, clique em **guardar**.

    ![Aprovisionamento de OnDemand pilares](./media/active-directory-saas-successcenter-provisioning-tutorial/Save.png) 


Esta operação inicia a sincronização inicial de todos os utilizadores e/ou grupos definidos no **âmbito** no **definições** secção. A sincronização inicial demora mais para efetuar a sincronizações subsequentes, o que ocorrer aproximadamente a cada 40 minutos, desde que o Azure AD aprovisionamento do serviço está em execução. Pode utilizar o **detalhes de sincronização** secção para monitorizar o progresso e siga as ligações para o relatório de atividade, que descreve todas as ações efetuadas pelo Azure AD aprovisionamento do serviço em OnDemand fundamental para o aprovisionamento.

Para obter mais informações sobre como ler o Azure AD, os registos de aprovisionamento, consulte [relatórios sobre o aprovisionamento da conta de utilizador automáticas](./active-directory-saas-provisioning-reporting.md).
## <a name="additional-resources"></a>Recursos adicionais

* [Gerir o aprovisionamento da conta de utilizador para aplicações da empresa](active-directory-enterprise-apps-manage-provisioning.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a>Passos Seguintes

* [Saiba como rever os registos e obter relatórios sobre o aprovisionamento de atividade](active-directory-saas-provisioning-reporting.md)

<!--Image references-->
[1]: ./media/active-directory-saas-successcenter-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-successcenter-provisioning-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-successcenter-provisioning-tutorial/tutorial_general_03.png
