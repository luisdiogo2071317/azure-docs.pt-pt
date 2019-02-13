---
title: 'Tutorial: Integração do Active Directory do Azure com Carlson Wagonlit viagens | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Carlson Wagonlit viagens.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 2745e165-94ab-43b1-970a-4547b4e5b501
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/19/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: beec63eaba78302fd795fb757f9bee48bab60c3c
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2019
ms.locfileid: "56168940"
---
# <a name="tutorial-azure-active-directory-integration-with-carlson-wagonlit-travel"></a>Tutorial: Integração do Active Directory do Azure com Carlson Wagonlit viagens

Neste tutorial, saiba como integrar Carlson Wagonlit viagem com o Azure Active Directory (Azure AD).

Integrar Carlson Wagonlit viagem com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Carlson Wagonlit viagens.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Carlson Wagonlit viagens (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Carlson Wagonlit viagens, terá dos seguintes itens:

- Uma subscrição do Azure
- Uma viagem de Wagonlit Carlson logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Carlson Wagonlit deslocação da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-carlson-wagonlit-travel-from-the-gallery"></a>Adicionando Carlson Wagonlit deslocação da Galeria
Para configurar a integração de Carlson Wagonlit viagem com o Azure AD, terá de adicionar Carlson Wagonlit viagens a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Carlson Wagonlit viagens a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

4. Na caixa de pesquisa, escreva **Carlson Wagonlit viagens**, selecione **Carlson Wagonlit viagens** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Carlson Wagonlit viagens na lista de resultados](./media/carlsonwagonlit-tutorial/tutorial_carlsonwagonlittravel_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com Carlson Wagonlit viagem com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Carlson Wagonlit deslocação para um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Carlson Wagonlit viagens deve ser estabelecido.

Carlson Wagonlit viagens, atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com Carlson Wagonlit viagens, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste Carlson Wagonlit viagens](#create-a-carlson-wagonlit-travel-test-user)**  - para ter um equivalente da Eduarda Almeida na Carlson Wagonlit viagens que está ligado à representação de utilizador do Azure AD.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo Carlson Wagonlit viagens.

**Para configurar o Azure AD início de sessão único com Carlson Wagonlit viagens, execute os seguintes passos:**

1. No portal do Azure, sobre o **Carlson Wagonlit viagens** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/carlsonwagonlit-tutorial/tutorial_carlsonwagonlittravel_samlbase.png)

3. Sobre o **Carlson Wagonlit viagens domínio e URLs** secção, execute os seguintes passos:

    ![Carlson Wagonlit viagens domínio e URLs únicas início de sessão em informações](./media/carlsonwagonlit-tutorial/tutorial_carlsonwagonlittravel_url.png)

    Na **identificador** caixa de texto, digite o valor: `cwt-stage`

4. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![O link de download de certificado](./media/carlsonwagonlit-tutorial/tutorial_carlsonwagonlittravel_certificate.png) 

5. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/carlsonwagonlit-tutorial/tutorial_general_400.png)

6. Para configurar o início de sessão único num **Carlson Wagonlit viagens** lado, terá de enviar o transferido **XML de metadados** para [equipa de suporte de deslocação de Wagonlit Carlson](http://www.carlsonwagonlit.in/content/cwt/in/en/technical-assistance.html). Se definir esta definição para que a ligação de SAML SSO definidas corretamente em ambos os lados.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [Documentação do Azure AD incorporado]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/carlsonwagonlit-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/carlsonwagonlit-tutorial/create_aaduser_02.png)

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/carlsonwagonlit-tutorial/create_aaduser_03.png)

4. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/carlsonwagonlit-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-carlson-wagonlit-travel-test-user"></a>Criar um utilizador de teste Carlson Wagonlit viagens

Nesta secção, vai criar um usuário chamado Eduarda Almeida no Carlson Wagonlit viagens. Trabalhar com [equipa de suporte de deslocação de Wagonlit Carlson](http://www.carlsonwagonlit.in/content/cwt/in/en/technical-assistance.html) para adicionar os utilizadores na plataforma Carlson Wagonlit viagens. Os utilizadores tem de ser criados e ativados antes de utilizar o início de sessão único. 

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para Carlson Wagonlit viagens.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a Carlson Wagonlit viagens, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **Carlson Wagonlit viagens**.

    ![A ligação de deslocação de Wagonlit Carlson na lista de aplicações](./media/carlsonwagonlit-tutorial/tutorial_carlsonwagonlittravel_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar o início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico Carlson Wagonlit viagens no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação Carlson Wagonlit viagens.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[1]: ./media/carlsonwagonlittravel-tutorial/tutorial_general_01.png
[2]: ./media/carlsonwagonlittravel-tutorial/tutorial_general_02.png
[3]: ./media/carlsonwagonlittravel-tutorial/tutorial_general_03.png
[4]: ./media/carlsonwagonlittravel-tutorial/tutorial_general_04.png

[100]: ./media/carlsonwagonlittravel-tutorial/tutorial_general_100.png

[200]: ./media/carlsonwagonlittravel-tutorial/tutorial_general_200.png
[201]: ./media/carlsonwagonlittravel-tutorial/tutorial_general_201.png
[202]: ./media/carlsonwagonlittravel-tutorial/tutorial_general_202.png
[203]: ./media/carlsonwagonlittravel-tutorial/tutorial_general_203.png

