---
title: 'Tutorial: Integração do Active Directory do Azure com a nuvem de Cisco | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e na Cloud da Cisco.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: db1cea1d-ff0a-4f0d-b5fd-50ca32702d56
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2018
ms.author: jeedes
ms.openlocfilehash: c5809426da95e2295c5bf318b6f17345002f536d
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55197947"
---
# <a name="tutorial-azure-active-directory-integration-with-cisco-cloud"></a>Tutorial: Integração do Active Directory do Azure com a nuvem de Cisco

Neste tutorial, saiba como integrar o Cisco Cloud no Azure Active Directory (Azure AD).

Integrar o Cisco Cloud no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso à nuvem da Cisco.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para a Cloud de Cisco (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com a nuvem da Cisco, terá dos seguintes itens:

- Uma subscrição do Azure
- Um nuvem de Cisco início de sessão único na subscrição ativado

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionar Cisco Cloud a partir da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-cisco-cloud-from-the-gallery"></a>Adicionar Cisco Cloud a partir da Galeria
Para configurar a integração da Cloud da Cisco para o Azure AD, terá de adicionar Cisco Cloud a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Cisco Cloud a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

1. Na caixa de pesquisa, escreva **Cisco Cloud**, selecione **Cisco Cloud** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Cisco nuvem na lista de resultados](./media/ciscocloud-tutorial/tutorial_ciscocloud_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com nuvem de Cisco, com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador equivalente na Cloud da Cisco para um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado na Cloud de Cisco deve ser estabelecido.

Para configurar e testar o Azure AD início de sessão único com a nuvem da Cisco, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste na Cloud da Cisco](#create-a-cisco-cloud-test-user)**  - para ter um equivalente da Eduarda Almeida na Cloud da Cisco que está ligado à representação de utilizador do Azure AD.
1. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único na sua aplicação na Cloud da Cisco.

**Para configurar o Azure AD início de sessão único com a nuvem da Cisco, execute os seguintes passos:**

1. No portal do Azure, sobre o **Cisco Cloud** página de integração de aplicativo, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.

    ![Caixa de diálogo de início de sessão único](./media/ciscocloud-tutorial/tutorial_ciscocloud_samlbase.png)

1. Sobre o **Cisco Cloud domínio e URLs** secção, execute os seguintes passos, se desejar configurar a aplicação no **IDP** iniciada pelo modo:

    ![Cisco Cloud domínio e URLs únicas início de sessão em informações](./media/ciscocloud-tutorial/tutorial_ciscocloud_url.png)

    a. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `<subdomain>.cisco.com`

    b. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://<subdomain>.cisco.com/sp/ACS.saml2`

1. Verifique **Mostrar definições de URL avançadas** e executar o passo seguinte, se desejar configurar a aplicação na **SP** iniciada pelo modo:

    ![Cisco Cloud domínio e URLs únicas início de sessão em informações](./media/ciscocloud-tutorial/tutorial_ciscocloud_url1.png)

    Na **URL de início de sessão** caixa de texto, escreva um URL: `https://<subdomain>.cloudapps.cisco.com`

    > [!NOTE]
    > Estes valores não são reais. Atualize estes valores com o identificador real, o URL de resposta e URL de início de sessão. Contacte [equipa de suporte de cliente do Cisco Cloud](mailto:cpr-ops@cisco.com) obter esses valores.

1. Aplicação da Cloud Cisco espera que as asserções SAML num formato específico. Configure as seguintes declarações para esta aplicação. Pode gerir os valores destes atributos do **atributos de utilizador** secção na página de integração de aplicações.
 Captura de ecrã seguinte mostra um exemplo sobre ele.

    ![Configurar o início de sessão único](./media/ciscocloud-tutorial/attribute.png)

1. Clique em **ver e editar todos os outros atributos de utilizador** caixa de seleção o **atributos de utilizador** secção para expandir os atributos. Execute os seguintes passos em cada um dos atributos apresentados-

    | Nome do Atributo | Valor do Atributo |
    | ---------------| ----------------|
    | País      |user.country |
    | Empresa      |user.companyname |

    a. Clique em **adicionar atributo** para abrir o **adicionar atributo** caixa de diálogo.

    ![Configurar o início de sessão único](./media/ciscocloud-tutorial/tutorial_attribute_04.png)

    ![Configurar o início de sessão único](./media/ciscocloud-tutorial/tutorial_attribute_05.png)

    b. Na **nome** caixa de texto, escreva o nome de atributo apresentado para essa linha.

    c. Partir do **valor** lista, digite o valor de atributo apresentado para essa linha.

    d. Deixe **espaço de nomes** valor como zero.

    e. Clique em **OK**.

1. Sobre o **certificado de assinatura SAML** secção, clique no botão de cópia para copiar **Url de metadados de Federação de aplicação** e cole-o no bloco de notas.

    ![O link de download de certificado](./media/ciscocloud-tutorial/tutorial_ciscocloud_certificate.png)

1. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/ciscocloud-tutorial/tutorial_general_400.png)

1. Para configurar o início de sessão único num **Cisco Cloud** lado, terá de enviar o **Url de metadados de Federação de aplicação** para [equipa de suporte do Cisco Cloud](mailto:cpr-ops@cisco.com). Se definir esta definição para que a ligação de SAML SSO definidas corretamente em ambos os lados.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/ciscocloud-tutorial/create_aaduser_01.png)

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/ciscocloud-tutorial/create_aaduser_02.png)

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/ciscocloud-tutorial/create_aaduser_03.png)

1. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/ciscocloud-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-cisco-cloud-test-user"></a>Criar um utilizador de teste na Cloud da Cisco

Nesta secção, vai criar um usuário chamado Eduarda Almeida na Cloud da Cisco. Trabalhar com [equipa de suporte do Cisco Cloud](mailto:cpr-ops@cisco.com) para adicionar os utilizadores na plataforma da Cloud da Cisco. Os utilizadores tem de ser criados e ativados antes de utilizar o início de sessão único

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso à nuvem da Cisco.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a Cloud da Cisco, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **Cisco Cloud**.

    ![A ligação de Cisco Cloud na lista de aplicações](./media/ciscocloud-tutorial/tutorial_ciscocloud_app.png)  

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar o início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico de Cisco Cloud no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação na Cloud da Cisco.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/ciscocloud-tutorial/tutorial_general_01.png
[2]: ./media/ciscocloud-tutorial/tutorial_general_02.png
[3]: ./media/ciscocloud-tutorial/tutorial_general_03.png
[4]: ./media/ciscocloud-tutorial/tutorial_general_04.png

[100]: ./media/ciscocloud-tutorial/tutorial_general_100.png

[200]: ./media/ciscocloud-tutorial/tutorial_general_200.png
[201]: ./media/ciscocloud-tutorial/tutorial_general_201.png
[202]: ./media/ciscocloud-tutorial/tutorial_general_202.png
[203]: ./media/ciscocloud-tutorial/tutorial_general_203.png

