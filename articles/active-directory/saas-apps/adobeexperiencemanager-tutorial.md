---
title: 'Tutorial: Integração do Azure Active Directory com o Adobe Experience Manager | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e o Adobe Experience Manager.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 88a95bb5-c17c-474f-bb92-1f80f5344b5a
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: jeedes
ms.openlocfilehash: 56b392e57809cea0ae93800df39bb9dacd164ce2
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39054187"
---
# <a name="tutorial-azure-active-directory-integration-with-adobe-experience-manager"></a>Tutorial: Integração do Azure Active Directory com o Adobe Experience Manager

Neste tutorial, saiba como integrar o Adobe Experience Manager com o Azure Active Directory (Azure AD).

Integrar o Adobe Experience Manager no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso para o Adobe Experience Manager.
- Pode permitir que os utilizadores automaticamente é feita Adobe Experience Manager com as suas contas do Azure AD.
- Pode gerir as suas contas num local central, o portal do Azure.

Para obter mais informações sobre a integração de aplicações SaaS com o Azure AD, consulte [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o Adobe Experience Manager, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Adobe Experience Manager logon único habilitado subscrição

> [!NOTE]
> Não é recomendado utilizar um ambiente de produção para testar os passos neste tutorial.

Para testar os passos neste tutorial, siga as seguintes recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, [obtenha uma avaliação gratuita de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário que é descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionar o Adobe Experience Manager a partir da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="add-adobe-experience-manager-from-the-gallery"></a>Adicionar o Adobe Experience Manager a partir da Galeria
Para configurar a integração do Adobe Experience Manager com o Azure AD, terá de adicionar o Adobe Experience Manager a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar o Adobe Experience Manager a partir da galeria, siga os passos seguintes:**

1. Na [portal do Azure](https://portal.azure.com), no painel esquerdo, selecione a **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Aceda a **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
3. Para adicionar uma nova aplicação, selecione o **nova aplicação** botão na parte superior da caixa de diálogo.

    ![O novo botão de aplicativo][3]

4. Na caixa de pesquisa, escreva **Adobe Experience Manager**. Selecione **Adobe Experience Manager** no painel de resultados e, em seguida, selecione a **Add** botão para adicionar a aplicação.

    ![O Adobe Experience Manager na lista de resultados](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com o Adobe Experience Manager com base num utilizador de teste chamado "Eduarda Almeida."

Para o início de sessão único funcione, o Azure AD precisa saber que o utilizador de contraparte no Adobe Experience Manager for para um utilizador no Azure AD. Em outras palavras, tem de estabelecer uma ligação entre um utilizador do Azure AD e o utilizador relacionado no Adobe Experience Manager.

No Adobe Experience Manager, atribua o valor **nome de utilizador** o mesmo valor da **nome de utilizador** no Azure AD. Agora que estabeleceu a ligação entre os dois usuários. 

Para configurar e testar o Azure AD início de sessão único com o Adobe Experience Manager, execute os seguintes blocos de construção:

1. [Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on) para permitir aos utilizadores utilizar esta funcionalidade.
2. [Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user) para testar o Azure AD início de sessão único com Eduarda Almeida.
3. [Criar um utilizador de teste do Adobe Experience Manager](#create-an-adobe-experience-manager-test-user) ter um equivalente da Eduarda Almeida no Adobe Experience Manager que está ligado à representação de utilizador do Azure AD.
4. [Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user) para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. [Testar início de sessão único](#test-single-sign-on) para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único na sua aplicação do Adobe Experience Manager.

**Para configurar o Azure AD início de sessão único com o Adobe Experience Manager, siga os passos seguintes:**

1. No portal do Azure, sobre o **Adobe Experience Manager** página de integração de aplicações, selecione **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Para ativar o início de sessão único, na **início de sessão único** caixa de diálogo a **modo** menu pendente, selecione **baseado em SAML logon**.
 
    ![Caixa de diálogo de início de sessão único](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_samlbase.png)

3. Na **Adobe Experience Manager domínio e URLs** secção, siga os passos seguintes para configurar a aplicação na **IdP** modo:

    ![O Adobe Experience Manager domínio e URLs únicas início de sessão em informações](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_url1.png)

    a. Na **identificador** , escreva um valor exclusivo que definir no servidor do AEM. 

    b. Na **URL de resposta** caixa, escreva um URL com o seguinte padrão: `https://<AEM Server Url>/saml_login`.

    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o identificador real e o URL de resposta. Para obter estes valores, entre em contato com o [equipa de suporte do Adobe Experience Manager](https://helpx.adobe.com/support/experience-manager.html).
 
4. Verifique **Mostrar definições de URL avançadas**. Em seguida, siga os passos seguintes para configurar a aplicação no **SP** iniciada pelo modo:

    ![O Adobe Experience Manager domínio e URLs únicas início de sessão em informações](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_spconfigure.png)

    Na **URL de início de sessão** caixa, escreva o URL do servidor Adobe Experience Manager. 

5. Na **certificado de assinatura SAML** secção, selecione **certificado (Base64)**. Em seguida, guarde o ficheiro de certificado no seu computador.

    ![O link de download de certificado](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_certificate.png) 

6. Para abrir a janela de início de sessão configuration na seção de configuração do Adobe Experience Manager, selecione **configurar o Adobe Experience Manager**. Copiar o **SAML início de sessão no URL do serviço**, **ID de entidade de SAML**, e **ID de fim de sessão** da seção de referência rápida.

    ![Ligação da secção de configuração](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_configure.png) 

7. Selecione **Guardar**.

    ![Configurar o início de sessão único botão save](./media/adobeexperiencemanager-tutorial/tutorial_general_400.png)

8. Na outra janela do browser, abra a **Adobe Experience Manager** portal de administração.

9. Selecione **configurações** > **segurança** > **utilizadores**.

    ![Configurar o início de sessão único botão save](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_user.png)

10. Selecione **administrador** ou qualquer outro utilizador relevante.

    ![Configurar o botão único início de sessão em Guardar](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin6.png)

11. Selecione **definições da conta** > **gerir TrustStore**.

    ![Configurar o botão único início de sessão em Guardar](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_managetrust.png)

12. Sob **adicionar certificados a partir do ficheiro CER**, clique em **selecionar ficheiro de certificado**. Procure e selecione o ficheiro de certificado, que já transferiu a partir do portal do Azure.

    ![Configurar o início de sessão único botão save](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_user2.png)

13. O certificado é adicionado ao TrustStore. Tenha em atenção o alias do certificado.

    ![Configurar o botão único início de sessão em Guardar](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin7.png)

14. Sobre o **usuários** página, selecione **serviço de autenticação**.

    ![Configurar o botão único início de sessão em Guardar](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin8.png)

15. Selecione **definições da conta** > **KeyStore de criar/gerir**. Crie KeyStore ao fornecer uma palavra-passe.

    ![Configurar o botão único início de sessão em Guardar](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin9.png)

16. Volte ao ecrã de administrador. Em seguida, selecione **configurações** > **operações** > **consola Web**.

    ![Configurar o botão único início de sessão em Guardar](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin1.png)

    Esta ação abre a página de configuração.

    ![Configurar o início de sessão único botão save](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin2.png)

17. Encontrar **Adobe Granite SAML 2.0 autenticação manipulador**. Em seguida, selecione o **adicionar** ícone.

    ![Configurar o botão único início de sessão em Guardar](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin3.png)

19. Efetue as seguintes ações nesta página.

    ![Configurar o botão único início de sessão em Guardar](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_admin4.png)

    a. Na **caminho** , introduza **/**.

    b. Na **URL de IDP** , introduza o **SAML início de sessão no URL do serviço** valor que copiou do portal do Azure.

    c. Na **Alias de certificado do IDP** , introduza o **Alias de certificado** valor que adicionou no TrustStore.

    d. Na **segurança fornecido ID de entidade** , introduza o exclusivo **ID de entidade de SAML** valor que configurou no portal do Azure.

    e. Na **URL do serviço de consumidor de asserção** , introduza o **URL de resposta** valor que configurou no portal do Azure.

    f. Na **Store de palavra-passe de chave** , introduza o **palavra-passe** definida no KeyStore.

    g. Na **ID de atributo do utilizador** , introduza o **ID de nome** ou outro ID de utilizador que seja relevante no seu caso.

    h. Selecione **Autocreate CRX utilizadores**.

    i. Na **URL de fim de sessão** , introduza o exclusivo **URL de fim de sessão** valor que recebeu do portal do Azure.

    j. Selecione **Guardar**.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com) enquanto estiver a configurar a aplicação. Depois de adicionar esta aplicação a partir da **do Active Directory** > **aplicações empresariais** secção, selecione o **Single Sign-On** separador. Em seguida, aceder a documentação do embedded através do **configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded em [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, siga os passos seguintes:**

1. No portal do Azure, no painel esquerdo, selecione o **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/adobeexperiencemanager-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, selecione **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/adobeexperiencemanager-tutorial/create_aaduser_02.png)

3. Para abrir o **usuário** caixa de diálogo, na parte superior a **todos os utilizadores** caixa de diálogo, selecione **Add**.

    ![Botão Adicionar](./media/adobeexperiencemanager-tutorial/create_aaduser_03.png)

4. Na **utilizador** diálogo caixa, siga os passos seguintes:

    ![A caixa de diálogo de utilizador](./media/adobeexperiencemanager-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação. Em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Selecione **Criar**.
  
### <a name="create-an-adobe-experience-manager-test-user"></a>Criar um utilizador de teste do Adobe Experience Manager

Nesta secção, vai criar um usuário chamado Eduarda Almeida no Adobe Experience Manager. Se tiver selecionado o **Autocreate CRX utilizadores** opção, os utilizadores são criados automaticamente após a autenticação com êxito. 

Se quiser criar manualmente os utilizadores, trabalhar com o [equipa de suporte do Adobe Experience Manager](https://helpx.adobe.com/support/experience-manager.html) para adicionar os utilizadores na plataforma do Adobe Experience Manager. 

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único, concedendo-lhes acesso para o Adobe Experience Manager.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida para o Adobe Experience Manager, siga os passos seguintes:**

1. No portal do Azure, abra a vista de aplicações. Em seguida, vá para a vista de diretório, selecione **aplicações empresariais**e, em seguida, selecione **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **Adobe Experience Manager**.

    ![A ligação do Adobe Experience Manager na lista de aplicações](./media/adobeexperiencemanager-tutorial/tutorial_adobeexperiencemanager_app.png)  

3. No menu à esquerda, selecione **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

4. Selecione o **adicionar** botão. Em seguida, na **adicionar atribuição** caixa de diálogo, selecione **utilizadores e grupos**.

    ![O painel Adicionar atribuição][203]

5. Na **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Na **utilizadores e grupos** caixa de diálogo, clique nas **selecione** botão.

7. Na **adicionar atribuição** caixa de diálogo, selecione a **atribuir** botão.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, vai testar seu única início de sessão em configuração do Azure AD utilizando o painel de acesso.

Ao selecionar o mosaico do Adobe Experience Manager no painel de acesso, deve obter automaticamente a sessão iniciada sua aplicação do Adobe Experience Manager.

Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



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

