---
title: 'Tutorial: Integração do Active Directory do Azure com Nuclino | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Nuclino.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 74bbab82-5581-4dcf-8806-78f77c746968
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 655ac490e528680f779eeca54899a022ddf3b89a
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2019
ms.locfileid: "56189564"
---
# <a name="tutorial-azure-active-directory-integration-with-nuclino"></a>Tutorial: Integração do Active Directory do Azure com Nuclino

Neste tutorial, saiba como integrar Nuclino com o Azure Active Directory (Azure AD).

Integrar Nuclino no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Nuclino.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Nuclino (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Nuclino, terá dos seguintes itens:

- Uma subscrição do Azure
- Um Nuclino logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Nuclino da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-nuclino-from-the-gallery"></a>Adicionando Nuclino da Galeria

Para configurar a integração do Nuclino com o Azure AD, terá de adicionar Nuclino a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Nuclino a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]

3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

4. Na caixa de pesquisa, escreva **Nuclino**, selecione **Nuclino** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Nuclino na lista de resultados](./media/nuclino-tutorial/tutorial_nuclino_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com Nuclino com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Nuclino a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Nuclino deve ser estabelecido.

Para configurar e testar o Azure AD início de sessão único com Nuclino, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste Nuclino](#create-a-nuclino-test-user)**  - para ter um equivalente da Eduarda Almeida na Nuclino que está ligado à representação de utilizador do Azure AD.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo Nuclino.

**Para configurar o Azure AD início de sessão único com Nuclino, execute os seguintes passos:**

1. No portal do Azure, sobre o **Nuclino** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.

    ![Caixa de diálogo de início de sessão único](./media/nuclino-tutorial/tutorial_nuclino_samlbase.png)

3. Sobre o **Nuclino domínio e URLs** secção, execute os seguintes passos, se desejar configurar a aplicação no **IDP** iniciada pelo modo:

    ![Nuclino domínio e URLs únicas início de sessão em informações](./media/nuclino-tutorial/tutorial_nuclino_url1.png)

    a. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://api.nuclino.com/api/sso/<UNIQUE-ID>/metadata`

    b. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://api.nuclino.com/api/sso/<UNIQUE-ID>/acs`

    > [!NOTE]
    > Estes valores não são reais. Atualize estes valores com o identificador real e o URL de resposta do **autenticação** seção, que é explicada posteriormente neste tutorial.

4. Verifique **Mostrar definições de URL avançadas** e executar o passo seguinte, se desejar configurar a aplicação na **SP** iniciada pelo modo:

    ![Nuclino domínio e URLs únicas início de sessão em informações](./media/nuclino-tutorial/tutorial_nuclino_url2.png)

    Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://app.nuclino.com/<UNIQUE-ID>/login`

    > [!NOTE]
    > Este valor não é real. Atualize este valor com o URL de início de sessão real. Contacte [equipa de suporte de cliente Nuclino](mailto:contact@nuclino.com) para obter este valor.

5. Aplicação de Nuclino espera que as asserções SAML num formato específico. Configure as seguintes declarações para esta aplicação. Pode gerir os valores destes atributos da "**atributos de utilizador**" secção na página de integração de aplicações. Captura de ecrã seguinte mostra um exemplo disso.

    ![Configurar o início de sessão único](./media/Nuclino-tutorial/tutorial_attribute.png)

6. Clique em **ver e editar todos os outros atributos de utilizador** caixa de seleção o **atributos de utilizador** secção para expandir os atributos. Execute os seguintes passos em cada um dos atributos apresentados-

    | Nome do Atributo | Valor do Atributo |
    | ---------------| --------------- |
    | first_name | user.givenname |
    | last_name | user.surname |

    a. Clique em **adicionar atributo** para abrir o **adicionar atributo** caixa de diálogo.

    ![Configurar o início de sessão único](./media/nuclino-tutorial/tutorial_attribute_04.png)

    ![Configurar o início de sessão único](./media/nuclino-tutorial/tutorial_attribute_05.png)

    b. Na **nome** caixa de texto, tipo a **nome do atributo** mostrado para essa linha.

    c. Partir do **valor** lista, digite o valor de atributo apresentado para essa linha.

    d. Clique em **OK**.

7. Sobre o **certificado de assinatura SAML** secção, clique em **certificado (Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![O link de download de certificado](./media/nuclino-tutorial/tutorial_nuclino_certificate.png)

8. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/nuclino-tutorial/tutorial_general_400.png)

9. Sobre o **Nuclino configuração** secção, clique em **configurar Nuclino** para abrir **configurar início de sessão** janela. Cópia a **ID de entidade de SAML e SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configuração de Nuclino](./media/nuclino-tutorial/tutorial_nuclino_configure.png)

10. Numa janela do browser web diferente, inicie sessão no site da sua empresa Nuclino como um administrador.

11. Clique nas **ÍCONE**.

    ![Configuração de Nuclino](./media/nuclino-tutorial/configure1.png)

12. Clique nas **SSO do Azure AD** e selecione **definições da Equipe** na lista pendente.

    ![Configuração de Nuclino](./media/nuclino-tutorial/configure2.png)

13. Selecione **autenticação** no painel de navegação esquerdo.

    ![Configuração de Nuclino](./media/nuclino-tutorial/configure3.png)

14. Na **autenticação** secção, execute os seguintes passos:

    ![Configuração de Nuclino](./media/nuclino-tutorial/configure4.png)

    a. Selecione **baseado em SAML início de sessão único (SSO)**.

    b. Cópia **URL de ACS (precisa copiar e colá-lo para o seu fornecedor SSO)** valor e cole-o no **URL de resposta** caixa de texto do **Nuclino domínio e URLs** secção no Azure Portal.

    c. Cópia **ID de entidade (precisa copiar e colá-lo para o seu fornecedor SSO)** valor e cole-o para o **identificador** caixa de texto do **Nuclino domínio e URLs** secção no Azure Portal.

    d. Na **URL de SSO** caixa de texto, colar a **SAML único início de sessão no URL do serviço** valor que copiou do portal do Azure.

    e. Na **ID de entidade** caixa de texto, colar a **ID de entidade de SAML** valor que copiou do portal do Azure.

    f. Abra sua transferido **Certificate(Base64)** ficheiro no bloco de notas. Copie o conteúdo do mesmo para a área de transferência e, em seguida, cole-os para o **certificado público** caixa de texto.

    g. Clique em **guardar alterações**.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/nuclino-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/nuclino-tutorial/create_aaduser_02.png)

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/nuclino-tutorial/create_aaduser_03.png)

4. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/nuclino-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.

### <a name="create-a-nuclino-test-user"></a>Criar um utilizador de teste Nuclino

O objetivo desta secção é criar um usuário chamado Eduarda Almeida no Nuclino. Nuclino suporta o aprovisionamento de just-in-time, que está por predefinição, ativada. Não existe nenhum item de ação para nesta secção. Um novo utilizador é criado durante uma tentativa de aceder Nuclino se não existir ainda.

> [!Note]
> Se precisar de criar manualmente um utilizador, contacte [equipa de suporte de Nuclino](mailto:contact@nuclino.com).

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para Nuclino.

![Atribuir a função de utilizador][200]

**Para atribuir a Eduarda Almeida a Nuclino, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201]

2. Na lista de aplicações, selecione **Nuclino**.

    ![A ligação de Nuclino na lista de aplicações](./media/nuclino-tutorial/tutorial_nuclino_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.

### <a name="test-single-sign-on"></a>Testar o início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico Nuclino no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo Nuclino.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[1]: ./media/nuclino-tutorial/tutorial_general_01.png
[2]: ./media/nuclino-tutorial/tutorial_general_02.png
[3]: ./media/nuclino-tutorial/tutorial_general_03.png
[4]: ./media/nuclino-tutorial/tutorial_general_04.png

[100]: ./media/nuclino-tutorial/tutorial_general_100.png

[200]: ./media/nuclino-tutorial/tutorial_general_200.png
[201]: ./media/nuclino-tutorial/tutorial_general_201.png
[202]: ./media/nuclino-tutorial/tutorial_general_202.png
[203]: ./media/nuclino-tutorial/tutorial_general_203.png
