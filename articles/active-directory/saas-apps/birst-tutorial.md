---
title: 'Tutorial: Integração do Azure Active Directory com análises de negócio Agile Birst | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e análise de negócio Birst ágil.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 677183b1-5348-4302-88cc-5c8ab63a3c6c
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: f3c9ff07b0cbb7b3f7aa6a23887ef86a0b53af0e
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39427497"
---
# <a name="tutorial-azure-active-directory-integration-with-birst-agile-business-analytics"></a>Tutorial: Integração do Azure Active Directory com análises de negócio Birst Agile

Neste tutorial, saiba como integrar Birst Agile análise de negócio com o Azure Active Directory (Azure AD).

Análise de negócio Agile Birst a integração com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Birst Agile análise de negócio
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para análise empresarial ágil Birst (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com análise de negócio Agile Birst, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um análise de negócio Agile Birst início de sessão único na subscrição ativado

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando a análise de negócio Agile Birst da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-birst-agile-business-analytics-from-the-gallery"></a>Adicionando a análise de negócio Agile Birst da Galeria
Para configurar a integração de análise de negócio Birst Agile para o Azure AD, terá de adicionar Birst Agile análise de negócio a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Birst Agile análise de negócio a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

1. Na caixa de pesquisa, escreva **análises de negócio Agile Birst**.

    ![Criar um utilizador de teste do Azure AD](./media/birst-tutorial/tutorial_birst_search.png)

1. No painel de resultados, selecione **Birst Agile análise de negócio**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/birst-tutorial/tutorial_birst_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com Birst de análise empresarial ágil, com base num utilizador de teste chamado "Eduarda Almeida."

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de equivalente na análise de negócio Agile Birst a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado na análise de negócio Agile Birst deve ser estabelecido.

Na análise de negócio Agile Birst, atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com análises de negócio Agile Birst, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste de análise de negócio Agile Birst](#creating-a-birst-agile-business-analytics-test-user)**  - para ter um equivalente da Eduarda Almeida na análise empresarial ágil Birst que está ligado à representação de utilizador do Azure AD.
1. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão único no portal do Azure e configurar o início de sessão único na sua aplicação de análise de negócio Birst ágil.

**Para configurar o Azure AD início de sessão único com análises de negócio Agile Birst, execute os seguintes passos:**

1. No portal do Azure, sobre o **Birst Agile análise de negócio** página de integração de aplicativo, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/birst-tutorial/tutorial_birst_samlbase.png)

1. Sobre o **Birst Agile Business Analytics domínio e URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/birst-tutorial/tutorial_birst_url.png)

     Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://login.bws.birst.com/SAMLSSO/Services.aspx?birst.idpid=TENANTIDPID`

     O URL depende do Centro de dados que está localizada a sua conta de Birst: 

     * Para utilização de datacenter do EUA seguindo o padrão: `https://login.bws.birst.com/SAMLSSO/Services.aspx?birst.idpid=TENANTIDPID` 

     * Para a Europa Centro de dados, utilize o seguinte padrão: `https://login.eu1.birst.com/SAMLSSO/Services.aspx?birst.idpid=TENANTIDPID`

    > [!NOTE] 
    > Este valor não é real. Atualize o valor com o URL de início de sessão real. Contacte [equipa de suporte de cliente de análise de negócio Agile Birst](mailto:info@birst.com) para obter o valor. 
 
1. Sobre o **certificado de assinatura SAML** secção, clique em **certificado (Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![Configurar o início de sessão único](./media/birst-tutorial/tutorial_birst_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/birst-tutorial/tutorial_general_400.png)

1. Sobre o **Birst Agile configuração de análise de negócio** secção, clique em **configurar Birst Agile análise de negócio** para abrir **configurar início de sessão** janela. Cópia a **URL de fim de sessão, o ID de entidade de SAML e o SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configurar o início de sessão único](./media/birst-tutorial/tutorial_birst_configure.png) 

1. Para configurar o início de sessão único num **Birst Agile análise de negócio** lado, terá de enviar o transferido **certificado (Base64)**, **URL de fim de sessão, ID de entidade de SAML e SAML Single Sign-On URL do serviço** para [equipa de suporte de análise de negócio Agile Birst](mailto:info@birst.com). 

    > [!NOTE]
    > Mencionar à equipa de Birst que esta integração necessita de algoritmo SHA256 (não haverá suporte SHA1) para que eles podem definir o SSO no servidor apropriado, como **app2101** etc.
  

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/birst-tutorial/create_aaduser_01.png) 

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/birst-tutorial/create_aaduser_02.png) 

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/birst-tutorial/create_aaduser_03.png) 

1. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/birst-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-a-birst-agile-business-analytics-test-user"></a>Criar um utilizador de teste de análise de negócio Birst Agile

O objetivo desta secção é criar um utilizador chamado Eduarda Almeida na análise de negócio Birst ágil. Trabalhar com [equipa de suporte de análise de negócio Agile Birst](mailto:info@birst.com) para adicionar os utilizadores na conta Birst. 

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida a utilizar do Azure-início de sessão único, conceder acesso à análise de negócio Birst ágil.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida a análise de negócio Agile Birst, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **análises de negócio Agile Birst**.

    ![Configurar o início de sessão único](./media/birst-tutorial/tutorial_birst_app.png) 

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

O objetivo desta secção consiste em testar a configuração de SSO do Azure AD através do painel de acesso.

Quando clica no mosaico de análise de negócio Agile Birst no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação de análise de negócio Birst ágil. 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/birst-tutorial/tutorial_general_01.png
[2]: ./media/birst-tutorial/tutorial_general_02.png
[3]: ./media/birst-tutorial/tutorial_general_03.png
[4]: ./media/birst-tutorial/tutorial_general_04.png

[100]: ./media/birst-tutorial/tutorial_general_100.png

[200]: ./media/birst-tutorial/tutorial_general_200.png
[201]: ./media/birst-tutorial/tutorial_general_201.png
[202]: ./media/birst-tutorial/tutorial_general_202.png
[203]: ./media/birst-tutorial/tutorial_general_203.png

