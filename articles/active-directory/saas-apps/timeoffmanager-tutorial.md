---
title: 'Tutorial: Integração do Azure Active Directory com TimeOffManager | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e TimeOffManager.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 3685912f-d5aa-4730-ab58-35a088fc1cc3
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: e7f0f6bb778dedeea61b74b5ca0c2edbadd5279b
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39430292"
---
# <a name="tutorial-azure-active-directory-integration-with-timeoffmanager"></a>Tutorial: Integração do Azure Active Directory com TimeOffManager

Neste tutorial, saiba como integrar TimeOffManager com o Azure Active Directory (Azure AD).

Integrar TimeOffManager no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao TimeOffManager
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para TimeOffManager (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com TimeOffManager, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um TimeOffManager logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionar TimeOffManager a partir da Galeria
1. Configurar e testar o Azure AD início de sessão único

## <a name="add-timeoffmanager-from-the-gallery"></a>Adicionar TimeOffManager a partir da Galeria
Para configurar a integração do TimeOffManager com o Azure AD, terá de adicionar TimeOffManager a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar TimeOffManager a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

1. Na caixa de pesquisa, escreva **TimeOffManager**, selecione **TimeOffManager** do painel de resultados e clique em **Add** botão para adicionar a aplicação.

    ![Adicionar a partir da Galeria](./media/timeoffmanager-tutorial/tutorial_timeoffmanager_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com TimeOffManager com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no TimeOffManager a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no TimeOffManager deve ser estabelecido.

TimeOffManager, atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com TimeOffManager, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste TimeOffManager](#create-a-timeoffmanager-test-user)**  - para ter um equivalente da Eduarda Almeida na TimeOffManager que está ligado à representação de utilizador do Azure AD.
1. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo TimeOffManager.

**Para configurar o Azure AD início de sessão único com TimeOffManager, execute os seguintes passos:**

1. No portal do Azure, sobre o **TimeOffManager** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![SAML com base em início de sessão](./media/timeoffmanager-tutorial/tutorial_timeoffmanager_samlbase.png)

1. Sobre o **TimeOffManager domínio e URLs** secção, efetue o seguinte:

     ![Secção TimeOffManager domínio e URLs](./media/timeoffmanager-tutorial/tutorial_timeoffmanager_url.png)

    Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://www.timeoffmanager.com/cpanel/sso/consume.aspx?company_id=<companyid>`

    > [!NOTE] 
    > Este valor não é real. Atualize este valor com o URL de resposta real. Pode obter este valor a partir **início de sessão único na página de definições** qual será explicado mais tarde no tutorial ou contacte [equipa de suporte de TimeOffManager](https://www.purelyhr.com/contact-us).
 
1. Sobre o **certificado de assinatura SAML** secção, clique em **certificado (Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![Secção do certificado de assinatura SAML](./media/timeoffmanager-tutorial/tutorial_timeoffmanager_certificate.png) 

1. É o objetivo desta secção descrevem como permitir que os utilizadores sejam autenticados TimeOffManger com suas contas no Azure AD com Federação com base no protocolo SAML.
    
    Seu aplicativo TimeOffManger espera que as asserções SAML num formato específico, o que requer a adição de mapeamentos de atributos personalizado à sua configuração de atributos de token SAML. Captura de ecrã seguinte mostra um exemplo disso.

    ![atributos de token SAML](./media/timeoffmanager-tutorial/tutorial_timeoffmanager_attrb.png "atributos de token saml")
    
    | Nome do Atributo | Valor do Atributo |
    | --- | --- |
    | FirstName |User.givenName |
    | Apelido |User.Surname |
    | Email |User.Mail |
    
    a.  Para cada linha de dados na tabela acima, clique em **adicione o atributo de utilizador**.
    
    ![atributos de token SAML](./media/timeoffmanager-tutorial/tutorial_timeoffmanager_addattrb.png "atributos de token saml")
    
    ![atributos de token SAML](./media/timeoffmanager-tutorial/tutorial_timeoffmanager_addattrb1.png "atributos de token saml")
    
    b.  Na **nome de atributo** caixa de texto, escreva o nome de atributo apresentado para essa linha.
    
    c.  Na **valor do atributo** caixa de texto, selecione o valor do atributo apresentado para essa linha.
    
    d.  Clique em **OK**.
    
1. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/timeoffmanager-tutorial/tutorial_general_400.png)

1. Sobre o **TimeOffManager configuração** secção, clique em **configurar TimeOffManager** para abrir **configurar início de sessão** janela. Cópia a **URL de fim de sessão, o ID de entidade de SAML e o SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Seção de configuração de TimeOffManager](./media/timeoffmanager-tutorial/tutorial_timeoffmanager_configure.png) 

1. Numa janela do browser web diferente, inicie sessão no site da sua empresa TimeOffManager como um administrador.

1. Aceda a **conta \> opções de conta \> único configurações de logon**.
   
   ![Único configurações de logon](./media/timeoffmanager-tutorial/ic795917.png "único configurações de logon")
1. Na **definições de início de sessão único** secção, execute os seguintes passos:
   
   ![Único configurações de logon](./media/timeoffmanager-tutorial/ic795918.png "único configurações de logon")
   
   a. Abra o certificado com codificação base 64 no bloco de notas, copie o conteúdo do mesmo para a área de transferência e, em seguida, cole o certificado completo para **certificado X.509** caixa de texto.
   
   b. Na **Idp emissor** caixa de texto, cole o valor de **ID de entidade de SAML** que copiou do portal do Azure.
   
   c. Na **URL de ponto final de IdP** caixa de texto, cole o valor de **SAML único início de sessão no URL do serviço** que copiou do portal do Azure.
   
   d. Como **impor SAML**, selecione **não**.
   
   e. Como **Auto-criar usuários**, selecione **Sim**.
   
   f. Na **URL de fim de sessão** caixa de texto, cole o valor de **URL de fim de sessão** que copiou do portal do Azure.
   
   g. Clique em **guardar alterações**.

1. Na **definições de início de sessão único** página, copie o valor da **URL do serviço de consumidor de asserção** e cole-a no **URL de resposta** caixa de texto em **TimeOffManager Domínio e URLs** secção no portal do Azure. 

      ![Único configurações de logon](./media/timeoffmanager-tutorial/ic795915.png "único configurações de logon")

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/timeoffmanager-tutorial/create_aaduser_01.png) 

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Utilizadores e grupos--> todos os utilizadores](./media/timeoffmanager-tutorial/create_aaduser_02.png) 

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Botão Adicionar](./media/timeoffmanager-tutorial/create_aaduser_03.png) 

1. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Página de diálogo de utilizador](./media/timeoffmanager-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="create-a-timeoffmanager-test-user"></a>Criar um utilizador de teste TimeOffManager

Para habilitar logon TimeOffManager de utilizadores do Azure AD, tem de ser aprovisionados para TimeOffManager.  

TimeOffManager oferece suporte apenas no tempo de aprovisionamento de utilizador. Não existe nenhum item de ação para.  

Os utilizadores são adicionados automaticamente durante o primeiro início de sessão com o início de sessão único no.

>[!NOTE]
>Pode utilizar quaisquer outras TimeOffManager utilizador conta criação ferramentas ou APIs fornecidas pelo TimeOffManager para aprovisionar contas de utilizador do Azure AD.
> 

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para TimeOffManager.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida a TimeOffManager, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **TimeOffManager**.

    ![TimeOffManager na lista de aplicações](./media/timeoffmanager-tutorial/tutorial_timeoffmanager_app.png) 

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico TimeOffManager no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo TimeOffManager. Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/timeoffmanager-tutorial/tutorial_general_01.png
[2]: ./media/timeoffmanager-tutorial/tutorial_general_02.png
[3]: ./media/timeoffmanager-tutorial/tutorial_general_03.png
[4]: ./media/timeoffmanager-tutorial/tutorial_general_04.png

[100]: ./media/timeoffmanager-tutorial/tutorial_general_100.png

[200]: ./media/timeoffmanager-tutorial/tutorial_general_200.png
[201]: ./media/timeoffmanager-tutorial/tutorial_general_201.png
[202]: ./media/timeoffmanager-tutorial/tutorial_general_202.png
[203]: ./media/timeoffmanager-tutorial/tutorial_general_203.png

