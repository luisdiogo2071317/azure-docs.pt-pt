---
title: 'Tutorial: Integração do Azure Active Directory com o GoToMeeting | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e o GoToMeeting.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: bcaf19f2-5809-4e1c-acbc-21a8d3498ccf
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/02/2018
ms.author: jeedes
ms.openlocfilehash: b62b3b7f9f3bfd55237ed4d894954a0bde48e7fc
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39043725"
---
# <a name="tutorial-azure-active-directory-integration-with-gotomeeting"></a>Tutorial: Integração do Azure Active Directory com o GoToMeeting

Neste tutorial, saiba como integrar o GoToMeeting com o Azure Active Directory (Azure AD).

Integrar o GoToMeeting com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao GoToMeeting.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para GoToMeeting (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o GoToMeeting, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um GoToMeeting logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando o GoToMeeting da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-gotomeeting-from-the-gallery"></a>Adicionando o GoToMeeting da Galeria
Para configurar a integração do GoToMeeting para o Azure AD, terá de adicionar o GoToMeeting a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar o GoToMeeting a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

4. Na caixa de pesquisa, escreva **GoToMeeting**, selecione **GoToMeeting** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![GoToMeeting na lista de resultados](./media/citrix-gotomeeting-tutorial/tutorial_gotomeeting_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com o GoToMeeting com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no GoToMeeting para um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no GoToMeeting deve ser estabelecido.

No GoToMeeting, atribuir o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com o GoToMeeting, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste GoToMeeting](#create-a-gotomeeting-test-user)**  - para ter um equivalente da Eduarda Almeida no GoToMeeting que está ligado à representação de utilizador do Azure AD.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo GoToMeeting.

**Para configurar o Azure AD início de sessão único com o GoToMeeting, execute os seguintes passos:**

1. No portal do Azure, sobre o **GoToMeeting** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/citrix-gotomeeting-tutorial/tutorial_gotomeeting_samlbase.png)

3. Sobre o **GoToMeeting domínio e URLs** secção, execute os seguintes passos:

    ![O GoToMeeting domínio e URLs únicas início de sessão em informações](./media/citrix-gotomeeting-tutorial/tutorial_gotomeeting_url.png)

    Na **identificador** caixa de texto, escreva o URL: `https://authentication.logmeininc.com/saml/sp`

4. Clique em **configuração de mostrar URL avançado** e configurar o abaixo URLs

    **Inicie sessão no URL** (manter isso em branco)
    
    **URL de resposta**: `https://authentication.logmeininc.com/saml/acs`
    
    **RelayState**:
    
    - Para a aplicação GoToMeeting, a utilização `https://global.gotomeeting.com`
    
    - Para GoToTraining, utilize `https://global.gototraining.com`
    
    - Para GoToWebinar, utilize `https://global.gotowebinar.com` 
    
    - Para GoToAssist, utilize `https://app.gotoassist.com`
    
5. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/citrix-gotomeeting-tutorial/tutorial_general_400.png)

6. Numa janela do browser diferente, inicie sessão no seu [GoToMeeting organização Center](https://organization.logmeininc.com/). Será solicitado a confirmar que o IdP foi atualizada

7. Ative a caixa de verificação "Meu provedor de identidade foi atualizado com o novo domínio". Clique em **feito** quando terminar.


> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/citrix-gotomeeting-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/citrix-gotomeeting-tutorial/create_aaduser_02.png)

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/citrix-gotomeeting-tutorial/create_aaduser_03.png)

4. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/citrix-gotomeeting-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-gotomeeting-test-user"></a>Criar um utilizador de teste GoToMeeting

Nesta secção, um usuário chamado Eduarda Almeida é criado no GoToMeeting. O GoToMeeting suporta o aprovisionamento de just-in-time, que está ativado por predefinição.

Não existe nenhum item de ação para nesta secção. Se um utilizador já não existir no GoToMeeting, uma nova é criada quando tentar acessar o GoToMeeting.

> [!NOTE]
> Se precisar de criar manualmente um utilizador, contacte [equipa de suporte do GoToMeeting](https://support.logmeininc.com/gotomeeting).

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para GoToMeeting.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a GoToMeeting, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **GoToMeeting**.

    ![A ligação de GoToMeeting, na lista de aplicações](./media/citrix-gotomeeting-tutorial/tutorial_gotomeeting_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico GoToMeeting no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação GoToMeeting.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)
* [Configurar o aprovisionamento do utilizador](https://docs.microsoft.com/azure/active-directory/active-directory-saas-citrixgotomeeting-provisioning-tutorial)


<!--Image references-->

[1]: ./media/gotomeeting-tutorial/tutorial_general_01.png
[2]: ./media/gotomeeting-tutorial/tutorial_general_02.png
[3]: ./media/gotomeeting-tutorial/tutorial_general_03.png
[4]: ./media/gotomeeting-tutorial/tutorial_general_04.png

[100]: ./media/gotomeeting-tutorial/tutorial_general_100.png

[200]: ./media/gotomeeting-tutorial/tutorial_general_200.png
[201]: ./media/gotomeeting-tutorial/tutorial_general_201.png
[202]: ./media/gotomeeting-tutorial/tutorial_general_202.png
[203]: ./media/gotomeeting-tutorial/tutorial_general_203.png

