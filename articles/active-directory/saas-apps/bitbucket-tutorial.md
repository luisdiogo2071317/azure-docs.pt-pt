---
title: 'Tutorial: Integração do Azure Active Directory com o SAML SSO para Bitbucket pela resolução GmbH | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e o SAML SSO para Bitbucket pela resolução GmbH.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: fc947df1-f24e-43ae-9a34-518293583d69
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/04/2017
ms.author: jeedes
ms.openlocfilehash: 21b6adb98fdb973b75aa1d6db519777bab730d73
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39048128"
---
# <a name="tutorial-azure-active-directory-integration-with-saml-sso-for-bitbucket-by-resolution-gmbh"></a>Tutorial: Integração do Azure Active Directory com o SAML SSO para Bitbucket pela resolução GmbH

Neste tutorial, saiba como integrar o SSO SAML para Bitbucket pela resolução GmbH com o Azure Active Directory (Azure AD).

Integrar o SSO SAML para Bitbucket pela resolução GmbH com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao SAML SSO para Bitbucket pela resolução GmbH.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para SAML SSO para Bitbucket pela resolução GmbH (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o SAML SSO para Bitbucket pela resolução GmbH, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um SAML SSO para Bitbucket pela resolução GmbH início de sessão único na subscrição ativada

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando SAML SSO para Bitbucket pela resolução GmbH da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-saml-sso-for-bitbucket-by-resolution-gmbh-from-the-gallery"></a>Adicionando SAML SSO para Bitbucket pela resolução GmbH da Galeria
Para configurar a integração de SAML SSO para Bitbucket pela resolução GmbH com o Azure AD, terá de adicionar SAML SSO para Bitbucket pela resolução GmbH a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar SAML SSO para Bitbucket pela resolução GmbH a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

4. Na caixa de pesquisa, escreva **SAML SSO para Bitbucket pela resolução GmbH**, selecione **SAML SSO para Bitbucket pela resolução GmbH** no painel de resultados, em seguida, clique em **Add** botão para adicionar o aplicação.

    ![SAML SSO para Bitbucket pela resolução GmbH, na lista de resultados](./media/bitbucket-tutorial/tutorial_bitbucket_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com o SAML SSO para Bitbucket pela resolução que GmbH com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa de saber o que o equivalente utilizador no SAML SSO para Bitbucket pela resolução GmbH for para um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no SAML SSO para Bitbucket pela resolução GmbH deve ser estabelecido.

No SAML SSO para Bitbucket pela resolução GmbH, atribuir o valor do **nome de utilizador** no Azure AD como o valor da **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com o SAML SSO para Bitbucket pela resolução GmbH, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um SAML SSO para Bitbucket por utilizador de teste de GmbH de resolução](#create-a-saml-sso-for-bitbucket-by-resolution-gmbh-test-user)**  - para ter um equivalente da Eduarda Almeida no SAML SSO para Bitbucket pela resolução GmbH que está ligado à representação de utilizador do Azure AD.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão único no portal do Azure e configurar o início de sessão único na sua SAML SSO para Bitbucket pela resolução GmbH aplicação.

**Para configurar o Azure AD início de sessão único com o SAML SSO para Bitbucket pela resolução GmbH, execute os seguintes passos:**

1. No portal do Azure, sobre o **SAML SSO para Bitbucket pela resolução GmbH** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/bitbucket-tutorial/tutorial_bitbucket_samlbase.png)

3. Sobre o **SAML SSO para Bitbucket pela resolução GmbH domínio e URLs** secção, execute os seguintes passos, se desejar configurar a aplicação no modo de IDP iniciado:

    ![Informações de início de sessão de único SAML SSO para Bitbucket pela resolução GmbH domínio e URLs](./media/bitbucket-tutorial/tutorial_bitbucket_url.png)

    a. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://<server-base-url>/plugins/servlet/samlsso`

    b. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://<server-base-url>/plugins/servlet/samlsso`

4. Verifique **Mostrar definições de URL avançadas** e executar o passo seguinte, se desejar configurar a aplicação na **SP** iniciada pelo modo:

    ![Informações de início de sessão de único SAML SSO para Bitbucket pela resolução GmbH domínio e URLs](./media/bitbucket-tutorial/tutorial_bitbucket_url1.png)

    Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<server-base-url>/plugins/servlet/samlsso`
     
    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o identificador de real, a URL de resposta e o URL de início de sessão. Contacte [equipa de suporte de SAML SSO para Bitbucket pela resolução GmbH cliente](https://marketplace.atlassian.com/plugins/com.resolution.atlasplugins.samlsso-bitbucket/server/support) obter esses valores. 

5. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![O link de download de certificado](./media/bitbucket-tutorial/tutorial_bitbucket_certificate.png) 

6. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/bitbucket-tutorial/tutorial_general_400.png)
    
7. Início de sessão na sua SAML SSO para Bitbucket pelo site de empresa resolução GmbH como administrador.

8. No lado direito da barra de ferramentas principal, clique em **definições**.

9. Aceda à secção de contas, clique em **SAML SingleSignOn** na barra de menus.

    ![O Samlsingle](./media/bitbucket-tutorial/tutorial_bitbucket_samlsingle.png)

10. Sobre o **página de configuração de plug-in SAML SIngleSignOn**, clique em **adicionar idp**. 

    ![A adicionar o idp](./media/bitbucket-tutorial/tutorial_bitbucket_addidp.png)

11. Sobre o **escolha o fornecedor de identidade de SAML** página, execute os seguintes passos:

    ![O fornecedor de identidade](./media/bitbucket-tutorial/tutorial_bitbucket_identityprovider.png)

    a. Selecione **tipo de Idp** como **do AZURE AD**.

    b. Na **nome** caixa de texto, escreva o nome.

    c. Na **Descrição** caixa de texto, digite a descrição.

    d. Clique em **Seguinte**.

12. Sobre o **página de configuração do fornecedor de identidade**, clique em **próxima**.

    ![A configuração de identidade](./media/bitbucket-tutorial/tutorial_bitbucket_identityconfig.png)

13.  Sobre o **importação SAML Idp metadados** página, clique em **carregar ficheiro** para carregar o **XML de METADADOS** ficheiro que transferiu a partir do portal do Azure.

    ![O idpmetadata](./media/bitbucket-tutorial/tutorial_bitbucket_idpmetadata.png)
    
14. Clique em **Seguinte**.

15. Clique em **guardar as definições de**.

    ![A guardar](./media/bitbucket-tutorial/tutorial_bitbucket_save.png)

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/bitbucket-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/bitbucket-tutorial/create_aaduser_02.png)

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/bitbucket-tutorial/create_aaduser_03.png)

4. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/bitbucket-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-saml-sso-for-bitbucket-by-resolution-gmbh-test-user"></a>Criar um SAML SSO para Bitbucket por utilizador de teste de GmbH de resolução

O objetivo desta secção é criar um usuário chamado Eduarda Almeida no SAML SSO para Bitbucket pela resolução GmbH. SAML SSO para Bitbucket pela resolução GmbH suporta o aprovisionamento de just-in-time e também os utilizadores podem ser criados manualmente, contacte [equipa de suporte de SAML SSO para Bitbucket pela resolução GmbH cliente](https://marketplace.atlassian.com/plugins/com.resolution.atlasplugins.samlsso-bitbucket/server/support) de acordo com seus requisitos.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único, concedendo acesso para SAML SSO para Bitbucket pela resolução GmbH.

![Atribuir a função de utilizador][200] 

**Para atribuir Eduarda Almeida a SAML SSO para Bitbucket pela resolução GmbH, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **SAML SSO para Bitbucket pela resolução GmbH**.

    ![O SSO SAML para Bitbucket pela ligação de GmbH resolução na lista de aplicações](./media/bitbucket-tutorial/tutorial_bitbucket_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Ao clicar o SAML SSO para Bitbucket por mosaico de GmbH resolução no painel de acesso, deve obter automaticamente com sessão iniciada para sua SAML SSO para Bitbucket pela resolução GmbH aplicação.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/bitbucket-tutorial/tutorial_general_01.png
[2]: ./media/bitbucket-tutorial/tutorial_general_02.png
[3]: ./media/bitbucket-tutorial/tutorial_general_03.png
[4]: ./media/bitbucket-tutorial/tutorial_general_04.png

[100]: ./media/bitbucket-tutorial/tutorial_general_100.png

[200]: ./media/bitbucket-tutorial/tutorial_general_200.png
[201]: ./media/bitbucket-tutorial/tutorial_general_201.png
[202]: ./media/bitbucket-tutorial/tutorial_general_202.png
[203]: ./media/bitbucket-tutorial/tutorial_general_203.png

