---
title: 'Tutorial: Integração do Active Directory do Azure com especificamente | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e saber.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 9541d5c4-4c82-4b5b-b01a-6a3f75a2b7a1
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: f78e978e80e27babcf8d2a2736eec06f35aa5e05
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2019
ms.locfileid: "56177840"
---
# <a name="tutorial-azure-active-directory-integration-with-namely"></a>Tutorial: Integração do Active Directory do Azure com saber,

Neste tutorial, saiba como integrar, ou seja, com o Azure Active Directory (Azure AD).

Ou seja, a integração com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ou seja
- Pode permitir que os utilizadores recebem automaticamente com sessão iniciada para saber, (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar o Azure AD a integração com a saber, precisa do seguinte:

- Uma subscrição do Azure
- Um ou seja, início de sessão único de subscrição ativada

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando, ou seja, da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-namely-from-the-gallery"></a>Adicionando, ou seja, da Galeria
Para configurar a integração de saber para o Azure AD, terá de adicionar ou seja, a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar ou seja, a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

1. Na caixa de pesquisa, escreva **nomeadamente**.

    ![Criar um utilizador de teste do Azure AD](./media/namely-tutorial/tutorial_namely_search.png)

1. No painel de resultados, selecione **nomeadamente**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/namely-tutorial/tutorial_namely_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com, ou seja, com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador equivalente no saber, a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado em saber, tem de ser estabelecida.

Ou seja, atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com ou seja, precisa concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste ou seja](#creating-a-namely-test-user)**  - para ter um equivalente da Eduarda Almeida em saber que está ligada à representação de utilizador do Azure AD.
1. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único na sua aplicação ou seja.

**Para configurar o Azure AD início de sessão único com a saber, execute os seguintes passos:**

1. No portal do Azure, no **nomeadamente** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/namely-tutorial/tutorial_namely_samlbase.png)

1. Sobre o **saber, domínio e URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/namely-tutorial/tutorial_namely_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<subdomain>.namely.com`

    b. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://<subdomain>.namely.com/saml/metadata`

    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o URL de início de sessão e o identificador real. Contacte [equipa de suporte de cliente ou seja](https://www.namely.com/contact/) obter esses valores. 
 
1. Sobre o **certificado de assinatura SAML** secção, clique em **certificado (Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![Configurar o início de sessão único](./media/namely-tutorial/tutorial_namely_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/namely-tutorial/tutorial_general_400.png)

1. Sobre o **saber, a configuração** secção, clique em **ou configurar seja** para abrir **configurar início de sessão** janela. Cópia a **SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configurar o início de sessão único](./media/namely-tutorial/tutorial_namely_configure.png) 

1. Na outra janela do browser, inicie sessão no seu site da empresa ou seja, como administrador.

1. Na barra de ferramentas na parte superior, clique em **empresa**.
   
    ![Configurar o início de sessão único](./media/namely-tutorial/tutorial_namely_06.png) 

1. Clique no separador **Definições**.
   
    ![Configurar o início de sessão único](./media/namely-tutorial/tutorial_namely_07.png) 

1. Clique em **SAML**.
   
    ![Configurar o início de sessão único](./media/namely-tutorial/tutorial_namely_08.png) 

1. Sobre o **definições de SAML** página, execute os seguintes passos:
   
    ![Configurar o início de sessão único](./media/namely-tutorial/tutorial_namely_09.png)
 
    a. Clique em **ativar SAML**. 

    b. Na **url SSO de fornecedor de identidade** caixa de texto, cole o valor de **SAML único início de sessão no URL do serviço**, que copiou do portal do Azure.
    
    c. Abra o seu certificado transferido no bloco de notas, copiar o conteúdo e, em seguida, cole-o para o **certificado do fornecedor de identidade** caixa de texto.
     
    d. Clique em **Guardar**.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [Documentação do Azure AD incorporado]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/namely-tutorial/create_aaduser_01.png) 

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/namely-tutorial/create_aaduser_02.png) 

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/namely-tutorial/create_aaduser_03.png) 

1. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/namely-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-a-namely-test-user"></a>Criar um utilizador de teste ou seja,

O objetivo desta secção é criar um usuário chamado Eduarda Almeida no, ou seja.

**Para criar um usuário chamado Eduarda Almeida no, ou seja, execute os seguintes passos:**

1. Início de sessão no seu site da empresa ou seja, como administrador.

1. Na barra de ferramentas na parte superior, clique em **pessoas**.
   
    ![Configurar o início de sessão único](./media/namely-tutorial/tutorial_namely_10.png) 

1. Clique nas **Directory** separador.
   
    ![Configurar o início de sessão único](./media/namely-tutorial/tutorial_namely_11.png) 

1. Clique em **Adicionar nova pessoa**.

    ![Configurar o início de sessão único](./media/namely-tutorial/tutorial_namely_12.png)

1. Sobre o **Adicionar nova pessoa** caixa de diálogo, execute os seguintes passos:

    a. Na **nome próprio** caixa de texto, tipo **Eduarda**.

    b. Na **Apelido** caixa de texto, tipo **Simon**.

    c. Na **E-Mail** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    d. Clique em **Guardar**.

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar do Azure-início de sessão único ao conceder acesso a saber.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida para ou seja, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **nomeadamente**.

    ![Configurar o início de sessão único](./media/namely-tutorial/tutorial_namely_app.png) 

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

O objetivo desta secção consiste em testar a configuração de SSO do Azure AD através do painel de acesso.

Quando clica no mosaico ou seja, no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação ou seja,

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/namely-tutorial/tutorial_general_01.png
[2]: ./media/namely-tutorial/tutorial_general_02.png
[3]: ./media/namely-tutorial/tutorial_general_03.png
[4]: ./media/namely-tutorial/tutorial_general_04.png

[100]: ./media/namely-tutorial/tutorial_general_100.png

[200]: ./media/namely-tutorial/tutorial_general_200.png
[201]: ./media/namely-tutorial/tutorial_general_201.png
[202]: ./media/namely-tutorial/tutorial_general_202.png
[203]: ./media/namely-tutorial/tutorial_general_203.png

