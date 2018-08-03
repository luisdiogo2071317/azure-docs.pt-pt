---
title: 'Tutorial: Integração do Azure Active Directory com a formação de Conscientização de segurança de KnowBe4 | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e o treinamento de Conscientização de segurança de KnowBe4.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: b80d2212-cc5f-4adb-836c-570640810c39
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/31/2017
ms.author: jeedes
ms.openlocfilehash: c27af4e7bc88f24b76310336859740d8795f7cbe
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39449278"
---
# <a name="tutorial-azure-active-directory-integration-with-knowbe4-security-awareness-training"></a>Tutorial: Integração do Azure Active Directory com a formação de Conscientização de segurança de KnowBe4

Neste tutorial, saiba como integrar o treinamento de Conscientização de segurança de KnowBe4 com o Azure Active Directory (Azure AD).

Treinamento de Conscientização de segurança de KnowBe4 a integração com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao treinamento de Conscientização de segurança de KnowBe4.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para treinamento de Conscientização de segurança de KnowBe4 (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com a formação de Conscientização de segurança de KnowBe4, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um treinamento de Conscientização de segurança de KnowBe4 logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando o treinamento de Conscientização de segurança de KnowBe4 da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-knowbe4-security-awareness-training-from-the-gallery"></a>Adicionando o treinamento de Conscientização de segurança de KnowBe4 da Galeria
Para configurar a integração de treinamento de Conscientização de segurança de KnowBe4 com o Azure AD, terá de adicionar o treinamento de Conscientização de segurança de KnowBe4 partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar o treinamento de Conscientização de segurança de KnowBe4 partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

1. Na caixa de pesquisa, escreva **treinamento de Conscientização de segurança de KnowBe4**, selecione **treinamento de Conscientização de segurança de KnowBe4** no painel de resultados, em seguida, clique em **Add** botão para adicionar o aplicação.

    ![Treinamento de Conscientização de segurança de KnowBe4 na lista de resultados](./media/knowbe4-tutorial/tutorial_knowbe4_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com a formação de Conscientização de segurança de KnowBe4 com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no treinamento de Conscientização de segurança de KnowBe4 a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no treinamento de Conscientização de segurança de KnowBe4 deve ser estabelecido.

No treinamento de Conscientização de segurança de KnowBe4, atribuir o valor do **nome de utilizador** no Azure AD como o valor da **nome de utilizador** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com a formação de Conscientização de segurança de KnowBe4, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste de treinamento de Conscientização de segurança de KnowBe4](#create-a-knowbe4-security-awareness-training-test-user)**  - para ter um equivalente da Eduarda Almeida na formação de Conscientização de segurança KnowBe4 que está ligado à representação de utilizador do Azure AD.
1. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo de treinamento de Conscientização de segurança de KnowBe4.

**Para configurar o Azure AD início de sessão único com a formação de Conscientização de segurança de KnowBe4, execute os seguintes passos:**

1. No portal do Azure, sobre o **treinamento de Conscientização de segurança de KnowBe4** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/knowbe4-tutorial/tutorial_knowbe4_samlbase.png)

1. Sobre o **URLs de domínio de treinamento de Conscientização de segurança de KnowBe4 e** secção, execute os seguintes passos:

    ![URLs de domínio de treinamento de Conscientização de segurança de KnowBe4 e únicas início de sessão em informações](./media/knowbe4-tutorial/tutorial_knowbe4_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<companyname>.KnowBe4.com/auth/saml/<instancename>`

    > [!NOTE] 
    > Valor do URL de início de sessão não é real. Atualize este valor com o URL de início de sessão real. Contacte [equipa de suporte de cliente de treinamento de Conscientização de segurança de KnowBe4](mailto:support@KnowBe4.com) para obter este valor. 

    b. Na **identificador** caixa de texto, digite o valor de cadeia de caracteres: `KnowBe4`

    > [!NOTE]
    >Diferencia maiúsculas de minúsculas

1. Sobre o **certificado de assinatura SAML** secção, clique em **certificado (bruto)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![O link de download de certificado](./media/knowbe4-tutorial/tutorial_knowbe4_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/knowbe4-tutorial/tutorial_general_400.png)

1. Sobre o **configuração de treinamento de Conscientização de segurança de KnowBe4** secção, clique em **configurar treinamento de Conscientização de segurança de KnowBe4** para abrir **configurar início de sessão** janela. Cópia a **URL de fim de sessão, o ID de entidade de SAML e o SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configuração de treinamento de Conscientização de segurança de KnowBe4](./media/knowbe4-tutorial/tutorial_knowbe4_configure.png) 

1. Para configurar o início de sessão único num **treinamento de Conscientização de segurança de KnowBe4** lado, terá de enviar o transferido **certificado (bruto)**, **URL de fim de sessão, ID de entidade de SAML e SAML Single Sign-On URL do serviço** para [equipa de suporte de cliente de treinamento de Conscientização de segurança de KnowBe4](mailto:support@KnowBe4.com).

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/knowbe4-tutorial/create_aaduser_01.png)

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/knowbe4-tutorial/create_aaduser_02.png)

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/knowbe4-tutorial/create_aaduser_03.png)

1. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/knowbe4-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-knowbe4-security-awareness-training-test-user"></a>Criar um utilizador de teste de treinamento de Conscientização de segurança de KnowBe4

O objetivo desta secção é criar um usuário chamado Eduarda Almeida no treinamento de Conscientização de segurança de KnowBe4. Treinamento de Conscientização de segurança de KnowBe4 suporta o aprovisionamento de just-in-time, que está por predefinição, ativada.

Não existe nenhum item de ação para nesta secção. Um novo utilizador é criado durante a tentativa de aceder à formação de Conscientização de segurança de KnowBe4 se não existir ainda. 

>[!NOTE]
>Se precisar de criar manualmente um utilizador, terá de contactar o [equipa de suporte de treinamento de Conscientização de segurança de KnowBe4](mailto:support@KnowBe4.com).

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso ao treinamento de Conscientização de segurança de KnowBe4.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a formação de Conscientização de segurança de KnowBe4, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **treinamento de Conscientização de segurança de KnowBe4**.

    ![A ligação de treinamento de Conscientização de segurança de KnowBe4 na lista de aplicações](./media/knowbe4-tutorial/tutorial_knowbe4_app.png)  

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

O objetivo desta secção é testar a configuração do Azure AD única início de sessão com o painel de acesso.
  
Quando clica no mosaico de treinamento de Conscientização de segurança de KnowBe4 no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação de treinamento de Conscientização de segurança de KnowBe4. 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[1]: ./media/knowbe4-tutorial/tutorial_general_01.png
[2]: ./media/knowbe4-tutorial/tutorial_general_02.png
[3]: ./media/knowbe4-tutorial/tutorial_general_03.png
[4]: ./media/knowbe4-tutorial/tutorial_general_04.png

[100]: ./media/knowbe4-tutorial/tutorial_general_100.png

[200]: ./media/knowbe4-tutorial/tutorial_general_200.png
[201]: ./media/knowbe4-tutorial/tutorial_general_201.png
[202]: ./media/knowbe4-tutorial/tutorial_general_202.png
[203]: ./media/knowbe4-tutorial/tutorial_general_203.png

