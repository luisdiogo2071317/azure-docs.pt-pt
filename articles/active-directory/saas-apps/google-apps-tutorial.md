---
title: 'Tutorial: Integração do Azure Active Directory com o G Suite | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e o G Suite.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 38a6ca75-7fd0-4cdc-9b9f-fae080c5a016
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/03/2018
ms.author: jeedes
ms.openlocfilehash: 6974350c6abbc6c5f5a8e10b22e91796e2564b08
ms.sourcegitcommit: f58fc4748053a50c34a56314cf99ec56f33fd616
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/04/2018
ms.locfileid: "48268064"
---
# <a name="tutorial-azure-active-directory-integration-with-g-suite"></a>Tutorial: Integração do Azure Active Directory com o G Suite

Neste tutorial, saiba como integrar o G Suite no Azure Active Directory (Azure AD).

Integrar o G Suite no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao G Suite.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada ao G Suite (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com G Suite, terá dos seguintes itens:

- Uma subscrição do Azure
- Um G Suite logon único habilitado subscrição
- Uma subscrição do Google Apps ou a subscrição de Google Cloud Platform.

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="frequently-asked-questions"></a>Perguntas Mais Frequentes

1.  **P: O integração de Google Cloud Platform SSO com o Azure AD para o suporte de integração?**
    
    R: Sim. Google Cloud Platform e o Google Apps compartilham a mesma plataforma de autenticação. Portanto, para fazer a integração de GCP terá de configurar o SSO com o Google Apps.


1. **P: são Chromebooks e outros dispositivos Chrome compatíveis com o Azure AD início de sessão único?**
   
    R: Sim, os utilizadores conseguem iniciar sessão nos respetivos dispositivos Chromebook através das respetivas credenciais do Azure AD. Ver isso [artigo de suporte do G Suite](https://support.google.com/chrome/a/answer/6060880) para obter informações sobre por que os utilizadores podem recebe o pedido de credenciais duas vezes.

1. **P: Se ativar a início de sessão único, os utilizadores poderão utilizar as credenciais do Azure AD para iniciar sessão em qualquer produto do Google, tais como a sala de aulas do Google, GMail, Google Drive, YouTube e assim por diante?**
   
    R: Sim, dependendo no [quais G Suite](https://support.google.com/a/answer/182442?hl=en&ref_topic=1227583) optar por ativar ou desativar para a sua organização.

1. **P: posso ativar o início de sessão único para apenas um subconjunto dos meus utilizadores do G Suite?**
   
    R: não, ativando o início de sessão único imediatamente requer que todos os seus utilizadores do G Suite autenticar com as credenciais do Azure AD. Porque o G Suite não suporta a ter vários fornecedores de identidade, o fornecedor de identidade para o seu ambiente do G Suite pode ser do Azure AD ou Google – mas não ambos simultaneamente.

1. **P: se um usuário está conectado por meio do Windows, são autenticam automaticamente ao G Suite sem introdução pedida uma palavra-passe?**
   
    R: Existem duas opções para ativar este cenário. Em primeiro lugar, os utilizadores podem iniciar sessão em dispositivos Windows 10 através da [Azure Active Directory Join](../device-management-introduction.md). Em alternativa, os utilizadores foi possível iniciar sessão em dispositivos Windows que estão associados a um domínio para um diretório de Active Directory no local que tenha sido ativada para início de sessão único para o Azure AD através de um [serviços de Federação do Active Directory (AD FS)](../hybrid/plan-connect-user-signin.md) implementação. Ambas as opções exigem que execute os passos no tutorial seguinte para ativar o início de sessão único entre o Azure AD e G Suite.

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando o G Suite da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-g-suite-from-the-gallery"></a>Adicionando o G Suite da Galeria

Para configurar a integração do G Suite para o Azure AD, terá de adicionar G Suite a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar o G Suite da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![image](./media/google-apps-tutorial/selectazuread.png)

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![image](./media/google-apps-tutorial/a_select_app.png)
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![image](./media/google-apps-tutorial/a_new_app.png)

4. Na caixa de pesquisa, escreva **G Suite**, selecione **G Suite** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

     ![image](./media/google-apps-tutorial/a_add_app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com G Suite com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no G Suite para um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no G Suite tem de ser estabelecida.

No G Suite, atribuir o valor do **nome de utilizador** no Azure AD como o valor da **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com o G Suite, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste do G Suite](#create-a-g-suite-test-user)**  - para ter um equivalente da Eduarda Almeida no G Suite, que está ligado à representação de utilizador do Azure AD.
1. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único na sua aplicação do G Suite.

**Para configurar o Azure AD início de sessão único com o G Suite, execute os seguintes passos:**

1. Na [portal do Azure](https://portal.azure.com/), na **G Suite** página de integração de aplicações, selecione **início de sessão único**.

    ![image](./media/google-apps-tutorial/b1_b2_select_sso.png)

2. Clique em **modo de início de sessão único de alteração** na parte superior do ecrã para selecionar o **SAML** modo.

      ![image](./media/google-apps-tutorial/b1_b2_saml_ssso.png)

3. Sobre o **selecionar um método de início de sessão único** caixa de diálogo, clique em **selecione** para **SAML** modo para ativar o início de sessão único.

    ![image](./media/google-apps-tutorial/b1_b2_saml_sso.png)

4. Sobre o **definido no início de sessão único com o SAML** página, clique em **editar** botão para abrir **configuração básica de SAML** caixa de diálogo.

    ![image](./media/google-apps-tutorial/b1-domains_and_urlsedit.png)

5. Sobre o **configuração básica de SAML** secção, execute os seguintes passos:

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://www.google.com/a/<yourdomain.com>/ServiceLogin?continue=https://mail.google.com`

    b. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: 
    | |
    |--|
    | `google.com/a/<yourdomain.com>` |
    | `google.com` |
    | `http://google.com` |
    | `http://google.com/a/<yourdomain.com>` |

    ![image](./media/google-apps-tutorial/b1-domains_and_urls.png)
 
    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o URL de início de sessão e o identificador real. Contacte [equipa de suporte de cliente do G Suite](https://www.google.com/contact/) obter esses valores.

6. Aplicação do G Suite espera que as asserções SAML num formato específico. Configure as seguintes declarações para esta aplicação. Pode gerir os valores destes atributos do **atributos de utilizador** secção na página de integração de aplicações. Sobre o **definido no início de sessão único com o SAML** página, clique em **editar** botão para abrir **atributos de utilizador** caixa de diálogo.

    ![image](./media/google-apps-tutorial/i3-attribute.png)

7. No **afirmações de utilizador** secção sobre o **atributos de utilizador** caixa de diálogo, configurar o atributo de token de SAML conforme mostrado na imagem acima e execute os seguintes passos:
    
    a. Clique em **edite** botão para abrir o **afirmações de utilizador de gerir** caixa de diálogo.

    ![image](./media/google-apps-tutorial/i2-attribute.png)

    ![image](./media/google-apps-tutorial/i4-attribute.png)

    b. Partir do **atributo de origem** listar, selelct o valor do atributo.

    c. Clique em **Guardar**.

8. No **definir a segurança de início de sessão único com o SAML** na página a **certificado de assinatura SAML** secção, clique em **transferir** para transferir o certificado apropriado, de acordo sua requisito e guarde-o no seu computador.

    ![image](./media/google-apps-tutorial/certificatebase64.png)

9. Sobre o **configurar o G Suite** secção, copie o URL adequado, de acordo com seus requisitos.

    Tenha em atenção que o URL pode dizer o seguinte:

    a. URL de início de sessão

    b. Identificador do Azure Ad

    c. URL de fim de sessão

    ![image](./media/google-apps-tutorial/d1_saml.png) 

10. Abra um novo separador no seu browser e inicie sessão no [consola de administração do G Suite](http://admin.google.com/) com a sua conta de administrador.

11. Clique em **segurança**. Se não vir a ligação, podem estar ocultos sob o **More Controls** menu na parte inferior do ecrã.
   
    ![Clique em Security.][10]

12. Sobre o **Security** página, clique em **configurar início de sessão único (SSO).**
   
    ![Clique em SSO.][11]

13. Execute as seguintes alterações de configuração:
   
    ![Configurar o SSO][12]
   
    a. Selecione **SSO de configuração com o fornecedor de identidade de terceiros**.

    b. Na **URL da página de início de sessão** campo no G Suite, cole o valor de **único URL de início de sessão no serviço** que copiou do portal do Azure.

    c. Na **URL de página de início de sessão** campo no G Suite, cole o valor de **URL de fim de sessão** que copiou do portal do Azure. 

    d. Na **alterar o URL de palavra-passe** campo no G Suite, cole o valor de **alterar a URL da palavra-passe** que copiou do portal do Azure. 

    e. No G Suite, para o **certificado de verificação**, carregue o certificado que transferiu a partir do portal do Azure.

    f. Selecione **utilizar um emissor específico do domínio**.

    g. Clique em **guardar alterações**.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

1. No portal do Azure, no painel esquerdo, selecione **do Azure Active Directory**, selecione **utilizadores**e, em seguida, selecione **todos os utilizadores**.

    ![image](./media/google-apps-tutorial/d_users_and_groups.png)

2. Selecione **novo utilizador** na parte superior do ecrã.

    ![image](./media/google-apps-tutorial/d_adduser.png)

3. Nas propriedades do utilizador, execute os seguintes passos.

    ![image](./media/google-apps-tutorial/d_userproperties.png)

    a. Na **Name** campo introduza **BrittaSimon**.
  
    b. Na **nome de utilizador** tipo de campo **brittasimon@yourcompanydomain.extension**  
    Por exemplo, BrittaSimon@contoso.com

    c. Selecione **propriedades**, selecione a **palavra-passe de Show** caixa de verificação e, em seguida, anote o valor que é apresentado na caixa de palavra-passe.

    d. Selecione **Criar**.
 
### <a name="create-a-g-suite-test-user"></a>Criar um utilizador de teste do G Suite

O objetivo desta secção é criar um usuário chamado Eduarda Almeida no Software do G Suite. G Suite suporta o aprovisionamento automático, que está por predefinição, ativada. Não existe nenhuma ação para nesta secção. Se um utilizador já não existir no Software do G Suite, uma nova é criada quando tentar acessar o Software do G Suite.

>[!NOTE]
>Certifique-se de que os seus utilizadores são já existe no G Suite se aprovisionamento no Azure AD não foi ativado antes de testar o início de sessão único.

>[!NOTE] 
>Se precisar de criar manualmente um utilizador, entre em contato com o [equipa de suporte do Google](https://www.google.com/contact/).



### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único, concedendo acesso ao G Suite.

1. No portal do Azure, selecione **aplicações empresariais**, selecione **todos os aplicativos**.

    ![image](./media/google-apps-tutorial/d_all_applications.png)

2. Na lista de aplicações, selecione **G Suite**.

    ![image](./media/google-apps-tutorial/d_all_proapplications.png)

3. No menu à esquerda, selecione **utilizadores e grupos**.

    ![image](./media/google-apps-tutorial/d_leftpaneusers.png)

4. Selecione o **Add** botão, em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![image](./media/google-apps-tutorial/d_assign_user.png)

4. Na **utilizadores e grupos** caixa de diálogo select **Eduarda Almeida** na lista de utilizadores, em seguida, clique o **selecionar** na parte inferior do ecrã.

5. Na **adicionar atribuição** caixa de diálogo select a **atribuir** botão.
    
### <a name="test-single-sign-on"></a>Testar o início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico do G Suite no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação do G Suite.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[10]: ./media/google-apps-tutorial/gapps-security.png
[11]: ./media/google-apps-tutorial/security-gapps.png
[12]: ./media/google-apps-tutorial/gapps-sso-config.png

