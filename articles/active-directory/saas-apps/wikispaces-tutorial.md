---
title: 'Tutorial: Integração do Azure Active Directory com Wikispaces | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Wikispaces.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 665b95aa-f7f5-4406-9e2a-6fc299a1599c
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: 920777aa56d9ea9c619815b99b234b66bd676b44
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39050576"
---
# <a name="tutorial-azure-active-directory-integration-with-wikispaces"></a>Tutorial: Integração do Azure Active Directory com Wikispaces

Neste tutorial, saiba como integrar Wikispaces com o Azure Active Directory (Azure AD).

Integrar Wikispaces no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Wikispaces
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Wikispaces (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Wikispaces, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Wikispaces logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Wikispaces da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-wikispaces-from-the-gallery"></a>Adicionando Wikispaces da Galeria
Para configurar a integração do Wikispaces com o Azure AD, terá de adicionar Wikispaces a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Wikispaces a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

4. Na caixa de pesquisa, escreva **Wikispaces**.

    ![Criar um utilizador de teste do Azure AD](./media/wikispaces-tutorial/tutorial_wikispaces_search.png)

5. No painel de resultados, selecione **Wikispaces**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/wikispaces-tutorial/tutorial_wikispaces_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com Wikispaces com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Wikispaces a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Wikispaces deve ser estabelecido.

Wikispaces, atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com Wikispaces, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste Wikispaces](#creating-a-wikispaces-test-user)**  - para ter um equivalente da Eduarda Almeida na Wikispaces que está ligado à representação de utilizador do Azure AD.
4. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo Wikispaces.

**Para configurar o Azure AD início de sessão único com Wikispaces, execute os seguintes passos:**

1. No portal do Azure, sobre o **Wikispaces** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/wikispaces-tutorial/tutorial_wikispaces_samlbase.png)

3. Sobre o **Wikispaces domínio e URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/wikispaces-tutorial/tutorial_wikispaces_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<companyname>.wikispaces.net`

    b. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://session.wikispaces.net/<instancename>`

    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o URL de início de sessão e o identificador real. Contacte [equipa de suporte de cliente Wikispaces](https://www.wikispaces.com/site/help) obter esses valores. 

4. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![Configurar o início de sessão único](./media/wikispaces-tutorial/tutorial_wikispaces_certificate.png) 

5. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/wikispaces-tutorial/tutorial_general_400.png)

6. Para configurar o início de sessão único num **Wikispaces** lado, terá de enviar o transferido **XML de metadados** para [equipa de suporte de Wikispaces](https://www.wikispaces.com/site/help). Receberá uma notificação quando a configuração foi concluída.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/wikispaces-tutorial/create_aaduser_01.png) 

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/wikispaces-tutorial/create_aaduser_02.png) 

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/wikispaces-tutorial/create_aaduser_03.png) 

4. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/wikispaces-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-a-wikispaces-test-user"></a>Criar um utilizador de teste Wikispaces

Para habilitar os utilizadores do Azure AD iniciar sessão no Wikispaces, tem de ser aprovisionados em Wikispaces. No caso de Wikispaces, o aprovisionamento é uma tarefa manual.

### <a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Para aprovisionar contas de utilizador, execute os seguintes passos:
1. Inicie sessão no seu **Wikispaces** site da empresa como administrador.

2. Aceda a **membros**.
   
    ![Os membros](./media/wikispaces-tutorial/ic787193.png "membros")

3. Clique nas **convidar pessoas**.
   
    ![Convidar pessoas](./media/wikispaces-tutorial/ic787194.png "convidar pessoas")

4. Na **convidar pessoas** secção, execute os seguintes passos:
   
    ![Convidar pessoas](./media/wikispaces-tutorial/ic787208.png "convidar pessoas")
   
    a. Tipo de **nomes de utilizador ou endereço de E-Mail** de uma conta do AAD válida que pretende aprovisionar em caixas de texto relacionadas.
   
    b. Clique em **enviar**.  
      
    > [!NOTE]
    > O titular da conta do Azure Active Directory recebe um e-mail, incluindo uma ligação para confirmar a conta até se tornar Active Directory.
    
> [!NOTE]
> Pode utilizar quaisquer outras Wikispaces utilizador conta criação ferramentas ou APIs fornecidas pelo Wikispaces para aprovisionar contas de utilizador do AAD.

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para Wikispaces.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida a Wikispaces, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **Wikispaces**.

    ![Configurar o início de sessão único](./media/wikispaces-tutorial/tutorial_wikispaces_app.png) 

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico Wikispaces no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo Wikispaces.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/wikispaces-tutorial/tutorial_general_01.png
[2]: ./media/wikispaces-tutorial/tutorial_general_02.png
[3]: ./media/wikispaces-tutorial/tutorial_general_03.png
[4]: ./media/wikispaces-tutorial/tutorial_general_04.png

[100]: ./media/wikispaces-tutorial/tutorial_general_100.png

[200]: ./media/wikispaces-tutorial/tutorial_general_200.png
[201]: ./media/wikispaces-tutorial/tutorial_general_201.png
[202]: ./media/wikispaces-tutorial/tutorial_general_202.png
[203]: ./media/wikispaces-tutorial/tutorial_general_203.png

