---
title: 'Tutorial: Integração do Azure Active Directory com Hightail | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Hightail.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: e15206ac-74b0-46e4-9329-892c7d242ec0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/15/2018
ms.author: jeedes
ms.openlocfilehash: 1151044d5c1002c808ae1214086aff5fad84a55e
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39431339"
---
# <a name="tutorial-azure-active-directory-integration-with-hightail"></a>Tutorial: Integração do Azure Active Directory com Hightail

Neste tutorial, saiba como integrar Hightail com o Azure Active Directory (Azure AD).

Integrar Hightail no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Hightail
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Hightail (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Hightail, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Hightail logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Hightail da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-hightail-from-the-gallery"></a>Adicionando Hightail da Galeria
Para configurar a integração do Hightail com o Azure AD, terá de adicionar Hightail a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Hightail a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

1. Na caixa de pesquisa, escreva **Hightail**.

    ![Criar um utilizador de teste do Azure AD](./media/hightail-tutorial/tutorial_hightail_search.png)

1. No painel de resultados, selecione **Hightail**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/hightail-tutorial/tutorial_hightail_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com Hightail com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Hightail a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Hightail deve ser estabelecido.

Hightail, atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com Hightail, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste Hightail](#creating-a-hightail-test-user)**  - para ter um equivalente da Eduarda Almeida na Hightail que está ligado à representação de utilizador do Azure AD.
1. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo Hightail.

**Para configurar o Azure AD início de sessão único com Hightail, execute os seguintes passos:**

1. No portal do Azure, sobre o **Hightail** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.

    ![Configurar o início de sessão único](./media/hightail-tutorial/tutorial_hightail_samlbase.png)

1. Sobre o **Hightail domínio e URLs** secção, execute os seguintes passos, se desejar configurar a aplicação no **IDP** iniciada pelo modo:

    ![Configurar o início de sessão único](./media/hightail-tutorial/tutorial_hightail_url.png)

    Na **URL de resposta** caixa de texto, escreva o URL como: `https://www.hightail.com/samlLogin?phi_action=app/samlLogin&subAction=handleSamlResponse`

    > [!NOTE]
    > O valor de URL de resposta não é o valor real. Atualizar o valor de URL de resposta com o URL de resposta real, o que é explicado mais tarde no tutorial.

1. Verifique **Mostrar definições de URL avançadas** e executar o passo seguinte, se desejar configurar a aplicação na **SP** iniciada pelo modo:

    ![Configurar o início de sessão único](./media/hightail-tutorial/tutorial_hightail_url1.png)

    Na **URL de início de sessão** caixa de texto, escreva o URL como: `https://www.hightail.com/loginSSO`

1. Sobre o **certificado de assinatura SAML** secção, clique em **certificado (Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![Configurar o início de sessão único](./media/hightail-tutorial/tutorial_hightail_certificate.png) 

1. Hightail aplicativo espera que as asserções SAML num formato específico. Configure as seguintes declarações para esta aplicação. Pode gerir os valores destes atributos do **". o atributo"** separador do aplicativo. Captura de ecrã seguinte mostra um exemplo disso. 

    ![Configurar o início de sessão único](./media/hightail-tutorial/tutorial_hightail_attribute.png) 

1. No **atributos de utilizador** secção sobre o **início de sessão único** caixa de diálogo, configurar o atributo de token de SAML conforme mostrado na imagem e execute os seguintes passos:
    
    | Nome do Atributo | Valor do Atributo |
    | ------------------- | -------------------- |
    | FirstName | User.givenName |
    | LastName | User.Surname |
    | Email | User.Mail |    
    | UserIdentity | User.Mail |
    
    a. Clique em **adicionar atributo** para abrir o **adicionar atributo** caixa de diálogo.

    ![Configurar o início de sessão único](./media/hightail-tutorial/tutorial_officespace_04.png)

    ![Configurar o início de sessão único](./media/hightail-tutorial/tutorial_officespace_05.png)

    b. Na **nome** caixa de texto, escreva o nome de atributo apresentado para essa linha.

    c. Partir do **valor** lista, digite o valor de atributo apresentado para essa linha.

    d. Deixe o **espaço de nomes** em branco.

    e. Clique em **OK**.

1. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/hightail-tutorial/tutorial_general_400.png)

1. Sobre o **Hightail configuração** secção, clique em **configurar Hightail** para abrir **configurar início de sessão** janela. Cópia a **SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configurar o início de sessão único](./media/hightail-tutorial/tutorial_hightail_configure.png)

    >[!NOTE]
    >Antes de configurar o início de sessão único na aplicação Hightail, inicie a lista de permissões da Equipe do seu domínio de e-mail com Hightail para que todos os utilizadores que estão a utilizar este domínio podem utilizar a funcionalidade de início de sessão único.

1. Na outra janela do browser, abra a **Hightail** portal de administração.

1. Clique em **ícone de utilizador** do canto superior direito da página. 

    ![Configurar o início de sessão único](./media/hightail-tutorial/configure1.png)

1. Clique em **consola de administração do modo de exibição** separador.

    ![Configurar o início de sessão único](./media/hightail-tutorial/configure2.png)

1. No menu na parte superior, clique nas **SAML** separador e execute os seguintes passos:

    ![Configurar o início de sessão único](./media/hightail-tutorial/configure3.png)

    a. Na **URL de início de sessão** caixa de texto, cole o valor de **SAML único início de sessão no URL do serviço** copiados a partir do portal do Azure.

    b. Abra o certificado com codificação base 64 no bloco de notas transferido a partir do portal do Azure, copie o conteúdo do mesmo para a área de transferência e, em seguida, cole-os para o **certificado SAML** caixa de texto.

    c. Clique em **cópia** para copiar o URL de consumidor SAML para a sua instância e cole-o na **URL de resposta** caixa de texto no **Hightail domínio e URLs** secção no portal do Azure.

    d. Clique em **salvar configurações**.

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/hightail-tutorial/create_aaduser_01.png) 

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/hightail-tutorial/create_aaduser_02.png) 

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/hightail-tutorial/create_aaduser_03.png) 

1. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/hightail-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-a-hightail-test-user"></a>Criar um utilizador de teste Hightail

O objetivo desta secção é criar um usuário chamado Eduarda Almeida no Hightail. 

Não existe nenhum item de ação para nesta secção. Hightail suporta aprovisionamento de utilizadores de just-in-time com base nas afirmações personalizadas. Se tiver configurado as declarações personalizadas, conforme mostrado na secção **[configuração do Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** acima, um utilizador é criado automaticamente na aplicação não ainda existir. 

>[!NOTE]
>Se precisar de criar manualmente um utilizador, terá de contactar o [equipa de suporte de Hightail](mailto:support@hightail.com). 

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para Hightail.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida a Hightail, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **Hightail**.

    ![Configurar o início de sessão único](./media/hightail-tutorial/tutorial_hightail_app.png) 

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202]

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.

### <a name="testing-single-sign-on"></a>Teste de início de sessão único

O objetivo desta secção é testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico Hightail no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo Hightail.


## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[1]: ./media/hightail-tutorial/tutorial_general_01.png
[2]: ./media/hightail-tutorial/tutorial_general_02.png
[3]: ./media/hightail-tutorial/tutorial_general_03.png
[4]: ./media/hightail-tutorial/tutorial_general_04.png

[100]: ./media/hightail-tutorial/tutorial_general_100.png

[200]: ./media/hightail-tutorial/tutorial_general_200.png
[201]: ./media/hightail-tutorial/tutorial_general_201.png
[202]: ./media/hightail-tutorial/tutorial_general_202.png
[203]: ./media/hightail-tutorial/tutorial_general_203.png

