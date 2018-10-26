---
title: 'Tutorial: Integração do Azure Active Directory com o Zscaler três | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e o Zscaler três.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: f352e00d-68d3-4a77-bb92-717d055da56f
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/25/2018
ms.author: jeedes
ms.openlocfilehash: b148967af0882993d8ab113bdf0fd3ad3835296f
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/25/2018
ms.locfileid: "50092615"
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-three"></a>Tutorial: Integração do Azure Active Directory com o Zscaler três

Neste tutorial, saiba como integrar o Zscaler três com o Azure Active Directory (Azure AD).

Integrar o Zscaler três no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Zscaler três
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Zscaler três (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o Zscaler três, terá dos seguintes itens:

- Uma subscrição do Azure
- Um Zscaler três logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma avaliação de um mês aqui: [oferta de avaliação](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste.
O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Zscaler três da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-zscaler-three-from-the-gallery"></a>Adicionando Zscaler três da Galeria

Para configurar a integração do Zscaler três para o Azure AD, terá de adicionar Zscaler três na Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Zscaler três a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Active Directory][1]

2. Navegue para **aplicações empresariais** e, em seguida, selecione a **todos os aplicativos** opção.

    ![Aplicações][2]

3. Clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

4. No painel de resultados, selecione **Zscaler três**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/zscaler-three-tutorial/tutorial_zscalerthree_addfromgallery.png)

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com o Zscaler três com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Zscaler três para um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Zscaler três deve ser estabelecido.

Para configurar e testar o Azure AD início de sessão único com o Zscaler três, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Configurar definições de proxy](#configuring-proxy-settings)**  - para configurar as definições de proxy no Internet Explorer
3. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
4. **[Criar um utilizador de teste Zscaler três](#creating-a-zscaler-three-test-user)**  - para ter um equivalente da Eduarda Almeida na Zscaler três que está ligado à representação de utilizador do Azure AD.
5. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
6. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo Zscaler três.

**Para configurar o Azure AD início de sessão único com o Zscaler três, execute os seguintes passos:**

1. No portal do Azure, sobre o **Zscaler três** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

2. Sobre o **selecionar um método de início de sessão único** caixa de diálogo, selecione **SAML/WS-Fed** modo para ativar o início de sessão único.

    ![Configurar o início de sessão único](./media/zscaler-three-tutorial/tutorial_general_301.png)

3. Se precisar de alterar para **SAML** modo de qualquer outro modo, clique em **alterar único início de sessão em modo** na parte superior do ecrã.

    ![Configurar o início de sessão único](./media/zscaler-three-tutorial/tutorial_general_300.png)

4. Sobre o **definir a segurança de início de sessão único com o SAML** página, clique em **editar** ícone para abrir **configuração básica de SAML** caixa de diálogo.

    ![Configurar o início de sessão único](./media/zscaler-three-tutorial/tutorial_general_302.png)

5. Sobre o **configuração básica de SAML** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/zscaler-three-tutorial/tutorial_zscalerthree_url.png)

    No início de sessão na caixa de texto do URL, introduza o URL: `https://login.zscalerthree.net/sfc_sso`

6. Sobre o **certificado de assinatura SAML** secção, clique em **transferir** para transferir **Certificate(Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![Configurar o início de sessão único](./media/zscaler-three-tutorial/tutorial_zscalerthree_certificate.png)

8. Sobre o **configurar Zscaler três** secção, copie a **URL de início de sessão**.

    ![Configurar o início de sessão único](./media/zscaler-three-tutorial/tutorial_zscalerthree_configure.png)

9. Numa janela do browser web diferente, inicie sessão no site da sua empresa Zscaler três como administrador.

10. No menu na parte superior, clique em **administração**.

    ![Administração](./media/zscaler-three-tutorial/ic800206.png "administração")

9. Sob **gerir administradores e funções**, clique em **autenticação do & Gerir utilizadores**.

    ![Gerir utilizadores e de autenticação](./media/zscaler-three-tutorial/ic800207.png "gerir utilizadores e de autenticação")

10. Na **escolher opções de autenticação para a sua organização** secção, execute os seguintes passos:

    ![Autenticação](./media/zscaler-three-tutorial/ic800208.png "autenticação")

    a. Selecione **autenticar com o SAML Single Sign-On**.

    b. Clique em **configurar SAML únicos início de sessão em parâmetros**.

11. Sobre o **configurar SAML único início de sessão em parâmetros** página de diálogo, execute os seguintes passos e, em seguida, clique em **feito**

    ![Início de sessão único](./media/zscaler-three-tutorial/ic800209.png "início de sessão único")

    a. Colar o **URL de início de sessão** valor, que copiou do portal do Azure para o **URL do Portal de SAML para o qual os utilizadores são enviados para a autenticação** caixa de texto.

    b. Na **atributo que contém o nome de início de sessão** caixa de texto, tipo **NameID**.

    c. Para carregar o certificado transferido, clique em **Zscaler pem**.

    d. Selecione **ativar o aprovisionamento automático do SAML**.

12. Sobre o **configurar a autenticação de utilizador** caixa de diálogo página, execute os seguintes passos:

    ![Administração](./media/zscaler-three-tutorial/ic800210.png "administração")

    a. Clique em **Guardar**.

    b. Clique em **ative agora**.

## <a name="configuring-proxy-settings"></a>Configurar definições de proxy

### <a name="to-configure-the-proxy-settings-in-internet-explorer"></a>Para configurar as definições de proxy no Internet Explorer

1. Inicie **do Internet Explorer**.

2. Selecione **opções da Internet** partir a **ferramentas** menu para abrir o **opções da Internet** caixa de diálogo.

     ![Opções da Internet](./media/zscaler-three-tutorial/ic769492.png "opções da Internet")

3. Clique nas **ligações** separador.
  
     ![Ligações](./media/zscaler-three-tutorial/ic769493.png "ligações")

4. Clique em **definições de LAN** para abrir o **definições de LAN** caixa de diálogo.

5. Na secção de servidor Proxy, execute os seguintes passos:

    ![Servidor proxy](./media/zscaler-three-tutorial/ic769494.png "servidor Proxy")

    a. Selecione **utilizar um servidor proxy para a rede local**.

    b. Na caixa de texto endereço, escreva **gateway.zscalerthree.net**.

    c. Na caixa de texto de porta, escreva **80**.

    d. Selecione **Ignorar servidor proxy para endereços locais**.

    e. Clique em **OK** para fechar a **definições de rede Local (LAN)** caixa de diálogo.

6. Clique em **OK** para fechar a **opções da Internet** caixa de diálogo.

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

1. No portal do Azure, no painel esquerdo, selecione **do Azure Active Directory**, selecione **utilizadores**e, em seguida, selecione **todos os utilizadores**.

    ![Criar utilizador do Azure AD][100]

2. Selecione **novo utilizador** na parte superior do ecrã.

    ![Criar um utilizador de teste do Azure AD](./media/zscaler-three-tutorial/create_aaduser_01.png) 

3. Nas propriedades do utilizador, execute os seguintes passos.

    ![Criar um utilizador de teste do Azure AD](./media/zscaler-three-tutorial/create_aaduser_02.png)

    a. Na **Name** campo introduza **BrittaSimon**.
  
    b. Na **nome de utilizador** tipo de campo **brittasimon@yourcompanydomain.extension**  
    Por exemplo, BrittaSimon@contoso.com

    c. Selecione **propriedades**, selecione a **palavra-passe de Show** caixa de verificação e, em seguida, anote o valor que é apresentado na caixa de palavra-passe.

    d. Clique em **Criar**.

### <a name="creating-a-zscaler-three-test-user"></a>Criar um utilizador de teste Zscaler três

Para ativar a utilizadores do Azure AD iniciar sessão no Zscaler três, tem de ser aprovisionados para Zscaler três. No caso do Zscaler três, o aprovisionamento é uma tarefa manual.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Para configurar o aprovisionamento de utilizadores, execute os seguintes passos:

1. Inicie sessão no seu **Zscaler três** inquilino.

2. Clique em **administração**.

    ![Administração](./media/zscaler-three-tutorial/ic781035.png "administração")

3. Clique em **gestão de utilizadores**.

     ![Adicione](./media/zscaler-three-tutorial/ic781036.png "adicionar")

4. Na **usuários** separador, clique em **Add**.

    ![Adicione](./media/zscaler-three-tutorial/ic781037.png "adicionar")

5. Na secção Adicionar utilizador, execute os seguintes passos:

    ![Adicionar utilizador](./media/zscaler-three-tutorial/ic781038.png "adicionar utilizador")

    a. Tipo de **UserID**, **nome a apresentar do utilizador**, **palavra-passe**, **Confirmar palavra-passe**e, em seguida, selecione **grupos**e o **departamento** de um Azure válido conta AD que pretende aprovisionar.

    b. Clique em **Guardar**.

> [!NOTE]
> Pode utilizar qualquer outro Zscaler três ferramentas de criação de conta de utilizador ou APIs fornecidas pelo Zscaler três para aprovisionar contas de utilizador do Azure AD.

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para Zscaler três.

1. No portal do Azure, selecione **aplicações empresariais**, selecione **todos os aplicativos**.

    ![Atribuir utilizador][201]

2. Na lista de aplicações, selecione **Zscaler três**.

    ![Configurar o início de sessão único](./media/zscaler-three-tutorial/tutorial_zscalerthree_app.png)

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202]

4. Clique em **adicionar utilizador** e, em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

5. Na **utilizadores e grupos** caixa de diálogo select **Eduarda Almeida** na lista de utilizadores, em seguida, clique o **selecionar** na parte inferior do ecrã.

6. Na **adicionar atribuição** clique da caixa de diálogo a **atribuir** botão.

### <a name="testing-single-sign-on"></a>Teste de início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico Zscaler três no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo Zscaler três.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[1]: ./media/zscaler-three-tutorial/tutorial_general_01.png
[2]: ./media/zscaler-three-tutorial/tutorial_general_02.png
[3]: ./media/zscaler-three-tutorial/tutorial_general_03.png
[4]: ./media/zscaler-three-tutorial/tutorial_general_04.png

[100]: ./media/zscaler-three-tutorial/tutorial_general_100.png

[200]: ./media/zscaler-three-tutorial/tutorial_general_200.png
[201]: ./media/zscaler-three-tutorial/tutorial_general_201.png
[202]: ./media/zscaler-three-tutorial/tutorial_general_202.png
[203]: ./media/zscaler-three-tutorial/tutorial_general_203.png