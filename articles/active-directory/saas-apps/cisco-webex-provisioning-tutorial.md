---
title: 'Tutorial: Configurar Cisco para aprovisionamento de utilizadores automática no Azure Active Directory | Microsoft Docs'
description: Saiba como configurar o Azure Active Directory para aprovisionar e anular o aprovisionamento contas de utilizador para Cisco Webex automaticamente.
services: active-directory
documentationcenter: ''
author: zhchia
writer: zhchia
manager: beatrizd
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/12/2018
ms.author: v-wingf
ms.openlocfilehash: fdaf77e3d8a1858372298fb0d67ca05c2717adf6
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/22/2018
ms.locfileid: "36321151"
---
# <a name="tutorial-configure-cisco-webex-for-automatic-user-provisioning"></a>Tutorial: Configurar Cisco Webex para aprovisionamento de utilizadores automática


O objetivo deste tutorial é demonstrar os passos para ser efetuada no Cisco Webex e Azure Active Directory (Azure AD) para configurar o Azure AD para aprovisionar e anular o aprovisionamento que os utilizadores Cisco Webex automaticamente.


> [!NOTE]
> Este tutorial descreve um conector desenvolvido com o serviço de fornecimento de utilizador do Azure AD. Para obter detalhes importantes sobre o que faz este serviço, como funciona e perguntas mais frequentes, consulte [automatizar utilizador aprovisionamento e desaprovisionamento para aplicações de SaaS no Azure Active Directory](../active-directory-saas-app-provisioning.md).

## <a name="prerequisites"></a>Pré-requisitos

O cenário descrito neste tutorial assume que já tem os seguintes pré-requisitos:

*   Um inquilino do Azure AD
*   Um inquilino Cisco Webex
*   Uma conta de utilizador no Cisco Webex com permissões de administrador


> [!NOTE]
> O Azure AD aprovisionamento integração depende o [Cisco Webex Webservice](https://developer.webex.com/getting-started.html), que está disponível para as equipas de Cisco Webex.

## <a name="adding-cisco-webex-from-the-gallery"></a>A adição de Cisco Webex de galeria
Antes de configurar Cisco Webex para aprovisionamento de utilizadores automática com o Azure AD, tem de adicionar Cisco Webex da Galeria de aplicações do Azure AD à sua lista de aplicações de SaaS geridas.

**Para adicionar Cisco Webex da Galeria de aplicações do Azure AD, execute os seguintes passos:**

1. No  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em de **do Azure Active Directory** ícone.

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais** > **todas as aplicações**.

    ![As secção de aplicações da empresa][2]

3. Para adicionar Cisco Webex, clique o **nova aplicação** botão na parte superior da caixa de diálogo.

    ![O novo botão de aplicação][3]

4. Na caixa de pesquisa, escreva **Cisco Webex**.

    ![Aprovisionamento de Webex Cisco](./media/cisco-webex-provisioning-tutorial/AppSearch.png)

5. No painel de resultados, selecione **Cisco Webex**e, em seguida, clique em de **adicionar** botão Adicionar Cisco Webex à sua lista de aplicações SaaS.

    ![Aprovisionamento de Webex Cisco](./media/cisco-webex-provisioning-tutorial/AppSearchResults.png)

    ![Aprovisionamento de Webex Cisco](./media/cisco-webex-provisioning-tutorial/AppCreation.png)

## <a name="assigning-users-to-cisco-webex"></a>Atribuir utilizadores a Cisco Webex

Azure Active Directory utiliza um conceito chamado "atribuições de" para determinar quais os utilizadores devem receber acesso às aplicações selecionadas. No contexto de aprovisionamento de utilizadores automático, são sincronizados apenas a utilizadores e/ou grupos que tenham sido "atribuídos" a uma aplicação no Azure AD.

Antes de configurar e ativar o aprovisionamento de utilizadores automática, deve decidir quais os utilizadores no Azure AD necessitam de aceder a Cisco Webex. Depois de decidir, pode atribuir estes utilizadores a Cisco Webex ao seguir as instruções aqui:

*   [Atribuir um utilizador ou grupo a uma aplicação empresarial](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-cisco-webex"></a>Sugestões importantes para atribuir utilizadores a Cisco Webex

*   Recomenda-se que um único utilizador do Azure AD é atribuído a Cisco Webex para testar a configuração de aprovisionamento de utilizadores automática. Os utilizadores adicionais podem ser atribuídos mais tarde.

*   Ao atribuir um utilizador a Cisco Webex, tem de selecionar uma função de específicas da aplicação válida (se disponível) na caixa de diálogo atribuição. Os utilizadores com o **predefinido acesso** função foram excluídos de aprovisionamento.

## <a name="configuring-automatic-user-provisioning-to-cisco-webex"></a>Configurar o aprovisionamento de utilizadores automática para Cisco Webex

Esta secção descreve os passos para configurar o Azure AD aprovisionamento do serviço para criar, atualizar e desativar os utilizadores Webex Cisco, com base em atribuições de utilizador no Azure AD.


### <a name="to-configure-automatic-user-provisioning-for-cisco-webex-in-azure-ad"></a>Para configurar o aprovisionamento de utilizadores automática para Cisco Webex no Azure AD:


1. Iniciar sessão para o [portal do Azure](https://portal.azure.com) e navegue para **do Azure Active Directory > aplicações da empresa > todas as aplicações**.

2. Selecione Cisco Webex na sua lista de aplicações SaaS.

    ![Aprovisionamento de Webex Cisco](./media/cisco-webex-provisioning-tutorial/Successcenter2.png)

3. Selecione o **aprovisionamento** separador.

    ![Aprovisionamento de Webex Cisco](./media/cisco-webex-provisioning-tutorial/ProvisioningTab.png)

4. Definir o **modo de aprovisionamento** para **automática**.

    ![Aprovisionamento de Webex Cisco](./media/cisco-webex-provisioning-tutorial/ProvisioningCredentials.png)

5. Sob o **credenciais de administrador** secção, de entrada a **URL de inquilino**, e **segredo Token** da conta da sua Webex Cisco.

    *   No **URL de inquilino** campo, preencher o URL da API Cisco Webex SCIM para o seu inquilino, que assume a forma de `https://api.webex.com/v1/scim/[Tenant ID]/`, onde `[Tenant ID]` é uma cadeia de carateres alfanumérica, conforme descrito no passo 6.

    *   No **segredo Token** campo, preencher o Token do segredo conforme descrito no passo 6.

1. O **ID do inquilino** e **segredo Token** para sua Webex Cisco conta pode ser encontrada, iniciando sessão no [site de programador Cisco Webex](https://developer.webex.com/) com a sua conta de administrador. Uma vez registado no -
    * Vá para o [página Introdução](https://developer.webex.com/getting-started.html)
    * Desloque para baixo até o [secção autenticação](https://developer.webex.com/getting-started.html#authentication)
    ![Cisco Webex autenticação Token](./media/cisco-webex-provisioning-tutorial/SecretToken.png)
    * A cadeia alfanumérica na caixa é sua **segredo Token**. Copie este token para a área de transferência
    * Vá para o [página obter os meus próprios detalhes](https://developer.webex.com/endpoint-people-me-get.html)
        * Certifique-se de que modo de teste no
        * Escreva a palavra "Portador" seguido de um espaço e, em seguida, cole o Token do segredo o campo de autorização ![Cisco Webex autenticação Token](./media/cisco-webex-provisioning-tutorial/GetMyDetails.png)
        * Clique em executar
    * O texto da resposta à direita, a **ID do inquilino** aparece como "orgId":

    ```json
    {
        "id": "(...)",
        "emails": [
            "admin.user@contoso.com"
        ],
        "displayName": "John Smith",
        "nickName": "John",
        "orgId": "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
        (...)
    }
    ```

1. Após preencher os campos mostrados no passo 5, clique em **Testar ligação** para garantir que o Azure AD pode ligar ao Cisco Webex. Se a ligação falhar, certifique-se de que a conta de Cisco Webex possui permissões de administrador e tente novamente.

    ![Aprovisionamento de Webex Cisco](./media/cisco-webex-provisioning-tutorial/TestConnection.png)

8. No **correio eletrónico de notificação** campo, introduza o endereço de e-mail de uma pessoa ou grupo que deve receber as notificações de erro de aprovisionamento e marque a caixa de verificação - **enviar uma notificação por e-mail quando uma falha ocorre**.

    ![Aprovisionamento de Webex Cisco](./media/cisco-webex-provisioning-tutorial/EmailNotification.png)

9. Clique em **Guardar**.

10. Sob o **mapeamentos** secção, selecione **sincronizar do Azure Active Directory que os utilizadores Cisco Webex**.

    ![Aprovisionamento de Webex Cisco](./media/cisco-webex-provisioning-tutorial/UserMapping.png)

11. Reveja os atributos de utilizador que são sincronizados a partir do Azure AD e Webex Cisco no **atributo mapeamento** secção. Os atributos selecionados como **correspondência** propriedades são utilizadas para corresponder as contas de utilizador no Cisco Webex para operações de atualização. Selecione o **guardar** botão para consolidar as alterações.

    ![Aprovisionamento de Webex Cisco](./media/cisco-webex-provisioning-tutorial/UserMappingAttributes.png)

12. Para configurar filtros de âmbito, consulte as seguintes instruções fornecidas a [tutorial de filtro Scoping](../active-directory-saas-scoping-filters.md).

13. Para ativar o Azure AD aprovisionamento do serviço para Cisco Webex, altere o **estado de aprovisionamento** para **no** no **definições** secção.

    ![Aprovisionamento de Webex Cisco](./media/cisco-webex-provisioning-tutorial/ProvisioningStatus.png)

14. Definir utilizadores e/ou grupos que pretende que o aprovisionamento Cisco Webex ao escolher os valores pretendidos no **âmbito** no **definições** secção.

    ![Aprovisionamento de Webex Cisco](./media/cisco-webex-provisioning-tutorial/SyncScope.png)

15. Quando estiver pronto para aprovisionar, clique em **guardar**.

    ![Aprovisionamento de Webex Cisco](./media/cisco-webex-provisioning-tutorial/Save.png)


Esta operação inicia a sincronização inicial de todos os utilizadores e/ou grupos definidos no **âmbito** no **definições** secção. A sincronização inicial demora mais para efetuar a sincronizações subsequentes, o que ocorrer aproximadamente a cada 40 minutos, desde que o Azure AD aprovisionamento do serviço está em execução. Pode utilizar o **detalhes de sincronização** secção para monitorizar o progresso e siga as ligações de aprovisionamento do relatório de atividade, que descreve todas as ações efetuadas pelo Azure AD aprovisionamento do serviço no Cisco Webex.

Para obter mais informações sobre como ler o Azure AD, os registos de aprovisionamento, consulte [relatórios sobre o aprovisionamento da conta de utilizador automáticas](../active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Gerir o aprovisionamento da conta de utilizador para aplicações da empresa](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)


## <a name="next-steps"></a>Passos Seguintes

* [Saiba como rever os registos e obter relatórios sobre o aprovisionamento de atividade](../active-directory-saas-provisioning-reporting.md)

<!--Image references-->
[1]: ./media/cisco-webex-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/cisco-webex-provisioning-tutorial/tutorial_general_02.png
[3]: ./media/cisco-webex-provisioning-tutorial/tutorial_general_03.png