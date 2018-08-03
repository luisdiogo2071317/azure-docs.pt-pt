---
title: 'Tutorial: Integração do Azure Active Directory com o Velpic SAML | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Velpic SAML.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: jeedes
ms.openlocfilehash: 2ca95f6fd94036e86aae2059c05a3fbb0380005e
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39446301"
---
# <a name="tutorial-azure-active-directory-integration-with-velpic-saml"></a>Tutorial: Integração do Azure Active Directory com Velpic SAML

Neste tutorial, saiba como integrar Velpic SAML com o Azure Active Directory (Azure AD).

Integrar Velpic SAML com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Velpic SAML
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Velpic SAML (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – o portal de gestão do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Velpic SAML, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Velpic SAML início de sessão único na subscrição ativado

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não deve utilizar o seu ambiente de produção, a menos que isso é necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Velpic SAML da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-velpic-saml-from-the-gallery"></a>Adicionando Velpic SAML da Galeria
Para configurar a integração de Velpic SAML para o Azure AD, terá de adicionar Velpic SAML a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Velpic SAML a partir da galeria, execute os seguintes passos:**

1. Na  **[do Portal de gestão do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
1. Clique em **adicionar** botão na parte superior da caixa de diálogo.

    ![Aplicações][3]

1. Na caixa de pesquisa, escreva **Velpic SAML**.

    ![Criar um utilizador de teste do Azure AD](./media/velpicsaml-tutorial/tutorial_velpicsaml_search.png)

1. No painel de resultados, selecione **Velpic SAML**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/velpicsaml-tutorial/tutorial_velpicsaml_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com Velpic SAML com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Velpic SAML a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado na Velpic SAML deve ser estabelecido.

Esta relação de ligação é estabelecida atribuindo o valor do **nome de utilizador** no Azure AD como o valor do **Username** na Velpic SAML.

Para configurar e testar o Azure AD início de sessão único com Velpic SAML, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste de Velpic SAML](#creating-a-velpic-saml-test-user)**  - para ter um equivalente da Eduarda Almeida na Velpic SAML, que está ligado à representação de ela do Azure AD.
1. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal de gestão do Azure e configurar início de sessão único em seu aplicativo Velpic SAML.

**Para configurar o Azure AD início de sessão único com Velpic SAML, execute os seguintes passos:**

1. No portal de gestão do Azure, sobre o **Velpic SAML** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

1. Sobre o **início de sessão único** caixa de diálogo, como **modo** selecione **baseado em SAML início de sessão** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/velpicsaml-tutorial/tutorial_velpicsaml_samlbase.png)

1. Introduza os detalhes no **Velpic SAML domínio e URLs** secção -

    ![Configurar o início de sessão único](./media/velpicsaml-tutorial/tutorial_velpicsaml_url.png)

    a. Na **URL de início de sessão** caixa de texto, digite o valor como: `https://<sub-domain>.velpicsaml.net`

    b. Na **identificador** caixa de texto, colar a **"URL de início de sessão único"** valor `https://auth.velpic.com/saml/v2/<entity-id>/login`
    
    > [!NOTE]
    > Tenha em atenção que o URL de início de sessão será fornecido pela equipe do Velpic SAML e o valor do identificador estará disponível quando configurar o plug-in do SSO no lado de Velpic SAML. Precisa copiar esse valor de página de aplicativo Velpic SAML e cole-o aqui.

1. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro XML em seu computador.

    ![Configurar o início de sessão único](./media/velpicsaml-tutorial/tutorial_velpicsaml_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/velpicsaml-tutorial/tutorial_general_400.png)

1. Na secção de configuração de SAML Velpic, clique em configurar o Velpic SAML para abrir a janela de início de sessão de configurar. Copie o ID de entidade de SAML da seção de referência rápida.

1. Numa janela do browser web diferente, inicie sessão no site da sua empresa Velpic SAML como um administrador.

1. Clique em **gerir** separador e aceda à **integração** secção onde tem de clicar em **plug-ins** botão para criar o novo plug-in para início de sessão.

    ![Plug-in](./media/velpicsaml-tutorial/velpic_1.png)

1. Clique nas **"Adicionar plug-in"** botão.
    
    ![Plug-in](./media/velpicsaml-tutorial/velpic_2.png)

1. Clique nas **SAML** mosaico na página Adicionar plug-in.
    
    ![Plug-in](./media/velpicsaml-tutorial/velpic_3.png)

1. Introduza o nome do novo plug-in do SAML e clique nas **'Add'** botão.

    ![Plug-in](./media/velpicsaml-tutorial/velpic_4.png)

1. Introduza os detalhes da seguinte forma:

    ![Plug-in](./media/velpicsaml-tutorial/velpic_5.png)

    a. Na **nome** caixa de texto, escreva o nome do plug-in SAML.

    b. No **URL de emissor** caixa de texto, colar a **ID de entidade de SAML** copiou do **configurar início de sessão** janela do portal do Azure.

    c. Na **fornecedor de configuração de metadados** carregar o ficheiro de metadados XML que transferiu a partir do portal do Azure.

    d. Também pode optar por ativar SAML apenas em tempo de aprovisionamento, permitindo que o **'Automática criar novos utilizadores'** caixa de verificação. Se um utilizador não existe no Velpic e este sinalizador não estiver ativado, o início de sessão do Azure irá falhar. Se o sinalizador estiver ativado o usuário será automaticamente provisionado no Velpic no momento do início de sessão. 

    e. Copiar o **início de sessão no URL único** o texto de caixa e cole-a no portal do Azure.
    
    f. Clique em **Guardar**.

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção consiste em criar um utilizador de teste no portal do Azure Management chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal de gestão do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/velpicsaml-tutorial/create_aaduser_01.png) 

1. Aceda a **utilizadores e grupos** e clique em **todos os utilizadores** para apresentar a lista de utilizadores.
    
    ![Criar um utilizador de teste do Azure AD](./media/velpicsaml-tutorial/create_aaduser_02.png) 

1. Na parte superior da caixa de diálogo, clique em **Add** para abrir o **utilizador** caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/velpicsaml-tutorial/create_aaduser_03.png) 

1. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/velpicsaml-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-a-velpic-saml-test-user"></a>Criar um utilizador de teste Velpic SAML

Neste passo, normalmente, não é necessário que o aplicativo oferece suporte apenas no tempo de aprovisionamento de utilizador. Se o aprovisionamento automático de utilizadores não estiver ativado, em seguida, a criação manual de utilizador pode ser feita conforme descrito abaixo.

Inicie sessão no site da sua empresa Velpic SAML como administrador e execute os seguintes passos:
    
1. Clique no separador de gerir e aceda à secção de utilizadores e, em seguida, clique no botão novo para adicionar utilizadores.

    ![Adicionar utilizador](./media/velpicsaml-tutorial/velpic_7.png)

1. Sobre o **"Criar o novo usuário"** caixa de diálogo página, execute os seguintes passos.

    ![Utilizador](./media/velpicsaml-tutorial/velpic_8.png)
    
    a. Na **nome próprio** caixa de texto, nome do tipo o primeiro da Eduarda Almeida.

    b. Na **Apelido** caixa de texto, digite o apelido da Eduarda Almeida.

    c. Na **nome de utilizador** caixa de texto, escreva o nome de utilizador da Eduarda Almeida.

    d. Na **E-Mail** caixa de texto, escreva o endereço de e-mail da conta da Eduarda Almeida.

    e. REST das informações é opcional, pode preenchê-lo se for necessário.
    
    f. Clique em **GUARDAR**.  

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único, concedendo acesso para Velpic SAML.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida a Velpic SAML, execute os seguintes passos:**

1. No portal de gestão do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **Velpic SAML**.

    ![Configurar o início de sessão único](./media/velpicsaml-tutorial/tutorial_velpicsaml_app.png) 

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

1. Quando clica no mosaico de Velpic SAML no painel de acesso, deve obter a página de início de sessão da aplicação de Velpic SAML. Deverá ver o **"Iniciar sessão no Azure AD"** botão na página de início de sessão.

    ![Plug-in](./media/velpicsaml-tutorial/velpic_6.png)

1. Clique nas **"Iniciar sessão no Azure AD"** botão para iniciar sessão no Velpic com a sua conta do Azure AD.


## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/velpicsaml-tutorial/tutorial_general_01.png
[2]: ./media/velpicsaml-tutorial/tutorial_general_02.png
[3]: ./media/velpicsaml-tutorial/tutorial_general_03.png
[4]: ./media/velpicsaml-tutorial/tutorial_general_04.png

[100]: ./media/velpicsaml-tutorial/tutorial_general_100.png

[200]: ./media/velpicsaml-tutorial/tutorial_general_200.png
[201]: ./media/velpicsaml-tutorial/tutorial_general_201.png
[202]: ./media/velpicsaml-tutorial/tutorial_general_202.png
[203]: ./media/velpicsaml-tutorial/tutorial_general_203.png

