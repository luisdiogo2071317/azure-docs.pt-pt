---
title: 'Tutorial: Integração do Active Directory do Azure com o Exchange de informações de Alcumus | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Alcumus informações do Exchange.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: d26034b8-f0d5-4f65-aa56-0fc168ceec8c
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2017
ms.author: jeedes
ms.openlocfilehash: d55aa873d0ad410b91dc678d1fe4784e00ed5487
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/23/2019
ms.locfileid: "54812476"
---
# <a name="tutorial-azure-active-directory-integration-with-alcumus-info-exchange"></a>Tutorial: Integração do Active Directory do Azure com o Exchange de informações de Alcumus

Neste tutorial, saiba como integrar Alcumus informações do Exchange com o Azure Active Directory (Azure AD).

Integrar Alcumus informações do Exchange com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Exchange de informações de Alcumus
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para o Exchange de informações de Alcumus (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Alcumus informações do Exchange, terá dos seguintes itens:

- Uma subscrição do Azure
- Um Exchange de informações de Alcumus início de sessão único na subscrição ativado

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionar Alcumus informações do Exchange a partir da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-alcumus-info-exchange-from-the-gallery"></a>Adicionar Alcumus informações do Exchange a partir da Galeria
Para configurar a integração do Exchange de informações de Alcumus com o Azure AD, terá de adicionar Alcumus informações do Exchange a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Alcumus informações do Exchange a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

4. Na caixa de pesquisa, escreva **Alcumus informações Exchange**.

    ![Criar um utilizador de teste do Azure AD](./media/alcumus-info-tutorial/tutorial_alcumusinfoexchange_search.png)

5. No painel de resultados, selecione **Alcumus informações Exchange**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/alcumus-info-tutorial/tutorial_alcumusinfoexchange_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com o Exchange de informações de Alcumus com base num utilizador de teste chamado "Eduarda Almeida."

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador equivalente na troca de informações de Alcumus a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado na troca de informações de Alcumus deve ser estabelecido.

Em troca de informações de Alcumus, atribuir o valor do **nome de utilizador** no Azure AD como o valor da **nome de utilizador** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com o Alcumus informações do Exchange, terá de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste do Exchange de informações de Alcumus](#creating-an-alcumus-info-exchange-test-user)**  - para ter um equivalente da Eduarda Almeida na troca de informações de Alcumus que está ligado à representação de utilizador do Azure AD.
4. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo Alcumus informações Exchange.

**Para configurar o Azure AD início de sessão único com o Exchange de informações de Alcumus, execute os seguintes passos:**

1. No portal do Azure, sobre o **Alcumus informações Exchange** página de integração de aplicativo, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/alcumus-info-tutorial/tutorial_alcumusinfoexchange_samlbase.png)

3. Sobre o **Alcumus informações Exchange domínio e URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/alcumus-info-tutorial/tutorial_alcumusinfoexchange_url.png)

    a. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://<subdomain>.info-exchange.com`

    b. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://<subdomain>.info-exchange.com/Auth/`

    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o identificador real e o URL de resposta. Contacte [equipa de suporte do Exchange de informações de Alcumus](mailto:helpdesk@alcumusgroup.com) obter esses valores.
 
4. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![Configurar o início de sessão único](./media/alcumus-info-tutorial/tutorial_alcumusinfoexchange_certificate.png) 

5. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/alcumus-info-tutorial/tutorial_general_400.png)

6. Para configurar o início de sessão único num **Alcumus informações Exchange** lado, terá de enviar o transferido **XML de metadados** para [equipa de suporte do Exchange de informações de Alcumus](mailto:helpdesk@alcumusgroup.com).

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [Documentação do Azure AD incorporado]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/alcumus-info-tutorial/create_aaduser_01.png) 

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/alcumus-info-tutorial/create_aaduser_02.png) 

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/alcumus-info-tutorial/create_aaduser_03.png) 

4. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/alcumus-info-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-an-alcumus-info-exchange-test-user"></a>Criar um utilizador de teste do Exchange de informações de Alcumus

O objetivo desta secção é criar um utilizador chamado Eduarda Almeida na troca de informações de Alcumus.

Para criar um usuário chamado Eduarda Almeida na troca de informações de Alcumus, entre em contato com o [equipa de suporte do Exchange de informações de Alcumus](mailto:helpdesk@alcumusgroup.com).

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso ao Exchange de informações de Alcumus.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida a Alcumus informações do Exchange, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **Alcumus informações Exchange**.

    ![Configurar o início de sessão único](./media/alcumus-info-tutorial/tutorial_alcumusinfoexchange_app.png) 

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

O objetivo desta secção é testar a configuração do Azure AD única início de sessão com o painel de acesso.  
Quando clica no mosaico do Exchange de informações de Alcumus no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação Alcumus informações Exchange.

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[1]: ./media/alcumus-info-tutorial/tutorial_general_01.png
[2]: ./media/alcumus-info-tutorial/tutorial_general_02.png
[3]: ./media/alcumus-info-tutorial/tutorial_general_03.png
[4]: ./media/alcumus-info-tutorial/tutorial_general_04.png

[100]: ./media/alcumus-info-tutorial/tutorial_general_100.png

[200]: ./media/alcumus-info-tutorial/tutorial_general_200.png
[201]: ./media/alcumus-info-tutorial/tutorial_general_201.png
[202]: ./media/alcumus-info-tutorial/tutorial_general_202.png
[203]: ./media/alcumus-info-tutorial/tutorial_general_203.png

