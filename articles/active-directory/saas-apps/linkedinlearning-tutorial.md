---
title: 'Tutorial: Integração do Azure Active Directory com o LinkedIn Learning | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e o LinkedIn Learning.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: d5857070-bf79-4bd3-9a2a-4c1919a74946
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2018
ms.author: jeedes
ms.openlocfilehash: 02e7d9d26b389e82365f3447cceb5566244236f1
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39440913"
---
# <a name="tutorial-azure-active-directory-integration-with-linkedin-learning"></a>Tutorial: Integração do Azure Active Directory com o LinkedIn Learning

Neste tutorial, saiba como integrar o LinkedIn Learning no Azure Active Directory (Azure AD).

Integrar o LinkedIn Learning no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao LinkedIn Learning
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para o LinkedIn Learning (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o LinkedIn Learning, precisa do seguinte:

- Uma subscrição do Azure AD
- Um LinkedIn Learning início de sessão único na subscrição ativado

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionar LinkedIn Learning a partir da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-linkedin-learning-from-the-gallery"></a>Adicionar LinkedIn Learning a partir da Galeria
Para configurar a integração do LinkedIn Learning no Azure AD, terá de adicionar LinkedIn Learning a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar o LinkedIn Learning a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
1. Clique em **adicionar** botão na parte superior da caixa de diálogo.

    ![Aplicações][3]

1. Na caixa de pesquisa, escreva **LinkedIn Learning**. No painel de resultados, clique em **LinkedIn Learning** para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/linkedinlearning-tutorial/tutorial-linkedinlearning_000.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com o LinkedIn Learning com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador equivalente no LinkedIn Learning para um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no LinkedIn Learning deve ser estabelecido.

Esta relação de ligação é estabelecida atribuindo o valor do **nome de utilizador** no Azure AD como o valor da **Username** no LinkedIn Learning.

Para configurar e testar o Azure AD início de sessão único com o LinkedIn Learning, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste do LinkedIn Learning](#creating-a-linkedin-learning-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo de aprendizagem do LinkedIn.

**Para configurar o Azure AD início de sessão único com o LinkedIn Learning, execute os seguintes passos:**

1. No portal do Azure, sobre o **LinkedIn Learning** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/linkedinlearning-tutorial/tutorial-linkedin_01.png)

1. Numa janela do browser web diferente, início de sessão no seu inquilino do LinkedIn Learning como administrador.

1. Na **Centro de contas**, clique em **definições globais** sob **definições**. Além disso, selecione **Learning - predefinição** na lista pendente.

    ![Configurar o início de sessão único](./media/linkedinlearning-tutorial/tutorial_linkedin_admin_01.png)

1. Clique em **ou clique aqui para carregar e copiar campos individuais do formulário** e copie **Id de entidade** e **Url de acesso de consumidor de asserção (ACS)**

    ![Configurar o início de sessão único](./media/linkedinlearning-tutorial/tutorial_linkedin_admin_03.png)

1. No portal do Azure, em **LinkedIn Learning domínio e URLs**, execute os seguintes passos para configurar o SSO no **IdP iniciada** modo

    ![Configurar o início de sessão único](./media/linkedinlearning-tutorial/tutorial_linkedin_signon_01.png)

    a. Na **identificador** caixa de texto, introduza o **ID de entidade** copiados a partir do Portal do LinkedIn 

    b. Na **URL de resposta** caixa de texto, introduza o **Url de acesso de consumidor de asserção (ACS)** copiados a partir do Portal do LinkedIn

1. Se quiser configurar o SSO no **SP iniciada**, em seguida, clique na opção de definição de mostrar URL avançado na secção de configuração e configurar o URL de início de sessão com o seguinte padrão:

    `https://www.linkedin.com/checkpoint/enterprise/login/<AccountId>?application=learning&applicationInstanceId=<InstanceId>`

    ![Configurar o início de sessão único](./media/linkedinlearning-tutorial/tutorial_linkedin_signon_02.png)   
    
1. A aplicação do LinkedIn Learning espera que as asserções SAML num formato específico, o que requer a adição de mapeamentos de atributos personalizado à sua configuração de atributos de token SAML. Captura de ecrã seguinte mostra um exemplo disso. O valor predefinido **identificador de utilizador** é **user.userprincipalname** mas LinkedIn Learning espera que isso seja mapeado com o endereço de e-mail do utilizador. Para que pode usar **user.mail** atributo da lista ou utilize o valor do atributo adequado com base na configuração da sua organização. 

    ![Configurar o início de sessão único](./media/linkedinlearning-tutorial/updateusermail.png)
    
1. Na **atributos de utilizador** secção, clique em **ver e editar todos os outros atributos de utilizador** e definir os atributos. O utilizador tem de adicionar quatro afirmações com o nome **e-mail**, **departamento**, **firstname**, e **lastname** e o valor deve ser mapeado com **user.mail**, **user.department**, **user.givenname**, e **user.surname** , respetivamente

    | Nome do Atributo | Valor do Atributo |
    | --- | --- |
    | e-mail| User.Mail |    
    | Departamento| User.Department |
    | FirstName| User.givenName |
    | Apelido| User.Surname |
    
    ![Criar um utilizador de teste do Azure AD](./media/linkedinlearning-tutorial/userattribute.png)
    
    a. Clique em **adicionar atributo** para abrir a caixa de diálogo do atributo.

    ![Criar um utilizador de teste do Azure AD](./media/linkedinlearning-tutorial/tutorial_attribute_04.png)

    ![Criar um utilizador de teste do Azure AD](./media/linkedinlearning-tutorial/tutorial_attribute_05.png)
    
    b. Na **nome** caixa de texto, escreva o nome de atributo apresentado para essa linha.
    
    c. Partir do **valor** lista, digite o valor de atributo apresentado para essa linha.
    
    d. Clique em **Ok**

1. Execute os seguintes passos no **nome** atributo -

    a. Clique no atributo para abrir o **Editar atributo** janela.

    ![Configurar o início de sessão único](./media/linkedinlearning-tutorial/url_update.png)

    b. Eliminar o valor de URL dos **espaço de nomes**.
    
    c. Clique em **Ok** para guardar a definição.

1. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro XML em seu computador.

    ![Configurar o início de sessão único](./media/linkedinlearning-tutorial/tutorial-linkedinlearning_certificate.png)

1. Clique em **Guardar**.

    ![Configurar o início de sessão único](./media/linkedinlearning-tutorial/tutorial_general_400.png)

1. Aceda a **definições de administração do LinkedIn** secção. Carregue o ficheiro XML que transferiu a partir do portal do Azure ao clicar na opção de arquivo XML de carregar.

    ![Configurar o início de sessão único](./media/linkedinlearning-tutorial/tutorial_linkedin_metadata_03.png)

1. Clique em **no** para ativar o SSO. Estado SSO é alterado de **não ligado** para **ligado**

    ![Configurar o início de sessão único](./media/linkedinlearning-tutorial/tutorial_linkedin_admin_05.png)

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/linkedinlearning-tutorial/create_aaduser_01.png) 

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/linkedinlearning-tutorial/create_aaduser_02.png) 

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/linkedinlearning-tutorial/create_aaduser_03.png) 

1. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/linkedinlearning-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.

### <a name="creating-a-linkedin-learning-test-user"></a>Criar um utilizador de teste do LinkedIn Learning

Aplicação do LinkedIn Learning suporta apenas em tempo de aprovisionamento de utilizador e de utilizadores de autenticação são criados automaticamente no aplicativo. Sobre o administrador de definições de página no flip de portal de aprendizagem do LinkedIn o comutador **automaticamente atribuir licenças** para Active Directory apenas em tempo de aprovisionamento e isso irão também atribuir uma licença ao utilizador.

   ![Criar um utilizador de teste do Azure AD](./media/linkedinlearning-tutorial/LinkedinUserprovswitch.png)

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único, concedendo acesso para o LinkedIn Learning.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida a aprendizagem do LinkedIn, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201]

1. Na lista de aplicações, selecione **LinkedIn Learning**.

    ![Configurar o início de sessão único](./media/linkedinlearning-tutorial/tutorial-linkedinlearning_0001.png)

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202]

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.

### <a name="testing-single-sign-on"></a>Teste de início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico do LinkedIn Learning no painel de acesso, deve obter a página de início de sessão do Azure e no após o logon com êxito, deverá obter na sua aplicação de aprendizagem do LinkedIn.

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[1]: ./media/linkedinlearning-tutorial/tutorial_general_01.png
[2]: ./media/linkedinlearning-tutorial/tutorial_general_02.png
[3]: ./media/linkedinlearning-tutorial/tutorial_general_03.png
[4]: ./media/linkedinlearning-tutorial/tutorial_general_04.png

[100]: ./media/linkedinlearning-tutorial/tutorial_general_100.png

[200]: ./media/linkedinlearning-tutorial/tutorial_general_200.png
[201]: ./media/linkedinlearning-tutorial/tutorial_general_201.png
[202]: ./media/linkedinlearning-tutorial/tutorial_general_202.png
[203]: ./media/linkedinlearning-tutorial/tutorial_general_203.png
