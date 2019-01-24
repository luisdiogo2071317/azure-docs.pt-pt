---
title: 'Tutorial: Integração do Active Directory do Azure com Gigya | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Gigya.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 2c7d200b-9242-44a5-ac8a-ab3214a78e41
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2017
ms.author: jeedes
ms.openlocfilehash: e57943e6ee227548459c6060814165ef4fe5b337
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/23/2019
ms.locfileid: "54825617"
---
# <a name="tutorial-azure-active-directory-integration-with-gigya"></a>Tutorial: Integração do Active Directory do Azure com Gigya

Neste tutorial, saiba como integrar Gigya com o Azure Active Directory (Azure AD).

Integrar Gigya no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Gigya
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Gigya (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Gigya, terá dos seguintes itens:

- Uma subscrição do Azure
- Um Gigya logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Gigya da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-gigya-from-the-gallery"></a>Adicionando Gigya da Galeria
Para configurar a integração do Gigya com o Azure AD, terá de adicionar Gigya a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Gigya a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

1. Na caixa de pesquisa, escreva **Gigya**.

    ![Criar um utilizador de teste do Azure AD](./media/gigya-tutorial/tutorial_gigya_search.png)

1. No painel de resultados, selecione **Gigya**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/gigya-tutorial/tutorial_gigya_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com Gigya com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Gigya a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Gigya deve ser estabelecido.

Gigya, atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com Gigya, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste Gigya](#creating-a-gigya-test-user)**  - para ter um equivalente da Eduarda Almeida na Gigya que está ligado à representação de utilizador do Azure AD.
1. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo Gigya.

**Para configurar o Azure AD início de sessão único com Gigya, execute os seguintes passos:**

1. No portal do Azure, sobre o **Gigya** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/gigya-tutorial/tutorial_gigya_samlbase.png)

1. Sobre o **Gigya domínio e URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/gigya-tutorial/tutorial_gigya_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `http://<companyname>.gigya.com`

    b. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://fidm.gigya.com/saml/v2.0/<companyname>`

    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o URL de início de sessão e o identificador real. Contacte [equipa de suporte de cliente Gigya](https://www.gigya.com/support-policy/) obter esses valores. 
 
1. Sobre o **certificado de assinatura SAML** secção, clique em **Certificate(Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![Configurar o início de sessão único](./media/gigya-tutorial/tutorial_gigya_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/gigya-tutorial/tutorial_general_400.png)

1. Sobre o **Gigya configuração** secção, clique em **configurar Gigya** para abrir **configurar início de sessão** janela. Cópia a **ID de entidade de SAML e SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configurar o início de sessão único](./media/gigya-tutorial/tutorial_gigya_configure.png) 

1. Numa janela do browser web diferente, inicie sessão no site da sua empresa Gigya como um administrador.

1. Aceda a **definições \> início de sessão de SAML**e, em seguida, clique nas **Add** botão.
   
    ![Início de sessão SAML](./media/gigya-tutorial/ic789532.png "início de sessão SAML")

1. Na **início de sessão de SAML** secção, execute os seguintes passos:
   
    ![Configuração de SAML](./media/gigya-tutorial/ic789533.png "configuração de SAML")
   
    a. Na **nome** caixa de texto, escreva um nome para a sua configuração.
   
    b. Na **emissor** caixa de texto, cole o valor de **ID de entidade de SAML** que copiou do Portal do Azure. 
   
    c. Na **único URL de início de sessão no serviço** caixa de texto, cole o valor de **único URL de início de sessão no serviço** que copiou do Portal do Azure.
   
    d. Na **formato do nome do ID** caixa de texto, cole o valor de **formato do nome do identificador** que copiou do Portal do Azure.
   
    e. Abra o certificado com codificação base 64 no bloco de notas transferido a partir do portal do Azure, copie o conteúdo do mesmo para a área de transferência e, em seguida, cole-os para o **certificado X.509** caixa de texto.
   
    f. Clique em **guardar as definições de**.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [Documentação do Azure AD incorporado]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/gigya-tutorial/create_aaduser_01.png) 

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/gigya-tutorial/create_aaduser_02.png) 

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/gigya-tutorial/create_aaduser_03.png) 

1. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/gigya-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-a-gigya-test-user"></a>Criar um utilizador de teste Gigya

Para habilitar logon Gigya de utilizadores do Azure AD, tem de ser aprovisionados em Gigya.  
No caso de Gigya, o aprovisionamento é uma tarefa manual.

### <a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Para aprovisionar contas de utilizador, execute os seguintes passos:

1. Inicie sessão no seu **Gigya** site da empresa como administrador.

1. Aceda a **administrador \> gerir utilizadores**e, em seguida, clique em **convidar utilizadores**.
   
    ![Gerir utilizadores](./media/gigya-tutorial/ic789535.png "gerir utilizadores")

1. Na caixa de diálogo Convidar utilizadores, execute os seguintes passos:
   
    ![Convidar utilizadores](./media/gigya-tutorial/ic789536.png "convidar utilizadores")
   
    a. Na **E-Mail** caixa de texto, escreva o alias de e-mail de uma conta válida do Azure Active Directory que pretende aprovisionar.
    
    b. Clique em **convidar utilizador**.
      
    > [!NOTE]
    > O titular da conta do Azure Active Directory irá receber uma mensagem de e-mail que inclui uma ligação para confirmar a conta até se tornar Active Directory.
    > 
    

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para Gigya.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida a Gigya, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **Gigya**.

    ![Configurar o início de sessão único](./media/gigya-tutorial/tutorial_gigya_app.png) 

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

O objetivo desta secção consiste em testar a configuração de SSO do Azure AD através do painel de acesso.

Quando clica no mosaico Gigya no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo Gigya.

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/gigya-tutorial/tutorial_general_01.png
[2]: ./media/gigya-tutorial/tutorial_general_02.png
[3]: ./media/gigya-tutorial/tutorial_general_03.png
[4]: ./media/gigya-tutorial/tutorial_general_04.png

[100]: ./media/gigya-tutorial/tutorial_general_100.png

[200]: ./media/gigya-tutorial/tutorial_general_200.png
[201]: ./media/gigya-tutorial/tutorial_general_201.png
[202]: ./media/gigya-tutorial/tutorial_general_202.png
[203]: ./media/gigya-tutorial/tutorial_general_203.png

