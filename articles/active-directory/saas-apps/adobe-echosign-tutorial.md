---
title: 'Tutorial: Integração do Azure Active Directory com início de sessão do Adobe | Microsoft Docs'
description: Saiba como configurar o início de sessão entre o Azure Active Directory e Adobe início de sessão.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: f9385723-8fe7-4340-8afb-1508dac3e92b
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2018
ms.author: jeedes
ms.openlocfilehash: 9a1a1f1a1866e5221775d583a9bafe86eef17131
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36221228"
---
# <a name="tutorial-azure-active-directory-integration-with-adobe-sign"></a>Tutorial: Integração do Azure Active Directory com início de sessão do Adobe

Neste tutorial, irá aprender a integrar o Adobe início de sessão no Azure Active Directory (Azure AD).

Integrar o Adobe início de sessão com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Adobe início de sessão.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada Adobe início de sessão (sessão-único) com as respetivas contas do Azure AD.
- Pode gerir as contas numa única localização central, o portal do Azure.

Para obter mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte [que é o acesso a aplicações e início de sessão no Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Adobe início de sessão, precisa de:

- Uma subscrição do Azure AD
- Uma sessão de Adobe-início de sessão único ativada subscrição

Para testar os passos neste tutorial, siga estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, teste do Azure AD-início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos modulares principais:

1. Adicionar início de sessão do Adobe partir da galeria.
2. Configurar e testar o Azure AD único início de sessão.

## <a name="add-adobe-sign-from-the-gallery"></a>Adicionar início de sessão do Adobe na galeria do
Para configurar a integração do Adobe início de sessão com o Azure AD, tem de adicionar início de sessão do Adobe na Galeria à sua lista de aplicações SaaS geridas.

1. No [portal do Azure](https://portal.azure.com), no painel esquerdo, selecione o **do Azure Active Directory** ícone. 

    ![Captura de ecrã do ícone do Azure Active Directory][1]

2. Navegue até à **aplicações empresariais** > **todas as aplicações**.

    ![Menus de captura de ecrã do Azure Active Directory, com aplicações da empresa e todas as aplicações realçadas][2]
    
3. Para adicionar uma nova aplicação, selecione **nova aplicação** na parte superior da caixa de diálogo.

    ![Captura de ecrã da nova opção de aplicação na parte superior da caixa de diálogo][3]

4. Na caixa de pesquisa, escreva **Adobe sessão**.

    ![Captura de ecrã da caixa de pesquisa](./media/adobe-echosign-tutorial/tutorial_adobesign_search.png)

5. No painel de resultados, selecione **Adobe sessão**e, em seguida, selecione **adicionar**.

    ![Captura de ecrã do painel de resultados](./media/adobe-echosign-tutorial/tutorial_adobesign_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD-início de sessão único
Nesta secção, configurar e testar o Azure AD-início de sessão único com início de sessão para o Adobe, com base num utilizador de teste chamado "Britta Simon."

Para início de sessão trabalhar, do Azure AD tem de reconhecer uma relação entre um utilizador do Azure AD e o utilizador relacionado no Adobe início de sessão ligada.

Para estabelecer a relação ligada, no início de sessão do Adobe, atribua o valor do **nome de utilizador** no Azure AD como o valor de **Username**.

Para configurar e testar o Azure AD-início de sessão único com Adobe início de sessão, execute os seguintes blocos de criação:

1. [Configurar o Azure AD-início de sessão único](#configuring-azure-ad-single-sign-on) para permitir aos utilizadores utilizar esta funcionalidade.
2. [Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user) para testar do Azure AD-início de sessão único com Britta Simon.
3. [Criar um utilizador de teste de início de sessão do Adobe](#creating-an-adobe-sign-test-user) para ter um homólogo de Britta Simon no Adobe início de sessão que está ligado a representação do Azure AD do utilizador.
4. [Atribua o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user) para ativar Britta Simon utilizar o Azure AD-início de sessão único.
5. [Teste o início de sessão único](#testing-single-sign-on) para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD-início de sessão único

Nesta secção, ativar o Azure AD início de sessão no portal do Azure e configurar o início de sessão único na sua aplicação Adobe início de sessão.

1. No portal do Azure, no **Adobe sessão** página de integração de aplicações, selecione **de sessão único-**.

    ![Página sessão de captura de ecrã do Adobe da integração de aplicações, com início de sessão realçado][4]

2. No **de sessão único-** caixa de diálogo, para **modo**, selecione **baseados em SAML início de sessão** para ativar o início de sessão único.

    ![Captura de ecrã de única caixa de diálogo de início de sessão, com caixa de modo realçada](./media/adobe-echosign-tutorial/tutorial_adobesign_samlbase.png)

3. No **URLs e de domínio de início de sessão do Adobe** secção, execute os seguintes passos:

    ![Secção de captura de ecrã do domínio de início de sessão Adobe e URLs](./media/adobe-echosign-tutorial/tutorial_adobesign_url.png)

    a. No **URL de início de sessão** caixa de texto, escreva um URL que utiliza o padrão do seguinte: `https://<companyname>.echosign.com/`

    b. No **identificador** caixa de texto, escreva um URL que utiliza o padrão do seguinte: `https://<companyname>.echosign.com`

    > [!NOTE] 
    > Estes valores não estiverem reais. Atualize estes valores com o URL de início de sessão real e o identificador. Contacte [equipa de suporte de cliente de início de sessão do Adobe](https://helpx.adobe.com/in/contact/support.html) para obter estes valores.

4. No **certificado de assinatura de SAML** secção, selecione **Certificate(Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![Secção de captura de ecrã de SAML certificado de assinatura](./media/adobe-echosign-tutorial/tutorial_adobesign_certificate.png) 

5. Selecione **Guardar**.

    ![Botão de captura de ecrã de guardar](./media/adobe-echosign-tutorial/tutorial_general_400.png)

6. No **configuração de sessão do Adobe** secção, selecione **configurar início de sessão do Adobe** para abrir o **configurar início de sessão** janela. Copiar o **Sign-Out URL**, **ID de entidade de SAML**, e **único início de sessão no URL do serviço SAML** do **referência rápida** secção.

    ![Secção de captura de ecrã de configuração de sessão Adobe, com configurar Adobe início de sessão realçado](./media/adobe-echosign-tutorial/tutorial_adobesign_configure.png)

7. Antes de configuração, contacte o [equipa de suporte de cliente de início de sessão do Adobe](https://helpx.adobe.com/in/contact/support.html) lista branca seu domínio no início de sessão Adobe. Eis como adicionar domínio:

    a. O [equipa de suporte de cliente de início de sessão do Adobe](https://helpx.adobe.com/in/contact/support.html) envia-lhe um token gerado aleatoriamente. Para o seu domínio, o token será semelhante ao seguinte: **verificação do início de sessão do adobe = xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx**

    b. Publicar o token de verificação num registo de texto DNS e notificar o [equipa de suporte de cliente de início de sessão do Adobe](https://helpx.adobe.com/in/contact/support.html).
    
    > [!NOTE]
    > Esta operação pode demorar alguns dias ou mais tempo. Tenha em atenção que os atrasos de propagação de DNS, significa que um valor publicado no DNS pode não estar visível para uma hora ou mais. O administrador de TI deve ser knowledgeable sobre como publicar este token num registo de texto DNS.
    
    c. Quando notificar o [equipa de suporte de cliente de início de sessão do Adobe](https://helpx.adobe.com/in/contact/support.html) através do pedido de suporte, depois do token é publicado, estes validar o domínio e adicioná-lo à sua conta.
    
    d. Geralmente, eis como publicar o token de um registo DNS:

    * Inicie sessão na sua conta de domínio
    * Localize a página para atualizar o registo DNS. Pode ser chamada nesta página Gestão de DNS, nome do servidor de gestão ou definições avançadas.
    * Localize os registos TXT para o seu domínio.
    * Adicione um registo TXT com o valor completa token fornecido pelo Adobe.
    * Guarde as alterações.

8. Numa janela do browser web diferente, inicie sessão no site da sua empresa Adobe sessão como administrador.

9. No menu de SAML, selecione **as definições da conta** > **SAML definições**.
   
    ![Captura de ecrã do Adobe sessão SAML página das definições](./media/adobe-echosign-tutorial/ic789520.png "conta")

10. No **SAML definições** secção, execute os seguintes passos:
  
    ![Captura de ecrã das definições de SAML](./media/adobe-echosign-tutorial/ic789521.png "SAML definições")
   
    a. Em **SAML modo**, selecione **SAML obrigatório**.
   
    b. Selecione **permitir Echosign conta aos administradores inicie sessão com as respetivas credenciais de Echosign**.
   
    c. Em **criação de utilizador**, selecione **adicionar automaticamente utilizadores autenticados através de SAML**.

    d. Colar **ID de entidade de SAML**, que copiou do portal do Azure para o **entidade ID/emissor URL** caixa de texto.
    
    e. Colar **único início de sessão no URL do serviço SAML**, que copiou do portal do Azure para o **ponto final do URL/SSO de início de sessão** caixa de texto.
   
    f. Colar **Sign-Out URL**, que copiou do portal do Azure para o **ponto final do URL/SLO de fim de sessão** caixa de texto.

    g. Abra o transferido **Certificate(Base64)** ficheiro no bloco de notas. Copie o conteúdo do mesmo para a sua área de transferência e, em seguida, cole-os para o **IdP certificado** caixa de texto.

    h. Selecione **guardar alterações**.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção consiste em criar um utilizador de teste, com o nome Britta Simon, no portal do Azure.

![Captura de ecrã do nome de utilizador de teste no portal do Azure][100]

1. No **portal do Azure**, no painel esquerdo, selecione o **do Azure Active Directory** ícone.

    ![Captura de ecrã do ícone do Azure AD](./media/adobe-echosign-tutorial/create_aaduser_01.png) 

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e selecione **todos os utilizadores**.
    
    ![Menus de captura de ecrã do Azure AD, com utilizadores e grupos e todos os utilizadores realçados](./media/adobe-echosign-tutorial/create_aaduser_02.png) 

3. Para abrir o **utilizador** caixa de diálogo, selecione **adicionar**.
 
    ![Captura de ecrã da parte superior todos os utilizadores da caixa de diálogo, com a opção Adicionar realçada](./media/adobe-echosign-tutorial/create_aaduser_03.png) 

4. No **utilizador** diálogo caixa, execute os seguintes passos:
 
    ![Caixa de diálogo de captura de ecrã de utilizador](./media/adobe-echosign-tutorial/create_aaduser_04.png) 

    a. No **nome** caixa de texto, escreva **BrittaSimon**.

    b. No **nome de utilizador** texto caixa, escreva o endereço de e-mail do BrittaSimon.

    c. Selecione **mostrar palavra-passe**e anote o valor da **palavra-passe**.

    d. Selecione **Criar**.
 
### <a name="create-an-adobe-sign-test-user"></a>Criar um utilizador de teste de início de sessão do Adobe

Para ativar o Azure AD aos utilizadores iniciar sessão Adobe início de sessão, têm de ser aprovisionados no Adobe início de sessão. Esta é uma tarefa manual.

>[!NOTE]
>Pode utilizar quaisquer outras ferramentas de criação de conta de utilizador de início de sessão do Adobe ou APIs fornecidas pelo início de sessão do Adobe aprovisionar contas de utilizador do Azure AD. 

1. Inicie sessão no seu **Adobe sessão** site da empresa como administrador.

2. No menu na parte superior, selecione **conta**. Em seguida, no painel esquerdo, selecione **utilizadores e grupos** > **criar um novo utilizador**.
   
    ![Captura de ecrã do Adobe início de sessão da empresa site, com a conta, os utilizadores e grupos e criar um novo utilizador realçado](./media/adobe-echosign-tutorial/ic789524.png "conta")
   
3. No **criar novo utilizador** secção, execute os seguintes passos:
   
    ![Secção de captura de ecrã de criar novo utilizador](./media/adobe-echosign-tutorial/ic789525.png "criar utilizador")
   
    a. Tipo de **endereço de correio eletrónico**, **nome próprio**, e **Apelido** de um Azure válido conta do AD que pretende aprovisionar nas caixas de texto correspondente.
   
    b. Selecione **criar utilizador**.

>[!NOTE]
>O marcador de posição de conta do Azure Active Directory recebe uma mensagem de e-mail que inclui uma ligação para confirmar a conta, para ficar ativa. 

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Britta Simon utilizar o Azure-início de sessão único, concedendo acesso Adobe início de sessão.

![Captura de ecrã do Azure portal-início de sessão único][200] 

1. No portal do Azure, abra a vista de aplicações. Em seguida, navegue para a vista de diretório, aceda a **aplicações empresariais**e selecione **todas as aplicações**.

    ![Vista de aplicações do portal de captura de ecrã do Azure, com aplicações da empresa e todas as aplicações realçadas][201] 

2. Na lista de aplicações, selecione **Adobe sessão**.

    ![Captura de ecrã da lista de aplicações, com início de sessão do Adobe realçado](./media/adobe-echosign-tutorial/tutorial_adobesign_app.png) 

3. No menu à esquerda, selecione **utilizadores e grupos**.

    ![Captura de ecrã do menu, com utilizadores e grupos realçados][202] 

4. Selecione **Adicionar**. Em seguida, no **adicionar atribuição** secção, selecione **utilizadores e grupos**.

    ![Captura de ecrã de utilizadores e grupos de página e adicione a secção de atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, na lista de utilizadores, selecione **Britta Simon**.

6. No **utilizadores e grupos** caixa de diálogo, clique em **selecione**.

7. No **adicionar atribuição** caixa de diálogo, selecione **atribuir**.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Quando seleciona o mosaico de início de sessão do Adobe no painel de acesso, esta deve obter automaticamente iniciada à sua aplicação de início de sessão do Adobe. Para mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/adobe-echosign-tutorial/tutorial_general_01.png
[2]: ./media/adobe-echosign-tutorial/tutorial_general_02.png
[3]: ./media/adobe-echosign-tutorial/tutorial_general_03.png
[4]: ./media/adobe-echosign-tutorial/tutorial_general_04.png

[100]: ./media/adobe-echosign-tutorial/tutorial_general_100.png

[200]: ./media/adobe-echosign-tutorial/tutorial_general_200.png
[201]: ./media/adobe-echosign-tutorial/tutorial_general_201.png
[202]: ./media/adobe-echosign-tutorial/tutorial_general_202.png
[203]: ./media/adobe-echosign-tutorial/tutorial_general_203.png
