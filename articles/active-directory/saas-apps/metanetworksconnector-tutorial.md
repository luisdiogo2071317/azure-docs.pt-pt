---
title: 'Tutorial: Integração do Active Directory do Azure com o conector de redes de Meta | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e o conector de redes de metadados.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 4ae5f30d-113b-4261-b474-47ffbac08bf7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3c4d702a33742da5501be208154cbdd593fa0524
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2019
ms.locfileid: "56199719"
---
# <a name="tutorial-azure-active-directory-integration-with-meta-networks-connector"></a>Tutorial: Integração do Active Directory do Azure com o conector de redes de metadados

Neste tutorial, saiba como integrar o conector de redes de Meta no Azure Active Directory (Azure AD).

Integrar o conector de redes de Meta no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao conector de redes de metadados.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para o conector de redes de Meta (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o conector de redes de Meta, terá dos seguintes itens:

- Uma subscrição do Azure
- Um conector de redes de Meta-início de sessão único de subscrição ativada

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionar o conector de redes de Meta a partir da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-meta-networks-connector-from-the-gallery"></a>Adicionar o conector de redes de Meta a partir da Galeria
Para configurar a integração do conector de redes de Meta no Azure AD, terá de adicionar Meta redes conector a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Meta redes conector a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 
    
    ![O botão do Azure Active Directory][1]
    
1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.
    
    ![O painel de aplicações empresariais][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.
    
    ![O novo botão de aplicativo][3]
    
1. Na caixa de pesquisa, escreva **conector de redes de Meta**, selecione **Meta redes conector** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.
    
    ![Meta redes conector na lista de resultados](./media/metanetworksconnector-tutorial/tutorial_metanetworksconnector_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com o conector de redes de metadados com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador equivalente no conector de redes de metadados para um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no conector de redes de Meta deve ser estabelecido.

Para configurar e testar o Azure AD início de sessão único com Meta redes conector, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste do conector de redes de Meta](#create-a-meta-networks-connector-test-user)**  - para ter um equivalente da Eduarda Almeida no conector de redes de metadados que está ligado à representação de utilizador do Azure AD.
1. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único na sua aplicação de conector de redes de Meta.

**Para configurar o Azure AD início de sessão único com o conector de redes de Meta, execute os seguintes passos:**

1. No portal do Azure, sobre o **Meta redes conector** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/metanetworksconnector-tutorial/tutorial_metanetworksconnector_samlbase.png)

1. Sobre o **Meta redes conector do domínio e URLs** secção, execute os seguintes passos, se desejar configurar a aplicação no **IDP** iniciada pelo modo:

    ![Meta redes conector do domínio e URLs únicas início de sessão em informações](./media/metanetworksconnector-tutorial/tutorial_metanetworksconnector_url.png)

    1. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://login.nsof.io/v1/<ORGANIZATION-SHORT-NAME>/saml/metadata`
    
    1. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://login.nsof.io/v1/<ORGANIZATION-SHORT-NAME>/sso/saml`
    
1. Verifique **Mostrar definições de URL avançadas** e executar o passo seguinte, se desejar configurar a aplicação na **SP** iniciada pelo modo:

    ![Meta redes conector do domínio e URLs únicas início de sessão em informações](./media/metanetworksconnector-tutorial/tutorial_metanetworksconnector_url1.png)

    1. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<ORGANIZATION-SHORT-NAME>.metanetworks.com/login`
    
    1. Na **estado de reencaminhamento** caixa de texto, escreva um URL com o seguinte padrão: `https://<ORGANIZATION-SHORT-NAME>.metanetworks.com/#/`
    
    > [!NOTE]
    > Estes valores não são reais. Atualize estes valores com o identificador real, o URL de resposta, e o URL de início de sessão são explicadas mais tarde no tutorial.
    
1. Conector de redes de meta espera que as asserções SAML num formato específico. Configure as seguintes declarações para esta aplicação. Pode gerir os valores destes atributos do **atributos de utilizador** secção na página de integração de aplicações. Captura de ecrã seguinte mostra um exemplo disso.
    
    ![Configurar o início de sessão único](./media/metanetworksconnector-tutorial/tutorial_metanetworksconnector_attribute.png)
    
1. No **atributos de utilizador** secção sobre o **início de sessão único** caixa de diálogo, configurar o atributo de token de SAML conforme mostrado na imagem acima e execute os seguintes passos:
    
    | Nome do Atributo | Valor do Atributo | ESPAÇO DE NOMES|
    | ---------------| --------------- | -------- |
    | FirstName | user.givenname | |
    | Apelido | user.surname | |
    | emailaddress| user.mail| `http://schemas.xmlsoap.org/ws/2005/05/identity/claims` |
    | nome | user.userprincipalname| `http://schemas.xmlsoap.org/ws/2005/05/identity/claims` |
    | telefone | user.telephonenumber | |

    1. Clique em **adicionar atributo** para abrir o **adicionar atributo** caixa de diálogo.

        ![Configurar o início de sessão único](./media/metanetworksconnector-tutorial/tutorial_attribute_04.png)
    
        ![Configurar o início de sessão único](./media/metanetworksconnector-tutorial/tutorial_attribute_05.png)   
    
    1. Na **nome** caixa de texto, escreva o nome de atributo apresentado para essa linha.
    
    1. Partir do **valor** lista, digite o valor de atributo apresentado para essa linha.
    
    1. Na **espaço de nomes** caixa de texto, digite o valor de espaço de nomes mostrado para essa linha.
    
    1. Clique em **Ok**
    
1. Sobre o **certificado de assinatura SAML** secção, clique em **Certificate(Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.
    
    ![O link de download de certificado](./media/metanetworksconnector-tutorial/tutorial_metanetworksconnector_certificate.png)
    
1. Sobre o **configuração do conector de redes de Meta** secção, clique em **configurar conector de redes de Meta** para abrir **configurar início de sessão** janela. Cópia a **ID de entidade de SAML e SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**
    
    ![Configurar o início de sessão único](./media/metanetworksconnector-tutorial/tutorial_metanetworksconnector_configure.png)
    
1. Clique em **guardar** botão.
    
    ![Configurar o botão único início de sessão em Guardar](./media/metanetworksconnector-tutorial/tutorial_general_400.png)
    
1. Abra um novo separador no seu browser e inicie sessão sua conta de administrador do conector de redes de Meta.
    
    > [!NOTE]
    > Conector de redes de meta é um sistema seguro. Portanto, antes de aceder ao seu portal tem de obter a lista de permissões de endereço IP pública no seu lado. Para obter seu endereço IP público, siga a ligação especificada abaixo [aqui](https://whatismyipaddress.com/). Enviar o seu endereço IP para o [equipa de suporte de cliente de conector de redes de Meta](mailto:support@metanetworks.com) para obter a lista de permissões de endereço IP.
    
1. Aceda a **administrador** e selecione **definições**.
    
    ![Configurar o início de sessão único](./media/metanetworksconnector-tutorial/configure3.png)
    
1. Certifique-se **Registar tráfego de Internet** e **força VPN MFA** estão definidos como desativado.
    
    ![Configurar o início de sessão único](./media/metanetworksconnector-tutorial/configure1.png)
    
1. Aceda a **administrador** e selecione **SAML**.
    
    ![Configurar o início de sessão único](./media/metanetworksconnector-tutorial/configure4.png)
    
1. Execute os seguintes passos no **detalhes** página:
    
    ![Configurar o início de sessão único](./media/metanetworksconnector-tutorial/configure2.png)
    
    1. Cópia **URL de SSO** valor e cole-o no **URL de início de sessão** caixa de texto no **Meta redes conector do domínio e URLs** secção.
    
    1. Cópia **URL de destinatário** valor e cole-o no **URL de resposta** caixa de texto no **Meta redes conector do domínio e URLs** secção.
    
    1. Cópia **URI de audiência (ID de entidade de SP)** valor e cole-o no **identificador (ID de entidade)** caixa de texto no **Meta redes conector do domínio e URLs** secção.
    
    1. Ativar o SAML
    
1. Sobre o **gerais** separador. Execute os seguintes passos:

    ![Configurar o início de sessão único](./media/metanetworksconnector-tutorial/configure5.png)

    1. Na **URL fornecedor de identidade única Sign-On**, cole a **SAML único início de sessão no URL do serviço** valor que copiou do portal do Azure.

    1. Na **emissor do fornecedor de identidade**, cole a **ID de entidade de SAML** valor que copiou do portal do Azure.

    1. Abra o certificado transferido a partir do portal do Azure no bloco de notas, cole-o no **certificado X.509** caixa de texto.

    1. Ativar a **Just-in-Time aprovisionamento**.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.
    
![Criar um utilizador de teste do Azure AD][100]
    
**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.
    
    ![O botão do Azure Active Directory](./media/metanetworksconnector-tutorial/create_aaduser_01.png)
    
1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.
    
    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/metanetworksconnector-tutorial/create_aaduser_02.png)
    
1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.
    
    ![Botão Adicionar](./media/metanetworksconnector-tutorial/create_aaduser_03.png)
    
1. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/metanetworksconnector-tutorial/create_aaduser_04.png)
    
    1. Na **Name** , escreva **BrittaSimon**.

    1. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.
    
    1. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.
    
    1. Clique em **Criar**.
    
### <a name="create-a-meta-networks-connector-test-user"></a>Criar um utilizador de teste do conector de redes de metadados

O objetivo desta secção é criar um usuário chamado Eduarda Almeida no conector de redes de metadados. Meta redes conector suporta o aprovisionamento de just-in-time, que está por predefinição, ativada. Não existe nenhum item de ação para nesta secção. Um novo utilizador é criado durante uma tentativa de aceder a Meta redes conector, se não existir ainda.

>[!Note]
>Se precisar de criar manualmente um utilizador, contacte [equipa de suporte de cliente de conector de redes de Meta](mailto:support@metanetworks.com).

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso ao conector de redes de Meta.

![Atribuir a função de utilizador][200]

**Para atribuir a Eduarda Almeida a Meta redes conector, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.
    
    ![Atribuir utilizador][201]
    
1. Na lista de aplicações, selecione **Meta redes conector**.
    
    ![A ligação de conector de redes de metadados na lista de aplicações](./media/metanetworksconnector-tutorial/tutorial_metanetworksconnector_app.png)  
    
1. No menu à esquerda, clique em **utilizadores e grupos**.
    
    ![A ligação "Utilizadores e grupos"][202]
    
1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.
    
    ![O painel Adicionar atribuição][203]
    
1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.
    
1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.
    
1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar o início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico de conector de redes de Meta no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação de conector de redes de metadados.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionais

- [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
- [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[1]: ./media/metanetworksconnector-tutorial/tutorial_general_01.png
[2]: ./media/metanetworksconnector-tutorial/tutorial_general_02.png
[3]: ./media/metanetworksconnector-tutorial/tutorial_general_03.png
[4]: ./media/metanetworksconnector-tutorial/tutorial_general_04.png

[100]: ./media/metanetworksconnector-tutorial/tutorial_general_100.png

[200]: ./media/metanetworksconnector-tutorial/tutorial_general_200.png
[201]: ./media/metanetworksconnector-tutorial/tutorial_general_201.png
[202]: ./media/metanetworksconnector-tutorial/tutorial_general_202.png
[203]: ./media/metanetworksconnector-tutorial/tutorial_general_203.png

