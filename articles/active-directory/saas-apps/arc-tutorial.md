---
title: 'Tutorial: Integração do Azure Active Directory com a publicação de arco - SSO | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e publicação de arco - SSO.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: ae609583-f875-4cb8-b68e-1b0b7938e9a7
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2018
ms.author: jeedes
ms.openlocfilehash: 9decab5c35cda03e8532c48654203be7d95a5801
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37858848"
---
# <a name="tutorial-azure-active-directory-integration-with-arc-publishing---sso"></a>Tutorial: Integração do Azure Active Directory com a publicação de arco - SSO

Neste tutorial, saiba como integrar a publicação de arco - SSO com o Azure Active Directory (Azure AD).

Integrar o arco de publicação - SSO com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso à publicação de arco - SSO.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para publicação de arco - SSO (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com a publicação de arco - SSO, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Uma publicação de arco - SSO logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionar publicação de arco - SSO partir da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-arc-publishing---sso-from-the-gallery"></a>Adicionar publicação de arco - SSO partir da Galeria
Para configurar a integração de publicação de arco - SSO para o Azure AD, terá de adicionar a publicação de arco - SSO partir da galeria, à sua lista de aplicações de SaaS geridas.

**Para adicionar publicação de arco - SSO a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

1. Na caixa de pesquisa, escreva **publicação arco - SSO**, selecione **publicação arco - SSO** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Publicação de arco - SSO na lista de resultados](./media/arc-tutorial/tutorial_arc_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com o arco de publicação - SSO com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único trabalhar, o Azure AD precisa de saber o que o utilizador equivalente na publicação de arco - SSO for para um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado na publicação de arco - SSO tem de ser estabelecida.

Para configurar e testar o Azure AD início de sessão único com a publicação de arco - SSO, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar uma publicação de arco - utilizador de teste de SSO](#create-an-arc-publishing---sso-test-user)**  - para ter um equivalente da Eduarda Almeida na publicação de arco - SSO que está ligado à representação de utilizador do Azure AD.
1. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único no seu arco Publishing - aplicação SSO.

**Para configurar o Azure AD início de sessão único com a publicação de arco - SSO, execute os seguintes passos:**

1. No portal do Azure, sobre o **publicação arco - SSO** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/arc-tutorial/tutorial_arc_samlbase.png)

1. Sobre o **arco publicação - SSO domínio e URLs** secção, execute os seguintes passos, se desejar configurar a aplicação no **IDP** iniciada pelo modo:

    ![Publicação de arco - SSO domínio e URLs únicas início de sessão em informações](./media/arc-tutorial/tutorial_arc_url.png)

    1. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://www.okta.com/saml2/service-provider/<Unique ID>`

    1. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://arcpublishing-<Customer>.okta.com/sso/saml2/<Unique ID>`

1. Verifique **Mostrar definições de URL avançadas** e executar o passo seguinte, se desejar configurar a aplicação na **SP** iniciada pelo modo:

    ![Publicação de arco - SSO domínio e URLs únicas início de sessão em informações](./media/arc-tutorial/tutorial_arc_url1.png)

    Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://arcpublishing-<Customer>.okta.com/sso/saml2/<Unique ID>`
     
    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o identificador de real, a URL de resposta e o URL de início de sessão. Contacte [publicação arco - equipa de suporte de cliente de SSO](mailto:inf@washpost.com) obter esses valores. 

1. Arco de publicação - aplicação SSO espera que as asserções SAML num formato específico. Configure as seguintes declarações para esta aplicação. Pode gerir os valores destes atributos do **atributos de utilizador** secção na página de integração de aplicações. Captura de ecrã seguinte mostra um exemplo disso.
    
    ![Configurar o início de sessão único](./media/arc-tutorial/tutorial_arc_attribute.png)

1. No **atributos de utilizador** secção sobre o **início de sessão único** caixa de diálogo, configurar o atributo de token de SAML conforme mostrado na imagem acima e execute os seguintes passos:
    
    | Nome do Atributo | Valor do Atributo |
    | ---------------| --------------- |    
    | firstName | User.givenName |
    | Apelido | User.Surname |
    | e-mail | User.Mail |
    | grupos | user.assignedroles |

    1. Clique em **adicionar atributo** para abrir o **adicionar atributo** caixa de diálogo.

     ![Configurar o início de sessão único](./media/arc-tutorial/tutorial_attribute_04.png)

     ![Configurar o início de sessão único](./media/arc-tutorial/tutorial_attribute_05.png)
    
    1. Na **nome** caixa de texto, escreva o nome de atributo apresentado para essa linha.
    
    1. Partir do **valor** lista, digite o valor de atributo apresentado para essa linha.

    1. Deixe o **espaço de nomes** em branco.
    
    1. Clique em **Ok**

    > [!NOTE]
    > Aqui o **grupos** atributo está mapeado com **user.assignedroles**. Estes são criadas no Azure AD para mapear os nomes de grupo na aplicação de funções personalizadas. Pode obter mais orientações [aqui](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-enterprise-app-role-management) sobre como criar funções personalizadas no Azure AD. 

1. Sobre o **certificado de assinatura SAML** secção, clique em **certificado (Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![O link de download de certificado](./media/arc-tutorial/tutorial_arc_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/arc-tutorial/tutorial_general_400.png)
    
1. Sobre o **publicação arco - configuração de SSO** secção, clique em **configurar a publicação de arco - SSO** para abrir **configurar início de sessão** janela. Cópia a **URL de fim de sessão, o ID de entidade de SAML e o SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Publicação de arco - configuração de SSO](./media/arc-tutorial/tutorial_arc_configure.png) 

1. Para configurar o início de sessão único num **publicação arco - SSO** lado, terá de enviar o transferido **certificado (Base64), o URL de fim de sessão, o ID de entidade de SAML e o SAML único início de sessão no URL do serviço** para [em arco Publicação - equipa de suporte SSO](mailto:inf@washpost.com). Se definir esta definição para que a ligação de SAML SSO definidas corretamente em ambos os lados.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/arc-tutorial/create_aaduser_01.png)

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/arc-tutorial/create_aaduser_02.png)

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/arc-tutorial/create_aaduser_03.png)

1. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/arc-tutorial/create_aaduser_04.png)

    1. Na **Name** , escreva **BrittaSimon**.

    1. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    1. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    1. Clique em **Criar**.
 
### <a name="create-an-arc-publishing---sso-test-user"></a>Criar um arco publicação - utilizador de teste SSO

O objetivo desta secção é criar um utilizador chamado Eduarda Almeida na publicação de arco - SSO. Arco de publicação - SSO suporta o aprovisionamento just-in-time, que está por predefinição, ativada. Não existe nenhum item de ação para nesta secção. Um novo utilizador é criado durante uma tentativa de aceder a publicação de arco - SSO se não existir ainda.

>[!Note]
>Se precisar de criar manualmente um utilizador, contacte [publicação arco - equipa de suporte SSO](mailto:inf@washpost.com).

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida a utilizar do Azure-início de sessão único, conceder acesso à publicação de arco - SSO.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a publicação de arco - SSO, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **publicação arco - SSO**.

    ![A publicação de arco - ligação SSO na lista de aplicações](./media/arc-tutorial/tutorial_arc_app.png)  

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Ao clicar o arco publicação - mosaico SSO no painel de acesso, deve obter automaticamente com sessão iniciada para o arco publicação - aplicação SSO.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/arc-tutorial/tutorial_general_01.png
[2]: ./media/arc-tutorial/tutorial_general_02.png
[3]: ./media/arc-tutorial/tutorial_general_03.png
[4]: ./media/arc-tutorial/tutorial_general_04.png

[100]: ./media/arc-tutorial/tutorial_general_100.png

[200]: ./media/arc-tutorial/tutorial_general_200.png
[201]: ./media/arc-tutorial/tutorial_general_201.png
[202]: ./media/arc-tutorial/tutorial_general_202.png
[203]: ./media/arc-tutorial/tutorial_general_203.png

