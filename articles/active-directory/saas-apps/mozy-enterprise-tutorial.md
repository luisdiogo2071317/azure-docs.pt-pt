---
title: 'Tutorial: Integração do Active Directory do Azure com o Mozy Enterprise | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Mozy Enterprise.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 489b5e62-85c2-45c9-8766-326632d48114
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: jeedes
ms.openlocfilehash: 5bbaa90554e09d27a3c521d4a13eda44021721c8
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55176901"
---
# <a name="tutorial-azure-active-directory-integration-with-mozy-enterprise"></a>Tutorial: Integração do Active Directory do Azure com o Mozy Enterprise

Neste tutorial, saiba como integrar Mozy Enterprise com o Azure Active Directory (Azure AD).

Integrar Mozy Enterprise no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Mozy Enterprise
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para a empresa Mozy (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Mozy empresarial, terá dos seguintes itens:

- Uma subscrição do Azure
- Uma empresa de Mozy logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Mozy Enterprise da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-mozy-enterprise-from-the-gallery"></a>Adicionando Mozy Enterprise da Galeria
Para configurar a integração do Mozy Enterprise com o Azure AD, terá de adicionar Mozy empresarial a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Mozy empresarial a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

1. Na caixa de pesquisa, escreva **Mozy Enterprise**.

    ![Criar um utilizador de teste do Azure AD](./media/mozy-enterprise-tutorial/tutorial_mozyenterprise_search.png)

1. No painel de resultados, selecione **Mozy Enterprise**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/mozy-enterprise-tutorial/tutorial_mozyenterprise_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com o Mozy Enterprise com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador equivalente na empresa Mozy a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado na empresa Mozy deve ser estabelecido.

Na empresa Mozy, atribuir o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com Mozy empresarial, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste de Mozy Enterprise](#creating-a-mozy-enterprise-test-user)**  - para ter um equivalente da Eduarda Almeida na empresa Mozy que está ligado à representação de utilizador do Azure AD.
1. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único na sua aplicação empresarial Mozy.

**Para configurar o Azure AD início de sessão único com o Mozy Enterprise, execute os seguintes passos:**

1. No portal do Azure, sobre o **Mozy Enterprise** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/mozy-enterprise-tutorial/tutorial_mozyenterprise_samlbase.png)

1. Sobre o **Mozy Enterprise domínio e URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/mozy-enterprise-tutorial/tutorial_mozyenterprise_url.png)

    Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<tenantname>.Mozyenterprise.com`

    > [!NOTE] 
    > Este valor não é real. Atualize este valor com o URL de início de sessão real. Contacte [equipa de suporte de Mozy Enterprise Client](http://support.mozy.com/) para obter este valor.

1. Sobre o **certificado de assinatura SAML** secção, clique em **Certificate(Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![Configurar o início de sessão único](./media/mozy-enterprise-tutorial/tutorial_mozyenterprise_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/mozy-enterprise-tutorial/tutorial_general_400.png)

1. Sobre o **Mozy Enterprise Configuration** secção, clique em **empresarial de Mozy configurar** para abrir **configurar início de sessão** janela. Cópia a **ID de entidade de SAML e SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configurar o início de sessão único](./media/mozy-enterprise-tutorial/tutorial_mozyenterprise_configure.png) 

1. Numa janela do browser web diferente, inicie sessão no site da sua empresa Mozy empresarial como um administrador.

1. Na **Configuration** secção, clique em **política de autenticação**.
   
   ![Política de autenticação](./media/mozy-enterprise-tutorial/ic777314.png "política de autenticação")

1. Sobre o **política de autenticação** secção, execute os seguintes passos:
   
   ![Política de autenticação](./media/mozy-enterprise-tutorial/ic777315.png "política de autenticação")
   
   a. Selecione **serviço de diretório** como **fornecedor**.
   
   b. Selecione **utilizar o Push LDAP**.
   
   c. Clique nas **autenticação SAML** separador.
   
   d. Colar **SAML único início de sessão no URL do serviço**, que copiou do portal do Azure para o **URL de autenticação** caixa de texto.
   
   e. Colar **ID de entidade de SAML**, que copiou do portal do Azure para o **ponto final de SAML** caixa de texto.
   
   f. Abra o seu certificado codificado de base 64 transferido no bloco de notas, copie o conteúdo do mesmo para a área de transferência e, em seguida, cole o certificado completo para **certificado SAML** caixa de texto.
   
   g. Selecione **ativar o SSO para os administradores iniciar sessão com as credenciais de rede**.
   
   h. Clique em **guardar alterações**.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [Documentação do Azure AD incorporado]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/mozy-enterprise-tutorial/create_aaduser_01.png) 

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/mozy-enterprise-tutorial/create_aaduser_02.png) 

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/mozy-enterprise-tutorial/create_aaduser_03.png) 

1. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/mozy-enterprise-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-a-mozy-enterprise-test-user"></a>Criar um utilizador de teste Mozy Enterprise

Para habilitar os utilizadores do Azure AD iniciar sessão no Mozy Enterprise, tem de ser aprovisionados em Mozy Enterprise. No caso de Mozy Enterprise, o aprovisionamento é uma tarefa manual.

>[!NOTE]
>Pode utilizar quaisquer outras Mozy Enterprise utilizador conta criação ferramentas ou APIs fornecidas pelo Mozy Enterprise para aprovisionar contas de utilizador do AAD.

**Para aprovisionar contas de utilizador, execute os seguintes passos:**

1. Inicie sessão no seu **Mozy Enterprise** inquilino.

1. Clique em **usuários**e, em seguida, clique em **adicionar novo utilizador**.
   
   ![Os utilizadores](./media/mozy-enterprise-tutorial/ic777317.png "utilizadores")
   
   >[!NOTE]
   >O **adicionar novo utilizador** opção só é apresentada apenas se **Mozy** está selecionado como o fornecedor em **política de autenticação**. Se estiver configurada a autenticação SAML, em seguida, os utilizadores são adicionados automaticamente no seu primeiro início de sessão através do início de sessão único no.
    
1. Na caixa de diálogo novo utilizador, execute os seguintes passos:
   
   ![Adicionar utilizadores](./media/mozy-enterprise-tutorial/ic777318.png "adicionar utilizadores")
   
   a. Partir do **escolha um grupo de** , selecione um grupo.
   
   b. Partir do **que tipo de utilizador** , selecione um tipo.
   
   c. Na **nome de utilizador** caixa de texto, escreva o nome do utilizador do Azure AD.
   
   d. Na **E-Mail** caixa de texto, escreva o endereço de e-mail do utilizador do Azure AD.
   
   e. Selecione **enviar mensagem de e-mail do utilizador instrução**.
   
   f. Clique em **adicionar utilizador (es)**.

     >[!NOTE]
     > Depois de criar o utilizador, será enviado um e-mail ao utilizador do Azure AD que inclui uma ligação para confirmar a conta até se tornar Active Directory.

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único, concedendo acesso ao Mozy Enterprise.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida a Mozy Enterprise, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **Mozy Enterprise**.

    ![Configurar o início de sessão único](./media/mozy-enterprise-tutorial/tutorial_mozyenterprise_app.png) 

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico Mozy Enterprise no painel de acesso, deve obter a página de início de sessão do aplicativo Mozy empresarial.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[1]: ./media/mozy-enterprise-tutorial/tutorial_general_01.png
[2]: ./media/mozy-enterprise-tutorial/tutorial_general_02.png
[3]: ./media/mozy-enterprise-tutorial/tutorial_general_03.png
[4]: ./media/mozy-enterprise-tutorial/tutorial_general_04.png

[100]: ./media/mozy-enterprise-tutorial/tutorial_general_100.png

[200]: ./media/mozy-enterprise-tutorial/tutorial_general_200.png
[201]: ./media/mozy-enterprise-tutorial/tutorial_general_201.png
[202]: ./media/mozy-enterprise-tutorial/tutorial_general_202.png
[203]: ./media/mozy-enterprise-tutorial/tutorial_general_203.png

