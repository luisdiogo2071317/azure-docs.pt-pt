---
title: 'Tutorial: Integração do Azure Active Directory com ClearCompany | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e ClearCompany.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 2819da18-c7eb-43cf-aac3-1403a540bf6e
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2017
ms.author: jeedes
ms.openlocfilehash: 9cf5092dabf5649f4d063ce05dae169b90228378
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52834099"
---
# <a name="tutorial-azure-active-directory-integration-with-clearcompany"></a>Tutorial: Integração do Azure Active Directory com ClearCompany

Neste tutorial, saiba como integrar ClearCompany com o Azure Active Directory (Azure AD).

Integrar ClearCompany no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao ClearCompany.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para ClearCompany (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com ClearCompany, terá dos seguintes itens:

- Uma subscrição do Azure
- Um ClearCompany início de sessão único na subscrição ativado

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando ClearCompany da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-clearcompany-from-the-gallery"></a>Adicionando ClearCompany da Galeria
Para configurar a integração do ClearCompany com o Azure AD, terá de adicionar ClearCompany a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar ClearCompany a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

4. Na caixa de pesquisa, escreva **ClearCompany**, selecione **ClearCompany** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![ClearCompany na lista de resultados](./media/clearcompany-tutorial/tutorial_clearcompany_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com ClearCompany com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no ClearCompany a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no ClearCompany deve ser estabelecido.

ClearCompany, atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com ClearCompany, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste ClearCompany](#create-a-clearcompany-test-user)**  - para ter um equivalente da Eduarda Almeida na ClearCompany que está ligado à representação de utilizador do Azure AD.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo ClearCompany.

**Para configurar o Azure AD início de sessão único com ClearCompany, execute os seguintes passos:**

1. No portal do Azure, sobre o **ClearCompany** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/clearcompany-tutorial/tutorial_clearcompany_samlbase.png)

3. Sobre o **ClearCompany domínio e URLs** secção, execute os seguintes passos, se desejar configurar a aplicação no modo de IDP iniciado:

    ![ClearCompany domínio e URLs únicas início de sessão em informações](./media/clearcompany-tutorial/tutorial_clearcompany_url1.png)

    Na **identificador** caixa de texto, escreva o URL: `https://api.clearcompany.com`

4. Verifique **Mostrar definições de URL avançadas** e executar o passo seguinte, se desejar configurar a aplicação na **SP** iniciada pelo modo:

    ![ClearCompany domínio e URLs únicas início de sessão em informações](./media/clearcompany-tutorial/tutorial_clearcompany_url2.png)

    Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<companyname>.clearcompany.com`
    
    > [!NOTE] 
    > Valor do URL de início de sessão não é um valor real. Atualize este valor com o URL de início de sessão real. Contacte [equipa de suporte de cliente ClearCompany](https://www.clearcompany.com/support) para obter este valor. 

5. Sobre o **certificado de assinatura SAML** secção, clique em **Certificate(Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![O link de download de certificado](./media/clearcompany-tutorial/tutorial_clearcompany_certificate.png) 

6. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/clearcompany-tutorial/tutorial_general_400.png)
    
7. Sobre o **ClearCompany configuração** secção, clique em **configurar ClearCompany** para abrir **configurar início de sessão** janela. Cópia a **SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configuração de ClearCompany](./media/clearcompany-tutorial/tutorial_clearcompany_configure.png) 

8. Para configurar o início de sessão único num **ClearCompany** lado, terá de enviar o transferido **Certificate(Base64)** e **SAML único início de sessão no URL do serviço** para [ A equipa de suporte ClearCompany](https://www.clearcompany.com/support). Se definir esta definição para que a ligação de SAML SSO definidas corretamente em ambos os lados.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/clearcompany-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/clearcompany-tutorial/create_aaduser_02.png)

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/clearcompany-tutorial/create_aaduser_03.png)

4. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/clearcompany-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-clearcompany-test-user"></a>Criar um utilizador de teste ClearCompany

Nesta secção, vai criar um usuário chamado Eduarda Almeida no ClearCompany. Trabalhar com [equipa de suporte de ClearCompany](https://www.clearcompany.com/support) para adicionar os utilizadores na plataforma ClearCompany. Os utilizadores tem de ser criados e ativados antes de utilizar o início de sessão único.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para ClearCompany.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a ClearCompany, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **ClearCompany**.

    ![A ligação de ClearCompany na lista de aplicações](./media/clearcompany-tutorial/tutorial_clearcompany_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar o início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico ClearCompany no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo ClearCompany.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[1]: ./media/clearcompany-tutorial/tutorial_general_01.png
[2]: ./media/clearcompany-tutorial/tutorial_general_02.png
[3]: ./media/clearcompany-tutorial/tutorial_general_03.png
[4]: ./media/clearcompany-tutorial/tutorial_general_04.png

[100]: ./media/clearcompany-tutorial/tutorial_general_100.png

[200]: ./media/clearcompany-tutorial/tutorial_general_200.png
[201]: ./media/clearcompany-tutorial/tutorial_general_201.png
[202]: ./media/clearcompany-tutorial/tutorial_general_202.png
[203]: ./media/clearcompany-tutorial/tutorial_general_203.png

