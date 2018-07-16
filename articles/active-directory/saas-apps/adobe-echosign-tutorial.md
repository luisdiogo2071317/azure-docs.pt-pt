---
title: 'Tutorial: Integração do Azure Active Directory com o Adobe Sign | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e o Adobe Sign.
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
ms.openlocfilehash: d5cdc2ec0c6cfcf52f84629485d0dd879fbf6fa2
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39054003"
---
# <a name="tutorial-azure-active-directory-integration-with-adobe-sign"></a>Tutorial: Integração do Azure Active Directory com o Adobe Sign

Neste tutorial, saiba como integrar o Adobe Sign com o Azure Active Directory (Azure AD).

Integrar o Adobe Sign no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Adobe Sign.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para o Adobe Sign (início de sessão único) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central, o portal do Azure.

Para obter mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o Adobe Sign, terá de:

- Uma subscrição do Azure AD
- Um início de sessão do Adobe logon único habilitado subscrição

Para testar os passos neste tutorial, siga as seguintes recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando o Adobe Sign da galeria.
2. Configuração e teste do Azure AD início de sessão único.

## <a name="add-adobe-sign-from-the-gallery"></a>Adicionar início de sessão do Adobe a partir da Galeria
Para configurar a integração do Adobe Sign para o Azure AD, terá de adicionar o Adobe Sign partir da Galeria à sua lista de aplicações de SaaS geridas.

1. Na [portal do Azure](https://portal.azure.com), no painel esquerdo, selecione a **Azure Active Directory** ícone. 

    ![Captura de ecrã do ícone do Azure Active Directory][1]

2. Navegue até **aplicações empresariais** > **todas as aplicações**.

    ![Menus de captura de ecrã do Azure Active Directory, com aplicações empresariais e todas as aplicações destacadas][2]
    
3. Para adicionar uma nova aplicação, selecione **nova aplicação** na parte superior da caixa de diálogo.

    ![Captura de ecrã da nova opção de aplicativo na parte superior da caixa de diálogo][3]

4. Na caixa de pesquisa, escreva **Adobe Sign**.

    ![Captura de ecrã da caixa de pesquisa](./media/adobe-echosign-tutorial/tutorial_adobesign_search.png)

5. No painel de resultados, selecione **Adobe Sign**e, em seguida, selecione **Add**.

    ![Captura de ecrã do painel de resultados](./media/adobe-echosign-tutorial/tutorial_adobesign_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único
Nesta secção, configurar e testar o Azure AD início de sessão único com o Adobe Sign, com base num utilizador de teste chamado "Eduarda Almeida."

Para início de sessão único funcione, o Azure AD precisa reconhecer uma relação ligada entre um utilizador do Azure AD e o utilizador relacionado no Adobe Sign.

Para estabelecer a relação de ligado, no início de sessão do Adobe, atribua o valor do **nome de utilizador** no Azure AD como o valor da **Username**.

Para configurar e testar o Azure AD início de sessão único com o Adobe Sign, conclua os seguintes blocos de construção:

1. [Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on) para permitir aos utilizadores utilizar esta funcionalidade.
2. [Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user) para testar o Azure AD início de sessão único com Eduarda Almeida.
3. [Criar um utilizador de teste de início de sessão do Adobe](#creating-an-adobe-sign-test-user) para ter um equivalente da Eduarda Almeida no início de sessão do Adobe que está ligada à representação de utilizador do Azure AD.
4. [Atribua o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user) para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. [Testar início de sessão único](#testing-single-sign-on) para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, ativar o Azure AD início de sessão único no portal do Azure e configurar o início de sessão único na sua aplicação do Adobe Sign.

1. No portal do Azure, sobre o **Adobe Sign** página de integração de aplicações, selecione **início de sessão único**.

    ![Página de integração de aplicativo de captura de ecrã do Adobe Sign, com início de sessão único realçado][4]

2. Sobre o **início de sessão único** caixa de diálogo, para **modo**, selecione **baseado em SAML início de sessão** para ativar o início de sessão único.

    ![Captura de ecrã do única caixa de diálogo de início de sessão, com a caixa de modo realçada](./media/adobe-echosign-tutorial/tutorial_adobesign_samlbase.png)

3. Na **domínio de início de sessão do Adobe e URLs** secção, execute os seguintes passos:

    ![Secção de captura de ecrã do Adobe Sign domínio e URLs](./media/adobe-echosign-tutorial/tutorial_adobesign_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL que utiliza o seguinte padrão: `https://<companyname>.echosign.com/`

    b. Na **identificador** caixa de texto, escreva um URL que utiliza o seguinte padrão: `https://<companyname>.echosign.com`

    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o URL de início de sessão real e o identificador. Contacte [equipa de suporte de cliente de início de sessão do Adobe](https://helpx.adobe.com/in/contact/support.html) obter esses valores.

4. Na **certificado de assinatura SAML** secção, selecione **Certificate(Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![Secção de captura de ecrã do certificado de assinatura SAML](./media/adobe-echosign-tutorial/tutorial_adobesign_certificate.png) 

5. Selecione **Guardar**.

    ![Botão de captura de ecrã de guardar](./media/adobe-echosign-tutorial/tutorial_general_400.png)

6. Na **configuração de início de sessão do Adobe** secção, selecione **configurar o Adobe Sign** para abrir o **configurar início de sessão** janela. Copiar o **URL de fim de sessão**, **ID de entidade de SAML**, e **SAML único início de sessão no URL do serviço** do **referência rápida** secção.

    ![Secção de captura de ecrã de configuração de início de sessão Adobe, com a configurar o Adobe Sign realçado](./media/adobe-echosign-tutorial/tutorial_adobesign_configure.png)

7. Antes de configuração, entre em contato com o [equipa de suporte de cliente de início de sessão do Adobe](https://helpx.adobe.com/in/contact/support.html) à lista de permissões seu domínio no Adobe Sign. Eis como adicionar o domínio:

    a. O [equipa de suporte de cliente de início de sessão do Adobe](https://helpx.adobe.com/in/contact/support.html) envia-lhe um token gerado aleatoriamente. Para o seu domínio, o token será semelhante ao seguinte: **verificação do início de sessão do adobe = xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx**

    b. Publicar o token de verificação num registo de texto DNS e notificar o [equipa de suporte de cliente de início de sessão do Adobe](https://helpx.adobe.com/in/contact/support.html).
    
    > [!NOTE]
    > Esta operação pode demorar alguns dias ou mais. Tenha em atenção que os atrasos de propagação de DNS, significa que um valor publicado no DNS pode não estar visível para uma hora ou mais. O administrador de TI deve ser informado sobre como publicar este token num registo de texto DNS.
    
    c. Quando notificar o [equipa de suporte de cliente de início de sessão do Adobe](https://helpx.adobe.com/in/contact/support.html) através do pedido de suporte, depois do token é publicado, eles validar o domínio e adicioná-la à sua conta.
    
    d. Em geral, eis como publicar o token de um registo DNS:

    * Inicie sessão sua conta de domínio
    * Localize a página para atualizar o registo DNS. Esta página poderia ser chamada de gestão de DNS, nome do servidor de gestão ou definições avançadas.
    * Localize os registos TXT para o seu domínio.
    * Adicione um registo TXT com o valor total token fornecido pelo Adobe.
    * Guarde as alterações.

8. Numa janela do browser web diferente, inicie sessão no site da sua empresa Adobe Sign como um administrador.

9. No menu de SAML, selecione **definições de conta** > **definições de SAML**.
   
    ![Página de captura de ecrã do Adobe início de sessão SAML configurações](./media/adobe-echosign-tutorial/ic789520.png "conta")

10. Na **definições de SAML** secção, execute os seguintes passos:
  
    ![Captura de ecrã das definições de SAML](./media/adobe-echosign-tutorial/ic789521.png "definições de SAML")
   
    a. Sob **modo de SAML**, selecione **SAML obrigatório**.
   
    b. Selecione **permitir Echosign aos administradores de conta para iniciar sessão com as respetivas credenciais de Echosign**.
   
    c. Sob **criação do utilizador**, selecione **adicionar automaticamente utilizadores autenticados através de SAML**.

    d. Colar **ID de entidade de SAML**, que copiou do portal do Azure para o **URL de emissor/ID de entidade** caixa de texto.
    
    e. Colar **SAML único início de sessão no URL do serviço**, que copiou do portal do Azure para o **ponto final do URL/SSO de início de sessão** caixa de texto.
   
    f. Colar **URL de fim de sessão**, que copiou do portal do Azure para o **ponto final do URL/SLO de fim de sessão** caixa de texto.

    g. Abra sua transferido **Certificate(Base64)** ficheiro no bloco de notas. Copie o conteúdo do mesmo para a área de transferência e, em seguida, cole-os para o **IdP certificado** caixa de texto.

    h. Selecione **guardar alterações**.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste, com o nome Eduarda Almeida, no portal do Azure.

![Captura de ecrã do nome de utilizador de teste no portal do Azure][100]

1. Na **portal do Azure**, no painel esquerdo, selecione a **Azure Active Directory** ícone.

    ![Captura de ecrã do ícone do Azure AD](./media/adobe-echosign-tutorial/create_aaduser_01.png) 

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e selecione **todos os utilizadores**.
    
    ![Menus de captura de ecrã do Azure AD, com os utilizadores e grupos e todos os utilizadores realçados](./media/adobe-echosign-tutorial/create_aaduser_02.png) 

3. Para abrir o **usuário** caixa de diálogo, selecione **Add**.
 
    ![Captura de ecrã da parte superior da caixa de diálogo utilizadores todos os, com a opção de adicionar realçada](./media/adobe-echosign-tutorial/create_aaduser_03.png) 

4. Sobre o **utilizador** diálogo caixa, execute os seguintes passos:
 
    ![Caixa de diálogo de captura de ecrã de utilizador](./media/adobe-echosign-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, escreva **BrittaSimon**.

    b. Na **nome de utilizador** texto, escreva o endereço de e-mail do BrittaSimon.

    c. Selecione **mostrar palavra-passe**, indique o valor da **palavra-passe**.

    d. Selecione **Criar**.
 
### <a name="create-an-adobe-sign-test-user"></a>Criar um utilizador de teste de início de sessão do Adobe

Para ativar a utilizadores do Azure AD iniciar sessão no Adobe Sign, tem de ser aprovisionados no Adobe Sign. Esta é uma tarefa manual.

>[!NOTE]
>Pode utilizar quaisquer outras ferramentas de criação da conta de utilizador de início de sessão do Adobe ou APIs fornecidas pelo início de sessão do Adobe para aprovisionar contas de utilizador do Azure AD. 

1. Inicie sessão no seu **Adobe Sign** site da empresa como administrador.

2. No menu na parte superior, selecione **conta**. Em seguida, no painel esquerdo, selecione **utilizadores e grupos** > **criar um novo utilizador**.
   
    ![Captura de ecrã do Adobe Sign da empresa site, com a conta, de utilizadores e grupos e criar um novo utilizador realçado](./media/adobe-echosign-tutorial/ic789524.png "conta")
   
3. Na **criar novo utilizador** secção, execute os seguintes passos:
   
    ![Secção de captura de ecrã de criar novo usuário](./media/adobe-echosign-tutorial/ic789525.png "criar utilizador")
   
    a. Tipo de **endereço de E-Mail**, **nome próprio**, e **Apelido** de um Azure válido conta AD que pretende aprovisionar nas caixas de texto relacionados.
   
    b. Selecione **criar utilizador**.

>[!NOTE]
>O titular da conta do Azure Active Directory recebe uma mensagem de e-mail que inclui uma ligação para confirmar a conta até se tornar Active Directory. 

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único, ao conceder acesso para o Adobe Sign.

![Captura de ecrã do Azure portal início de sessão único][200] 

1. No portal do Azure, abra a vista de aplicações. Em seguida, navegue para a vista de diretório, aceda a **aplicações empresariais**e selecione **todos os aplicativos**.

    ![Vista de aplicações do portal de captura de ecrã do Azure, com aplicações empresariais e todas as aplicações destacadas][201] 

2. Na lista de aplicações, selecione **Adobe Sign**.

    ![Captura de ecrã da lista de aplicações, com o Adobe Sign realçado](./media/adobe-echosign-tutorial/tutorial_adobesign_app.png) 

3. No menu à esquerda, selecione **utilizadores e grupos**.

    ![Captura de ecrã do menu, com utilizadores e grupos realçados][202] 

4. Selecione **Adicionar**. Em seguida, na **adicionar atribuição** secção, selecione **utilizadores e grupos**.

    ![Captura de ecrã dos utilizadores e grupos de página e adicione a secção de atribuição][203]

5. Na **utilizadores e grupos** caixa de diálogo, na lista de utilizadores, selecione **Eduarda Almeida**.

6. Na **utilizadores e grupos** caixa de diálogo, clique em **selecione**.

7. Na **adicionar atribuição** caixa de diálogo, selecione **atribuir**.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Ao selecionar o mosaico de início de sessão do Adobe no painel de acesso, deve obter automaticamente a sessão iniciada sua aplicação do Adobe Sign. Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

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
