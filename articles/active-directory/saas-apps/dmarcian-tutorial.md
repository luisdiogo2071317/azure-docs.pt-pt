---
title: 'Tutorial: Integração do Azure Active Directory com dmarcian | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e dmarcian.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: a04b9383-3a60-4d54-9412-123daaddff3b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/21/2018
ms.author: jeedes
ms.openlocfilehash: 0f8878505280371bf6046c1d1f0d7fc1275dd496
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "47039892"
---
# <a name="tutorial-azure-active-directory-integration-with-dmarcian"></a>Tutorial: Integração do Azure Active Directory com dmarcian

Neste tutorial, saiba como integrar dmarcian com o Azure Active Directory (Azure AD).

Integrar dmarcian no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao dmarcian.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para dmarcian (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com dmarcian, terá dos seguintes itens:

- Uma subscrição do Azure
- Um dmarcian logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando dmarcian da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-dmarcian-from-the-gallery"></a>Adicionando dmarcian da Galeria
Para configurar a integração do dmarcian com o Azure AD, terá de adicionar dmarcian a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar dmarcian a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações empresariais][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

4. Na caixa de pesquisa, escreva **dmarcian**, selecione **dmarcian** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![dmarcian na lista de resultados](./media/dmarcian-tutorial/tutorial_dmarcian_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com dmarcian com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no dmarcian a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no dmarcian deve ser estabelecido.

Para configurar e testar o Azure AD início de sessão único com dmarcian, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste dmarcian](#create-a-dmarcian-test-user)**  - para ter um equivalente da Eduarda Almeida na dmarcian que está ligado à representação de utilizador do Azure AD.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo dmarcian.

**Para configurar o Azure AD início de sessão único com dmarcian, execute os seguintes passos:**

1. No portal do Azure, sobre o **dmarcian** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/dmarcian-tutorial/tutorial_dmarcian_samlbase.png)

3. Sobre o **dmarcian domínio e URLs** secção, execute os seguintes passos, se desejar configurar a aplicação no **IDP** iniciada pelo modo:

    ![dmarcian domínio e URLs únicas início de sessão em informações](./media/dmarcian-tutorial/tutorial_dmarcian_url.png)

    a. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão:
    | |
    | -- |
    | `https://us.dmarcian.com/sso/saml/<ACCOUNT_ID>/sp.xml` |
    | `https://dmarcian-eu.com/sso/saml/<ACCOUNT_ID>/sp.xml` |
    | `https://dmarcian-ap.com/sso/saml/<ACCOUNT_ID>/sp.xml` |

    b. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão:
    | |
    |--|
    | `https://us.dmarcian.com/login/<ACCOUNT_ID>/handle/` |
    | `https://dmarcian-eu.com/login/<ACCOUNT_ID>/handle/` |
    | `https://dmarcian-ap.com/login/<ACCOUNT_ID>/handle/` |

4. Verifique **Mostrar definições de URL avançadas** e executar o passo seguinte, se desejar configurar a aplicação na **SP** iniciada pelo modo:

    ![dmarcian domínio e URLs únicas início de sessão em informações](./media/dmarcian-tutorial/tutorial_dmarcian_url1.png)

    Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão:
    | |
    |--|
    | `https://us.dmarcian.com/login/<ACCOUNT_ID>` |
    | `https://dmarcian-eu.com/login/<ACCOUNT_ID>` |
    | `https://dmarciam-ap.com/login/<ACCOUNT_ID>` |
     
    > [!NOTE] 
    > Estes valores não são reais. Atualizar estes valores com o identificador real, o URL de resposta e URL de início de sessão que é explicado mais tarde no tutorial. 

5. Sobre o **certificado de assinatura SAML** secção, clique no botão de cópia para copiar **Url de metadados de Federação de aplicação** e cole-o no bloco de notas.

    ![O link de download de certificado](./media/dmarcian-tutorial/tutorial_dmarcian_certificate.png) 

6. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/dmarcian-tutorial/tutorial_general_400.png)
    
7. Numa janela do browser web diferente, inicie sessão no dmarcian como um administrador de segurança.

8. Clique em **perfil** no canto superior direito canto e navegue até à **preferências**.

    ![As preferências ](./media/dmarcian-tutorial/tutorial_dmarcian_pref.png)

9. Desloque para baixo e clique em **início de sessão único** secção, em seguida, clique em **configurar**.

    ![A única ](./media/dmarcian-tutorial/tutorial_dmarcian_sso.png)

10. Sobre o **SAML Single Sign-On** página conjunto a **estado** como **ativado** e execute os seguintes passos:

    ![A autenticação ](./media/dmarcian-tutorial/tutorial_dmarcian_auth.png)

    * Sob **adicionar dmarcian ao seu fornecedor de identidade** secção, clique em **cópia** para copiar o **URL do serviço de consumidor de asserção** para a sua instância e cole-a no  **URL de resposta** na caixa de texto **dmarcian secção de domínio e URLs** no portal do Azure.

    * Sob **adicionar dmarcian ao seu fornecedor de identidade** secção, clique em **cópia** para copiar o **ID de entidade** para a sua instância e cole-a no **identificador**na caixa de texto **dmarcian secção de domínio e URLs** no portal do Azure.

    * Sob **configurar a autenticação** na secção a **metadados do fornecedor de identidade** colar de caixa de texto a **Url de metadados de Federação de aplicação**, que copiou do portal do Azure.

    * Sob **configurar a autenticação** secção, além do **declarações de atributo** caixa de texto, cole o url `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.

    * Sob **configurar o URL de início de sessão** secção, copie a **URL de início de sessão** para a sua instância e cole-a no **URL de início de sessão** caixa de texto no **dmarcian secção do domínio e URLs** no portal do Azure.

        > [!Note]
        > Pode modificar os **URL de início de sessão** , de acordo com sua organização.

    * Clique em **Guardar**.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/dmarcian-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/dmarcian-tutorial/create_aaduser_02.png)

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/dmarcian-tutorial/create_aaduser_03.png)

4. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/dmarcian-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-dmarcian-test-user"></a>Criar um utilizador de teste dmarcian

Para ativar a utilizadores do Azure AD iniciar sessão no dmarcian, tem de ser aprovisionados em dmarcian. Dmarcian, aprovisionamento é uma tarefa manual.

**Para Aprovisionar uma conta de utilizador, execute os seguintes passos:**

1. Inicie sessão no dmarcian como um administrador de segurança.

2. Clique em **perfil** no canto superior direito canto e navegue até à **Manage Users**.

    ![O utilizador ](./media/dmarcian-tutorial/tutorial_dmarcian_user.png)

3. No lado direito da **utilizadores SSO** secção, clique em **adicionar novo utilizador**.

    ![O adicionar utilizador ](./media/dmarcian-tutorial/tutorial_dmarcian_addnewuser.png)

4. Sobre o **adicionar novo utilizador** pop-up, execute os seguintes passos:

    ![O novo utilizador ](./media/dmarcian-tutorial/tutorial_dmarcian_save.png)

    a. Na **novo E-Mail do utilizador** caixa de texto, introduza o e-mail do utilizador, como **brittasimon@contoso.com**.

    b. Se pretende conceder direitos de administrador para o utilizador, selecione **fazer com que o utilizador seja um administrador**.

    c. Clique em **adicionar utilizador**.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso a dmarcian.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a dmarcian, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **dmarcian**.

    ![A ligação de dmarcian na lista de aplicações](./media/dmarcian-tutorial/tutorial_dmarcian_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar o início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico dmarcian no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo dmarcian.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/dmarcian-tutorial/tutorial_general_01.png
[2]: ./media/dmarcian-tutorial/tutorial_general_02.png
[3]: ./media/dmarcian-tutorial/tutorial_general_03.png
[4]: ./media/dmarcian-tutorial/tutorial_general_04.png

[100]: ./media/dmarcian-tutorial/tutorial_general_100.png

[200]: ./media/dmarcian-tutorial/tutorial_general_200.png
[201]: ./media/dmarcian-tutorial/tutorial_general_201.png
[202]: ./media/dmarcian-tutorial/tutorial_general_202.png
[203]: ./media/dmarcian-tutorial/tutorial_general_203.png

