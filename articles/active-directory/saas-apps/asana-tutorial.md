---
title: 'Tutorial: Integração do Azure Active Directory com Asana | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Asana.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 837e38fe-8f55-475c-87f4-6394dc1fee2b
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2018
ms.author: jeedes
ms.openlocfilehash: f5ea7a330891d4befeb6388bbe7f37b2a4aa848f
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39438213"
---
# <a name="tutorial-azure-active-directory-integration-with-asana"></a>Tutorial: Integração do Azure Active Directory com Asana

Neste tutorial, saiba como integrar Asana com o Azure Active Directory (Azure AD).

Integração do Asana com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Asana
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Asana (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Asana, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Asana início de sessão único na subscrição ativado

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste.
O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Asana da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-asana-from-the-gallery"></a>Adicionando Asana da Galeria
Para configurar a integração do Asana para o Azure AD, terá de adicionar Asana a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Asana a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]

1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

1. Na caixa de pesquisa, escreva **Asana**, selecione **Asana** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/asana-tutorial/tutorial_asana_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com Asana com base num utilizador de teste chamado "Eduarda Almeida."

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador equivalente no Asana para um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Asana deve ser estabelecido.

No Asana, atribuir o valor do **nome de utilizador** no Azure AD como o valor da **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com Asana, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste do Asana](#create-an-asana-test-user)**  - para ter um equivalente da Eduarda Almeida no Asana, que está ligado à representação de utilizador do Azure AD.
1. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único na sua aplicação do Asana.

**Para configurar o Azure AD início de sessão único com Asana, execute os seguintes passos:**

1. No portal do Azure, sobre o **Asana** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.

    ![Caixa de diálogo de início de sessão único](./media/asana-tutorial/tutorial_asana_samlbase.png)

1. Sobre o **Asana domínio e URLs** secção, execute os seguintes passos:

    ![Asana domínio e URLs únicas início de sessão em informações](./media/asana-tutorial/tutorial_asana_url.png)

    a. Na **URL de início de sessão** caixa de texto, URL de tipo: `https://app.asana.com/`

    b. Na **identificador** caixa de texto, o valor de tipo: `https://app.asana.com/`

1. Sobre o **certificado de assinatura SAML** secção, clique em **Certificate(Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![O link de download de certificado](./media/asana-tutorial/tutorial_asana_certificate.png)

1. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/asana-tutorial/tutorial_general_400.png)

1. Na **configuração do Asana** secção, clique em **configurar Asana** para abrir **configurar início de sessão** janela. Cópia a **SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configuração do Asana](./media/asana-tutorial/tutorial_asana_configure.png)

1. Numa janela do browser diferente, início de sessão na sua aplicação do Asana. Para configurar o SSO no Asana, aceder às definições da área de trabalho ao clicar no nome de área de trabalho no canto superior direito da tela. Em seguida, clique em  **\<o nome da sua área de trabalho\> definições**.

    ![Definições de sso do Asana](./media/asana-tutorial/tutorial_asana_09.png)

1. Sobre o **definições de organização** janela, clique em **administração**. Em seguida, clique em **membros têm de iniciar sessão através de SAML** para ativar a configuração de SSO. A executar os seguintes passos:

    ![Configurar as definições de organização de início de sessão único](./media/asana-tutorial/tutorial_asana_10.png)  

     a. Na **URL da página de início de sessão** caixa de texto, colar a **SAML único início de sessão no URL do serviço**.

     b. Clique com o botão direito do rato no certificado transferido a partir do portal do Azure, em seguida, abra o ficheiro de certificado com o bloco de notas ou no seu editor de texto preferido. Copie o conteúdo entre o início e o título de certificado do fim e cole-na **certificado X.509** caixa de texto.

1. Clique em **Guardar**. Aceda a [Guia do Asana para configurar o SSO](https://asana.com/guide/help/premium/authentication#gl-saml) se precisar de mais assistência.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![O botão do Azure Active Directory](./media/asana-tutorial/create_aaduser_01.png) 

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/asana-tutorial/create_aaduser_02.png)

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.

    ![Criar um utilizador de teste do Azure AD](./media/asana-tutorial/create_aaduser_03.png)

1. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:

    ![Botão Adicionar](./media/asana-tutorial/create_aaduser_04.png)

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.

### <a name="create-an-asana-test-user"></a>Criar um utilizador de teste do Asana

O objetivo desta secção é criar um usuário chamado Eduarda Almeida no Asana. Asana suporta o aprovisionamento automático de utilizadores, que está por predefinição, ativada. Pode encontrar mais detalhes [aqui](asana-provisioning-tutorial.md) sobre como configurar o aprovisionamento automático de utilizadores.

**Se precisar de criar manualmente o utilizador, execute, os seguintes passos:**

Nesta secção, vai criar um usuário chamado Eduarda Almeida no Asana.

1. No **Asana**, aceda ao **Equipes** secção no painel esquerdo. Clique no botão do sinal de adição.

    ![Criar um utilizador de teste do Azure AD](./media/asana-tutorial/tutorial_asana_12.png)

1. Escreva a mensagem de e-mail britta.simon@contoso.com na caixa de texto e, em seguida, selecione **convidar**.

1. Clique em **Enviar convite**. O novo utilizador irá receber um e-mail na sua conta de e-mail. Ela terá de criar e validar a conta.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para Asana.

![Atribuir a função de utilizador][200]

**Para atribuir a Eduarda Almeida a Asana, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201]

1. Na lista de aplicações, selecione **Asana**.

    ![A ligação do Asana na lista de aplicações](./media/asana-tutorial/tutorial_asana_app.png)

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.

### <a name="test-single-sign-on"></a>Testar início de sessão único

O objetivo desta secção consiste em testar sua do Azure AD início de sessão único.

Aceda à página de início de sessão do Asana. Na caixa de texto de endereço de E-Mail, insira o endereço de e-mail britta.simon@contoso.com. Deixe a caixa de texto de palavra-passe em branco e, em seguida, clique em **sessão**. Será redirecionado para a página de início de sessão do Azure AD. Conclua as credenciais do Azure AD. Agora, está conectado Asana.

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)
* [Configurar o aprovisionamento do utilizador](asana-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/asana-tutorial/tutorial_general_01.png
[2]: ./media/asana-tutorial/tutorial_general_02.png
[3]: ./media/asana-tutorial/tutorial_general_03.png
[4]: ./media/asana-tutorial/tutorial_general_04.png

[100]: ./media/asana-tutorial/tutorial_general_100.png

[200]: ./media/asana-tutorial/tutorial_general_200.png
[201]: ./media/asana-tutorial/tutorial_general_201.png
[202]: ./media/asana-tutorial/tutorial_general_202.png
[203]: ./media/asana-tutorial/tutorial_general_203.png
[10]: ./media/asana-tutorial/tutorial_general_060.png
[11]: ./media/asana-tutorial/tutorial_general_070.png