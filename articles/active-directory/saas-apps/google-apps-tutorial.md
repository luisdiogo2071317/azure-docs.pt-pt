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
ms.date: 04/02/2018
ms.author: jeedes
ms.openlocfilehash: a8478436bfae20ee729ce41bcb134bd3a77fb26e
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39053476"
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

- Uma subscrição do Azure AD
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


2. **P: são Chromebooks e outros dispositivos Chrome compatíveis com o Azure AD início de sessão único?**
   
    R: Sim, os utilizadores conseguem iniciar sessão nos respetivos dispositivos Chromebook através das respetivas credenciais do Azure AD. Ver isso [artigo de suporte do G Suite](https://support.google.com/chrome/a/answer/6060880) para obter informações sobre por que os utilizadores podem recebe o pedido de credenciais duas vezes.

3. **P: Se ativar a início de sessão único, os utilizadores poderão utilizar as credenciais do Azure AD para iniciar sessão em qualquer produto do Google, tais como a sala de aulas do Google, GMail, Google Drive, YouTube e assim por diante?**
   
    R: Sim, dependendo no [quais G Suite](https://support.google.com/a/answer/182442?hl=en&ref_topic=1227583) optar por ativar ou desativar para a sua organização.

4. **P: posso ativar o início de sessão único para apenas um subconjunto dos meus utilizadores do G Suite?**
   
    R: não, ativando o início de sessão único imediatamente requer que todos os seus utilizadores do G Suite autenticar com as credenciais do Azure AD. Porque o G Suite não suporta a ter vários fornecedores de identidade, o fornecedor de identidade para o seu ambiente do G Suite pode ser do Azure AD ou Google – mas não ambos simultaneamente.

5. **P: se um usuário está conectado por meio do Windows, são autenticam automaticamente ao G Suite sem introdução pedida uma palavra-passe?**
   
    R: Existem duas opções para ativar este cenário. Em primeiro lugar, os utilizadores podem iniciar sessão em dispositivos Windows 10 através da [Azure Active Directory Join](../device-management-introduction.md). Em alternativa, os utilizadores foi possível iniciar sessão em dispositivos Windows que estão associados a um domínio para um diretório de Active Directory no local que tenha sido ativada para início de sessão único para o Azure AD através de um [serviços de Federação do Active Directory (AD FS)](../connect/active-directory-aadconnect-user-signin.md) implementação. Ambas as opções exigem que execute os passos no tutorial seguinte para ativar o início de sessão único entre o Azure AD e G Suite.

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando o G Suite da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-g-suite-from-the-gallery"></a>Adicionando o G Suite da Galeria
Para configurar a integração do G Suite para o Azure AD, terá de adicionar G Suite a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar o G Suite da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

4. Na caixa de pesquisa, escreva **G Suite**, selecione **G Suite** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![G Suite na lista de resultados](./media/google-apps-tutorial/tutorial_googleapps_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com G Suite com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no G Suite para um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no G Suite tem de ser estabelecida.

No G Suite, atribuir o valor do **nome de utilizador** no Azure AD como o valor da **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com o G Suite, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste do G Suite](#create-a-g-suite-test-user)**  - para ter um equivalente da Eduarda Almeida no G Suite, que está ligado à representação de utilizador do Azure AD.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único na sua aplicação do G Suite.

**Para configurar o Azure AD início de sessão único com o G Suite, execute os seguintes passos:**

1. No portal do Azure, sobre o **G Suite** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.

    ![Caixa de diálogo de início de sessão único](./media/google-apps-tutorial/tutorial_googleapps_samlbase.png)

3. Na **domínio do G Suite e URLs** secção, se pretende configurar para o **Gmail** execute os seguintes passos:

    ![URLs de domínio do G Suite e únicas início de sessão em informações](./media/google-apps-tutorial/tutorial_googleapps_urlgmail.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL de URL com o seguinte padrão: `https://www.google.com/a/<yourdomain.com>/ServiceLogin?continue=https://mail.google.com`

    b. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: 
    | |
    |--|
    | `google.com/a/<yourdomain.com>` |
    | `google.com` |
    | `http://google.com` |
    | `http://google.com/a/<yourdomain.com>` |
 
    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o URL de início de sessão e o identificador real. Contacte [equipa de suporte de cliente do G Suite](https://www.google.com/contact/) obter esses valores.

4. Na **domínio do G Suite e URLs** secção, se pretende configurar para o **Google Cloud Platform** execute os seguintes passos:

    ![URLs de domínio do G Suite e únicas início de sessão em informações](./media/google-apps-tutorial/tutorial_googleapps_url1.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL de URL com o seguinte padrão: `https://www.google.com/a/<yourdomain.com>/ServiceLogin?continue=https://console.cloud.google.com `

    b. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: 
    | |
    |--|
    | `google.com/a/<yourdomain.com>` |
    | `google.com` |
    | `http://google.com` |
    | `http://google.com/a/<yourdomain.com>` |
    
    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o URL de início de sessão e o identificador real. Contacte [equipa de suporte de cliente do G Suite](https://www.google.com/contact/) obter esses valores. 

5. Sobre o **certificado de assinatura SAML** secção, clique em **certificado** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![O link de download de certificado](./media/google-apps-tutorial/tutorial_googleapps_certificate.png) 

6. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/google-apps-tutorial/tutorial_general_400.png)

7. Sobre o **configuração do G Suite** secção, clique em **configurar o G Suite** para abrir **configurar início de sessão** janela. Cópia a **URL de palavra-passe do URL de fim de sessão, SAML único início de sessão no URL do serviço e de alterações** partir o **secção de referência rápida.**

    ![Configuração do G Suite](./media/google-apps-tutorial/tutorial_googleapps_configure.png) 

8. Abra um novo separador no seu browser e inicie sessão no [consola de administração do G Suite](http://admin.google.com/) com a sua conta de administrador.

9. Clique em **segurança**. Se não vir a ligação, podem estar ocultos sob o **More Controls** menu na parte inferior do ecrã.
   
    ![Clique em Security.][10]

10. Sobre o **Security** página, clique em **configurar início de sessão único (SSO).**
   
    ![Clique em SSO.][11]

11. Execute as seguintes alterações de configuração:
   
    ![Configurar o SSO][12]
   
    a. Selecione **SSO de configuração com o fornecedor de identidade de terceiros**.

    b. Na **URL da página de início de sessão** campo no G Suite, cole o valor de **único URL de início de sessão no serviço** que copiou do portal do Azure.

    c. Na **URL de página de início de sessão** campo no G Suite, cole o valor de **URL de fim de sessão** que copiou do portal do Azure. 

    d. Na **alterar o URL de palavra-passe** campo no G Suite, cole o valor de **alterar a URL da palavra-passe** que copiou do portal do Azure. 

    e. No G Suite, para o **certificado de verificação**, carregue o certificado que transferiu a partir do portal do Azure.

    f. Selecione **utilizar um emissor específico do domínio**.

    g. Clique em **guardar alterações**.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/google-apps-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/google-apps-tutorial/create_aaduser_02.png)

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/google-apps-tutorial/create_aaduser_03.png)

4. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/google-apps-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-g-suite-test-user"></a>Criar um utilizador de teste do G Suite

O objetivo desta secção é criar um usuário chamado Eduarda Almeida no Software do G Suite. G Suite suporta o aprovisionamento automático, que está por predefinição, ativada. Não existe nenhuma ação para nesta secção. Se um utilizador já não existir no Software do G Suite, uma nova é criada quando tentar acessar o Software do G Suite.

>[!NOTE] 
>Se precisar de criar manualmente um utilizador, entre em contato com o [equipa de suporte do Google](https://www.google.com/contact/).

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único, concedendo acesso ao G Suite.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida ao G Suite, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **G Suite**.

    ![A ligação do G Suite na lista de aplicações](./media/google-apps-tutorial/tutorial_googleapps_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico do G Suite no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação do G Suite.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/googleapps-tutorial/tutorial_general_01.png
[2]: ./media/googleapps-tutorial/tutorial_general_02.png
[3]: ./media/googleapps-tutorial/tutorial_general_03.png
[4]: ./media/googleapps-tutorial/tutorial_general_04.png

[100]: ./media/googleapps-tutorial/tutorial_general_100.png

[200]: ./media/googleapps-tutorial/tutorial_general_200.png
[201]: ./media/googleapps-tutorial/tutorial_general_201.png
[202]: ./media/googleapps-tutorial/tutorial_general_202.png
[203]: ./media/googleapps-tutorial/tutorial_general_203.png
[10]: ./media/googleapps-tutorial/gapps-security.png
[11]: ./media/googleapps-tutorial/security-gapps.png
[12]: ./media/googleapps-tutorial/gapps-sso-config.png

