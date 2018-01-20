---
title: "Tutorial: Integração do Azure Active Directory com Dome9 arco | Microsoft Docs"
description: "Saiba como configurar o início de sessão entre o Azure Active Directory e Dome9 arco."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 4c12875f-de71-40cb-b9ac-216a805334e5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/18/2018
ms.author: jeedes
ms.openlocfilehash: 2ce4bb1be8b0124c69991765e18ce9922bd2f4a4
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/20/2018
---
# <a name="tutorial-azure-active-directory-integration-with-dome9-arc"></a>Tutorial: Integração do Azure Active Directory com Dome9 arco

Neste tutorial, irá aprender a integrar o arco de Dome9 com o Azure Active Directory (Azure AD).

Integrar o arco de Dome9 com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso arco Dome9.
- Pode permitir aos utilizadores automaticamente obter sessão iniciada em arco Dome9 (Single Sign-On) com as respetivas contas do Azure AD.
- Pode gerir as contas numa localização central - portal do Azure.

Se pretender saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [que é o acesso a aplicações e início de sessão no Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Dome9 arco, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um arco Dome9-início de sessão único ativada subscrição

> [!NOTE]
> Para testar os passos neste tutorial, não recomendamos a utilização num ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, teste do Azure AD-início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos modulares principais:

1. A adição de arco Dome9 de galeria
2. Configurar e testar o Azure AD de sessão único-

## <a name="adding-dome9-arc-from-the-gallery"></a>A adição de arco Dome9 de galeria
Para configurar a integração de arco Dome9 com o Azure AD, tem de adicionar Dome9 arco na Galeria à sua lista de aplicações SaaS geridas.

**Para adicionar Dome9 arco a partir da galeria, execute os seguintes passos:**

1. No  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **do Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todas as aplicações**.

    ![O painel de aplicações da empresa][2]
    
3. Para adicionar a nova aplicação, clique em **nova aplicação** botão no topo da caixa de diálogo.

    ![O novo botão de aplicação][3]

4. Na caixa de pesquisa, escreva **Dome9 arco**, selecione **Dome9 arco** partir do painel de resultados, em seguida, clique em **adicionar** botão para adicionar a aplicação.

    ![Dome9 arco na lista de resultados](./media/active-directory-saas-dome9arc-tutorial/tutorial_dome9arc_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD-início de sessão único

Nesta secção, configure e teste do Azure AD-início de sessão único com o arco de Dome9 com base num utilizador de teste chamado "Britta Simon".

Para início de sessão trabalhar, do Azure AD tem de saber o que o utilizador homólogo em arco Dome9 é um utilizador no Azure AD. Por outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Dome9 arco tem de ser estabelecida.

Em arco Dome9, atribua o valor do **nome de utilizador** no Azure AD como o valor a **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD-início de sessão único com Dome9 arco, tem de concluir os blocos modulares seguintes:

1. **[Configurar o Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)**  - para permitir aos utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD-início de sessão único com Britta Simon.
3. **[Criar um utilizador de teste de arco Dome9](#create-a-dome9-arc-test-user)**  - para ter um homólogo de Britta Simon arco Dome9 que está ligada a representação do Azure AD do utilizador.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar Britta Simon utilizar o Azure AD-início de sessão único.
5. **[Teste o início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD-início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão no portal do Azure e configurar o início de sessão único na sua aplicação Dome9 arco.

**Para configurar o Azure AD-início de sessão único com Dome9 arco, execute os seguintes passos:**

1. No portal do Azure, no **Dome9 arco** página de integração de aplicações, clique em **de sessão único-**.

    ![Configurar a ligação de início de sessão único][4]

2. No **de sessão único-** caixa de diálogo, selecione **modo** como **baseados em SAML início de sessão** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/active-directory-saas-dome9arc-tutorial/tutorial_dome9arc_samlbase.png)

3. No **Dome9 arco domínio e os URLs** secção, execute os seguintes passos, se pretender configurar a aplicação no **IDP** iniciada modo:

    ![Dome9 arco domínio e os URLs únicos de informações de início de sessão](./media/active-directory-saas-dome9arc-tutorial/tutorial_dome9arc_url.png)

    a. No **identificador** caixa de texto, escreva o URL:`https://secure.dome9.com/`

    b. No **URL de resposta** caixa de texto, escreva um URL a utilizar o padrão do seguinte:`https://secure.dome9.com/sso/saml/yourcompanyname`

    > [!NOTE]
    > Irá selecionar o valor de nome de empresa no portal de administração dome9, que é explicado mais tarde no tutorial.

4. Verifique **Mostrar avançadas definições de URL** e executar o passo seguinte, se pretender configurar a aplicação no **SP** iniciada modo:

    ![Dome9 arco domínio e os URLs únicos de informações de início de sessão](./media/active-directory-saas-dome9arc-tutorial/tutorial_dome9arc_url1.png)

    No **URL de início de sessão** caixa de texto, escreva um URL a utilizar o padrão do seguinte:`https://secure.dome9.com/sso/saml/<yourcompanyname>`
     
    > [!NOTE] 
    > Estes valores não estiverem reais. Atualize estes valores com o URL de resposta real e o URL de início de sessão. Contacte [equipa de suporte de cliente de arco Dome9](https://dome9.com/about/contact-us/) para obter estes valores. 

5. A aplicação de Software de arco Dome9 espera as asserções de SAML num formato específico. Configure as seguintes afirmações para esta aplicação. Pode gerir os valores destes atributos a partir de "**atributos de utilizador**" secção na página de integração de aplicações. A seguinte captura de ecrã mostra um exemplo para este.

    ![Configurar attb Single Sign-On](./media/active-directory-saas-dome9arc-tutorial/tutorial_dome9arc_attribute.png)

6. No **atributos de utilizador** secção no **de sessão único-** caixa de diálogo, configurar atributos token SAML, conforme mostrado na imagem acima e execute os seguintes passos:
    
    | Nome do Atributo  | Valor do Atributo | 
    | --------------- | --------------- | 
    | memberof | user.assignedroles | 
    
    a. Clique em **adicionar atributo** para abrir o **adicionar atributo** caixa de diálogo.

    ![Configurar o início de sessão único adicionar attb](./media/active-directory-saas-dome9arc-tutorial/tutorial_dome9_04.png)

    ![Configurar o início de sessão único attb de edição](./media/active-directory-saas-dome9arc-tutorial/tutorial_attribute_05.png)

    b. No **nome** caixa de texto, escreva o nome de atributo apresentado para essa linha.

    c. Do **valor** lista, digite o valor de atributo apresentado para essa linha.
    
    d. Clique em **OK**.

7. No **certificado de assinatura de SAML** secção, clique em **Certificate(Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![A hiperligação de transferência do certificado](./media/active-directory-saas-dome9arc-tutorial/tutorial_dome9arc_certificate.png) 

8. Clique em **guardar** botão.

    ![Configurar botão único início de sessão guardar](./media/active-directory-saas-dome9arc-tutorial/tutorial_general_400.png)
    
9. No **Dome9 arco configuração** secção, clique em **configurar o arco de Dome9** para abrir **configurar início de sessão** janela. Copiar o **ID de entidade de SAML e único início de sessão no URL do serviço SAML** do **secção de referência rápida.**

    ![Configuração do Dome9 em arco](./media/active-directory-saas-dome9arc-tutorial/tutorial_dome9arc_configure.png) 

10. Numa janela do browser web diferente, inicie sessão no site da sua empresa Dome9 arco como administrador.

11. Clique em de **definições de perfil** no canto superior direito e, em seguida, clique em **as definições da conta**. 

    ![Configuração do Dome9 em arco](./media/active-directory-saas-dome9arc-tutorial/configure1.png)

12. Navegue para **SSO** e, em seguida, clique em **ATIVAR**.

    ![Configuração do Dome9 em arco](./media/active-directory-saas-dome9arc-tutorial/configure2.png)

13. Na secção de configuração de SSO, execute os seguintes passos:

    ![Configuração do Dome9 em arco](./media/active-directory-saas-dome9arc-tutorial/configure3.png)

    a. Introduza o nome da empresa no **ID da conta** caixa de texto. Este valor está a ser utilizado na resposta que URL mencionado na secção URL do portal do Azure.

    b. No **emissor** caixa de texto, colar o valor de **ID de entidade de SAML**que copiou o formulário do portal do Azure.

    c. No **url de ponto final do Idp** caixa de texto, colar o valor de **único início de sessão no URL do serviço SAML**que copiou o formulário do portal do Azure.

    d. Abra o certificado codificado em Base64 transferido no bloco de notas, copie o conteúdo do mesmo para a sua área de transferência e, em seguida, cole-os para o **certificado x. 509** caixa de texto.

    e. Clique em **Guardar**.

> [!TIP]
> Pode agora ler estas instruções dentro de uma versão concisa o [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir do **do Active Directory > aplicações da empresa** secção, basta clicar no **Single Sign-On** separador e aceder à documentação do embedded através de **configuração** secção na parte inferior. Pode ler mais sobre a funcionalidade de documentação incorporados aqui: [do Azure AD incorporado documentação]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção consiste em criar um utilizador de teste no portal do Azure chamado Britta Simon.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique em de **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/active-directory-saas-dome9arc-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e "Todos os utilizadores" ligações](./media/active-directory-saas-dome9arc-tutorial/create_aaduser_02.png)

3. Para abrir o **utilizador** caixa de diálogo, clique em **adicionar** na parte superior do **todos os utilizadores** caixa de diálogo.

    ![O botão de adição](./media/active-directory-saas-dome9arc-tutorial/create_aaduser_03.png)

4. No **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/active-directory-saas-dome9arc-tutorial/create_aaduser_04.png)

    a. No **nome** caixa, escreva **BrittaSimon**.

    b. No **nome de utilizador** caixa, escreva o endereço de e-mail do utilizador Britta Simon.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado no **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-dome9-arc-test-user"></a>Criar um utilizador de teste Dome9 arco

Para permitir que os utilizadores do Azure AD iniciem sessão arco Dome9, têm de ser aprovisionados na aplicação. Dome9 arco suportam o aprovisionamento de just-in-time, mas para que funcione corretamente, o utilizador tem de selecionar específico **função** e atribuir o mesmo ao utilizador.

   >[!Note] 
   >Para **função** criação e outros detalhes de contacto [equipa de suporte de cliente de arco Dome9](https://dome9.com/about/contact-us/).

**Para Aprovisionar uma conta de utilizador manualmente, execute os seguintes passos:**

1. Inicie sessão no site da sua empresa Dome9 arco como administrador.

2. Clique em de **utilizadores e funções** e, em seguida, clique em **utilizadores**.

    ![Adicionar empregado](./media/active-directory-saas-dome9arc-tutorial/user1.png)

3. Clique em **adicionar utilizador**.

    ![Adicionar empregado](./media/active-directory-saas-dome9arc-tutorial/user2.png)

4. No **criar utilizador** secção, execute os seguintes passos:
    
    ![Adicionar empregado](./media/active-directory-saas-dome9arc-tutorial/user3.png)

    a. No **E-Mail** caixa de texto, como o tipo de mensagem de correio eletrónico do utilizador Brittasimon@contoso.com.

    b. No **nome próprio** caixa de texto, nome do primeiro tipo de utilizador como Britta.

    c. No **Apelido** caixa de texto, apelido tipo do utilizador como Simon.

    d. Certifique- **SSO utilizador** como **no**.

    e. Clique em **criar**.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Britta Simon utilizar o Azure-início de sessão único, concedendo acesso arco Dome9.

![Atribuir a função de utilizador][200] 

**Para atribuir Britta Simon arco Dome9, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicações e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todas as aplicações**.

    ![Atribua o utilizador][201] 

2. Na lista de aplicações, selecione **Dome9 arco**.

    ![A ligação de arco Dome9 na lista de aplicações](./media/active-directory-saas-dome9arc-tutorial/tutorial_dome9arc_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação de "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Britta Simon** na lista utilizadores.

6. Clique em **selecione** botão no **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribuir** botão no **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Teste o início de sessão único

Nesta secção, testar a configuração do Azure AD único início de sessão através do painel de acesso.

Quando clica no mosaico Dome9 arco no painel de acesso, deve obter automaticamente com sessão iniciada para a aplicação de arco Dome9.
Para mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](active-directory-saas-tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-dome9arc-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-dome9arc-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-dome9arc-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-dome9arc-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-dome9arc-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-dome9arc-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-dome9arc-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-dome9arc-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-dome9arc-tutorial/tutorial_general_203.png

