---
title: 'Tutorial: Integração do Active Directory do Azure com Edcor | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Edcor.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 5b06f2d8-9cd7-498d-bdd6-88570a0a0a15
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/13/2017
ms.author: jeedes
ms.openlocfilehash: 0b3ca7a101a0c3979058b7094f8e51ed80e02649
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55174521"
---
# <a name="tutorial-azure-active-directory-integration-with-edcor"></a>Tutorial: Integração do Active Directory do Azure com Edcor

Neste tutorial, saiba como integrar Edcor com o Azure Active Directory (Azure AD).

Integrar Edcor no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Edcor.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Edcor (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Edcor, terá dos seguintes itens:

- Uma subscrição do Azure
- Um Edcor logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Edcor da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-edcor-from-the-gallery"></a>Adicionando Edcor da Galeria
Para configurar a integração do Edcor com o Azure AD, terá de adicionar Edcor a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Edcor a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

4. Na caixa de pesquisa, escreva **Edcor**, selecione **Edcor** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Edcor na lista de resultados](./media/edcor-tutorial/tutorial_edcor_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com Edcor com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Edcor a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Edcor deve ser estabelecido.

Edcor, atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com Edcor, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste Edcor](#create-an-edcor-test-user)**  - para ter um equivalente da Eduarda Almeida na Edcor que está ligado à representação de utilizador do Azure AD.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo Edcor.

**Para configurar o Azure AD início de sessão único com Edcor, execute os seguintes passos:**

1. No portal do Azure, sobre o **Edcor** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/edcor-tutorial/tutorial_edcor_samlbase.png)

3. Sobre o **Edcor domínio e URLs** secção, execute os seguintes passos:

    ![Edcor domínio e URLs únicas início de sessão em informações](./media/edcor-tutorial/tutorial_edcor_url1.png)

    Na **identificador** caixa de texto, escreva um URL: `https://sso.edcor.com/sp/ACS.saml2`


4. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![O link de download de certificado](./media/edcor-tutorial/tutorial_edcor_certificate.png) 

5. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/edcor-tutorial/tutorial_general_400.png)

6. Sobre o **Edcor configuração** secção, clique em **configurar Edcor** para abrir **configurar início de sessão** janela. Copiar o **URL de fim de sessão e o ID de entidade de SAML** partir o **secção de referência rápida.**

    ![Configuração de Edcor](./media/edcor-tutorial/tutorial_edcor_configure.png) 

7. Para configurar o início de sessão único num **Edcor** lado, terá de enviar o transferido **XML de metadados**, **URL de fim de sessão** e **ID de entidade de SAML** para [Equipa de suporte de Edcor](http://www.edcor.com/contact-us-2/). Se definir esta definição para que a ligação de SAML SSO definidas corretamente em ambos os lados.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [Documentação do Azure AD incorporado]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/edcor-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/edcor-tutorial/create_aaduser_02.png)

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/edcor-tutorial/create_aaduser_03.png)

4. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/edcor-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-an-edcor-test-user"></a>Criar um utilizador de teste Edcor

Nesta secção, vai criar um usuário chamado Eduarda Almeida no Edcor. Trabalhar com [equipa de suporte de Edcor](http://www.edcor.com/contact-us-2/) para adicionar os utilizadores na plataforma Edcor. Os utilizadores tem de ser criados e ativados antes de utilizar o início de sessão único.  

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para Edcor.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a Edcor, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **Edcor**.

    ![A ligação de Edcor na lista de aplicações](./media/edcor-tutorial/tutorial_edcor_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar o início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico Edcor no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo Edcor.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/edcor-tutorial/tutorial_general_01.png
[2]: ./media/edcor-tutorial/tutorial_general_02.png
[3]: ./media/edcor-tutorial/tutorial_general_03.png
[4]: ./media/edcor-tutorial/tutorial_general_04.png

[100]: ./media/edcor-tutorial/tutorial_general_100.png

[200]: ./media/edcor-tutorial/tutorial_general_200.png
[201]: ./media/edcor-tutorial/tutorial_general_201.png
[202]: ./media/edcor-tutorial/tutorial_general_202.png
[203]: ./media/edcor-tutorial/tutorial_general_203.png

