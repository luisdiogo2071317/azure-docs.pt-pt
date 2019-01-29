---
title: 'Tutorial: Integração do Active Directory do Azure com Vodeclic | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Vodeclic.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: d77a0f53-e3a3-445e-ab3e-119cef6e2e1d
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2017
ms.author: jeedes
ms.openlocfilehash: 14dc205857a6a067421fbb51dbbbf0c35c556e0e
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55150602"
---
# <a name="tutorial-azure-active-directory-integration-with-vodeclic"></a>Tutorial: Integração do Active Directory do Azure com Vodeclic

Neste tutorial, saiba como integrar Vodeclic com o Azure Active Directory (Azure AD).

Integrar Vodeclic no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Vodeclic.
- Pode permitir que os utilizadores automaticamente é feita para Vodeclic (início de sessão único, ou SSO) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central, o portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Vodeclic, terá dos seguintes itens:

- Uma subscrição do Azure
- Uma subscrição Vodeclic SSO ativado

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, siga as seguintes recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, [obtenha uma avaliação gratuita de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Vodeclic da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="add-vodeclic-from-the-gallery"></a>Adicionar Vodeclic a partir da Galeria
Para configurar a integração do Vodeclic com o Azure AD, terá de adicionar Vodeclic a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Vodeclic a partir da galeria, siga os passos seguintes:**

1. Na [portal do Azure](https://portal.azure.com), no painel esquerdo, selecione a **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

1. Aceda a **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
1. Para adicionar uma nova aplicação, selecione o **nova aplicação** botão na parte superior da caixa de diálogo.

    ![O novo botão de aplicativo][3]

1. Na caixa de pesquisa, escreva **Vodeclic**. Selecione **Vodeclic** no painel de resultados e, em seguida, selecione a **Add** botão para adicionar a aplicação.

    ![Vodeclic na lista de resultados](./media/vodeclic-tutorial/tutorial_vodeclic_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com Vodeclic com base num utilizador de teste chamado "Eduarda Almeida."

Para o início de sessão único funcione, o Azure AD precisa saber que o utilizador de contraparte no Vodeclic for para um utilizador no Azure AD. Em outras palavras, tem de estabelecer uma ligação entre um utilizador do Azure AD e o utilizador relacionado no Vodeclic.

Na Vodeclic, atribua o valor **nome de utilizador** o mesmo valor como **nome de utilizador** no Azure AD. Agora que estabeleceu a ligação entre os dois usuários.

Para configurar e testar o Azure AD início de sessão único com Vodeclic, conclua os seguintes blocos de construção:

1. [Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on) para permitir aos utilizadores utilizar esta funcionalidade.
1. [Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user) para testar o Azure AD início de sessão único com Eduarda Almeida.
1. [Criar um utilizador de teste Vodeclic](#create-a-vodeclic-test-user) ter um equivalente da Eduarda Almeida na Vodeclic que está ligado a representação do Azure AD do utilizador.
1. [Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user) para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. [Testar início de sessão único](#test-single-sign-on) para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo Vodeclic.

**Para configurar o Azure AD início de sessão único com Vodeclic, siga os passos seguintes:**

1. No portal do Azure, sobre o **Vodeclic** página de integração de aplicações, selecione **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

1. Na **início de sessão único** caixa de diálogo em **modo de início de sessão único**, selecione **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/vodeclic-tutorial/tutorial_vodeclic_samlbase.png)

1. Se quiser configurar a aplicação no **IDP** iniciadas pelo modo, na **Vodeclic domínio e URLs** secção, siga os passos seguintes:

    ![Informações de início de sessão de único Vodeclic domínio e URLs](./media/vodeclic-tutorial/tutorial_vodeclic_url.png)

    a. Na **identificador** caixa, escreva um URL com o seguinte padrão: `https://<companyname>.lms.vodeclic.net/auth/saml`

    b. Na **URL de resposta** caixa, escreva um URL com o seguinte padrão: `https://<companyname>.lms.vodeclic.net/auth/saml/callback`

1. Se quiser configurar a aplicação no **SP** modo iniciado, selecione a **Mostrar definições de URL avançadas** caixa de verificação e executar os seguintes passos:

    ![Informações de início de sessão de único Vodeclic domínio e URLs](./media/vodeclic-tutorial/tutorial_vodeclic_url1.png)

    Na **URL de início de sessão** caixa, escreva um URL com o seguinte padrão: `https://<companyname>.lms.vodeclic.net/auth/saml`
     
    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o identificador real, URL de resposta e início de sessão no URL. Contacte os [equipa de suporte de cliente Vodeclic](mailto:hotline@vodeclic.com) obter esses valores.

1. Na **certificado de assinatura SAML** secção, selecione **XML de metadados**. Em seguida, guarde o ficheiro de metadados no seu computador.

    ![O link de download de certificado](./media/vodeclic-tutorial/tutorial_vodeclic_certificate.png) 

1. Selecione **Guardar**.

    ![Configurar o botão único início de sessão em Guardar](./media/vodeclic-tutorial/tutorial_general_400.png)
    
1. Para configurar o início de sessão único no **Vodeclic** lado, envie o transferido **XML de metadados** para o [equipa de suporte de Vodeclic](mailto:hotline@vodeclic.com). Se definir esta definição para que a ligação de SAML SSO definidas corretamente em ambos os lados.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com) enquanto estiver a configurar a aplicação. Depois de adicionar esta aplicação a partir da **do Active Directory** > **aplicações empresariais** secção, selecione o **Single Sign-On** separador e acessar o embedded documentação através da **configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded em [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, siga os passos seguintes:**

1. No portal do Azure, no painel esquerdo, selecione o **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/vodeclic-tutorial/create_aaduser_01.png)

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**. Em seguida, selecione **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/vodeclic-tutorial/create_aaduser_02.png)

1. Para abrir o **utilizador** caixa de diálogo, selecione **Add** na parte superior do **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/vodeclic-tutorial/create_aaduser_03.png)

1. Na **utilizador** diálogo caixa, siga os passos seguintes:

    ![A caixa de diálogo de utilizador](./media/vodeclic-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Selecione **Criar**.
 
### <a name="create-a-vodeclic-test-user"></a>Criar um utilizador de teste Vodeclic

Nesta secção, vai criar um usuário chamado Eduarda Almeida no Vodeclic. Trabalhar com o [equipa de suporte de Vodeclic](mailto:hotline@vodeclic.com) para adicionar os utilizadores na plataforma Vodeclic. Os utilizadores tem de ser criados e ativados antes de utilizar o início de sessão único.

> [!NOTE]
> De acordo com requisitos de aplicações, poderá ter de obter a lista de permissões de máquina. Para que isso aconteça, precisa compartilhar seu endereço IP público com o [equipa de suporte de Vodeclic](mailto:hotline@vodeclic.com).

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para Vodeclic.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a Vodeclic, siga os passos seguintes:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, vá para o modo de exibição do diretório. Em seguida, aceda a **aplicações empresariais**e, em seguida, selecione **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **Vodeclic**.

    ![A ligação de Vodeclic na lista de aplicações](./media/vodeclic-tutorial/tutorial_vodeclic_app.png)  

1. No menu à esquerda, selecione **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

1. Selecione o **adicionar** botão. Em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

1. Na **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** no **utilizadores** lista.

1. Na **utilizadores e grupos** caixa de diálogo, selecione a **selecione** botão.

1. Na **adicionar atribuição** caixa de diálogo, selecione a **atribuir** botão.
    
### <a name="test-single-sign-on"></a>Testar o início de sessão único

Nesta secção, vai testar seu única início de sessão em configuração do Azure AD utilizando o painel de acesso.

Ao selecionar o mosaico Vodeclic no painel de acesso, obter automaticamente conectado à sua aplicação Vodeclic.

Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/vodeclic-tutorial/tutorial_general_01.png
[2]: ./media/vodeclic-tutorial/tutorial_general_02.png
[3]: ./media/vodeclic-tutorial/tutorial_general_03.png
[4]: ./media/vodeclic-tutorial/tutorial_general_04.png

[100]: ./media/vodeclic-tutorial/tutorial_general_100.png

[200]: ./media/vodeclic-tutorial/tutorial_general_200.png
[201]: ./media/vodeclic-tutorial/tutorial_general_201.png
[202]: ./media/vodeclic-tutorial/tutorial_general_202.png
[203]: ./media/vodeclic-tutorial/tutorial_general_203.png

