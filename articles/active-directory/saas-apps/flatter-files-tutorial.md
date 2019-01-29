---
title: 'Tutorial: Integração do Active Directory do Azure com mais simples de arquivos | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e o mais simples de arquivos.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: f86fe5e3-0e91-40d6-869c-3df6912d27ea
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/21/2017
ms.author: jeedes
ms.openlocfilehash: fe15a1aa0d284913c77d62e254b8827d9d60c850
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55188240"
---
# <a name="tutorial-azure-active-directory-integration-with-flatter-files"></a>Tutorial: Integração do Active Directory do Azure com mais simples de arquivos

Neste tutorial, saiba como integrar o mais simples de arquivos com o Azure Active Directory (Azure AD).

Integrar arquivos mais simples no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso a mais simples de ficheiros
- Pode permitir que os utilizadores automaticamente obter sessão iniciada em arquivos mais simples (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com ficheiros mais simples, precisa do seguinte:

- Uma subscrição do Azure
- Um ficheiros mais simples de logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando mais simples de arquivos da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-flatter-files-from-the-gallery"></a>Adicionando mais simples de arquivos da Galeria
Para configurar a integração de arquivos mais simples para o Azure AD, terá de adicionar mais simples de arquivos a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar ficheiros mais simples a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

1. Na caixa de pesquisa, escreva **mais simples de arquivos**.

    ![Criar um utilizador de teste do Azure AD](./media/flatter-files-tutorial/tutorial_flatterfiles_search.png)

1. No painel de resultados, selecione **mais simples de arquivos**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/flatter-files-tutorial/tutorial_flatterfiles_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com arquivos mais simples com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte em arquivos mais simples para um utilizador no Azure AD. Em outras palavras, uma relação entre um utilizador do Azure AD e o utilizador relacionado em arquivos mais simples de ligação tem de ser estabelecida.

Em arquivos mais simples, atribuir o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com mais simples de arquivos, precisa concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste mais simples de arquivos](#creating-a-flatter-files-test-user)**  - para ter um equivalente da Eduarda Almeida em arquivos mais simples que está ligado à representação de utilizador do Azure AD.
1. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único na sua aplicação de arquivos mais simples.

**Para configurar o Azure AD início de sessão único com mais simples de arquivos, execute os seguintes passos:**

1. No portal do Azure, sobre o **mais simples de arquivos** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/flatter-files-tutorial/tutorial_flatterfiles_samlbase.png)

1. Sobre o **mais simples de arquivos de domínio e URLs** secção, o utilizador não tem de efetuar outros passos de como a aplicação já está pré-integrada com o Azure.

    ![Configurar o início de sessão único](./media/flatter-files-tutorial/tutorial_flatterfiles_url.png)
 
1. Sobre o **certificado de assinatura SAML** secção, clique em **Certificate(Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![Configurar o início de sessão único](./media/flatter-files-tutorial/tutorial_flatterfiles_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/flatter-files-tutorial/tutorial_general_400.png)

1. Sobre o **mais simples de arquivos de configuração** secção, clique em **configurar ficheiros mais simples** para abrir **configurar início de sessão** janela. Cópia a **SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configurar o início de sessão único](./media/flatter-files-tutorial/tutorial_flatterfiles_configure.png) 

1. Início de sessão na sua aplicação de arquivos mais simples, como administrador.

1. Clique em **DASHBOARD**. 
   
    ![Configurar o início de sessão único](./media/flatter-files-tutorial/tutorial_flatter_files_05.png)  

1. Clique em **configurações**e, em seguida, execute os seguintes passos no **empresa** separador: 
   
    ![Configurar o início de sessão único](./media/flatter-files-tutorial/tutorial_flatter_files_06.png)  
    
    a. Selecione **utilizar SAML 2.0 para autenticação**.
    
    b. Clique em **configurar SAML**.

1. Sobre o **configuração SAML do** caixa de diálogo, execute os seguintes passos: 
   
    ![Configurar o início de sessão único](./media/flatter-files-tutorial/tutorial_flatter_files_08.png)  
   
    a. Na **domínio** caixa de texto, escreva o seu domínio registado.
   
    >[!NOTE]
    >Se não tiver um domínio registado ainda, contacte os ficheiros mais simples equipa através de suporte [ support@flatterfiles.com ](mailto:support@flatterfiles.com). 
    
    b. Na **URL de fornecedor de identidade** caixa de texto, cole o valor de **SAML único início de sessão no URL do serviço** que copiou formam o portal do Azure.
   
    c.  Abra o certificado com codificação base 64 no bloco de notas, copie o conteúdo do mesmo para a área de transferência e, em seguida, cole-os para o **certificado do fornecedor de identidade** caixa de texto.

    d. Clique em **Atualizar**.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [Documentação do Azure AD incorporado]( https://go.microsoft.com/fwlink/?linkid=845985)


### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/flatter-files-tutorial/create_aaduser_01.png) 

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/flatter-files-tutorial/create_aaduser_02.png) 

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/flatter-files-tutorial/create_aaduser_03.png) 

1. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/flatter-files-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-a-flatter-files-test-user"></a>Criar um utilizador de teste mais simples de arquivos

O objetivo desta secção é criar um utilizador chamado Eduarda Almeida em arquivos mais simples.

**Para criar um usuário chamado Eduarda Almeida no mais simples de arquivos, execute os seguintes passos:**

1. Inicie sessão no seu **mais simples de arquivos** site da empresa como administrador.

1. No painel de navegação à esquerda, clique em **configurações**e, em seguida, clique nas **utilizadores** separador.
   
    ![Criar um utilizador mais simples de arquivos](./media/flatter-files-tutorial/tutorial_flatter_files_09.png)

1. Clique em **adicionar utilizador**. 

1. Sobre o **adicionar utilizador** caixa de diálogo, execute os seguintes passos:
   
    ![Criar um utilizador mais simples de arquivos](./media/flatter-files-tutorial/tutorial_flatter_files_10.png)

    a. Na **nome próprio** caixa de texto, tipo **Eduarda**.
   
    b. Na **sobrenome** caixa de texto, tipo **Simon**. 
   
    c. Na **endereço de E-Mail** caixa de texto, escreva o endereço de e-mail da Eduarda no portal do Azure.
   
    d. Clique em **Submit** (Submeter).   


### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único, concedendo acesso aos ficheiros mais simples.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida a mais simples de arquivos, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **mais simples de arquivos**.

    ![Configurar o início de sessão único](./media/flatter-files-tutorial/tutorial_flatterfiles_app.png) 

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico de arquivos mais simples no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação de arquivos mais simples.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/flatter-files-tutorial/tutorial_general_01.png
[2]: ./media/flatter-files-tutorial/tutorial_general_02.png
[3]: ./media/flatter-files-tutorial/tutorial_general_03.png
[4]: ./media/flatter-files-tutorial/tutorial_general_04.png

[100]: ./media/flatter-files-tutorial/tutorial_general_100.png

[200]: ./media/flatter-files-tutorial/tutorial_general_200.png
[201]: ./media/flatter-files-tutorial/tutorial_general_201.png
[202]: ./media/flatter-files-tutorial/tutorial_general_202.png
[203]: ./media/flatter-files-tutorial/tutorial_general_203.png

