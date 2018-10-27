---
title: 'Tutorial: Integração do Azure Active Directory com 4me | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e 4me.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 983eecc6-41f8-49b7-b7f6-dcf833dde121
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/03/2018
ms.author: jeedes
ms.openlocfilehash: c33edf13a8bcafd4a6c3d4885553fc856ec941d8
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/26/2018
ms.locfileid: "50158130"
---
# <a name="tutorial-azure-active-directory-integration-with-4me"></a>Tutorial: Integração do Azure Active Directory com 4me

Neste tutorial, saiba como integrar 4me com o Azure Active Directory (Azure AD).

Integrar 4me no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao 4me.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para 4me (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com 4me, terá dos seguintes itens:

- Uma subscrição do Azure
- Um 4me logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando 4me da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-4me-from-the-gallery"></a>Adicionando 4me da Galeria
Para configurar a integração do 4me com o Azure AD, terá de adicionar 4me a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar 4me a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

4. Na caixa de pesquisa, escreva **4me**, selecione **4me** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![4me na lista de resultados](./media/4me-tutorial/tutorial_4me_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com 4me com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no 4me a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no 4me deve ser estabelecido.

Para configurar e testar o Azure AD início de sessão único com 4me, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste 4me](#create-a-4me-test-user)**  - para ter um equivalente da Eduarda Almeida na 4me que está ligado à representação de utilizador do Azure AD.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo 4me.

**Para configurar o Azure AD início de sessão único com 4me, execute os seguintes passos:**

1. No portal do Azure, sobre o **4me** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/4me-tutorial/tutorial_4me_samlbase.png)

3. Sobre o **4me domínio e URLs** secção, execute os seguintes passos:

    ![4me domínio e URLs únicas início de sessão em informações](./media/4me-tutorial/tutorial_4me_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão:

    | Ambiente| do IdP|
    |---|---|
    | PRODUÇÃO | `https://<SUBDOMAIN>.4me.com`|
    | CONTROLE DE QUALIDADE| `https://<SUBDOMAIN>.4me.qa`|
  
    b. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão:
    
    | Ambiente| do IdP|
    |---|---|
    | PRODUÇÃO | `https://<SUBDOMAIN>.4me.com`|
    | CONTROLE DE QUALIDADE| `https://<SUBDOMAIN>.4me.qa`|

    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o URL de início de sessão e o identificador real. Contacte [equipa de suporte de cliente 4me](mailto:support@4me.com) obter esses valores. 
 
4. aplicação de 4me espera que as asserções SAML num formato específico. Configure as seguintes declarações para esta aplicação. Pode gerir os valores destes atributos do **atributos de utilizador** secção na página de integração de aplicações. Captura de ecrã seguinte mostra um exemplo disso.
    
    ![Configurar o início de sessão único](./media/4me-tutorial/tutorial_4me_attribute.png)

5. No **atributos de utilizador** secção sobre o **início de sessão único** caixa de diálogo, configurar o atributo de token de SAML conforme mostrado na imagem acima e execute os seguintes passos:
    
    | Nome do Atributo | Valor do Atributo |
    | ---------------| --------------- |    
    | first_name | User.givenName |
    | last_name | User.Surname |

    a. Clique em **adicionar atributo** para abrir o **adicionar atributo** caixa de diálogo.

    ![Configurar o início de sessão único](./media/4me-tutorial/tutorial_attribute_04.png)

    ![Configurar o início de sessão único](./media/4me-tutorial/tutorial_attribute_05.png)
    
    b. Na **nome** caixa de texto, escreva o nome de atributo apresentado para essa linha.
    
    c. Partir do **valor** lista, digite o valor de atributo apresentado para essa linha.

    d. Deixe o **espaço de nomes** em branco.
    
    d. Clique em **Ok**

6. Sobre o **certificado de assinatura SAML** secção, copie a **THUMBPRINT** valor no seu computador.

    ![O link de download de certificado](./media/4me-tutorial/tutorial_4me_certificate.png) 

7. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/4me-tutorial/tutorial_general_400.png)

8. Sobre o **4me Configuration** secção, clique em **configurar 4me** para abrir **configurar início de sessão** janela. Cópia a **URL de fim de sessão e SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configuração de 4me](./media/4me-tutorial/tutorial_4me_configure.png) 

9. Numa janela do browser web diferente, início de sessão para 4me como administrador.

10. No canto superior esquerdo, clique em **configurações** logótipo e a barra do lado esquerdo, clique em **Single Sign-On**.

    ![definições de 4me](./media/4me-tutorial/tutorial_4me_settings.png)

11. Sobre o **Single Sign-On** página, execute os seguintes passos:

    ![4me singleasignon](./media/4me-tutorial/tutorial_4me_singlesignon.png)

    a. Selecione o **ativado** opção.

    b. Na **URL de fim de sessão remoto** caixa de texto, cole o valor de **URL de fim de sessão**, que copiou do portal do Azure.

    c. Sob **SAML** na secção a **URL de SSO SAML** caixa de texto, cole o valor de **SAML único início de sessão no URL do serviço**, que copiou do portal do Azure.

    d. Na **impressão digital de certificado** caixa de texto, colar a **THUMBPRINT** valor separado por vírgula em ordem de duplets (AA:BB:CC:DD:EE:FF:GG:HH:II), que copiou do portal do Azure.

    e. Clique em **Guardar**.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/4me-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/4me-tutorial/create_aaduser_02.png)

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/4me-tutorial/create_aaduser_03.png)

4. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/4me-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-4me-test-user"></a>Criar um utilizador de teste 4me

O objetivo desta secção é criar um usuário chamado Eduarda Almeida no 4me. 4me suporta o aprovisionamento de just-in-time, que está por predefinição, ativada. Não existe nenhum item de ação para nesta secção. Um novo utilizador é criado durante uma tentativa de aceder 4me se não existir ainda.

>[!Note]
>Se precisar de criar manualmente um utilizador, contacte [equipa de suporte de 4me](mailto:support@4me.com).

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso a 4me.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a 4me, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **4me**.

    ![A ligação de 4me na lista de aplicações](./media/4me-tutorial/tutorial_4me_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar o início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico 4me no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo 4me.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/4me-tutorial/tutorial_general_01.png
[2]: ./media/4me-tutorial/tutorial_general_02.png
[3]: ./media/4me-tutorial/tutorial_general_03.png
[4]: ./media/4me-tutorial/tutorial_general_04.png

[100]: ./media/4me-tutorial/tutorial_general_100.png

[200]: ./media/4me-tutorial/tutorial_general_200.png
[201]: ./media/4me-tutorial/tutorial_general_201.png
[202]: ./media/4me-tutorial/tutorial_general_202.png
[203]: ./media/4me-tutorial/tutorial_general_203.png

