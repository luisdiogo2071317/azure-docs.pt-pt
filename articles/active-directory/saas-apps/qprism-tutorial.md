---
title: 'Tutorial: Integração do Active Directory do Azure com QPrism | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e QPrism.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 72ab75ba-132b-4f83-a34b-d28b81b6d7bc
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/23/2018
ms.author: jeedes
ms.openlocfilehash: b59efe3faa00a172e5948f18b06337eedd2a1f6b
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55184806"
---
# <a name="tutorial-azure-active-directory-integration-with-qprism"></a>Tutorial: Integração do Active Directory do Azure com QPrism

Neste tutorial, saiba como integrar QPrism com o Azure Active Directory (Azure AD).

Integrar QPrism no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao QPrism.
- Pode permitir que os utilizadores automaticamente é feita para QPrism (início de sessão único) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central: portal do Azure.

Para obter mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com QPrism, terá dos seguintes itens:

- Uma subscrição do Azure
- Um QPrism logon único habilitado subscrição

Para testar os passos neste tutorial, siga as seguintes recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando QPrism da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="add-qprism-from-the-gallery"></a>Adicionar QPrism a partir da Galeria
Para configurar a integração do QPrism com o Azure AD, terá de adicionar QPrism a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar QPrism a partir da galeria:**

1. Na [portal do Azure](https://portal.azure.com), no painel esquerdo, selecione **Azure Active Directory**. 

    ![O botão do Azure Active Directory][1]

1. Navegue para **aplicações empresariais** > **todas as aplicações**.

    ![O painel de aplicações empresariais][2]
    
1. Para adicionar um novo aplicativo, na parte superior da caixa de diálogo, selecione **nova aplicação**.

    ![O novo botão de aplicativo][3]

1. Na caixa de pesquisa, escreva **QPrism**e selecione **QPrism** partir do painel de resultados. Em seguida, clique em **adicionar** para adicionar a aplicação.

    ![QPrism na lista de resultados](./media/qprism-tutorial/tutorial_qprism_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configurar e testar o Azure AD início de sessão único com QPrism, com base num utilizador de teste chamado "Eduarda Almeida."

Para o início de sessão único funcione, o Azure AD precisa saber que o utilizador de contraparte no QPrism for para um utilizador no Azure AD. Em outras palavras, tem de existir uma relação entre um utilizador do Azure AD e o utilizador relacionado no QPrism de ligado.

Para estabelecer esta relação na QPrism, atribua o valor do **nome de utilizador** no Azure AD como o valor da **Username**.

Para configurar e testar o Azure AD início de sessão único com QPrism, conclua os seguintes blocos de construção:

1. [Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on) para permitir aos utilizadores utilizar esta funcionalidade.
1. [Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user) para testar o Azure AD início de sessão único com Eduarda Almeida.
1. [Criar um utilizador de teste QPrism](#create-a-qprism-test-user) ter um equivalente da Eduarda Almeida na QPrism que está ligada à representação de utilizador do Azure AD.
1. [Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user) para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. [Testar início de sessão único](#test-single-sign-on) para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, ativar o Azure AD início de sessão único no portal do Azure e configurar o início de sessão único em seu aplicativo QPrism.

1. No portal do Azure, sobre o **QPrism** página de integração de aplicações, selecione **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/qprism-tutorial/tutorial_qprism_samlbase.png)

1. Na **QPrism domínio e URLs** secção, efetue o seguinte:

    ![QPrism domínio e URLs únicas início de sessão em informações](./media/qprism-tutorial/tutorial_qprism_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL que utiliza o seguinte padrão: `https://<customer domain>.qmyzone.com/login`

    b. Na **identificador** caixa de texto, escreva um URL que utiliza o seguinte padrão: `https://<customer domain>.qmyzone.com/metadata.php`
         
    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o identificador real e início de sessão no URL. Contacte [equipa de suporte de cliente QPrism](mailto:qsupport-ce@quatrro.com) obter esses valores. 

1. Sobre o **certificado de assinatura SAML** secção, clique no botão de cópia para copiar **Url de metadados de Federação de aplicação** e cole-o no bloco de notas.

     ![O link de download de certificado](./media/qprism-tutorial/tutorial_qprism_certificate.png)

1. Selecione **Guardar**.

    ![Configurar o início de sessão único botão Save](./media/qprism-tutorial/tutorial_general_400.png)
    
1. Para configurar o início de sessão único num **QPrism** lado, terá de enviar o **Url de metadados de Federação de aplicação** para [equipa de suporte de QPrism](mailto:qsupport-ce@quatrro.com). Se definir esta definição para que a ligação de SAML SSO definidas corretamente em ambos os lados.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD:**

1. No portal do Azure, no painel esquerdo, selecione **do Azure Active Directory**.

    ![O botão do Azure Active Directory](./media/qprism-tutorial/create_aaduser_01.png)

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/qprism-tutorial/create_aaduser_02.png)

1. Para abrir o **usuário** caixa de diálogo, na parte superior a **todos os utilizadores** caixa de diálogo, selecione **Add**.

    ![Botão Adicionar](./media/qprism-tutorial/create_aaduser_03.png)

1. Na **utilizador** diálogo caixa, faça o seguinte:

    ![A caixa de diálogo de utilizador](./media/qprism-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Selecione **Criar**.
 
### <a name="create-a-qprism-test-user"></a>Criar um utilizador de teste QPrism

Nesta secção, vai criar um usuário chamado Eduarda Almeida no QPrism. Trabalhar com o [equipa de suporte de QPrism](mailto:qsupport-ce@quatrro.com) para adicionar utilizadores na plataforma de QPrism. Os utilizadores tem de ser criados e ativados antes de utilizar o início de sessão único. 

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para QPrism.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a QPrism:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório. Aceda a **aplicações empresariais**e selecione **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **QPrism**.

    ![A ligação de QPrism na lista de aplicações](./media/qprism-tutorial/tutorial_qprism_app.png)  

1. No menu à esquerda, selecione **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

1. Selecione **Adicionar**. Em seguida, em **adicionar atribuição**, selecione **utilizadores e grupos**.

    ![O painel Adicionar atribuição][203]

1. Sobre o **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** no **utilizadores** lista.

1. Sobre o **utilizadores e grupos** caixa de diálogo, selecione **selecione**.

1. Sob **adicionar atribuição**, selecione **atribuir**.
    
### <a name="test-single-sign-on"></a>Testar o início de sessão único

Nesta secção, vai testar seu única início de sessão em configuração do Azure AD utilizando o painel de acesso.

No painel de acesso, ao selecionar o mosaico QPrism, deve obter automaticamente a sessão iniciada sua aplicação QPrism.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/qprism-tutorial/tutorial_general_01.png
[2]: ./media/qprism-tutorial/tutorial_general_02.png
[3]: ./media/qprism-tutorial/tutorial_general_03.png
[4]: ./media/qprism-tutorial/tutorial_general_04.png

[100]: ./media/qprism-tutorial/tutorial_general_100.png

[200]: ./media/qprism-tutorial/tutorial_general_200.png
[201]: ./media/qprism-tutorial/tutorial_general_201.png
[202]: ./media/qprism-tutorial/tutorial_general_202.png
[203]: ./media/qprism-tutorial/tutorial_general_203.png

