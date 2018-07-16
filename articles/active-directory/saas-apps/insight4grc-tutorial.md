---
title: 'Tutorial: Integração do Azure Active Directory com Insight4GRC | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Insight4GRC.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: db3b4bd1-b372-4660-88d7-aea0b0ca962e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2018
ms.author: jeedes
ms.openlocfilehash: 7372a51cda8c4f84de6cab3c0cd39ecbd1854175
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39052487"
---
# <a name="tutorial-azure-active-directory-integration-with-insight4grc"></a>Tutorial: Integração do Azure Active Directory com Insight4GRC

Neste tutorial, saiba como integrar Insight4GRC com o Azure Active Directory (Azure AD).

Integrar Insight4GRC no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Insight4GRC.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Insight4GRC (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Insight4GRC, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Insight4GRC logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Insight4GRC da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-insight4grc-from-the-gallery"></a>Adicionando Insight4GRC da Galeria
Para configurar a integração do Insight4GRC com o Azure AD, terá de adicionar Insight4GRC a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Insight4GRC a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

4. Na caixa de pesquisa, escreva **Insight4GRC**, selecione **Insight4GRC** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Insight4GRC na lista de resultados](./media/insight4grc-tutorial/tutorial_insight_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com Insight4GRC com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Insight4GRC a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Insight4GRC deve ser estabelecido.

Para configurar e testar o Azure AD início de sessão único com Insight4GRC, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste Insight4GRC](#create-an-insight4grc-test-user)**  - para ter um equivalente da Eduarda Almeida na Insight4GRC que está ligado à representação de utilizador do Azure AD.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo Insight4GRC.

**Para configurar o Azure AD início de sessão único com Insight4GRC, execute os seguintes passos:**

1. No portal do Azure, sobre o **Insight4GRC** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/insight4grc-tutorial/tutorial_insight_samlbase.png)

3. Sobre o **Insight4GRC domínio e URLs** secção, execute os seguintes passos, se desejar configurar a aplicação no **IDP** iniciada pelo modo:

    ![Insight4GRC domínio e URLs únicas início de sessão em informações](./media/insight4grc-tutorial/tutorial_insight_url.png)

    a. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://<subdomain>.Insight4GRC.com/SAML`

    b. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://<subdomain>.Insight4GRC.com/Public/SAML/ACS.aspx`

4. Verifique **Mostrar definições de URL avançadas** e executar o passo seguinte, se desejar configurar a aplicação na **SP** iniciada pelo modo:

    ![Insight4GRC domínio e URLs únicas início de sessão em informações](./media/insight4grc-tutorial/tutorial_insight_url1.png)

    Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<subdomain>.Insight4GRC.com/Public/Login.aspx`
     
    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o identificador de real, a URL de resposta e o URL de início de sessão. Contacte [equipa de suporte de cliente Insight4GRC](mailto:support.ss@rsmuk.com) obter esses valores. 

5. Sobre o **certificado de assinatura SAML** secção, clique no botão de cópia para copiar **Url de metadados de Federação de aplicação** e cole-o no bloco de notas.

    ![O link de download de certificado](./media/insight4grc-tutorial/tutorial_insight_certificate.png) 

6. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/insight4grc-tutorial/tutorial_general_400.png)
    
7. Para configurar o início de sessão único num **Insight4GRC** lado, terá de enviar o copiado **Url de metadados de Federação de aplicação** para [equipa de suporte de Insight4GRC](mailto:support.ss@rsmuk.com). Se definir esta definição para que a ligação de SAML SSO definidas corretamente em ambos os lados.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/insight4grc-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/insight4grc-tutorial/create_aaduser_02.png)

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/insight4grc-tutorial/create_aaduser_03.png)

4. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/insight4grc-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-an-insight4grc-test-user"></a>Criar um utilizador de teste Insight4GRC

O objetivo desta secção é criar um usuário chamado Eduarda Almeida no Insight4GRC. Insight4GRC suporta o aprovisionamento de just-in-time, que está por predefinição, ativada. Não existe nenhum item de ação para nesta secção. Um novo utilizador é criado durante uma tentativa de aceder Insight4GRC se não existir ainda.

>[!Note]
>Se precisar de criar manualmente um utilizador, contacte [equipa de suporte de cliente Insight4GRC](mailto:support.ss@rsmuk.com).

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para Insight4GRC.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a Insight4GRC, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **Insight4GRC**.

    ![A ligação de Insight4GRC na lista de aplicações](./media/insight4grc-tutorial/tutorial_insight_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico Insight4GRC no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo Insight4GRC.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/insight4grc-tutorial/tutorial_general_01.png
[2]: ./media/insight4grc-tutorial/tutorial_general_02.png
[3]: ./media/insight4grc-tutorial/tutorial_general_03.png
[4]: ./media/insight4grc-tutorial/tutorial_general_04.png

[100]: ./media/insight4grc-tutorial/tutorial_general_100.png

[200]: ./media/insight4grc-tutorial/tutorial_general_200.png
[201]: ./media/insight4grc-tutorial/tutorial_general_201.png
[202]: ./media/insight4grc-tutorial/tutorial_general_202.png
[203]: ./media/insight4grc-tutorial/tutorial_general_203.png

