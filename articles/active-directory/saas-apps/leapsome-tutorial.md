---
title: 'Tutorial: Integração do Azure Active Directory com Leapsome | Microsoft Docs'
description: Saiba como configurar o início de sessão entre o Azure Active Directory e Leapsome.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: cb523e97-add8-4289-b106-927bf1a02188
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2018
ms.author: jeedes
ms.openlocfilehash: b23a93db7912aa25b420157241c41533f4f48a27
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/25/2018
ms.locfileid: "36938442"
---
# <a name="tutorial-azure-active-directory-integration-with-leapsome"></a>Tutorial: Integração do Azure Active Directory com Leapsome

Neste tutorial, irá aprender a integrar Leapsome com o Azure Active Directory (Azure AD).

Integrar Leapsome com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Leapsome.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Leapsome (Single Sign-On) com as respetivas contas do Azure AD.
- Pode gerir as contas numa localização central - portal do Azure.

Se pretender saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [que é o acesso a aplicações e início de sessão no Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Leapsome, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Leapsome-início de sessão único ativada subscrição

> [!NOTE]
> Para testar os passos neste tutorial, não recomendamos a utilização num ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, teste do Azure AD-início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos modulares principais:

1. Adicionar Leapsome a partir da Galeria
2. Configurar e testar o Azure AD de sessão único-

## <a name="adding-leapsome-from-the-gallery"></a>Adicionar Leapsome a partir da Galeria
Para configurar a integração de Leapsome com o Azure AD, terá de adicionar Leapsome a partir da Galeria à sua lista de aplicações SaaS geridas.

**Para adicionar Leapsome a partir da galeria, execute os seguintes passos:**

1. No  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **do Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todas as aplicações**.

    ![O painel de aplicações da empresa][2]
    
3. Para adicionar a nova aplicação, clique em **nova aplicação** botão no topo da caixa de diálogo.

    ![O novo botão de aplicação][3]

4. Na caixa de pesquisa, escreva **Leapsome**, selecione **Leapsome** partir do painel de resultados, em seguida, clique em **adicionar** botão para adicionar a aplicação.

    ![Leapsome na lista de resultados](./media/leapsome-tutorial/tutorial_leapsome_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD-início de sessão único

Nesta secção, configure e teste do Azure AD-início de sessão único com Leapsome com base num utilizador de teste chamado "Britta Simon".

Para início de sessão trabalhar, do Azure AD tem de saber o que o utilizador homólogo no Leapsome é um utilizador no Azure AD. Por outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Leapsome tem de ser estabelecida.

Para configurar e testar o Azure AD-início de sessão único com Leapsome, tem de concluir os blocos modulares seguintes:

1. **[Configurar o Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)**  - para permitir aos utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD-início de sessão único com Britta Simon.
3. **[Criar um utilizador de teste Leapsome](#create-a-leapsome-test-user)**  - para ter um homólogo de Britta Simon Leapsome que está ligada a representação do Azure AD do utilizador.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar Britta Simon utilizar o Azure AD-início de sessão único.
5. **[Teste o início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD-início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão no portal do Azure e configurar o início de sessão único na sua aplicação Leapsome.

**Para configurar o Azure AD-início de sessão único com Leapsome, execute os seguintes passos:**

1. No portal do Azure, no **Leapsome** página de integração de aplicações, clique em **de sessão único-**.

    ![Configurar a ligação de início de sessão único][4]

2. No **de sessão único-** caixa de diálogo, selecione **modo** como **baseados em SAML início de sessão** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/leapsome-tutorial/tutorial_leapsome_samlbase.png)

3. No **Leapsome domínio e os URLs** secção, execute os seguintes passos, se pretender configurar a aplicação no **IDP** iniciada modo:

    ![Domínio Leapsome e os URLs únicos de informações de início de sessão](./media/leapsome-tutorial/tutorial_leapsome_url.png)

    a. No **identificador** caixa de texto, escreva um URL: `https://www.leapsome.com`

    b. No **URL de resposta** caixa de texto, escreva um URL a utilizar o padrão do seguinte: `https://www.leapsome.com/api/users/auth/saml/<CLIENTID>/assert`

4. Verifique **Mostrar avançadas definições de URL** e executar o passo seguinte, se pretender configurar a aplicação no **SP** iniciada modo:

    ![Domínio Leapsome e os URLs únicos de informações de início de sessão](./media/leapsome-tutorial/tutorial_leapsome_url1.png)

    No **URL de início de sessão** caixa de texto, escreva um URL a utilizar o padrão do seguinte: `https://www.leapsome.com/api/users/auth/saml/<CLIENTID>/login`
     
    > [!NOTE] 
    > O valor de URL de início de sessão e URL de resposta anterior não é o valor real. Atualizar estes valores reais, que é explicada mais tarde no tutorial.

5. Aplicação de Leapsome espera as asserções de SAML num formato específico. Configure as seguintes afirmações para esta aplicação. Pode gerir os valores destes atributos do **atributos de utilizador** secção na página de integração de aplicações. A seguinte captura de ecrã mostra um exemplo.
    
    ![Configurar o início de sessão único](./media/leapsome-tutorial/tutorial_Leapsome_attribute.png)

6. No **atributos de utilizador** secção no **de sessão único-** caixa de diálogo, configurar atributos token SAML, conforme mostrado na imagem acima e execute os seguintes passos:
    
    | Nome do Atributo | Valor do Atributo | Espaço de Nomes |
    | ---------------| --------------- | --------- |   
    | nome próprio | User.givenName | http://schemas.xmlsoap.org/ws/2005/05/identity/claims |
    | Apelido | User.Surname | http://schemas.xmlsoap.org/ws/2005/05/identity/claims |
    | título | user.jobtitle | http://schemas.xmlsoap.org/ws/2005/05/identity/claims |
    | imagem em | URL de imagem do empregado | http://schemas.xmlsoap.org/ws/2005/05/identity/claims |

    > [!Note]
    > O valor do atributo de imagem não é real. Atualize este valor com o URL da imagem real. Para obter contacte este valor [equipa de suporte de cliente Leapsome](mailto:support@leapsome.com).
    
    a. Clique em **adicionar atributo** para abrir o **adicionar atributo** caixa de diálogo.

    ![Configurar o início de sessão único](./media/leapsome-tutorial/tutorial_attribute_04.png)

    ![Configurar o início de sessão único](./media/leapsome-tutorial/tutorial_attribute_05.png)
    
    b. No **nome** caixa de texto, escreva o nome de atributo apresentado para essa linha.
    
    c. Do **valor** lista, digite o valor de atributo apresentado para essa linha.

    d. No **espaço de nomes** caixa de texto, escreva o uri de espaço de nomes para essa linha.
    
    e. Clique em **Ok**

7. No **certificado de assinatura de SAML** secção, clique em **certificado (Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![A hiperligação de transferência do certificado](./media/leapsome-tutorial/tutorial_leapsome_certificate.png) 

8. Clique em **guardar** botão.

    ![Configurar botão único início de sessão guardar](./media/leapsome-tutorial/tutorial_general_400.png)
    
9. No **Leapsome configuração** secção, clique em **configurar Leapsome** para abrir **configurar início de sessão** janela. Copiar o **único início de sessão no URL do serviço SAML** do **secção de referência rápida.**

    ![Configuração de Leapsome](./media/leapsome-tutorial/tutorial_leapsome_configure.png)

10. Numa janela do browser web diferente, inicie sessão no Leapsome como um administrador de segurança.

11. No canto superior direito, clique em logótipo de definições e, em seguida, clique em **definições de administrador**. 

    ![Conjunto de Leapsome](./media/leapsome-tutorial/tutorial_leapsome_admin.png)

12. Na barra de menu à esquerda, clique em **único início de sessão (SSO)** e o **baseados em SAML-início de sessão único (SSO)** página execute os seguintes passos:
    
    ![Leapsome saml](./media/leapsome-tutorial/tutorial_leapsome_samlsettings.png)

    a. Selecione **baseados em SAML ativar início de sessão**.

    b. Copiar o **URL de início de sessão (ponto aqui os seus utilizadores para iniciar o início de sessão)** valor e cole-o para o **URL de início de sessão** textbox em **Leapsome domínio e os URLs** secção no portal do Azure.

    c. Copiar o **URL de resposta (recieves resposta do seu fornecedor de identidade)** valor e cole-o para o **URL de resposta** textbox em **Leapsome domínio e os URLs** secção no portal do Azure.

    d. No **URL de início de sessão de SSO (fornecido pelo fornecedor de identidade)** caixa de texto, cole o valor de **único início de sessão no URL do serviço SAML**, que copiou do portal do Azure.

    e. Copie o certificado que transferiram a partir do portal do Azure sem – certificado começar e certificado do fim - comentários e cole-a no **certificado (fornecido pelo fornecedor de identidade)** caixa de texto.

    f. Clique em **definições de ATUALIZAÇÃO de SSO**.
    
### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção consiste em criar um utilizador de teste no portal do Azure chamado Britta Simon.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique em de **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/leapsome-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e "Todos os utilizadores" ligações](./media/leapsome-tutorial/create_aaduser_02.png)

3. Para abrir o **utilizador** caixa de diálogo, clique em **adicionar** na parte superior do **todos os utilizadores** caixa de diálogo.

    ![O botão de adição](./media/leapsome-tutorial/create_aaduser_03.png)

4. No **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/leapsome-tutorial/create_aaduser_04.png)

    a. No **nome** caixa, escreva **BrittaSimon**.

    b. No **nome de utilizador** caixa, escreva o endereço de e-mail do utilizador Britta Simon.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado no **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-leapsome-test-user"></a>Criar um utilizador de teste Leapsome

Nesta secção, vai criar um utilizador chamado Britta Simon Leapsome. Trabalhar com [equipa de suporte de cliente Leapsome](mailto:support@leapsome.com) para adicionar os utilizadores ou de domínio, tem de estar na lista de permissões na plataforma Leapsome. Se o domínio for adicionado pela equipa do, irá obter automaticamente aprovisionados para a plataforma de Leapsome. Os utilizadores tem de ser criados e ativados antes de utilizar o início de sessão único. 

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Britta Simon utilizar o Azure-início de sessão único, concedendo acesso para Leapsome.

![Atribuir a função de utilizador][200] 

**Para atribuir Britta Simon a Leapsome, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicações e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todas as aplicações**.

    ![Atribua o utilizador][201] 

2. Na lista de aplicações, selecione **Leapsome**.

    ![A ligação de Leapsome na lista de aplicações](./media/leapsome-tutorial/tutorial_leapsome_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação de "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Britta Simon** na lista utilizadores.

6. Clique em **selecione** botão no **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribuir** botão no **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, testar a configuração do Azure AD único início de sessão através do painel de acesso.

Quando clica no mosaico Leapsome no painel de acesso, deve obter automaticamente com sessão iniciada para a aplicação de Leapsome.
Para mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/leapsome-tutorial/tutorial_general_01.png
[2]: ./media/leapsome-tutorial/tutorial_general_02.png
[3]: ./media/leapsome-tutorial/tutorial_general_03.png
[4]: ./media/leapsome-tutorial/tutorial_general_04.png

[100]: ./media/leapsome-tutorial/tutorial_general_100.png

[200]: ./media/leapsome-tutorial/tutorial_general_200.png
[201]: ./media/leapsome-tutorial/tutorial_general_201.png
[202]: ./media/leapsome-tutorial/tutorial_general_202.png
[203]: ./media/leapsome-tutorial/tutorial_general_203.png

