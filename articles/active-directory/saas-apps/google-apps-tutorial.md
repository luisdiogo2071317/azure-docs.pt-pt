---
title: 'Tutorial: Integração do Azure Active Directory com G Suite | Microsoft Docs'
description: Saiba como configurar o início de sessão entre o Azure Active Directory e G Suite.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 38a6ca75-7fd0-4cdc-9b9f-fae080c5a016
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/02/2018
ms.author: jeedes
ms.openlocfilehash: f3a59a6a9cc004ade972fd0ea86c5d8de3c9d562
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/18/2018
ms.locfileid: "35955881"
---
# <a name="tutorial-azure-active-directory-integration-with-g-suite"></a>Tutorial: Integração do Azure Active Directory com G Suite

Neste tutorial, irá aprender a integrar G Suite com o Azure Active Directory (Azure AD).

Integrar G Suite com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao G Suite.
- Pode permitir aos utilizadores automaticamente obter com sessão iniciada para Suite G (Single Sign-On) com as respetivas contas do Azure AD.
- Pode gerir as contas numa localização central - portal do Azure.

Se pretender saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [que é o acesso a aplicações e início de sessão no Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Suite de G, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um G Suite-início de sessão único ativada subscrição
- Uma subscrição do Google Apps ou a subscrição do Google Cloud Platform.

> [!NOTE]
> Para testar os passos neste tutorial, não recomendamos a utilização num ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="frequently-asked-questions"></a>Perguntas Mais Frequentes
1.  **P: funciona este suporte de integração integração de SSO de plataforma do Google Cloud com o Azure AD?**
    
    R: Sim. Google Cloud Platform e o Google Apps partilham a mesma plataforma de autenticação. Para efetuar a integração de GCP terá de configurar o SSO com o Google Apps.


2. **P: são Chromebooks e outros dispositivos Chrome compatíveis com o Azure AD-início de sessão único?**
   
    R: Sim, os utilizadores conseguem iniciar sessão em dispositivos Chromebook utilizando as credenciais do Azure AD. Consulte este [artigo de suporte de conjunto de G](https://support.google.com/chrome/a/answer/6060880) para obter informações sobre o motivo os utilizadores podem obter pedidos as credenciais duas vezes.

3. **P: Se ativar a início de sessão único, os utilizadores poderão utilizar as respetivas credenciais do Azure AD para iniciar sessão em qualquer produto da Google, tais como o Google sala de aula, GMail, Google Drive, YouTube e assim sucessivamente?**
   
    R: Sim, dependendo da [que Suite G](https://support.google.com/a/answer/182442?hl=en&ref_topic=1227583) optar por ativar ou desativar para a sua organização.

4. **P: posso ativar início de sessão para apenas um subconjunto de utilizadores G Suite?**
   
    R: não, ativar o início de sessão único imediatamente requer que todos os utilizadores de G Suite autenticar com as respetivas credenciais do Azure AD. Porque G Suite não suporta a ter vários fornecedores de identidade, o fornecedor de identidade para o seu ambiente de G Suite pode ser do Azure AD ou Google - mas não ambos simultaneamente.

5. **P: se um utilizador é iniciada no através do Windows, são autenticam automaticamente para G Suite sem obter um pedido de uma palavra-passe?**
   
    R: Existem duas opções para ativar este cenário. Em primeiro lugar, os utilizadores foi inicie sessão em dispositivos Windows 10 através de [associação do Azure Active Directory](../device-management-introduction.md). Em alternativa, os utilizadores foi inicie sessão em dispositivos Windows que estão associados a um domínio para um diretório de Active Directory no local que foi ativada para o início de sessão para o Azure AD através de um [serviços de Federação do Active Directory (AD FS)](../connect/active-directory-aadconnect-user-signin.md) implementação. Ambas as opções tem de efetuar os passos do tutorial seguinte para ativar o início de sessão entre o Azure AD e G Suite.

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, teste do Azure AD-início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos modulares principais:

1. A adição de G Suite de galeria
2. Configurar e testar o Azure AD de sessão único-

## <a name="adding-g-suite-from-the-gallery"></a>A adição de G Suite de galeria
Para configurar a integração de G Suite com o Azure AD, tem de adicionar G Suite na Galeria à sua lista de aplicações SaaS geridas.

**Para adicionar G Suite a partir da galeria, execute os seguintes passos:**

1. No  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **do Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todas as aplicações**.

    ![O painel de aplicações da empresa][2]
    
3. Para adicionar a nova aplicação, clique em **nova aplicação** botão no topo da caixa de diálogo.

    ![O novo botão de aplicação][3]

4. Na caixa de pesquisa, escreva **G Suite**, selecione **G Suite** partir do painel de resultados, em seguida, clique em **adicionar** botão para adicionar a aplicação.

    ![G Suite na lista de resultados](./media/google-apps-tutorial/tutorial_googleapps_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD-início de sessão único

Nesta secção, configure e teste do Azure AD-início de sessão único com Suite G com base num utilizador de teste chamado "Britta Simon".

Para início de sessão trabalhar, do Azure AD tem de saber o que o utilizador homólogo no G Suite é um utilizador no Azure AD. Por outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no G Suite tem de ser estabelecida.

No conjunto de G, atribua o valor do **nome de utilizador** no Azure AD como o valor a **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD-início de sessão único com o conjunto de G, tem de concluir os blocos modulares seguintes:

1. **[Configurar o Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)**  - para permitir aos utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD-início de sessão único com Britta Simon.
3. **[Criar um utilizador de teste de G Suite](#create-a-g-suite-test-user)**  - para ter um homólogo de Britta Simon Suite G que está ligada a representação do Azure AD do utilizador.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar Britta Simon utilizar o Azure AD-início de sessão único.
5. **[Teste o início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD-início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão no portal do Azure e configurar o início de sessão único na sua aplicação G Suite.

**Para configurar o Azure AD-início de sessão único com Suite de G, execute os seguintes passos:**

1. No portal do Azure, no **G Suite** página de integração de aplicações, clique em **de sessão único-**.

    ![Configurar a ligação de início de sessão único][4]

2. No **de sessão único-** caixa de diálogo, selecione **modo** como **baseados em SAML início de sessão** para ativar o início de sessão único.

    ![Caixa de diálogo de início de sessão único](./media/google-apps-tutorial/tutorial_googleapps_samlbase.png)

3. No **G Suite domínio e os URLs** secção, se pretende configurar para o **Gmail** execute os seguintes passos:

    ![G Suite domínio e os URLs únicos de informações de início de sessão](./media/google-apps-tutorial/tutorial_googleapps_urlgmail.png)

    a. No **URL de início de sessão** caixa de texto, escreva um URL de URL utilizando o padrão do seguinte: `https://www.google.com/a/<yourdomain.com>/ServiceLogin?continue=https://mail.google.com`

    b. No **identificador** caixa de texto, escreva um URL a utilizar o padrão do seguinte: 
    | |
    |--|
    | `google.com/a/<yourdomain.com>` |
    | `google.com` |
    | `http://google.com` |
    | `http://google.com/a/<yourdomain.com>` |
 
    > [!NOTE] 
    > Estes valores não estiverem reais. Atualize estes valores com o URL de início de sessão e o identificador real. Contacte [equipa de suporte de cliente de Suite G](https://www.google.com/contact/) para obter estes valores.

4. No **G Suite domínio e os URLs** secção, se pretende configurar para o **Google Cloud Platform** execute os seguintes passos:

    ![G Suite domínio e os URLs únicos de informações de início de sessão](./media/google-apps-tutorial/tutorial_googleapps_url1.png)

    a. No **URL de início de sessão** caixa de texto, escreva um URL de URL utilizando o padrão do seguinte: `https://www.google.com/a/<yourdomain.com>/ServiceLogin?continue=https://console.cloud.google.com `

    b. No **identificador** caixa de texto, escreva um URL a utilizar o padrão do seguinte: 
    | |
    |--|
    | `google.com/a/<yourdomain.com>` |
    | `google.com` |
    | `http://google.com` |
    | `http://google.com/a/<yourdomain.com>` |
    
    > [!NOTE] 
    > Estes valores não estiverem reais. Atualize estes valores com o URL de início de sessão e o identificador real. Contacte [equipa de suporte de cliente de Suite G](https://www.google.com/contact/) para obter estes valores. 

5. No **certificado de assinatura de SAML** secção, clique em **certificado** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![A hiperligação de transferência do certificado](./media/google-apps-tutorial/tutorial_googleapps_certificate.png) 

6. Clique em **guardar** botão.

    ![Configurar botão único início de sessão guardar](./media/google-apps-tutorial/tutorial_general_400.png)

7. No **G Suite configuração** secção, clique em **configurar Suite de G** para abrir **configurar início de sessão** janela. Copiar o **Sign-Out URL, único início de sessão no URL do serviço SAML e alteração URL de palavra-passe** do **secção de referência rápida.**

    ![G Suite configuração](./media/google-apps-tutorial/tutorial_googleapps_configure.png) 

8. Abra um novo separador no seu browser e inicie sessão no [consola de administração de Suite G](http://admin.google.com/) utilizando a sua conta de administrador.

9. Clique em **segurança**. Se não vir a ligação, pode estar ocultado sob o **mais controlos** menu na parte inferior do ecrã.
   
    ![Clique em Security.][10]

10. No **segurança** página, clique em **configurar início de sessão único (SSO).**
   
    ![Clique em SSO.][11]

11. Execute as seguintes alterações de configuração:
   
    ![Configurar o SSO][12]
   
    a. Selecione **SSO de configuração com o fornecedor de identidade de terceiros**.

    b. No **URL de página de início de sessão** campo no conjunto de G, cole o valor de **URL Single Sign-On serviço** que copiou do portal do Azure.

    c. No **URL da página de início de sessão** campo no conjunto de G, cole o valor de **Sign-Out URL** que copiou do portal do Azure. 

    d. No **alterar palavra-passe URL** campo no conjunto de G, cole o valor de **alterar palavra-passe URL** que copiou do portal do Azure. 

    e. No conjunto de G, para o **certificado de verificação**, carregue o certificado que transferiu a partir do portal do Azure.

    f. Selecione **utilizar um emissor específicas do domínio**.

    g. Clique em **guardar alterações**.

> [!TIP]
> Pode agora ler estas instruções dentro de uma versão concisa o [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir do **do Active Directory > aplicações da empresa** secção, basta clicar no **Single Sign-On** separador e aceder à documentação do embedded através de **configuração** secção na parte inferior. Pode ler mais sobre a funcionalidade de documentação incorporados aqui: [do Azure AD incorporado documentação]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção consiste em criar um utilizador de teste no portal do Azure chamado Britta Simon.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique em de **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/google-apps-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e "Todos os utilizadores" ligações](./media/google-apps-tutorial/create_aaduser_02.png)

3. Para abrir o **utilizador** caixa de diálogo, clique em **adicionar** na parte superior do **todos os utilizadores** caixa de diálogo.

    ![O botão de adição](./media/google-apps-tutorial/create_aaduser_03.png)

4. No **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/google-apps-tutorial/create_aaduser_04.png)

    a. No **nome** caixa, escreva **BrittaSimon**.

    b. No **nome de utilizador** caixa, escreva o endereço de e-mail do utilizador Britta Simon.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado no **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-g-suite-test-user"></a>Criar um utilizador de teste de G Suite

O objetivo desta secção consiste em criar um utilizador chamado Britta Simon G Suite software. G Suite suporta o aprovisionamento automático, que é por predefinição ativada. Não há nenhuma ação por si nesta secção. Se um utilizador já não existe no Software de conjunto de G, uma nova é criada quando tentar aceder G Suite Software.

>[!NOTE] 
>Se precisar de criar manualmente um utilizador, contacte o [equipa de suporte do Google](https://www.google.com/contact/).

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Britta Simon utilizar o Azure-início de sessão único, concedendo acesso para G Suite.

![Atribuir a função de utilizador][200] 

**Para atribuir Britta Simon a G Suite, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicações e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todas as aplicações**.

    ![Atribua o utilizador][201] 

2. Na lista de aplicações, selecione **G Suite**.

    ![A ligação de G Suite na lista de aplicações](./media/google-apps-tutorial/tutorial_googleapps_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação de "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Britta Simon** na lista utilizadores.

6. Clique em **selecione** botão no **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribuir** botão no **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, testar a configuração do Azure AD único início de sessão através do painel de acesso.

Quando clica no mosaico de G Suite no painel de acesso, deve obter automaticamente com sessão iniciada para a aplicação de G Suite.
Para mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

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

