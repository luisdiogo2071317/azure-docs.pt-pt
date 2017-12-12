---
title: "Tutorial: Integração do Azure Active Directory com o Adobe experiência Manager | Microsoft Docs"
description: "Saiba como configurar o início de sessão entre o Azure Active Directory e Adobe experiência Manager."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 88a95bb5-c17c-474f-bb92-1f80f5344b5a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: jeedes
ms.openlocfilehash: 4a4fccc4210fd6cf0ddbe99089c84a1fd38d5b09
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-adobe-experience-manager"></a>Tutorial: Integração do Azure Active Directory com o Adobe experiência Manager

Neste tutorial, irá aprender a integrar o Adobe experiência gestor no Azure Active Directory (Azure AD).

Integrar o Adobe experiência Manager com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Adobe experiência Manager.
- Pode permitir aos utilizadores automaticamente obter com sessão iniciada no Adobe experiência Manager (Single Sign-On) com as respetivas contas do Azure AD.
- Pode gerir as contas numa localização central - portal do Azure.

Se pretender saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [que é o acesso a aplicações e início de sessão no Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o Adobe experiência Manager, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Gestor de experiência de Adobe-início de sessão único ativada subscrição

> [!NOTE]
> Para testar os passos neste tutorial, não recomendamos a utilização num ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, teste do Azure AD-início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos modulares principais:

1. Adicionar Gestor de experiência de Adobe na galeria do
2. Configurar e testar o Azure AD de sessão único-

## <a name="adding-adobe-experience-manager-from-the-gallery"></a>Adicionar Gestor de experiência de Adobe na galeria do
Para configurar a integração do Adobe experiência Manager com o Azure AD, tem de adicionar Gestor de experiência de Adobe na Galeria à sua lista de aplicações SaaS geridas.

**Para adicionar o Gestor de experiência de Adobe na galeria do, execute os seguintes passos:**

1. No  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **do Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todas as aplicações**.

    ![O painel de aplicações da empresa][2]
    
3. Para adicionar a nova aplicação, clique em **nova aplicação** botão no topo da caixa de diálogo.

    ![O novo botão de aplicação][3]

4. Na caixa de pesquisa, escreva **Adobe experiência Manager**, selecione **Adobe experiência Manager** partir do painel de resultados, em seguida, clique em **adicionar** botão para adicionar a aplicação.

    ![Gestor de experiência de Adobe na lista de resultados](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD-início de sessão único

Nesta secção, configure e teste do Azure AD-início de sessão único com o Adobe experiência Manager com base num utilizador de teste chamado "Britta Simon".

Para início de sessão trabalhar, do Azure AD tem de saber o que o utilizador homólogo no Adobe experiência Manager é um utilizador no Azure AD. Por outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Adobe experiência Manager tem de ser estabelecida.

No Gestor de experiência do Adobe, atribua o valor do **nome de utilizador** no Azure AD como o valor a **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD-início de sessão único com o Adobe experiência Manager, tem de concluir os blocos modulares seguintes:

1. **[Configurar o Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)**  - para permitir aos utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD-início de sessão único com Britta Simon.
3. **[Criar um utilizador de teste do Gestor de experiência de Adobe](#create-an-adobe-experience-manager-test-user)**  - para ter um homólogo de Britta Simon no Adobe experiência Manager que está ligada a representação do Azure AD do utilizador.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar Britta Simon utilizar o Azure AD-início de sessão único.
5. **[Teste o início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD-início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão no portal do Azure e configurar o início de sessão único na sua aplicação Adobe experiência Manager.

**Para configurar do Azure AD-início de sessão único com o Adobe experiência Manager, execute os seguintes passos:**

1. No portal do Azure, no **Adobe experiência Manager** página de integração de aplicações, clique em **de sessão único-**.

    ![Configurar a ligação de início de sessão único][4]

2. No **de sessão único-** caixa de diálogo, selecione **modo** como **baseados em SAML início de sessão** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_samlbase.png)

3. No **URLs e de domínio do Gestor de experiência de Adobe** secção, execute os seguintes passos para configurar a aplicação no **IdP** modo:

    ![Domínio do Gestor de experiência de Adobe e URLs único informações de início de sessão](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_url1.png)

    a. No **identificador** caixa de texto, escreva um valor exclusivo que definem no servidor de AEM. 

    b. No **URL de resposta** caixa de texto, escreva um URL a utilizar o padrão do seguinte:`https://<AEM Server Url>/saml_login`

    > [!NOTE] 
    > Estes valores não estiverem reais. Atualize estes valores com o identificador e o URL de resposta real. Contacte [equipa de suporte do Gestor de experiência de Adobe](https://helpx.adobe.com/support/experience-manager.html) para obter estes valores.
 
4. Verifique a mostrar avançadas definições de URL e executar o passo seguinte, se pretender configurar a aplicação no **SP** iniciada modo:

    ![Domínio do Gestor de experiência de Adobe e URLs único informações de início de sessão](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_spconfigure.png)

    No **URL de início de sessão** caixa de texto, escreva o URL do servidor de Gestor do Adobe experiência. 

5. No **certificado de assinatura de SAML** secção, clique em **certificado (Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![A hiperligação de transferência do certificado](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_certificate.png) 

6. Na secção de configuração do Gestor de experiência de Adobe, clique em configurar o Adobe experiência Manager para abrir a janela de início de sessão de configurar. Copiar o **início de sessão no URL do serviço SAML**, **ID de entidade de SAML** e **Sign-Out ID** da secção de referência rápida.

    ![Ligação da secção de configuração](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_configure.png) 

7. Clique em **guardar** botão.

    ![Configurar botão único início de sessão guardar](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_general_400.png)

8. Abra **Adobe experiência Manager** portal de administrador na outra janela do browser.

9. Selecione **definições** -> **segurança** -> **utilizadores**.

    ![Configurar botão único início de sessão guardar](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_user.png)

10. Selecione **administrador** ou qualquer outro utilizador relevante.

    ![Configurar botão único início de sessão guardar](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin6.png)

11. Selecione **definições da conta** -> **criar/gerir TrustStore**.

    ![Configurar botão único início de sessão guardar](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_managetrust.png)

12. Clique em **selecionar ficheiro de certificado** de **adicionar certificado a partir do ficheiro CER** botão. Procurar e selecionar o ficheiro de certificado, o que transferiu a partir do portal do Azure.

    ![Configurar botão único início de sessão guardar](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_user2.png)

13. O certificado foi adicionado para o TrustStore. Tenha em atenção o alias do certificado.

    ![Configurar botão único início de sessão guardar](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin7.png)

14. No **utilizadores** página, selecione **serviço de autenticação**.

    ![Configurar botão único início de sessão guardar](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin8.png)

15. Selecione **definições da conta** -> **criar/gerir KeyStore**. Crie KeyStore, fornecendo uma palavra-passe.

    ![Configurar botão único início de sessão guardar](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin9.png)

16. Volte ao ecrã de administrador e selecione **definições** -> **operações** -> **consola Web**.

    ![Configurar botão único início de sessão guardar](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin1.png)

17. Esta ação abre a página de configuração.

    ![Configurar botão único início de sessão guardar](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin2.png)

18. Localizar **Adobe Granite SAML 2.0 autenticação processador** e clique em **adicionar** ícone.

    ![Configurar botão único início de sessão guardar](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin3.png)

19. Execute os seguintes ações nesta página.

    ![Configurar botão único início de sessão guardar](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin4.png)

    a. No **caminho** caixa de texto, introduza  **/** .

    b. No **IDP URL** caixa de texto, introduza o valor de **início de sessão no URL do serviço SAML**, que copiou do Portal do Azure.

    c. No **IDP certificado Alias** caixa de texto, introduza o valor de **certificado Alias**, que adicionou no TrustStore.

    d. No **segurança fornecido ID de entidade** caixa de texto, introduza o valor de exclusivo **ID de entidade de SAML**, que configurou no Portal do Azure.

    e. No **asserção URL do serviço de consumidor** caixa de texto, introduza o valor de **URL de resposta**, que configurou no Portal do Azure.

    f. No **palavra-passe de chave de arquivo** caixa de texto, introduza o **palavra-passe**, que definiu na KeyStore.

    g. No **ID de atributo de utilizador** caixa de texto, introduza **ID de nome** ou outro ID de utilizador que é relevante no seu caso.

    h. Selecione **Autocreate CRX utilizadores.**

    posso. No **URL de fim de sessão** caixa de texto, introduza o valor de exclusivo **Sign-Out URL** que têm a partir do Portal do Azure.

    j. Clique em **guardar**

> [!TIP]
> Pode agora ler estas instruções dentro de uma versão concisa o [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir do **do Active Directory > aplicações da empresa** secção, basta clicar no **Single Sign-On** separador e aceder à documentação do embedded através de **configuração** secção na parte inferior. Pode ler mais sobre a funcionalidade de documentação incorporados aqui: [do Azure AD incorporado documentação]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção consiste em criar um utilizador de teste no portal do Azure chamado Britta Simon.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique em de **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/active-directory-saas-adobeexperiencemanager-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e "Todos os utilizadores" ligações](./media/active-directory-saas-adobeexperiencemanager-tutorial/create_aaduser_02.png)

3. Para abrir o **utilizador** caixa de diálogo, clique em **adicionar** na parte superior do **todos os utilizadores** caixa de diálogo.

    ![O botão de adição](./media/active-directory-saas-adobeexperiencemanager-tutorial/create_aaduser_03.png)

4. No **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/active-directory-saas-adobeexperiencemanager-tutorial/create_aaduser_04.png)

    a. No **nome** caixa, escreva **BrittaSimon**.

    b. No **nome de utilizador** caixa, escreva o endereço de e-mail do utilizador Britta Simon.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado no **palavra-passe** caixa.

    d. Clique em **Criar**.
  
### <a name="create-an-adobe-experience-manager-test-user"></a>Criar um utilizador de teste do Gestor de experiência de Adobe

Nesta secção, vai criar um utilizador chamado Britta Simon no Adobe experiência Manager. Se tiver selecionado **Autocreate CRX utilizadores** opção, em seguida, os utilizadores serão criados automaticamente após a autenticação com êxito. 

Se pretender criar utilizadores manualmente,. trabalhar com [equipa de suporte do Gestor de experiência de Adobe](https://helpx.adobe.com/support/experience-manager.html) para adicionar os utilizadores na plataforma Adobe experiência Manager. 

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Britta Simon utilizar o Azure-início de sessão único, concedendo acesso ao Gestor de experiência do Adobe.

![Atribuir a função de utilizador][200] 

**Para atribuir Britta Simon no Adobe experiência Manager, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicações e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todas as aplicações**.

    ![Atribua o utilizador][201] 

2. Na lista de aplicações, selecione **Adobe experiência Manager**.

    ![A ligação do Gestor de experiência de Adobe na lista de aplicações](./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação de "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Britta Simon** na lista utilizadores.

6. Clique em **selecione** botão no **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribuir** botão no **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Teste o início de sessão único

Nesta secção, testar a configuração do Azure AD único início de sessão através do painel de acesso.

Quando clica no mosaico do Gestor de experiência de Adobe no painel de acesso, deve obter automaticamente com sessão iniciada para a aplicação Adobe experiência Manager.
Para mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](active-directory-saas-tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-adobeexperiencemanager-tutorial/tutorial_general_203.png

