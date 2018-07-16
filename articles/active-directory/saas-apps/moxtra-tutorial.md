---
title: 'Tutorial: Integração do Azure Active Directory com Moxtra | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Moxtra.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 2aed2d4b-1dcd-4839-8fed-9419d107c61c
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: jeedes
ms.openlocfilehash: 3c0c91e293f04665bc284daa4f496b9bff74aa3c
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39051943"
---
# <a name="tutorial-azure-active-directory-integration-with-moxtra"></a>Tutorial: Integração do Azure Active Directory com Moxtra

Neste tutorial, saiba como integrar Moxtra com o Azure Active Directory (Azure AD).

Integrar Moxtra no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Moxtra
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Moxtra (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Moxtra, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Moxtra logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Moxtra da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-moxtra-from-the-gallery"></a>Adicionando Moxtra da Galeria
Para configurar a integração do Moxtra com o Azure AD, terá de adicionar Moxtra a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Moxtra a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

4. Na caixa de pesquisa, escreva **Moxtra**.

    ![Criar um utilizador de teste do Azure AD](./media/moxtra-tutorial/tutorial_moxtra_search.png)

5. No painel de resultados, selecione **Moxtra**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/moxtra-tutorial/tutorial_moxtra_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com Moxtra com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Moxtra a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Moxtra deve ser estabelecido.

Moxtra, atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com Moxtra, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste Moxtra](#creating-a-moxtra-test-user)**  - para ter um equivalente da Eduarda Almeida na Moxtra que está ligado à representação de utilizador do Azure AD.
4. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo Moxtra.

**Para configurar o Azure AD início de sessão único com Moxtra, execute os seguintes passos:**

1. No portal do Azure, sobre o **Moxtra** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/moxtra-tutorial/tutorial_moxtra_samlbase.png)

3. Sobre o **Moxtra domínio e URLs** secção, executar o passo seguinte:

    ![Configurar o início de sessão único](./media/moxtra-tutorial/tutorial_moxtra_url.png)

    Na **URL de início de sessão** caixa de texto, escreva um URL como: `https://www.moxtra.com/service/#login`

4. Aplicação de Moxtra espera que as asserções SAML num formato específico. Configure as seguintes declarações para esta aplicação. Pode gerir os valores destes atributos da "**atributos de utilizador**" secção na página de integração de aplicações. Captura de ecrã seguinte mostra um exemplo para esta configuração. 

    ![Configurar o início de sessão único](./media/moxtra-tutorial/tutorial_moxtra_attributes.png)
    
5. No **atributos de utilizador** secção sobre o **início de sessão único** caixa de diálogo, configurar o atributo de token de SAML conforme mostrado na imagem e execute os seguintes passos:
    
    | Nome do Atributo | Valor do Atributo |
    | ------------------- | -------------------- |    
    | FirstName | User.givenName |
    | Apelido | User.Surname |
    | idpid    | < ID da entidade SAML > 

    > [!Note]
    > O valor de **idpid** atributo não é real. Pode obter o valor real do **referência rápida** secção sob **Moxtra configuração**.
    
    a. Clique em **adicionar atributo** para abrir o **adicionar atributo** caixa de diálogo.

    ![Configurar o início de sessão único](./media/moxtra-tutorial/tutorial_attribute_04.png)

    b. Na **nome** caixa de texto, escreva o nome de atributo apresentado para essa linha.

    ![Configurar o início de sessão único](./media/moxtra-tutorial/tutorial_attribute_05.png)

    c. Partir do **valor** lista, digite o valor de atributo apresentado para essa linha.

    d. Clique em **OK**.
    
5. Sobre o **certificado de assinatura SAML** secção, clique em **Certificate(Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![Configurar o início de sessão único](./media/moxtra-tutorial/tutorial_moxtra_certificate.png) 

6. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/moxtra-tutorial/tutorial_general_400.png)

7. Sobre o **Moxtra configuração** secção, clique em **configurar Moxtra** para abrir **configurar início de sessão** janela. Cópia a **ID de entidade de SAML e SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configurar o início de sessão único](./media/moxtra-tutorial/tutorial_moxtra_configure.png) 

8. Na outra janela do browser, inicie sessão site da sua empresa Moxtra como um administrador.

9. Na barra de ferramentas do lado esquerdo, clique em **consola de administração > o início de sessão único SAML**e, em seguida, clique em **New**.
   
    ![Configurar o início de sessão único](./media/moxtra-tutorial/tutorial_moxtra_06.png) 

10. Sobre o **SAML** página, execute os seguintes passos:
   
    ![Configurar o início de sessão único](./media/moxtra-tutorial/tutorial_moxtra_08.png)   
 
    a. Na **Name** caixa de texto, escreva um nome para a sua configuração (p. ex.: *SAML*). 
  
    b. Na **ID de entidade do IdP** caixa de texto, cole o valor de **ID de entidade de SAML** que copiou do portal do Azure. 
 
    c. Na **URL de início de sessão** caixa de texto, cole o valor de **SAML único início de sessão no URL do serviço** que copiou do portal do Azure. 
 
    d. Na **AuthnContextClassRef** caixa de texto, tipo **urn: oasis: nomes: tc: SAML:2.0:ac:classes:Password**. 
 
    e. Na **formato NameID** caixa de texto, tipo **urn: oasis: nomes: tc: SAML:1.1:nameid-formato: endereço de correio eletrónico**. 
 
    f. Abrir certificado que transferiu a partir do portal do Azure no bloco de notas, copie o conteúdo e, em seguida, cole-o no **certificado** caixa de texto.    
 
    g. Na textbox de domínio de e-mail SAML, escreva o seu domínio de e-mail SAML.    
  
    >[!NOTE]
    >Para ver os passos para verificar o domínio, clique a "**eu**" abaixo.

    h. Clique em **atualização**.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/moxtra-tutorial/create_aaduser_01.png) 

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/moxtra-tutorial/create_aaduser_02.png) 

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/moxtra-tutorial/create_aaduser_03.png) 

4. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/moxtra-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-a-moxtra-test-user"></a>Criar um utilizador de teste Moxtra

O objetivo desta secção é criar um usuário chamado Eduarda Almeida no Moxtra.

**Para criar um usuário chamado Eduarda Almeida no Moxtra, execute os seguintes passos:**

1. Inicie sessão site da sua empresa Moxtra como um administrador.

2. Na barra de ferramentas do lado esquerdo, clique em **consola de administrador > Gestão de utilizadores**e, em seguida **adicionar utilizador**.
   
    ![Configurar o início de sessão único](./media/moxtra-tutorial/tutorial_moxtra_10.png) 

3. Sobre o **adicionar utilizador** caixa de diálogo, execute os seguintes passos:
  
    a. Na **nome próprio** caixa de texto, tipo **Eduarda**.
  
    b. Na **sobrenome** caixa de texto, tipo **Simon**.
  
    c. Na **E-Mail** caixa de texto, tipo da Eduarda endereço de e-mail mesmo que no portal do Azure.
  
    d. Na **divisão** caixa de texto, tipo **Dev**.
  
    e. Na **departamento** caixa de texto, tipo **IT**.
  
    f. Selecione **administrador**.
  
    g. Clique em **Adicionar**.

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para Moxtra.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida a Moxtra, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **Moxtra**.

    ![Configurar o início de sessão único](./media/moxtra-tutorial/tutorial_moxtra_app.png) 

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico Moxtra no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo Moxtra.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/moxtra-tutorial/tutorial_general_01.png
[2]: ./media/moxtra-tutorial/tutorial_general_02.png
[3]: ./media/moxtra-tutorial/tutorial_general_03.png
[4]: ./media/moxtra-tutorial/tutorial_general_04.png

[100]: ./media/moxtra-tutorial/tutorial_general_100.png

[200]: ./media/moxtra-tutorial/tutorial_general_200.png
[201]: ./media/moxtra-tutorial/tutorial_general_201.png
[202]: ./media/moxtra-tutorial/tutorial_general_202.png
[203]: ./media/moxtra-tutorial/tutorial_general_203.png

