---
title: 'Tutorial: Integração do Azure Active Directory com Communifire | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Communifire.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: de2a164d-2115-43e7-a9ed-e54f483f4aeb
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2017
ms.author: jeedes
ms.openlocfilehash: 580abf18367dd15f5b59858e53d5b380f566f2d6
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39053812"
---
# <a name="tutorial-azure-active-directory-integration-with-communifire"></a>Tutorial: Integração do Azure Active Directory com Communifire

Neste tutorial, saiba como integrar Communifire com o Azure Active Directory (Azure AD).

Integrar Communifire no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Communifire.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Communifire (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Communifire, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Communifire início de sessão único na subscrição ativado

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Communifire da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-communifire-from-the-gallery"></a>Adicionando Communifire da Galeria
Para configurar a integração do Communifire com o Azure AD, terá de adicionar Communifire a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Communifire a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

4. Na caixa de pesquisa, escreva **Communifire**, selecione **Communifire** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Communifire na lista de resultados](./media/communifire-tutorial/tutorial_communifire_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com Communifire com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Communifire a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Communifire deve ser estabelecido.

Communifire, atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com Communifire, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste Communifire](#create-a-communifire-test-user)**  - para ter um equivalente da Eduarda Almeida na Communifire que está ligado à representação de utilizador do Azure AD.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo Communifire.

**Para configurar o Azure AD início de sessão único com Communifire, execute os seguintes passos:**

1. No portal do Azure, sobre o **Communifire** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/communifire-tutorial/tutorial_communifire_samlbase.png)

3. Sobre o **Communifire domínio e URLs** secção, execute os seguintes passos, se desejar configurar a aplicação no modo de IDP iniciado:

    ![Communifire domínio e URLs únicas início de sessão em informações](./media/communifire-tutorial/tutorial_communifire_url.png)

    a. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://<subdomain>.communifire.com`

    b. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://<subdomain>.communifire.com/SAML/AssertionConsumerService.aspx`

4. Verifique **Mostrar definições de URL avançadas** e executar o passo seguinte, se desejar configurar a aplicação na **SP** iniciada pelo modo:

    ![Communifire domínio e URLs únicas início de sessão em informações](./media/communifire-tutorial/tutorial_communifire_url1.png)

    Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<subdomain>.communifire.com/login`
     
    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o identificador de real, a URL de resposta e o URL de início de sessão. Contacte [equipa de suporte de cliente Communifire](https://my.axerosolutions.com/spaces/77/communifire-support/help/welcome) obter esses valores. 

5. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![O link de download de certificado](./media/communifire-tutorial/tutorial_communifire_certificate.png) 

6.  Verifique **Mostrar definições de assinatura de certificado avançadas** e selecione **opção assinatura** como **asserção e resposta SAML de início de sessão**.

    ![A opção de certificado](./media/communifire-tutorial/tutorial_communifire_certificateoption.png) 

7. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/communifire-tutorial/tutorial_general_400.png)
    
8. Para configurar o início de sessão único num **Communifire** lado, terá de enviar o transferido **XML de metadados** para [equipa de suporte de Communifire](https://my.axerosolutions.com/spaces/77/communifire-support/help/welcome). Se definir esta definição para que a ligação de SAML SSO definidas corretamente em ambos os lados.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/communifire-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/communifire-tutorial/create_aaduser_02.png)

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/communifire-tutorial/create_aaduser_03.png)

4. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/communifire-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-communifire-test-user"></a>Criar um utilizador de teste Communifire

O objetivo desta secção é criar um usuário chamado Eduarda Almeida no Communifire. Communifire suporta o aprovisionamento de just-in-time, que está por predefinição, ativada. Um novo utilizador é criado depois de guardar os detalhes de perfil durante uma tentativa de aceder Communifire se não existir ainda.

>[!Note]
>Se precisar de criar manualmente um utilizador, contacte [equipa de suporte de Communifire](https://my.axerosolutions.com/spaces/77/communifire-support/help/welcome).

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para Communifire.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a Communifire, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **Communifire**.

    ![A ligação de Communifire na lista de aplicações](./media/communifire-tutorial/tutorial_communifire_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico Communifire no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo Communifire.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/communifire-tutorial/tutorial_general_01.png
[2]: ./media/communifire-tutorial/tutorial_general_02.png
[3]: ./media/communifire-tutorial/tutorial_general_03.png
[4]: ./media/communifire-tutorial/tutorial_general_04.png

[100]: ./media/communifire-tutorial/tutorial_general_100.png

[200]: ./media/communifire-tutorial/tutorial_general_200.png
[201]: ./media/communifire-tutorial/tutorial_general_201.png
[202]: ./media/communifire-tutorial/tutorial_general_202.png
[203]: ./media/communifire-tutorial/tutorial_general_203.png

