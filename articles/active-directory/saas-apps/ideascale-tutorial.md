---
title: 'Tutorial: Integração do Active Directory do Azure com IdeaScale | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e IdeaScale.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: e16dda6b-fdf9-43cc-9bbb-a523f085a8af
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: ef05adede0efff209c4aabab266a1c1c552d2b77
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2019
ms.locfileid: "56202422"
---
# <a name="tutorial-azure-active-directory-integration-with-ideascale"></a>Tutorial: Integração do Active Directory do Azure com IdeaScale

Neste tutorial, saiba como integrar IdeaScale com o Azure Active Directory (Azure AD).

Integrar IdeaScale no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao IdeaScale
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para IdeaScale (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com IdeaScale, terá dos seguintes itens:

- Uma subscrição do Azure
- Um IdeaScale início de sessão único na subscrição ativado

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando IdeaScale da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-ideascale-from-the-gallery"></a>Adicionando IdeaScale da Galeria
Para configurar a integração do IdeaScale com o Azure AD, terá de adicionar IdeaScale a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar IdeaScale a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

1. Na caixa de pesquisa, escreva **IdeaScale**.

    ![Criar um utilizador de teste do Azure AD](./media/ideascale-tutorial/tutorial_ideascale_search.png)

1. No painel de resultados, selecione **IdeaScale**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/ideascale-tutorial/tutorial_ideascale_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com IdeaScale com base num utilizador de teste chamado "Eduarda Almeida."

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no IdeaScale a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no IdeaScale deve ser estabelecido.

IdeaScale, atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com IdeaScale, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste IdeaScale](#creating-an-ideascale-test-user)**  - para ter um equivalente da Eduarda Almeida na IdeaScale que está ligado à representação de utilizador do Azure AD.
1. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo IdeaScale.

**Para configurar o Azure AD início de sessão único com IdeaScale, execute os seguintes passos:**

1. No portal do Azure, sobre o **IdeaScale** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/ideascale-tutorial/tutorial_ideascale_samlbase.png)

1. Sobre o **IdeaScale domínio e URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/ideascale-tutorial/tutorial_ideascale_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<companyname>.ideascale.com`

    b. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão:
    | |
    |--|
    | `http://<companyname>.ideascale.com`  |
    | `https://<companyname>.ideascale.com` |

    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o URL de início de sessão e o identificador real. Contacte [equipa de suporte de cliente IdeaScale](https://support.ideascale.com/) obter esses valores. 
 
1. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![Configurar o início de sessão único](./media/ideascale-tutorial/tutorial_ideascale_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/ideascale-tutorial/tutorial_general_400.png)

1. Sobre o **IdeaScale configuração** secção, clique em **configurar IdeaScale** para abrir **configurar início de sessão** janela. Cópia a **URL de fim de sessão e ID de entidade de SAML** partir a **seção de referência rápida**.

    ![Configurar o início de sessão único](./media/ideascale-tutorial/tutorial_ideascale_configure.png) 

1. Numa janela do browser web diferente, inicie sessão no site da sua empresa IdeaScale como administrador.

1. Aceda a **definições de Comunidade**.
   
    ![Definições da Comunidade](./media/ideascale-tutorial/ic790847.png "definições da Comunidade")

1. Aceda a **Security \> único definições de início de sessão**.
   
    ![Definições de início de sessão de único](./media/ideascale-tutorial/ic790848.png "único definições de início de sessão")

1. Como **tipo de início de sessão único**, selecione **SAML 2.0**.
   
    ![Único tipo de início de sessão](./media/ideascale-tutorial/ic790849.png "único tipo de início de sessão")

1. Sobre o **definições de início de sessão único** caixa de diálogo, execute os seguintes passos:
   
    ![Definições de início de sessão de único](./media/ideascale-tutorial/ic790850.png "único definições de início de sessão")
   
    a. Na **ID de entidade do IdP SAML** caixa de texto, cole o valor de **ID de entidade de SAML** que copiou do portal do Azure.

    b. Copie o conteúdo do seu ficheiro de metadados baixado a partir do portal do Azure e cole-o para o **SAML IdP metadados** caixa de texto.

    c. Na **URL de sucesso de fim de sessão** caixa de texto, cole o valor de **URL de fim de sessão** que copiou do portal do Azure.

    d. Clique em **guardar alterações**.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [Documentação do Azure AD incorporado]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/ideascale-tutorial/create_aaduser_01.png) 

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/ideascale-tutorial/create_aaduser_02.png) 

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/ideascale-tutorial/create_aaduser_03.png) 

1. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/ideascale-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-an-ideascale-test-user"></a>Criar um utilizador de teste IdeaScale

Para ativar a utilizadores do Azure AD iniciar sessão no IdeaScale, eles têm de ser aprovisionados para IdeaScale. No caso de IdeaScale, o aprovisionamento é uma tarefa manual.

**Para configurar o aprovisionamento de utilizadores, execute os seguintes passos:**

1. Inicie sessão no seu **IdeaScale** site da empresa como administrador.

1. Aceda a **definições de Comunidade**.
   
    ![Definições da Comunidade](./media/ideascale-tutorial/ic790847.png "definições da Comunidade")

1. Aceda a **definições básicas \> gestão de membro**.

1. Clique em **Adicionar membro**.
   
    ![Gestão de membro](./media/ideascale-tutorial/ic790852.png "gestão membro")

1. Na secção de adicionar o novo membro, execute os seguintes passos:
   
    ![Adicionar novo membro](./media/ideascale-tutorial/ic790853.png "adicionar novo membro")
   
    a. Na **endereços de E-Mail** caixa de texto, escreva o endereço de e-mail de uma conta do AAD válido que pretende aprovisionar.
   
    b. Clique em **guardar alterações**. 
   
    >[!NOTE]
    >O titular da conta do Azure Active Directory, obtém uma mensagem de e-mail com uma ligação para confirmar a conta até se tornar Active Directory.
      
>[!NOTE]
>Pode utilizar quaisquer outras IdeaScale utilizador conta criação ferramentas ou APIs fornecidas pelo IdeaScale para aprovisionar contas de utilizador do AAD.
 

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para IdeaScale.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida a IdeaScale, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **IdeaScale**.

    ![Configurar o início de sessão único](./media/ideascale-tutorial/tutorial_ideascale_app.png) 

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único


O objetivo desta secção é testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico IdeaScale no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo IdeaScale.

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/ideascale-tutorial/tutorial_general_01.png
[2]: ./media/ideascale-tutorial/tutorial_general_02.png
[3]: ./media/ideascale-tutorial/tutorial_general_03.png
[4]: ./media/ideascale-tutorial/tutorial_general_04.png

[100]: ./media/ideascale-tutorial/tutorial_general_100.png

[200]: ./media/ideascale-tutorial/tutorial_general_200.png
[201]: ./media/ideascale-tutorial/tutorial_general_201.png
[202]: ./media/ideascale-tutorial/tutorial_general_202.png
[203]: ./media/ideascale-tutorial/tutorial_general_203.png

