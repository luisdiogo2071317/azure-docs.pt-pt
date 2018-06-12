---
title: 'Tutorial: Integração do Azure Active Directory com Settling música | Microsoft Docs'
description: Saiba como configurar o início de sessão entre o Azure Active Directory e Settling música.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 6f86a8a2-4bd0-40cc-b1b4-752fce123328
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2018
ms.author: jeedes
ms.openlocfilehash: 2a7c0fdef07ae573d5ff0459d4ff8043cff4e3fb
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35303924"
---
# <a name="tutorial-azure-active-directory-integration-with-settling-music"></a>Tutorial: Integração do Azure Active Directory com Settling música

Neste tutorial, irá aprender a integrar Settling música com o Azure Active Directory (Azure AD).

A integração de leitores de música Settling com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Settling música.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Settling música (Single Sign-On) com as respetivas contas do Azure AD.
- Pode gerir as contas numa localização central - portal do Azure.

Se pretender saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [que é o acesso a aplicações e início de sessão no Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Settling música, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Settling música-início de sessão único ativada subscrição

> [!NOTE]
> Para testar os passos neste tutorial, não recomendamos a utilização num ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, teste do Azure AD-início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos modulares principais:

1. A adição de música Settling de galeria
2. Configurar e testar o Azure AD de sessão único-

## <a name="adding-settling-music-from-the-gallery"></a>A adição de música Settling de galeria
Para configurar a integração de leitores de música Settling com o Azure AD, tem de adicionar Settling música na Galeria à sua lista de aplicações SaaS geridas.

**Para adicionar Settling música na galeria do, execute os seguintes passos:**

1. No  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **do Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todas as aplicações**.

    ![O painel de aplicações da empresa][2]
    
3. Para adicionar a nova aplicação, clique em **nova aplicação** botão no topo da caixa de diálogo.

    ![O novo botão de aplicação][3]

4. Na caixa de pesquisa, escreva **Settling música**, selecione **Settling música** partir do painel de resultados, em seguida, clique em **adicionar** botão para adicionar a aplicação.

    ![Settling música na lista de resultados](./media/active-directory-saas-settlingmusic-tutorial/tutorial_settlingmusic_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD-início de sessão único

Nesta secção, configure e teste do Azure AD-início de sessão único com Settling música com base num utilizador de teste chamado "Britta Simon".

Para início de sessão trabalhar, do Azure AD tem de saber o que o utilizador homólogo no Settling música é um utilizador no Azure AD. Por outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Settling música tem de ser estabelecida.

Para configurar e testar o Azure AD-início de sessão único com Settling música, tem de concluir os blocos modulares seguintes:

1. **[Configurar o Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)**  - para permitir aos utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD-início de sessão único com Britta Simon.
3. **[Criar um utilizador de teste de música Settling](#create-a-settling-music-test-user)**  - para ter um homólogo de Britta Simon Settling música que está ligada a representação do Azure AD do utilizador.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar Britta Simon utilizar o Azure AD-início de sessão único.
5. **[Teste o início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD-início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão no portal do Azure e configurar o início de sessão único na sua aplicação de música Settling.

**Para configurar o Azure AD-início de sessão único com Settling música, execute os seguintes passos:**

1. No portal do Azure, no **Settling música** página de integração de aplicações, clique em **de sessão único-**.

    ![Configurar a ligação de início de sessão único][4]

2. No **de sessão único-** caixa de diálogo, selecione **modo** como **baseados em SAML início de sessão** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/active-directory-saas-settlingmusic-tutorial/tutorial_settlingmusic_samlbase.png)

3. No **Settling música domínio e os URLs** secção, execute os seguintes passos:

    ![Settling música URLs e de domínio único início de sessão informações](./media/active-directory-saas-settlingmusic-tutorial/tutorial_settlingmusic_url.png)

    a. No **URL de início de sessão** caixa de texto, escreva um URL a utilizar o padrão do seguinte: `https://<SUBDOMAIN>.rakurakuseisan.jp/<USERACCOUNT>/`

    b. No **identificador** caixa de texto, escreva um URL a utilizar o padrão do seguinte: `https://<SUBDOMAIN>.rakurakuseisan.jp/<USERACCOUNT>/`

    > [!NOTE] 
    > Estes valores não estiverem reais. Atualize estes valores com o URL de início de sessão e o identificador real. Contacte [Settling equipa de suporte de cliente de música](https://rakurakuseisan.jp/) para obter estes valores. 
 
4. No **certificado de assinatura de SAML** secção, clique em **certificado (Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![A hiperligação de transferência do certificado](./media/active-directory-saas-settlingmusic-tutorial/tutorial_settlingmusic_certificate.png) 

5. Clique em **guardar** botão.

    ![Configurar botão único início de sessão guardar](./media/active-directory-saas-settlingmusic-tutorial/tutorial_general_400.png)

6. No **Settling música configuração** secção, clique em **configurar Settling música** para abrir **configurar início de sessão** janela. Copiar o **Sign-Out URL e o único início de sessão no URL do serviço SAML** do **secção de referência rápida.**

    ![Settling música configuração](./media/active-directory-saas-settlingmusic-tutorial/tutorial_settlingmusic_configure.png) 

7. Numa janela do browser web diferente, início de sessão para Settling música como um administrador de segurança.

8. Na parte superior da página, clique em **gestão** separador.

    ![Passo 1 de música settling](./media/active-directory-saas-settlingmusic-tutorial/tutorial_settlingmusic_step1.png)

9. Clique em **definição do sistema** separador.

    ![Settling música step2](./media/active-directory-saas-settlingmusic-tutorial/tutorial_settlingmusic_step2.png)

10. Mudar para **segurança** separador.

    ![Passo 3 de música settling](./media/active-directory-saas-settlingmusic-tutorial/tutorial_settlingmusic_step3.png)

11. No **única definição de início de sessão** secção, execute os seguintes passos:

    ![Settling música step5](./media/active-directory-saas-settlingmusic-tutorial/tutorial_settlingmusic_step4.png)

    a. Clique em **para ativar**.

    b. No **URL de início de sessão do fornecedor de ID de** caixa de texto, cole o valor de **único início de sessão no URL do serviço SAML** que copiou do portal do Azure.

    c. No **URL de fim de sessão do fornecedor de ID** caixa de texto, cole o valor de **Sign-Out URL** que copiou do portal do Azure.

    d. Clique em **Escolher ficheiro** para carregar o **certificado (Base64)** que já tenha transferido formar o portal do Azure.

    e. Clique no botão **Guardar**.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção consiste em criar um utilizador de teste no portal do Azure chamado Britta Simon.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique em de **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/active-directory-saas-settlingmusic-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e "Todos os utilizadores" ligações](./media/active-directory-saas-settlingmusic-tutorial/create_aaduser_02.png)

3. Para abrir o **utilizador** caixa de diálogo, clique em **adicionar** na parte superior do **todos os utilizadores** caixa de diálogo.

    ![O botão de adição](./media/active-directory-saas-settlingmusic-tutorial/create_aaduser_03.png)

4. No **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/active-directory-saas-settlingmusic-tutorial/create_aaduser_04.png)

    a. No **nome** caixa, escreva **BrittaSimon**.

    b. No **nome de utilizador** caixa, escreva o endereço de e-mail do utilizador Britta Simon.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado no **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-settling-music-test-user"></a>Criar um utilizador de teste de música Settling

Nesta secção, vai criar um utilizador chamado Britta Simon Settling música. Trabalhar com [Settling equipa de suporte de cliente de música](https://rakurakuseisan.jp/) para adicionar os utilizadores na plataforma de música Settling. Os utilizadores tem de ser criados e ativados antes de utilizar o início de sessão único.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Britta Simon utilizar o Azure-início de sessão único, concedendo acesso aos Settling música.

![Atribuir a função de utilizador][200] 

**Para atribuir Britta Simon a Settling música, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicações e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todas as aplicações**.

    ![Atribua o utilizador][201] 

2. Na lista de aplicações, selecione **Settling música**.

    ![A ligação de música Settling na lista de aplicações](./media/active-directory-saas-settlingmusic-tutorial/tutorial_settlingmusic_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação de "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Britta Simon** na lista utilizadores.

6. Clique em **selecione** botão no **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribuir** botão no **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, testar a configuração do Azure AD único início de sessão através do painel de acesso.

Quando clica no mosaico de música Settling no painel de acesso, deve obter automaticamente com sessão iniciada para a aplicação de música Settling.
Para mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](active-directory-saas-tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-settlingmusic-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-settlingmusic-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-settlingmusic-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-settlingmusic-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-settlingmusic-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-settlingmusic-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-settlingmusic-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-settlingmusic-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-settlingmusic-tutorial/tutorial_general_203.png

