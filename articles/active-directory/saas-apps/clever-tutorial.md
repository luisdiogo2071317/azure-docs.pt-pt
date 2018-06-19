---
title: 'Tutorial: Integração do Azure Active Directory com Clever | Microsoft Docs'
description: Saiba como configurar o início de sessão entre o Azure Active Directory e Clever.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 069ff13a-310e-4366-a147-d6ec5cca12a5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2018
ms.author: jeedes
ms.openlocfilehash: 2e71ed3496ae28e71255498027dfccd69651e115
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/18/2018
ms.locfileid: "35955766"
---
# <a name="tutorial-azure-active-directory-integration-with-clever"></a>Tutorial: Integração do Azure Active Directory com Clever

Neste tutorial, irá aprender a integrar Clever com o Azure Active Directory (Azure AD).

Integrar Clever com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Clever.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Clever (Single Sign-On) com as respetivas contas do Azure AD.
- Pode gerir as contas numa localização central - portal do Azure.

Se pretender saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [que é o acesso a aplicações e início de sessão no Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Clever, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Clever-início de sessão único ativada subscrição

> [!NOTE]
> Para testar os passos neste tutorial, não recomendamos a utilização num ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, teste do Azure AD-início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos modulares principais:

1. Adicionar Clever a partir da Galeria
2. Configurar e testar o Azure AD de sessão único-

## <a name="adding-clever-from-the-gallery"></a>Adicionar Clever a partir da Galeria
Para configurar a integração de Clever com o Azure AD, terá de adicionar Clever a partir da Galeria à sua lista de aplicações SaaS geridas.

**Para adicionar Clever a partir da galeria, execute os seguintes passos:**

1. No  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **do Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todas as aplicações**.

    ![O painel de aplicações da empresa][2]
    
3. Para adicionar a nova aplicação, clique em **nova aplicação** botão no topo da caixa de diálogo.

    ![O novo botão de aplicação][3]

4. Na caixa de pesquisa, escreva **Clever**, selecione **Clever** partir do painel de resultados, em seguida, clique em **adicionar** botão para adicionar a aplicação.

    ![Clever na lista de resultados](./media/clever-tutorial/tutorial_clever_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD-início de sessão único

Nesta secção, configure e teste do Azure AD-início de sessão único com Clever com base num utilizador de teste chamado "Britta Simon".

Para início de sessão trabalhar, do Azure AD tem de saber o que o utilizador homólogo no Clever é um utilizador no Azure AD. Por outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Clever tem de ser estabelecida.

No Clever, atribua o valor do **nome de utilizador** no Azure AD como o valor a **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD-início de sessão único com Clever, tem de concluir os blocos modulares seguintes:

1. **[Configurar o Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)**  - para permitir aos utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD-início de sessão único com Britta Simon.
3. **[Criar um utilizador de teste Clever](#create-a-clever-test-user)**  - para ter um homólogo de Britta Simon Clever que está ligada a representação do Azure AD do utilizador.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar Britta Simon utilizar o Azure AD-início de sessão único.
5. **[Teste o início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD-início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão no portal do Azure e configurar o início de sessão único na sua aplicação Clever.

**Para configurar o Azure AD-início de sessão único com Clever, execute os seguintes passos:**

1. No portal do Azure, no **Clever** página de integração de aplicações, clique em **de sessão único-**.

    ![Configurar a ligação de início de sessão único][4]

2. No **de sessão único-** caixa de diálogo, selecione **modo** como **baseados em SAML início de sessão** para ativar o início de sessão único.

    ![Caixa de diálogo de início de sessão único](./media/clever-tutorial/tutorial_clever_samlbase.png)

3. No **domínio Clever e URLs** secção, execute os seguintes passos:

    ![Clever URLs de domínio e única informações de início de sessão](./media/clever-tutorial/tutorial_clever_url.png)

    a. No **URL de início de sessão** caixa de texto, escreva um URL a utilizar o padrão do seguinte: `https://clever.com/in/<companyname>`

    b. No **identificador** caixa de texto, escreva o URL: `https://clever.com/oauth/saml/metadata.xml`

    > [!NOTE]
    > Início de sessão do valor de URL não é real. Atualize este valor com o URL de início de sessão real. Contacte [equipa de suporte de cliente Clever](https://clever.com/about/contact/) para obter este valor.

4. No **certificado de assinatura de SAML** secção, clique no botão Copiar para copiar **Url de metadados de Federação de aplicação** e cole-o bloco de notas.
    
    ![Configurar o início de sessão único](./media/clever-tutorial/tutorial_metadataurl.png)

5. A aplicação Clever espera as asserções de SAML num formato específico, que necessita para adicionar mapeamentos de atributos personalizados para o **atributos Token SAML** configuração.

    A seguinte captura de ecrã mostra um exemplo para este.

    ![Configurar o início de sessão único](./media/clever-tutorial/tutorial_clever_07.png)

6. No **atributos de utilizador** secção no **de sessão único-** caixa de diálogo, configurar atributos token SAML, conforme mostrado na imagem acima e execute os seguintes passos:
    
    | Nome do atributo  | Valor do Atributo |
    | --------------- | -------------------- |
    | clever.Teacher.credentials.district_username|user.userprincipalname|
    | clever.Student.credentials.district_username| user.userprincipalname |
    | nome próprio  | User.givenName |
    | Apelido  | User.Surname |

    a. Clique em **adicionar atributo** para abrir o **adicionar atributo** caixa de diálogo.

    ![Configurar o início de sessão único](./media/clever-tutorial/tutorial_attribute_04.png)
    
    ![Configurar o início de sessão único](./media/clever-tutorial/tutorial_attribute_05.png)
    
    b. No **nome** caixa de texto, escreva o nome de atributo apresentado para essa linha.

    c. Do **valor** lista, digite o valor de atributo apresentado para essa linha.

    d. Deixe o **espaço de nomes** caixa de texto em branco.
    
    d. Clique em **OK**.
    
7. Clique em **guardar** botão.

    ![Configurar botão único início de sessão guardar](./media/clever-tutorial/tutorial_general_400.png)

8. Numa janela do browser web diferente, inicie sessão no site da sua empresa Clever como administrador.

9. Na barra de ferramentas, clique em **instantânea de início de sessão**.

    ![Início de sessão instantânea](./media/clever-tutorial/ic798984.png "instantânea início de sessão")

    > [!NOTE]
    > Antes de testar o início de sessão único, terá de contactar [equipa de suporte de cliente Clever](https://clever.com/about/contact/) para ativar o Office 365 SSO no back-end.

10. No **instantânea de início de sessão** página, execute os seguintes passos:
    
      ![Início de sessão instantânea](./media/clever-tutorial/ic798985.png "instantânea início de sessão")
    
      a. Tipo de **URL de início de sessão**.
    
      >[!NOTE]
      >O **URL de início de sessão** é um valor personalizado. Contacte [equipa de suporte de cliente Clever](https://clever.com/about/contact/) para obter este valor.
    
      b. Como **sistema de identidade**, selecione **ADFS**.

      c. No **URL de metadados** caixa de texto, colar **Url de metadados de Federação de aplicação** valor que copiou do portal do Azure.
    
      d. Clique em **Guardar**.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção consiste em criar um utilizador de teste no portal do Azure chamado Britta Simon.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique em de **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/clever-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e "Todos os utilizadores" ligações](./media/clever-tutorial/create_aaduser_02.png)

3. Para abrir o **utilizador** caixa de diálogo, clique em **adicionar** na parte superior do **todos os utilizadores** caixa de diálogo.

    ![O botão de adição](./media/clever-tutorial/create_aaduser_03.png)

4. No **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/clever-tutorial/create_aaduser_04.png)

    a. No **nome** caixa, escreva **BrittaSimon**.

    b. No **nome de utilizador** caixa, escreva o endereço de e-mail do utilizador Britta Simon.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado no **palavra-passe** caixa.

    d. Clique em **Criar**.

### <a name="create-a-clever-test-user"></a>Criar um utilizador de teste Clever

Para permitir que os utilizadores do Azure AD iniciem sessão nos Clever, têm de ser aprovisionados para Clever.

Em caso de Clever, trabalhar com [equipa de suporte de cliente Clever](https://clever.com/about/contact/) para adicionar os utilizadores na plataforma Clever. Os utilizadores tem de ser criados e ativados antes de utilizar o início de sessão único.

>[!NOTE]
>Pode utilizar quaisquer outras ferramentas do utilizador Clever conta criação ou APIs fornecidas pelo Clever aprovisionar contas de utilizador do Azure AD.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Britta Simon utilizar o Azure-início de sessão único, concedendo acesso para Clever.

![Atribuir a função de utilizador][200]

**Para atribuir Britta Simon a Clever, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicações e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todas as aplicações**.

    ![Atribua o utilizador][201]

2. Na lista de aplicações, selecione **Clever**.

    ![O Clever ligação na lista de aplicações](./media/clever-tutorial/tutorial_clever_app.png)

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação de "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Britta Simon** na lista utilizadores.

6. Clique em **selecione** botão no **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribuir** botão no **adicionar atribuição** caixa de diálogo.

### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, testar a configuração do Azure AD único início de sessão através do painel de acesso.

Quando clica no mosaico Clever no painel de acesso, deve obter automaticamente com sessão iniciada para a aplicação Clever.
Para mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/clever-tutorial/tutorial_general_01.png
[2]: ./media/clever-tutorial/tutorial_general_02.png
[3]: ./media/clever-tutorial/tutorial_general_03.png
[4]: ./media/clever-tutorial/tutorial_general_04.png

[100]: ./media/clever-tutorial/tutorial_general_100.png

[200]: ./media/clever-tutorial/tutorial_general_200.png
[201]: ./media/clever-tutorial/tutorial_general_201.png
[202]: ./media/clever-tutorial/tutorial_general_202.png
[203]: ./media/clever-tutorial/tutorial_general_203.png

