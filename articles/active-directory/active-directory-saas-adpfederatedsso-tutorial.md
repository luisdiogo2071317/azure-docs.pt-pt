---
title: "Tutorial: Integração do Azure Active Directory com o SSO federado do ADP | Microsoft Docs"
description: "Saiba como configurar o início de sessão entre o Azure Active Directory e ADP federado SSO."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 7be5331b-0481-48f7-9d6b-619dfec657e1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/12/2018
ms.author: jeedes
ms.openlocfilehash: d2049e3bc8a35792703d06374263466ddc35b5ce
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/24/2018
---
# <a name="tutorial-azure-active-directory-integration-with-adp-federated-sso"></a>Tutorial: Integração do Azure Active Directory com o SSO federado do ADP

Neste tutorial, irá aprender a integração de SSO federado do ADP com o Azure Active Directory (Azure AD).

A integração de SSO federado do ADP com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao ADP federado SSO.
- Pode permitir aos utilizadores automaticamente obter com sessão iniciada para ADP federado SSO (Single Sign-On) com as respetivas contas do Azure AD.
- Pode gerir as contas numa localização central - portal do Azure.

Se pretender saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [que é o acesso a aplicações e início de sessão no Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com ADP federado SSO, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Uma subscrição de SSO federado ADP ' ativada

> [!NOTE]
> Para testar os passos neste tutorial, não recomendamos a utilização num ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, teste do Azure AD-início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos modulares principais:

1. Adicionar ADP SSO federado na galeria do
2. Configurar e testar o Azure AD de sessão único-

## <a name="adding-adp-federated-sso-from-the-gallery"></a>Adicionar ADP SSO federado na galeria do
Para configurar a integração de SSO federado do ADP com o Azure AD, tem de adicionar SSO federado do ADP na Galeria à sua lista de aplicações SaaS geridas.

**Para adicionar o SSO federado do ADP na galeria do, execute os seguintes passos:**

1.  Inicie sessão no seu ambiente de fornecedor de identidade do Microsoft Azure como administrador.

2. No  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **do Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

3. Navegue para **aplicações empresariais**. Em seguida, aceda a **todas as aplicações**.

    ![O painel de aplicações da empresa][2]
    
4. Para adicionar a nova aplicação, clique em **nova aplicação** botão no topo da caixa de diálogo.

    ![O novo botão de aplicação][3]

5. Na caixa de pesquisa, escreva **SSO federado do ADP**, selecione **SSO federado do ADP** partir do painel de resultados, em seguida, clique em **adicionar** botão para adicionar a aplicação.

    ![ADP SSO federado na lista de resultados](./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_adpfederatedsso_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD-início de sessão único

Nesta secção, configure e teste do Azure AD-início de sessão único com o SSO Federado de ADP com base num utilizador de teste chamado "Britta Simon".

Para início de sessão trabalhar, do Azure AD tem de saber o que o utilizador homólogo em ADP federado SSO é um utilizador no Azure AD. Por outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado em ADP federado SSO tem de ser estabelecida.

SSO federado ADP, atribuir o valor da **nome de utilizador** no Azure AD como o valor a **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD-início de sessão único com ADP federado SSO, tem de concluir os blocos modulares seguintes:

1. **[Configurar o Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)**  - para permitir aos utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD-início de sessão único com Britta Simon.
3. **[Criar um utilizador de teste de SSO federado do ADP](#create-an-adp-federated-sso-test-user)**  - para ter um homólogo de Britta Simon ADP SSO federado que está ligada a representação do Azure AD do utilizador.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar Britta Simon utilizar o Azure AD-início de sessão único.
5. **[Teste o início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD-início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão no portal do Azure e configurar o início de sessão único na sua aplicação de SSO federado do ADP.

**Para configurar o Azure AD-início de sessão único com ADP federado SSO, execute os seguintes passos:**

1. No portal do Azure, no **SSO federado do ADP** página de integração de aplicações, clique em **separador Propriedades** e execute os seguintes passos: 

    ![Propriedades de início de sessão único](./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_adpfederatedsso_prop.png)

    a. Definir o **ativado para os utilizadores de início de sessão** campo valor a **Sim**.

    b. Copiar o **URL de acesso de utilizador** e cole-a no **secção Configurar início de sessão URL**, que é explicada mais tarde no tutorial.

    c. Definir o **atribuição de utilizador necessária** campo valor a **Sim**.

    d. Definir o **Visible aos utilizadores** campo valor a **não**.

2. Clique em **de sessão único-** no **SSO federado do ADP** página de integração de aplicações.

    ![Configurar a ligação de início de sessão único][4]

3. No **de sessão único-** caixa de diálogo, selecione **modo** como **baseados em SAML início de sessão** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_adpfederatedsso_samlbase.png)

4. No **URLs e de domínio de SSO federado ADP** secção, execute os seguintes passos:

    ![Domínio de SSO federado ADP e os URLs únicos de informações de início de sessão](./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_adpfederatedsso_url.png)

    No **identificador** caixa de texto, escreva um URL:`https://fed.adp.com/` 
    
5. A aplicação de SSO federado do ADP espera as asserções de SAML num formato específico, que necessita para adicionar mapeamentos de atributos personalizado à sua configuração de atributos token SAML. A seguinte captura de ecrã mostra um exemplo para este. O nome de afirmação será sempre **"PersonImmutableID"** e o valor que podemos ter mapeado **campo IDdeEmpregado**. 

    Aqui o mapeamento de utilizador do Azure AD para SSO federado do ADP será possível efetuar a **campo IDdeEmpregado** , mas pode mapear isto para um valor diferente com base nas suas definições de aplicação. Por isso, consulte o trabalho com [equipa de suporte de ADP](https://www.adp.com/contact-us/overview.aspx) primeiro para utilizar o identificador correto de um utilizador e esse valor com o mapeamento o **"PersonImmutableID"** de afirmação.

    ![Configurar o início de sessão único](./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_adpfederatedsso_attribute.png)

6. No **atributos de utilizador** secção no **de sessão único-** caixa de diálogo, configurar atributo token SAML, conforme mostrado na imagem e efetuar os seguintes passos:
    
    | Nome do Atributo | Valor do Atributo |
    | ------------------- | -------------------- |    
    | PersonImmutableID | User.employeeid |
    
    a. Clique em **adicionar atributo** para abrir o **adicionar atributo** caixa de diálogo.

    ![Configurar o início de sessão único](./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_attribute_04.png)

    ![Configurar o início de sessão único](./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_attribute_05.png)

    b. No **nome** caixa de texto, escreva o nome de atributo apresentado para essa linha.

    c. Do **valor** lista, digite o valor de atributo apresentado para essa linha.
    
    d. Clique em **OK**.

    > [!NOTE] 
    > Antes de poder configurar a asserção SAML, terá de contactar o seu [equipa de suporte de ADP](https://www.adp.com/contact-us/overview.aspx) e pedir o valor do atributo de identificador exclusivo para o seu inquilino. É necessário este valor para configurar a afirmação personalizada para a sua aplicação. 

7. No **certificado de assinatura de SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![A hiperligação de transferência do certificado](./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_adpfederatedsso_certificate.png) 

8. Para configurar o início de sessão único em **SSO federado do ADP** lado, tem de carregar o transferido **XML de metadados** no [Web site de SSO federado do ADP](https://adpfedsso.adp.com/public/login/index.fcc).

> [!NOTE]  
> Este processo pode demorar de alguns dias. 

### <a name="configure-your-adp-services-for-federated-access"></a>Configurar os serviços de ADP para acesso federado

>[!Important]
> Os seus empregados que precisam de acesso federado aos seus serviços ADP tem de ser atribuídos para a aplicação de serviço ADP e posteriormente, os utilizadores têm ser reatribuídos para o serviço de ADP específico.
Após a receção de confirmação do seu representante ADP, configure os seus ADP serviços e atribuir/gerir utilizadores para controlar o acesso de utilizador para o serviço de ADP específico.

1. No  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **do Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todas as aplicações**.

    ![O painel de aplicações da empresa][2]
    
3. Para adicionar a nova aplicação, clique em **nova aplicação** botão no topo da caixa de diálogo.

    ![O novo botão de aplicação][3]

4. Na caixa de pesquisa, escreva **SSO federado do ADP**, selecione **SSO federado do ADP** partir do painel de resultados, em seguida, clique em **adicionar** botão para adicionar a aplicação.

    ![ADP SSO federado na lista de resultados](./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_adpfederatedsso_addservicegallery.png)

5. No portal do Azure, no seu **SSO federado do ADP** página de integração de aplicações, clique em **separador Propriedades** e execute os seguintes passos:  

    ![Linkedproperties de início de sessão único](./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_adpfederatedsso_linkedproperties.png)

    a.  Definir o **ativado para os utilizadores de início de sessão** campo valor a **Sim**.

    b.  Definir o **atribuição de utilizador necessária** campo valor a **Sim**.

    c.  Definir o **Visible aos utilizadores** campo valor a **Sim**.

6. Clique em **de sessão único-** no **SSO federado do ADP** página de integração de aplicações.

    ![Configurar a ligação de início de sessão único][4]

7. No **de sessão único-** caixa de diálogo, selecione **modo** como **ligado início de sessão**. ligar a sua aplicação para **SSO federado do ADP**.

    ![Início de sessão ligado](./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_adpfederatedsso_linked.png)

8. Navegue para o **URL de início de sessão configurar** secção, execute os seguintes passos:

    ![Prop de início de sessão único](./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_adpfederatedsso_linkedsignon.png)
                                                              
    a. Colar o **URL de acesso de utilizador**, que copiou do acima **separador Propriedades** (a partir da aplicação de SSO federado do ADP principal).
                                                             
    b. Seguem-se a 5 as aplicações que suportam diferentes **URLs de estado de reencaminhamento**. Tem a acrescentar apropriados **URL de estado de reencaminhamento** manualmente para o valor para a aplicação em particular o **URL de acesso de utilizador**.
    
    * **Agora força de trabalho de ADP**
        
        `<User access URL>?Relay State=https://fed.adp.com/saml/fedlanding.html?WFN`

    * **Agora, força de trabalho ADP melhorada tempo**
        
        `<User access URL>?Relay State=https://fed.adp.com/saml/fedlanding.html?EETDC2`
    
    * **ADP Vantage HCM**
        
        `<User access URL>?Relay State=https://fed.adp.com/saml/fedlanding.html?ADPVANTAGE`

    * **ADP Enterprise HR**

        `<User access URL>?Relay State=https://fed.adp.com/saml/fedlanding.html?PORTAL`

    * **MyADP**

        `<User access URL>?Relay State=https://fed.adp.com/saml/fedlanding.html?REDBOX`

9. **Guardar** as suas alterações.

10. Após a receção de confirmação do seu representante ADP, iniciar o teste com um ou dois utilizadores.

    a. Atribua alguns utilizadores para o serviço de ADP aplicação para testar o acesso federado.

    b. Teste está concluída com êxito quando os utilizadores aceder à aplicação de serviço ADP na galeria e podem aceder ao seu serviço ADP.
 
11. No caso de confirmação de um teste com êxito, atribua o serviço de ADP federado para utilizadores individuais ou grupos de utilizadores, que é explicada mais tarde no tutorial e implementar para os seus empregados. 

> [!TIP]
> Pode agora ler estas instruções dentro de uma versão concisa o [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir do **do Active Directory > aplicações da empresa** secção, basta clicar no **Single Sign-On** separador e aceder à documentação do embedded através de **configuração** secção na parte inferior. Pode ler mais sobre a funcionalidade de documentação incorporados aqui: [do Azure AD incorporado documentação]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção consiste em criar um utilizador de teste no portal do Azure chamado Britta Simon.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique em de **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/active-directory-saas-adpfederatedsso-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e "Todos os utilizadores" ligações](./media/active-directory-saas-adpfederatedsso-tutorial/create_aaduser_02.png)

3. Para abrir o **utilizador** caixa de diálogo, clique em **adicionar** na parte superior do **todos os utilizadores** caixa de diálogo.

    ![O botão de adição](./media/active-directory-saas-adpfederatedsso-tutorial/create_aaduser_03.png)

4. No **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/active-directory-saas-adpfederatedsso-tutorial/create_aaduser_04.png)

    a. No **nome** caixa, escreva **BrittaSimon**.

    b. No **nome de utilizador** caixa, escreva o endereço de e-mail do utilizador Britta Simon.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado no **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-an-adp-federated-sso-test-user"></a>Criar um utilizador de teste de SSO federado do ADP

O objetivo desta secção consiste em criar um utilizador chamado Britta Simon em ADP federado SSO. Trabalhar com [equipa de suporte de ADP](https://www.adp.com/contact-us/overview.aspx) para adicionar os utilizadores na conta de SSO federado do ADP.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Britta Simon utilizar o Azure-início de sessão único, concedendo acesso para ADP federado SSO.

![Atribuir a função de utilizador][200] 

**Para atribuir Britta Simon para SSO federado do ADP, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicações e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todas as aplicações**.

    ![Atribua o utilizador][201] 

2. Na lista de aplicações, selecione **SSO federado do ADP**.

    ![A ligação de SSO federado do ADP na lista de aplicações](./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_adpfederatedsso_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação de "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Britta Simon** na lista utilizadores.

6. Clique em **selecione** botão no **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribuir** botão no **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Teste o início de sessão único

Nesta secção, testar a configuração do Azure AD único início de sessão através do painel de acesso.

Quando clica no mosaico de SSO federado do ADP no painel de acesso, deve obter automaticamente com sessão iniciada para a aplicação de SSO federado do ADP.
Para mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](active-directory-saas-tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-adpfederatedsso-tutorial/tutorial_general_203.png

