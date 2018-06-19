---
title: 'Tutorial: Integração do Azure Active Directory com o Adobe experiência Manager | Microsoft Docs'
description: Saiba como configurar o início de sessão entre o Azure Active Directory e Adobe experiência Manager.
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
ms.openlocfilehash: 4904d9b0fc0746bc915477bb0ac83a0083ca198f
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/18/2018
ms.locfileid: "35954378"
---
# <a name="tutorial-azure-active-directory-integration-with-adobe-experience-manager"></a>Tutorial: Integração do Azure Active Directory com o Adobe experiência Manager

Neste tutorial, irá aprender a integrar o Adobe experiência gestor no Azure Active Directory (Azure AD).

Integrar o Adobe experiência Manager com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Adobe experiência Manager.
- Pode permitir aos utilizadores obter inicie automaticamente ao Gestor de experiência do Adobe com as respetivas contas do Azure AD.
- Pode gerir as contas numa única localização central, o portal do Azure.

Para obter mais informações sobre a integração de aplicações SaaS com o Azure AD, consulte [que é o acesso a aplicações e início de sessão no Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o Adobe experiência Manager, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Gestor de experiência de Adobe-início de sessão único ativada subscrição

> [!NOTE]
> Não recomendamos a utilização de um ambiente de produção para testar os passos neste tutorial.

Para testar os passos neste tutorial, siga estas recomendações:

- Não utilize o seu ambiente de produção, exceto se for necessário.
- Se não tiver um ambiente de avaliação do Azure AD, [obtenha uma avaliação gratuita de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, teste do Azure AD-início de sessão único num ambiente de teste. O cenário é descrito neste tutorial consiste em dois blocos modulares principais:

1. Adicionar Gestor de experiência de Adobe na galeria do
2. Configurar e testar o Azure AD de sessão único-

## <a name="add-adobe-experience-manager-from-the-gallery"></a>Adicionar Gestor de experiência de Adobe a partir da Galeria
Para configurar a integração do Adobe experiência Manager com o Azure AD, tem de adicionar Gestor de experiência de Adobe na Galeria à sua lista de aplicações SaaS geridas.

**Para adicionar o Gestor de experiência de Adobe na galeria do, siga os passos seguintes:**

1. No [portal do Azure](https://portal.azure.com), no painel esquerdo, selecione o **do Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Aceda a **aplicações empresariais**. Em seguida, aceda a **todas as aplicações**.

    ![O painel de aplicações da empresa][2]
    
3. Para adicionar uma nova aplicação, selecione o **nova aplicação** botão no topo da caixa de diálogo.

    ![O novo botão de aplicação][3]

4. Na caixa de pesquisa, escreva **Adobe experiência Manager**. Selecione **Adobe experiência Manager** no painel de resultados e, em seguida, selecione o **adicionar** botão para adicionar a aplicação.

    ![Gestor de experiência de Adobe na lista de resultados](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD-início de sessão único

Nesta secção, configure e teste do Azure AD-início de sessão único com o Adobe experiência Manager com base num utilizador de teste chamado "Britta Simon."

Para início de sessão trabalhar, do Azure AD tem de saber que o utilizador homólogo no Adobe experiência Manager for para um utilizador no Azure AD. Por outras palavras, tem de estabelecer uma ligação entre um utilizador do Azure AD e o utilizador relacionado no Adobe experiência Manager.

No Gestor de experiência do Adobe, atribua o valor **Username** o mesmo valor da **nome de utilizador** no Azure AD. Agora tem de estabelecer a ligação entre dois utilizadores. 

Para configurar e testar o Azure AD-início de sessão único com o Adobe experiência Manager, execute os seguintes blocos de criação:

1. [Configurar o Azure AD-início de sessão único](#configure-azure-ad-single-sign-on) para permitir aos utilizadores utilizar esta funcionalidade.
2. [Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user) para testar do Azure AD-início de sessão único com Britta Simon.
3. [Criar um utilizador de teste do Gestor de experiência de Adobe](#create-an-adobe-experience-manager-test-user) ter um homólogo de Britta Simon no Adobe experiência Manager que está ligada a representação do Azure AD do utilizador.
4. [Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user) para ativar Britta Simon utilizar o Azure AD-início de sessão único.
5. [Teste o início de sessão único](#test-single-sign-on) para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD-início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão no portal do Azure e configurar o início de sessão único na sua aplicação Adobe experiência Manager.

**Para configurar do Azure AD-início de sessão único com o Adobe experiência Manager, siga os passos seguintes:**

1. No portal do Azure, no **Adobe experiência Manager** página de integração de aplicações, selecione **de sessão único-**.

    ![Configurar a ligação de início de sessão único][4]

2. Para ativar o início de sessão, no **de sessão único-** caixa de diálogo a **modo** menu pendente, selecione **baseados em SAML início de sessão**.
 
    ![Caixa de diálogo de início de sessão único](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_samlbase.png)

3. No **URLs e de domínio do Gestor de experiência de Adobe** secção, siga os passos seguintes se pretender configurar a aplicação no **IdP** modo:

    ![Domínio do Gestor de experiência de Adobe e URLs único informações de início de sessão](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_url1.png)

    a. No **identificador** caixa, escreva um valor exclusivo que definem no servidor de AEM. 

    b. No **URL de resposta** caixa, escreva um URL com o padrão seguinte: `https://<AEM Server Url>/saml_login`.

    > [!NOTE] 
    > Estes valores não estiverem reais. Atualizar estes valores com o identificador real e o URL de resposta. Para obter estes valores, contacte o [equipa de suporte do Gestor de experiência de Adobe](https://helpx.adobe.com/support/experience-manager.html).
 
4. Verifique **Mostrar avançadas definições de URL**. Em seguida, siga os passos seguintes se quiser configurar a aplicação no **SP** iniciada modo:

    ![Domínio do Gestor de experiência de Adobe e URLs único informações de início de sessão](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_spconfigure.png)

    No **URL de início de sessão** caixa, escreva o URL do servidor de Gestor do Adobe experiência. 

5. No **certificado de assinatura de SAML** secção, selecione **certificado (Base64)**. Em seguida, guarde o ficheiro de certificado no seu computador.

    ![A hiperligação de transferência do certificado](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_certificate.png) 

6. Para abrir a janela de início de sessão configuração na secção de configuração do Gestor de experiência de Adobe, selecione **configurar gestor do Adobe experiência**. Copiar o **início de sessão no URL do serviço SAML**, **ID de entidade de SAML**, e **Sign-Out ID** da secção de referência rápida.

    ![Ligação da secção de configuração](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_configure.png) 

7. Selecione **Guardar**.

    ![Configurar o início de sessão de gravação botão](./media/adobeexperiencemanager-tutorial/tutorial_general_400.png)

8. Na outra janela do browser, abra o **Adobe experiência Manager** portal de administração.

9. Selecione **definições** > **segurança** > **utilizadores**.

    ![Configurar o início de sessão de gravação botão](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_user.png)

10. Selecione **administrador** ou qualquer outro utilizador relevante.

    ![Configurar botão único início de sessão guardar](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin6.png)

11. Selecione **definições da conta** > **gerir TrustStore**.

    ![Configurar botão único início de sessão guardar](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_managetrust.png)

12. Em **adicionar certificado a partir do ficheiro CER**, clique em **selecionar ficheiro de certificado**. Procure e selecione o ficheiro de certificado, o que já transferiu a partir do portal do Azure.

    ![Configurar o início de sessão de gravação botão](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_user2.png)

13. O certificado foi adicionado para o TrustStore. Tenha em atenção o alias do certificado.

    ![Configurar botão único início de sessão guardar](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin7.png)

14. No **utilizadores** página, selecione **serviço de autenticação**.

    ![Configurar botão único início de sessão guardar](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin8.png)

15. Selecione **definições da conta** > **criar/gerir KeyStore**. Crie KeyStore, fornecendo uma palavra-passe.

    ![Configurar botão único início de sessão guardar](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin9.png)

16. Volte ao ecrã de admin. Em seguida, selecione **definições** > **operações** > **consola Web**.

    ![Configurar botão único início de sessão guardar](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin1.png)

    Esta ação abre a página de configuração.

    ![Configurar o início de sessão de gravação botão](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin2.png)

17. Localizar **Adobe Granite SAML 2.0 autenticação processador**. Em seguida, selecione o **adicionar** ícone.

    ![Configurar botão único início de sessão guardar](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin3.png)

19. Execute as ações seguintes nesta página.

    ![Configurar botão único início de sessão guardar](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin4.png)

    a. No **caminho** box, introduza **/**.

    b. No **IDP URL** box, introduza o **início de sessão no URL do serviço SAML** valor que copiou do portal do Azure.

    c. No **IDP certificado Alias** box, introduza o **certificado Alias** valor que adicionou no TrustStore.

    d. No **segurança fornecido ID de entidade** box, introduza o exclusivo **ID de entidade de SAML** valor que configurou no portal do Azure.

    e. No **asserção URL do serviço de consumidor** box, introduza o **URL de resposta** valor que configurou no portal do Azure.

    f. No **palavra-passe de chave de arquivo** box, introduza o **palavra-passe** definida no KeyStore.

    g. No **ID de atributo de utilizador** box, introduza o **ID de nome** ou outro ID de utilizador que é relevante no seu caso.

    h. Selecione **Autocreate CRX utilizadores**.

    i. No **URL de fim de sessão** box, introduza o exclusivo **Sign-Out URL** valor que recebeu do portal do Azure.

    j. Selecione **Guardar**.

> [!TIP]
> Pode agora ler estas instruções dentro de uma versão concisa o [portal do Azure](https://portal.azure.com) enquanto estiver a configurar a aplicação. Depois de adicionar esta aplicação a partir de **do Active Directory** > **aplicações empresariais** secção, selecione o **Single Sign-On** separador. Em seguida, aceder a documentação incorporada através de **configuração** secção na parte inferior. Pode ler mais sobre a funcionalidade de documentação incorporados em [do Azure AD incorporado documentação]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção consiste em criar um utilizador de teste no portal do Azure chamado Britta Simon.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, siga os passos seguintes:**

1. No portal do Azure, no painel esquerdo, selecione o **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/adobeexperiencemanager-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, selecione **todos os utilizadores**.

    !["Os utilizadores e grupos" e "Todos os utilizadores" ligações](./media/adobeexperiencemanager-tutorial/create_aaduser_02.png)

3. Para abrir o **utilizador** caixa de diálogo, na parte superior do **todos os utilizadores** caixa de diálogo, selecione **adicionar**.

    ![O botão de adição](./media/adobeexperiencemanager-tutorial/create_aaduser_03.png)

4. No **utilizador** diálogo caixa, siga os passos seguintes:

    ![A caixa de diálogo de utilizador](./media/adobeexperiencemanager-tutorial/create_aaduser_04.png)

    a. No **nome** caixa, escreva **BrittaSimon**.

    b. No **nome de utilizador** caixa, escreva o endereço de e-mail do utilizador Britta Simon.

    c. Selecione o **mostrar palavra-passe** caixa de verificação. Em seguida, anote o valor que é apresentado no **palavra-passe** caixa.

    d. Selecione **Criar**.
  
### <a name="create-an-adobe-experience-manager-test-user"></a>Criar um utilizador de teste do Gestor de experiência de Adobe

Nesta secção, vai criar um utilizador chamado Britta Simon no Adobe experiência Manager. Se tiver selecionado o **Autocreate CRX utilizadores** opção, os utilizadores são criados automaticamente após a autenticação com êxito. 

Se pretender criar utilizadores manualmente, trabalhar com o [equipa de suporte do Gestor de experiência de Adobe](https://helpx.adobe.com/support/experience-manager.html) para adicionar os utilizadores na plataforma Adobe experiência Manager. 

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Britta Simon utilizar o Azure-início de sessão único, concedendo-lhes acesso ao Gestor de experiência do Adobe.

![Atribuir a função de utilizador][200] 

**Para atribuir Britta Simon ao Gestor de experiência do Adobe, siga os passos seguintes:**

1. No portal do Azure, abra a vista de aplicações. Em seguida, vá para a vista de diretório, selecione **aplicações empresariais**e, em seguida, selecione **todas as aplicações**.

    ![Atribua o utilizador][201] 

2. Na lista de aplicações, selecione **Adobe experiência Manager**.

    ![A ligação do Gestor de experiência de Adobe na lista de aplicações](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_app.png)  

3. No menu à esquerda, selecione **utilizadores e grupos**.

    ![A ligação de "Utilizadores e grupos"][202]

4. Selecione o **adicionar** botão. Em seguida, no **adicionar atribuição** caixa de diálogo, selecione **utilizadores e grupos**.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Britta Simon** na lista utilizadores.

6. No **utilizadores e grupos** caixa de diálogo, clique em de **selecione** botão.

7. No **adicionar atribuição** caixa de diálogo, selecione o **atribuir** botão.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, teste configuração do Azure AD único início de sessão utilizando o painel de acesso.

Quando seleciona o mosaico do Gestor de experiência de Adobe no painel de acesso, que deve obter automaticamente sessão na aplicação Adobe experiência Manager.

Para mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS no Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/adobeexperiencemanager-tutorial/tutorial_general_01.png
[2]: ./media/adobeexperiencemanager-tutorial/tutorial_general_02.png
[3]: ./media/adobeexperiencemanager-tutorial/tutorial_general_03.png
[4]: ./media/adobeexperiencemanager-tutorial/tutorial_general_04.png

[100]: ./media/adobeexperiencemanager-tutorial/tutorial_general_100.png

[200]: ./media/adobeexperiencemanager-tutorial/tutorial_general_200.png
[201]: ./media/adobeexperiencemanager-tutorial/tutorial_general_201.png
[202]: ./media/adobeexperiencemanager-tutorial/tutorial_general_202.png
[203]: ./media/adobeexperiencemanager-tutorial/tutorial_general_203.png

