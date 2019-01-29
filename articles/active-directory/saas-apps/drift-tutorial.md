---
title: 'Tutorial: Integração do Active Directory do Azure com Descompassos | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e desvios.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 39dcbb95-c192-448c-86a1-cedede1c0972
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2018
ms.author: jeedes
ms.openlocfilehash: f7973ccb384a8e882a9ced5020a53824bf0c4e7d
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55169880"
---
# <a name="tutorial-azure-active-directory-integration-with-drift"></a>Tutorial: Integração do Active Directory do Azure com Descompassos

Neste tutorial, saiba como integrar os Descompassos com o Azure Active Directory (Azure AD).

Integrar Descompassos no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Descompassos.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para desvios (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Descompassos, terá dos seguintes itens:

- Uma subscrição do Azure
- Um Descompassos logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando os desvios da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-drift-from-the-gallery"></a>Adicionando os desvios da Galeria

Para configurar a integração de desvios para o Azure AD, terá de adicionar Descompassos a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Descompassos a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]

3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

4. Na caixa de pesquisa, escreva **Descompassos**, selecione **Descompassos** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Na lista de resultados de inconsistências](./media/drift-tutorial/tutorial_drift_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com Descompassos com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Descompassos a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Descompassos deve ser estabelecido.

Para configurar e testar o Azure AD início de sessão único com Descompassos, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste Descompassos](#creating-a-drift-test-user)**  - para ter um equivalente da Eduarda Almeida na Descompassos que está ligado à representação de utilizador do Azure AD.
4. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo Descompassos.

**Para configurar o Azure AD início de sessão único com Descompassos, execute os seguintes passos:**

1. No portal do Azure, sobre o **Descompassos** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Sobre o **selecionar um método de início de sessão único** caixa de diálogo, clique em **selecione** para **SAML** modo para ativar o início de sessão único.

    ![Configurar o início de sessão único](common/tutorial_general_301.png)

3. Sobre o **definir a segurança de início de sessão único com o SAML** página, clique em **editar** ícone para abrir **configuração básica de SAML** caixa de diálogo.

    ![Configurar o início de sessão único](common/editconfigure.png)

4. Sobre o **configuração básica de SAML** secção, execute os seguintes passos, se desejar configurar a aplicação no **IDP** iniciada pelo modo:

    ![Domínio e URLs únicas início de sessão em informações de inconsistências](./media/drift-tutorial/tutorial_drift_url.png)

    a. Clique em **definir URLs adicionais**.

    b. Na **estado de reencaminhamento** caixa de texto, escreva um URL: `https://app.drift.com`

    c. Se desejar configurar a aplicação no **SP** iniciadas pelo modo, na **URL de início de sessão** caixa de texto, escreva um URL: `https://start.drift.com`

5. Aplicação de desvios espera que as asserções SAML num formato específico. Configure as seguintes declarações para esta aplicação. Pode gerir os valores destes atributos do **atributos de utilizador e afirmações** secção na página de integração de aplicações. Clique em **edite** botão para abrir **atributos de utilizador e afirmações** caixa de diálogo.

    ![image](./media/drift-tutorial/tutorial_drift_attribute.png)

6. No **afirmações de utilizador** secção sobre o **atributos de utilizador e afirmações** caixa de diálogo, configurar o atributo de token de SAML conforme mostrado na imagem acima e execute os seguintes passos:
    
    | Nome do Atributo | Valor do Atributo |
    | ---------------| --------------- |    
    | Nome | user.displayname |

    a. Clique nas `name` declaração (declaração de realçada) para abrir o **Gerir afirmações de utilizador** caixa de diálogo.

    ![image](./media/drift-tutorial/tutorial_drift_attribute1.png)

    ![image](./media/drift-tutorial/tutorial_drift_attribute3.png)

    b. Selecione a origem de dado **atributo**.

    c. Tornar o **espaço de nomes** em branco.

    d. Partir do **atributo de origem** lista, selecione **user.displayname**.

    e. Clique em **Guardar**.

7. No **certificado de assinatura SAML** página, além da **certificado de assinatura SAML** secção, clique em **transferir** para transferir **XML de metadados de Federação** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![O link de download de certificado](./media/drift-tutorial/tutorial_drift_certificate.png) 

8. Numa janela do browser web diferente, início de sessão para desvios como administrador.

9. Do lado esquerdo da barra de menu, clique em **ícone de definições** > **definições de aplicação** > **autenticação** e execute os seguintes passos:

    ![A ligação de administrador](./media/drift-tutorial/tutorial_drift_admin.png)

    a. Carregar o **XML de metadados de Federação** que transferiu do portal do Azure, para o **ficheiro de metadados de carregar o fornecedor de identidade** caixa de texto.

    b. Depois de carregar o ficheiro de metadados, os restantes valores obtém automaticamente preenchido automaticamente na página.

    c. Clique em **ativar SAML**.

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

1. No portal do Azure, no painel esquerdo, selecione **do Azure Active Directory**, selecione **utilizadores**e, em seguida, selecione **todos os utilizadores**.

    ![Criar utilizador do Azure AD][100]

2. Selecione **novo utilizador** na parte superior do ecrã.

    ![Criar um utilizador de teste do Azure AD](common/create_aaduser_01.png) 

3. Nas propriedades do utilizador, execute os seguintes passos.

    ![Criar um utilizador de teste do Azure AD](common/create_aaduser_02.png)

    a. Na **Name** , insira **BrittaSimon**.
  
    b. Na **nome de utilizador** , digite **brittasimon@yourcompanydomain.extension**  
    Por exemplo, BrittaSimon@contoso.com

    c. Selecione **propriedades**, selecione a **palavra-passe de Show** caixa de verificação e, em seguida, anote o valor que é apresentado na caixa de palavra-passe.

    d. Selecione **Criar**.

### <a name="creating-a-drift-test-user"></a>Criar um utilizador de teste Descompassos

O objetivo desta secção é criar um usuário chamado Eduarda Almeida no Descompassos. Descompassos suporta o aprovisionamento de just-in-time, que está por predefinição, ativada. Não existe nenhum item de ação para nesta secção. Um novo utilizador é criado durante uma tentativa de aceder Descompassos se não existir ainda.
>[!Note]
>Se precisar de criar manualmente um utilizador, contacte [equipa de suporte de desvios](mailto:integrations@drift.com).

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para desvios.

1. No portal do Azure, selecione **aplicações empresariais**, selecione **todos os aplicativos**.

    ![Atribuir utilizador][201]

2. Na lista de aplicações, selecione **Descompassos**.

    ![Configurar o início de sessão único](./media/drift-tutorial/tutorial_drift_app.png) 

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

5. Na **utilizadores e grupos** caixa de diálogo select **Eduarda Almeida** na lista de utilizadores, em seguida, clique o **selecionar** na parte inferior do ecrã.

6. Na **adicionar atribuição**, selecione caixa de diálogo a **atribuir** botão.

### <a name="testing-single-sign-on"></a>Teste de início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico Descompassos no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação de desvios.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[1]: common/tutorial_general_01.png
[2]: common/tutorial_general_02.png
[3]: common/tutorial_general_03.png
[4]: common/tutorial_general_04.png

[100]: common/tutorial_general_100.png

[201]: common/tutorial_general_201.png
[202]: common/tutorial_general_202.png
[203]: common/tutorial_general_203.png
