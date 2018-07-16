---
title: 'Tutorial: Integração do Azure Active Directory com 123ContactForm | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e 123ContactForm.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 5211910a-ab96-4709-959a-524c4d57c43e
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: ecbe627697fc4f8b5fbfecf96c3cb65d9ffe4607
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39054357"
---
# <a name="tutorial-azure-active-directory-integration-with-123contactform"></a>Tutorial: Integração do Azure Active Directory com 123ContactForm

Neste tutorial, saiba como integrar 123ContactForm com o Azure Active Directory (Azure AD).

Integrar 123ContactForm no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao 123ContactForm
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para 123ContactForm (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com 123ContactForm, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um 123ContactForm logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando 123ContactForm da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-123contactform-from-the-gallery"></a>Adicionando 123ContactForm da Galeria
Para configurar a integração do 123ContactForm com o Azure AD, terá de adicionar 123ContactForm a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar 123ContactForm a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

4. Na caixa de pesquisa, escreva **123ContactForm**.

    ![Criar um utilizador de teste do Azure AD](./media/123contactform-tutorial/tutorial_123contactform_search.png)

5. No painel de resultados, selecione **123ContactForm**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/123contactform-tutorial/tutorial_123contactform_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com 123ContactForm com base num utilizador de teste chamado "Eduarda Almeida."

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no 123ContactForm a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no 123ContactForm deve ser estabelecido.

123ContactForm, atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com 123ContactForm, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste 123ContactForm](#creating-a-123contactform-test-user)**  - para ter um equivalente da Eduarda Almeida na 123ContactForm que está ligado à representação de utilizador do Azure AD.
4. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo 123ContactForm.

**Para configurar o Azure AD início de sessão único com 123ContactForm, execute os seguintes passos:**

1. No portal do Azure, sobre o **123ContactForm** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/123contactform-tutorial/tutorial_123contactform_samlbase.png)

3. Sobre o **123ContactForm domínio e URLs** secção, se desejar configurar a aplicação no **IDP iniciada pelo modo**, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/123contactform-tutorial/url1.png)

    a. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://www.123contactform.com/saml/azure_ad/<tenant_id>/metadata`

    b. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://www.123contactform.com/saml/azure_ad/<tenant_id>/acs`

4. Se desejar configurar a aplicação no **SP iniciada pelo modo**, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/123contactform-tutorial/url2.png)

    a. Clique nas **Mostrar definições de URL avançadas** opção

    b. Na **URL de início de sessão** caixa de texto, escreva um URL como: `https://www.123contactform.com/saml/azure_ad/<tenant_id>/sso`

    > [!NOTE] 
    > Estes valores não são reais. Terá de atualizar esses valor do identificador que é explicado mais tarde no tutorial e URLs real.
    
5. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![Configurar o início de sessão único](./media/123contactform-tutorial/tutorial_123contactform_certificate.png) 

6. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/123contactform-tutorial/tutorial_general_400.png)

7. Para configurar o início de sessão único num **123ContactForm** lado, acedo à [ https://www.123contactform.com/form-2709121/ ](https://www.123contactform.com/form-2709121/) e execute os seguintes passos:

    ![Configurar o início de sessão único](./media/123contactform-tutorial/submit.png) 

    a. Na **E-Mail** caixa de texto, escreva a mensagem de e-mail de como o utilizador **BrittaSimon@Contoso.com**.

    b. Clique em **carregar** e procure o ficheiro XML de metadados, que transferiu a partir do portal do Azure.

    c. Clique em **formulário de submissão**.

8. Sobre o **Microsoft Azure AD - início de sessão único - configurar definições de aplicação** execute os seguintes passos:
    
    ![Configurar o início de sessão único](./media/123contactform-tutorial/url3.png)

    a. Se desejar configurar a aplicação no **IDP iniciada pelo modo**, cópia a **identificador** valor para a sua instância e cole-a no **identificador** caixa de texto no  **123ContactForm domínio e URLs** secção no portal do Azure.
    
    b. Se desejar configurar a aplicação no **IDP iniciada pelo modo**, cópia a **URL de resposta** valor para a sua instância e cole-a no **URL de resposta** caixa de texto no  **123ContactForm domínio e URLs** secção no portal do Azure.

    c. Se desejar configurar a aplicação no **SP iniciada pelo modo**, cópia a **URL de início de sessão** valor para a sua instância e cole-a no **URL de início de sessão** caixa de texto no  **123ContactForm domínio e URLs** secção no portal do Azure.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/123contactform-tutorial/create_aaduser_01.png) 

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/123contactform-tutorial/create_aaduser_02.png) 

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/123contactform-tutorial/create_aaduser_03.png) 

4. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/123contactform-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-a-123contactform-test-user"></a>Criar um utilizador de teste 123ContactForm

Aplicativo oferece suporte apenas no tempo de aprovisionamento de utilizador e de utilizadores de autenticação serão criados no aplicativo automaticamente.

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso a 123ContactForm.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida a 123ContactForm, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **123ContactForm**.

    ![Configurar o início de sessão único](./media/123contactform-tutorial/tutorial_123contactform_app.png) 

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico 123ContactForm no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo 123ContactForm.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/123contactform-tutorial/tutorial_general_01.png
[2]: ./media/123contactform-tutorial/tutorial_general_02.png
[3]: ./media/123contactform-tutorial/tutorial_general_03.png
[4]: ./media/123contactform-tutorial/tutorial_general_04.png

[100]: ./media/123contactform-tutorial/tutorial_general_100.png

[200]: ./media/123contactform-tutorial/tutorial_general_200.png
[201]: ./media/123contactform-tutorial/tutorial_general_201.png
[202]: ./media/123contactform-tutorial/tutorial_general_202.png
[203]: ./media/123contactform-tutorial/tutorial_general_203.png

