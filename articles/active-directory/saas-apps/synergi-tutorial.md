---
title: 'Tutorial: Integração do Active Directory do Azure com Synergi | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Synergi.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 73c970e1-f1ba-420b-b225-414fdf93b140
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 35e5142d691b1da39d66be43144cc1b048608524
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2019
ms.locfileid: "56167053"
---
# <a name="tutorial-azure-active-directory-integration-with-synergi"></a>Tutorial: Integração do Active Directory do Azure com Synergi

Neste tutorial, saiba como integrar Synergi com o Azure Active Directory (Azure AD).

Integrar Synergi no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Synergi.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Synergi (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Synergi, terá dos seguintes itens:

- Uma subscrição do Azure
- Um Synergi logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Synergi da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-synergi-from-the-gallery"></a>Adicionando Synergi da Galeria
Para configurar a integração do Synergi com o Azure AD, terá de adicionar Synergi a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Synergi a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

1. Na caixa de pesquisa, escreva **Synergi**, selecione **Synergi** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Synergi na lista de resultados](./media/synergi-tutorial/tutorial_synergi_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com Synergi com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Synergi a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Synergi deve ser estabelecido.

Synergi, atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com Synergi, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste Synergi](#create-a-synergi-test-user)**  - para ter um equivalente da Eduarda Almeida na Synergi que está ligado à representação de utilizador do Azure AD.
1. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo Synergi.

**Para configurar o Azure AD início de sessão único com Synergi, execute os seguintes passos:**

1. No portal do Azure, sobre o **Synergi** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/synergi-tutorial/tutorial_synergi_samlbase.png)

1. Sobre o **Synergi domínio e URLs** secção, execute os seguintes passos:

    ![Synergi domínio e URLs únicas início de sessão em informações](./media/synergi-tutorial/tutorial_synergi_url.png)

    a. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://<company name>.irmsecurity.com`

    b. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://<company name>.irmsecurity.com/sso/<organization id>`

    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o identificador real e o URL de resposta. Contacte [equipa de suporte de Synergi](https://www.irmsecurity.com/contact/) obter esses valores.

1. Sobre o **certificado de assinatura SAML** secção, clique em **Certificate(Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![O link de download de certificado](./media/synergi-tutorial/tutorial_synergi_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/synergi-tutorial/tutorial_general_400.png)

1. Sobre o **Synergi configuração** secção, clique em **configurar Synergi** para abrir **configurar início de sessão** janela. Copiar o **URL de fim de sessão e o ID de entidade de SAML** partir o **secção de referência rápida.**

    ![Configuração de Synergi](./media/synergi-tutorial/tutorial_synergi_configure.png) 

1. Para configurar o início de sessão único num **Synergi** lado, terá de enviar o transferido **Certificate(Base64), o URL de fim de sessão e o ID de entidade de SAML** para [equipa de suporte de Synergi](https://www.irmsecurity.com/contact/).

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [Documentação do Azure AD incorporado]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/synergi-tutorial/create_aaduser_01.png)

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/synergi-tutorial/create_aaduser_02.png)

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/synergi-tutorial/create_aaduser_03.png)

1. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/synergi-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
  
### <a name="create-a-synergi-test-user"></a>Criar um utilizador de teste Synergi

Nesta secção, vai criar um usuário chamado Eduarda Almeida no Synergi. Trabalhar com [equipa de suporte de Synergi](https://www.irmsecurity.com/contact/) para adicionar os utilizadores na plataforma Synergi. Os utilizadores tem de ser criados e ativados antes de utilizar o início de sessão único.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para Synergi.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a Synergi, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **Synergi**.

    ![A ligação de Synergi na lista de aplicações](./media/synergi-tutorial/tutorial_synergi_app.png)  

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar o início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico Synergi no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo Synergi.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[1]: ./media/synergi-tutorial/tutorial_general_01.png
[2]: ./media/synergi-tutorial/tutorial_general_02.png
[3]: ./media/synergi-tutorial/tutorial_general_03.png
[4]: ./media/synergi-tutorial/tutorial_general_04.png

[100]: ./media/synergi-tutorial/tutorial_general_100.png

[200]: ./media/synergi-tutorial/tutorial_general_200.png
[201]: ./media/synergi-tutorial/tutorial_general_201.png
[202]: ./media/synergi-tutorial/tutorial_general_202.png
[203]: ./media/synergi-tutorial/tutorial_general_203.png

