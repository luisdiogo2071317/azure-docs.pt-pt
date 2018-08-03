---
title: 'Tutorial: Integração do Azure Active Directory com a loja Digital de EFI | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Vitrine Digital de EFI.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 33c148fc-d490-4bb9-90c1-d5933679ce4e
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/08/2017
ms.author: jeedes
ms.openlocfilehash: 6959521b0f700a0afafef0950e9cb336488cc94b
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39425311"
---
# <a name="tutorial-azure-active-directory-integration-with-efi-digital-storefront"></a>Tutorial: Integração do Azure Active Directory com a loja Digital do EFI

Neste tutorial, saiba como integrar Vitrine de EFI Digital no Azure Active Directory (Azure AD).

Integrar Vitrine de EFI Digital no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso à loja Digital de EFI.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para a loja Digital do EFI (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com loja Digital de EFI, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Uma loja Digital de EFI logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando loja Digital de EFI da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-efi-digital-storefront-from-the-gallery"></a>Adicionando loja Digital de EFI da Galeria
Para configurar a integração da loja Digital de EFI para o Azure AD, terá de adicionar loja Digital de EFI a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar loja Digital de EFI a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

1. Na caixa de pesquisa, escreva **loja Digital de EFI**, selecione **loja Digital de EFI** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Vitrine Digital do EFI na lista de resultados](./media/efidigitalstorefront-tutorial/tutorial_efidigital_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com EFI loja Digital, com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de equivalente na loja Digital de EFI para um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado na loja Digital de EFI deve ser estabelecido.

Na loja Digital de EFI, atribua o valor do **nome de utilizador** no Azure AD como o valor da **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com loja Digital de EFI, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste de loja Digital de EFI](#create-a-efi-digital-storefront-test-user)**  - para ter um equivalente da Eduarda Almeida na loja Digital EFI e que está ligado à representação de utilizador do Azure AD.
1. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único na sua aplicação de loja Digital de EFI.

**Para configurar o Azure AD início de sessão único com loja Digital de EFI, execute os seguintes passos:**

1. No portal do Azure, sobre o **loja Digital de EFI** página de integração de aplicativo, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/efidigitalstorefront-tutorial/tutorial_efidigital_samlbase.png)

1. Sobre o **EFI Digital Vitrine domínio e URLs** secção, execute os seguintes passos:

    ![Domínio de loja Digital EFI e URLs únicas início de sessão em informações](./media/efidigitalstorefront-tutorial/tutorial_efidigital_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<companyname>.myprintdesk.net/DSF`

    b. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://<companyname>.myprintdesk.net/DSF/asp4/`

1. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![O link de download de certificado](./media/efidigitalstorefront-tutorial/tutorial_efidigital_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/efidigitalstorefront-tutorial/tutorial_general_400.png)

1. Para configurar o início de sessão único num **loja Digital de EFI** lado, terá de enviar o transferido **XML de metadados** para [equipa de suporte de loja Digital de EFI](http://www.efi.com/products/productivity-software/ecommerce-web-to-print/efi-digital-storefront/support/). Se definir esta definição para que a ligação de SAML SSO definidas corretamente em ambos os lados.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/efidigitalstorefront-tutorial/create_aaduser_01.png)

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/efidigitalstorefront-tutorial/create_aaduser_02.png)

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/efidigitalstorefront-tutorial/create_aaduser_03.png)

1. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/efidigitalstorefront-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-efi-digital-storefront-test-user"></a>Criar um utilizador de teste de loja Digital do EFI

Nesta secção, vai criar um usuário chamado Eduarda Almeida na loja Digital de EFI. Trabalhar com [equipa de suporte de loja Digital de EFI](http://www.efi.com/products/productivity-software/ecommerce-web-to-print/efi-digital-storefront/support/) para adicionar os utilizadores na plataforma da loja Digital de EFI. Os utilizadores tem de ser criados e ativados antes de utilizar o início de sessão único. 

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida a utilizar do Azure-início de sessão único, conceder acesso à loja Digital de EFI.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a loja Digital de EFI, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **loja Digital de EFI**.

    ![A ligação de loja Digital de EFI na lista de aplicações](./media/efidigitalstorefront-tutorial/tutorial_efidigital_app.png)  

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico de loja Digital de EFI, no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação de loja Digital de EFI.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/efidigitalstorefront-tutorial/tutorial_general_01.png
[2]: ./media/efidigitalstorefront-tutorial/tutorial_general_02.png
[3]: ./media/efidigitalstorefront-tutorial/tutorial_general_03.png
[4]: ./media/efidigitalstorefront-tutorial/tutorial_general_04.png

[100]: ./media/efidigitalstorefront-tutorial/tutorial_general_100.png

[200]: ./media/efidigitalstorefront-tutorial/tutorial_general_200.png
[201]: ./media/efidigitalstorefront-tutorial/tutorial_general_201.png
[202]: ./media/efidigitalstorefront-tutorial/tutorial_general_202.png
[203]: ./media/efidigitalstorefront-tutorial/tutorial_general_203.png

