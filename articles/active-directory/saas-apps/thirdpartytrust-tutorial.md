---
title: 'Tutorial: Integração do Active Directory do Azure com ThirdPartyTrust | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e ThirdPartyTrust.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 3c496939-4201-4108-b0cc-d3e7c4244229
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/12/2018
ms.author: jeedes
ms.openlocfilehash: ace619e5eba8d5f1b4f1740f2c88fbce7bc83790
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55150520"
---
# <a name="tutorial-azure-active-directory-integration-with-thirdpartytrust"></a>Tutorial: Integração do Active Directory do Azure com ThirdPartyTrust

Neste tutorial, saiba como integrar ThirdPartyTrust com o Azure Active Directory (Azure AD).

Integrar ThirdPartyTrust no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao ThirdPartyTrust.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para ThirdPartyTrust (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com ThirdPartyTrust, terá dos seguintes itens:

- Uma subscrição do Azure
- Um ThirdPartyTrust início de sessão único na subscrição ativado

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando ThirdPartyTrust da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-thirdpartytrust-from-the-gallery"></a>Adicionando ThirdPartyTrust da Galeria
Para configurar a integração do ThirdPartyTrust com o Azure AD, terá de adicionar ThirdPartyTrust a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar ThirdPartyTrust a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

4. Na caixa de pesquisa, escreva **ThirdPartyTrust**, selecione **ThirdPartyTrust** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![ThirdPartyTrust na lista de resultados](./media/thirdpartytrust-tutorial/tutorial_thirdpartytrust_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com ThirdPartyTrust com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no ThirdPartyTrust a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no ThirdPartyTrust deve ser estabelecido.

Para configurar e testar o Azure AD início de sessão único com ThirdPartyTrust, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste ThirdPartyTrust](#create-a-thirdpartytrust-test-user)**  - para ter um equivalente da Eduarda Almeida na ThirdPartyTrust que está ligado à representação de utilizador do Azure AD.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo ThirdPartyTrust.

**Para configurar o Azure AD início de sessão único com ThirdPartyTrust, execute os seguintes passos:**

1. No portal do Azure, sobre o **ThirdPartyTrust** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/thirdpartytrust-tutorial/tutorial_thirdpartytrust_samlbase.png)

3. Sobre o **ThirdPartyTrust domínio e URLs** secção, execute os seguintes passos, se desejar configurar a aplicação no **IDP** iniciada pelo modo:

    ![ThirdPartyTrust domínio e URLs únicas início de sessão em informações](./media/thirdpartytrust-tutorial/tutorial_thirdpartytrust_url.png)

    Na **identificador** caixa de texto, escreva um URL: `https://api.thirdpartytrust.com/sai3/saml/metadata`

4. Verifique **Mostrar definições de URL avançadas** e executar o passo seguinte, se desejar configurar a aplicação na **SP** iniciada pelo modo:

    ![ThirdPartyTrust domínio e URLs únicas início de sessão em informações](./media/thirdpartytrust-tutorial/tutorial_thirdpartytrust_url1.png)

    Na **URL de início de sessão** caixa de texto, escreva um URL: `https://api.thirdpartytrust.com/sai3/test`
     
5. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![O link de download de certificado](./media/thirdpartytrust-tutorial/tutorial_thirdpartytrust_certificate.png) 

6. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/thirdpartytrust-tutorial/tutorial_general_400.png)
    
7. Para configurar o início de sessão único num **ThirdPartyTrust** lado, terá de enviar o transferido **XML de metadados** para [equipa de suporte de ThirdPartyTrust](mailto:support@thirdpartytrust.com). Se definir esta definição para que a ligação de SAML SSO definidas corretamente em ambos os lados.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [Documentação do Azure AD incorporado]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/thirdpartytrust-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/thirdpartytrust-tutorial/create_aaduser_02.png)

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/thirdpartytrust-tutorial/create_aaduser_03.png)

4. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/thirdpartytrust-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-thirdpartytrust-test-user"></a>Criar um utilizador de teste ThirdPartyTrust

Nesta secção, vai criar um usuário chamado Eduarda Almeida no ThirdPartyTrust. Trabalhar com [equipa de suporte de ThirdPartyTrust](mailto:support@thirdpartytrust.com) para adicionar os utilizadores na plataforma ThirdPartyTrust. Os utilizadores tem de ser criados e ativados antes de utilizar o início de sessão único. 


### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para ThirdPartyTrust.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a ThirdPartyTrust, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **ThirdPartyTrust**.

    ![A ligação de ThirdPartyTrust na lista de aplicações](./media/thirdpartytrust-tutorial/tutorial_thirdpartytrust_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar o início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico ThirdPartyTrust no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo ThirdPartyTrust.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/thirdpartytrust-tutorial/tutorial_general_01.png
[2]: ./media/thirdpartytrust-tutorial/tutorial_general_02.png
[3]: ./media/thirdpartytrust-tutorial/tutorial_general_03.png
[4]: ./media/thirdpartytrust-tutorial/tutorial_general_04.png

[100]: ./media/thirdpartytrust-tutorial/tutorial_general_100.png

[200]: ./media/thirdpartytrust-tutorial/tutorial_general_200.png
[201]: ./media/thirdpartytrust-tutorial/tutorial_general_201.png
[202]: ./media/thirdpartytrust-tutorial/tutorial_general_202.png
[203]: ./media/thirdpartytrust-tutorial/tutorial_general_203.png

