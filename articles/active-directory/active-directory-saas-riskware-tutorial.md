---
title: 'Tutorial: Integração do Azure Active Directory com Riskware | Microsoft Docs'
description: Saiba como configurar o início de sessão entre o Azure Active Directory e Riskware.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 81866167-b163-4695-8978-fd29a25dac7a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2018
ms.author: jeedes
ms.openlocfilehash: bc3b4c75875661de65f559df9a83c3c92e34304e
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655282"
---
# <a name="tutorial-azure-active-directory-integration-with-riskware"></a>Tutorial: Integração do Azure Active Directory com Riskware

Neste tutorial, irá aprender a integrar Riskware com o Azure Active Directory (Azure AD).

Integrar Riskware com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Riskware.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Riskware (Single Sign-On) com as respetivas contas do Azure AD.
- Pode gerir as contas numa localização central - portal do Azure.

Se pretender saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [que é o acesso a aplicações e início de sessão no Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Riskware, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Riskware-início de sessão único ativada subscrição

> [!NOTE]
> Para testar os passos neste tutorial, não recomendamos a utilização num ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, teste do Azure AD-início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos modulares principais:

1. Adicionar Riskware a partir da Galeria
2. Configurar e testar o Azure AD de sessão único-

## <a name="adding-riskware-from-the-gallery"></a>Adicionar Riskware a partir da Galeria
Para configurar a integração de Riskware com o Azure AD, terá de adicionar Riskware a partir da Galeria à sua lista de aplicações SaaS geridas.

**Para adicionar Riskware a partir da galeria, execute os seguintes passos:**

1. No  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **do Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todas as aplicações**.

    ![O painel de aplicações da empresa][2]
    
3. Para adicionar a nova aplicação, clique em **nova aplicação** botão no topo da caixa de diálogo.

    ![O novo botão de aplicação][3]

4. Na caixa de pesquisa, escreva **Riskware**, selecione **Riskware** partir do painel de resultados, em seguida, clique em **adicionar** botão para adicionar a aplicação.

    ![Riskware na lista de resultados](./media/active-directory-saas-riskware-tutorial/tutorial_riskware_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD-início de sessão único

Nesta secção, configure e teste do Azure AD-início de sessão único com Riskware com base num utilizador de teste chamado "Britta Simon".

Para início de sessão trabalhar, do Azure AD tem de saber o que o utilizador homólogo no Riskware é um utilizador no Azure AD. Por outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Riskware tem de ser estabelecida.

Para configurar e testar o Azure AD-início de sessão único com Riskware, tem de concluir os blocos modulares seguintes:

1. **[Configurar o Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)**  - para permitir aos utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD-início de sessão único com Britta Simon.
3. **[Criar um utilizador de teste Riskware](#create-a-riskware-test-user)**  - para ter um homólogo de Britta Simon Riskware que está ligada a representação do Azure AD do utilizador.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar Britta Simon utilizar o Azure AD-início de sessão único.
5. **[Teste o início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD-início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão no portal do Azure e configurar o início de sessão único na sua aplicação Riskware.

**Para configurar o Azure AD-início de sessão único com Riskware, execute os seguintes passos:**

1. No portal do Azure, no **Riskware** página de integração de aplicações, clique em **de sessão único-**.

    ![Configurar a ligação de início de sessão único][4]

2. No **de sessão único-** caixa de diálogo, selecione **modo** como **baseados em SAML início de sessão** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/active-directory-saas-riskware-tutorial/tutorial_riskware_samlbase.png)

3. No **Riskware domínio e os URLs** secção, execute os seguintes passos:

    ![Domínio Riskware e os URLs únicos de informações de início de sessão](./media/active-directory-saas-riskware-tutorial/tutorial_riskware_url.png)

    a. No **iniciar sessão no URL** caixa de texto, escreva um URL a utilizar o padrão do seguinte:
    | Ambiente| Padrão de URL|
    |--|--|
    | UAT|  `https://riskcloud.net/uat?ccode=<COMPANYCODE>` |
    | PROD| `https://riskcloud.net/prod?ccode=<COMPANYCODE>` |
    | DEMO| `https://riskcloud.net/demo?ccode=<COMPANYCODE>` | 
    |||

    b. No **identificador (ID de entidade)** caixa de texto, escreva um URL:
    | Ambiente| Padrão de URL|
    |--|--|
    | UAT| `https://riskcloud.net/uat` |
    | PROD| `https://riskcloud.net/prod` |
    | DEMO| `https://riskcloud.net/demo` | 
    |||

    > [!NOTE] 
    > O início de sessão no valor de URL não é real. Atualize o valor com o URL de início de sessão real. Contacte [equipa de suporte de cliente Riskware](mailto:support@pansoftware.com.au) para obter o valor.

4. No **certificado de assinatura de SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![A hiperligação de transferência do certificado](./media/active-directory-saas-riskware-tutorial/tutorial_riskware_certificate.png) 

5. Clique em **guardar** botão.

    ![Configurar botão único início de sessão guardar](./media/active-directory-saas-riskware-tutorial/tutorial_general_400.png)

6. No **Riskware configuração** secção, clique em **configurar Riskware** para abrir **configurar início de sessão** janela. Copiar o **Sign-Out URL e o único início de sessão no URL do serviço SAML** do **secção de referência rápida.**

    ![Configuração de Riskware](./media/active-directory-saas-riskware-tutorial/tutorial_riskware_configure.png) 

7. Numa janela do browser web diferente, inicie sessão no site da sua empresa Riskware como administrador.

8. No canto superior direito, clique em **manutenção** para abrir a página de manutenção. 

    ![Configurações de Riskware manter](./media/active-directory-saas-riskware-tutorial/tutorial_riskware_maintain.png)

9. Na página de manutenção, clique em **autenticação**.

    ![Configuração de Riskware authen](./media/active-directory-saas-riskware-tutorial/tutorial_riskware_authen.png)

10. No **configuração da autenticação** página, execute os seguintes passos:

    ![Configuração de Riskware authenconfig](./media/active-directory-saas-riskware-tutorial/tutorial_riskware_config.png)

    a. Selecione **tipo** como **SAML** para autenticação.

    b. No **código** caixa de texto, escreva o seu código como AZURE_UAT.

    c. No **Descrição** caixa de texto, digite a descrição do seu, como a configuração do AZURE para SSO.

    d. No **início de sessão único na página** caixa de texto, cole o valor de **único início de sessão no URL do serviço SAML**, que copiou do portal do Azure.

    e. No **terminar página sessão** caixa de texto, cole o valor de **Sign-Out URL**, que copiou do portal do Azure.

    f. No **campo de formulário Post** caixa de texto, escreva o nome do campo presente na resposta Post que contém SAML como SamlResponse

    g. No **nome da Tag XML identidade** caixa de texto, o atributo tipo, que contém o identificador exclusivo na resposta SAML como NameID.

    h. Abra o transferido **Xml de metadados** no portal do Azure no bloco de notas, copie o certificado do ficheiro de metadados e cole-o para o **certificado** caixa de texto
    
    i. No **URL de consumidor** caixa de texto, colar o valor de **URL de resposta**, que pode aproveitar a equipa de suporte.

    j. No **emissor** caixa de texto, cole o valor de **identificador**, que pode aproveitar a equipa de suporte.

    > [!Note]
    > Contacte [equipa de suporte de cliente Riskware](mailto:support@pansoftware.com.au) para obter estes valores

    k. Selecione **após a utilização** para passar o pedido de SAML como um parâmetro de post.

    l. Selecione **utilize SAML pedido** passar SSO permitir pedidos de SAML para SP2 iniciada.

    m. Clique em **Guardar**.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção consiste em criar um utilizador de teste no portal do Azure chamado Britta Simon.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique em de **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/active-directory-saas-riskware-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e "Todos os utilizadores" ligações](./media/active-directory-saas-riskware-tutorial/create_aaduser_02.png)

3. Para abrir o **utilizador** caixa de diálogo, clique em **adicionar** na parte superior do **todos os utilizadores** caixa de diálogo.

    ![O botão de adição](./media/active-directory-saas-riskware-tutorial/create_aaduser_03.png)

4. No **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/active-directory-saas-riskware-tutorial/create_aaduser_04.png)

    a. No **nome** caixa, escreva **BrittaSimon**.

    b. No **nome de utilizador** caixa, escreva o endereço de e-mail do utilizador Britta Simon.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado no **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-riskware-test-user"></a>Criar um utilizador de teste Riskware

Para permitir que os utilizadores do Azure AD iniciem sessão nos Riskware, têm de ser aprovisionados para Riskware. No Riskware, o aprovisionamento é uma tarefa manual.

**Para Aprovisionar uma conta de utilizador, execute os seguintes passos:**

1. Inicie sessão no Riskware como um administrador de segurança.

2. No canto superior direito, clique em **manutenção** para abrir a página de manutenção. 

    ![Configuração de Riskware mantém](./media/active-directory-saas-riskware-tutorial/tutorial_riskware_maintain.png)

3. Na página de manutenção, clique em **pessoas**.

    ![Pessoas Riskware configuração](./media/active-directory-saas-riskware-tutorial/tutorial_riskware_people.png)

4. No **detalhes** separador, execute os seguintes passos:
    
    ![Detalhes de configuração de Riskware](./media/active-directory-saas-riskware-tutorial/tutorial_riskware_details.png)

    a. Selecione **pessoa tipo** como aos empregados.

    b. No **nome próprio** caixa de texto, introduza o nome de utilizador como **Britta**.

    c. No **Apelido** caixa de texto, introduza o apelido do utilizador, como **Simon**.

5. No **segurança** separador, execute os seguintes passos:    

    ![Segurança de configuração de Riskware](./media/active-directory-saas-riskware-tutorial/tutorial_riskware_security.png)

    a. Em **autenticação** secção, selecione o **autenticação** modo, o que poderá ter de configuração, como a configuração do AZURE para SSO.

    b. Em **detalhes de início de sessão** na secção de **ID de utilizador** caixa de texto, introduza o e-mail do utilizador, como **brittasimon@contoso.com**.

    c. No **palavra-passe** caixa de texto, introduza a palavra-passe do utilizador.

6. No **informático** separador, execute os seguintes passos:

    ![Configuração de Riskware org](./media/active-directory-saas-riskware-tutorial/tutorial_riskware_org.png)

    a. Em **informático** secção, selecione o informático como **Level1** informático.
    
    b. Em **à área de trabalho principal da pessoa** na secção de **localização** caixa de texto, escreva a localização.

    c. Em **empregado** secção, selecione **empregado estado** como casuais.

7. Clique em **Guardar**.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Britta Simon utilizar o Azure-início de sessão único, concedendo acesso para Riskware.

![Atribuir a função de utilizador][200] 

**Para atribuir Britta Simon a Riskware, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicações e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todas as aplicações**.

    ![Atribua o utilizador][201] 

2. Na lista de aplicações, selecione **Riskware**.

    ![A ligação de Riskware na lista de aplicações](./media/active-directory-saas-riskware-tutorial/tutorial_riskware_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação de "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Britta Simon** na lista utilizadores.

6. Clique em **selecione** botão no **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribuir** botão no **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, testar a configuração do Azure AD único início de sessão através do painel de acesso.

Quando clica no mosaico Riskware no painel de acesso, deve obter automaticamente com sessão iniciada para a aplicação de Riskware.
Para mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](active-directory-saas-tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-riskware-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-riskware-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-riskware-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-riskware-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-riskware-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-riskware-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-riskware-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-riskware-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-riskware-tutorial/tutorial_general_203.png

