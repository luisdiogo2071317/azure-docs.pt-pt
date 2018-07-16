---
title: 'Tutorial: Integração do Azure Active Directory com o sinal dinâmica | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e o sinal dinâmico.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 863f7340-b065-4f59-b092-daa67da6f703
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/10/2018
ms.author: jeedes
ms.openlocfilehash: 7a8b0387ebda574da572ac738bf3b75ef882224c
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39053137"
---
# <a name="tutorial-azure-active-directory-integration-with-dynamic-signal"></a>Tutorial: Integração do Azure Active Directory com o sinal dinâmico

Neste tutorial, saiba como integrar o sinal dinâmico com o Azure Active Directory (Azure AD).

Integrar o sinal dinâmico no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao sinal dinâmico.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para sinal dinâmico (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com sinal dinâmico, precisa do seguinte:

- Uma subscrição do Azure AD
- Um sinal dinâmica início de sessão único na subscrição ativado

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando o sinal dinâmica da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-dynamic-signal-from-the-gallery"></a>Adicionando o sinal dinâmica da Galeria
Para configurar a integração do sinal dinâmico no Azure AD, terá de adicionar sinal dinâmico a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar sinal dinâmico a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

4. Na caixa de pesquisa, escreva **sinal dinâmica**, selecione **sinal dinâmico** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Sinal dinâmico na lista de resultados](./media/dynamicsignal-tutorial/tutorial_dynamicsignal_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com sinal dinâmica com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no sinal dinâmico a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no sinal dinâmico tem de ser estabelecida.

Para configurar e testar o Azure AD início de sessão único com o sinal dinâmico, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste de sinal dinâmica](#create-a-dynamic-signal-test-user)**  - para ter um equivalente da Eduarda Almeida no sinal dinâmico que está ligado à representação de utilizador do Azure AD.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo de sinal dinâmico.

**Para configurar o Azure AD início de sessão único com sinal dinâmico, execute os seguintes passos:**

1. No portal do Azure, sobre o **sinal dinâmica** página de integração de aplicativo, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/dynamicsignal-tutorial/tutorial_dynamicsignal_samlbase.png)

3. Sobre o **dinâmico de sinal de domínio e URLs** secção, execute os seguintes passos:
 
    ![Dinâmica de sinal de domínio e URLs único informações de início de sessão](./media/dynamicsignal-tutorial/tutorial_dynamicsignal_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<subdomain>.voicestorm.com`

    b. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://<subdomain>.voicestorm.com`

    c. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://<subdomain>.voicestorm.com/User/SsoResponse`

    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o identificador de real, a URL de resposta e o URL de início de sessão. Contacte [equipa de suporte de cliente de sinal dinâmica](mailto:support@dynamicsignal.com) obter esses valores. 

4. Sobre o **certificado de assinatura SAML** secção, clique em **certificado (Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![O link de download de certificado](./media/dynamicsignal-tutorial/tutorial_dynamicsignal_certificate.png) 

5. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/dynamicsignal-tutorial/tutorial_general_400.png)
    
6. Na **configuração dinâmica de sinal** secção, clique em **Configurar Sinal dinâmico** para abrir **configurar início de sessão** janela. Cópia a **URL de fim de sessão, o ID de entidade de SAML e o SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configuração dinâmica de sinal](./media/dynamicsignal-tutorial/tutorial_dynamicsignal_configure.png) 

7. Para configurar o início de sessão único num **sinal dinâmica** lado, terá de enviar o transferido **certificado (Base64), o URL de fim de sessão, o ID de entidade de SAML e o SAML único início de sessão no URL do serviço** para [dinâmico A equipa de suporte do sinal](mailto:support@dynamicsignal.com). Se definir esta definição para que a ligação de SAML SSO definidas corretamente em ambos os lados.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/dynamicsignal-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/dynamicsignal-tutorial/create_aaduser_02.png)

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/dynamicsignal-tutorial/create_aaduser_03.png)

4. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/dynamicsignal-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-dynamic-signal-test-user"></a>Criar um utilizador de teste de sinal dinâmico

O objetivo desta secção é criar um usuário chamado Eduarda Almeida no sinal dinâmico. Sinal dinâmica suporta o aprovisionamento de just-in-time, que está por predefinição, ativada. Não existe nenhum item de ação para nesta secção. Um novo utilizador é criado durante uma tentativa de aceder sinal dinâmica, se não existir ainda.

>[!Note]
>Se precisar de criar manualmente um utilizador, contacte [equipa de suporte de sinal dinâmica](mailto:support@dynamicsignal.com).

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso a dinâmica de sinal.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a dinâmica de sinal, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **sinal dinâmica**.

    ![A ligação de sinal dinâmico na lista de aplicações](./media/dynamicsignal-tutorial/tutorial_dynamicsignal_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico de sinal dinâmico no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação de sinal dinâmico.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/dynamicsignal-tutorial/tutorial_general_01.png
[2]: ./media/dynamicsignal-tutorial/tutorial_general_02.png
[3]: ./media/dynamicsignal-tutorial/tutorial_general_03.png
[4]: ./media/dynamicsignal-tutorial/tutorial_general_04.png

[100]: ./media/dynamicsignal-tutorial/tutorial_general_100.png

[200]: ./media/dynamicsignal-tutorial/tutorial_general_200.png
[201]: ./media/dynamicsignal-tutorial/tutorial_general_201.png
[202]: ./media/dynamicsignal-tutorial/tutorial_general_202.png
[203]: ./media/dynamicsignal-tutorial/tutorial_general_203.png

