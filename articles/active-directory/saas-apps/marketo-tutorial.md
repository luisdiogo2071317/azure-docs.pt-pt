---
title: 'Tutorial: Integração do Active Directory do Azure com o Marketo | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e do Marketo.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: b88c45f5-d288-4717-835c-ca965add8735
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: abab9f6e38fcf69dcb04bfea0f84d883dc5267b7
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2019
ms.locfileid: "56199668"
---
# <a name="tutorial-azure-active-directory-integration-with-marketo"></a>Tutorial: Integração do Active Directory do Azure com o Marketo

Neste tutorial, saiba como integrar o Marketo com o Azure Active Directory (Azure AD).

Integração do Marketo com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Marketo
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada ao Marketo (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Marketo, terá dos seguintes itens:

- Uma subscrição do Azure
- Um Marketo início de sessão único na subscrição ativado

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando o Marketo da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-marketo-from-the-gallery"></a>Adicionando o Marketo da Galeria
Para configurar a integração do Marketo com o Azure AD, terá de adicionar Marketo a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Marketo a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

1. Na caixa de pesquisa, escreva **Marketo**.

    ![Criar um utilizador de teste do Azure AD](./media/marketo-tutorial/tutorial_marketo_search.png)

1. No painel de resultados, selecione **Marketo**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/marketo-tutorial/tutorial_marketo_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com Marketo com base num utilizador de teste chamado "Eduarda Almeida."

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Marketo para um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Marketo tem de ser estabelecida.

No Marketo, atribuir o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com o Marketo, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste do Marketo](#creating-a-marketo-test-user)**  - para ter um equivalente da Eduarda Almeida no Marketo que está ligado à representação de utilizador do Azure AD.
1. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único na sua aplicação do Marketo.

**Para configurar o Azure AD início de sessão único com o Marketo, execute os seguintes passos:**

1. No portal do Azure, sobre o **Marketo** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/marketo-tutorial/tutorial_marketo_samlbase.png)

1. Sobre o **Marketo domínio e URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/marketo-tutorial/tutorial_marketo_url.png)

    a. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://saml.marketo.com/sp`

    b. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://login.marketo.com/saml/assertion/\<munchkinid\>`

    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o identificador real e o URL de resposta. Contacte [equipa de suporte do Marketo](http://investors.marketo.com/contactus.cfm) obter esses valores.
 
1. Sobre o **certificado de assinatura SAML** secção, clique em **certificado (Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![Configurar o início de sessão único](./media/marketo-tutorial/tutorial_marketo_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/marketo-tutorial/tutorial_general_400.png)

1. Na **configuração do Marketo** secção, clique em **configurar Marketo** para abrir **configurar início de sessão** janela. Cópia a **URL de fim de sessão, o ID de entidade de SAML e o SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configurar o início de sessão único](./media/marketo-tutorial/tutorial_marketo_configure.png) 

1. Para obter o Id de Munchkin da sua aplicação, inicie sessão no Marketo com credenciais de administrador e execute as ações seguintes:
   
    a. Inicie sessão na aplicação do Marketo com credenciais de administrador.
   
    b. Clique nas **administrador** botão no painel de navegação superior.
   
    ![Configurar o início de sessão único](./media/marketo-tutorial/tutorial_marketo_06.png) 
   
    c. Navegue para o menu de integração e clique nas **Munchkin link**.
   
    ![Configurar o início de sessão único](./media/marketo-tutorial/tutorial_marketo_11.png)
   
    d. Copie o Id de Munchkin aparecem no ecrã e concluir o seu URL de resposta no Assistente de configuração do Azure AD.
   
    ![Configurar o início de sessão único](./media/marketo-tutorial/tutorial_marketo_12.png) 

1. Para configurar o SSO na aplicação, siga os passos abaixo:
   
    a. Inicie sessão na aplicação do Marketo com credenciais de administrador.
   
    b. Clique nas **administrador** botão no painel de navegação superior.
   
    ![Configurar o início de sessão único](./media/marketo-tutorial/tutorial_marketo_06.png) 
   
    c. Navegue para o menu de integração e clique em **início de sessão único**.
   
    ![Configurar o início de sessão único](./media/marketo-tutorial/tutorial_marketo_07.png) 
   
    d. Para ativar as definições de SAML, clique em **editar** botão.
   
    ![Configurar o início de sessão único](./media/marketo-tutorial/tutorial_marketo_08.png) 
   
    e. **Ativado** as definições de início de sessão único.
   
    f. Colar o **ID de entidade de SAML**, na **ID do emissor** caixa de texto.
   
    g. Na **ID de entidade** caixa de texto, introduza o URL como `http://saml.marketo.com/sp`.
   
    h. Selecione a localização de ID de utilizador como **elemento de identificador de nome**.
   
    ![Configurar o início de sessão único](./media/marketo-tutorial/tutorial_marketo_09.png)
   
    > [!NOTE]
    > Se o identificador de utilizador não é o valor UPN, em seguida, altere o valor no separador de atributo.
   
    i. Carregue o certificado que transferiu a partir do Assistente de configuração do Azure AD. **Guardar** as definições.
   
    j. Edite as definições de páginas de redirecionamento.
   
    k. Colar o **SAML único início de sessão no URL do serviço** no **URL de início de sessão** caixa de texto.
   
    l. Colar o **URL de fim de sessão** no **URL de fim de sessão** caixa de texto.
   
    m. Na **URL de erro**, cópia sua **URL de instância do Marketo** e clique em **guardar** botão para guardar as definições.
   
    ![Configurar o início de sessão único](./media/marketo-tutorial/tutorial_marketo_10.png)

1. Para ativar o SSO para os utilizadores, conclua as seguintes ações:
   
    a. Inicie sessão na aplicação do Marketo com credenciais de administrador.
   
    b. Clique nas **administrador** botão no painel de navegação superior.
   
    ![Configurar o início de sessão único](./media/marketo-tutorial/tutorial_marketo_06.png) 
   
    c. Navegue para o **Security** menu e clique em **definições de início de sessão**.
   
    ![Configurar o início de sessão único](./media/marketo-tutorial/tutorial_marketo_13.png)
   
    d. Verifique os **requerem SSO** opção e **guardar** as definições.
   
    ![Configurar o início de sessão único](./media/marketo-tutorial/tutorial_marketo_14.png)

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [Documentação do Azure AD incorporado]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/marketo-tutorial/create_aaduser_01.png) 

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/marketo-tutorial/create_aaduser_02.png) 

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/marketo-tutorial/create_aaduser_03.png) 

1. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/marketo-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-a-marketo-test-user"></a>Criar um utilizador de teste do Marketo

Nesta secção, vai criar um usuário chamado Eduarda Almeida no Marketo. Siga estes passos para criar um utilizador na plataforma do Marketo.

1. Inicie sessão na aplicação do Marketo com credenciais de administrador.

1. Clique nas **administrador** botão no painel de navegação superior.
   
    ![Configurar o início de sessão único](./media/marketo-tutorial/tutorial_marketo_06.png) 

1. Navegue para o **Security** menu e clique em **de utilizadores e funções**
   
    ![Configurar o início de sessão único](./media/marketo-tutorial/tutorial_marketo_19.png)  

1. Clique nas **convidar novo utilizador** ligação no separador utilizadores
   
    ![Configurar o início de sessão único](./media/marketo-tutorial/tutorial_marketo_15.png) 

1. No Assistente de convidar novo utilizador preencher as seguintes informações
   
    a. Introduza o utilizador **E-Mail** endereço na caixa de texto
   
    ![Configurar o início de sessão único](./media/marketo-tutorial/tutorial_marketo_16.png)
   
    b. Introduza o **nome próprio** na caixa de texto
   
    c. Introduza o **Apelido** na caixa de texto
   
    d. Clique em **Seguinte**

1. Na **permissões** separador, selecione a **funções de utilizador** e clique em **seguinte**
   
    ![Configurar o início de sessão único](./media/marketo-tutorial/tutorial_marketo_17.png)
1. Clique nas **enviar** botão para enviar o convite de utilizador
   
    ![Configurar o início de sessão único](./media/marketo-tutorial/tutorial_marketo_18.png)

1. Recebe a notificação de e-mail e tem de clicar na ligação e alterar a palavra-passe para ativar a conta de utilizador. 

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único, concedendo acesso ao Marketo.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida ao Marketo, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **Marketo**.

    ![Configurar o início de sessão único](./media/marketo-tutorial/tutorial_marketo_app.png) 

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico do Marketo no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação do Marketo.

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/marketo-tutorial/tutorial_general_01.png
[2]: ./media/marketo-tutorial/tutorial_general_02.png
[3]: ./media/marketo-tutorial/tutorial_general_03.png
[4]: ./media/marketo-tutorial/tutorial_general_04.png

[100]: ./media/marketo-tutorial/tutorial_general_100.png

[200]: ./media/marketo-tutorial/tutorial_general_200.png
[201]: ./media/marketo-tutorial/tutorial_general_201.png
[202]: ./media/marketo-tutorial/tutorial_general_202.png
[203]: ./media/marketo-tutorial/tutorial_general_203.png

