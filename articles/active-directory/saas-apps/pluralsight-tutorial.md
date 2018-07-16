---
title: 'Tutorial: Integração do Azure Active Directory com a Pluralsight | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e da Pluralsight.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 4c3f07d2-4e1f-4ea3-9025-c663f1f2b7b4
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/18/2017
ms.author: jeedes
ms.openlocfilehash: 30cf33de7385add20ec5c255bf20b4e3cb7580fc
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39051521"
---
# <a name="tutorial-azure-active-directory-integration-with-pluralsight"></a>Tutorial: Integração do Azure Active Directory com a Pluralsight

Neste tutorial, saiba como integrar o Pluralsight com o Azure Active Directory (Azure AD).

Integrar o Pluralsight no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD, quem tem acesso a Pluralsight.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para a Pluralsight (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com a Pluralsight, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Pluralsight logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Pluralsight da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-pluralsight-from-the-gallery"></a>Adicionando Pluralsight da Galeria
Para configurar a integração da Pluralsight para o Azure AD, terá de adicionar Pluralsight a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Pluralsight a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

4. Na caixa de pesquisa, escreva **Pluralsight**, selecione **Pluralsight** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Pluralsight na lista de resultados](./media/pluralsight-tutorial/tutorial_pluralsight_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com a Pluralsight com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de equivalente na Pluralsight a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado na Pluralsight deve ser estabelecido.

Na Pluralsight, atribua o valor do **nome de utilizador** no Azure AD como o valor da **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com a Pluralsight, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste do Pluralsight](#create-a-pluralsight-test-user)**  - para ter um equivalente da Eduarda Almeida na Pluralsight que está ligada à representação de utilizador do Azure AD.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo da Pluralsight.

**Para configurar o Azure AD início de sessão único com a Pluralsight, execute os seguintes passos:**

1. No portal do Azure, sobre o **Pluralsight** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/pluralsight-tutorial/tutorial_pluralsight_samlbase.png)

3. Sobre o **Pluralsight domínio e URLs** secção, execute os seguintes passos:

    ![Domínio da Pluralsight e URLs únicas início de sessão em informações](./media/pluralsight-tutorial/tutorial_pluralsight_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<instancename>.pluralsight.com/sso/<companyname>`

    b. Na **identificador** caixa de texto, escreva o URL: `www.pluralsight.com`

    c. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://<instancename>.pluralsight.com/sp/ACS.saml2`
     
    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o URL de resposta e o URL de início de sessão real. Contacte [equipa de suporte de cliente do Pluralsight](mailto:support@pluralsight.com) obter esses valores. 

4. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![O link de download de certificado](./media/pluralsight-tutorial/tutorial_pluralsight_certificate.png) 

5. O objetivo desta secção é para ativar o Azure AD início de sessão único no portal do Azure e para configurar o SSO no aplicativo da Pluralsight.

    O aplicativo de Pluralsight espera que as asserções SAML num formato específico, o que requer a adição de mapeamentos de atributos personalizado à sua configuração de atributos de token SAML. Captura de ecrã seguinte mostra um exemplo disso.

    ![Configurar o início de sessão único](./media/pluralsight-tutorial/tutorial_pluralsight_attribute.png)

    >[!NOTE]
    >Também pode adicionar os **"ID exclusivo"** atributo com o valor apropriado, como EmployeeID ou outra coisa que lhe seja conveniente para a sua organização. Observe também que esse não é o atributo necessário; No entanto, pode adicioná-lo para identificar o utilizador exclusivo. 

6. Para adicionar necessários **atributos de token SAML**, para cada linha mostrada na tabela abaixo, execute os seguintes passos:
   
   | Nome do Atributo | Valor do Atributo |
   | ---| --- |
   | Nome Próprio |User.givenName |
   | Apelido |User.Surname |
   | Email |User.Mail |
   
   a. Clique em **adicione o atributo de utilizador** para abrir o **adicionar atributo de utilizador** caixa de diálogo.
    
     ![Configurar o início de sessão único](./media/pluralsight-tutorial/tutorial_pluralsight_addattribute.png)
  
   b. Na **nome de atributo** caixa de texto, escreva o nome de atributo apresentado para essa linha.
  
   c. Partir do **valor do atributo** , selecione o valor do atributo apresentado para essa linha.
  
   d. Clique em **OK**.    

7. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/pluralsight-tutorial/tutorial_general_400.png)

8. Para obter SSO configurado para a sua aplicação, contacte [Pluralsight Professional serviços](mailTo:professionalservices@pluralsight.com) da Equipe e fornecer o ficheiro de metadados baixado.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/pluralsight-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/pluralsight-tutorial/create_aaduser_02.png)

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/pluralsight-tutorial/create_aaduser_03.png)

4. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/pluralsight-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-pluralsight-test-user"></a>Criar um utilizador de teste do Pluralsight

O objetivo desta secção é criar um utilizador chamado Eduarda Almeida na Pluralsight. Trabalhe em conjunto com [equipa de suporte de cliente do Pluralsight](mailto:support@pluralsight.com) para adicionar os utilizadores na conta da Pluralsight.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único, concedendo acesso para a Pluralsight.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a Pluralsight, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **Pluralsight**.

    ![A ligação da Pluralsight, na lista de aplicações](./media/pluralsight-tutorial/tutorial_pluralsight_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico da Pluralsight, no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação da Pluralsight.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/pluralsight-tutorial/tutorial_general_01.png
[2]: ./media/pluralsight-tutorial/tutorial_general_02.png
[3]: ./media/pluralsight-tutorial/tutorial_general_03.png
[4]: ./media/pluralsight-tutorial/tutorial_general_04.png

[100]: ./media/pluralsight-tutorial/tutorial_general_100.png

[200]: ./media/pluralsight-tutorial/tutorial_general_200.png
[201]: ./media/pluralsight-tutorial/tutorial_general_201.png
[202]: ./media/pluralsight-tutorial/tutorial_general_202.png
[203]: ./media/pluralsight-tutorial/tutorial_general_203.png

