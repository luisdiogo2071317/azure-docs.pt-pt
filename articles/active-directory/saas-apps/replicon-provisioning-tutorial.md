---
title: 'Tutorial: Configurar Replicon para aprovisionamento de utilizadores automática no Azure Active Directory | Microsoft Docs'
description: Saiba como configurar o Azure Active Directory para aprovisionar e anular o aprovisionamento contas de utilizador para Replicon automaticamente.
services: active-directory
documentationcenter: ''
author: zhchia
writer: zhchia
manager: beatrizd-msft
ms.assetid: na
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/20/2018
ms.author: v-ant
ms.openlocfilehash: 0f71c449e2fb0ab8b26ee9ba68850cbf7fe03b28
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36214751"
---
# <a name="tutorial-configure-replicon-for-automatic-user-provisioning"></a>Tutorial: Configurar Replicon para aprovisionamento de utilizadores automática

O objetivo deste tutorial é demonstrar os passos para ser efetuada no Replicon e Azure Active Directory (Azure AD) para configurar o Azure AD para aprovisionar automaticamente e anular o aprovisionamento utilizadores e/ou grupos para Replicon.

> [!NOTE]
> Este tutorial descreve um conector desenvolvido com o serviço de fornecimento de utilizador do Azure AD. Para obter detalhes importantes sobre o que faz este serviço, como funciona e perguntas mais frequentes, consulte [automatizar utilizador aprovisionamento e desaprovisionamento para aplicações de SaaS no Azure Active Directory](./../active-directory-saas-app-provisioning.md).

## <a name="prerequisites"></a>Pré-requisitos

O cenário descrito neste tutorial assume que já tem os seguintes itens:

*   Um inquilino do Azure AD
*   Um inquilino Replicon com o [Plus](https://www.replicon.com/time-bill-pricing/) planear ou melhor ativado
*   Uma conta de utilizador no Replicon com permissões de administrador

> [!NOTE]
> O Azure AD aprovisionamento integração depende o [Replicon API](https://www.replicon.com/help/developers), que está disponível para as equipas de Replicon no sinal de adição planear ou uma melhor.

## <a name="adding-replicon-from-the-gallery"></a>Adicionar Replicon a partir da Galeria
Antes de configurar Replicon para aprovisionamento de utilizadores automática com o Azure AD, tem de adicionar Replicon da Galeria de aplicações do Azure AD à sua lista de aplicações de SaaS geridas.

**Para adicionar Replicon da Galeria de aplicações do Azure AD, execute os seguintes passos:**

1. No  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em de **do Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais** > **todas as aplicações**.

    ![As secção de aplicações da empresa][2]
    
3. Para adicionar Replicon, clique o **nova aplicação** botão na parte superior da caixa de diálogo.

    ![O novo botão de aplicação][3]

4. Na caixa de pesquisa, escreva **Replicon**.

    ![Pesquisa de aplicação replicon](./media/replicon-provisioning-tutorial/RepliconAppSearch.png)

5. No painel de resultados, selecione **Replicon**e, em seguida, clique em de **adicionar** botão Adicionar Replicon à sua lista de aplicações SaaS.

    ![Resultados da pesquisa replicon](./media/replicon-provisioning-tutorial/RepliconAppSearchResults.png)

    ![Replicon Criar aplicação](./media/replicon-provisioning-tutorial/RepliconAppCreate.png)


## <a name="assigning-users-to-replicon"></a>Atribuir utilizadores a Replicon

Azure Active Directory utiliza um conceito chamado "atribuições de" para determinar quais os utilizadores devem receber acesso às aplicações selecionadas. No contexto de aprovisionamento de utilizadores automático, são sincronizados apenas a utilizadores e/ou grupos que tenham sido "atribuídos" a uma aplicação no Azure AD.

Antes de configurar e ativar o aprovisionamento de utilizadores automática, deve decidir quais os utilizadores e/ou grupos no Azure AD necessitam de aceder a Replicon. Depois de decidir, pode atribuir estes utilizadores e/ou grupos a Replicon ao seguir as instruções aqui:

*   [Atribuir um utilizador ou grupo a uma aplicação empresarial](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-replicon"></a>Sugestões importantes para atribuir utilizadores a Replicon

*   Recomenda-se que um único utilizador do Azure AD é atribuído a Replicon para testar a configuração de aprovisionamento de utilizadores automática. Os utilizadores adicionais e/ou grupos podem ser atribuídos mais tarde.

*   Ao atribuir um utilizador a Replicon, tem de selecionar uma função de específicas da aplicação válida (se disponível) na caixa de diálogo atribuição. Os utilizadores com o **predefinido acesso** função foram excluídos de aprovisionamento.

## <a name="configuring-automatic-user-provisioning-to-replicon"></a>Configurar o aprovisionamento de utilizadores automática para Replicon 

Esta secção descreve os passos para configurar o Azure AD aprovisionamento do serviço para criar, atualizar e desativar os utilizadores e/ou grupos no Replicon com base em atribuições de utilizador e/ou grupo no Azure AD.

> [!TIP]
> Também pode optar por ativar baseados em SAML-início de sessão único para Replicon, seguindo as instruções fornecidas a [Replicon único início de sessão tutorial](replicon-tutorial.md). O início de sessão único a pode ser configurado independentemente do aprovisionamento de utilizadores automática, apesar destas duas funcionalidades complementar dos entre si.

### <a name="to-configure-automatic-user-provisioning-for-replicon-in-azure-ad"></a>Para configurar o aprovisionamento de utilizadores automática para Replicon no Azure AD:

1. Iniciar sessão para o [portal do Azure](https://portal.azure.com) e navegue para **do Azure Active Directory > aplicações da empresa > todas as aplicações**.

2. Selecione Replicon na sua lista de aplicações SaaS.

    ![Aprovisionamento de replicon](./media/replicon-provisioning-tutorial/Replicon2.png)

3. Selecione o **aprovisionamento** separador.

    ![Aprovisionamento de replicon](./media/replicon-provisioning-tutorial/RepliconProvisioningTab.png)

4. Definir o **modo de aprovisionamento** para **automática**.

    ![Aprovisionamento de replicon](./media/replicon-provisioning-tutorial/Replicon1.png)

5. Sob o **credenciais de administrador** secção, introduza o **nome de utilizador de Admin**, **palavra-passe de administrador**, **: % CompanyId**, e **domínio**  da conta da sua Replicon. Exemplos destes valores são:

    *   No **nome de utilizador de Admin** campo, preencher o nome de utilizador da conta de administrador no seu inquilino Replicon. Exemplo: contosoadmin.

    *   No **palavra-passe de administrador** campo, preencher a palavra-passe correspondente ao nome de utilizador administrador.

    *   No **CompanyId** campo, preencher CompanyId do seu inquilino Replicon. Exemplo: CompanyID com base na entrada de registo abaixo é Contoso.

    ![Replicon início de sessão](./media/replicon-provisioning-tutorial/RepliconLogin.png)

    *   No **domínio** campo, preencher o domínio, tal como descrito no passo 6.
    
6. Obter **serviceEndpointRootURL** para sua Replicon inquilino conta com base nos passos mencionados [Replicon serviço ajudar](https://www.replicon.com/help/determining-the-url-for-your-service-calls). Após obter o URL, a **domínio** seria o subdomínio **serviceEndpointRootURL** como realçado. 

    ![Aprovisionamento de replicon](./media/replicon-provisioning-tutorial/RepliconEndpoint.png)

7. Após preencher os campos mostrados no passo 5, clique em **Testar ligação** para garantir que o Azure AD pode ligar ao Replicon. Se a ligação falhar, certifique-se de que a conta de Replicon possui permissões de administrador e tente novamente.

    ![Aprovisionamento de replicon](./media/replicon-provisioning-tutorial/RepliconTestConnection.png)

8. No **correio eletrónico de notificação** campo, introduza o endereço de e-mail de uma pessoa ou grupo que deve receber as notificações de erro de aprovisionamento e marque a caixa de verificação **enviar uma notificação por e-mail quando uma falha ocorre**.

    ![Aprovisionamento de replicon](./media/replicon-provisioning-tutorial/RepliconNotificationEmail.png)

9. Clique em **Guardar**.

10. Sob o **mapeamentos** secção, selecione **sincronizar utilizadores do Azure Active Directory para Replicon**.
    
    ![Aprovisionamento de replicon](./media/replicon-provisioning-tutorial/RepliconUserMapping.png)

11. Reveja os atributos de utilizador que são sincronizados a partir do Azure AD e Replicon no **atributo mapeamento** secção. Os atributos selecionados como **correspondência** propriedades são utilizadas para corresponder as contas de utilizador no Replicon para operações de atualização. Selecione o **guardar** botão para consolidar as alterações.

    ![Aprovisionamento de replicon](./media/replicon-provisioning-tutorial/RepliconUserMappingAtrributes.png)

12. Para configurar filtros de âmbito, consulte as seguintes instruções fornecidas a [tutorial de filtro Scoping](../active-directory-saas-scoping-filters.md).

13. Para ativar o Azure AD aprovisionamento do serviço para Replicon, altere o **estado de aprovisionamento** para **no** no **definições** secção.

    ![Aprovisionamento de replicon](./media/replicon-provisioning-tutorial/RepliconProvisioningStatus.png)

14. Definir utilizadores e/ou grupos que pretende que o aprovisionamento Replicon ao escolher os valores pretendidos no **âmbito** no **definições** secção.

    ![Aprovisionamento de replicon](./media/replicon-provisioning-tutorial/UserGroupSelection.png)

15. Quando estiver pronto para aprovisionar, clique em **guardar**.

    ![Aprovisionamento de replicon](./media/replicon-provisioning-tutorial/SaveProvisioning.png)

Esta operação inicia a sincronização inicial de todos os utilizadores e/ou grupos definidos no **âmbito** no **definições** secção. A sincronização inicial demora mais para efetuar a sincronizações subsequentes, o que ocorrer aproximadamente a cada 40 minutos, desde que o Azure AD aprovisionamento do serviço está em execução. Pode utilizar o **detalhes de sincronização** secção para monitorizar o progresso e siga as ligações para o relatório de atividade, que descreve todas as ações efetuadas pelo Azure AD aprovisionamento do serviço no Replicon de aprovisionamento.

Para obter mais informações sobre como ler o Azure AD, os registos de aprovisionamento, consulte [relatórios sobre o aprovisionamento da conta de utilizador automáticas](../active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Gerir o aprovisionamento da conta de utilizador para aplicações da empresa](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Passos Seguintes

* [Saiba como rever os registos e obter relatórios sobre o aprovisionamento de atividade](../active-directory-saas-provisioning-reporting.md)

<!--Image references-->
[1]: ./media/replicon-tutorial/tutorial_general_01.png
[2]: ./media/replicon-tutorial/tutorial_general_02.png
[3]: ./media/replicon-tutorial/tutorial_general_03.png
