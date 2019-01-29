---
title: 'Tutorial: Integração do Active Directory do Azure com Bime | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Bime.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: bdcf0729-c880-4c95-b739-0f6345b17dd8
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: jeedes
ms.openlocfilehash: 5d1aa3ebf089cd7024792be35f31d9febc19b0c5
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55189056"
---
# <a name="tutorial-azure-active-directory-integration-with-bime"></a>Tutorial: Integração do Active Directory do Azure com Bime

Neste tutorial, saiba como integrar Bime com o Azure Active Directory (Azure AD).

Integrar Bime no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Bime
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Bime (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Bime, terá dos seguintes itens:

- Uma subscrição do Azure
- Um Bime logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma avaliação de um mês aqui: [Oferta de avaliação](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Bime da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-bime-from-the-gallery"></a>Adicionando Bime da Galeria
Para configurar a integração do Bime com o Azure AD, terá de adicionar Bime a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Bime a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

1. Na caixa de pesquisa, escreva **Bime**.

    ![Criar um utilizador de teste do Azure AD](./media/bime-tutorial/tutorial_bime_search.png)

1. No painel de resultados, selecione **Bime**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/bime-tutorial/tutorial_bime_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com Bime com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Bime a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Bime deve ser estabelecido.

Bime, atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com Bime, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste Bime](#creating-a-bime-test-user)**  - para ter um equivalente da Eduarda Almeida na Bime que está ligado à representação de utilizador do Azure AD.
1. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo Bime.

**Para configurar o Azure AD início de sessão único com Bime, execute os seguintes passos:**

1. No portal do Azure, sobre o **Bime** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/bime-tutorial/tutorial_bime_samlbase.png)

1. Sobre o **Bime domínio e URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/bime-tutorial/tutorial_bime_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<tenant-name>.Bimeapp.com`

    b. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://<tenant-name>.Bimeapp.com`

    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o URL de início de sessão e o identificador real. Contacte [equipa de suporte de cliente Bime](https://bime.zendesk.com/hc/categories/202604307-Support-tech-notes-and-tips-) obter esses valores. 
 
1. Na **certificado de assinatura SAML** secção, copie a **THUMBPRINT** valor a partir do certificado.

    ![Configurar o início de sessão único](./media/bime-tutorial/tutorial_bime_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/bime-tutorial/tutorial_general_400.png)

1. Sobre o **Bime configuração** secção, clique em **configurar Bime** para abrir **configurar início de sessão** janela. Cópia a **SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configurar o início de sessão único](./media/bime-tutorial/tutorial_bime_configure.png) 

1. Numa janela do browser web diferente, inicie sessão no site da sua empresa Bime como um administrador.

1. Na barra de ferramentas, clique em **administrador**e, em seguida **conta**.
   
    ![Admin](./media/bime-tutorial/ic775558.png "Admin")

1. Na página de configuração da conta, execute os seguintes passos:
   
    ![Configurar o início de sessão único](./media/bime-tutorial/ic775559.png "configurar início de sessão único")
   
    a. Selecione **autenticação SAML ativar**.

    b. Na **URL de início de sessão remoto** caixa de texto, cole o valor de **SAML único início de sessão no URL do serviço**, que copiou do portal do Azure.

    c.  Colar o **Thumbprint** valor a partir do portal do Azure para o **impressão digital do certificado** caixa de texto.       
   
    d. Clique em **Guardar**.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [Documentação do Azure AD incorporado]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/bime-tutorial/create_aaduser_01.png) 

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/bime-tutorial/create_aaduser_02.png) 

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/bime-tutorial/create_aaduser_03.png) 

1. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/bime-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-a-bime-test-user"></a>Criar um utilizador de teste Bime

Para habilitar os utilizadores do Azure AD iniciar sessão no Bime, tem de ser aprovisionados em Bime. No caso de Bime, o aprovisionamento é uma tarefa manual.

**Para configurar o aprovisionamento de utilizadores, execute os seguintes passos:**

1. Inicie sessão no seu **Bime** inquilino.

1. Na barra de ferramentas, clique em **administrador**e, em seguida **utilizadores**.
   
    ![Admin](./media/bime-tutorial/ic775561.png "Admin")

1. Na **lista de utilizadores**, clique em **adicionar novo utilizador** ("+").
   
    ![Os utilizadores](./media/bime-tutorial/ic775562.png "utilizadores")

1. Sobre o **detalhes de utilizador** caixa de diálogo página, execute os seguintes passos:
   
    ![Detalhes de utilizador](./media/bime-tutorial/ic775563.png "detalhes de utilizador")
   
    a. Na **nome próprio** caixa de texto, introduza o nome de utilizador, como **Eduarda**.

    b. Na **Apelido** caixa de texto, introduza o apelido do utilizador, como **Simon**.
 
    c. Na **E-Mail** caixa de texto, introduza o e-mail do utilizador, como **brittasimon@contoso.com**.

    d. Clique em **Guardar**.

>[!NOTE]
>Pode utilizar quaisquer outras Bime utilizador conta criação ferramentas ou APIs fornecidas pelo Bime para aprovisionar contas de utilizador do AAD.
>  

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para Bime.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida a Bime, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **Bime**.

    ![Configurar o início de sessão único](./media/bime-tutorial/tutorial_bime_app.png) 

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

O objetivo desta secção é testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico Bime no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo Bime.

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/bime-tutorial/tutorial_general_01.png
[2]: ./media/bime-tutorial/tutorial_general_02.png
[3]: ./media/bime-tutorial/tutorial_general_03.png
[4]: ./media/bime-tutorial/tutorial_general_04.png

[100]: ./media/bime-tutorial/tutorial_general_100.png

[200]: ./media/bime-tutorial/tutorial_general_200.png
[201]: ./media/bime-tutorial/tutorial_general_201.png
[202]: ./media/bime-tutorial/tutorial_general_202.png
[203]: ./media/bime-tutorial/tutorial_general_203.png

