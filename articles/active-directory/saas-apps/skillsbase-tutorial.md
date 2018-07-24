---
title: 'Tutorial: Integração do Azure Active Directory com Base de habilidades | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e a Base de habilidades.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 237d90c4-8243-4f80-a305-b5ad9204159e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2018
ms.author: jeedes
ms.openlocfilehash: e11ba8ca9c4ad17b2ade909bb474ad2d1fcf4410
ms.sourcegitcommit: 30221e77dd199ffe0f2e86f6e762df5a32cdbe5f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/23/2018
ms.locfileid: "39205379"
---
# <a name="tutorial-azure-active-directory-integration-with-skills-base"></a>Tutorial: Integração do Azure Active Directory com Base de habilidades

Neste tutorial, saiba como integrar a Base de habilidades com o Azure Active Directory (Azure AD).

Integração de Base de habilidades com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso à Base de habilidades.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para a Base de habilidades (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com a Base de habilidades, precisa do seguinte:

- Uma subscrição do Azure AD
- Uma Base de habilidades de logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando a Base de habilidades da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-skills-base-from-the-gallery"></a>Adicionando a Base de habilidades da Galeria
Para configurar a integração da Base de habilidades para o Azure AD, terá de adicionar a Base de conhecimentos a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar a Base de habilidades da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

4. Na caixa de pesquisa, escreva **Base de habilidades**, selecione **Base de habilidades** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Base de habilidades na lista de resultados](./media/skillsbase-tutorial/tutorial_skillsbase_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com Base de habilidades com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador equivalente na Base de habilidades para um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado na Base de habilidades deve ser estabelecido.

Para configurar e testar o Azure AD início de sessão único com a Base de habilidades, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste de Base de habilidades](#create-a-skills-base-test-user)**  - para ter um equivalente da Eduarda Almeida na Base de habilidades que está ligado à representação de utilizador do Azure AD.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único na sua aplicação de Base de habilidades.

**Para configurar o Azure AD início de sessão único com a Base de habilidades, execute os seguintes passos:**

1. No portal do Azure, sobre o **Base de habilidades** página de integração de aplicativo, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/skillsbase-tutorial/tutorial_skillsbase_samlbase.png)

3. Sobre o **habilidades de Base de domínio e URLs** secção, execute os seguintes passos:

    ![Domínio de Base de habilidades e URLs únicas início de sessão em informações](./media/skillsbase-tutorial/tutorial_skillsbase_url.png)

    Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://app.skills-base.com/o/<customer-unique-key>`

    > [!NOTE] 
    > Pode obter o URL de início de sessão da aplicação de Base de habilidades. . Início de sessão como administrador e para ir para o administrador -> Definições -> instância detalhes -> ligação de atalho. Copie o URL de início de sessão e cole-a no acima da caixa de texto.

4. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![O link de download de certificado](./media/skillsbase-tutorial/tutorial_skillsbase_certificate.png) 

5. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/skillsbase-tutorial/tutorial_general_400.png)

6. Numa janela do browser web diferente, início de sessão para a Base de habilidades como um administrador de segurança.

7. Do lado esquerdo do menu, sob **administrador** clique em **autenticação**.

    ![O administrador](./media/skillsbase-tutorial/tutorial_skillsbase_auth.png)

8. Sobre o **autenticação** página, selecione início de sessão único como **SAML 2**.

    ![A única](./media/skillsbase-tutorial/tutorial_skillsbase_single.png)

9. Sobre o **autenticação** página, execute os seguintes passos:

    ![A única](./media/skillsbase-tutorial/tutorial_skillsbase_save.png)

    a. Clique em **metadados de atualização IdP** junto a **estado** opção e cole o conteúdo do XML de metadados que transferiu a partir do portal do Azure na caixa de texto especificado.

    > [!Note]
    > Também pode validar se os metadados de idp por meio da **validação de metadados do** ferramenta como realçada na captura de ecrã acima.

    b. Clique em **Guardar**.
    
### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/skillsbase-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/skillsbase-tutorial/create_aaduser_02.png)

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/skillsbase-tutorial/create_aaduser_03.png)

4. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/skillsbase-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-skills-base-test-user"></a>Criar um utilizador de teste de Base de habilidades

O objetivo desta secção é criar um utilizador chamado Eduarda Almeida na Base de habilidades. Base de habilidades suporta o aprovisionamento de just-in-time, que está por predefinição, ativada. Não existe nenhum item de ação para nesta secção. Um novo utilizador é criado durante a tentativa de aceder à Base de habilidades, se não existir ainda.

>[!Note]
>Se precisar de criar manualmente um utilizador, siga as instruções [aqui](http://wiki.skills-base.net/index.php?title=Adding_people_and_enabling_them_to_log_in).

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso à Base de habilidades.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a Base de habilidades, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **Base de habilidades**.

    ![A ligação de Base de habilidades na lista de aplicações](./media/skillsbase-tutorial/tutorial_skillsbase_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico da Base de habilidades no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação de Base de habilidades.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/skillsbase-tutorial/tutorial_general_01.png
[2]: ./media/skillsbase-tutorial/tutorial_general_02.png
[3]: ./media/skillsbase-tutorial/tutorial_general_03.png
[4]: ./media/skillsbase-tutorial/tutorial_general_04.png

[100]: ./media/skillsbase-tutorial/tutorial_general_100.png

[200]: ./media/skillsbase-tutorial/tutorial_general_200.png
[201]: ./media/skillsbase-tutorial/tutorial_general_201.png
[202]: ./media/skillsbase-tutorial/tutorial_general_202.png
[203]: ./media/skillsbase-tutorial/tutorial_general_203.png

