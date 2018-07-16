---
title: 'Tutorial: Integração do Azure Active Directory com a Amplitude | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Amplitude.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 496c9ffa-c833-41fa-8d17-2dc3044954d1
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/16/2018
ms.author: jeedes
ms.openlocfilehash: a2815b60799f98071915a0f06908fd92ff3fb2f2
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39043936"
---
# <a name="tutorial-azure-active-directory-integration-with-amplitude"></a>Tutorial: Integração do Azure Active Directory com a Amplitude

Neste tutorial, saiba como integrar a Amplitude com o Azure Active Directory (Azure AD).

Integrar a Amplitude no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso a Amplitude.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada a Amplitude (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Amplitude, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Uma Amplitude de logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando a Amplitude da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-amplitude-from-the-gallery"></a>Adicionando a Amplitude da Galeria
Para configurar a integração da Amplitude para o Azure AD, terá de adicionar a Amplitude da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar a Amplitude da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

4. Na caixa de pesquisa, escreva **Amplitude**, selecione **Amplitude** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Amplitude na lista de resultados](./media/amplitude-tutorial/tutorial_amplitude_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com Amplitude com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de equivalente na Amplitude para um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado na Amplitude deve ser estabelecido.

Para configurar e testar o Azure AD início de sessão único com Amplitude, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste da Amplitude](#create-an-amplitude-test-user)**  - para ter um equivalente da Eduarda Almeida na Amplitude que está ligado à representação de utilizador do Azure AD.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo Amplitude.

**Para configurar o Azure AD início de sessão único com Amplitude, execute os seguintes passos:**

1. No portal do Azure, sobre o **Amplitude** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/amplitude-tutorial/tutorial_amplitude_samlbase.png)

3. Sobre o **Amplitude domínio e URLs** secção, execute os seguintes passos, se desejar configurar a aplicação no **IDP** iniciada pelo modo:

    ![Amplitude domínio e URLs únicas início de sessão em informações](./media/amplitude-tutorial/tutorial_amplitude_url.png)

    a. Na **identificador** caixa de texto, escreva o URL: `https://amplitude.com/saml/sso/metadata`

    b. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://analytics.amplitude.com/saml/sso/<uniqueid>`

    > [!NOTE]
    > O valor de URL de resposta não é real. Obterá o valor de URL de resposta mais tarde neste tutorial.

4. Verifique **Mostrar definições de URL avançadas** e executar o passo seguinte, se desejar configurar a aplicação na **SP** iniciada pelo modo:

    ![Amplitude domínio e URLs únicas início de sessão em informações](./media/amplitude-tutorial/tutorial_amplitude_url1.png)

    Na **URL de início de sessão** caixa de texto, escreva o URL: `https://analytics.amplitude.com/sso`

5. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![O link de download de certificado](./media/amplitude-tutorial/tutorial_amplitude_certificate.png) 

6. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/amplitude-tutorial/tutorial_general_400.png)
    
7. Início de sessão no site da sua empresa Amplitude como administrador.

8. Clique nas **planear a administração** na barra de navegação esquerdo.

    ![Configurar o início de sessão único](./media/amplitude-tutorial/configure1.png)

9. Selecione **metadados do Microsoft Azure Active Directory** partir a **SSO integração**.

    ![Configurar o início de sessão único](./media/amplitude-tutorial/configure2.png)

10. Sobre o **definido no início de sessão único** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/amplitude-tutorial/configure3.png)

    a. Abra o transferido **Xml de metadados** partir do portal do Azure no bloco de notas, cole o conteúdo para o **metadados do Microsoft Azure Active Directory** caixa de texto.

    b. Cópia a **URL de resposta (ACS)** valor e cole-o para o **URL de resposta** caixa de texto da secção de URLs de domínio da Amplitude e no portal do Azure.

    c. Clicar em **Guardar**

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/amplitude-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/amplitude-tutorial/create_aaduser_02.png)

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/amplitude-tutorial/create_aaduser_03.png)

4. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/amplitude-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-an-amplitude-test-user"></a>Criar um utilizador de teste da Amplitude

O objetivo desta secção é criar um utilizador chamado Eduarda Almeida na Amplitude. Amplitude suporta o aprovisionamento de just-in-time, que está por predefinição, ativada. Não existe nenhum item de ação para nesta secção. Um novo utilizador é criado durante uma tentativa de aceder a Amplitude se não existir ainda.
>[!Note]
>Se precisar de criar manualmente um utilizador, contacte [equipa de suporte da Amplitude](https://amplitude.zendesk.com).

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida a utilizar do Azure-início de sessão único, conceder acesso à Amplitude.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a Amplitude, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **Amplitude**.

    ![A ligação da Amplitude na lista de aplicações](./media/amplitude-tutorial/tutorial_amplitude_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico da Amplitude no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação Amplitude.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/amplitude-tutorial/tutorial_general_01.png
[2]: ./media/amplitude-tutorial/tutorial_general_02.png
[3]: ./media/amplitude-tutorial/tutorial_general_03.png
[4]: ./media/amplitude-tutorial/tutorial_general_04.png

[100]: ./media/amplitude-tutorial/tutorial_general_100.png

[200]: ./media/amplitude-tutorial/tutorial_general_200.png
[201]: ./media/amplitude-tutorial/tutorial_general_201.png
[202]: ./media/amplitude-tutorial/tutorial_general_202.png
[203]: ./media/amplitude-tutorial/tutorial_general_203.png

