---
title: 'Tutorial: Integração do Azure Active Directory com o FreshDesk | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e o FreshDesk.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: c2a3e5aa-7b5a-4fe4-9285-45dbe6e8efcc
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: d0fbed347805a581fb66e0218290993817277214
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39428337"
---
# <a name="tutorial-azure-active-directory-integration-with-freshdesk"></a>Tutorial: Integração do Azure Active Directory com o FreshDesk

Neste tutorial, saiba como integrar o FreshDesk no Azure Active Directory (Azure AD).

Integrar o FreshDesk no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao FreshDesk
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para o FreshDesk (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – o portal de gestão do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o FreshDesk, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um FreshDesk início de sessão único na subscrição ativado

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não deve utilizar o seu ambiente de produção, a menos que isso é necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionar FreshDesk a partir da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-freshdesk-from-the-gallery"></a>Adicionar FreshDesk a partir da Galeria
Para configurar a integração do FreshDesk no Azure AD, terá de adicionar FreshDesk a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar FreshDesk a partir da galeria, execute os seguintes passos:**

1. Na  **[do Portal de gestão do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
1. Clique em **adicionar** botão na parte superior da caixa de diálogo.

    ![Aplicações][3]

1. Na caixa de pesquisa, escreva **FreshDesk**.

    ![Criar um utilizador de teste do Azure AD](./media/freshdesk-tutorial/tutorial_freshdesk_search.png)

1. No painel de resultados, selecione **FreshDesk**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/freshdesk-tutorial/tutorial_freshdesk_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com o FreshDesk, com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no FreshDesk a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no FreshDesk deve ser estabelecido.

Esta relação de ligação é estabelecida atribuindo o valor do **nome de utilizador** no Azure AD como o valor da **Username** no FreshDesk.

Para configurar e testar o Azure AD início de sessão único com o FreshDesk, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste do FreshDesk](#creating-a-freshdesk-test-user)**  - para ter um equivalente da Eduarda Almeida no FreshDesk a que está ligado à representação de ela do Azure AD.
1. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal de gestão do Azure e configurar início de sessão único em seu aplicativo de FreshDesk.

**Para configurar o Azure AD início de sessão único com o FreshDesk, execute os seguintes passos:**

1. No portal de gestão do Azure, sobre o **FreshDesk** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

1. Sobre o **início de sessão único** caixa de diálogo, como **modo** selecione **baseado em SAML início de sessão** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/freshdesk-tutorial/tutorial_freshdesk_samlbase.png)

1. Sobre o **FreshDesk domínio e URLs** secção, introduza o **URL de início de sessão** como: `https://<tenant-name>.freshdesk.com` ou qualquer outro valor Freshdesk sugeriu.

    ![Configurar o início de sessão único](./media/freshdesk-tutorial/tutorial_freshdesk_url.png)

    > [!NOTE] 
    > Tenha em atenção que isto não é o valor real. Terá de atualizar o valor com o URL de início de sessão real. Contacte [equipa de suporte de cliente do FreshDesk](https://freshdesk.com/helpdesk-software?utm_source=Google-AdWords&utm_medium=Search-IND-Brand&utm_campaign=Search-IND-Brand&utm_term=freshdesk&device=c&gclid=COSH2_LH7NICFVUDvAodBPgBZg) para obter este valor.  

1. Sobre o **certificado de assinatura SAML** secção, clique em **certificado** e, em seguida, guarde o certificado no seu computador.

    ![Configurar o início de sessão único](./media/freshdesk-tutorial/tutorial_freshdesk_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/freshdesk-tutorial/tutorial_general_400.png)

1. Na **configuração do FreshDesk** secção, clique em **configurar FreshDesk** para abrir a janela de início de sessão de configurar. Copie o SAML único início de sessão no URL do serviço e o URL de fim de sessão a partir da **referência rápida** secção.

    ![Configurar o início de sessão único](./media/freshdesk-tutorial/tutorial_freshdesk_configure.png)

1. Numa janela do browser web diferente, inicie sessão no site da sua empresa Freshdesk como um administrador.

1. No menu na parte superior, clique em **administrador**.
   
   ![Admin](./media/freshdesk-tutorial/IC776768.png "Admin")

1. Na **definições gerais** separador, clique em **segurança**.
   
   ![Segurança](./media/freshdesk-tutorial/IC776769.png "segurança")

1. Na **segurança** secção, execute os seguintes passos:
   
    ![Início de sessão único](./media/freshdesk-tutorial/IC776770.png "início de sessão único")
   
    a. Para **único início de sessão (SSO)**, selecione **no**.

    b. Selecione **SAML SSO**.

    c. Tipo de **SAML único início de sessão no URL do serviço** que copiou do portal do Azure para o **URL de início de sessão de SAML** caixa de texto.

    d. Tipo de **URL de fim de sessão** que copiou do portal do Azure para o **URL de fim de sessão** caixa de texto.

    e. Cópia a **Thumbprint** valor a partir do certificado transferido do portal do Azure e cole-o para o **impressão digital do certificado de segurança** caixa de texto.  
 
    >[!TIP]
    >Para obter mais detalhes, consulte [como obter o valor do thumbprint de um certificado](http://youtu.be/YKQF266SAxI). 
    
    f. Clique em **Guardar**.


### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção consiste em criar um utilizador de teste no portal do Azure Management chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal de gestão do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/freshdesk-tutorial/create_aaduser_01.png) 

1. Aceda a **utilizadores e grupos** e clique em **todos os utilizadores** para apresentar a lista de utilizadores.
    
    ![Criar um utilizador de teste do Azure AD](./media/freshdesk-tutorial/create_aaduser_02.png) 

1. Na parte superior da caixa de diálogo, clique em **Add** para abrir o **utilizador** caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/freshdesk-tutorial/create_aaduser_03.png) 

1. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/freshdesk-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-a-freshdesk-test-user"></a>Criar um utilizador de teste do FreshDesk

Para habilitar os utilizadores do Azure AD iniciar sessão no FreshDesk, tem de ser aprovisionados no FreshDesk.  
No caso do FreshDesk, aprovisionamento é uma tarefa manual.

**Para aprovisionar contas de utilizador, execute os seguintes passos:**

1. Inicie sessão no seu **Freshdesk** inquilino.
1. No menu na parte superior, clique em **administrador**.
   
   ![Admin](./media/freshdesk-tutorial/IC776772.png "Admin")

1. Na **definições gerais** separador, clique em **agentes**.
   
   ![Agentes](./media/freshdesk-tutorial/IC776773.png "agentes")

1. Clique em **novo agente**.
   
    ![Novo agente](./media/freshdesk-tutorial/IC776774.png "novo agente")

1. Na caixa de diálogo informações do agente, execute os seguintes passos:
   
   ![Informações de agentes](./media/freshdesk-tutorial/IC776775.png "informações de agentes")
   
   a. Na **FullName** caixa de texto, escreva o nome da conta do Azure AD que pretende aprovisionar.

   b. Na **E-Mail** caixa de texto, o Azure AD de tipo de endereço de e-mail da conta do Azure AD que pretende aprovisionar.

   c. Na **Title** caixa de texto, digite o título da conta do Azure AD que pretende aprovisionar.

   d. Selecione **função de agentes**e, em seguida, clique em **atribuir**.
       
   e. Clique em **Guardar**.     
   
    >[!NOTE]
    >O titular da conta do Azure AD irá receber uma mensagem de e-mail que inclui uma ligação para confirmar a conta antes de ser ativado. 
    > 
    
    >[!NOTE]
    >Pode utilizar quaisquer outras Freshdesk utilizador conta criação ferramentas ou APIs fornecidas pelo Freshdesk para aprovisionar contas de utilizador do AAD. para o FreshDesk.

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único, concedendo acesso à caixa.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida para o FreshDesk, execute os seguintes passos:**

1. No portal de gestão do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **FreshDesk**.

    ![Configurar o início de sessão único](./media/freshdesk-tutorial/tutorial_freshdesk_app.png) 

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico do FreshDesk no painel de acesso, deve obter a página de início de sessão para obter com sessão iniciada para a sua aplicação de FreshDesk.

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/freshdesk-tutorial/tutorial_general_01.png
[2]: ./media/freshdesk-tutorial/tutorial_general_02.png
[3]: ./media/freshdesk-tutorial/tutorial_general_03.png
[4]: ./media/freshdesk-tutorial/tutorial_general_04.png

[100]: ./media/freshdesk-tutorial/tutorial_general_100.png

[200]: ./media/freshdesk-tutorial/tutorial_general_200.png
[201]: ./media/freshdesk-tutorial/tutorial_general_201.png
[202]: ./media/freshdesk-tutorial/tutorial_general_202.png
[203]: ./media/freshdesk-tutorial/tutorial_general_203.png

