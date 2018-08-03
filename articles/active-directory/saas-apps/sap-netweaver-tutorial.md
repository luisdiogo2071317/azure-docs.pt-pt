---
title: 'Tutorial: Integração do Azure Active Directory com SAP NetWeaver | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o SAP NetWeaver e o Azure Active Directory.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 1b9e59e3-e7ae-4e74-b16c-8c1a7ccfdef3
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: jeedes
ms.openlocfilehash: b773380a21e0c47a1e1519e592aa0ddd5e6388fa
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39421955"
---
# <a name="tutorial-azure-active-directory-integration-with-sap-netweaver"></a>Tutorial: Integração do Azure Active Directory com SAP NetWeaver

Neste tutorial, saiba como integrar o SAP NetWeaver no Azure Active Directory (Azure AD).

Integrar o SAP NetWeaver no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao SAP NetWeaver
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para o SAP NetWeaver (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).


## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o SAP NetWeaver, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um SAP NetWeaver início de sessão único na subscrição ativado

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando o SAP NetWeaver da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-sap-netweaver-from-the-gallery"></a>Adicionando o SAP NetWeaver da Galeria
Para configurar a integração do SAP NetWeaver no Azure AD, terá de adicionar o SAP NetWeaver partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar o SAP NetWeaver partir da galeria, execute os seguintes passos:**

1. Na  **[Portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

1. Na caixa de pesquisa, escreva **SAP NetWeaver**.

    ![Criar um utilizador de teste do Azure AD](./media/sap-netweaver-tutorial/tutorial_sapnetweaver_search.png)

1. No painel de resultados, selecione **SAP NetWeaver**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/sap-netweaver-tutorial/tutorial_sapnetweaver_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com o SAP NetWeaver, com base num utilizador de teste chamado "Eduarda Almeida."

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no SAP NetWeaver a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado em SAP NetWeaver deve ser estabelecido.

Esta relação de ligação é estabelecida atribuindo o valor do **nome de utilizador** no Azure AD como o valor da **Username** em SAP NetWeaver.

Para configurar e testar o Azure AD início de sessão único com o SAP NetWeaver, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste de SAP NetWeaver](#creating-an-sap-netweaver-test-user)**  - para ter um equivalente da Eduarda Almeida no SAP NetWeaver, que está ligado à representação de utilizador do Azure AD.
1. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único na sua aplicação SAP NetWeaver.

**Para configurar o Azure AD início de sessão único com o SAP NetWeaver, execute os seguintes passos:**

1. No portal do Azure, sobre o **SAP NetWeaver** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/sap-netweaver-tutorial/tutorial_sapnetweaver_samlbase.png)

1. Sobre o **SAP NetWeaver domínio e URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/sap-netweaver-tutorial/tutorial_sapnetweaver_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<your company instance of SAP NetWeaver>`

    b. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://<your company instance of SAP NetWeaver>`

    c. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://<your company instance of SAP NetWeaver>/sap/saml2/sp/acs/100`
     
    > [!NOTE] 
    > Estes valores não são o real. Atualize estes valores com o identificador real e o URL de resposta e URL de início de sessão. Aqui iremos sugerir-lhe utilizar o valor único de cadeia de caracteres no identificador de. Contacte [equipa de suporte do SAP NetWeaver cliente](https://www.sap.com/support.html) obter esses valores. 

1. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro XML em seu computador.

    ![Configurar o início de sessão único](./media/sap-netweaver-tutorial/tutorial_sapnetweaver_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/sap-netweaver-tutorial/tutorial_general_400.png)
    
1. Na **configuração do SAP NetWeaver** secção, clique em **configurar o SAP NetWeaver** para abrir **configurar início de sessão** janela. Cópia a **ID de entidade de SAML** partir o **secção de referência rápida.**

    ![Configurar o início de sessão único](./media/sap-netweaver-tutorial/tutorial_sapnetweaver_configure.png) 

1. Para configurar o início de sessão único num **SAP NetWeaver** lado, terá de enviar o transferido **XML de metadados** e **ID de entidade de SAML** para [suporte do SAP NetWeaver](https://www.sap.com/support.html). 

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/sap-netweaver-tutorial/create_aaduser_01.png) 

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/sap-netweaver-tutorial/create_aaduser_02.png) 

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/sap-netweaver-tutorial/create_aaduser_03.png) 

1. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/sap-netweaver-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **Eduarda Almeida**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** da Eduarda Almeida.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-an-sap-netweaver-test-user"></a>Criar um utilizador de teste de SAP NetWeaver

Nesta secção, vai criar um usuário chamado Eduarda Almeida no SAP NetWeaver. Trabalhar com sua [suporte do SAP NetWeaver](https://www.sap.com/support.html) para adicionar os utilizadores na plataforma do SAP NetWeaver.

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único, concedendo acesso para SAP NetWeaver.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida para SAP NetWeaver, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **SAP NetWeaver**.

    ![Configurar o início de sessão único](./media/sap-netweaver-tutorial/tutorial_sapnetweaver_app.png) 

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico do SAP NetWeaver no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação SAP NetWeaver.

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[1]: ./media/sap-netweaver-tutorial/tutorial_general_01.png
[2]: ./media/sap-netweaver-tutorial/tutorial_general_02.png
[3]: ./media/sap-netweaver-tutorial/tutorial_general_03.png
[4]: ./media/sap-netweaver-tutorial/tutorial_general_04.png

[100]: ./media/sap-netweaver-tutorial/tutorial_general_100.png

[200]: ./media/sap-netweaver-tutorial/tutorial_general_200.png
[201]: ./media/sap-netweaver-tutorial/tutorial_general_201.png
[202]: ./media/sap-netweaver-tutorial/tutorial_general_202.png
[203]: ./media/sap-netweaver-tutorial/tutorial_general_203.png

