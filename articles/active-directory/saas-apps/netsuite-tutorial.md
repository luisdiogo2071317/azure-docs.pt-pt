---
title: 'Tutorial: Integração do Azure Active Directory com o NetSuite | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e o NetSuite.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: dafa0864-aef2-4f5e-9eac-770504688ef4
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2018
ms.author: jeedes
ms.openlocfilehash: 5a20af1130d50209b29ad44195c14f30cba30c43
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39051858"
---
# <a name="tutorial-azure-active-directory-integration-with-netsuite"></a>Tutorial: Integração do Azure Active Directory com o NetSuite

Neste tutorial, saiba como integrar o NetSuite com o Azure Active Directory (Azure AD).

Integrar o NetSuite no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao NetSuite
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para o NetSuite (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o NetSuite, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um NetSuite logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando o NetSuite da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-netsuite-from-the-gallery"></a>Adicionando o NetSuite da Galeria
Para configurar a integração entre o NetSuite com o Azure AD, terá de adicionar o NetSuite a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar o NetSuite a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]

3. Clique em **nova aplicação** botão na parte superior da caixa de diálogo.

    ![Aplicações][3]

4. Na caixa de pesquisa, escreva **NetSuite**, selecione **NetSuite** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![NetSuite na lista de resultados](./media/netsuite-tutorial/tutorial_netsuite_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com o NetSuite com base num utilizador de teste chamado "Eduarda Almeida."

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no NetSuite a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no NetSuite deve ser estabelecido.

Esta relação de ligação é estabelecida atribuindo o valor do **nome de utilizador** no Azure AD como o valor do **Username** no NetSuite.

Para configurar e testar o Azure AD início de sessão único com o NetSuite, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste NetSuite](#creating-a-netsuite-test-user)**  - para ter um equivalente da Eduarda Almeida na NetSuite que está ligado à representação de utilizador do Azure AD.
4. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo NetSuite.

**Para configurar o Azure AD início de sessão único com o NetSuite, execute os seguintes passos:**

1. No portal do Azure, sobre o **NetSuite** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.

    ![Configurar o início de sessão único](./media/NetSuite-tutorial/tutorial_NetSuite_samlbase.png)

3. Sobre o **NetSuite domínio e URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/NetSuite-tutorial/tutorial_NetSuite_url.png)

    Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão:

    `https://<tenant-name>.NetSuite.com/saml2/acs`

    `https://<tenant-name>.na1.NetSuite.com/saml2/acs`

    `https://<tenant-name>.na2.NetSuite.com/saml2/acs`

    `https://<tenant-name>.sandbox.NetSuite.com/saml2/acs`

    `https://<tenant-name>.na1.sandbox.NetSuite.com/saml2/acs`

    `https://<tenant-name>.na2.sandbox.NetSuite.com/saml2/acs`
    
    > [!NOTE]
    > Essas não são valores reais. Atualize estes valores com o URL de resposta real. Contacte [equipa de suporte de NetSuite](http://www.NetSuite.com/portal/services/support.shtml) obter esses valores.

4. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro XML em seu computador.

    ![Configurar o início de sessão único](./media/NetSuite-tutorial/tutorial_NetSuite_certificate.png) 

5. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/NetSuite-tutorial/tutorial_general_400.png)

6. Na **NetSuite Configuration** secção, clique em **configurar o NetSuite** para abrir **configurar início de sessão** janela. Cópia a **SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configurar o início de sessão único](./media/NetSuite-tutorial/tutorial_NetSuite_configure.png)

7. Abra um novo separador no seu browser e inicie sessão no site da sua empresa NetSuite como administrador.

8. Na barra de ferramentas na parte superior da página, clique em **programa de configuração**, em seguida, navegue até à **empresa** e clique em **ativar funcionalidades**.

    ![Configurar o início de sessão único](./media/NetSuite-tutorial/ns-setupsaml.png)

9. Na barra de ferramentas no meio da página, clique em **SuiteCloud**.

    ![Configurar o início de sessão único](./media/NetSuite-tutorial/ns-suitecloud.png)

10. Sob **Gerir autenticação** secção, selecione **-Diante de início de sessão único SAML** para ativar a opção de início de sessão único SAML-no NetSuite.

    ![Configurar o início de sessão único](./media/NetSuite-tutorial/ns-ticksaml.png)

11. Na barra de ferramentas na parte superior da página, clique em **configuração**.

    ![Configurar o início de sessão único](./media/NetSuite-tutorial/ns-setup.png)

12. Partir do **tarefas de configuração** , clique em **integração**.

    ![Configurar o início de sessão único](./media/NetSuite-tutorial/ns-integration.png)

13. Na **GERIR autenticação** secção, clique em **o início de sessão único SAML**.

    ![Configurar o início de sessão único](./media/NetSuite-tutorial/ns-saml.png)

14. Sobre o **configuração de SAML** página, em **NetSuite configuração** secção execute os seguintes passos:

    ![Configurar o início de sessão único](./media/NetSuite-tutorial/ns-saml-setup.png)
  
    a. Selecione **método de autenticação primária**.

    b. Para o campo rotulado **METADADOS de fornecedor de identidade SAMLV2**, selecione **carregar ficheiro de METADADOS de IDP**. Em seguida, clique em **procurar** para carregar o ficheiro de metadados que transferiu a partir do portal do Azure.

    c. Clique em **submeter**.

15. No Azure AD, clique em **ver e editar todos os outros atributos de utilizador** caixa de verificação e adicione o atributo.

    ![Configurar o início de sessão único](./media/NetSuite-tutorial/ns-attributes.png)

16. Para o **nome do atributo** , digite no `account`. Para o **valor do atributo** , digite no seu ID de conta NetSuite. Este valor é constante e de alterações com a conta. As instruções sobre como encontrar o ID da conta estão incluídas abaixo:

    ![Configurar o início de sessão único](./media/NetSuite-tutorial/ns-add-attribute.png)

    a. No NetSuite, clique em **programa de configuração** , em seguida, navegue até à **empresa** e clique em **informações da empresa** no menu de navegação superior.

    ![Configurar o início de sessão único](./media/NetSuite-tutorial/ns-com.png)

    ![Configurar o início de sessão único](./media/NetSuite-tutorial/ns-account-id.png)

    b. Na **informações da empresa** página sobre a cópia de coluna da direita a **ID de conta**.

    c. Colar o **ID da conta** que copiou da conta de NetSuite-lo para o **o valor do atributo** campo no Azure AD. 

17. Antes dos utilizadores podem efetuar o início de sessão único para o NetSuite, eles tem primeiro de atribuir as permissões adequadas no NetSuite. Siga as instruções abaixo para atribuir estas permissões.

    a. No menu de navegação superior, clique em **configuração**.

    ![Configurar o início de sessão único](./media/NetSuite-tutorial/ns-setup.png)

    b. No menu de navegação esquerdo, selecione **os utilizadores/funções**, em seguida, clique em **gerir funções**.

    ![Configurar o início de sessão único](./media/NetSuite-tutorial/ns-manage-roles.png)

    c. Clique em **nova função**.

    d. Escreva um **nome** para a sua nova função.

    ![Configurar o início de sessão único](./media/NetSuite-tutorial/ns-new-role.png)

    e. Clique em **Guardar**.

    f. No menu na parte superior, clique em **permissões**. Em seguida, clique em **configuração**.

    ![Configurar o início de sessão único](./media/NetSuite-tutorial/ns-sso.png)

    g. Selecione **o início de sessão único SAML**e, em seguida, clique em **Add**.

    h. Clique em **Guardar**.

    i. No menu de navegação superior, clique em **programa de configuração**, em seguida, clique em **Gestor de configuração**.

    ![Configurar o início de sessão único](./media/NetSuite-tutorial/ns-setup.png)

    j. No menu de navegação esquerdo, selecione **os utilizadores/funções**, em seguida, clique em **Manage Users**.

    ![Configurar o início de sessão único](./media/NetSuite-tutorial/ns-manage-users.png)

    k. Selecione um utilizador de teste. Em seguida, clique em **edite** e, em seguida, navegue até **acesso** separador.

    ![Configurar o início de sessão único](./media/NetSuite-tutorial/ns-edit-user.png)

    l. Na caixa de diálogo Funções, atribua a função adequada que tenha criado.

    ![Configurar o início de sessão único](./media/NetSuite-tutorial/ns-add-role.png)

    m. Clique em **Guardar**.
 
### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/NetSuite-tutorial/create_aaduser_01.png) 

2.  Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/NetSuite-tutorial/create_aaduser_02.png) 

3. Na parte superior da caixa de diálogo, clique em **Add** para abrir o **utilizador** caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/NetSuite-tutorial/create_aaduser_03.png) 

4. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/NetSuite-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**. 

### <a name="creating-a-netsuite-test-user"></a>Criar um utilizador de teste NetSuite

Nesta secção, um usuário chamado Eduarda Almeida é criado na NetSuite. O NetSuite suporta o aprovisionamento de just-in-time, que está ativado por predefinição.
Não existe nenhum item de ação para nesta secção. Se um utilizador já não existir no NetSuite, uma nova é criada quando tentar acessar o NetSuite.


### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único, concedendo acesso para o NetSuite.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida a NetSuite, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **NetSuite**.

    ![Configurar o início de sessão único](./media/NetSuite-tutorial/tutorial_NetSuite_app.png) 

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Para testar as definições de início de sessão únicas, abra o painel de acesso em [ https://myapps.microsoft.com ](https://myapps.microsoft.com/), a sessão na conta de teste e clique em **NetSuite**.

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)
* [Configurar o aprovisionamento do utilizador](NetSuite-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/NetSuite-tutorial/tutorial_general_01.png
[2]: ./media/NetSuite-tutorial/tutorial_general_02.png
[3]: ./media/NetSuite-tutorial/tutorial_general_03.png
[4]: ./media/NetSuite-tutorial/tutorial_general_04.png

[100]: ./media/NetSuite-tutorial/tutorial_general_100.png

[200]: ./media/NetSuite-tutorial/tutorial_general_200.png
[201]: ./media/NetSuite-tutorial/tutorial_general_201.png
[202]: ./media/NetSuite-tutorial/tutorial_general_202.png
[203]: ./media/NetSuite-tutorial/tutorial_general_203.png

