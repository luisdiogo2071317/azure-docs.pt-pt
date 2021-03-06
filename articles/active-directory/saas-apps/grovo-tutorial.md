---
title: 'Tutorial: Integração do Active Directory do Azure com Grovo | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Grovo.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 399cecc3-aa62-4914-8b6c-5a35289820c1
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/12/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 535e4a9c0555636d4ed068a7983b31c2cb91e75f
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2019
ms.locfileid: "56198769"
---
# <a name="tutorial-azure-active-directory-integration-with-grovo"></a>Tutorial: Integração do Active Directory do Azure com Grovo

Neste tutorial, saiba como integrar Grovo com o Azure Active Directory (Azure AD).

Integrar Grovo no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Grovo.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Grovo (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Grovo, terá dos seguintes itens:

- Uma subscrição do Azure
- Um Grovo logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Grovo da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-grovo-from-the-gallery"></a>Adicionando Grovo da Galeria
Para configurar a integração do Grovo com o Azure AD, terá de adicionar Grovo a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Grovo a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

1. Na caixa de pesquisa, escreva **Grovo**, selecione **Grovo** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Grovo na lista de resultados](./media/grovo-tutorial/tutorial_grovo_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com Grovo com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Grovo a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Grovo deve ser estabelecido.

Grovo, atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com Grovo, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste Grovo](#create-a-grovo-test-user)**  - para ter um equivalente da Eduarda Almeida na Grovo que está ligado à representação de utilizador do Azure AD.
1. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo Grovo.

**Para configurar o Azure AD início de sessão único com Grovo, execute os seguintes passos:**

1. No portal do Azure, sobre o **Grovo** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/grovo-tutorial/tutorial_grovo_samlbase.png)

1. Sobre o **Grovo domínio e URLs** secção, execute os seguintes passos, se desejar configurar a aplicação no **IDP** iniciada pelo modo:

    ![Grovo domínio e URLs únicas início de sessão em informações](./media/grovo-tutorial/tutorial_grovo_url.png)

    a. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://<subdomain>.grovo.com/sso/saml2/metadata`

    b. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://<subdomain>.grovo.com/sso/saml2/saml-assertion`

1. Verifique **Mostrar definições de URL avançadas**, executar o passo seguinte:

    ![Grovo domínio e URLs únicas início de sessão em informações](./media/grovo-tutorial/tutorial_grovo_url1.png)

    a. Na **estado de reencaminhamento** caixa de texto, escreva um URL com o seguinte padrão:`https://<subdomain>.grovo.com`

    b. Se desejar configurar a aplicação no **SP** iniciada pelo modo, execute os seguintes passos:

    ![Grovo domínio e URLs únicas início de sessão em informações](./media/grovo-tutorial/tutorial_grovo_url2.png)
    
    Na **iniciar sessão no URL** caixa de texto, escreva um URL com o seguinte padrão: `https://<subdomain>.grovo.com/sso/saml2/saml-assertion`

    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o sinal de identificador, o URL de resposta, real no estado de URL e o reencaminhamento. Contacte [equipa de suporte de Grovo](https://www.grovo.com/contact-us) obter esses valores.
 
1. Aplicação de Grovo espera que as asserções SAML num formato específico. Configure as seguintes declarações para esta aplicação. Pode gerir os valores destes atributos da "**atributos de utilizador**" secção na página de integração de aplicações. Mapeie **identificador de utilizador** com **user.mail** e configurar outros atributos, conforme mostrado na captura de ecrã abaixo.
    
    ![Configurar o início de sessão único attb](./media/grovo-tutorial/tutorial_grovo_attribute.png)
    
1. No **atributos de utilizador** secção sobre o **início de sessão único** caixa de diálogo, configurar o atributo de token de SAML conforme mostrado na imagem e execute os seguintes passos:
    
    | Nome do Atributo | Valor do Atributo |
    | ------------------- | -------------------- |    
    | Nome Próprio          | user.givenname |
    | Apelido           | user.surname |
    | Endereço de E-mail       | user.mail    |
    | employeeID          | user.employeeid |

    a. Clique em **adicionar atributo** para abrir o **adicionar atributo** caixa de diálogo.

    ![Configurar o início de sessão único adicionar](./media/grovo-tutorial/tutorial_attribute_04.png)

    ![Configurar Addattb de início de sessão único](./media/grovo-tutorial/tutorial_attribute_05.png)

    b. Na **nome** caixa de texto, escreva o nome de atributo apresentado para essa linha.

    c. Partir do **valor** lista, digite o valor de atributo apresentado para essa linha.

    d. Deixe o **espaço de nomes** em branco.
    
    e. Clique em **OK**.


1. Sobre o **certificado de assinatura SAML** secção, clique em **Certificate(Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![O link de download de certificado](./media/grovo-tutorial/tutorial_grovo_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/grovo-tutorial/tutorial_general_400.png)

1. Sobre o **Grovo configuração** secção, clique em **configurar Grovo** para abrir **configurar início de sessão** janela. Cópia a **ID de entidade de SAML e SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configuração de Grovo](./media/grovo-tutorial/tutorial_grovo_configure.png) 

1. Numa janela do browser web diferente, início de sessão para Grovo como administrador.

1. Aceda a **administrador** > **integrações**.
 
    ![Configuração de Grovo](./media/grovo-tutorial/tutorial_grovo_admin.png) 

1. Clique em **CONFIGURADA** sob **SP iniciada SAML 2.0** secção.

    ![Configuração de Grovo](./media/grovo-tutorial/tutorial_grovo_setup.png)

1. Na **SP iniciada SAML 2.0** janela pop-up, execute os seguintes passos:

    ![Configuração de Grovo](./media/grovo-tutorial/tutorial_grovo_saml.png)

    a. Na **id de entidade** caixa de texto, cole o valor de **ID de entidade de SAML**, que copiou do portal do Azure.

    b. Na **início de sessão no ponto final de serviço único** caixa de texto, cole o valor de **SAML único início de sessão no URL do serviço**, que copiou do portal do Azure.

    c. Selecione **início de sessão na ligação de ponto final de serviço único** como `urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect`.
    
    d. Abra o transferido **certificado codificado Base64** partir do portal do Azure no bloco de notas, cole-o no **chave pública** caixa de texto.

    e. Clique em **Seguinte**.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [Documentação do Azure AD incorporado]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/grovo-tutorial/create_aaduser_01.png)

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/grovo-tutorial/create_aaduser_02.png)

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/grovo-tutorial/create_aaduser_03.png)

1. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/grovo-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
  
### <a name="create-a-grovo-test-user"></a>Criar um utilizador de teste Grovo

O objetivo desta secção é criar um usuário chamado Eduarda Almeida no Grovo. Grovo suporta o aprovisionamento de just-in-time, que está por predefinição, ativada. Não existe nenhum item de ação para nesta secção. Um novo utilizador é criado durante uma tentativa de aceder Grovo se não existir ainda.
>[!Note]
>Se precisar de criar manualmente um utilizador, contacte [equipa de suporte de Grovo](https://www.grovo.com/contact-us).

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para Grovo.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a Grovo, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **Grovo**.

    ![A ligação de Grovo na lista de aplicações](./media/grovo-tutorial/tutorial_grovo_app.png)  

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar o início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico Grovo no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo Grovo.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/grovo-tutorial/tutorial_general_01.png
[2]: ./media/grovo-tutorial/tutorial_general_02.png
[3]: ./media/grovo-tutorial/tutorial_general_03.png
[4]: ./media/grovo-tutorial/tutorial_general_04.png

[100]: ./media/grovo-tutorial/tutorial_general_100.png

[200]: ./media/grovo-tutorial/tutorial_general_200.png
[201]: ./media/grovo-tutorial/tutorial_general_201.png
[202]: ./media/grovo-tutorial/tutorial_general_202.png
[203]: ./media/grovo-tutorial/tutorial_general_203.png

