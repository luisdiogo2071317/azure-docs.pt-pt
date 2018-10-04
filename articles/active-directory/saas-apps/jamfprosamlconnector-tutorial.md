---
title: 'Tutorial: Integração do Azure Active Directory com o Jamf Pro | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e o Jamf Pro.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 35e86d08-c29e-49ca-8545-b0ff559c5faf
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/03/2018
ms.author: jeedes
ms.openlocfilehash: d28e28a2c4f8144da16c4838f07c9b8bb5ce67f0
ms.sourcegitcommit: f58fc4748053a50c34a56314cf99ec56f33fd616
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/04/2018
ms.locfileid: "48268162"
---
# <a name="tutorial-azure-active-directory-integration-with-jamf-pro"></a>Tutorial: Integração do Azure Active Directory com o Jamf Pro

Neste tutorial, saiba como integrar o Jamf Pro com o Azure Active Directory (Azure AD).

Integrar o Jamf Pro com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Jamf Pro.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para o Jamf Pro (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o Jamf Pro, terá dos seguintes itens:

- Uma subscrição do Azure
- Um Jamf Pro logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste.
O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando o Jamf Pro da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-jamf-pro-from-the-gallery"></a>Adicionando o Jamf Pro da Galeria

Para configurar a integração do Jamf Pro com o Azure AD, terá de adicionar o Jamf Pro a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar o Jamf Pro a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![image](./media/jamfprosamlconnector-tutorial/selectazuread.png)

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![image](./media/jamfprosamlconnector-tutorial/a_select_app.png)
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![image](./media/jamfprosamlconnector-tutorial/a_new_app.png)

4. Na caixa de pesquisa, escreva **Jamf Pro**, selecione **Jamf Pro** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

     ![image](./media/jamfprosamlconnector-tutorial/a_add_app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com o Jamf Pro com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador equivalente no Jamf Pro para um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Jamf Pro deve ser estabelecido.

Para configurar e testar o Azure AD início de sessão único com o Jamf Pro, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste do Jamf Pro](#create-a-jamf-pro-test-user)**  - para ter um equivalente da Eduarda Almeida no Jamf Pro que está ligado à representação de utilizador do Azure AD.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único na sua aplicação do Jamf Pro.

**Para configurar o Azure AD início de sessão único com o Jamf Pro, execute os seguintes passos:**

1. Na [portal do Azure](https://portal.azure.com/), na **Jamf Pro** página de integração de aplicações, selecione **início de sessão único**.

    ![image](./media/jamfprosamlconnector-tutorial/b1_b2_select_sso.png)

2. Clique em **modo de início de sessão único de alteração** na parte superior do ecrã para selecionar o **SAML** modo.

      ![image](./media/jamfprosamlconnector-tutorial/b1_b2_saml_ssso.png)

3. Sobre o **selecionar um método de início de sessão único** caixa de diálogo, clique em **selecione** para **SAML** modo para ativar o início de sessão único.

    ![image](./media/jamfprosamlconnector-tutorial/b1_b2_saml_sso.png)

4. Sobre o **definido no início de sessão único com o SAML** página, clique em **editar** botão para abrir **configuração básica de SAML** caixa de diálogo.

    ![image](./media/jamfprosamlconnector-tutorial/b1-domains_and_urlsedit.png)

5. Sobre o **configuração básica de SAML** secção, execute os seguintes passos:

    a. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://<subdomain>.jamfcloud.com/saml/metadata`.

    b. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://<subdomain>.jamfcloud.com/saml/SSO`.

    ![image](./media/jamfprosamlconnector-tutorial//b2-domains_and_urls.png)

    c. Clique em **definir URLs adicionais**.

    d. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<subdomain>.jamfcloud.com`.

    ![image](./media/jamfprosamlconnector-tutorial//b4-domains_and_urls.png)

    > [!NOTE]
    > Estes valores não são reais. Atualize estes valores com o identificador de real, a URL de resposta e o URL de início de sessão. Obterá o valor do identificador real da **Single Sign-On** secção no portal do Jamf Pro, que é explicado mais tarde no tutorial. É possível extrair o real **subdomínio** valor do valor de identificador e usá-lo **subdomínio** informações no URL de início de sessão e o URL de resposta.

6. No **definir a segurança de início de sessão único com o SAML** na página a **certificado de assinatura SAML** secção, clique em botão Copiar para copiar **Url de metadados de Federação de aplicação** e guarde-o no seu computador.

    ![image](./media/jamfprosamlconnector-tutorial/C2_certificate.png)

7. Para automatizar a configuração no Jamf Pro, tem de instalar **segura de aplicações meu início de sessão da extensão de browser** ao clicar em **instalar a extensão**.

    ![image](./media/jamfprosamlconnector-tutorial/install_extension.png)
 
8. Depois de adicionar a extensão para o navegador, clique em **configurar o Jamf Pro** irá direcioná-lo para a aplicação do Jamf Pro. A partir daí, forneça as credenciais de administrador para iniciar sessão no Jamf Pro. A extensão do browser irá configurar o aplicativo para e automatizar as etapas 9 a 12 automaticamente.

    ![image](./media/jamfprosamlconnector-tutorial/d1_saml.png)

9. Se desejar configurar manualmente o Jamf Pro, abra uma nova janela de browser e o registo no site da sua empresa Jamf Pro como administrador e execute os seguintes passos:

10. Clique nas **ícone de definições** do canto superior direito da página.

    ![Configuração do Jamf Pro](./media/jamfprosamlconnector-tutorial/configure1.png)

11. Clique em **início de sessão único**.

    ![Configuração do Jamf Pro](./media/jamfprosamlconnector-tutorial/configure2.png)

12. Sobre o **Single Sign-On** página execute os seguintes passos:

    ![O Jamf Pro único](./media/jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_single.png)

    a. Selecione **servidor de Jamf Pro** para ativar o acesso de início de sessão único.

    b. Selecionando **omissão de permissões para todos os utilizadores** os utilizadores não serão redirecionados para a página de início de sessão do fornecedor de identidade para a autenticação, mas pode iniciar sessão no Jamf Pro diretamente em vez disso. Quando um usuário tentar acessar o Jamf Pro através do fornecedor de identidade, autorização e autenticação de SSO iniciado por IdP ocorre.

    c. Selecione o **NameID** opção para **mapeamento de utilizadores: SAML**. Por predefinição, esta definição está definida como **NameID** , mas pode definir um atributo personalizado.

    d. Selecione **E-Mail** para **mapeamento de utilizador: o JAMF PRO**. O Jamf Pro mapeia os atributos SAML enviados pelo IdP das seguintes formas: por utilizadores e grupos. Quando um usuário tenta acessar o Jamf Pro, por predefinição o Jamf Pro obtém informações sobre o utilizador a partir do fornecedor de identidade e compara com algo de contas de utilizador do Jamf Pro. Se a conta de utilizador de entrada não existir no Jamf Pro, em seguida, a correspondência de nomes de grupo ocorre.

    e. Cole o valor `http://schemas.microsoft.com/ws/2008/06/identity/claims/groups` no **o nome de ATRIBUTO de grupo** caixa de texto.

13. Sobre o mesmo desloque-se de página para baixo até **fornecedor de identidade** sob a **Single Sign-On** secção e execute os seguintes passos:

    ![Configuração do Jamf Pro](./media/jamfprosamlconnector-tutorial/configure3.png)

    a. Selecione **outras** como uma opção a partir do **fornecedor de identidade** lista pendente.

    b. Na **outros fornecedor** caixa de texto, introduza **do Azure AD**.

    c. Selecione **URL de metadados** como uma opção da **origem de METADADOS do fornecedor de identidade** menu pendente e, na caixa de texto seguinte, cole o **Url de metadados de Federação de aplicação** valor que copiou do portal do Azure.

    d. Cópia a **ID de entidade** valor e cole-o no **identificador (ID de entidade)** caixa de texto no **domínio de Jamf Pro e URLs** secção no portal do Azure.

    >[!NOTE]
    > Este valor tênues é a parte de subdomínio. Utilize este valor para concluir o início de sessão no URL e o URL de resposta no **domínio de Jamf Pro e URLs** secção no portal do Azure.

    e. Clique em **Guardar**.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD 

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

1. No portal do Azure, no painel esquerdo, selecione **do Azure Active Directory**, selecione **utilizadores**e, em seguida, selecione **todos os utilizadores**.

    ![image](./media/jamfprosamlconnector-tutorial/d_users_and_groups.png)

2. Selecione **novo utilizador** na parte superior do ecrã.

    ![image](./media/jamfprosamlconnector-tutorial/d_adduser.png)

3. Nas propriedades do utilizador, execute os seguintes passos.

    ![image](./media/jamfprosamlconnector-tutorial/d_userproperties.png)

    a. Na **Name** campo introduza **BrittaSimon**.
  
    b. Na **nome de utilizador** tipo de campo **brittasimon@yourcompanydomain.extension**  
    Por exemplo, BrittaSimon@contoso.com

    c. Selecione **propriedades**, selecione a **palavra-passe de Show** caixa de verificação e, em seguida, anote o valor que é apresentado na caixa de palavra-passe.

    d. Selecione **Criar**.

### <a name="create-a-jamf-pro-test-user"></a>Criar um utilizador de teste do Jamf Pro

Para ativar a utilizadores do Azure AD iniciar sessão no Jamf Pro, tem de ser aprovisionados no Jamf Pro. No caso do Jamf Pro, o aprovisionamento é uma tarefa manual.

**Para Aprovisionar uma conta de utilizador, execute os seguintes passos:**

1. Inicie sessão no site da sua empresa Jamf Pro como administrador.

2. Clique nas **ícone de definições** do canto superior direito da página.

    ![Adicionar o funcionário](./media/jamfprosamlconnector-tutorial/configure1.png)

3. Clique em **contas de utilizador do Jamf Pro & grupos**.

    ![Adicionar o funcionário](./media/jamfprosamlconnector-tutorial/user1.png)

4. Clique em **Novo**.

    ![Adicionar o funcionário](./media/jamfprosamlconnector-tutorial/user2.png)

5. Selecione **criar conta Standard**.

    ![Adicionar o funcionário](./media/jamfprosamlconnector-tutorial/user3.png)

6. Sobre o **nova conta** dailog, execute os seguintes passos:

    ![Adicionar o funcionário](./media/jamfprosamlconnector-tutorial/user4.png)

    a. Na **nome de utilizador** caixa de texto, escreva o nome completo do BrittaSimon.

    b. Selecione as opções apropriadas de acordo com a sua organização para o **nível de acesso**, **PRIVILÉGIO definir**e para **estado de acesso**.

    c. Na **FULLNAME** caixa de texto, escreva o nome completo da Eduarda Almeida.

    d. Na **endereço de E-Mail** caixa de texto, escreva o endereço de e-mail da conta da Eduarda Almeida.

    e. Na **palavra-passe** caixa de texto, escreva a palavra-passe do utilizador.

    f. Na **Verifique se a palavra-passe** caixa de texto, escreva a palavra-passe do utilizador.

    g. Clique em **Guardar**.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único, concedendo acesso para o Jamf Pro.

1. No portal do Azure, selecione **aplicações empresariais**, selecione **todos os aplicativos**, em seguida, selecione **Jamf Pro**.

    ![image](./media/jamfprosamlconnector-tutorial/d_all_applications.png)

2. Na lista de aplicações, selecione **Jamf Pro**.

    ![image](./media/jamfprosamlconnector-tutorial/d_all_proapplications.png)

3. No menu à esquerda, selecione **utilizadores e grupos**.

    ![image](./media/jamfprosamlconnector-tutorial/d_leftpaneusers.png)

4. Selecione o **Add** botão, em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![image](./media/jamfprosamlconnector-tutorial/d_assign_user.png)

4. Na **utilizadores e grupos** caixa de diálogo select **Eduarda Almeida** na lista de utilizadores, em seguida, clique o **selecionar** na parte inferior do ecrã.

5. Na **adicionar atribuição** caixa de diálogo select a **atribuir** botão.

### <a name="test-single-sign-on"></a>Testar o início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico do Jamf Pro no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação do Jamf Pro.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)
