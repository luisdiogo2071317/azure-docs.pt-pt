---
title: 'Tutorial: Integração do Azure Active Directory com o Portal de pessoal de Mimecast | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e o Portal de pessoal de Mimecast.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 345b22be-d87e-45a4-b4c0-70a67eaf9bfd
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/24/2018
ms.author: jeedes
ms.openlocfilehash: 88952d09672fb4dd2d31878d56a59cb425f53de2
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52850453"
---
# <a name="tutorial-azure-active-directory-integration-with-mimecast-personal-portal"></a>Tutorial: Integração do Azure Active Directory com o Portal de pessoal de Mimecast

Neste tutorial, saiba como integrar o Portal de Mimecast pessoais com o Azure Active Directory (Azure AD).

Integrar Portal de Mimecast pessoais no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Portal de Mimecast pessoais.
- Pode permitir que os utilizadores automaticamente obter sessão iniciada em Mimecast portal pessoal (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o Portal de pessoal de Mimecast, terá dos seguintes itens:

- Uma subscrição do Azure
- Um Portal de pessoal de Mimecast logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando o Portal de pessoal de Mimecast da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-mimecast-personal-portal-from-the-gallery"></a>Adicionando o Portal de pessoal de Mimecast da Galeria
Para configurar a integração do Portal de Mimecast pessoais no Azure AD, terá de adicionar Mimecast de Portal pessoais a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar o Portal de pessoal de Mimecast partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

1. Na caixa de pesquisa, escreva **Portal de pessoal de Mimecast**, selecione **Portal pessoal de Mimecast** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Portal de pessoal de Mimecast na lista de resultados](./media/mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com Mimecast Portal pessoal, com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador equivalente no Portal de pessoal de Mimecast a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Portal de pessoal de Mimecast deve ser estabelecido.

Para configurar e testar o Azure AD início de sessão único com o Portal de pessoal de Mimecast, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste do Portal de pessoal de Mimecast](#create-a-mimecast-personal-portal-test-user)**  - para ter um equivalente da Eduarda Almeida no Portal de pessoal de Mimecast que está ligado à representação de utilizador do Azure AD.
1. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão único no portal do Azure e configurar o início de sessão único na sua aplicação do Portal de Mimecast pessoais.

**Para configurar o Azure AD início de sessão único com o Portal de pessoal de Mimecast, execute os seguintes passos:**

1. No portal do Azure, sobre o **Portal pessoal de Mimecast** página de integração de aplicativo, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_samlbase.png)

1. Sobre o **Mimecast pessoais Portal domínio e URLs** secção, execute os seguintes passos:

    ![Domínio de Portal pessoal Mimecast e URLs únicas início de sessão em informações](./media/mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL: 

    | Região  |  Valor | 
    | --------------- | --------------- | 
    | Europa          | `https://eu-api.mimecast.com/login/saml`|
    | Estados Unidos   | `https://us-api.mimecast.com/login/saml`|
    | África do Sul    | `https://za-api.mimecast.com/login/saml`|
    | Austrália       | `https://au-api.mimecast.com/login/saml`|
    | No exterior        | `https://jer-api.mimecast.com/login/saml`|

    b. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão:

    | Região  |  Valor | 
    | --------------- | --------------- |
    | Europa          | `https://eu-api.mimecast.com/sso/<accountcode>`|
    | Estados Unidos   | `https://us-api.mimecast.com/sso/<accountcode>`|    
    | África do Sul    | `https://za-api.mimecast.com/sso/<accountcode>`|
    | Austrália       | `https://au-api.mimecast.com/sso/<accountcode>`|
    | No exterior        | `https://jer-api.mimecast.com/sso/<accountcode>`|

    c. Na **URL de resposta** caixa de texto, escreva um URL: 

    | Região  |  Valor | 
    | --------------- | --------------- | 
    | Europa          | `https://eu-api.mimecast.com/login/saml`|
    | Estados Unidos   | `https://us-api.mimecast.com/login/saml`|
    | África do Sul    | `https://za-api.mimecast.com/login/saml`|
    | Austrália       | `https://au-api.mimecast.com/login/saml`|
    | No exterior        | `https://jer-api.mimecast.com/login/saml`|
    
    > [!NOTE] 
    > O valor do identificador não é real. Atualize o valor com o identificador real. Contacte [equipa de suporte de cliente de Portal pessoal Mimecast](https://www.mimecast.com/customer-success/technical-support/) para obter o valor. 

1. Sobre o **certificado de assinatura SAML** secção, clique em **Certificate(Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![O link de download de certificado](./media/mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/mimecast-personal-portal-tutorial/tutorial_general_400.png)

1. Sobre o **Mimecast pessoal a configuração do Portal** secção, clique em **Portal pessoal de Mimecast configurar** para abrir **configurar início de sessão** janela. Cópia a **URL de fim de sessão, o ID de entidade de SAML e o SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configuração do Portal Mimecast pessoal](./media/mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_configure.png) 

1. Numa janela do browser web diferente, inicie sessão no seu Portal pessoal Mimecast como um administrador.

1. Aceda a **serviços \> aplicativos**.
   
    ![Aplicativos](./media/mimecast-personal-portal-tutorial/ic794998.png "aplicativos")

1. Clique em **perfis de autenticação**.
   
    ![Perfis de autenticação](./media/mimecast-personal-portal-tutorial/ic794999.png "perfis de autenticação")

1. Clique em **novo perfil de autenticação**.
   
    ![Novo perfil de autenticação](./media/mimecast-personal-portal-tutorial/ic795000.png "novo perfil de autenticação")

1. Na **perfil de autenticação** secção, execute os seguintes passos:
   
    ![Perfil de autenticação](./media/mimecast-personal-portal-tutorial/ic795001.png "perfil de autenticação")
   
    a. Na **Descrição** caixa de texto, escreva um nome para a sua configuração.
   
    b. Selecione **impor autenticação SAML para o Portal de pessoal de Mimecast**.
   
    c. Como **fornecedor**, selecione **Azure Active Directory**.
   
    d. Na **URL de emissor** caixa de texto, cole o valor de **ID de entidade de SAML**, que copiou do portal do Azure.
   
    e. Na **URL de início de sessão** caixa de texto, cole o valor de **SAML único início de sessão no URL do serviço**, que copiou do portal do Azure.
   
    f. Na **URL de fim de sessão** caixa de texto, cole o valor de **URL de fim de sessão**, que copiou do portal do Azure.

    g. Abra sua **base 64** certificado codificado no bloco de notas transferido a partir do portal do Azure, copie o conteúdo do mesmo para a área de transferência e, em seguida, cole-os para o **certificado do fornecedor de identidade (metadados)** caixa de texto.

    h. Selecione **permitir o início de sessão único em**.
   
    i. Clique em **Guardar**.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/mimecast-personal-portal-tutorial/create_aaduser_01.png)

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/mimecast-personal-portal-tutorial/create_aaduser_02.png)

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/mimecast-personal-portal-tutorial/create_aaduser_03.png)

1. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/mimecast-personal-portal-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-mimecast-personal-portal-test-user"></a>Criar um utilizador de teste do Portal de pessoal de Mimecast

Para permitir que os utilizadores do Azure AD inicie sessão no Portal de pessoal de Mimecast, tem de ser aprovisionados no Portal de Mimecast pessoais. No caso do Portal de pessoal Mimecast, aprovisionamento é uma tarefa manual.

É necessário registrar um domínio, antes de poder criar os utilizadores.

**Para configurar o aprovisionamento de utilizadores, execute os seguintes passos:**

1. Inicie sessão no seu **Portal de pessoal de Mimecast** como administrador.

1. Aceda a **diretórios \> interno**.
   
    ![Diretórios](./media/mimecast-personal-portal-tutorial/ic795003.png "diretórios")

1. Clique em **registar o novo domínio**.
   
    ![Registar o novo domínio](./media/mimecast-personal-portal-tutorial/ic795004.png "registar o novo domínio")

1. Depois de ter sido criado o seu novo domínio, clique em **novo endereço**.
   
    ![Novo endereço](./media/mimecast-personal-portal-tutorial/ic795005.png "novo endereço")

1. Na caixa de diálogo novo endereço, execute os seguintes passos de um Azure válido conta AD que pretende aprovisionar:
   
    ![Guarde](./media/mimecast-personal-portal-tutorial/ic795006.png "guardar")
   
    a. Na **endereço de E-Mail** caixa de texto, tipo **endereço de E-Mail** do utilizador como **BrittaSimon@contoso.com**.
    
    b. Na **nome Global** caixa de texto, tipo a **nome de utilizador** como **BrittaSimon**.

    c. Na **palavra-passe**, e **Confirmar palavra-passe** caixas de texto, tipo o **palavra-passe** do utilizador.
   
    b. Clique em **Guardar**.

>[!NOTE]
>Pode utilizar quaisquer outras ferramentas de criação da conta de utilizador de Portal Mimecast de pessoais ou APIs fornecidas pelo Portal de pessoal de Mimecast para aprovisionar contas de utilizador do Azure AD.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar do Azure-início de sessão único, concedendo acesso ao Portal de Mimecast pessoais.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida Mimecast de portal pessoal, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **Portal de pessoal de Mimecast**.

    ![A ligação do Portal de pessoal de Mimecast na lista de aplicações](./media/mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_app.png)  

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar o início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico Mimecast de Portal pessoal no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação do Portal de Mimecast pessoais.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/mimecast-personal-portal-tutorial/tutorial_general_01.png
[2]: ./media/mimecast-personal-portal-tutorial/tutorial_general_02.png
[3]: ./media/mimecast-personal-portal-tutorial/tutorial_general_03.png
[4]: ./media/mimecast-personal-portal-tutorial/tutorial_general_04.png

[100]: ./media/mimecast-personal-portal-tutorial/tutorial_general_100.png

[200]: ./media/mimecast-personal-portal-tutorial/tutorial_general_200.png
[201]: ./media/mimecast-personal-portal-tutorial/tutorial_general_201.png
[202]: ./media/mimecast-personal-portal-tutorial/tutorial_general_202.png
[203]: ./media/mimecast-personal-portal-tutorial/tutorial_general_203.png

