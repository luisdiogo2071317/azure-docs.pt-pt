---
title: 'Tutorial: Integração do Azure Active Directory com Bridgeline desvinculado | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Bridgeline desvinculado.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: b018472f-c8b3-403d-ae66-9ed26a35f413
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2018
ms.author: jeedes
ms.openlocfilehash: c429afa12bc11db68d041fef96f66b3f4c7f0b1b
ms.sourcegitcommit: 30221e77dd199ffe0f2e86f6e762df5a32cdbe5f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/23/2018
ms.locfileid: "39206114"
---
# <a name="tutorial-azure-active-directory-integration-with-bridgeline-unbound"></a>Tutorial: Integração do Azure Active Directory com Bridgeline desvinculado

Neste tutorial, saiba como integrar Bridgeline desvinculado com o Azure Active Directory (Azure AD).

Integrar Bridgeline desvinculado no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Bridgeline desvinculado.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Bridgeline desvinculado (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Bridgeline desvinculada, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Bridgeline desvinculado logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Bridgeline desvinculado da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-bridgeline-unbound-from-the-gallery"></a>Adicionando Bridgeline desvinculado da Galeria
Para configurar a integração do Bridgeline desvinculado para o Azure AD, terá de adicionar Bridgeline desvinculado na Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Bridgeline desvinculado a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

4. Na caixa de pesquisa, escreva **Bridgeline desvinculado**, selecione **Bridgeline desvinculado** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Bridgeline desvinculado na lista de resultados](./media/bridgelineunbound-tutorial/tutorial_bridgelineunbound_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com Bridgeline desvinculado com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Bridgeline desvinculado para um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Bridgeline desvinculado deve ser estabelecido.

Para configurar e testar o Azure AD início de sessão único com Bridgeline desvinculada, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste Bridgeline desvinculado](#create-a-bridgeline-unbound-test-user)**  - para ter um equivalente da Eduarda Almeida na Bridgeline desvinculado que está ligado à representação de utilizador do Azure AD.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo Bridgeline desvinculado.

**Para configurar o Azure AD início de sessão único com Bridgeline desvinculada, execute os seguintes passos:**

1. No portal do Azure, sobre o **Bridgeline desvinculado** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/bridgelineunbound-tutorial/tutorial_bridgelineunbound_samlbase.png)
 
3. Sobre o **domínio desvinculada de Bridgeline e URLs** secção, execute os seguintes passos, se desejar configurar a aplicação no **IDP** iniciada pelo modo:

    ![Bridgeline domínio e URLs únicas início de sessão em informações](./media/bridgelineunbound-tutorial/tutorial_bridgelineunbound_url.png)

    a. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `iApps_UPSTT_<ENVIRONMENTNAME>`

    b. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://<SUBDOMAIN>.iapps.com/SAMLAssertionService.aspx`

4. Verifique **Mostrar definições de URL avançadas** e executar o passo seguinte, se desejar configurar a aplicação na **SP** iniciada pelo modo:

    ![Bridgeline domínio e URLs únicas início de sessão em informações](./media/bridgelineunbound-tutorial/tutorial_bridgelineunbound_url1.png)

    Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<SUBDOMAIN>.iapps.com/CommonLogin/login?<INSTANCENAME>`

    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o identificador real, o URL de resposta e URL de início de sessão. Contacte [equipa de suporte de cliente de desvinculado Bridgeline](mailto:support@iapps.com) obter esses valores. 

4. Sobre o **certificado de assinatura SAML** secção, clique em **certificado (Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![O link de download de certificado](./media/bridgelineunbound-tutorial/tutorial_bridgelineunbound_certificate.png) 

5. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/bridgelineunbound-tutorial/tutorial_general_400.png)

6. Na **configuração desvinculada de Bridgeline** secção, clique em **configurar Bridgeline desvinculado** para abrir **configurar início de sessão** janela. Cópia a **URL de fim de sessão, o ID de entidade de SAML e o SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configuração de desvinculado Bridgeline](./media/bridgelineunbound-tutorial/tutorial_bridgelineunbound_configure.png) 

7. Para configurar o início de sessão único num **Bridgeline desvinculado** lado, terá de enviar o transferido **certificado (Base64)**, **URL de fim de sessão, o ID de entidade de SAML e o SAML único início de sessão no URL do serviço** para [equipa de suporte Bridgeline desvinculado](mailto:support@iapps.com). Se definir esta definição para que a ligação de SAML SSO definidas corretamente em ambos os lados.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/bridgelineunbound-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grobridgelineinbound**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grobridgelineinbound" e os links de "Todos os utilizadores"](./media/bridgelineunbound-tutorial/create_aaduser_02.png)

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/bridgelineunbound-tutorial/create_aaduser_03.png)

4. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/bridgelineunbound-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-bridgeline-unbound-test-user"></a>Criar um utilizador de teste Bridgeline desvinculado

O objetivo desta secção é criar um usuário chamado Eduarda Almeida no Bridgeline desvinculado. Bridgeline desvinculado suporta o aprovisionamento just-in-time, que está por predefinição, ativada. Não existe nenhum item de ação para nesta secção. Um novo utilizador é criado durante uma tentativa de aceder Bridgeline desvinculado se não existir ainda.

>[!Note]
>Se precisar de criar manualmente um utilizador, contacte [Bridgeline desvinculado equipa de suporte](mailto:support@iapps.com).

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para Bridgeline desvinculado.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a Bridgeline desvinculada, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **Bridgeline desvinculado**.

    ![A ligação Bridgeline desvinculado na lista de aplicações](./media/bridgelineunbound-tutorial/tutorial_bridgelineunbound_app.png)  

3. No menu à esquerda, clique em **utilizadores e grobridgelineinbound**.

    ![A ligação "Utilizadores e grobridgelineinbound"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grobridgelineinbound** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grobridgelineinbound** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grobridgelineinbound** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico Bridgeline desvinculado no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação Bridgeline desvinculado.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/bridgelineunbound-tutorial/tutorial_general_01.png
[2]: ./media/bridgelineunbound-tutorial/tutorial_general_02.png
[3]: ./media/bridgelineunbound-tutorial/tutorial_general_03.png
[4]: ./media/bridgelineunbound-tutorial/tutorial_general_04.png

[100]: ./media/bridgelineunbound-tutorial/tutorial_general_100.png

[200]: ./media/bridgelineunbound-tutorial/tutorial_general_200.png
[201]: ./media/bridgelineunbound-tutorial/tutorial_general_201.png
[202]: ./media/bridgelineunbound-tutorial/tutorial_general_202.png
[203]: ./media/bridgelineunbound-tutorial/tutorial_general_203.png

