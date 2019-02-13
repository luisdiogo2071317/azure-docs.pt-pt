---
title: 'Tutorial: Integração do Active Directory do Azure com a forma como fazemos | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e a forma como fazemos.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 84fc4f36-ecd1-42c6-8a70-cb0f3dc15655
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5dc6d8e2cf7ac4786f30484325406a1fe696dff3
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2019
ms.locfileid: "56165132"
---
# <a name="tutorial-azure-active-directory-integration-with-way-we-do"></a>Tutorial: Integração do Active Directory do Azure com a forma como fazemos

Neste tutorial, saiba como integrar a forma como fazemos com o Azure Active Directory (Azure AD).

Integrar a forma como fazemos com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD, quem tem acesso a forma como fazemos.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para a forma como fazemos (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com a forma como fazemos, terá dos seguintes itens:

- Uma subscrição do Azure
- Ativado de forma A que início de sessão único subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando a forma como fazemos da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-way-we-do-from-the-gallery"></a>Adicionando a forma como fazemos da Galeria
Para configurar a integração de forma podemos fazer com o Azure AD, terá de adicionar a maneira que fazemos em partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar a forma como fazemos a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

4. Na caixa de pesquisa, escreva **forma fazemos**, selecione **forma fazemos** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Forma como fazemos na lista de resultados](./media/waywedo-tutorial/tutorial_waywedo_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com a forma como fazemos com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de equivalente na forma como fazemos para um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado na forma como fazemos tem de ser estabelecida.

Para configurar e testar o Azure AD início de sessão único com a forma como fazemos, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste de forma que fazemos](#create-a-way-we-do-test-user)**  - para ter um equivalente da Eduarda Almeida na forma como fazemos isso está ligada à representação de utilizador do Azure AD.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único na sua aplicação de forma que fazemos.

**Para configurar o Azure AD início de sessão único com a forma como fazemos, execute os seguintes passos:**

1. No portal do Azure, sobre o **forma fazemos** página de integração de aplicativo, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/waywedo-tutorial/tutorial_waywedo_samlbase.png)

3. Sobre o **forma podemos fazer domínio e URLs** secção, execute os seguintes passos:

    ![Informações de início de sessão de individual de forma podemos fazer domínio e URLs](./media/waywedo-tutorial/tutorial_waywedo_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<SUBDOMAIN>.waywedo.com/Authentication/ExternalSignIn`

    b. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://<SUBDOMAIN>.waywedo.com`

    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o URL de início de sessão e o identificador real. Contacte [equipa de suporte de forma podemos fazer cliente](mailto:support@waywedo.com) obter esses valores. 
 
4. Sobre o **certificado de assinatura SAML** secção, clique em **certificado (bruto)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![O link de download de certificado](./media/waywedo-tutorial/tutorial_waywedo_certificate.png) 

5. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/waywedo-tutorial/tutorial_general_400.png)

6. Sobre o **forma podemos fazer Configuration** secção, clique em **configurar a forma como fazemos** para abrir **configurar início de sessão** janela. Cópia a **ID de entidade de SAML e SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Como fazemos configuração](./media/waywedo-tutorial/tutorial_waywedo_configure.png) 

7. Numa janela do browser web diferente, início de sessão para a forma como fazemos como um administrador de segurança.

8. Clique nas **ícone de pessoa** no canto superior direito de qualquer página na forma como fazemos, em seguida, clique em **conta** no menu pendente.

    ![Conta de fazemos forma](./media/waywedo-tutorial/tutorial_waywedo_account.png) 

9. Clique nas **ícone de menu** para abrir o menu de navegação de push e clique em **início de sessão único**.

    ![Forma como fazemos único](./media/waywedo-tutorial/tutorial_waywedo_single.png)

10. Sobre o **configuração do início de sessão única** página, execute os seguintes passos:

    ![Forma como fazemos guardar](./media/waywedo-tutorial/tutorial_waywedo_save.png)

    a. Clique nas **ativar o início de sessão único** alternar para **Sim** para ativar o início de sessão único.

    b. Na **nome de início de sessão único** caixa de texto, introduza o seu nome.

    c. Na **ID de entidade** caixa de texto, cole o valor de **ID de entidade de SAML**, que copiou do portal do Azure.

    d. Na **URL de SSO SAML** caixa de texto, cole o valor de **SAML único início de sessão no URL do serviço**, que copiou do portal do Azure.

    e. Carregue o certificado ao clicar o **selecionar** junto a **certificado**.

    f. **Definições opcionais** -
    
    * Permitir palavras-passe - quando esta opção está desativada, a palavra-passe regular funciona de forma podemos fazer para que os utilizadores só podem utilizar o início de sessão único.

    * Ativar aprovisionamento automático - quando esta opção estiver ativada, o endereço de e-mail utilizado para início de sessão será automaticamente comparado com a lista de utilizadores na forma como fazemos. Se o endereço de e-mail não corresponder a um utilizador ativo na forma como fazemos, ele adiciona automaticamente uma nova conta de utilizador da pessoa a iniciar sessão, solicitando todas as informações em falta.

      > [!NOTE]
      > Os utilizadores adicionados através do início de sessão único são adicionados como usuários gerais e não estão atribuídos uma função do sistema. Um administrador pode entrar e modificar a sua função de segurança como um editor ou administrador e também pode atribuir um ou vários organograma funções. 

    g. Clique em **guardar** para manter as suas definições.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/waywedo-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/waywedo-tutorial/create_aaduser_02.png)

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/waywedo-tutorial/create_aaduser_03.png)

4. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/waywedo-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-way-we-do-test-user"></a>Criar um utilizador de teste de forma podemos fazer

O objetivo desta secção é criar um utilizador chamado Eduarda Almeida na forma como fazemos. Forma fazemos suporta o aprovisionamento just-in-time, que está por predefinição, ativada. Não existe nenhum item de ação para nesta secção. Um novo utilizador é criado durante uma tentativa de aceder de forma podemos fazer se não existir ainda.

> [!Note]
> Se precisar de criar manualmente um utilizador, contacte [equipa de suporte de forma podemos fazer cliente](mailto:support@waywedo.com).

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida a utilizar do Azure-início de sessão único, conceder acesso à forma como fazemos.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a forma como fazemos, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **forma fazemos**.

    ![A ligação de forma que fazemos na lista de aplicações](./media/waywedo-tutorial/tutorial_waywedo_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar o início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico de forma que fazemos no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação de forma que fazemos.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/waywedo-tutorial/tutorial_general_01.png
[2]: ./media/waywedo-tutorial/tutorial_general_02.png
[3]: ./media/waywedo-tutorial/tutorial_general_03.png
[4]: ./media/waywedo-tutorial/tutorial_general_04.png

[100]: ./media/waywedo-tutorial/tutorial_general_100.png

[200]: ./media/waywedo-tutorial/tutorial_general_200.png
[201]: ./media/waywedo-tutorial/tutorial_general_201.png
[202]: ./media/waywedo-tutorial/tutorial_general_202.png
[203]: ./media/waywedo-tutorial/tutorial_general_203.png

