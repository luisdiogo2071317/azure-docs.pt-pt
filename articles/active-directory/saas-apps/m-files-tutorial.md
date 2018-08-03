---
title: 'Tutorial: Integração do Azure Active Directory com o M-Files | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e o M-Files.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 4536fd49-3a65-4cff-9620-860904f726d0
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: 41b53cb785679dec47ead99188e5cefbb132d87a
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39424957"
---
# <a name="tutorial-azure-active-directory-integration-with-m-files"></a>Tutorial: Integração do Azure Active Directory com o M-Files

Neste tutorial, saiba como integrar o M-Files com o Azure Active Directory (Azure AD).

Integrar o M-Files no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao M-Files
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para M-Files (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o M-Files, precisa do seguinte:

- Uma subscrição do Azure AD
- Um M-Files logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando o M-Files da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-m-files-from-the-gallery"></a>Adicionando o M-Files da Galeria
Para configurar a integração de M-Files com o Azure AD, terá de adicionar M-Files a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar o M-Files partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

1. Na caixa de pesquisa, escreva **M-Files**.

    ![Criar um utilizador de teste do Azure AD](./media/m-files-tutorial/tutorial_m-files_search.png)

1. No painel de resultados, selecione **M-Files**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/m-files-tutorial/tutorial_m-files_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com o M-Files com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no M-Files a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado na M-Files tem de ser estabelecida.

M-Files, atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com o M-Files, precisa concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste de M-Files](#creating-a-m-files-test-user)**  - para ter um equivalente da Eduarda Almeida na M-Files, que está ligado à representação de utilizador do Azure AD.
1. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo de M-Files.

**Para configurar o Azure AD início de sessão único com o M-Files, execute os seguintes passos:**

1. No portal do Azure, sobre o **M-Files** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/m-files-tutorial/tutorial_m-files_samlbase.png)

1. Sobre o **M-Files domínio e URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/m-files-tutorial/tutorial_m-files_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<tenantname>.cloudvault.m-files.com/authentication/MFiles.AuthenticationProviders.Core/sso`

    b. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://<tenantname>.cloudvault.m-files.com`

    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o URL de início de sessão e o identificador real. Contacte [equipa de suporte de cliente de M-Files](mailto:support@m-files.com) obter esses valores. 
 
1. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![Configurar o início de sessão único](./media/m-files-tutorial/tutorial_m-files_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/m-files-tutorial/tutorial_general_400.png)

1. Para obter SSO configurado para a sua aplicação, contacte [M-Files equipa de suporte](mailto:support@m-files.com) e forneça os metadados baixado.
   
    >[!NOTE]
    >Siga os passos seguintes para configurar o SSO para que o aplicativo de área de trabalho de M ficheiro. Não existem passos adicionais são necessários se quiser configurar o SSO para a versão da web de M-Files.  

1. Siga os passos seguintes para configurar a aplicação de ambiente de trabalho de arquivo M para ativar o SSO com o Azure AD. Para transferir o M-Files, aceda a [M-Files transferir](https://www.m-files.com/en/download-latest-version) página.

1. Abra o **definições de ambiente de trabalho de M-Files** janela. Em seguida, clique em **adicionar**.
   
    ![Configurar o início de sessão único](./media/m-files-tutorial/tutorial_m_files_10.png)

1. Sobre o **propriedades de ligação do Cofre de documento** janela, execute os seguintes passos:
   
    ![Configurar o início de sessão único](./media/m-files-tutorial/tutorial_m_files_11.png)  

    Sob o servidor de secção tipo, os valores da seguinte forma:  

    a. Para **Name**, tipo `<tenant-name>.cloudvault.m-files.com`. 
 
    b. Para **número de porta**, tipo **4466**. 

    c. Para **protocolo**, selecione **HTTPS**. 

    d. Na **autenticação** campo, selecione **utilizador do Windows específicas**. Em seguida, obterá uma página de assinatura. Inserir as credenciais do Azure AD. 

    e. Para o **cofre no servidor**, selecione o Cofre correspondente no servidor.
 
    f. Clique em **OK**.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/m-files-tutorial/create_aaduser_01.png) 

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/m-files-tutorial/create_aaduser_02.png) 

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/m-files-tutorial/create_aaduser_03.png) 

1. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/m-files-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-a-m-files-test-user"></a>Criar um utilizador de teste de M-Files

O objetivo desta secção é criar um utilizador chamado Eduarda Almeida na M-Files. Trabalhar com [M-Files equipa de suporte](mailto:support@m-files.com) para adicionar os utilizadores nos ficheiros de M.

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único, concedendo acesso aos ficheiros de M.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida a M-Files, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **M-Files**.

    ![Configurar o início de sessão único](./media/m-files-tutorial/tutorial_m-files_app.png) 

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

O objetivo desta secção consiste em testar a configuração de SSO do Azure AD através do painel de acesso.

Quando clica no mosaico de M-Files no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo de M-Files.

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/m-files-tutorial/tutorial_general_01.png
[2]: ./media/m-files-tutorial/tutorial_general_02.png
[3]: ./media/m-files-tutorial/tutorial_general_03.png
[4]: ./media/m-files-tutorial/tutorial_general_04.png

[100]: ./media/m-files-tutorial/tutorial_general_100.png

[200]: ./media/m-files-tutorial/tutorial_general_200.png
[201]: ./media/m-files-tutorial/tutorial_general_201.png
[202]: ./media/m-files-tutorial/tutorial_general_202.png
[203]: ./media/m-files-tutorial/tutorial_general_203.png

