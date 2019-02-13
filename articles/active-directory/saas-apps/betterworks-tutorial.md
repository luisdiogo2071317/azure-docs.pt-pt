---
title: 'Tutorial: Integração do Active Directory do Azure com BetterWorks | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e BetterWorks.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 5bb9505a-be02-46ae-9979-5308715d2b47
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8490a91a261d26069aca474a891a5a7ce27c71bd
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2019
ms.locfileid: "56197866"
---
# <a name="tutorial-azure-active-directory-integration-with-betterworks"></a>Tutorial: Integração do Active Directory do Azure com BetterWorks

Neste tutorial, saiba como integrar BetterWorks com o Azure Active Directory (Azure AD).

Integrar BetterWorks no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao BetterWorks
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para BetterWorks (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com BetterWorks, terá dos seguintes itens:

- Uma subscrição do Azure
- Um BetterWorks início de sessão único na subscrição ativado

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando BetterWorks da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-betterworks-from-the-gallery"></a>Adicionando BetterWorks da Galeria
Para configurar a integração do BetterWorks com o Azure AD, terá de adicionar BetterWorks a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar BetterWorks a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

1. Na caixa de pesquisa, escreva **BetterWorks**.

    ![Criar um utilizador de teste do Azure AD](./media/betterworks-tutorial/tutorial_betterworks_search.png)

1. No painel de resultados, selecione **BetterWorks**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/betterworks-tutorial/tutorial_betterworks_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com BetterWorks com base num utilizador de teste chamado "Eduarda Almeida."

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no BetterWorks a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no BetterWorks deve ser estabelecido.

BetterWorks, atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com BetterWorks, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste BetterWorks](#creating-a-betterworks-test-user)**  - para ter um equivalente da Eduarda Almeida na BetterWorks que está ligado à representação de utilizador do Azure AD.
1. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo BetterWorks.

**Para configurar o Azure AD início de sessão único com BetterWorks, execute os seguintes passos:**

1. No portal do Azure, sobre o **BetterWorks** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/betterworks-tutorial/tutorial_betterworks_samlbase.png)

1. Sobre o **BetterWorks domínio e URLs** secção, se desejar configurar a aplicação no **IDP iniciada pelo modo**:

    ![Configurar o início de sessão único](./media/betterworks-tutorial/tutorial_betterworks_url.png)

    a. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://app.betterworks.com/saml2/metadata/`

    b. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://app.betterworks.com/saml2/acs/`

1. Sobre o **BetterWorks domínio e URLs** secção, se desejar configurar a aplicação no **SP iniciada pelo modo**, execute os seguintes passos:
    
    ![Configurar o início de sessão único](./media/betterworks-tutorial/tutorial_betterworks_url1.png)

    a. Clique nas **Mostrar definições de URL avançadas**.

    b. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://app.betterworks.com`

    > [!NOTE] 
    > Essas não são valores reais. Atualize estes valores com o URL de resposta, o identificador e o início de sessão no URL real. Contacte [equipa de suporte de BetterWorks](mailto:support@betterworks.com) obter esses valores.
 
1. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![Configurar o início de sessão único](./media/betterworks-tutorial/tutorial_betterworks_certificate.png)  

1. Aplicação de BetterWorks espera que as asserções SAML num formato específico. Configure as seguintes declarações para esta aplicação. Pode gerir os valores destes atributos da "**atributo**" Guia do aplicativo. Captura de ecrã seguinte mostra um exemplo disso. 

    ![Configurar o início de sessão único](./media/betterworks-tutorial/tutorial_betterworks_attribute.png)

1. Sobre o **atributos de token SAML** caixa de diálogo, para cada linha, mostrada na tabela abaixo, execute os seguintes passos:
 
   | Nome do Atributo | Valor do Atributo |
   | -------------- |  ------------ |
   | saml_token     | bd189cf6-1701-11e6-8f90-d26992eca2a5 |

   a. Clique em **adicionar atributo** para abrir o **adicionar atributo** caixa de diálogo.

    ![Configurar o início de sessão único](./media/betterworks-tutorial/tutorial_officespace_04.png)

    ![Configurar o início de sessão único](./media/betterworks-tutorial/tutorial_officespace_05.png)

   b. Na **nome** caixa de texto, escreva o nome de atributo apresentado para essa linha. 

   c. Partir do **valor** lista, digite o valor de atributo apresentado para essa linha.
    
   d. Clique em **OK**.

1. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/betterworks-tutorial/tutorial_general_400.png)

1. Para configurar o início de sessão único num **BetterWorks** lado, terá de enviar o transferido **XML de metadados** para [equipa de suporte de BetterWorks](mailto:support@betterworks.com).


> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [Documentação do Azure AD incorporado]( https://go.microsoft.com/fwlink/?linkid=845985)
 

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/betterworks-tutorial/create_aaduser_01.png) 

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/betterworks-tutorial/create_aaduser_02.png) 

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/betterworks-tutorial/create_aaduser_03.png) 

1. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/betterworks-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-a-betterworks-test-user"></a>Criar um utilizador de teste BetterWorks

Nesta secção, vai criar um usuário chamado Eduarda Almeida no BetterWorks. Trabalhar com [equipa de suporte de BetterWorks](mailto:support@betterworks.com) para adicionar os utilizadores na plataforma BetterWorks.

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para BetterWorks.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida a BetterWorks, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **BetterWorks**.

    ![Configurar o início de sessão único](./media/betterworks-tutorial/tutorial_betterworks_app.png) 

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

O objetivo desta secção é testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico BetterWorks no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo BetterWorks.

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)


<!--Image references-->

[1]: ./media/betterworks-tutorial/tutorial_general_01.png
[2]: ./media/betterworks-tutorial/tutorial_general_02.png
[3]: ./media/betterworks-tutorial/tutorial_general_03.png
[4]: ./media/betterworks-tutorial/tutorial_general_04.png

[100]: ./media/betterworks-tutorial/tutorial_general_100.png

[200]: ./media/betterworks-tutorial/tutorial_general_200.png
[201]: ./media/betterworks-tutorial/tutorial_general_201.png
[202]: ./media/betterworks-tutorial/tutorial_general_202.png
[203]: ./media/betterworks-tutorial/tutorial_general_203.png

