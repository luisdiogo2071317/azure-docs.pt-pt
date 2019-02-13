---
title: 'Tutorial: Integração do Active Directory do Azure com EBSCO | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e EBSCO.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 144f7f65-69e9-4016-a151-fe1104fd6ba8
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: c0973001cda4a817ab464f825ad50deb52019f65
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2019
ms.locfileid: "56180662"
---
# <a name="tutorial-azure-active-directory-integration-with-ebsco"></a>Tutorial: Integração do Active Directory do Azure com EBSCO

Neste tutorial, saiba como integrar EBSCO com o Azure Active Directory (Azure AD).

Integrar EBSCO no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao EBSCO.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para EBSCO (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com EBSCO, terá dos seguintes itens:

- Uma subscrição do Azure
- Um EBSCO início de sessão único na subscrição ativado

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando EBSCO da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-ebsco-from-the-gallery"></a>Adicionando EBSCO da Galeria
Para configurar a integração do EBSCO com o Azure AD, terá de adicionar EBSCO a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar EBSCO a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

1. Na caixa de pesquisa, escreva **EBSCO**, selecione **EBSCO** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![EBSCO na lista de resultados](./media/ebsco-tutorial/tutorial_ebsco_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com EBSCO com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no EBSCO a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no EBSCO deve ser estabelecido.

Para configurar e testar o Azure AD início de sessão único com EBSCO, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste EBSCO](#create-an-ebsco-test-user)**  -é possível automatizar EBSCOhost aprovisionamento/personalização de utilizador. EBSCO suporta Just-In-Time aprovisionamento de utilizadores.
1. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo EBSCO.

**Para configurar o Azure AD início de sessão único com EBSCO, execute os seguintes passos:**

1. No portal do Azure, sobre o **EBSCO** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/ebsco-tutorial/tutorial_ebsco_samlbase.png)

1. Sobre o **EBSCO domínio e URLs** secção, execute os seguintes passos, se desejar configurar a aplicação no **IDP** iniciada pelo modo:

    ![EBSCO domínio e URLs únicas início de sessão em informações](./media/ebsco-tutorial/tutorial_ebsco_url.png)

    Na **identificador** caixa de texto, escreva um URL: `pingsso.ebscohost.com`

1. Verifique **Mostrar definições de URL avançadas** e executar o passo seguinte, se desejar configurar a aplicação na **SP** iniciada pelo modo:

    ![EBSCO domínio e URLs únicas início de sessão em informações](./media/ebsco-tutorial/tutorial_ebsco_url1.png)

    Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `http://search.ebscohost.com/login.aspx?authtype=sso&custid=<unique EBSCO customer ID>&profile=<profile ID>`
     
    > [!NOTE] 
    > O valor de URL de início de sessão não é real. Atualize o valor com o URL de início de sessão real. Contacte [equipa de suporte de cliente EBSCO](mailto:sso@ebsco.com) para obter o valor. 

    o **elementos exclusivos:**  

    s **Custid** = o ID de cliente EBSCO Enter exclusivo 

    s **perfil** = clientes podem personalizar a ligação para direcionar os utilizadores a um perfil específico (consoante o que eles comprar de EBSCO). Eles podem digitar um ID de perfil específico. Os IDs principais são eds (serviço de deteção de EBSCO) e ehost (EBSOCOhost bases de dados). Instruções para a mesma recebem [aqui](https://help.ebsco.com/interfaces/EBSCOhost/EBSCOhost_FAQs/How_do_I_set_up_direct_links_to_EBSCOhost_profiles_and_or_databases#profile).

1. Aplicação de EBSCO espera que as asserções SAML num formato específico. Configure as seguintes declarações para esta aplicação. Pode gerir os valores destes atributos da "**atributos de utilizador**" secção na página de integração de aplicações. Captura de ecrã seguinte mostra um exemplo disso.
    
    ![Configurar o início de sessão único](./media/ebsco-tutorial/tutorial_ebsco_attribute.png)

    > [!Note]
    > O **name** atributo é obrigatório e está mapeado com **identificador de utilizador** no aplicativo EBSCO. Este valor é adicionado por predefinição pelo que não precisa adicionar isso manualmente.
    
1. No **atributos de utilizador** secção sobre o **início de sessão único** caixa de diálogo, configurar o atributo de token de SAML conforme mostrado na imagem acima e execute os seguintes passos:
    
    | Nome do Atributo | Valor do Atributo |
    | ---------------| --------------- |    
    | FirstName   | user.givenname |
    | LastName   | user.surname |
    | Email   | user.mail |

    a. Clique em **adicionar atributo** para abrir o **adicionar atributo** caixa de diálogo.

    ![Configurar o início de sessão único](./media/ebsco-tutorial/tutorial_officespace_04.png)

    ![Configurar o início de sessão único](./media/ebsco-tutorial/tutorial_attribute_05.png)
    
    b. Na **nome** caixa de texto, escreva o nome de atributo apresentado para essa linha.
    
    c. Partir do **valor** lista, digite o valor de atributo apresentado para essa linha.
    
    d. Clique em **Ok**

1. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![O link de download de certificado](./media/ebsco-tutorial/tutorial_ebsco_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/ebsco-tutorial/tutorial_general_400.png)
    
1. Para configurar o início de sessão único num **EBSCO** lado, terá de enviar o transferido **XML de metadados** para [equipa de suporte de EBSCO](mailto:sso@ebsco.com). Se definir esta definição para que a ligação de SAML SSO definidas corretamente em ambos os lados.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [Documentação do Azure AD incorporado]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/ebsco-tutorial/create_aaduser_01.png)

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/ebsco-tutorial/create_aaduser_02.png)

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/ebsco-tutorial/create_aaduser_03.png)

1. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/ebsco-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-an-ebsco-test-user"></a>Criar um utilizador de teste EBSCO

No caso de EBSCO, aprovisionamento de utilizadores é automática.

**Para Aprovisionar uma conta de utilizador, execute os seguintes passos:**

Do Azure AD passa os dados necessários para a aplicação de EBSCO. Aprovisionamento de utilizadores do EBSCO pode ser automática ou adquirir uma forma de uso individual. Depende se o cliente possui muita pré-existente EBSCOhost contas com configurações pessoais guardadas. O mesmo podem ser discutido com o [equipa de suporte de EBSCO](mailto:sso@ebsco.com) durante a implementação. De qualquer forma, o cliente não tem que criar as contas de EBSCOhost antes de testar.

   >[!Note]
   >Pode automatizar EBSCOhost aprovisionamento/personalização de utilizador. Contacte [equipa de suporte de EBSCO](mailto:sso@ebsco.com) sobre Just-In-Time aprovisionamento de utilizadores. 
 
### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para EBSCO.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a EBSCO, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **EBSCO**.

    ![A ligação EBSCO na lista de aplicações](./media/ebsco-tutorial/tutorial_ebsco_app.png)  

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar o início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

1. Quando clica no mosaico EBSCO no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo EBSCO.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md).

1. Assim que faça logon para a aplicação, clique nas **iniciar sessão** botão no canto superior direito.

    ![O início de sessão da EBSCO na lista de aplicações](./media/ebsco-tutorial/tutorial_ebsco_signin.png)
 
1. Receberá uma única linha de comandos para emparelhar o início de sessão institucional/SAML com uma **ligar a sua conta existente do MyEBSCOhost à sua conta da instituição agora** ou **criar uma nova conta de MyEBSCOhost e ligá-lo para seu conta da instituição**. A conta é utilizada para personalização no aplicativo EBSCOhost. Selecione a opção **criar uma nova conta** e verá que o formulário para personalização é previamente foi concluído com os valores da resposta saml, conforme mostrado na captura de ecrã abaixo. Clique em **'Continuar'** para guardar esta seleção.
    
     ![O utilizador EBSCO na lista de aplicações](./media/ebsco-tutorial/tutorial_ebsco_user.png)

1. Depois de concluir a configuração acima, limpe a cache de cookies/e início de sessão novamente. Não precisará manualmente início de sessão novamente e as definições de personalização podem ser memorizadas

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/ebsco-tutorial/tutorial_general_01.png
[2]: ./media/ebsco-tutorial/tutorial_general_02.png
[3]: ./media/ebsco-tutorial/tutorial_general_03.png
[4]: ./media/ebsco-tutorial/tutorial_general_04.png

[100]: ./media/ebsco-tutorial/tutorial_general_100.png

[200]: ./media/ebsco-tutorial/tutorial_general_200.png
[201]: ./media/ebsco-tutorial/tutorial_general_201.png
[202]: ./media/ebsco-tutorial/tutorial_general_202.png
[203]: ./media/ebsco-tutorial/tutorial_general_203.png

