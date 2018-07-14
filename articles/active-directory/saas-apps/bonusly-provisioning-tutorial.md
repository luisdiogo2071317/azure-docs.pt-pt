---
title: 'Tutorial: Configurar Bonusly para aprovisionamento automático de utilizadores no Azure Active Directory | Documentos da Microsoft'
description: Saiba como configurar o Azure Active Directory para aprovisionar e desaprovisionar contas de utilizador para Bonusly automaticamente.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd-msft
ms.assetid: na
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2018
ms.author: v-wingf-msft
ms.openlocfilehash: 5a28e1249ab8b1d1e3db3433fcce27dcbcc35176
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/12/2018
ms.locfileid: "38991442"
---
# <a name="tutorial-configure-bonusly-for-automatic-user-provisioning"></a>Tutorial: Configurar Bonusly para aprovisionamento automático de utilizadores

O objetivo deste tutorial é demonstrar as etapas a serem executadas no Bonusly e o Azure Active Directory (Azure AD) para configurar o Azure AD automaticamente aprovisionar e desaprovisionar utilizadores e/ou grupos para Bonusly.

> [!NOTE]
> Este tutorial descreve um conector assentes no serviço de aprovisionamento de utilizador do Azure AD. Para obter detalhes importantes sobre o que faz este serviço, como ele funciona e perguntas mais frequentes, consulte [automatizar o aprovisionamento de utilizador e a aplicações SaaS com o Azure Active Directory de desaprovisionamento](../active-directory-saas-app-provisioning.md).

## <a name="prerequisites"></a>Pré-requisitos

O cenário descrito neste tutorial parte do princípio de que já tem o seguinte:

*   Um inquilino do Azure AD
*   A [Bonusly inquilino](https://bonus.ly/pricing)
*   Uma conta de utilizador no Bonusly com permissões de administrador

> [!NOTE]
> O Azure AD aprovisionamento integração depende a [Bonusly API de Rest](https://bonusly.gelato.io/reference), que está disponível para os desenvolvedores Bonusly.

## <a name="adding-bonusly-from-the-gallery"></a>Adicionando Bonusly da Galeria
Antes de configurar Bonusly para aprovisionamento automático de utilizadores com o Azure AD, terá de adicionar Bonusly a partir da Galeria de aplicações do Azure AD à sua lista de aplicações de SaaS geridas.

**Para adicionar Bonusly a partir da Galeria de aplicações do Azure AD, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique nas **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais** > **todas as aplicações**.

    ![As secção de aplicações empresariais][2]
    
3. Para adicionar Bonusly, clique a **nova aplicação** botão na parte superior da caixa de diálogo.

    ![O novo botão de aplicativo][3]

4. Na caixa de pesquisa, escreva **Bonusly**.

    ![Aprovisionamento bonusly](./media/bonusly-provisioning-tutorial/AppSearch.png)

5. No painel de resultados, selecione **Bonusly**e, em seguida, clique nas **Add** botão para adicionar Bonusly à sua lista de aplicações SaaS.

    ![Aprovisionamento bonusly](./media/bonusly-provisioning-tutorial/AppSearchResults.png)

    ![Aprovisionamento bonusly](./media/bonusly-provisioning-tutorial/AppCreation.png)

## <a name="assigning-users-to-bonusly"></a>Atribuir utilizadores a Bonusly

O Azure Active Directory utiliza um conceito chamado "atribuições" para determinar quais os utilizadores devem receber acesso às aplicações selecionadas. No contexto de aprovisionamento automático de utilizadores, apenas a utilizadores e/ou grupos que foram "atribuídos" a uma aplicação no Azure AD são sincronizados. 

Antes de configurar e ativar o aprovisionamento de utilizador automático, deve decidir o que os utilizadores e/ou grupos no Azure AD precisam de acesso a Bonusly. Depois de decidir, pode atribuir estes utilizadores e/ou grupos a Bonusly ao seguir as instruções aqui:

*   [Atribuir um utilizador ou grupo a uma aplicação empresarial](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-bonusly"></a>Dicas importantes para atribuir utilizadores a Bonusly

*   Recomenda-se que um único utilizador do Azure AD está atribuído a Bonusly para testar o configuração de aprovisionamento automático de utilizadores. Os utilizadores adicionais e/ou grupos podem ser atribuídos mais tarde.

*   Ao atribuir um utilizador para Bonusly, tem de selecionar qualquer função de específicas da aplicação válida (se disponível) na caixa de diálogo atribuição. Os utilizadores com o **acesso predefinido** função são excluídas desde o aprovisionamento.

## <a name="configuring-automatic-user-provisioning-to-bonusly"></a>Configurar o aprovisionamento automático de utilizadores para Bonusly

Esta secção orienta-o pelos passos para configurar o Azure AD do serviço de aprovisionamento para criar, atualizar e desativar os utilizadores e/ou grupos no Bonusly com base em atribuições de utilizador e/ou grupo no Azure AD.

> [!TIP]
> Também pode optar por ativar baseado em SAML início de sessão único para Bonusly, seguindo as instruções fornecidas no [Bonusly único início de sessão tutorial](bonus-tutorial.md). Início de sessão único a pode ser configurada independentemente de aprovisionamento automático de utilizadores, embora esses dois recursos complementar entre si.

### <a name="to-configure-automatic-user-provisioning-for-bonusly-in-azure-ad"></a>Para configurar o aprovisionamento automático de utilizadores para Bonusly no Azure AD:

1. Entrar para o [portal do Azure](https://portal.azure.com) e procure **Azure Active Directory > aplicações empresariais > todos os aplicativos**.

2. Selecione Bonusly da sua lista de aplicações SaaS.
 
    ![Aprovisionamento bonusly](./media/bonusly-provisioning-tutorial/AppInstanceSearch.png)

3. Selecione o **aprovisionamento** separador.
    
    ![Aprovisionamento bonusly](./media/bonusly-provisioning-tutorial/ProvisioningTab.png)

4. Definir o **modo de aprovisionamento** ao **automática**.

    ![Aprovisionamento bonusly](./media/bonusly-provisioning-tutorial/ProvisioningCredentials.png)

5. Sob o **credenciais de administrador** secção, de entrada a **segredo de Token** da sua Bonusly conta conforme descrito no passo 6.

6. O **segredo de Token** para sua Bonusly conta está localizada na **administração > empresa > integrações**. Na **se quiser código** secção, clique em **API > criar Token de acesso de novo API** para criar um novo segredo do Token.

    ![Aprovisionamento bonusly](./media/bonusly-provisioning-tutorial/BonuslyIntegrations.png)

    ![Aprovisionamento bonusly](./media/bonusly-provisioning-tutorial/BonsulyRestApi.png)

    ![Aprovisionamento bonusly](./media/bonusly-provisioning-tutorial/CreateToken.png)

7. No ecrã seguinte, escreva um nome para o token de acesso na caixa de texto fornecido, em seguida, prima **criar chave de Api**. O novo token de acesso será apresentado por alguns segundos num pop-up.

    ![Aprovisionamento bonusly](./media/bonusly-provisioning-tutorial/Token01.png)

    ![Aprovisionamento bonusly](./media/bonusly-provisioning-tutorial/Token02.png)

8. Após preencher os campos mostrados no passo 5, clique em **Testar ligação** para garantir que o Azure AD pode ligar-se Bonusly. Se a ligação falhar, certifique-se de que sua conta Bonusly tem permissões de administrador e tente novamente.

    ![Aprovisionamento bonusly](./media/bonusly-provisioning-tutorial/TestConnection.png)
    
9. Na **notificação por E-Mail** campo, introduza o endereço de e-mail de uma pessoa ou grupo que deve receber as notificações de erro de aprovisionamento e marque a caixa de verificação **enviar uma notificação por e-mail quando uma falha ocorre**.

    ![Aprovisionamento bonusly](./media/bonusly-provisioning-tutorial/EmailNotification.png)

10. Clique em **Guardar**.

11. Sob o **mapeamentos** secção, selecione **sincronizar utilizadores do Azure Active Directory para Bonusly**.

    ![Aprovisionamento bonusly](./media/bonusly-provisioning-tutorial/UserMappings.png)

12. Reveja os atributos de utilizador que são sincronizados a partir do Azure AD para Bonusly no **mapeamento do atributo** secção. Os atributos selecionados como **correspondência** propriedades são usadas de acordo com as contas de utilizador no Bonusly para operações de atualização. Selecione o **guardar** botão para consolidar as alterações.

    ![Aprovisionamento bonusly](./media/bonusly-provisioning-tutorial/UserAttributeMapping.png)

13. Para configurar filtros de âmbito, consulte as seguintes instruções fornecidas a [tutorial de filtro de Scoping](../active-directory-saas-scoping-filters.md).

14. Para ativar o Azure AD para Bonusly do serviço de aprovisionamento, altere a **estado de aprovisionamento** para **no** no **definições** secção.

    ![Aprovisionamento bonusly](./media/bonusly-provisioning-tutorial/ProvisioningStatus.png)

15. Definir a utilizadores e/ou grupos que deseja fazer o aprovisionamento Bonusly escolhendo os valores pretendidos na **âmbito** no **definições** secção.

    ![Aprovisionamento bonusly](./media/bonusly-provisioning-tutorial/ScopeSync.png)

16. Quando estiver pronto para aprovisionar, clique em **guardar**.

    ![Aprovisionamento bonusly](./media/bonusly-provisioning-tutorial/SaveProvisioning.png)


Esta operação inicia a sincronização inicial de todos os utilizadores e/ou grupos definidos no **âmbito** no **definições** secção. A sincronização inicial demora mais tempo a serem executados do que as sincronizações subsequentes, o que ocorrer aproximadamente a cada 40 minutos, desde que o serviço de aprovisionamento do AD do Azure está em execução. Pode utilizar o **detalhes de sincronização** secção para monitorizar o progresso e siga as ligações para o relatório de atividade, que descreve todas as ações executadas pelo Azure AD no Bonusly do serviço de aprovisionamento de aprovisionamento.

Para obter mais informações sobre como ler o registos de aprovisionamento do AD do Azure, consulte [relatórios sobre o aprovisionamento de contas de utilizadores automático](../active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Gerir o aprovisionamento da conta de utilizador para aplicações empresariais](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)


## <a name="next-steps"></a>Passos Seguintes

* [Saiba como rever os registos e obter relatórios de atividade de aprovisionamento](../active-directory-saas-provisioning-reporting.md)

<!--Image references-->
[1]: ./media/bonusly-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/bonusly-provisioning-tutorial/tutorial_general_02.png
[3]: ./media/bonusly-provisioning-tutorial/tutorial_general_03.png
