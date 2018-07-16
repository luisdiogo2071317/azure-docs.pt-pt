---
title: 'Tutorial: Integração do Azure Active Directory com Egnyte | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Egnyte.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 8c2101d4-1779-4b36-8464-5c1ff780da18
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2017
ms.author: jeedes
ms.openlocfilehash: d52672099c309dfeac641cb3aedaf32c7230b676
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39047764"
---
# <a name="tutorial-azure-active-directory-integration-with-egnyte"></a>Tutorial: Integração do Azure Active Directory com Egnyte

Neste tutorial, saiba como integrar Egnyte com o Azure Active Directory (Azure AD).

Integrar Egnyte no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Egnyte
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Egnyte (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Egnyte, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Egnyte logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Egnyte da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-egnyte-from-the-gallery"></a>Adicionando Egnyte da Galeria
Para configurar a integração do Egnyte com o Azure AD, terá de adicionar Egnyte a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Egnyte a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

4. Na caixa de pesquisa, escreva **Egnyte**.

    ![Criar um utilizador de teste do Azure AD](./media/egnyte-tutorial/tutorial_egnyte_search.png)

5. No painel de resultados, selecione **Egnyte**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/egnyte-tutorial/tutorial_egnyte_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com Egnyte com base num utilizador de teste chamado "Eduarda Almeida."

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Egnyte a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Egnyte deve ser estabelecido.

Egnyte, atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com Egnyte, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste Egnyte](#creating-an-egnyte-test-user)**  - para ter um equivalente da Eduarda Almeida na Egnyte que está ligado à representação de utilizador do Azure AD.
4. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo Egnyte.

**Para configurar o Azure AD início de sessão único com Egnyte, execute os seguintes passos:**

1. No portal do Azure, sobre o **Egnyte** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/egnyte-tutorial/tutorial_egnyte_samlbase.png)

3. Sobre o **Egnyte domínio e URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/egnyte-tutorial/tutorial_egnyte_url.png)

    Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<companyname>.egnyte.com`

    > [!NOTE] 
    > Este valor não é real. Atualize este valor com o URL de início de sessão real. Contacte [equipa de suporte de cliente Egnyte](https://www.egnyte.com/corp/contact_egnyte.html) para obter este valor. 
 
4. Sobre o **certificado de assinatura SAML** secção, clique em **Certificate(Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![Configurar o início de sessão único](./media/egnyte-tutorial/tutorial_egnyte_certificate.png) 

5. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/egnyte-tutorial/tutorial_general_400.png)

6. Sobre o **Egnyte configuração** secção, clique em **configurar Egnyte** para abrir **configurar início de sessão** janela. Cópia a **ID de entidade de SAML e SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configurar o início de sessão único](./media/egnyte-tutorial/tutorial_egnyte_configure.png) 

7. Numa janela do browser web diferente, inicie sessão no site da sua empresa Egnyte como administrador.

8. Clique em **definições**.
   
   ![As definições](./media/egnyte-tutorial/ic787819.png "definições")

9. No menu, clique em **definições**.

   ![As definições](./media/egnyte-tutorial/ic787820.png "definições")

10. Clique nas **Configuration** separador e, em seguida, clique em **segurança**.

    ![Segurança](./media/egnyte-tutorial/ic787821.png "segurança")

11. Na **autenticação de início de sessão único** secção, execute os seguintes passos:

    ![Na autenticação de início de sessão único](./media/egnyte-tutorial/ic787822.png "na autenticação de início de sessão único")   
    
    a. Como **autenticação de início de sessão único**, selecione **SAML 2.0**.
   
    b. Como **fornecedor de identidade**, selecione **AzureAD**.
   
    c. Colar **SAML único início de sessão no URL do serviço** copiados a partir do portal do Azure para o **URL de início de sessão do fornecedor de identidade** caixa de texto.
   
    d. Colar **ID de entidade de SAML** que copiou do portal do Azure para o **ID de entidade do fornecedor de identidade** caixa de texto.
      
    e. Abra o certificado com codificação base 64 no bloco de notas transferido a partir do portal do Azure, copie o conteúdo do mesmo para a área de transferência e, em seguida, cole-os para o **certificado do fornecedor de identidade** caixa de texto.
   
    f. Como **predefinido o mapeamento de utilizador**, selecione **endereço de E-Mail**.
   
    g. Como **utilizar o valor de emissor específicos de domínio**, selecione **desativada**.
   
    h. Clique em **Guardar**.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/egnyte-tutorial/create_aaduser_01.png) 

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/egnyte-tutorial/create_aaduser_02.png) 

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/egnyte-tutorial/create_aaduser_03.png) 

4. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/egnyte-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-an-egnyte-test-user"></a>Criar um utilizador de teste Egnyte

Para ativar a utilizadores do Azure AD iniciar sessão no Egnyte, tem de ser aprovisionados em Egnyte. No caso de Egnyte, o aprovisionamento é uma tarefa manual.

**Para aprovisionar contas de utilizador, execute os seguintes passos:**

1. Inicie sessão no seu **Egnyte** site da empresa como administrador.

2. Aceda a **configurações \> os utilizadores e grupos**.

3. Clique em **adicionar novo utilizador**e, em seguida, selecione o tipo de utilizador que pretende adicionar.
   
   ![Os utilizadores](./media/egnyte-tutorial/ic787824.png "utilizadores")

4. Na **novo utilizador padrão** secção, execute os seguintes passos:
   
   ![Novo utilizador padrão](./media/egnyte-tutorial/ic787825.png "novo utilizador padrão")   

   a. Tipo de **E-Mail**, **Username**e outros detalhes de uma conta válida do Azure Active Directory que pretende aprovisionar.
   
   b. Clique em **Guardar**.
    
    >[!NOTE]
    >O titular da conta do Azure Active Directory irão receber um e-mail de notificação.
    >

>[!NOTE]
>Pode utilizar quaisquer outras Egnyte utilizador conta criação ferramentas ou APIs fornecidas pelo Egnyte para aprovisionar contas de utilizador do AAD.
> 

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para Egnyte.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida a Egnyte, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **Egnyte**.

    ![Configurar o início de sessão único](./media/egnyte-tutorial/tutorial_egnyte_app.png) 

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico Egnyte no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo Egnyte.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/egnyte-tutorial/tutorial_general_01.png
[2]: ./media/egnyte-tutorial/tutorial_general_02.png
[3]: ./media/egnyte-tutorial/tutorial_general_03.png
[4]: ./media/egnyte-tutorial/tutorial_general_04.png

[100]: ./media/egnyte-tutorial/tutorial_general_100.png

[200]: ./media/egnyte-tutorial/tutorial_general_200.png
[201]: ./media/egnyte-tutorial/tutorial_general_201.png
[202]: ./media/egnyte-tutorial/tutorial_general_202.png
[203]: ./media/egnyte-tutorial/tutorial_general_203.png

