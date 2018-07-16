---
title: 'Tutorial: Integração do Azure Active Directory com TOPdesk - seguro | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e TOPdesk - segura.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 8e06ee33-18f9-4c05-9168-e6b162079d88
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/08/2017
ms.author: jeedes
ms.openlocfilehash: 2da2a2cae3993f7c29726b842db6767d4300cacc
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39045262"
---
# <a name="tutorial-azure-active-directory-integration-with-topdesk---secure"></a>Tutorial: Integração do Azure Active Directory com TOPdesk - seguro

Neste tutorial, saiba como integrar TOPdesk - proteger com o Azure Active Directory (Azure AD).

Integrar TOPdesk - segura com o Azure AD fornece-lhe as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao TOPdesk - segura.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para TOPdesk - seguro (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com TOPdesk - seguro, precisa do seguinte:

- Uma subscrição do Azure AD
- Um TOPdesk - seguro logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionar TOPdesk - seguro a partir da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-topdesk---secure-from-the-gallery"></a>Adicionar TOPdesk - seguro a partir da Galeria
Para configurar a integração do TOPdesk - proteger com o Azure AD, terá de adicionar TOPdesk - proteger da galeria, à sua lista de SaaS geridos, as aplicações.

**Para adicionar TOPdesk - seguro a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

4. Na caixa de pesquisa, escreva **TOPdesk - segura**, selecione **TOPdesk - seguro** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![TOPdesk - proteger na lista de resultados](./media/topdesk-secure-tutorial/tutorial_topdesk-secure_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com TOPdesk - seguro com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no TOPdesk - seguro a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no TOPdesk - seguro tem de ser estabelecida.

No TOPdesk - proteger, atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com TOPdesk - seguro, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um TOPdesk - utilizador de teste seguro](#create-a-topdesk---secure-test-user)**  - para ter um equivalente da Eduarda Almeida na TOPdesk - seguro, que está ligado à representação de utilizador do Azure AD.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único na sua TOPdesk - aplicação segura.

**Para configurar o Azure AD início de sessão único com TOPdesk - proteger, execute os seguintes passos:**

1. No portal do Azure, sobre o **TOPdesk - segura** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/topdesk-secure-tutorial/tutorial_topdesk-secure_samlbase.png)

3. Sobre o **TOPdesk - proteger o domínio e URLs** secção, execute os seguintes passos:

    ![Informações de início de sessão de único TOPdesk - seguro de domínio e URLs](./media/topdesk-secure-tutorial/tutorial_topdesk-secure_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<companyname>.topdesk.net`

    b. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://<companyname>.topdesk.net/tas/secure/login/verify`

    c. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://<companyname>.topdesk.net/tas/public/login/saml`

    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o URL de início de sessão e o identificador real. URL de resposta é explicado mais tarde no tutorial. Contacte [TOPdesk - equipa de suporte de cliente proteger](http://www.topdesk.com/us/support) obter esses valores. 

4. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![O link de download de certificado](./media/topdesk-secure-tutorial/tutorial_topdesk-secure_certificate.png) 

5. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/topdesk-secure-tutorial/tutorial_general_400.png)

6. Na **TOPdesk - configuração segura** secção, clique em **configurar TOPdesk - seguro** para abrir **configurar início de sessão** janela. Cópia a **URL de fim de sessão, o ID de entidade de SAML e o SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![TOPdesk - uma configuração segura](./media/topdesk-secure-tutorial/tutorial_topdesk-secure_configure.png)
    
7. Inicie sessão no seu **TOPdesk - seguro** site da empresa como administrador.

8. Na **TOPdesk** menu, clique em **definições**.

    ![As definições](./media/topdesk-secure-tutorial/ic790598.png "definições")

9. Clique em **definições de início de sessão**.

    ![Definições de início de sessão](./media/topdesk-secure-tutorial/ic790599.png "definições de início de sessão")

10. Expanda a **definições de início de sessão** e, em seguida, clique **geral**.

    ![Gerais](./media/topdesk-secure-tutorial/ic790600.png "geral")

11. Na **seguro** secção a **início de sessão SAML** configuração secção, execute os seguintes passos:

    ![Definições técnicas](./media/topdesk-secure-tutorial/ic790855.png "definições técnicas")
   
    a. Clique em **transferir** para transferir o ficheiro de metadados públicos e, em seguida, guarde-o localmente no seu computador.
   
    b. Abra o ficheiro de metadados e, em seguida, localize a **AssertionConsumerService** nó.
    
    ![Serviço de consumidor de asserção](./media/topdesk-secure-tutorial/ic790856.png "serviço de consumidor de asserção")
   
    c. Copiar o **AssertionConsumerService** valor, cole este valor na caixa de texto no URL de resposta **TOPdesk - proteger o domínio e URLs** secção.

12. Para criar um ficheiro de certificado, execute os seguintes passos:
    
    ![Certificado](./media/topdesk-secure-tutorial/ic790606.png "certificado")
    
    a. Abra o ficheiro de metadados baixado a partir do portal do Azure.

    b. Expanda a **RoleDescriptor** nó que tem um **xsi: type** de **inseridas: ApplicationServiceType**.

    c. Copie o valor do **X509Certificate** nó.

    d. Guardar o copiado **X509Certificate** valor localmente no seu computador num arquivo.

13. Na **pública** secção, clique em **Add**.
    
    ![Adicione](./media/topdesk-secure-tutorial/ic790607.png "adicionar")

14. Sobre o **Assistente de configuração de SAML** caixa de diálogo página, execute os seguintes passos:
    
    ![Assistente de configuração de SAML](./media/topdesk-secure-tutorial/ic790608.png "Assistente de configuração de SAML")
    
    a. Para carregar o ficheiro de metadados baixado no portal do Azure, em **metadados de Federação**, clique em **procurar**.

    b. Para carregar o ficheiro de certificado, em **certificados RSA ()**, clique em **procurar**.

    c. Para carregar o ficheiro do logótipo da equipe de suporte do TOPdesk, tem em **ícone de logótipo**, clique em **procurar**.

    d. Na **atributo de nome de utilizador** caixa de texto, tipo `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.

    e. Na **nome a apresentar** caixa de texto, escreva um nome para a sua configuração.

    f. Clique em **Guardar**.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/topdesk-secure-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/topdesk-secure-tutorial/create_aaduser_02.png)

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/topdesk-secure-tutorial/create_aaduser_03.png)

4. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/topdesk-secure-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-topdesk---secure-test-user"></a>Criar um TOPdesk - utilizador de teste seguro

Para permitir que os utilizadores do Azure AD logon TOPdesk - seguro, eles têm de ser aprovisionados em TOPdesk - segura.  
No caso TOPdesk - seguros, aprovisionamento é uma tarefa manual.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Para configurar o aprovisionamento de utilizadores, execute os seguintes passos:
1. Inicie sessão no seu **TOPdesk - seguro** site da empresa como administrador.
2. No menu na parte superior, clique em **TOPdesk \> New \> ficheiros de suporte \> operador**.
   
    ![Operador](./media/topdesk-secure-tutorial/ic790610.png "operador")

3. Sobre o **operador New** caixa de diálogo, execute os seguintes passos:
   
    ![Novo operador](./media/topdesk-secure-tutorial/ic790611.png "novo operador")
   
    a. Clique nas **gerais** separador.
   
    b. Na **Apelido** caixa de texto, como o tipo de apelido do utilizador **Simon**.
   
    c. Selecione um **Site** para a conta na **localização** secção.
   
    d. Na **nome de início de sessão** caixa de texto da **TOPdesk início de sessão** secção, escreva um nome de início de sessão para o utilizador.
   
    e. Clique em **Guardar**.

> [!NOTE]
> Pode usar qualquer outra TOPdesk - ferramentas de criação de conta de usuário segura ou APIs fornecidas pelos TOPdesk - segura para aprovisionar contas de utilizador do AAD.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para TOPdesk - segura.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a TOPdesk - proteger, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **TOPdesk - seguro**.

    ![TOPdesk - ligação segura na lista de aplicações](./media/topdesk-secure-tutorial/tutorial_topdesk-secure_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica em TOPdesk - mosaico seguro no painel de acesso, deve obter automaticamente com sessão iniciada para sua TOPdesk - aplicativo seguro.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/topdesk-secure-tutorial/tutorial_general_01.png
[2]: ./media/topdesk-secure-tutorial/tutorial_general_02.png
[3]: ./media/topdesk-secure-tutorial/tutorial_general_03.png
[4]: ./media/topdesk-secure-tutorial/tutorial_general_04.png

[100]: ./media/topdesk-secure-tutorial/tutorial_general_100.png

[200]: ./media/topdesk-secure-tutorial/tutorial_general_200.png
[201]: ./media/topdesk-secure-tutorial/tutorial_general_201.png
[202]: ./media/topdesk-secure-tutorial/tutorial_general_202.png
[203]: ./media/topdesk-secure-tutorial/tutorial_general_203.png

