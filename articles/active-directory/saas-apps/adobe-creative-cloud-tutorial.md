---
title: 'Tutorial: Integração do Azure Active Directory com o Adobe Creative Cloud | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e o Adobe Creative Cloud.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: c199073f-02ce-45c2-b515-8285d4bbbca2
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2018
ms.author: jeedes
ms.openlocfilehash: e1788de7c2372797b2034eb1753ab435c1299889
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38548287"
---
# <a name="tutorial-azure-active-directory-integration-with-adobe-creative-cloud"></a>Tutorial: Integração do Azure Active Directory com o Adobe Creative Cloud

Neste tutorial, saiba como integrar o Adobe Creative Cloud no Azure Active Directory (Azure AD).

Integrar o Adobe Creative Cloud no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso à Adobe Creative Cloud.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para o Adobe Creative Cloud (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o Adobe Creative Cloud, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Adobe Creative Cloud logon único habilitado subscrição
- Uma versão do Adobe Creative Cloud empresarial necessária

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionar o Adobe Creative Cloud a partir da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-adobe-creative-cloud-from-the-gallery"></a>Adicionar o Adobe Creative Cloud a partir da Galeria

Para configurar a integração do Adobe Creative Cloud no Azure AD, terá de adicionar Adobe Creative Cloud a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar o Adobe Creative Cloud a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

4. Na caixa de pesquisa, escreva **Adobe Creative Cloud**, selecione **Adobe Creative Cloud** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Adobe Creative Cloud na lista de resultados](./media/adobe-creative-cloud-tutorial/tutorial_adobecreativecloud_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com o Adobe Creative Cloud com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Adobe Creative Cloud a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Adobe Creative Cloud tem de ser estabelecida.

Para configurar e testar o Azure AD início de sessão único com o Adobe Creative Cloud, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste do Adobe Creative Cloud](#create-an-adobe-creative-cloud-test-user)**  - para ter um equivalente da Eduarda Almeida na Adobe Creative Cloud que está ligado à representação de utilizador do Azure AD.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo Adobe Creative Cloud.

**Para configurar o Azure AD início de sessão único com o Adobe Creative Cloud, execute os seguintes passos:**

1. No portal do Azure, sobre o **Adobe Creative Cloud** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.

    ![Caixa de diálogo de início de sessão único](./media/adobe-creative-cloud-tutorial/tutorial_adobecreativecloud_samlbase.png)

3. Sobre o **Adobe Creative Cloud domínio e URLs** secção, execute os seguintes passos, se desejar configurar a aplicação no modo de IDP iniciado:

    ![Adobe Creative Cloud domínio e URLs únicas início de sessão em informações](./media/adobe-creative-cloud-tutorial/tutorial_adobecreativecloud_url.png)

    a. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://www.okta.com/saml2/service-provider/<token>`

    b. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://<company name>.okta.com/auth/saml20/accauthlinktest`

    > [!NOTE]
    > Estes valores não são reais. Atualize estes valores com o identificador real e o URL de resposta. Contacte [Adobe Creative Cloud empresarial](https://www.adobe.com/au/creativecloud/business/teams/plans.html) obter esses valores.

4. Verifique **Mostrar definições de URL avançadas** e executar o passo seguinte, se desejar configurar a aplicação na **SP** iniciada pelo modo:

    ![Adobe Creative Cloud domínio e URLs únicas início de sessão em informações](./media/adobe-creative-cloud-tutorial/tutorial_adobecreativecloud_url2.png)

    Na **URL de início de sessão** caixa de texto, digite o valor como: `https://adobe.com`

5. Sobre o **certificado de assinatura SAML** secção, clique em **Certificate(Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![O link de download de certificado](./media/adobe-creative-cloud-tutorial/tutorial_adobecreativecloud_certificate.png)
     
6. Aplicação do Adobe Creative Cloud espera que as asserções SAML num formato específico. Configure as seguintes declarações para esta aplicação. Pode gerir os valores destes atributos do **atributo de utilizador** separador do aplicativo. Captura de ecrã seguinte mostra um exemplo disso.

    ![Configurar o início de sessão único](./media/adobe-creative-cloud-tutorial/tutorial_attribute.png)

7. No **atributos de utilizador** secção sobre o **início de sessão único** caixa de diálogo, configurar o atributo de token de SAML conforme mostrado na imagem acima e execute os seguintes passos:

    | Nome do Atributo | Valor do Atributo |
    | ---------------| ----------------|
    | FirstName |User.givenName |
    | LastName |User.Surname |
    | Email |User.Mail |

    a. Clique em **adicionar atributo** para abrir o **adicionar atributo** caixa de diálogo.

    ![Configurar o início de sessão único](./media/adobe-creative-cloud-tutorial/tutorial_attribute_04.png)

    ![Configurar o início de sessão único](./media/adobe-creative-cloud-tutorial/tutorial_attribute_05.png)

    b. Na **nome** caixa de texto, escreva o nome de atributo apresentado para essa linha.

    c. Partir do **valor** lista, digite o valor de atributo apresentado para essa linha.

    d. Clique em **OK**.

    > [!NOTE]
    > Os utilizadores tem de ter uma licença válida do ExO do Office 365 para e-mail valor deve ser preenchida na resposta SAML da afirmação.

8. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/adobe-creative-cloud-tutorial/tutorial_general_400.png)

9. Sobre o **configuração do Adobe Creative Cloud** secção, clique em **configurar Adobe Creative Cloud** para abrir **configurar início de sessão** janela. Cópia a **ID de entidade de SAML e SAML único início de sessão no URL do serviço** partir a **seção de referência rápida**.

    ![Configuração do Adobe Creative Cloud](./media/adobe-creative-cloud-tutorial/tutorial_adobecreativecloud_configure.png)

10. Numa janela do browser web diferente, inicie sessão no [consola de administração do Adobe](https://adminconsole.adobe.com) como administrador.

11. Aceda a **configurações** no painel de navegação superior da barra e, em seguida, escolha **identidade**. É aberta a lista de domínios. Clique em **configurar** link em relação a seu domínio. Em seguida, execute os seguintes passos no **início de sessão único na configuração necessária** secção. Para obter mais informações, consulte [um domínio de configuração](https://helpx.adobe.com/enterprise/using/set-up-domain.html)

    ![As definições](https://helpx.adobe.com/content/dam/help/en/enterprise/using/configure-microsoft-azure-with-adobe-sso/_jcr_content/main-pars/procedure_719391630/proc_par/step_3/step_par/image/edit-sso-configuration.png "definições")

    a. Clique em **navegue** para carregar o certificado transferido do Azure AD para **IDP certificado**.

    b. Na **emissor IDP** caixa de texto, colocar o valor de **Id de entidade de SAML** que copiou do **configurar início de sessão** secção no portal do Azure.

    c. Na **URL de início de sessão do IDP** caixa de texto, colocar o valor de **URL do serviço SAML SSO** que copiou do **configurar início de sessão** secção no portal do Azure.

    d. Selecione **HTTP - redirecionamento** como **IDP enlace**.

    e. Selecione **endereço de E-Mail** como **definição de início de sessão do utilizador**.

    f. Clique em **guardar** botão.

12. O dashboard agora apresentará o XML **"Metadados transferir"** ficheiro. Ela contém a EntityDescriptor URL da Adobe e a AssertionConsumerService URL. Abra o ficheiro e configurá-los na aplicação do Azure AD.

    ![Configurar o início de sessão único no lado de aplicação](./media/adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_003.png)

    a. Utilize o valor de EntityDescriptor Adobe fornecido por **identificador** sobre o **configurar as definições da aplicação** caixa de diálogo.

    b. Utilize o valor de AssertionConsumerService Adobe fornecido por **URL de resposta** sobre o **configurar as definições da aplicação** caixa de diálogo.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/adobe-creative-cloud-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/adobe-creative-cloud-tutorial/create_aaduser_02.png)

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/adobe-creative-cloud-tutorial/create_aaduser_03.png)

4. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/adobe-creative-cloud-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-an-adobe-creative-cloud-test-user"></a>Criar um utilizador de teste do Adobe Creative Cloud

Para habilitar os utilizadores do Azure AD iniciar sessão no Adobe Creative Cloud, tem de ser aprovisionados no Adobe Creative Cloud. No caso do Adobe Creative Cloud, o aprovisionamento é uma tarefa manual.

### <a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Para aprovisionar contas de utilizador, execute os seguintes passos:

1. Inicie sessão no [consola de administração do Adobe](https://adminconsole.adobe.com) site como um administrador.

2. Adicionar o utilizador da consola da Adobe como ID Federado e atribuí-las a um perfil de produto. Para obter informações detalhadas sobre como adicionar utilizadores, consulte [adiciona os utilizadores da consola de administração do Adobe](https://helpx.adobe.com/enterprise/using/users.html#Addusers) 

3. Neste momento, digite seu endereço de e-mail/upn no formulário de início de sessão do Adobe, prima tab, e deve ser federado para o Azure AD:
    * Acesso Web: www.adobe.com > início de sessão
    * Dentro do utilitário de aplicativo de desktop > início de sessão
    * Dentro da aplicação > Ajuda > início de sessão

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso à Adobe Creative Cloud.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a Adobe Creative Cloud, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **Adobe Creative Cloud**.

    ![A ligação do Adobe Creative Cloud na lista de aplicações](./media/adobe-creative-cloud-tutorial/tutorial_adobecreativecloud_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico do Adobe Creative Cloud no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo Adobe Creative Cloud.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)
* [Configurar um domínio (adobe.com)](https://helpx.adobe.com/enterprise/using/set-up-domain.html)
* [Configurar o Azure para utilização com o Adobe SSO (adobe.com)](https://helpx.adobe.com/enterprise/kb/configure-microsoft-azure-with-adobe-sso.html)

<!--Image references-->

[1]: ./media/adobe-creative-cloud-tutorial/tutorial_general_01.png
[2]: ./media/adobe-creative-cloud-tutorial/tutorial_general_02.png
[3]: ./media/adobe-creative-cloud-tutorial/tutorial_general_03.png
[4]: ./media/adobe-creative-cloud-tutorial/tutorial_general_04.png

[100]: ./media/adobe-creative-cloud-tutorial/tutorial_general_100.png

[200]: ./media/adobe-creative-cloud-tutorial/tutorial_general_200.png
[201]: ./media/adobe-creative-cloud-tutorial/tutorial_general_201.png
[202]: ./media/adobe-creative-cloud-tutorial/tutorial_general_202.png
[203]: ./media/adobe-creative-cloud-tutorial/tutorial_general_203.png
