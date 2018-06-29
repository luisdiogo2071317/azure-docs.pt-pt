---
title: 'Tutorial: Configurar Cisco Spark para o aprovisionamento de utilizador automáticas com o Azure Active Directory | Microsoft Docs'
description: Saiba como configurar o Azure Active Directory para aprovisionar e anular o aprovisionamento contas de utilizador com o Spark Cisco automaticamente.
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
ms.openlocfilehash: 74907693270e6cd340d3b34585a80077aa87f0f7
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37059181"
---
# <a name="tutorial-configure-cisco-spark-for-automatic-user-provisioning"></a>Tutorial: Configurar Cisco Spark para o aprovisionamento de utilizador automáticas


O objetivo deste tutorial é demonstrar os passos para ser efetuada no Cisco Spark e Azure Active Directory (Azure AD) para configurar o Azure AD para aprovisionar e anular o aprovisionamento que os utilizadores Cisco Spark automaticamente.


> [!NOTE]
> Este tutorial descreve um conector desenvolvido com o serviço de fornecimento de utilizador do Azure AD. Para obter detalhes importantes sobre o que faz este serviço, como funciona e perguntas mais frequentes, consulte [automatizar utilizador aprovisionamento e desaprovisionamento para aplicações de SaaS no Azure Active Directory](./../active-directory-saas-app-provisioning.md).

## <a name="prerequisites"></a>Pré-requisitos

O cenário descrito neste tutorial assume que já tem os seguintes pré-requisitos:

*   Um inquilino do Azure AD
*   Um inquilino do Cisco Spark
*   Uma conta de utilizador no Cisco Spark com permissões de administrador


> [!NOTE]
> O Azure AD aprovisionamento integração depende o [Cisco Spark Webservice](https://developer.webex.com/getting-started.html), que está disponível para as equipas de Cisco Spark.

## <a name="adding-cisco-spark-from-the-gallery"></a>A adição de Cisco Spark de galeria
Antes de configurar Cisco Spark para o aprovisionamento de utilizador automáticas com o Azure AD, tem de adicionar Cisco Spark da Galeria de aplicações do Azure AD à sua lista de aplicações de SaaS geridas.

**Para adicionar Cisco Spark da Galeria de aplicações do Azure AD, execute os seguintes passos:**

1. No  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em de **do Azure Active Directory** ícone.

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais** > **todas as aplicações**.

    ![As secção de aplicações da empresa][2]

3. Para adicionar Cisco Spark, clique o **nova aplicação** botão na parte superior da caixa de diálogo.

    ![O novo botão de aplicação][3]

4. Na caixa de pesquisa, escreva **Cisco Spark**.

    ![Spark Cisco aprovisionamento](./media/cisco-spark-provisioning-tutorial/AppSearch.png)

5. No painel de resultados, selecione **Cisco Spark**e, em seguida, clique em de **adicionar** botão Adicionar Cisco Spark à sua lista de aplicações SaaS.

    ![Spark Cisco aprovisionamento](./media/cisco-spark-provisioning-tutorial/AppSearchResults.png)

    ![Spark Cisco aprovisionamento](./media/cisco-spark-provisioning-tutorial/AppCreation.png)

## <a name="assigning-users-to-cisco-spark"></a>Atribuir utilizadores a Cisco Spark

Azure Active Directory utiliza um conceito chamado "atribuições de" para determinar quais os utilizadores devem receber acesso às aplicações selecionadas. No contexto de aprovisionamento de utilizadores automático, são sincronizados apenas a utilizadores e/ou grupos que tenham sido "atribuídos" a uma aplicação no Azure AD.

Antes de configurar e ativar o aprovisionamento de utilizadores automática, deve decidir quais os utilizadores no Azure AD necessitam de aceder a Cisco Spark. Depois de decidir, pode atribuir estes utilizadores Cisco spark ao seguir as instruções aqui:

*   [Atribuir um utilizador ou grupo a uma aplicação empresarial](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-cisco-spark"></a>Sugestões importantes para atribuir utilizadores a Cisco Spark

*   Recomenda-se que um único utilizador do Azure AD é atribuído a Cisco Spark para testar a configuração de aprovisionamento de utilizadores automática. Os utilizadores adicionais podem ser atribuídos mais tarde.

*   Ao atribuir um utilizador a Cisco Spark, tem de selecionar uma função de específicas da aplicação válida (se disponível) na caixa de diálogo atribuição. Os utilizadores com o **predefinido acesso** função foram excluídos de aprovisionamento.

## <a name="configuring-automatic-user-provisioning-to-cisco-spark"></a>Configurar o aprovisionamento de utilizadores automática spark Cisco

Esta secção descreve os passos para configurar o Azure AD aprovisionamento do serviço para criar, atualizar e desativar os utilizadores no Spark Cisco, com base em atribuições de utilizador no Azure AD.


### <a name="to-configure-automatic-user-provisioning-for-cisco-spark-in-azure-ad"></a>Para configurar o aprovisionamento de utilizadores automática para Cisco Spark no Azure AD:


1. Iniciar sessão para o [portal do Azure](https://portal.azure.com) e navegue para **do Azure Active Directory > aplicações da empresa > todas as aplicações**.

2. Selecione o Spark Cisco na sua lista de aplicações SaaS.

    ![Spark Cisco aprovisionamento](./media/cisco-spark-provisioning-tutorial/Successcenter2.png)

3. Selecione o **aprovisionamento** separador.

    ![Spark Cisco aprovisionamento](./media/cisco-spark-provisioning-tutorial/ProvisioningTab.png)

4. Definir o **modo de aprovisionamento** para **automática**.

    ![Spark Cisco aprovisionamento](./media/cisco-spark-provisioning-tutorial/ProvisioningCredentials.png)

5. Sob o **credenciais de administrador** secção, de entrada a **URL de inquilino**, e **segredo Token** da conta do Spark o Cisco.

    *   No **URL de inquilino** campo, preencher o URL da API Cisco Spark SCIM para o seu inquilino, que assume a forma de `https://api.ciscospark.com/v1/scim/[Tenant ID]/`, onde `[Tenant ID]` é uma cadeia de carateres alfanumérica, conforme descrito no passo 6.

    *   No **segredo Token** campo, preencher o Token do segredo conforme descrito no passo 6.

1. O **ID do inquilino** e **segredo Token** para o Spark Cisco conta pode ser encontrada, iniciando sessão no [site de programador do Cisco Spark](https://developer.webex.com/) com a sua conta de administrador. Uma vez registado no -
    * Vá para o [página Introdução](https://developer.webex.com/getting-started.html)
    * Desloque para baixo até o [secção autenticação](https://developer.webex.com/getting-started.html#authentication)
    ![Token de autenticação de Spark do Cisco](./media/cisco-spark-provisioning-tutorial/SecretToken.png)
    * A cadeia alfanumérica na caixa é sua **segredo Token**. Copie este token para a área de transferência
    * Vá para o [página obter os meus próprios detalhes](https://developer.webex.com/endpoint-people-me-get.html)
        * Certifique-se de que modo de teste no
        * Escreva a palavra "Portador" seguido de um espaço e, em seguida, cole o Token do segredo o campo de autorização ![Token de autenticação de Spark do Cisco](./media/cisco-spark-provisioning-tutorial/GetMyDetails.png)
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

1. Após preencher os campos mostrados no passo 5, clique em **Testar ligação** para garantir que o Azure AD pode ligar ao Cisco Spark. Se a ligação falhar, certifique-se de que a conta de Cisco Spark possui permissões de administrador e tente novamente.

    ![Spark Cisco aprovisionamento](./media/cisco-spark-provisioning-tutorial/TestConnection.png)

8. No **correio eletrónico de notificação** campo, introduza o endereço de e-mail de uma pessoa ou grupo que deve receber as notificações de erro de aprovisionamento e marque a caixa de verificação - **enviar uma notificação por e-mail quando uma falha ocorre**.

    ![Spark Cisco aprovisionamento](./media/cisco-spark-provisioning-tutorial/EmailNotification.png)

9. Clique em **Guardar**.

10. Sob o **mapeamentos** secção, selecione **sincronizar utilizadores do Azure Active Directory Cisco spark**.

    ![Spark Cisco aprovisionamento](./media/cisco-spark-provisioning-tutorial/UserMapping.png)

11. Reveja os atributos de utilizador são sincronizados a partir do Azure AD para Cisco o Spark no **atributo mapeamento** secção. Os atributos selecionados como **correspondência** propriedades são utilizadas para corresponder as contas de utilizador no Cisco Spark para operações de atualização. Selecione o **guardar** botão para consolidar as alterações.

    ![Spark Cisco aprovisionamento](./media/cisco-spark-provisioning-tutorial/UserMappingAttributes.png)

12. Para configurar filtros de âmbito, consulte as seguintes instruções fornecidas a [tutorial de filtro Scoping](../active-directory-saas-scoping-filters.md).

13. Para ativar o Azure AD aprovisionamento do serviço para o Spark Cisco, altere o **estado de aprovisionamento** para **no** no **definições** secção.

    ![Spark Cisco aprovisionamento](./media/cisco-spark-provisioning-tutorial/ProvisioningStatus.png)

14. Definir utilizadores e/ou grupos que pretende que o aprovisionamento Cisco Spark ao escolher os valores pretendidos no **âmbito** no **definições** secção.

    ![Spark Cisco aprovisionamento](./media/cisco-spark-provisioning-tutorial/SyncScope.png)

15. Quando estiver pronto para aprovisionar, clique em **guardar**.

    ![Spark Cisco aprovisionamento](./media/cisco-spark-provisioning-tutorial/Save.png)


Esta operação inicia a sincronização inicial de todos os utilizadores e/ou grupos definidos no **âmbito** no **definições** secção. A sincronização inicial demora mais para efetuar a sincronizações subsequentes, o que ocorrer aproximadamente a cada 40 minutos, desde que o Azure AD aprovisionamento do serviço está em execução. Pode utilizar o **detalhes de sincronização** secção para monitorizar o progresso e siga as ligações de aprovisionamento do relatório de atividade, que descreve todas as ações efetuadas pelo Azure AD aprovisionamento do serviço no Cisco Spark.

Para obter mais informações sobre como ler o Azure AD, os registos de aprovisionamento, consulte [relatórios sobre o aprovisionamento da conta de utilizador automáticas](../active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Gerir o aprovisionamento da conta de utilizador para aplicações da empresa](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)


## <a name="next-steps"></a>Passos Seguintes

* [Saiba como rever os registos e obter relatórios sobre o aprovisionamento de atividade](../active-directory-saas-provisioning-reporting.md)

<!--Image references-->
[1]: ./media/cisco-spark-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/cisco-spark-provisioning-tutorial/tutorial_general_02.png
[3]: ./media/cisco-spark-provisioning-tutorial/tutorial_general_03.png