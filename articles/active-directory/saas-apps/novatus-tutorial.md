---
title: 'Tutorial: Integração do Active Directory do Azure com Novatus | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Novatus.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: d2f13779-bdb7-4408-9738-be67ed3de4e5
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/02/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1b58e1d2fc13543c80899c61c8532ab8a93a3b63
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2019
ms.locfileid: "56207165"
---
# <a name="tutorial-azure-active-directory-integration-with-novatus"></a>Tutorial: Integração do Active Directory do Azure com Novatus

Neste tutorial, saiba como integrar Novatus com o Azure Active Directory (Azure AD).

Integrar Novatus no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Novatus
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Novatus (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Novatus, terá dos seguintes itens:

- Uma subscrição do Azure
- Um Novatus logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Novatus da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-novatus-from-the-gallery"></a>Adicionando Novatus da Galeria
Para configurar a integração do Novatus com o Azure AD, terá de adicionar Novatus a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Novatus a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

1. Na caixa de pesquisa, escreva **Novatus**.

    ![Criar um utilizador de teste do Azure AD](./media/novatus-tutorial/tutorial_novatus_search.png)

1. No painel de resultados, selecione **Novatus**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/novatus-tutorial/tutorial_novatus_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com Novatus com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Novatus a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Novatus deve ser estabelecido.

Novatus, atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com Novatus, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste Novatus](#creating-a-novatus-test-user)**  - para ter um equivalente da Eduarda Almeida na Novatus que está ligado à representação de utilizador do Azure AD.
1. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo Novatus.

**Para configurar o Azure AD início de sessão único com Novatus, execute os seguintes passos:**

1. No portal do Azure, sobre o **Novatus** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/novatus-tutorial/tutorial_novatus_samlbase.png)

1. Sobre o **Novatus domínio e URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/novatus-tutorial/tutorial_novatus_url.png)

     Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://sso.novatuscontracts.com/<companyname>`

    > [!NOTE] 
    > Este valor não é real. Atualize este valor com o URL de início de sessão real. Contacte [equipa de suporte de cliente Novatus](mailto:jvinci@novatusinc.com) para obter este valor. 
 


1. Sobre o **certificado de assinatura SAML** secção, clique em **Certificate(Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![Configurar o início de sessão único](./media/novatus-tutorial/tutorial_novatus_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/novatus-tutorial/tutorial_general_400.png)

1. Sobre o **Novatus configuração** secção, clique em **configurar Novatus** para abrir **configurar início de sessão** janela. Cópia a **URL de fim de sessão, o ID de entidade de SAML e o SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configurar o início de sessão único](./media/novatus-tutorial/tutorial_novatus_configure.png) 

1. Para obter SSO configurado para a sua aplicação, contacte o seu [equipa de suporte de Novatus](mailto:jvinci@novatusinc.com). Anexar o **transferido o certificado** ficheiro para o correio e partilha os **urls de metadados** (**URL de fim de sessão, o ID de entidade de SAML e o SAML único início de sessão no URL do serviço**) com Novatus equipe para configurar o SSO no seu lado.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [Documentação do Azure AD incorporado]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/novatus-tutorial/create_aaduser_01.png) 

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/novatus-tutorial/create_aaduser_02.png) 

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/novatus-tutorial/create_aaduser_03.png) 

1. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/novatus-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-a-novatus-test-user"></a>Criar um utilizador de teste Novatus

O objetivo desta secção é criar um usuário chamado Eduarda Almeida no Novatus. Novatus suporta o aprovisionamento de just-in-time, que está por predefinição, ativada.

Não existe nenhum item de ação para nesta secção. Durante uma tentativa de aceder Novatus se não existir ainda será criado um novo utilizador.

>[!NOTE]
>Se precisar de criar manualmente um utilizador, terá de contactar o [equipa de suporte de Novatus](mailto:jvinci@novatusinc.com). 
> 

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para Novatus.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida a Novatus, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **Novatus**.

    ![Configurar o início de sessão único](./media/novatus-tutorial/tutorial_novatus_app.png) 

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

O objetivo desta secção é testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico Novatus no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo Novatus. Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/novatus-tutorial/tutorial_general_01.png
[2]: ./media/novatus-tutorial/tutorial_general_02.png
[3]: ./media/novatus-tutorial/tutorial_general_03.png
[4]: ./media/novatus-tutorial/tutorial_general_04.png

[100]: ./media/novatus-tutorial/tutorial_general_100.png

[200]: ./media/novatus-tutorial/tutorial_general_200.png
[201]: ./media/novatus-tutorial/tutorial_general_201.png
[202]: ./media/novatus-tutorial/tutorial_general_202.png
[203]: ./media/novatus-tutorial/tutorial_general_203.png

