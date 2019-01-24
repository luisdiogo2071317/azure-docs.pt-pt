---
title: 'Tutorial: Integração do Active Directory do Azure com o Zscaler privada acesso (ZPA) | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e o acesso privado Zscaler (ZPA).
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 83711115-1c4f-4dd7-907b-3da24b37c89e
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/06/2017
ms.author: jeedes
ms.openlocfilehash: 5610316fdadb9a169c90e7e7761d9ddd2a74dcbe
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/23/2019
ms.locfileid: "54819123"
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-private-access-zpa"></a>Tutorial: Integração do Active Directory do Azure com o Zscaler privada acesso (ZPA)

Neste tutorial, saiba como integrar o acesso privado Zscaler (ZPA) com o Azure Active Directory (Azure AD).

Integrar o acesso privado Zscaler (ZPA) no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso para Zscaler privada acesso (ZPA)
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Zscaler privada acesso (ZPA) (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – o portal de gestão do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o Zscaler privada acesso (ZPA), terá dos seguintes itens:

- Uma subscrição do Azure
- Um acesso privado Zscaler (ZPA) início de sessão único na subscrição ativado


> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.


Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não deve utilizar o seu ambiente de produção, a menos que isso é necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando o acesso privado Zscaler (ZPA) da Galeria
1. Configuração e teste do Azure AD início de sessão único


## <a name="adding-zscaler-private-access-zpa-from-the-gallery"></a>Adicionando o acesso privado Zscaler (ZPA) da Galeria
Para configurar a integração do Zscaler privada acesso (ZPA) para o Azure AD, terá de adicionar acesso privado Zscaler (ZPA) a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar acesso privado Zscaler (ZPA) a partir da galeria, execute os seguintes passos:**

1. Na  **[do Portal de gestão do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
1. Clique em **adicionar** botão na parte superior da caixa de diálogo.

    ![Aplicações][3]

1. Na caixa de pesquisa, escreva **Zscaler privada acesso (ZPA)**.

    ![Criar um utilizador de teste do Azure AD](./media/zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_001.png)

1. No painel de resultados, selecione **Zscaler privada acesso (ZPA)** e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_0001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com o Zscaler privada acesso (ZPA) com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Zscaler privada acesso (ZPA) a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Zscaler privada acesso (ZPA) deve ser estabelecido.

Esta relação de ligação é estabelecida atribuindo o valor do **nome de utilizador** no Azure AD como o valor da **Username** no Zscaler privada acesso (ZPA).

Para configurar e testar o Azure AD início de sessão único com o Zscaler privada acesso (ZPA), tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste de acesso privado Zscaler (ZPA)](#creating-a-zscaler-private-access-(zpa)-test-user)**  - para ter um equivalente da Eduarda Almeida na Zscaler privada acesso (ZPA) que está ligado à representação de ela do Azure AD.
1. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal de gestão do Azure e configurar início de sessão único na sua aplicação de acesso privado Zscaler (ZPA).

**Para configurar o Azure AD início de sessão único com o Zscaler privada acesso (ZPA), execute os seguintes passos:**

1. No portal de gestão do Azure, sobre o **Zscaler privada acesso (ZPA)** página de integração de aplicativo, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

1. Sobre o **início de sessão único** caixa de diálogo, como **modo** selecione **baseado em SAML início de sessão** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/zscalerprivateaccess-tutorial/tutorial_general_300.png)
    
1. Sobre o **Zscaler privada acesso (ZPA) de domínio e URLs** secção, execute os seguintes passos:
    
    ![Configurar o início de sessão único](./media/zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_01.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://samlsp.private.zscaler.com/auth/login?domain=<your-domain-name>`

    b. Na **identificador** caixa de texto, tipo: `https://samlsp.private.zscaler.com/auth/metadata`

    > [!NOTE] 
    > Tenha em atenção que estes não são os valores reais. Terá de atualizar estes valores com o URL de início de sessão e o identificador real. Aqui iremos sugerir-lhe utilizar o valor exclusivo do URL no identificador de. Contacte [equipa de suporte de acesso privado Zscaler (ZPA)](https://help.zscaler.com/zpa-submit-ticket) obter esses valores.

1. Sobre o **certificado de assinatura SAML** secção, clique em **criar novo certificado**.

    ![Configurar o início de sessão único](./media/zscalerprivateaccess-tutorial/tutorial_general_400.png)     

1. Sobre o **criar novo certificado** caixa de diálogo, clique no ícone de calendário e selecione um **data de expiração**. Em seguida, clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/zscalerprivateaccess-tutorial/tutorial_general_500.png)

1. Sobre o **certificado de assinatura SAML** secção, selecione **ativar o novo certificado** e clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_02.png)

1. No pop-up **certificado de Rollover** janela, clique em **OK**.

    ![Configurar o início de sessão único](./media/zscalerprivateaccess-tutorial/tutorial_general_600.png)

1. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![Configurar o início de sessão único](./media/zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_03.png) 

1. Numa janela do browser web diferente, inicie sessão no site da sua empresa Zscaler acesso (ZPA privada) como um administrador.

1. Navegue para **administrador** e, em seguida, clique em **configuração do Idp**.

    ![Configurar o início de sessão único no lado de aplicação](./media/zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_04.png)

1. Na **configuração do Idp** secção, clique em **adicione a nova configuração do IDP**.

    ![Configurar o início de sessão único no lado de aplicação](./media/zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_05.png)

1. Na **configuração do IDP novo** secção, execute os seguintes passos:

    ![Configurar o início de sessão único no lado de aplicação](./media/zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_06.png)

    a. Clique em **selecionar ficheiro** e carregar o ficheiro de metadados baixado.

    b. Clique em **guardar** botão.
    


### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção consiste em criar um utilizador de teste no portal do Azure Management chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal de gestão do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/zscalerprivateaccess-tutorial/create_aaduser_01.png) 

1. Aceda a **utilizadores e grupos** e clique em **todos os utilizadores** para apresentar a lista de utilizadores.
    
    ![Criar um utilizador de teste do Azure AD](./media/zscalerprivateaccess-tutorial/create_aaduser_02.png) 

1. Na parte superior da caixa de diálogo, clique em **Add** para abrir o **utilizador** caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/zscalerprivateaccess-tutorial/create_aaduser_03.png) 

1. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/zscalerprivateaccess-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**. 



### <a name="creating-a-zscaler-private-access-zpa-test-user"></a>Criar um utilizador de teste de acesso privado Zscaler (ZPA)

Nesta secção, vai criar um usuário chamado Eduarda Almeida na Zscaler privada acesso (ZPA). Trabalhe em conjunto com [equipa de suporte de acesso privado Zscaler (ZPA)](https://help.zscaler.com/zpa-submit-ticket) para adicionar os utilizadores na plataforma do Zscaler privada acesso (ZPA).


### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único, concedendo acesso para Zscaler privada acesso (ZPA).

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida para Zscaler privada acesso (ZPA), execute os seguintes passos:**

1. No portal de gestão do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **Zscaler privada acesso (ZPA)**.

    ![Configurar o início de sessão único](./media/zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_50.png) 

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    


### <a name="testing-single-sign-on"></a>Teste de início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico de acesso privado Zscaler (ZPA) no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação de acesso privado Zscaler (ZPA).


## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/zscalerprivateaccess-tutorial/tutorial_general_01.png
[2]: ./media/zscalerprivateaccess-tutorial/tutorial_general_02.png
[3]: ./media/zscalerprivateaccess-tutorial/tutorial_general_03.png
[4]: ./media/zscalerprivateaccess-tutorial/tutorial_general_04.png

[100]: ./media/zscalerprivateaccess-tutorial/tutorial_general_100.png

[200]: ./media/zscalerprivateaccess-tutorial/tutorial_general_200.png
[201]: ./media/zscalerprivateaccess-tutorial/tutorial_general_201.png
[202]: ./media/zscalerprivateaccess-tutorial/tutorial_general_202.png
[203]: ./media/zscalerprivateaccess-tutorial/tutorial_general_203.png