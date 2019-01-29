---
title: 'Tutorial: Integração do Active Directory do Azure com o planejamento de gama Predictix | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e o planejamento de gama Predictix.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 37e686ff-f8e5-40b1-9d7e-f64b076917b7
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: 9bc1fc5c6532d9c3070e13777a23d57a838468e7
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55173467"
---
# <a name="tutorial-azure-active-directory-integration-with-predictix-assortment-planning"></a>Tutorial: Integração do Active Directory do Azure com o planejamento de gama Predictix

Neste tutorial, saiba como integrar o planejamento de gama Predictix com o Azure Active Directory (Azure AD).

Integrar o planejamento de gama Predictix com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao planeamento da variedade de Predictix.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para planejamento de gama Predictix (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o planejamento de gama Predictix, terá dos seguintes itens:

- Uma subscrição do Azure
- Uma variedade de Predictix planejamento logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando o planejamento de gama Predictix da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-predictix-assortment-planning-from-the-gallery"></a>Adicionando o planejamento de gama Predictix da Galeria
Para configurar a integração do planeamento de gama Predictix com o Azure AD, terá de adicionar Predictix variedade de planeamento da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar o planejamento de gama Predictix partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

1. Na caixa de pesquisa, escreva **planejamento de gama Predictix**, selecione **planejamento de gama Predictix** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Planear a variedade Predictix na lista de resultados](./media/predictix-assortment-planning-tutorial/tutorial_predictixassortmentplanning_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com Predictix variedade planeamento com base num utilizador de teste chamado "Eduarda Almeida."

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador equivalente no planeamento da variedade de Predictix a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no planeamento da variedade de Predictix deve ser estabelecido.

No planejamento da variedade Predictix, atribuir o valor do **nome de utilizador** no Azure AD como o valor da **nome de utilizador** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com o planejamento de gama Predictix, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste de planejamento de gama Predictix](#create-a-predictix-assortment-planning-test-user)**  - para ter um equivalente da Eduarda Almeida no Predictix variedade planejamento que está ligado à representação de utilizador do Azure AD.
1. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo de planejamento de gama Predictix.

**Para configurar do Azure AD início de sessão único com o planejamento de gama Predictix, execute os seguintes passos:**

1. No portal do Azure, sobre o **Predictix variedade de planeamento** página de integração de aplicativo, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/predictix-assortment-planning-tutorial/tutorial_predictixassortmentplanning_samlbase.png)

1. Sobre o **Predictix variedade planejamento de domínio e URLs** secção, execute os seguintes passos:

    ![Domínio de planejamento do Predictix variedade e URLs únicas início de sessão em informações](./media/predictix-assortment-planning-tutorial/tutorial_predictixassortmentplanning_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão:

    | |
    |--|--|
    | `https://<sub-domain>.ap.predictix.com/sso/request`|
    | `https://<sub-domain>.dev.ap.predictix.com/`|

    b. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão:
    
    | |
    |--|--|
    | `https://<sub-domain>.ap.predictix.com`|
    | `https://<sub-domain>.dev.ap.predictix.com`|
    
    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o URL de início de sessão e o identificador real. Contacte [equipa de suporte de cliente de planejamento de gama de Predictix](https://www.infor.com/support) obter esses valores. 
 


1. Sobre o **certificado de assinatura SAML** secção, clique em **Certificate(Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![O link de download de certificado](./media/predictix-assortment-planning-tutorial/tutorial_predictixassortmentplanning_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/predictix-assortment-planning-tutorial/tutorial_general_400.png)

1. No **Predictix variedade planejamento Configuration** secção, clique em **configurar o planejamento de gama Predictix** para abrir **configurar início de sessão** janela. Cópia a **URL de fim de sessão, o ID de entidade de SAML e o SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configuração de planejamento de gama de Predictix](./media/predictix-assortment-planning-tutorial/tutorial_predictixassortmentplanning_configure.png) 

1. Para configurar o início de sessão único num **planejamento de gama Predictix** lado, terá de enviar o transferido **Certificate(Base64)**, **ID de entidade de SAML**, **SAML Single Sign-On URL do serviço**, e **URL de fim de sessão** para [equipa de suporte de planeamento de gama Predictix](https://www.infor.com/support). Se definir esta definição para que a ligação de SAML SSO definidas corretamente em ambos os lados.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [Documentação do Azure AD incorporado]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/predictix-assortment-planning-tutorial/create_aaduser_01.png)

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/predictix-assortment-planning-tutorial/create_aaduser_02.png)

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/predictix-assortment-planning-tutorial/create_aaduser_03.png)

1. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/predictix-assortment-planning-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-predictix-assortment-planning-test-user"></a>Criar um utilizador de teste Predictix variedade de planeamento

Nesta secção, vai criar um usuário chamado Eduarda Almeida no planejamento da variedade de Predictix. Trabalhe em conjunto com [equipa de suporte de planeamento de gama Predictix](https://www.infor.com/contact/) para adicionar os utilizadores na plataforma do planeamento de gama Predictix.
 > [!NOTE]
 > O titular da conta do Azure Active Directory recebe uma mensagem de e-mail e segue uma ligação para confirmar a respetiva conta até se tornar Active Directory.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para planejamento de gama Predictix.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a variedade de Predictix planejamento, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **planejamento de gama Predictix**.

    ![A ligação de planejamento de gama Predictix na lista de aplicações](./media/predictix-assortment-planning-tutorial/tutorial_predictixassortmentplanning_app.png)  

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar o início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico de planejamento do Predictix variedade de mensagens em fila no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação de planeamento de gama Predictix.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/predictix-assortment-planning-tutorial/tutorial_general_01.png
[2]: ./media/predictix-assortment-planning-tutorial/tutorial_general_02.png
[3]: ./media/predictix-assortment-planning-tutorial/tutorial_general_03.png
[4]: ./media/predictix-assortment-planning-tutorial/tutorial_general_04.png

[100]: ./media/predictix-assortment-planning-tutorial/tutorial_general_100.png

[200]: ./media/predictix-assortment-planning-tutorial/tutorial_general_200.png
[201]: ./media/predictix-assortment-planning-tutorial/tutorial_general_201.png
[202]: ./media/predictix-assortment-planning-tutorial/tutorial_general_202.png
[203]: ./media/predictix-assortment-planning-tutorial/tutorial_general_203.png

