---
title: 'Tutorial: Integração do Azure Active Directory com o Software de recrutamento Comeet | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e o Software de recrutamento Comeet.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 75f51dc9-9525-4ec6-80bf-28374f0c8adf
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/29/2018
ms.author: jeedes
ms.openlocfilehash: 137ba7a7e82ff3e57d862868859e8049838701a3
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/30/2018
ms.locfileid: "43307828"
---
# <a name="tutorial-azure-active-directory-integration-with-comeet-recruiting-software"></a>Tutorial: Integração do Azure Active Directory com o Software de recrutamento Comeet

Neste tutorial, saiba como integrar o Software de recrutamento Comeet com o Azure Active Directory (Azure AD).

Integrar o Software de recrutamento Comeet no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Software de recrutamento Comeet.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para o Software de recrutamento Comeet (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o Software de recrutamento Comeet, terá dos seguintes itens:

- Uma subscrição do Azure
- Um Software de recrutamento Comeet logon único habilitado subscrição

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando o Software de recrutamento Comeet da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-comeet-recruiting-software-from-the-gallery"></a>Adicionando o Software de recrutamento Comeet da Galeria

Para configurar a integração de Software de recrutamento Comeet com o Azure AD, terá de adicionar o Software de recrutamento Comeet partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Software de recrutamento Comeet a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]

3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

4. Na caixa de pesquisa, escreva **Software de recrutamento Comeet**, selecione **Software de recrutamento Comeet** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Software de recrutamento comeet na lista de resultados](./media/comeetrecruitingsoftware-tutorial/tutorial_comeetrecruitingsoftware_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com o Software de recrutamento de Comeet com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Software de recrutamento Comeet a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Software de recrutamento Comeet deve ser estabelecido.

Para configurar e testar o Azure AD início de sessão único com o Software de recrutamento Comeet, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste de Software de recrutamento Comeet](#create-a-comeet-recruiting-software-test-user)**  - para ter um equivalente da Eduarda Almeida na Comeet recrutamento Software que está ligada à representação de utilizador do Azure AD.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo de Software de recrutamento Comeet.

**Para configurar o Azure AD início de sessão único com o Software de recrutamento Comeet, execute os seguintes passos:**

1. No portal do Azure, sobre o **Software de recrutamento Comeet** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.

    ![Caixa de diálogo de início de sessão único](./media/comeetrecruitingsoftware-tutorial/tutorial_comeetrecruitingsoftware_samlbase.png)

3. Sobre o **Comeet recrutamento Software domínio e URLs** secção, execute os seguintes passos, se desejar configurar a aplicação no **IDP** iniciada pelo modo:

    ![Comeet domínio e URLs únicas início de sessão em informações](./media/comeetrecruitingsoftware-tutorial/tutorial_comeetrecruitingsoftware_url1.png)

    a. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://app.comeet.co/adfs_auth/acs/<UNIQUEID>/`

    b. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://app.comeet.co/adfs_auth/acs/<UNIQUEID>/`

    > [!NOTE]
    > Estes valores não são reais. Atualize estes valores com o identificador real, o URL de resposta. Irá obter estes valores a partir do portal do Software de recrutamento Comeet conforme mostrado na [página de suporte](https://support.comeet.co/knowledgebase/adfs-single-sign-on/).

4. Verifique **Mostrar definições de URL avançadas** e executar o passo seguinte, se desejar configurar a aplicação na **SP** iniciada pelo modo:

    ![Comeet URLs de domínio do Software de recrutamento e únicas início de sessão em informações](./media/comeetrecruitingsoftware-tutorial/tutorial_comeetrecruitingsoftware_url2.png)

    Na **URL de início de sessão** caixa de texto, escreva o URL: `https://app.comeet.co`

5. Aplicativo de recrutamento Software comeet espera que as asserções SAML num formato específico, o que requer a adição de mapeamentos de atributos personalizado à sua configuração de atributos de token SAML. Captura de ecrã seguinte mostra um exemplo disso. O valor predefinido **identificador de utilizador** é **user.userprincipalname** mas **Software de recrutamento Comeet** espera que isso seja mapeado com o endereço de e-mail do utilizador. Para que pode usar **user.mail** atributo da lista ou utilize o valor do atributo adequado com base na configuração da sua organização.

    ![Configurar o início de sessão único](./media/comeetrecruitingsoftware-tutorial/tutorial_comeetrecruitingsoftware_attribute.png)

6. Clique em **ver e editar todos os outros atributos de utilizador** caixa de seleção o **atributos de utilizador** secção para expandir os atributos. Execute os seguintes passos em cada um dos atributos apresentados-

    | Nome do Atributo | Valor do Atributo |
    | ---------------| --------------- |
    | comeet_id | user.userprincipalname |

    a. Clique em **adicionar atributo** para abrir o **adicionar atributo** caixa de diálogo.

    ![Configurar o início de sessão único](./media/comeetrecruitingsoftware-tutorial/tutorial_attribute_04.png)

    ![Configurar o início de sessão único](./media/comeetrecruitingsoftware-tutorial/tutorial_attribute_05.png)

    b. Na **nome** caixa de texto, tipo a **nome do atributo** mostrado para essa linha.

    c. Partir do **valor** lista, digite o valor de atributo apresentado para essa linha.

    d. Clique em **OK**.

7. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![O link de download de certificado](./media/comeetrecruitingsoftware-tutorial/tutorial_comeetrecruitingsoftware_certificate.png)

8. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/comeetrecruitingsoftware-tutorial/tutorial_general_400.png)

9. Para configurar o início de sessão único num **Software de recrutamento Comeet** lado, cole o conteúdo do XML de metadados baixado em Comeet recrutamento Software, como mostra a [página de suporte](https://support.comeet.co/knowledgebase/adfs-single-sign-on/).

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/comeetrecruitingsoftware-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/comeetrecruitingsoftware-tutorial/create_aaduser_02.png)

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/comeetrecruitingsoftware-tutorial/create_aaduser_03.png)

4. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/comeetrecruitingsoftware-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.

### <a name="create-a-comeet-recruiting-software-test-user"></a>Criar um utilizador de teste de Software de recrutamento Comeet

Nesta secção, vai criar um usuário chamado Eduarda Almeida no Software de recrutamento Comeet. Trabalhar com [equipa de suporte do Software de recrutamento Comeet](mailto:support@comeet.co) para adicionar os utilizadores na plataforma do Software de recrutamento Comeet. Os utilizadores tem de ser criados e ativados antes de utilizar o início de sessão único.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único, concedendo acesso ao Software de recrutamento Comeet.

![Atribuir a função de utilizador][200]

**Para atribuir a Eduarda Almeida a Software de recrutamento Comeet, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201]

2. Na lista de aplicações, selecione **Software de recrutamento Comeet**.

    ![A ligação de Software de recrutamento Comeet na lista de aplicações](./media/comeetrecruitingsoftware-tutorial/tutorial_comeetrecruitingsoftware_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.

### <a name="test-single-sign-on"></a>Testar o início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico de Software de recrutamento Comeet no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo de Software de recrutamento Comeet.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[1]: ./media/comeetrecruitingsoftware-tutorial/tutorial_general_01.png
[2]: ./media/comeetrecruitingsoftware-tutorial/tutorial_general_02.png
[3]: ./media/comeetrecruitingsoftware-tutorial/tutorial_general_03.png
[4]: ./media/comeetrecruitingsoftware-tutorial/tutorial_general_04.png

[100]: ./media/comeetrecruitingsoftware-tutorial/tutorial_general_100.png

[200]: ./media/comeetrecruitingsoftware-tutorial/tutorial_general_200.png
[201]: ./media/comeetrecruitingsoftware-tutorial/tutorial_general_201.png
[202]: ./media/comeetrecruitingsoftware-tutorial/tutorial_general_202.png
[203]: ./media/comeetrecruitingsoftware-tutorial/tutorial_general_203.png