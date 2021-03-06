---
title: 'Tutorial: Integração do Active Directory do Azure com o Studio de lógica de Neota | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Neota lógica Studio.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 842605e6-a91d-42cc-a0bb-e23e67173ae2
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/03/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 66a51fda8ae9a5520189231e12fc574030209dc9
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2019
ms.locfileid: "56199583"
---
# <a name="tutorial-azure-active-directory-integration-with-neota-logic-studio"></a>Tutorial: Integração do Active Directory do Azure com o Studio de lógica de Neota

Neste tutorial, saiba como integrar Neota lógica Studio com o Azure Active Directory (Azure AD).

Integrar Neota lógica Studio no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Neota lógica Studio
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Neota lógica Studio (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo. [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Neota lógica Studio, precisa do seguinte:

- Uma subscrição do Azure
- Um Neota lógica Studio início de sessão único na subscrição ativado

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Neota lógica Studio da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-neota-logic-studio-from-the-gallery"></a>Adicionando Neota lógica Studio da Galeria

Para configurar a integração do Neota lógica Studio para o Azure AD, terá de adicionar Neota lógica Studio a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Neota lógica Studio a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

1. Na caixa de pesquisa, escreva **Neota lógica Studio**.

    ![Criar um utilizador de teste do Azure AD](./media/neotalogicstudio-tutorial/tutorial_neotalogicstudio_search.png)

1. No painel de resultados, selecione **Neota lógica Studio**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/neotalogicstudio-tutorial/tutorial_neotalogicstudio_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com Neota lógica Studio com base num utilizador de teste chamado "Eduarda Almeida."

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador equivalente no Neota lógica Studio para um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Neota lógica Studio deve ser estabelecido.

Esta relação de ligação é estabelecida atribuindo o valor do **nome de utilizador** no Azure AD como o valor do **Username** no Neota lógica Studio.

Para configurar e testar o Azure AD início de sessão único com o Studio de lógica de Neota, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste do Studio de lógica de Neota](#creating-a-neota-logic-studio-test-user)**  - para ter um equivalente da Eduarda Almeida no Neota lógica Studio que está ligado à representação de utilizador do Azure AD.
1. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo Neota lógica Studio.

**Para configurar o Azure AD início de sessão único com Neota lógica Studio, execute os seguintes passos:**

1. No portal do Azure, sobre o **Neota lógica Studio** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/neotalogicstudio-tutorial/tutorial_neotalogicstudio_samlbase.png)

1. Sobre o **Neota lógica Studio domínio e URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/neotalogicstudio-tutorial/tutorial_neotalogicstudio_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<sub domain>.neotalogic.com/a/<sub application>`

    b. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://<sub domain>.neotalogic.com/wb`

    > [!NOTE] 
    > Estes valores não são o real. Atualize estes valores com o URL de início de sessão & Identificador real. Aqui iremos sugerir-lhe utilizar o valor único de cadeia de caracteres no identificador de. Contacte [equipa de suporte de cliente do Neota lógica Studio](https://www.neotalogic.com/contact-us/) obter esses valores. 
 
1. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![Configurar o início de sessão único](./media/neotalogicstudio-tutorial/tutorial_neotalogicstudio_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/neotalogicstudio-tutorial/tutorial_general_400.png)

1. Para obter SSO configurado para a sua aplicação, contacte [suporte Neota lógica Studio](https://www.neotalogic.com/contact-us/) da Equipe e fornecê-los com transferido **XML de metadados** ficheiro.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [Documentação do Azure AD incorporado]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/neotalogicstudio-tutorial/create_aaduser_01.png) 

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/neotalogicstudio-tutorial/create_aaduser_02.png) 

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/neotalogicstudio-tutorial/create_aaduser_03.png) 

1. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/neotalogicstudio-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-a-neota-logic-studio-test-user"></a>Criar um utilizador de teste Neota lógica Studio

Nesta secção, vai criar um usuário chamado Eduarda Almeida no Neota lógica Studio. trabalhar com [equipa de suporte de cliente de Studio de lógica de Neota](https://www.neotalogic.com/contact-us/) para adicionar os utilizadores na plataforma Neota lógica Studio. Os utilizadores tem de ser criados e ativados antes de utilizar o início de sessão único. 

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso ao Neota lógica Studio.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida a Neota lógica Studio, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **Neota lógica Studio**.

    ![Configurar o início de sessão único](./media/neotalogicstudio-tutorial/tutorial_neotalogicstudio_app.png) 

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Clique no mosaico Neota lógica Studio no painel de acesso, será redirecionado à página de início de sessão de organização. Após o logon bem-sucedido, irá ser com sessão iniciada para a sua aplicação Neota lógica Studio. Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[1]: ./media/neotalogicstudio-tutorial/tutorial_general_01.png
[2]: ./media/neotalogicstudio-tutorial/tutorial_general_02.png
[3]: ./media/neotalogicstudio-tutorial/tutorial_general_03.png
[4]: ./media/neotalogicstudio-tutorial/tutorial_general_04.png

[100]: ./media/neotalogicstudio-tutorial/tutorial_general_100.png

[200]: ./media/neotalogicstudio-tutorial/tutorial_general_200.png
[201]: ./media/neotalogicstudio-tutorial/tutorial_general_201.png
[202]: ./media/neotalogicstudio-tutorial/tutorial_general_202.png
[203]: ./media/neotalogicstudio-tutorial/tutorial_general_203.png

