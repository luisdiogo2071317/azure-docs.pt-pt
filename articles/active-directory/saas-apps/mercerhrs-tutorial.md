---
title: 'Tutorial: Integração do Active Directory do Azure com Mercer BenefitsCentral (MBC) | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Mercer BenefitsCentral (MBC).
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 3788b28c-49aa-4208-9acd-630362008e89
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/21/2017
ms.author: jeedes
ms.openlocfilehash: ec37ea10e1318fb707a656587c5f91637ed1cf55
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55173835"
---
# <a name="tutorial-azure-active-directory-integration-with-mercer-benefitscentral-mbc"></a>Tutorial: Integração do Active Directory do Azure com Mercer BenefitsCentral (MBC)

Neste tutorial, saiba como integrar Mercer BenefitsCentral (MBC) com o Azure Active Directory (Azure AD).

Integrar Mercer BenefitsCentral (MBC) no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso Mercer BenefitsCentral (MBC).
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Mercer BenefitsCentral (MBC) (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Mercer BenefitsCentral (MBC), terá dos seguintes itens:

- Uma subscrição do Azure
- Um Mercer BenefitsCentral (MBC) início de sessão único de subscrição ativada

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Mercer BenefitsCentral (MBC) da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-mercer-benefitscentral-mbc-from-the-gallery"></a>Adicionando Mercer BenefitsCentral (MBC) da Galeria
Para configurar a integração de Mercer BenefitsCentral (MBC) para o Azure AD, terá de adicionar Mercer BenefitsCentral (MBC) a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Mercer BenefitsCentral (MBC) a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

1. Na caixa de pesquisa, escreva **Mercer BenefitsCentral (MBC)**, selecione **Mercer BenefitsCentral (MBC)** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Mercer BenefitsCentral (MBC) na lista de resultados](./media/mercerhrs-tutorial/tutorial_mercerhrs_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com Mercer BenefitsCentral (MBC) com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Mercer BenefitsCentral (MBC) a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Mercer BenefitsCentral (MBC) deve ser estabelecido.

No BenefitsCentral Mercer (MBC), atribuir o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com Mercer BenefitsCentral (MBC), tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste Mercer BenefitsCentral (MBC)](#create-a-mercer-benefitscentral-mbc-test-user)**  - para ter um equivalente da Eduarda Almeida na Mercer BenefitsCentral (MBC) que está ligado à representação de utilizador do Azure AD.
1. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo Mercer BenefitsCentral (MBC).

**Para configurar o Azure AD início de sessão único com Mercer BenefitsCentral (MBC), execute os seguintes passos:**

1. No portal do Azure, sobre o **Mercer BenefitsCentral (MBC)** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/mercerhrs-tutorial/tutorial_mercerhrs_samlbase.png)

1. Sobre o **Mercer BenefitsCentral (MBC) de domínio e URLs** secção, execute os seguintes passos:

    ![Mercer BenefitsCentral (MBC) domínio e URLs únicas início de sessão em informações](./media/mercerhrs-tutorial/tutorial_mercerhrs_url.png)

    a. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `stg.mercerhrs.com/saml2.0`

    b. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://ssous-stg.mercerhrs.com/SP2/Saml2AssertionConsumer.aspx`

    > [!NOTE] 
    > Valor de URL de resposta não é real. Atualize este valor com o URL de resposta real. Contacte [equipa de suporte de Mercer BenefitsCentral (MBC)](https://www.mercer.com/contact-us.html) para obter este valor.

1. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![O link de download de certificado](./media/mercerhrs-tutorial/tutorial_mercerhrs_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/mercerhrs-tutorial/tutorial_general_400.png)

1. Sobre o **Mercer BenefitsCentral (MBC) configuração** secção, clique em **configurar Mercer BenefitsCentral (MBC)** para abrir **configurar início de sessão** janela. Cópia a **SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configuração do mercer BenefitsCentral (MBC)](./media/mercerhrs-tutorial/tutorial_mercerhrs_configure.png) 

1. Para configurar o início de sessão único num **Mercer BenefitsCentral (MBC)** lado, terá de enviar o transferido **XML de metadados** e **SAML único início de sessão no URL do serviço** para [Equipa de suporte de mercer BenefitsCentral (MBC)](https://www.mercer.com/contact-us.html). Se definir esta definição para que a ligação de SAML SSO definidas corretamente em ambos os lados.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [Documentação do Azure AD incorporado]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/mercerhrs-tutorial/create_aaduser_01.png)

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/mercerhrs-tutorial/create_aaduser_02.png)

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/mercerhrs-tutorial/create_aaduser_03.png)

1. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/mercerhrs-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
  
### <a name="create-a-mercer-benefitscentral-mbc-test-user"></a>Criar um utilizador de teste Mercer BenefitsCentral (MBC)

Nesta secção, vai criar um usuário chamado Eduarda Almeida na Mercer HRS. Trabalhar com [equipa de suporte de Mercer BenefitsCentral (MBC)](https://www.mercer.com/contact-us.html) para adicionar os utilizadores na plataforma Mercer horas. Os utilizadores tem de ser criados e ativados antes de utilizar o início de sessão único.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único, concedendo acesso Mercer BenefitsCentral (MBC).

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida Mercer BenefitsCentral (MBC), execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **Mercer BenefitsCentral (MBC)**.

    ![A ligação de Mercer BenefitsCentral (MBC) na lista de aplicações](./media/mercerhrs-tutorial/tutorial_mercerhrs_app.png)  

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar o início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico Mercer BenefitsCentral (MBC) no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo Mercer BenefitsCentral (MBC).
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[1]: ./media/mercerhrs-tutorial/tutorial_general_01.png
[2]: ./media/mercerhrs-tutorial/tutorial_general_02.png
[3]: ./media/mercerhrs-tutorial/tutorial_general_03.png
[4]: ./media/mercerhrs-tutorial/tutorial_general_04.png

[100]: ./media/mercerhrs-tutorial/tutorial_general_100.png

[200]: ./media/mercerhrs-tutorial/tutorial_general_200.png
[201]: ./media/mercerhrs-tutorial/tutorial_general_201.png
[202]: ./media/mercerhrs-tutorial/tutorial_general_202.png
[203]: ./media/mercerhrs-tutorial/tutorial_general_203.png

