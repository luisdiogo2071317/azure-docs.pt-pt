---
title: 'Tutorial: Integração do Active Directory do Azure com Huddle | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Huddle.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 8389ba4c-f5f8-4ede-b2f4-32eae844ceb0
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/08/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 73beb4ba039db65c20886566f887db39fd233164
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2019
ms.locfileid: "56182668"
---
# <a name="tutorial-azure-active-directory-integration-with-huddle"></a>Tutorial: Integração do Active Directory do Azure com Huddle

Neste tutorial, saiba como integrar Huddle com o Azure Active Directory (Azure AD).

Integrar Huddle no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Huddle
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Huddle (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Huddle, terá dos seguintes itens:

- Uma subscrição do Azure
- Um Huddle logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Huddle da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-huddle-from-the-gallery"></a>Adicionando Huddle da Galeria

Para configurar a integração do Huddle com o Azure AD, terá de adicionar Huddle a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Huddle a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]

3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

4. Na caixa de pesquisa, escreva **Huddle**. Selecione **Huddle** no painel de resultados e, em seguida, clique o **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/huddle-tutorial/tutorial_huddle_addfromgallery.png)

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com Huddle com base num utilizador de teste chamado "Eduarda Almeida."

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Huddle a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Huddle deve ser estabelecido.

Para configurar e testar o Azure AD início de sessão único com Huddle, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste Huddle](#creating-a-huddle-test-user)**  - para ter um equivalente da Eduarda Almeida na Huddle que está ligado à representação de utilizador do Azure AD.
4. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo Huddle.

**Para configurar o Azure AD início de sessão único com Huddle, execute os seguintes passos:**

1. No portal do Azure, sobre o **Huddle** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

2. Clique em **modo de início de sessão único de alteração** na parte superior do ecrã para selecionar o **SAML** modo.

    ![Configurar o início de sessão único](./media/huddle-tutorial/tutorial_general_300.png)

3. Sobre o **selecionar um método de início de sessão único** caixa de diálogo, clique em **selecione** para **SAML** modo para ativar o início de sessão único.

    ![Configurar o início de sessão único](./media/huddle-tutorial/tutorial_general_301.png)

4. Sobre o **definir a segurança de início de sessão único com o SAML** página, clique em **editar** ícone para abrir **configuração básica de SAML** caixa de diálogo.

    ![Configurar o início de sessão único](./media/huddle-tutorial/tutorial_general_302.png)

5. Sobre o **configuração básica de SAML** secção execute os seguintes passos, se desejar configurar a aplicação no **IDP** intiated modo:

    > [!NOTE]
    > A instância de huddle será automaticamente detetada do domínio, que introduza abaixo.

    ![Huddle domínio e URLs únicas início de sessão em informações](./media/huddle-tutorial/tutorial_huddle_url.png)

    a. Na **identificador** caixa de texto, escreva um URL:

    | | |
    |--|--|
    | `https://login.huddle.net`|
    | `https://login.huddle.com`|
    | |

    b. Na **URL de resposta** caixa de texto, escreva um URL:

    | | |
    |--|--|
    | `https://login.huddle.net/saml/browser-sso`|
    | `https://login.huddle.com/saml/browser-sso`|
    | `https://login.huddle.com/saml/idp-initiated-sso`|
    | |

    c. Clique em **definir URLs adicionais** e executar o passo seguinte, se desejar configurar a aplicação na **SP** iniciada pelo modo:

    ![Huddle domínio e URLs únicas início de sessão em informações](./media/huddle-tutorial/tutorial_huddle_url1.png)

    Na **URL de início de sessão** caixa de texto, escreva qualquer um da URL usando o seguinte padrão:

    | | |
    |--|--|
    | `https://<customsubdomain>.huddle.com`|
    | `https://us.huddle.com`|
    | |

    > [!NOTE]
    > O valor de URL de início de sessão não é real. Atualize este valor com o URL de início de sessão real. Contacte [equipa de suporte de cliente Huddle](https://huddle.zendesk.com) para obter este valor.

6. No **definir a segurança de início de sessão único com o SAML** na página a **certificado de assinatura SAML** secção, clique em **transferir** para transferir o certificado apropriado, de acordo sua requisito e guarde-o no seu computador.

    ![Configurar o início de sessão único](./media/huddle-tutorial/tutorial_huddle_certificate.png)

7. Sobre o **configurar Huddle** secção, copie os URLs apropriados de acordo com seus requisitos.

    ![Configurar o início de sessão único](./media/huddle-tutorial/tutorial_huddle_configure.png)

8. Para configurar o início de sessão único num **Huddle** lado, terá de enviar o certificado que transferiu e os URLs que copiou do **configurar** **Huddle** secção no portal do Azure para [equipa de suporte de cliente Huddle](https://huddle.zendesk.com). Se definir esta definição para que a ligação de SAML SSO definidas corretamente em ambos os lados.

    >[!NOTE]
    > Início de sessão único tem de ser ativado pela equipe de suporte do Huddle. Receba uma notificação quando a configuração foi concluída.

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

1. No portal do Azure, no painel esquerdo, selecione **do Azure Active Directory**, selecione **utilizadores**e, em seguida, selecione **todos os utilizadores**.

    ![Criar utilizador do Azure AD][100]

2. Selecione **novo utilizador** na parte superior do ecrã.

    ![Criar um utilizador de teste do Azure AD](./media/huddle-tutorial/create_aaduser_01.png) 

3. Nas propriedades do utilizador, execute os seguintes passos.

    ![Criar um utilizador de teste do Azure AD](./media/huddle-tutorial/create_aaduser_02.png)

    a. Na **Name** campo introduza **BrittaSimon**.
  
    b. Na **nome de utilizador** tipo de campo **brittasimon@yourcompanydomain.extension**  
    Por exemplo, BrittaSimon@contoso.com

    c. Selecione **propriedades**, selecione a **palavra-passe de Show** caixa de verificação e, em seguida, anote o valor que é apresentado na caixa de palavra-passe.

    d. Selecione **Criar**.

### <a name="creating-a-huddle-test-user"></a>Criar um utilizador de teste Huddle

Para ativar a utilizadores do Azure AD iniciar sessão no Huddle, tem de ser aprovisionados em Huddle. No caso de Huddle, o aprovisionamento é uma tarefa manual.

**Para configurar o aprovisionamento de utilizadores, execute os seguintes passos:**

1. Inicie sessão no seu **Huddle** site da empresa como administrador.

2. Clique em **área de trabalho**.

3. Clique em **pessoas \> convidar pessoas**.

    ![As pessoas](./media/huddle-tutorial/IC787838.png "pessoas")

4. Na **criar um novo convite** secção, execute os seguintes passos:
  
    ![Novo convite](./media/huddle-tutorial/IC787839.png "novo convite")
  
    a. Na **escolha uma equipe para convidar pessoas para ingressar** lista, selecione **equipe**.

    b. Tipo de **endereço de E-Mail** de um Azure válido conta AD que pretende aprovisionar para **introduza o endereço de e-mail para pessoas que gostaria de convidar** caixa de texto.

    c. Clique em **convidar**.

    >[!NOTE]
    > O titular da conta do Azure AD irá receber um e-mail, incluindo uma ligação para confirmar a conta até se tornar Active Directory.

>[!NOTE]
>Pode utilizar quaisquer outras ferramentas de criação da conta de utilizador de Huddle ou APIs fornecidas pelos Huddle para aprovisionar contas de utilizador do Azure AD.

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para Huddle.

1. No portal do Azure, selecione **aplicações empresariais**, selecione **todos os aplicativos**.

    ![Atribuir utilizador][201]

2. Na lista de aplicações, selecione **Huddle**.

    ![Configurar o início de sessão único](./media/huddle-tutorial/tutorial_huddle_app.png) 

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

5. Na **utilizadores e grupos** caixa de diálogo select **Eduarda Almeida** na lista de utilizadores, em seguida, clique o **selecionar** na parte inferior do ecrã.

6. Na **adicionar atribuição** caixa de diálogo select a **atribuir** botão.

### <a name="testing-single-sign-on"></a>Teste de início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico Huddle no painel de acesso, deve obter automaticamente a página de início de sessão de aplicativo Huddle.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[1]: ./media/huddle-tutorial/tutorial_general_01.png
[2]: ./media/huddle-tutorial/tutorial_general_02.png
[3]: ./media/huddle-tutorial/tutorial_general_03.png
[4]: ./media/huddle-tutorial/tutorial_general_04.png
[100]: ./media/huddle-tutorial/tutorial_general_100.png
[200]: ./media/huddle-tutorial/tutorial_general_200.png
[201]: ./media/huddle-tutorial/tutorial_general_201.png
[202]: ./media/huddle-tutorial/tutorial_general_202.png
[203]: ./media/huddle-tutorial/tutorial_general_203.png
