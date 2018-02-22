---
title: "Tutorial: Integração do Azure Active Directory com Remix do Gestor de vendas da I | Microsoft Docs"
description: "Saiba como configurar o início de sessão entre o Azure Active Directory e Remix do Gestor de vendas da I."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 89b5022c-0d5b-4103-9877-ddd32b6e1c02
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/21/2018
ms.author: jeedes
ms.openlocfilehash: 56c2860b6aeac9fdc66f2b1fdd1ed9126bf77d3f
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/22/2018
---
# <a name="tutorial-azure-active-directory-integration-with-e-sales-manager-remix"></a>Tutorial: Integração do Azure Active Directory com Remix do Gestor de vendas da i

Neste tutorial, saiba como integrar o i vendas Manager Remix com o Azure Active Directory (Azure AD).

Integrar o i vendas Manager Remix com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Remix do Gestor de vendas da I.
- Pode permitir aos utilizadores automaticamente obter com sessão iniciada para I Remix de Gestor de vendas (Single Sign-On) com as respetivas contas do Azure AD.
- Pode gerir as contas numa localização central - portal do Azure.

Se pretender saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [que é o acesso a aplicações e início de sessão no Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Remix do Gestor de vendas da I, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Remix de Gestor de vendas de I-início de sessão único ativada subscrição

> [!NOTE]
> Para testar os passos neste tutorial, não recomendamos a utilização num ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, teste do Azure AD-início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos modulares principais:

1. A adição de Remix do Gestor de vendas da i de galeria
2. Configurar e testar o Azure AD de sessão único-

## <a name="adding-e-sales-manager-remix-from-the-gallery"></a>A adição de Remix do Gestor de vendas da i de galeria
Para configurar a integração de Remix do Gestor de vendas da i com o Azure AD, tem de adicionar Remix do Gestor de vendas da i na Galeria à sua lista de aplicações SaaS geridas.

**Para adicionar o i vendas Manager Remix na galeria do, execute os seguintes passos:**

1. No  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **do Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todas as aplicações**.

    ![O painel de aplicações da empresa][2]
    
3. Para adicionar a nova aplicação, clique em **nova aplicação** botão no topo da caixa de diálogo.

    ![O novo botão de aplicação][3]

4. Na caixa de pesquisa, escreva **Remix do Gestor de vendas da i**, selecione **Remix do Gestor de vendas da i** partir do painel de resultados, em seguida, clique em **adicionar** botão para adicionar a aplicação.

    ![I Remix de Gestor de vendas na lista de resultados](./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD-início de sessão único

Nesta secção, configure e teste do Azure AD-início de sessão único com i vendas Manager Remix com base num utilizador de teste chamado "Britta Simon".

Para início de sessão trabalhar, do Azure AD tem de saber o que o utilizador homólogo Remix do Gestor de vendas da i é um utilizador no Azure AD. Por outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Remix do Gestor de vendas da i tem de ser estabelecida.

Para configurar e testar o Azure AD-início de sessão único com Remix do Gestor de vendas da I, tem de concluir os blocos modulares seguintes:

1. **[Configurar o Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)**  - para permitir aos utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD-início de sessão único com Britta Simon.
3. **[Criar um utilizador de teste Remix do Gestor de vendas da i](#create-an-e-sales-manager-remix-test-user)**  - para ter um homólogo de Britta Simon I vendas Manager Remix que está ligada a representação do Azure AD do utilizador.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar Britta Simon utilizar o Azure AD-início de sessão único.
5. **[Teste o início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD-início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão no portal do Azure e configurar o início de sessão único na sua aplicação Remix do Gestor de vendas da I.

**Para configurar o Azure AD-início de sessão único com Remix do Gestor de vendas da I, execute os seguintes passos:**

1. No portal do Azure, no **Remix do Gestor de vendas da i** página de integração de aplicações, clique em **de sessão único-**.

    ![Configurar a ligação de início de sessão único][4]

2. No **de sessão único-** caixa de diálogo, selecione **modo** como **baseados em SAML início de sessão** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_samlbase.png)

3. No **i vendas Manager Remix domínio e os URLs** secção, execute os seguintes passos:

    ![I vendas Manager Remix domínio e os URLs únicos de informações de início de sessão](./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_url.png)

    a. No **URL de início de sessão** caixa de texto, escreva um URL a utilizar o padrão do seguinte: `https://<Server-Based-URL>/<sub-domain>/esales-pc`

    b. No **identificador** caixa de texto, escreva um URL a utilizar o padrão do seguinte: `https://<Server-Based-URL>/<sub-domain>/`

    c. Copiar o **identificador** valor no bloco de notas. Irá utilizar o valor do identificador mais tarde no tutorial.
    
    > [!NOTE] 
    > Estes valores não estiverem reais. Atualize estes valores com o URL de início de sessão e o identificador real. Contacte [equipa de suporte de cliente i vendas Manager Remix](mailto:esupport@softbrain.co.jp) para obter estes valores.

4. No **certificado de assinatura de SAML** secção, clique em **Certificate(Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![A hiperligação de transferência do certificado](./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_certificate.png) 

5. Selecione **ver e editar todos os outros atributos de utilizador** e clique em **emailaddress** atributo.
    
    ![Configuração de Remix do Gestor de vendas i](./media/active-directory-saas-esalesmanagerremix-tutorial/configure1.png)

6. Copiar o **espaço de nomes** e **nome** valor da afirmação de caixa de texto. Gerar o valor padrão seguinte - `<Namespace>/<Name>`. Utilizará este valor mais adiante neste tutorial.

    ![Configuração de Remix do Gestor de vendas i](./media/active-directory-saas-esalesmanagerremix-tutorial/configure2.png)

7. No **Remix configuração do Gestor de vendas i** secção, clique em **configurar i vendas Manager Remix** para abrir **configurar início de sessão** janela. Copiar o **Sign-Out URL e o único início de sessão no URL do serviço SAML** do **secção de referência rápida.**

    ![Configuração de Remix do Gestor de vendas i](./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_configure.png) 

8. Clique em **guardar** botão.

    ![Configurar botão único início de sessão guardar](./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_400.png)

9. Iniciar sessão sua aplicação Remix do Gestor de vendas da i como administrador.

10. Selecione **ao Menu de administrador** no menu na parte superior direita.

    ![Configuração de Remix do Gestor de vendas i](./media/active-directory-saas-esalesmanagerremix-tutorial/configure4.png)

11. Selecione **definições do sistema**>**cooperação com o sistema externo**

    ![Configuração de Remix do Gestor de vendas i](./media/active-directory-saas-esalesmanagerremix-tutorial/configure5.png)
    
12. Selecione **SAML**

    ![Configuração de Remix do Gestor de vendas i](./media/active-directory-saas-esalesmanagerremix-tutorial/configure6.png)

13. No **definição de autenticação SAML** secção, execute os seguintes passos:

    ![Configuração de Remix do Gestor de vendas i](./media/active-directory-saas-esalesmanagerremix-tutorial/configure3.png)
    
    a. Selecione **versão de PC**
    
    b. Selecione **e-mail** na lista pendente na colaboração do item de secção.

    c. Na caixa de texto do item de colaboração, cole o **valor de afirmação** que copiou do revertidos portal do Azure `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`

    d. No **emissor (ID de entidade)** caixa de texto, cole o **identificador** valor que copiou do portal do Azure de **i vendas Manager Remix domínio e os URLs** secção.

    e. Para carregar o transferido **certificado** a partir do portal do Azure, selecione **seleção de ficheiros**.

    f. No **URL de início de sessão do fornecedor de ID** caixa de texto, colar **único início de sessão no URL do serviço SAML** que copiou a partir do portal do Azure.

    g. No **URL de fim de sessão do fornecedor de identidade** caixa de texto, colar **Sign-Out URL** valor que copiou a partir do portal do Azure.

    h. Clique em **definição completa**

> [!TIP]
> Pode agora ler estas instruções dentro de uma versão concisa o [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir do **do Active Directory > aplicações da empresa** secção, basta clicar no **Single Sign-On** separador e aceder à documentação do embedded através de **configuração** secção na parte inferior. Pode ler mais sobre a funcionalidade de documentação incorporados aqui: [do Azure AD incorporado documentação]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção consiste em criar um utilizador de teste no portal do Azure chamado Britta Simon.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique em de **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/active-directory-saas-esalesmanagerremix-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e "Todos os utilizadores" ligações](./media/active-directory-saas-esalesmanagerremix-tutorial/create_aaduser_02.png)

3. Para abrir o **utilizador** caixa de diálogo, clique em **adicionar** na parte superior do **todos os utilizadores** caixa de diálogo.

    ![O botão de adição](./media/active-directory-saas-esalesmanagerremix-tutorial/create_aaduser_03.png)

4. No **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/active-directory-saas-esalesmanagerremix-tutorial/create_aaduser_04.png)

    a. No **nome** caixa, escreva **BrittaSimon**.

    b. No **nome de utilizador** caixa, escreva o endereço de e-mail do utilizador Britta Simon.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado no **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-an-e-sales-manager-remix-test-user"></a>Criar um utilizador de teste Remix do Gestor de vendas da i

1. Iniciar sessão sua aplicação Remix do Gestor de vendas da i como administrador.

2. Selecione **ao Menu de administrador** no menu na parte superior direita.

    ![Configuração de Remix do Gestor de vendas i](./media/active-directory-saas-esalesmanagerremix-tutorial/configure4.png)

3. Selecione **as definições da empresa**>**manutenção dos departamentos e funcionários** e selecione **funcionários registados**.

    ![O utilizador](./media/active-directory-saas-esalesmanagerremix-tutorial/user1.png)

4. No **novo registo de empregado** secção, execute os seguintes passos:
    
    ![O utilizador](./media/active-directory-saas-esalesmanagerremix-tutorial/user2.png)

    a. No **empregado nome** caixa de texto, escreva o nome de utilizador como Britta.

    b. Preencha todos os campos obrigatórios respetivos com as informações do utilizador.
    
    c. Se ativar SAML, o administrador não conseguirá iniciar sessão a partir do ecrã de início de sessão, por isso, conceda administrador privilégios de início de sessão para o utilizador selecionando o **início de sessão de administrador**

    d. Clique em **registo**

5. No futuro, se pretender iniciar sessão como administrador, inicie sessão com o utilizador que foi concedido permissão de administrador e clique em **ao Menu de administrador** no menu na parte superior direita.

    ![O utilizador](./media/active-directory-saas-esalesmanagerremix-tutorial/configure4.png)

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Britta Simon utilizar o Azure-início de sessão único, concedendo acesso para Remix do Gestor de vendas da I.

![Atribuir a função de utilizador][200] 

**Para atribuir Britta Simon a Remix do Gestor de vendas da I, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicações e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todas as aplicações**.

    ![Atribua o utilizador][201] 

2. Na lista de aplicações, selecione **Remix do Gestor de vendas da i**.

    ![A ligação Remix do Gestor de vendas da i na lista de aplicações](./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação de "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Britta Simon** na lista utilizadores.

6. Clique em **selecione** botão no **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribuir** botão no **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Teste o início de sessão único

Nesta secção, testar a configuração do Azure AD único início de sessão através do painel de acesso.

Quando clica no mosaico Remix do Gestor de vendas da i no painel de acesso, deve obter automaticamente com sessão iniciada para a aplicação Remix do Gestor de vendas da I.
Para mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](active-directory-saas-tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_203.png

