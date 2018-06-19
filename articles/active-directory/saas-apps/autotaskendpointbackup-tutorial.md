---
title: 'Tutorial: Integração do Azure Active Directory com cópia de segurança de ponto final de Autotask | Microsoft Docs'
description: Saiba como configurar o início de sessão entre o Azure Active Directory e a cópia de segurança do Autotask ponto final.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 9f55319e-895b-4130-8460-71713f25ed04
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/11/2018
ms.author: jeedes
ms.openlocfilehash: ef68047542e8d1f9f8cc1f997e5ceca2dfa84cef
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/18/2018
ms.locfileid: "35969427"
---
# <a name="tutorial-azure-active-directory-integration-with-autotask-endpoint-backup"></a>Tutorial: Integração do Azure Active Directory com cópia de segurança de ponto final de Autotask

Neste tutorial, irá aprender a integração de cópia de segurança de ponto final de Autotask com o Azure Active Directory (Azure AD).

A integração de cópia de segurança do Autotask ponto final com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso à cópia de segurança do Autotask ponto final.
- Pode permitir aos utilizadores automaticamente obter com sessão iniciada a cópia de segurança de ponto final de Autotask (Single Sign-On) com as respetivas contas do Azure AD.
- Pode gerir as contas numa localização central - portal do Azure.

Se pretender saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [que é o acesso a aplicações e início de sessão no Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do AD do Azure com cópia de segurança de ponto final de Autotask, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Uma cópia de segurança de ponto final de Autotask-início de sessão único ativada subscrição

> [!NOTE]
> Para testar os passos neste tutorial, não recomendamos a utilização num ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, teste do Azure AD-início de sessão único num ambiente de teste.
O cenário descrito neste tutorial consiste em dois blocos modulares principais:

1. A adição de cópia de segurança de ponto final de Autotask de galeria
2. Configurar e testar o Azure AD de sessão único-

## <a name="adding-autotask-endpoint-backup-from-the-gallery"></a>A adição de cópia de segurança de ponto final de Autotask de galeria
Para configurar a integração de cópia de segurança do Autotask ponto final com o Azure AD, tem de adicionar Autotask cópia de segurança do ponto final da Galeria à sua lista de aplicações SaaS geridas.

**Para adicionar a cópia de segurança de ponto final de Autotask na galeria do, execute os seguintes passos:**

1. No  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **do Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todas as aplicações**.

    ![O painel de aplicações da empresa][2]

3. Para adicionar a nova aplicação, clique em **nova aplicação** botão no topo da caixa de diálogo.

    ![O novo botão de aplicação][3]

4. Na caixa de pesquisa, escreva **cópia de segurança de ponto final de Autotask**, selecione **cópia de segurança de ponto final de Autotask** partir do painel de resultados, em seguida, clique em **adicionar** botão para adicionar a aplicação.

    ![Cópia de segurança de ponto final de Autotask na lista de resultados](./media/autotaskendpointbackup-tutorial/tutorial_autotaskendpointbackup_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD-início de sessão único

Nesta secção, configure e teste do Azure AD-início de sessão único com cópia de segurança do ponto final de Autotask com base num utilizador de teste chamado "Britta Simon".

Para início de sessão trabalhar, do Azure AD tem de saber o que o utilizador homólogo na cópia de segurança de ponto final de Autotask é um utilizador no Azure AD. Por outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado na cópia de segurança do Autotask ponto final tem de ser estabelecida.

Para configurar e testar o Azure AD-início de sessão único com cópia de segurança de ponto final de Autotask, tem de concluir os blocos modulares seguintes:

1. **[Configurar o Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)**  - para permitir aos utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD-início de sessão único com Britta Simon.
3. **[Criar um utilizador de teste de cópia de segurança de ponto final de Autotask](#create-a-autotask-endpoint-backup-test-user)**  - para ter um homólogo de Britta Simon na cópia de segurança do ponto final Autotask que está ligada a representação do Azure AD do utilizador.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar Britta Simon utilizar o Azure AD-início de sessão único.
5. **[Teste o início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD-início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão no portal do Azure e configurar o início de sessão único na sua aplicação de cópia de segurança do Autotask ponto final.

**Para configurar o Azure AD-início de sessão único com cópia de segurança de ponto final de Autotask, execute os seguintes passos:**

1. No portal do Azure, no **cópia de segurança de ponto final de Autotask** página de integração de aplicações, clique em **de sessão único-**.

    ![Configurar a ligação de início de sessão único][4]

2. No **de sessão único-** caixa de diálogo, selecione **modo** como **baseados em SAML início de sessão** para ativar o início de sessão único.

    ![Caixa de diálogo de início de sessão único](./media/autotaskendpointbackup-tutorial/tutorial_autotaskendpointbackup_samlbase.png)

3. No **Autotask ponto final de cópia de segurança do domínio e os URLs** secção, execute os seguintes passos:

    ![Domínio de cópia de segurança do ponto final de Autotask e URLs único informações de início de sessão](./media/autotaskendpointbackup-tutorial/tutorial_autotaskendpointbackup_url.png)

    a. No **identificador** caixa de texto, escreva um URL a utilizar o padrão do seguinte: `https://<subdomain>.backup.autotask.net/singlesignon/saml/metadata`

    b. No **URL de resposta** caixa de texto, escreva um URL a utilizar o padrão do seguinte: `https://<subdomain>.backup.autotask.net/singlesignon/saml/SSO`

    > [!NOTE]
    > Estes valores não estiverem reais. Atualize estes valores com o identificador e o URL de resposta real. Contacte [equipa de suporte de cópia de segurança de ponto final de Autotask](https://backup.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm) para obter estes valores.

4. No **certificado de assinatura de SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![A hiperligação de transferência do certificado](./media/autotaskendpointbackup-tutorial/tutorial_autotaskendpointbackup_certificate.png) 

5. Clique em **guardar** botão.

    ![Configurar botão único início de sessão guardar](./media/autotaskendpointbackup-tutorial/tutorial_general_400.png)

6. Para configurar o início de sessão único em **cópia de segurança de ponto final de Autotask** lado, terá de enviar o transferido **XML de metadados** para [equipa de suporte de cópia de segurança de ponto final de Autotask](https://backup.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm). Se definir esta definição para que a ligação de SAML SSO corretamente em ambos os lados.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção consiste em criar um utilizador de teste no portal do Azure chamado Britta Simon.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique em de **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/autotaskendpointbackup-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e "Todos os utilizadores" ligações](./media/autotaskendpointbackup-tutorial/create_aaduser_02.png)

3. Para abrir o **utilizador** caixa de diálogo, clique em **adicionar** na parte superior do **todos os utilizadores** caixa de diálogo.

    ![O botão de adição](./media/autotaskendpointbackup-tutorial/create_aaduser_03.png)

4. No **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/autotaskendpointbackup-tutorial/create_aaduser_04.png)

    a. No **nome** caixa, escreva **BrittaSimon**.

    b. No **nome de utilizador** caixa, escreva o endereço de e-mail do utilizador Britta Simon.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado no **palavra-passe** caixa.

    d. Clique em **Criar**.
  
### <a name="create-a-autotask-endpoint-backup-test-user"></a>Criar um utilizador de teste de cópia de segurança de ponto final de Autotask

Nesta secção, vai criar um utilizador chamado Britta Simon na cópia de segurança do Autotask ponto final. Trabalhar com [equipa de suporte de cópia de segurança de ponto final de Autotask](https://backup.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm) para adicionar os utilizadores na plataforma de cópia de segurança do Autotask ponto final. Os utilizadores tem de ser criados e ativados antes de utilizar o início de sessão único.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Britta Simon utilizar o Azure-início de sessão único, concedendo acesso Autotask ponto final da cópia de segurança.

![Atribuir a função de utilizador][200]

**Para atribuir Britta Simon a cópia de segurança de ponto final de Autotask, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicações e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todas as aplicações**.

    ![Atribua o utilizador][201]

2. Na lista de aplicações, selecione **cópia de segurança de ponto final de Autotask**.

    ![A ligação de cópia de segurança de ponto final de Autotask na lista de aplicações](./media/autotaskendpointbackup-tutorial/tutorial_autotaskendpointbackup_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação de "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Britta Simon** na lista utilizadores.

6. Clique em **selecione** botão no **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribuir** botão no **adicionar atribuição** caixa de diálogo.

### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, testar a configuração do Azure AD único início de sessão através do painel de acesso.

Quando clica no mosaico de cópia de segurança de ponto final de Autotask no painel de acesso, deve obter automaticamente com sessão iniciada para a aplicação de cópia de segurança do Autotask ponto final.
Para mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/autotaskendpointbackup-tutorial/tutorial_general_01.png
[2]: ./media/autotaskendpointbackup-tutorial/tutorial_general_02.png
[3]: ./media/autotaskendpointbackup-tutorial/tutorial_general_03.png
[4]: ./media/autotaskendpointbackup-tutorial/tutorial_general_04.png

[100]: ./media/autotaskendpointbackup-tutorial/tutorial_general_100.png

[200]: ./media/autotaskendpointbackup-tutorial/tutorial_general_200.png
[201]: ./media/autotaskendpointbackup-tutorial/tutorial_general_201.png
[202]: ./media/autotaskendpointbackup-tutorial/tutorial_general_202.png
[203]: ./media/autotaskendpointbackup-tutorial/tutorial_general_203.png

