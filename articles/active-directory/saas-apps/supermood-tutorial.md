---
title: 'Tutorial: Integração do Azure Active Directory com Supermood | Microsoft Docs'
description: Saiba como configurar o início de sessão entre o Azure Active Directory e Supermood.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: afc04efa-2eba-4e47-8ce4-b71eb293cd09
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/12/2018
ms.author: jeedes
ms.openlocfilehash: 9cd6a373f23c69f920d0e46bad368f17c8d21035
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36215004"
---
# <a name="tutorial-azure-active-directory-integration-with-supermood"></a>Tutorial: Integração do Azure Active Directory com Supermood

Neste tutorial, irá aprender a integrar Supermood com o Azure Active Directory (Azure AD).

Integrar Supermood com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Supermood.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Supermood (Single Sign-On) com as respetivas contas do Azure AD.
- Pode gerir as contas numa localização central - portal do Azure.

Se pretender saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [que é o acesso a aplicações e início de sessão no Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Supermood, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Supermood-início de sessão único ativada subscrição

> [!NOTE]
> Para testar os passos neste tutorial, não recomendamos a utilização num ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, teste do Azure AD-início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos modulares principais:

1. Adicionar Supermood a partir da Galeria
2. Configurar e testar o Azure AD de sessão único-

## <a name="adding-supermood-from-the-gallery"></a>Adicionar Supermood a partir da Galeria
Para configurar a integração de Supermood com o Azure AD, terá de adicionar Supermood a partir da Galeria à sua lista de aplicações SaaS geridas.

**Para adicionar Supermood a partir da galeria, execute os seguintes passos:**

1. No  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **do Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todas as aplicações**.

    ![O painel de aplicações da empresa][2]
    
3. Para adicionar a nova aplicação, clique em **nova aplicação** botão no topo da caixa de diálogo.

    ![O novo botão de aplicação][3]

4. Na caixa de pesquisa, escreva **Supermood**, selecione **Supermood** partir do painel de resultados, em seguida, clique em **adicionar** botão para adicionar a aplicação.

    ![Supermood na lista de resultados](./media/supermood-tutorial/tutorial_supermood_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD-início de sessão único

Nesta secção, configure e teste do Azure AD-início de sessão único com Supermood com base num utilizador de teste chamado "Britta Simon".

Para início de sessão trabalhar, do Azure AD tem de saber o que o utilizador homólogo no Supermood é um utilizador no Azure AD. Por outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Supermood tem de ser estabelecida.

Para configurar e testar o Azure AD-início de sessão único com Supermood, tem de concluir os blocos modulares seguintes:

1. **[Configurar o Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)**  - para permitir aos utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD-início de sessão único com Britta Simon.
3. **[Criar um utilizador de teste Supermood](#create-a-supermood-test-user)**  - para ter um homólogo de Britta Simon Supermood que está ligada a representação do Azure AD do utilizador.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar Britta Simon utilizar o Azure AD-início de sessão único.
5. **[Teste o início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD-início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão no portal do Azure e configurar o início de sessão único na sua aplicação Supermood.

**Para configurar o Azure AD-início de sessão único com Supermood, execute os seguintes passos:**

1. No portal do Azure, no **Supermood** página de integração de aplicações, clique em **de sessão único-**.

    ![Configurar a ligação de início de sessão único][4]

2. No **de sessão único-** caixa de diálogo, selecione **modo** como **baseados em SAML início de sessão** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/supermood-tutorial/tutorial_supermood_samlbase.png)

3. No **Supermood domínio e os URLs** secção, execute os seguintes passos:

    ![Domínio supermood e os URLs únicos de informações de início de sessão](./media/supermood-tutorial/tutorial_supermood_url.png)

    a. Verifique **Mostrar avançadas definições de URL**.

    b. Se pretender configurar a aplicação no **IDP** iniciou modo, o **reencaminhamento estado** caixa de texto, escreva um URL: `https://supermood.co/auth/sso/saml20`

    c. Se pretender configurar a aplicação no **SP** iniciou modo, o **URL de início de sessão** caixa de texto, escreva um URL: `https://supermood.co/app/#!/loginv2`

4. Aplicação de supermood espera as asserções de SAML num formato específico. Configure as seguintes afirmações para esta aplicação. Pode gerir os valores destes atributos do **atributos de utilizador** secção na página de integração de aplicações. A seguinte captura de ecrã mostra um exemplo para este.
    
    ![Configurar o início de sessão único](./media/supermood-tutorial/tutorial_supermood_attribute.png)

5. No **atributos de utilizador** secção no **de sessão único-** caixa de diálogo, configurar atributos token SAML, conforme mostrado na imagem acima e execute os seguintes passos:
    
    | Nome do Atributo | Valor do Atributo |
    | ---------------| --------------- |    
    | firstName | User.givenName |
    | Apelido | User.Surname |

    a. Clique em **adicionar atributo** para abrir o **adicionar atributo** caixa de diálogo.

    ![Configurar o início de sessão único](./media/supermood-tutorial/tutorial_attribute_04.png)

    ![Configurar o início de sessão único](./media/supermood-tutorial/tutorial_attribute_05.png)
    
    b. No **nome** caixa de texto, escreva o nome de atributo apresentado para essa linha.
    
    c. Do **valor** lista, digite o valor de atributo apresentado para essa linha.

    d. Deixe o **espaço de nomes** em branco.
    
    d. Clique em **Ok**

6. No **certificado de assinatura de SAML** secção, clique no botão Copiar para copiar **Url de metadados de Federação de aplicação** e cole-o bloco de notas.

    ![A hiperligação de transferência do certificado](./media/supermood-tutorial/tutorial_supermood_certificate.png) 

7. Clique em **guardar** botão.

    ![Configurar botão único início de sessão guardar](./media/supermood-tutorial/tutorial_general_400.png)

8. Vá para o painel de administração Supermood.co como administrador de segurança.

9. Clique em **minha conta** (na parte inferior esquerda) e **único início de sessão (SSO)**.

    ![O certificado único](./media/supermood-tutorial/tutorial_supermood_single.png)
10. No **configurações do seu SAML 2.0**, clique em **adicionar uma configuração de SAML 2.0 para um domínio de correio eletrónico**.

    ![Adicionar o certificado](./media/supermood-tutorial/tutorial_supermood_add.png)

11. No **adicionar uma configuração de SAML 2.0 para um domínio de correio eletrónico**. secção, execute os seguintes passos:

    ![O certificado saml](./media/supermood-tutorial/tutorial_supermood_saml.png)

    a. No **domínio de correio eletrónico para este fornecedor de identidade** caixa de texto, escreva o seu domínio.

    b. No **utilizar um URL de metadados** caixa de texto, cole o **Url de metadados de Federação de aplicação** que copiou do portal do Azure.

    c. Clique em **Adicionar**.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção consiste em criar um utilizador de teste no portal do Azure chamado Britta Simon.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique em de **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/supermood-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e "Todos os utilizadores" ligações](./media/supermood-tutorial/create_aaduser_02.png)

3. Para abrir o **utilizador** caixa de diálogo, clique em **adicionar** na parte superior do **todos os utilizadores** caixa de diálogo.

    ![O botão de adição](./media/supermood-tutorial/create_aaduser_03.png)

4. No **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/supermood-tutorial/create_aaduser_04.png)

    a. No **nome** caixa, escreva **BrittaSimon**.

    b. No **nome de utilizador** caixa, escreva o endereço de e-mail do utilizador Britta Simon.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado no **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-supermood-test-user"></a>Criar um utilizador de teste Supermood

Nesta secção, vai criar um utilizador chamado Britta Simon Supermood. Supermood suportam o aprovisionamento de just-in-time, que é por predefinição ativada para os utilizadores cujos e-mails pertencem a domínios que são adicionados durante a configuração no fim Supermood. Não há nenhum item de ação para si nesta secção. Um novo utilizador é criado durante a tentativa de aceder Supermood se não existir ainda.

>[!Note]
>Se precisar de criar manualmente um utilizador, contacte [equipa de suporte de Supermood](mailto:hello@supermood.fr).


### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Britta Simon utilizar o Azure-início de sessão único, concedendo acesso para Supermood.

![Atribuir a função de utilizador][200] 

**Para atribuir Britta Simon a Supermood, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicações e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todas as aplicações**.

    ![Atribua o utilizador][201] 

2. Na lista de aplicações, selecione **Supermood**.

    ![A ligação de Supermood na lista de aplicações](./media/supermood-tutorial/tutorial_supermood_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação de "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Britta Simon** na lista utilizadores.

6. Clique em **selecione** botão no **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribuir** botão no **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, testar a configuração do Azure AD único início de sessão através do painel de acesso.

Quando clica no mosaico Supermood no painel de acesso, deve obter automaticamente com sessão iniciada para a aplicação de Supermood.
Para mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/supermood-tutorial/tutorial_general_01.png
[2]: ./media/supermood-tutorial/tutorial_general_02.png
[3]: ./media/supermood-tutorial/tutorial_general_03.png
[4]: ./media/supermood-tutorial/tutorial_general_04.png

[100]: ./media/supermood-tutorial/tutorial_general_100.png

[200]: ./media/supermood-tutorial/tutorial_general_200.png
[201]: ./media/supermood-tutorial/tutorial_general_201.png
[202]: ./media/supermood-tutorial/tutorial_general_202.png
[203]: ./media/supermood-tutorial/tutorial_general_203.png

