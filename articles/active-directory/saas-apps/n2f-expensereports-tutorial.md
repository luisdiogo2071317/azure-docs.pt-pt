---
title: 'Tutorial: Relatórios de despesas integração do Azure Active Directory com N2F - | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e N2F - relatórios de despesas.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: f56d53d7-5a08-490a-bfb9-78fefc2751ec
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2018
ms.author: jeedes
ms.openlocfilehash: 27fb299bc3bbbbf75bdf40ae02eac627763ce6d4
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/09/2018
ms.locfileid: "40006649"
---
# <a name="tutorial-azure-active-directory-integration-with-n2f---expense-reports"></a>Tutorial: Integração do Azure Active Directory com N2F - despesas relatórios

Neste tutorial, saiba como integrar N2F - relatórios de despesas no Azure Active Directory (Azure AD).

Integrando N2F - relatórios de despesas com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao N2F - relatórios de despesas.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para N2F - relatórios de despesas (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com N2F - relatórios de despesas, terá dos seguintes itens:

- Uma subscrição do Azure
- Um N2F - relatórios de despesas de início de sessão de subscrição de ativado individual

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionar N2F - relatórios de despesas da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-n2f---expense-reports-from-the-gallery"></a>Adicionar N2F - relatórios de despesas da Galeria

Para configurar a integração do N2F - relatórios de despesas no Azure AD, tem de adicionar N2F - relatórios a partir da Galeria à sua lista de aplicações de SaaS geridas de despesas.

**Para adicionar N2F - relatórios de despesas a partir da galeria, execute os seguintes passos:**

1. Na ** [portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]

3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

4. Na caixa de pesquisa, escreva **N2F - relatórios de despesas**, selecione **N2F - relatórios de despesas** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![N2F - relatórios de despesas na lista de resultados](./media/n2f-expensereports-tutorial/tutorial_n2f-expensereports_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com N2F - relatórios de despesas com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único trabalhar, o Azure AD precisa de saber o que o utilizador de contraparte N2F - relatórios de despesas for para um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no N2F - despesas relatórios tem de ser estabelecida.

Para configurar e testar o Azure AD início de sessão único com N2F - relatórios de despesas, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on) ** - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user) ** - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um N2F - utilizador de teste de relatórios de despesas](#create-a-n2f---expense-reports-test-use) ** - para ter um equivalente da Eduarda Almeida na N2F - despesas relatórios que está ligada à representação de utilizador do Azure AD.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user) ** - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Testar início de sessão único](#test-single-sign-on) ** - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único na sua N2F - aplicação de relatórios de despesas.

**Para configurar o Azure AD início de sessão único com N2F - relatórios de despesas, execute os seguintes passos:**

1. No portal do Azure, sobre o **N2F - relatórios de despesas** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.

    ![Caixa de diálogo de início de sessão único](./media/n2f-expensereports-tutorial/tutorial_n2f-expensereports_samlbase.png)

3. Sobre o **N2F - relatórios de despesas de domínio e URLs** secção, se desejar configurar a aplicação no **IDP** modo iniciado, o utilizador não tem de efetuar outros passos de como a aplicação já está pré-integrado com Azure.

    ![Informações de início de sessão de único N2F - relatórios de despesas, domínio e URLs](./media/n2f-expensereports-tutorial/tutorial_n2f-expensereports_url1.png)

4. Verifique **Mostrar definições de URL avançadas** e executar o passo seguinte, se desejar configurar a aplicação na **SP** iniciada pelo modo:

    ![Informações de início de sessão de único N2F - relatórios de despesas, domínio e URLs](./media/n2f-expensereports-tutorial/tutorial_n2f-expensereports_url2.png)

    Na **URL de início de sessão** caixa de texto, escreva o URL: `https://www.n2f.com/app/`

5. Sobre o **certificado de assinatura SAML** secção, clique no botão de cópia para copiar **Url de metadados de Federação de aplicação** e cole-o no bloco de notas.

    ![O link de download de certificado](./media/n2f-expensereports-tutorial/tutorial_n2f-expensereports_certificate.png)

6. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/n2f-expensereports-tutorial/tutorial_general_400.png)

7. Sobre o **N2F - configuração de relatórios de despesas** secção, clique em **configurar N2F - relatórios de despesas** para abrir **configurar início de sessão** janela. Cópia a **ID de entidade de SAML** partir o **secção de referência rápida.**

    ![N2F - configuração de relatórios de despesas](./media/n2f-expensereports-tutorial/tutorial_n2f-expensereports_configure.png)

8. Numa janela do browser web diferente, inicie sessão no seu N2F - site de empresa de relatórios de despesas como administrador.

9. Clique em **configurações** e, em seguida, selecione **definições avançadas** na lista pendente.

    ![N2F - configuração de relatórios de despesas](./media/n2f-expensereports-tutorial/configure1.png)

10. Selecione **definições da conta** separador.

    ![N2F - configuração de relatórios de despesas](./media/n2f-expensereports-tutorial/configure2.png)

11. Selecione **autenticação** e, em seguida, selecione **+ adicionar um método de autenticação** separador.

    ![N2F - configuração de relatórios de despesas](./media/n2f-expensereports-tutorial/configure3.png)

12. Selecione **SAML Microsoft Office 365** como método de autenticação.

    ![N2F - configuração de relatórios de despesas](./media/n2f-expensereports-tutorial/configure4.png)

13. Sobre o **método de autenticação** secção, execute os seguintes passos:

    ![N2F - configuração de relatórios de despesas](./media/n2f-expensereports-tutorial/configure5.png)

    a. Na **ID de entidade** caixa de texto, colar a **ID de entidade de SAML** valor, que copiou do portal do Azure.

    b. Na **URL de metadados** caixa de texto, colar a **Url de metadados de Federação de aplicação** valor, que copiou do portal do Azure.

    c. Clique em **Guardar**.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/n2f-expensereports-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/n2f-expensereports-tutorial/create_aaduser_02.png)

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/n2f-expensereports-tutorial/create_aaduser_03.png)

4. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/n2f-expensereports-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.

### <a name="create-a-n2f---expense-reports-test-user"></a>Criar um N2F - utilizador de teste de relatórios de despesas

Para permitir que utilizadores do Azure AD iniciar sessão no N2F - relatórios de despesas, eles têm de ser aprovisionados em N2F - relatórios de despesas. No caso de N2F - relatórios de despesas, o aprovisionamento é uma tarefa manual.

**Para Aprovisionar uma conta de utilizador, execute os seguintes passos:**

1. Inicie sessão na sua N2F - site de empresa de relatórios de despesas como administrador.

2. Clique em **configurações** e, em seguida, selecione **definições avançadas** na lista pendente.

   ![N2F - despesas adicionar utilizador](./media/n2f-expensereports-tutorial/configure1.png)

3. Selecione **utilizadores** separador do painel de navegação à esquerda.

    ![N2F - configuração de relatórios de despesas](./media/n2f-expensereports-tutorial/user1.png)

4. Selecione **+ novo utilizador** separador.

   ![N2F - configuração de relatórios de despesas](./media/n2f-expensereports-tutorial/user2.png)

5. Sobre o **utilizador** secção, execute os seguintes passos:

    ![N2F - configuração de relatórios de despesas](./media/n2f-expensereports-tutorial/user3.png)

    a. Na **endereço de E-Mail** caixa de texto, introduza o endereço de e-mail do utilizador, como ** brittasimon@contoso.com **.

    b. Na **nome próprio** caixa de texto, introduza o nome de utilizador, como **Eduarda**.

    c. Na **Name** caixa de texto, introduza o nome de utilizador, como **BrittaSimon**.

    d. Escolher **função de Gestor direto (N + 1)**, e **divisão** de acordo com seus requisitos de organização.

    e. Clique em **para validar e enviar convite**.

    > [!NOTE]
    > Se estiver com problemas ao adicionar o utilizador, entre em contato com [N2F - equipa de suporte de relatórios de despesas](mailto:support@n2f.com)

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, ativar a Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para N2F - relatórios de despesas.

![Atribuir a função de utilizador][200]

**Para atribuir a Eduarda Almeida a N2F - relatórios de despesas, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201]

2. Na lista de aplicações, selecione **N2F - relatórios de despesas**.

    ![Relatórios de despesas de N2F - link na lista de aplicações](./media/n2f-expensereports-tutorial/tutorial_n2f-expensereports_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.

### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clicar o N2F - mosaico de relatórios de despesas no painel de acesso, deve obter automaticamente com sessão iniciada para sua N2F - aplicação de relatórios de despesas.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[1]: ./media/n2f-expensereports-tutorial/tutorial_general_01.png
[2]: ./media/n2f-expensereports-tutorial/tutorial_general_02.png
[3]: ./media/n2f-expensereports-tutorial/tutorial_general_03.png
[4]: ./media/n2f-expensereports-tutorial/tutorial_general_04.png

[100]: ./media/n2f-expensereports-tutorial/tutorial_general_100.png

[200]: ./media/n2f-expensereports-tutorial/tutorial_general_200.png
[201]: ./media/n2f-expensereports-tutorial/tutorial_general_201.png
[202]: ./media/n2f-expensereports-tutorial/tutorial_general_202.png
[203]: ./media/n2f-expensereports-tutorial/tutorial_general_203.png

