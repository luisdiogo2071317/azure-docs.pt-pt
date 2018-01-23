---
title: "Tutorial: Integração do Azure Active Directory com o estado de funcionamento RedBrick | Microsoft Docs"
description: "Saiba como configurar o início de sessão entre o Azure Active Directory e o estado de funcionamento RedBrick."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 26290c65-9aa3-42ab-8ba5-901b14dc8e73
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/22/2018
ms.author: jeedes
ms.openlocfilehash: 598592d87cf6471a431dab89d19c5e8beb48e661
ms.sourcegitcommit: 5ac112c0950d406251551d5fd66806dc22a63b01
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/23/2018
---
# <a name="tutorial-azure-active-directory-integration-with-redbrick-health"></a>Tutorial: Integração do Azure Active Directory com o estado de funcionamento RedBrick

Neste tutorial, irá aprender a integrar o estado de funcionamento RedBrick com o Azure Active Directory (Azure AD).

Integrar o estado de funcionamento RedBrick com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao estado de funcionamento RedBrick.
- Pode permitir aos utilizadores automaticamente obter com sessão iniciada para o estado de funcionamento RedBrick (Single Sign-On) com as respetivas contas do Azure AD.
- Pode gerir as contas numa localização central - portal do Azure.

Se pretender saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [que é o acesso a aplicações e início de sessão no Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o estado de funcionamento RedBrick, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Estado de funcionamento RedBrick-início de sessão único ativada subscrição

> [!NOTE]
> Para testar os passos neste tutorial, não recomendamos a utilização num ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, teste do Azure AD-início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos modulares principais:

1. Adicionar o estado de funcionamento RedBrick na galeria do
2. Configurar e testar o Azure AD de sessão único-

## <a name="adding-redbrick-health-from-the-gallery"></a>Adicionar o estado de funcionamento RedBrick na galeria do
Para configurar a integração do Estado de funcionamento RedBrick com o Azure AD, tem de adicionar RedBrick do Estado de funcionamento da Galeria à sua lista de aplicações SaaS geridas.

**Para adicionar o estado de funcionamento RedBrick na galeria do, execute os seguintes passos:**

1. No  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **do Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todas as aplicações**.

    ![O painel de aplicações da empresa][2]
    
3. Para adicionar a nova aplicação, clique em **nova aplicação** botão no topo da caixa de diálogo.

    ![O novo botão de aplicação][3]

4. Na caixa de pesquisa, escreva **RedBrick do Estado de funcionamento**, selecione **estado de funcionamento RedBrick** partir do painel de resultados, em seguida, clique em **adicionar** botão para adicionar a aplicação.

    ![Estado de funcionamento redBrick na lista de resultados](./media/active-directory-saas-redbrickhealth-tutorial/tutorial_redbrickhealth_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD-início de sessão único

Nesta secção, configure e teste do Azure AD-início de sessão único com o estado de funcionamento RedBrick com base num utilizador de teste chamado "Britta Simon".

Para início de sessão trabalhar, do Azure AD tem de saber o que o utilizador homólogo no estado de funcionamento RedBrick é um utilizador no Azure AD. Por outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no estado de funcionamento RedBrick tem de ser estabelecida.

No estado de funcionamento RedBrick, atribua o valor do **nome de utilizador** no Azure AD como o valor a **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD-início de sessão único com o estado de funcionamento RedBrick, tem de concluir os blocos modulares seguintes:

1. **[Configurar o Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)**  - para permitir aos utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD-início de sessão único com Britta Simon.
3. **[Criar um utilizador de teste do Estado de funcionamento RedBrick](#create-a-redbrick-health-test-user)**  - para ter um homólogo de Britta Simon no estado de funcionamento RedBrick que está ligada a representação do Azure AD do utilizador.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar Britta Simon utilizar o Azure AD-início de sessão único.
5. **[Teste o início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD-início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão no portal do Azure e configurar o início de sessão único na sua aplicação de estado de funcionamento RedBrick.

**Para configurar o Azure AD-início de sessão único com o estado de funcionamento RedBrick, execute os seguintes passos:**

1. No portal do Azure, no **estado de funcionamento RedBrick** página de integração de aplicações, clique em **de sessão único-**.

    ![Configurar a ligação de início de sessão único][4]

2. No **de sessão único-** caixa de diálogo, selecione **modo** como **baseados em SAML início de sessão** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/active-directory-saas-redbrickhealth-tutorial/tutorial_redbrickhealth_samlbase.png)

3. No **RedBrick de estado de funcionamento de domínio e os URLs** secção, execute os seguintes passos:

    ![RedBrick domínio de estado de funcionamento e os URLs únicos de informações de início de sessão](./media/active-directory-saas-redbrickhealth-tutorial/tutorial_redbrickhealth_url.png)

    a. No **identificador** caixa de texto, escreva um URL:`http://www.redbrickhealth.com`
    
    b. No **URL de resposta** caixa de texto, escreva um URL:`https://sso-intg.redbrickhealth.com/sp/ACS.saml2`
    
    Para o ambiente de produção:`https://sso.redbrickhealth.com/sp/ACS.saml2`

    c. Clique em **Mostrar avançadas definições de URL**.
    
    ![RedBrick domínio de estado de funcionamento e os URLs únicos de informações de início de sessão](./media/active-directory-saas-redbrickhealth-tutorial/tutorial_redbrickhealth_url1.png)

    d. No **reencaminhamento estado** caixa de texto, escreva um URL a utilizar o padrão do seguinte:`https://api-sso2.redbricktest.com/identity/sso/nbound?target=https://vanity9-sso2.redbrickdev.com/portal&connection=<companyname>conn1`
    
    > [!NOTE] 
    > O valor do Estado de reencaminhamento não é real. Atualize este valor com o estado real de reencaminhamento. Contacte [equipa de suporte do Estado de funcionamento RedBrick](https://home.redbrickhealth.com/contact/) para obter este valor.

4. A aplicação de estado de funcionamento RedBrick espera as asserções de SAML num formato específico, que necessita para adicionar mapeamentos de atributos personalizado à sua configuração de atributos token SAML. Estas afirmações sejam específico do cliente e depende o requisito. Exemplo as seguintes afirmações opcionais são apenas que pode configurar para a sua aplicação. Pode gerir os valores destes atributos do **atributos de utilizador** secção na página de integração de aplicações.

    ![Configurar o início de sessão único](./media/active-directory-saas-redbrickhealth-tutorial/attribute.png)

5. No **atributos de utilizador** secção no **de sessão único-** caixa de diálogo, configurar atributos token SAML, conforme mostrado na imagem acima e execute os seguintes passos:

    | Nome do Atributo | Valor do Atributo |
    | ---------------| ----------------|
    | nome principal | ********** |
    | id de cliente | ********** |
    | id do participante | ********** |
    
    > [!NOTE]
    > Estes valores destinam-se apenas a fins de referência. Tem de definir os atributos de acordo com os requisitos da sua organização. Contacte [equipa de suporte do Estado de funcionamento RedBrick](https://home.redbrickhealth.com/contact/) para obter mais informações sobre as afirmações necessárias.
    
    a. Clique em **adicionar atributo** para abrir o **adicionar atributo** caixa de diálogo.
    
    ![Configurar o início de sessão único](./media/active-directory-saas-redbrickhealth-tutorial/tutorial_attribute_04.png)
    
    ![Configurar o início de sessão único](./media/active-directory-saas-redbrickhealth-tutorial/tutorial_attribute_05.png)
    
    b. No **nome** caixa de texto, escreva o nome de atributo apresentado para essa linha.
    
    c. Do **valor** lista, digite o valor de atributo apresentado para essa linha.

    d. Deixe o **espaço de nomes** em branco.
    
    e. Clique em **OK**.

6. No **certificado de assinatura de SAML** secção, clique em **Certificate(Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![A hiperligação de transferência do certificado](./media/active-directory-saas-redbrickhealth-tutorial/tutorial_redbrickhealth_certificate.png) 

7. Clique em **guardar** botão.

    ![Configurar botão único início de sessão guardar](./media/active-directory-saas-redbrickhealth-tutorial/tutorial_general_400.png)

8. No **RedBrick de estado de funcionamento de configuração** secção, clique em **configurar o estado de funcionamento RedBrick** para abrir **configurar início de sessão** janela. Copiar o **ID de entidade de SAML** do **secção de referência rápida.**

    ![Configuração de estado de funcionamento redBrick](./media/active-directory-saas-redbrickhealth-tutorial/tutorial_redbrickhealth_configure.png) 

9. Para configurar o início de sessão único em **estado de funcionamento RedBrick** lado, terá de enviar o transferido **Certificate(Base64)** e **ID de entidade de SAML** para [RedBrick do Estado de funcionamento equipa de suporte](https://home.redbrickhealth.com/contact/). Se definir esta definição para que a ligação de SAML SSO corretamente em ambos os lados.

> [!TIP]
> Pode agora ler estas instruções dentro de uma versão concisa o [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir do **do Active Directory > aplicações da empresa** secção, basta clicar no **Single Sign-On** separador e aceder à documentação do embedded através de **configuração** secção na parte inferior. Pode ler mais sobre a funcionalidade de documentação incorporados aqui: [do Azure AD incorporado documentação]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção consiste em criar um utilizador de teste no portal do Azure chamado Britta Simon.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique em de **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/active-directory-saas-redbrickhealth-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e "Todos os utilizadores" ligações](./media/active-directory-saas-redbrickhealth-tutorial/create_aaduser_02.png)

3. Para abrir o **utilizador** caixa de diálogo, clique em **adicionar** na parte superior do **todos os utilizadores** caixa de diálogo.

    ![O botão de adição](./media/active-directory-saas-redbrickhealth-tutorial/create_aaduser_03.png)

4. No **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/active-directory-saas-redbrickhealth-tutorial/create_aaduser_04.png)

    a. No **nome** caixa, escreva **BrittaSimon**.

    b. No **nome de utilizador** caixa, escreva o endereço de e-mail do utilizador Britta Simon.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado no **palavra-passe** caixa.

    d. Clique em **Criar**.
  
### <a name="create-a-redbrick-health-test-user"></a>Criar um utilizador de teste do Estado de funcionamento RedBrick

Nesta secção, vai criar um utilizador chamado Britta Simon no estado de funcionamento RedBrick. Trabalhar com [equipa de suporte do Estado de funcionamento RedBrick](https://home.redbrickhealth.com/contact/) para adicionar os utilizadores na plataforma RedBrick estado de funcionamento. Os utilizadores tem de ser criados e ativados antes de utilizar o início de sessão único. 

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Britta Simon a utilizar Azure-início de sessão único, conceder acesso ao estado de funcionamento RedBrick.

![Atribuir a função de utilizador][200] 

**Para atribuir Britta Simon para Estado de funcionamento RedBrick, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicações e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todas as aplicações**.

    ![Atribua o utilizador][201] 

2. Na lista de aplicações, selecione **estado de funcionamento RedBrick**.

    ![A ligação de estado de funcionamento RedBrick na lista de aplicações](./media/active-directory-saas-redbrickhealth-tutorial/tutorial_redbrickhealth_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação de "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Britta Simon** na lista utilizadores.

6. Clique em **selecione** botão no **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribuir** botão no **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Teste o início de sessão único

Nesta secção, testar a configuração do Azure AD único início de sessão através do painel de acesso.

Quando clica no mosaico do Estado de funcionamento RedBrick no painel de acesso, deve obter automaticamente com sessão iniciada para a aplicação de estado de funcionamento RedBrick.
Para mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](active-directory-saas-tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-redbrickhealth-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-redbrickhealth-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-redbrickhealth-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-redbrickhealth-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-redbrickhealth-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-redbrickhealth-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-redbrickhealth-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-redbrickhealth-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-redbrickhealth-tutorial/tutorial_general_203.png

