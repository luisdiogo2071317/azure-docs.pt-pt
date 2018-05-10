---
title: 'Tutorial: Integração do Azure Active Directory com a publicação de arco - SSO | Microsoft Docs'
description: Saiba como configurar o início de sessão entre o Azure Active Directory e a publicação de arco - SSO.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: ae609583-f875-4cb8-b68e-1b0b7938e9a7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2018
ms.author: jeedes
ms.openlocfilehash: 64e02ed953059303250138b655526d75afd3e2b3
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
---
# <a name="tutorial-azure-active-directory-integration-with-arc-publishing---sso"></a>Tutorial: Integração do Azure Active Directory com a publicação de arco - SSO

Neste tutorial, saiba como integrar a publicação de arco - SSO ao Azure Active Directory (Azure AD).

Integrar o arco de publicação - SSO com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso à publicação de arco - SSO.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para publicação de arco - SSO (Single Sign-On) com as respetivas contas do Azure AD.
- Pode gerir as contas numa localização central - portal do Azure.

Se pretender saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [que é o acesso a aplicações e início de sessão no Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com a publicação de arco - SSO, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Uma publicação de arco - SSO-início de sessão único ativada subscrição

> [!NOTE]
> Para testar os passos neste tutorial, não recomendamos a utilização num ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, teste do Azure AD-início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos modulares principais:

1. Adição de publicação de arco - SSO a partir da Galeria
2. Configurar e testar o Azure AD de sessão único-

## <a name="adding-arc-publishing---sso-from-the-gallery"></a>Adição de publicação de arco - SSO a partir da Galeria
Para configurar a integração da publicação de arco - SSO com o Azure AD, tem de adicionar a publicação de arco - SSO na Galeria à sua lista de aplicações SaaS geridas.

**Para adicionar publicação de arco - SSO da galeria, execute os seguintes passos:**

1. No  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **do Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todas as aplicações**.

    ![O painel de aplicações da empresa][2]
    
3. Para adicionar a nova aplicação, clique em **nova aplicação** botão no topo da caixa de diálogo.

    ![O novo botão de aplicação][3]

4. Na caixa de pesquisa, escreva **publicação arco - SSO**, selecione **publicação arco - SSO** partir do painel de resultados, em seguida, clique em **adicionar** botão para adicionar a aplicação.

    ![Publicação de arco - SSO na lista de resultados](./media/active-directory-saas-arc-tutorial/tutorial_arc_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD-início de sessão único

Nesta secção, configure e teste do Azure AD-início de sessão único com o arco de publicação - SSO baseada num utilizador de teste chamado "Britta Simon".

Para início de sessão trabalhar, do Azure AD tem de saber que o utilizador homólogo na publicação arco - SSO for para um utilizador no Azure AD. Por outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado na publicação arco - SSO tem de ser estabelecida.

Para configurar e testar o Azure AD-início de sessão único com a publicação de arco - SSO, tem de concluir os blocos modulares seguintes:

1. **[Configurar o Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)**  - para permitir aos utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD-início de sessão único com Britta Simon.
3. **[Criar uma publicação de arco - utilizador de teste de SSO](#create-an-arc-publishing---sso-test-user)**  - para ter um homólogo de Britta Simon na publicação arco - SSO que está ligada a representação do Azure AD do utilizador.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar Britta Simon utilizar o Azure AD-início de sessão único.
5. **[Teste o início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD-início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão no portal do Azure e configurar o início de sessão único na sua arco publicação - aplicação SSO.

**Para configurar o Azure AD-início de sessão único com a publicação de arco - SSO, execute os seguintes passos:**

1. No portal do Azure, no **publicação arco - SSO** página de integração de aplicações, clique em **de sessão único-**.

    ![Configurar a ligação de início de sessão único][4]

2. No **de sessão único-** caixa de diálogo, selecione **modo** como **baseados em SAML início de sessão** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/active-directory-saas-arc-tutorial/tutorial_arc_samlbase.png)

3. No **arco publicação - domínio SSO e URLs** secção, execute os seguintes passos, se pretender configurar a aplicação no **IDP** iniciada modo:

    ![Publicação de arco - domínio SSO e URLs único início de sessão informações](./media/active-directory-saas-arc-tutorial/tutorial_arc_url.png)

    a. No **identificador** caixa de texto, escreva um URL a utilizar o padrão do seguinte: `https://www.okta.com/saml2/service-provider/<Unique ID>`

    b. No **URL de resposta** caixa de texto, escreva um URL a utilizar o padrão do seguinte: `https://arcpublishing-<Customer>.okta.com/sso/saml2/<Unique ID>`

4. Verifique **Mostrar avançadas definições de URL** e executar o passo seguinte, se pretender configurar a aplicação no **SP** iniciada modo:

    ![Publicação de arco - domínio SSO e URLs único início de sessão informações](./media/active-directory-saas-arc-tutorial/tutorial_arc_url1.png)

    No **URL de início de sessão** caixa de texto, escreva um URL a utilizar o padrão do seguinte: `https://arcpublishing-<Customer>.okta.com/sso/saml2/<Unique ID>`
     
    > [!NOTE] 
    > Estes valores não estiverem reais. Atualize estes valores com o identificador de real, a URL de resposta e o URL de início de sessão. Contacte [publicação arco - equipa de suporte de cliente de SSO](mailto:inf@washpost.com) para obter estes valores. 

5. Publicação de arco - aplicação SSO espera as asserções de SAML num formato específico. Configure as seguintes afirmações para esta aplicação. Pode gerir os valores destes atributos do **atributos de utilizador** secção na página de integração de aplicações. A seguinte captura de ecrã mostra um exemplo para este.
    
    ![Configurar o início de sessão único](./media/active-directory-saas-arc-tutorial/tutorial_arc_attribute.png)

6. No **atributos de utilizador** secção no **de sessão único-** caixa de diálogo, configurar atributos token SAML, conforme mostrado na imagem acima e execute os seguintes passos:
    
    | Nome do Atributo | Valor do Atributo |
    | ---------------| --------------- |    
    | firstName | User.givenName |
    | Apelido | User.Surname |
    | e-mail | User.Mail |
    | grupos | user.assignedroles |

    a. Clique em **adicionar atributo** para abrir o **adicionar atributo** caixa de diálogo.

     ![Configurar o início de sessão único](./media/active-directory-saas-arc-tutorial/tutorial_attribute_04.png)

     ![Configurar o início de sessão único](./media/active-directory-saas-arc-tutorial/tutorial_attribute_05.png)
    
    b. No **nome** caixa de texto, escreva o nome de atributo apresentado para essa linha.
    
    c. Do **valor** lista, digite o valor de atributo apresentado para essa linha.

    d. Deixe o **espaço de nomes** em branco.
    
    d. Clique em **Ok**

    > [!NOTE]
    > Aqui o **grupos** atributo está mapeado com **user.assignedroles**. Estas são as funções personalizadas criadas no Azure AD para mapear os nomes de grupo novamente na aplicação. Pode encontrar mais orientação [aqui](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-enterprise-app-role-management) sobre como criar funções personalizadas no Azure AD. 

7. No **certificado de assinatura de SAML** secção, clique em **certificado (Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![A hiperligação de transferência do certificado](./media/active-directory-saas-arc-tutorial/tutorial_arc_certificate.png) 

8. Clique em **guardar** botão.

    ![Configurar botão único início de sessão guardar](./media/active-directory-saas-arc-tutorial/tutorial_general_400.png)
    
9. No **publicação arco - configuração SSO** secção, clique em **configurar a publicação de arco - SSO** para abrir **configurar início de sessão** janela. Copiar o **Sign-Out URL, o ID de entidade de SAML e o único início de sessão no URL do serviço SAML** do **secção de referência rápida.**

    ![Publicação de arco - configuração SSO](./media/active-directory-saas-arc-tutorial/tutorial_arc_configure.png) 

10. Para configurar o início de sessão único em **publicação arco - SSO** lado, terá de enviar o transferido **certificado (Base64), Sign-Out URL, ID de entidade de SAML e único início de sessão no URL do serviço SAML** para [em arco Publicação - a equipa de suporte do SSO](mailto:inf@washpost.com). Se definir esta definição para que a ligação de SAML SSO corretamente em ambos os lados.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção consiste em criar um utilizador de teste no portal do Azure chamado Britta Simon.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique em de **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/active-directory-saas-arc-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e "Todos os utilizadores" ligações](./media/active-directory-saas-arc-tutorial/create_aaduser_02.png)

3. Para abrir o **utilizador** caixa de diálogo, clique em **adicionar** na parte superior do **todos os utilizadores** caixa de diálogo.

    ![O botão de adição](./media/active-directory-saas-arc-tutorial/create_aaduser_03.png)

4. No **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/active-directory-saas-arc-tutorial/create_aaduser_04.png)

    a. No **nome** caixa, escreva **BrittaSimon**.

    b. No **nome de utilizador** caixa, escreva o endereço de e-mail do utilizador Britta Simon.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado no **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-an-arc-publishing---sso-test-user"></a>Criar um arco publicação - utilizador de teste SSO

O objetivo desta secção consiste em criar um utilizador chamado Britta Simon na publicação arco - SSO. Publicação de arco - SSO suporte just-in-time aprovisionamento, que é por predefinição ativada. Não há nenhum item de ação para si nesta secção. Um novo utilizador é criado durante a tentativa de aceder a publicação de arco - SSO se não existir ainda.

>[!Note]
>Se precisar de criar manualmente um utilizador, contacte [publicação arco - a equipa de suporte do SSO](mailto:inf@washpost.com).

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Britta Simon utilizar o Azure-início de sessão único, concedendo acesso à publicação de arco - SSO.

![Atribuir a função de utilizador][200] 

**Para atribuir Britta Simon a publicação de arco - SSO, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicações e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todas as aplicações**.

    ![Atribua o utilizador][201] 

2. Na lista de aplicações, selecione **publicação arco - SSO**.

    ![A publicação de arco - ligação SSO na lista de aplicações](./media/active-directory-saas-arc-tutorial/tutorial_arc_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação de "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Britta Simon** na lista utilizadores.

6. Clique em **selecione** botão no **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribuir** botão no **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, testar a configuração do Azure AD único início de sessão através do painel de acesso.

Quando clica o arco publicação - mosaico SSO no painel de acesso, deve obter automaticamente com sessão iniciada para o arco publicação - aplicação SSO.
Para mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](active-directory-saas-tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-arc-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-arc-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-arc-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-arc-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-arc-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-arc-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-arc-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-arc-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-arc-tutorial/tutorial_general_203.png

