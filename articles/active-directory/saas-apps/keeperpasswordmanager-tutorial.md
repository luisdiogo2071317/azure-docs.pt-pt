---
title: 'Tutorial: Integração do Azure Active Directory com o Gestor de palavra-passe de extinção do responsável & cofre Digital | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Gestor de palavra-passe de extinção do responsável & Digital de cofre.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: e1a98f6a-2dae-4734-bdbf-4fba742a61d2
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: fb57ab0f746fcad702c18032b9da1a9ec820b809
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39052422"
---
# <a name="tutorial-azure-active-directory-integration-with-keeper-password-manager--digital-vault"></a>Tutorial: Integração do Azure Active Directory com o Gestor de palavra-passe de extinção do responsável & Digital de cofre

Neste tutorial, saiba como integrar o Gestor de palavra-passe de extinção do responsável & Digital cofre com o Azure Active Directory (Azure AD).

Integrar o Gestor de palavra-passe de extinção do responsável e cofre Digital no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Gestor de palavra-passe de extinção do responsável & Digital de cofre
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para o Gestor de palavra-passe de extinção do responsável & cofre Digital (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o Gestor de palavra-passe de extinção do responsável & cofre Digital, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Gestor de palavra-passe de extinção do responsável e cofre Digital início de sessão único na subscrição ativado

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionar Gestor de palavra-passe de extinção do responsável & Digital cofre a partir da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-keeper-password-manager--digital-vault-from-the-gallery"></a>Adicionar Gestor de palavra-passe de extinção do responsável & Digital cofre a partir da Galeria
Para configurar a integração do Gestor de palavra-passe de extinção do responsável & cofre Digital no Azure AD, terá de adicionar Gestor de palavra-passe de extinção do responsável & Digital cofre a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Gestor de palavra-passe de extinção do responsável & Digital cofre a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

4. Na caixa de pesquisa, escreva **Gestor de palavra-passe de extinção do responsável e cofre Digital**.

    ![Criar um utilizador de teste do Azure AD](./media/keeperpasswordmanager-tutorial/tutorial_keeper_search.png)

5. No painel de resultados, selecione **Gestor de palavra-passe de extinção do responsável e cofre Digital**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/keeperpasswordmanager-tutorial/tutorial_keeper_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configurar e testar o Azure AD início de sessão único com o Gestor de palavra-passe de extinção do responsável & Digital cofre com base num utilizador de teste chamado "Eduarda Almeida."

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Gestor de palavra-passe de extinção do responsável e cofre Digital para um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Gestor de palavra-passe de extinção do responsável & Digital cofre tem de ser estabelecida.

No Gestor de palavra-passe de extinção do responsável & cofre Digital, atribuir o valor do **nome de utilizador** no Azure AD como o valor da **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com o Gestor de palavra-passe de extinção do responsável & Digital de cofre, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste do Gestor de palavra-passe de extinção do responsável e cofre Digital](#creating-a-keeperpasswordmanager-test-user)**  - para ter um equivalente da Eduarda Almeida no Gestor de palavra-passe de extinção do responsável & Digital cofre que está ligada à representação de utilizador do Azure AD.
4. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão único no portal do Azure e configurar o início de sessão único na sua aplicação do Gestor de palavra-passe de extinção do responsável & Digital de cofre.

**Para configurar o Azure AD início de sessão único com o Gestor de palavra-passe de extinção do responsável & Digital de cofre, execute os seguintes passos:**

1. No portal do Azure, sobre o **Gestor de palavra-passe de extinção do responsável e cofre Digital** página de integração de aplicativo, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/keeperpasswordmanager-tutorial/tutorial_keeper_samlbase.png)

3. Sobre o **Gestor de palavra-passe de extinção do responsável & Digital de Cofre de domínio e URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/keeperpasswordmanager-tutorial/tutorial_keeper_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://{SSO CONNECT SERVER}/sso-connect/saml/login`

    b. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://{SSO CONNECT SERVER}/sso-connect/saml/sso`

    c. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://{SSO CONNECT SERVER}/sso-connect`

    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o URL de resposta real e o URL de início de sessão. Contacte [equipa de suporte do Gestor de palavra-passe de extinção do responsável & cliente do cofre Digital](https://keepersecurity.com/contact.html) obter esses valores. 

4. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![Configurar o início de sessão único](./media/keeperpasswordmanager-tutorial/tutorial_keeper_certificate.png) 

5. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/keeperpasswordmanager-tutorial/tutorial_general_400.png)
    
6. Sobre o **Gestor de palavra-passe de extinção do responsável e configuração do cofre Digital** secção, clique em **configurar Gestor de palavra-passe de extinção do responsável e cofre Digital** para abrir **configurar início de sessão** janela. Cópia a **URL de fim de sessão, o ID de entidade de SAML e o SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configurar o início de sessão único](./media/keeperpasswordmanager-tutorial/tutorial_keeper_configure.png) 

7. Para configurar o início de sessão único num **Gestor de palavra-passe de extinção do responsável e configuração do cofre Digital** lado, siga as diretrizes fornecidas durante [guia de suporte do guardião](https://keepersecurity.com/assets/pdf/KeeperSSOConnect_v11.pdf).

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/keeperpasswordmanager-tutorial/create_aaduser_01.png) 

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/keeperpasswordmanager-tutorial/create_aaduser_02.png) 

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/keeperpasswordmanager-tutorial/create_aaduser_03.png) 

4. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/keeperpasswordmanager-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-a-keeper-password-manager--digital-vault-test-user"></a>Criar um utilizador de teste do Gestor de palavra-passe de extinção do responsável & Digital de cofre

Para ativar a utilizadores do Azure AD iniciar sessão no Gestor de palavra-passe de extinção do responsável & Digital de cofre, tem de ser aprovisionados no Gestor de palavra-passe de extinção do responsável & Digital de cofre. Aplicativo oferece suporte apenas no tempo de aprovisionamento de utilizador e de utilizadores de autenticação serão criados no aplicativo automaticamente. Pode contactar [suporte do guardião](https://keepersecurity.com/contact.html), se desejar configurar manualmente os utilizadores.

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para o Gestor de palavra-passe de extinção do responsável & Digital de cofre.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida para o Gestor de palavra-passe de extinção do responsável & Digital de cofre, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **Gestor de palavra-passe de extinção do responsável e cofre Digital**.

    ![Configurar o início de sessão único](./media/keeperpasswordmanager-tutorial/tutorial_keeper_app.png) 

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico do Gestor de palavra-passe de extinção do responsável e cofre Digital no painel de acesso, deve obter a página de início de sessão da aplicação de Gestor de palavra-passe de extinção do responsável & Digital de cofre. Após a autenticação com êxito, deverá obter no aplicativo. Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/keeperpasswordmanager-tutorial/tutorial_general_01.png
[2]: ./media/keeperpasswordmanager-tutorial/tutorial_general_02.png
[3]: ./media/keeperpasswordmanager-tutorial/tutorial_general_03.png
[4]: ./media/keeperpasswordmanager-tutorial/tutorial_general_04.png

[100]: ./media/keeperpasswordmanager-tutorial/tutorial_general_100.png

[200]: ./media/keeperpasswordmanager-tutorial/tutorial_general_200.png
[201]: ./media/keeperpasswordmanager-tutorial/tutorial_general_201.png
[202]: ./media/keeperpasswordmanager-tutorial/tutorial_general_202.png
[203]: ./media/keeperpasswordmanager-tutorial/tutorial_general_203.png

