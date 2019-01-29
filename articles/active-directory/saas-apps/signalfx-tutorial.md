---
title: 'Tutorial: Integração do Active Directory do Azure com SignalFx | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e SignalFx.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 6d5ab4b0-29bc-4b20-8536-d64db7530f32
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/16/2018
ms.author: jeedes
ms.openlocfilehash: 6f2d869f345aeb8f50d42de6b1533b849ffb2182
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55197573"
---
# <a name="tutorial-azure-active-directory-integration-with-signalfx"></a>Tutorial: Integração do Active Directory do Azure com SignalFx

Neste tutorial, saiba como integrar SignalFx com o Azure Active Directory (Azure AD).

Integrar SignalFx no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao SignalFx.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para SignalFx (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com SignalFx, terá dos seguintes itens:

- Uma subscrição do Azure
- Um SignalFx logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando SignalFx da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-signalfx-from-the-gallery"></a>Adicionando SignalFx da Galeria
Para configurar a integração do SignalFx com o Azure AD, terá de adicionar SignalFx a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar SignalFx a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

1. Na caixa de pesquisa, escreva **SignalFx**, selecione **SignalFx** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![SignalFx na lista de resultados](./media/signalfx-tutorial/tutorial_signalfx_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com SignalFx com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no SignalFx a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no SignalFx deve ser estabelecido.

Para configurar e testar o Azure AD início de sessão único com SignalFx, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste SignalFx](#create-a-signalfx-test-user)**  - para ter um equivalente da Eduarda Almeida na SignalFx que está ligado à representação de utilizador do Azure AD.
1. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo SignalFx.

**Para configurar o Azure AD início de sessão único com SignalFx, execute os seguintes passos:**

1. No portal do Azure, sobre o **SignalFx** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/signalfx-tutorial/tutorial_signalfx_samlbase.png)

1. Sobre o **SignalFx domínio e URLs** secção, execute os seguintes passos:

    ![SignalFx domínio e URLs únicas início de sessão em informações](./media/signalfx-tutorial/tutorial_signalfx_url.png)

    a. Na **identificador** caixa de texto, escreva um URL: `https://api.signalfx.com/v1/saml/metadata`

    b. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://api.signalfx.com/v1/saml/acs/<integration ID>`

    > [!NOTE] 
    > O valor anterior não é o valor real. Atualize o valor com o URL de resposta real, o que é explicado mais tarde no tutorial.

1. Aplicação de SignalFx espera que as asserções SAML num formato específico. Configure as seguintes declarações para esta aplicação. Pode gerir os valores destes atributos do **atributos de utilizador** secção na página de integração de aplicações. Captura de ecrã seguinte mostra um exemplo disso.   

    ![Configurar o início de sessão único](./media/signalfx-tutorial/tutorial_signalfx_attribute.png)

1. No **atributos de utilizador** secção sobre o **início de sessão único** caixa de diálogo, configurar o atributo de token de SAML conforme mostrado na imagem e execute os seguintes passos:
    
    | Nome de atributo | Valor do atributo |
    | ------------------- | -------------------- |    
    | User.FirstName          | user.givenname |
    | User.email          | user.mail |
    | PersonImmutableID       | user.userprincipalname    |
    | User.LastName       | user.surname    |

    a. Clique em **adicionar atributo** para abrir o **adicionar atributo** caixa de diálogo.

    ![Configurar o início de sessão único adicionar](./media/signalfx-tutorial/tutorial_attribute_04.png)

    ![Configurar Addattb de início de sessão único](./media/signalfx-tutorial/tutorial_attribute_05.png)

    b. Na **nome** caixa de texto, escreva o nome de atributo apresentado para essa linha.

    c. Partir do **valor** lista, digite o valor de atributo apresentado para essa linha.

    d. Deixe o **espaço de nomes** em branco.
    
    e. Clique em **OK**.
 
1. Sobre o **certificado de assinatura SAML** secção, execute os seguintes passos: 

    ![O link de download de certificado](./media/signalfx-tutorial/tutorial_signalfx_certificate.png)

    a. Clique no botão de cópia para copiar **Url de metadados de Federação de aplicação** e cole-o no bloco de notas.

    b. Clique em **Certificate(Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

1. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/signalfx-tutorial/tutorial_general_400.png)

1. Sobre o **SignalFx configuração** secção, clique em **configurar SignalFx** para abrir **configurar início de sessão** janela. Cópia a **ID de entidade de SAML** partir o **secção de referência rápida.**

    ![Configuração de SignalFx](./media/signalfx-tutorial/tutorial_signalfx_configure.png) 

1. Início de sessão no site da sua empresa SignalFx como administrador.

1. No SignalFx, sobre o superior, clique em **integrações** para abrir a página de integrações.

    ![Integração de SignalFx](./media/signalfx-tutorial/tutorial_signalfx_intg.png)

1. Clique em **do Azure Active Directory** mosaico sob **dos serviços de início de sessão** secção.
 
    ![SignalFx saml](./media/signalfx-tutorial/tutorial_signalfx_saml.png)

1. Clique em **nova integração** e, no **instalar** separador execute os seguintes passos:
 
    ![SignalFx samlintgpage](./media/signalfx-tutorial/tutorial_signalfx_azure.png)

    a. Na **Name** tipo de caixa de texto, um novo nome de integração, como **OurOrgName SAML SSO**.

    b. Copiar o **ID de integração** valor e de acréscimo com o **URL de resposta** como `https://api.signalfx.com/v1/saml/acs/<integration ID>` no **URL de resposta** caixa de texto de **SignalFx domínio e URLs** secção do portal do Azure.

    c. Clique em **carregar o ficheiro** para carregar o **certificado codificado Base64** transferido a partir do portal do Azure no **certificado** caixa de texto.

    d. Na **URL de emissor** caixa de texto, cole o valor de **ID de entidade de SAML**, que copiou do portal do Azure.

    e. Na **URL de metadados** caixa de texto, colar a **Url de metadados de Federação de aplicação** que copiou do portal do Azure.

    f. Clique em **Guardar**.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/signalfx-tutorial/create_aaduser_01.png)

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/signalfx-tutorial/create_aaduser_02.png)

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/signalfx-tutorial/create_aaduser_03.png)

1. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/signalfx-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
  
### <a name="create-a-signalfx-test-user"></a>Criar um utilizador de teste SignalFx

O objetivo desta secção é criar um usuário chamado Eduarda Almeida no SignalFx. SignalFx suporta o aprovisionamento de just-in-time, que está por predefinição, ativada. Não existe nenhum item de ação para nesta secção. Um novo utilizador é criado durante uma tentativa de aceder SignalFx se não existir ainda.

Quando um utilizador inicia sessão SignalFx do SAML SSO pela primeira vez, [equipa de suporte de SignalFx](mailto:kmazzola@signalfx.com) envia um e-mail com uma ligação que eles devem clicar para autenticar. Isso só acontecerá na primeira vez que o utilizador inicia sessão; tentativas de início de sessão subsequentes não irão precisar de validação de e-mail.

>[!Note]
>Se precisar de criar manualmente um utilizador, contacte [SignalFx a equipa de suporte](mailto:kmazzola@signalfx.com)

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para SignalFx.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a SignalFx, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **SignalFx**.

    ![A ligação de SignalFx na lista de aplicações](./media/signalfx-tutorial/tutorial_signalfx_app.png)  

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar o início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico SignalFx no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo SignalFx.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/signalfx-tutorial/tutorial_general_01.png
[2]: ./media/signalfx-tutorial/tutorial_general_02.png
[3]: ./media/signalfx-tutorial/tutorial_general_03.png
[4]: ./media/signalfx-tutorial/tutorial_general_04.png

[100]: ./media/signalfx-tutorial/tutorial_general_100.png

[200]: ./media/signalfx-tutorial/tutorial_general_200.png
[201]: ./media/signalfx-tutorial/tutorial_general_201.png
[202]: ./media/signalfx-tutorial/tutorial_general_202.png
[203]: ./media/signalfx-tutorial/tutorial_general_203.png

