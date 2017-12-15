---
title: "Tutorial: Integração do Azure Active Directory com Vodeclic | Microsoft Docs"
description: "Saiba como configurar o início de sessão entre o Azure Active Directory e Vodeclic."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: d77a0f53-e3a3-445e-ab3e-119cef6e2e1d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2017
ms.author: jeedes
ms.openlocfilehash: 940c7bb5040fb91a03b01dc43ee07d52e3d4e63b
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/14/2017
---
# <a name="tutorial-azure-active-directory-integration-with-vodeclic"></a>Tutorial: Integração do Azure Active Directory com Vodeclic

Neste tutorial, irá aprender a integrar Vodeclic com o Azure Active Directory (Azure AD).

Integrar Vodeclic com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Vodeclic.
- Pode permitir aos utilizadores obter iniciada automaticamente para Vodeclic (o início de sessão único, ou SSO) com as respetivas contas do Azure AD.
- Pode gerir as contas numa única localização central, o portal do Azure.

Se pretender saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [que é o acesso a aplicações e início de sessão no Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Vodeclic, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Uma subscrição Vodeclic SSO ativado

> [!NOTE]
> Para testar os passos neste tutorial, não recomendamos a utilização num ambiente de produção.

Para testar os passos neste tutorial, siga estas recomendações:

- Não utilize o seu ambiente de produção, exceto se for necessário.
- Se não tiver um ambiente de avaliação do Azure AD, [obtenha uma avaliação gratuita de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, teste do Azure AD-início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos modulares principais:

1. Adicionar Vodeclic a partir da Galeria
2. Configurar e testar o Azure AD de sessão único-

## <a name="add-vodeclic-from-the-gallery"></a>Adicionar Vodeclic a partir da Galeria
Para configurar a integração de Vodeclic com o Azure AD, terá de adicionar Vodeclic a partir da Galeria à sua lista de aplicações SaaS geridas.

**Para adicionar Vodeclic a partir da galeria, siga os passos seguintes:**

1. No [portal do Azure](https://portal.azure.com), no painel esquerdo, selecione o **do Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Aceda a **aplicações empresariais**. Em seguida, aceda a **todas as aplicações**.

    ![O painel de aplicações da empresa][2]
    
3. Para adicionar uma nova aplicação, selecione o **nova aplicação** botão na parte superior da caixa de diálogo.

    ![O novo botão de aplicação][3]

4. Na caixa de pesquisa, escreva **Vodeclic**. Selecione **Vodeclic** no painel de resultados e, em seguida, selecione o **adicionar** botão para adicionar a aplicação.

    ![Vodeclic na lista de resultados](./media/active-directory-saas-vodeclic-tutorial/tutorial_vodeclic_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD-início de sessão único

Nesta secção, configure e teste do Azure AD-início de sessão único com Vodeclic com base num utilizador de teste chamado "Britta Simon."

Para início de sessão trabalhar, do Azure AD tem de saber que o utilizador homólogo no Vodeclic for para um utilizador no Azure AD. Por outras palavras, tem de estabelecer uma ligação entre um utilizador do Azure AD e o utilizador relacionado no Vodeclic.

No Vodeclic, atribua o valor **Username** o mesmo valor como **nome de utilizador** no Azure AD. Agora tem de estabelecer a ligação entre dois utilizadores.

Para configurar e testar o Azure AD-início de sessão único com Vodeclic, conclua os blocos modulares seguintes:

1. [Configurar o Azure AD-início de sessão único](#configure-azure-ad-single-sign-on) para permitir aos utilizadores utilizar esta funcionalidade.
2. [Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user) para testar do Azure AD-início de sessão único com Britta Simon.
3. [Criar um utilizador de teste Vodeclic](#create-a-vodeclic-test-user) para ter um homólogo de Britta Simon no Vodeclic que está ligada a representação do Azure AD do utilizador.
4. [Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user) para ativar Britta Simon utilizar o Azure AD-início de sessão único.
5. [Teste o início de sessão único](#test-single-sign-on) para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD-início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão no portal do Azure e configurar o início de sessão único na sua aplicação Vodeclic.

**Para configurar o Azure AD-início de sessão único com Vodeclic, siga os passos seguintes:**

1. No portal do Azure, no **Vodeclic** página de integração de aplicações, selecione **de sessão único-**.

    ![Configurar a ligação de início de sessão único][4]

2. No **de sessão único-** caixa de diálogo em **modo de início de sessão único**, selecione **baseados em SAML início de sessão** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/active-directory-saas-vodeclic-tutorial/tutorial_vodeclic_samlbase.png)

3. Se pretender configurar a aplicação no **IDP** iniciou modo, o **Vodeclic domínio e os URLs** secção, siga os passos seguintes:

    ![Domínio Vodeclic e os URLs únicos de informações de início de sessão](./media/active-directory-saas-vodeclic-tutorial/tutorial_vodeclic_url.png)

    a. No **identificador** caixa, escreva um URL com o padrão do seguinte:`https://<companyname>.lms.vodeclic.net/auth/saml`

    b. No **URL de resposta** caixa, escreva um URL com o padrão do seguinte:`https://<companyname>.lms.vodeclic.net/auth/saml/callback`

4. Se pretender configurar a aplicação no **SP** modo iniciado, selecione o **Mostrar avançadas definições de URL** caixa de verificação e executar os seguintes passos:

    ![Domínio Vodeclic e os URLs únicos de informações de início de sessão](./media/active-directory-saas-vodeclic-tutorial/tutorial_vodeclic_url1.png)

    No **URL de início de sessão** caixa, escreva um URL com o padrão do seguinte:`https://<companyname>.lms.vodeclic.net/auth/saml`
     
    > [!NOTE] 
    > Estes valores não são reais. Atualizar estes valores com o identificador real, URL de resposta e início de sessão no URL. Contacte o [equipa de suporte de cliente Vodeclic](mailto:hotline@vodeclic.com) para obter estes valores.

5. No **certificado de assinatura de SAML** secção, selecione **XML de metadados**. Em seguida, guarde o ficheiro de metadados no seu computador.

    ![A hiperligação de transferência do certificado](./media/active-directory-saas-vodeclic-tutorial/tutorial_vodeclic_certificate.png) 

6. Selecione **Guardar**.

    ![Configurar botão único início de sessão guardar](./media/active-directory-saas-vodeclic-tutorial/tutorial_general_400.png)
    
7. Para configurar o início de sessão único no **Vodeclic** lado, enviar o transferido **XML de metadados** para o [equipa de suporte de Vodeclic](mailto:hotline@vodeclic.com). Se definir esta definição para que a ligação de SAML SSO corretamente em ambos os lados.

> [!TIP]
> Pode agora ler estas instruções dentro de uma versão concisa o [portal do Azure](https://portal.azure.com) enquanto estiver a configurar a aplicação. Depois de adicionar esta aplicação a partir de **do Active Directory** > **aplicações empresariais** secção, selecione o **Single Sign-On** separador e aceder a incorporados documentação através de **configuração** secção na parte inferior. Pode ler mais sobre a funcionalidade de documentação incorporados em [do Azure AD incorporado documentação]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção consiste em criar um utilizador de teste no portal do Azure chamado Britta Simon.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, siga os passos seguintes:**

1. No portal do Azure, no painel esquerdo, selecione o **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/active-directory-saas-vodeclic-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**. Em seguida, selecione **todos os utilizadores**.

    !["Os utilizadores e grupos" e "Todos os utilizadores" ligações](./media/active-directory-saas-vodeclic-tutorial/create_aaduser_02.png)

3. Para abrir o **utilizador** caixa de diálogo, selecione **adicionar** na parte superior do **todos os utilizadores** caixa de diálogo.

    ![O botão de adição](./media/active-directory-saas-vodeclic-tutorial/create_aaduser_03.png)

4. No **utilizador** diálogo caixa, siga os passos seguintes:

    ![A caixa de diálogo de utilizador](./media/active-directory-saas-vodeclic-tutorial/create_aaduser_04.png)

    a. No **nome** caixa, escreva **BrittaSimon**.

    b. No **nome de utilizador** caixa, escreva o endereço de e-mail do utilizador Britta Simon.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado no **palavra-passe** caixa.

    d. Selecione **Criar**.
 
### <a name="create-a-vodeclic-test-user"></a>Criar um utilizador de teste Vodeclic

Nesta secção, vai criar um utilizador chamado Britta Simon Vodeclic. Trabalhar com o [equipa de suporte de Vodeclic](mailto:hotline@vodeclic.com) para adicionar os utilizadores na plataforma Vodeclic. Os utilizadores tem de ser criados e ativados antes de utilizar o início de sessão único.

> [!NOTE]
> De acordo com requisitos de aplicações, poderá ter de obter na lista de permissões da máquina. Para que isso aconteça, terá de partilhar o seu endereço IP público com o [equipa de suporte de Vodeclic](mailto:hotline@vodeclic.com).

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Britta Simon utilizar o Azure-início de sessão único, concedendo acesso para Vodeclic.

![Atribuir a função de utilizador][200] 

**Para atribuir Britta Simon a Vodeclic, siga os passos seguintes:**

1. No portal do Azure, abra a vista de aplicações e, em seguida, avance para a vista de diretório. Em seguida, aceda a **aplicações empresariais**e, em seguida, selecione **todas as aplicações**.

    ![Atribua o utilizador][201] 

2. Na lista de aplicações, selecione **Vodeclic**.

    ![A ligação de Vodeclic na lista de aplicações](./media/active-directory-saas-vodeclic-tutorial/tutorial_vodeclic_app.png)  

3. No menu à esquerda, selecione **utilizadores e grupos**.

    ![A ligação de "Utilizadores e grupos"][202]

4. Selecione o **adicionar** botão. Em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Britta Simon** no **utilizadores** lista.

6. No **utilizadores e grupos** caixa de diálogo, selecione o **selecione** botão.

7. No **adicionar atribuição** caixa de diálogo, selecione o **atribuir** botão.
    
### <a name="test-single-sign-on"></a>Teste o início de sessão único

Nesta secção, teste configuração do Azure AD único início de sessão utilizando o painel de acesso.

Quando seleciona o mosaico Vodeclic no painel de acesso, obter iniciada automaticamente aplicação Vodeclic.

Para mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS no Azure Active Directory](active-directory-saas-tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-vodeclic-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-vodeclic-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-vodeclic-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-vodeclic-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-vodeclic-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-vodeclic-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-vodeclic-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-vodeclic-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-vodeclic-tutorial/tutorial_general_203.png

