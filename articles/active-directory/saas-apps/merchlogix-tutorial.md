---
title: 'Tutorial: Integração do Active Directory do Azure com Merchlogix | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Merchlogix.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: a1f49bb8-6b17-433d-8f25-9d26fb390e77
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: d4c2ea78643b0efbea30746d60d4c988e2108b5f
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2019
ms.locfileid: "56204921"
---
# <a name="tutorial-azure-active-directory-integration-with-merchlogix"></a>Tutorial: Integração do Active Directory do Azure com Merchlogix

Neste tutorial, saiba como integrar Merchlogix com o Azure Active Directory (Azure AD).

Integrar Merchlogix no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Merchlogix.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Merchlogix (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Merchlogix, terá dos seguintes itens:

- Uma subscrição do Azure
- Um Merchlogix logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Merchlogix da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-merchlogix-from-the-gallery"></a>Adicionando Merchlogix da Galeria
Para configurar a integração do Merchlogix com o Azure AD, terá de adicionar Merchlogix a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Merchlogix a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

1. Na caixa de pesquisa, escreva **Merchlogix**, selecione **Merchlogix** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Merchlogix na lista de resultados](./media/merchlogix-tutorial/tutorial_merchlogix_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com Merchlogix com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Merchlogix a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Merchlogix deve ser estabelecido.

Merchlogix, atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com Merchlogix, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste Merchlogix](#create-a-merchlogix-test-user)**  - para ter um equivalente da Eduarda Almeida na Merchlogix que está ligado à representação de utilizador do Azure AD.
1. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo Merchlogix.

**Para configurar o Azure AD início de sessão único com Merchlogix, execute os seguintes passos:**

1. No portal do Azure, sobre o **Merchlogix** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/merchlogix-tutorial/tutorial_merchlogix_samlbase.png)

1. Sobre o **Merchlogix domínio e URLs** secção, execute os seguintes passos:

    ![Merchlogix domínio e URLs únicas início de sessão em informações](./media/merchlogix-tutorial/tutorial_merchlogix_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<DOMAIN>/login.php?saml=true`

    b. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://<DOMAIN>/simplesaml/module.php/saml/sp/metadata.php/<SAML_NAME>`
    
    > [!NOTE]
    > Estes valores não são reais. Atualize estes valores com o URL de início de sessão e o identificador real. Contacte [equipa de suporte de Merchlogix](https://www.merchlogix.com/contact/) obter esses valores.

1. Sobre o **certificado de assinatura SAML** secção, clique em **certificado (Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![O link de download de certificado](./media/merchlogix-tutorial/tutorial_merchlogix_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/merchlogix-tutorial/tutorial_general_400.png)

1. Sobre o **Merchlogix configuração** secção, clique em **configurar Merchlogix** para abrir **configurar início de sessão** janela. Cópia a **URL de fim de sessão, o ID de entidade de SAML** e **SAML único início de sessão no URL do serviço** do **secção de referência rápida.**

    ![Configuração de Merchlogix](./media/merchlogix-tutorial/tutorial_merchlogix_configure.png) 

1. Para configurar o início de sessão único num **Merchlogix** lado, terá de enviar o transferido **certificado (Base64)**, **URL de fim de sessão, o ID de entidade de SAML e o SAML único início de sessão no URL do serviço**para [a equipa de suporte Merchlogix](https://www.merchlogix.com/contact/). Se definir esta definição para que a ligação de SAML SSO definidas corretamente em ambos os lados.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [Documentação do Azure AD incorporado]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/merchlogix-tutorial/create_aaduser_01.png)

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/merchlogix-tutorial/create_aaduser_02.png)

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/merchlogix-tutorial/create_aaduser_03.png)

1. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/merchlogix-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-merchlogix-test-user"></a>Criar um utilizador de teste Merchlogix

Nesta secção, vai criar um usuário chamado Eduarda Almeida no Merchlogix. Trabalhar com [equipa de suporte de Merchlogix](https://www.merchlogix.com/contact/) para adicionar os utilizadores na plataforma Merchlogix.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para Merchlogix.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a Merchlogix, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **Merchlogix**.

    ![A ligação de Merchlogix na lista de aplicações](./media/merchlogix-tutorial/tutorial_merchlogix_app.png)  

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar o início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico Merchlogix no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo Merchlogix.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/merchlogix-tutorial/tutorial_general_01.png
[2]: ./media/merchlogix-tutorial/tutorial_general_02.png
[3]: ./media/merchlogix-tutorial/tutorial_general_03.png
[4]: ./media/merchlogix-tutorial/tutorial_general_04.png

[100]: ./media/merchlogix-tutorial/tutorial_general_100.png

[200]: ./media/merchlogix-tutorial/tutorial_general_200.png
[201]: ./media/merchlogix-tutorial/tutorial_general_201.png
[202]: ./media/merchlogix-tutorial/tutorial_general_202.png
[203]: ./media/merchlogix-tutorial/tutorial_general_203.png

