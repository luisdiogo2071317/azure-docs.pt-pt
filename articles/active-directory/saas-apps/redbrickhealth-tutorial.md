---
title: 'Tutorial: Integração do Azure Active Directory com o estado de funcionamento RedBrick | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e o estado de funcionamento RedBrick.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 26290c65-9aa3-42ab-8ba5-901b14dc8e73
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/22/2018
ms.author: jeedes
ms.openlocfilehash: 11452dfb94a5a1c8cf4734b1ef21a44cccafbea1
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39430513"
---
# <a name="tutorial-azure-active-directory-integration-with-redbrick-health"></a>Tutorial: Integração do Azure Active Directory com o estado de funcionamento RedBrick

Neste tutorial, saiba como integrar o estado de funcionamento RedBrick com o Azure Active Directory (Azure AD).

Integrar o estado de funcionamento RedBrick no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao estado de funcionamento RedBrick.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para o estado de funcionamento RedBrick (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o estado de funcionamento RedBrick, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Estado de funcionamento RedBrick logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando o estado de funcionamento RedBrick da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-redbrick-health-from-the-gallery"></a>Adicionando o estado de funcionamento RedBrick da Galeria
Para configurar a integração do Estado de funcionamento RedBrick com o Azure AD, terá de adicionar RedBrick do Estado de funcionamento a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar RedBrick do Estado de funcionamento a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

1. Na caixa de pesquisa, escreva **estado de funcionamento RedBrick**, selecione **estado de funcionamento RedBrick** no painel de resultados, em seguida, clique em **adicionar** botão para adicionar a aplicação.

    ![Estado de funcionamento redBrick na lista de resultados](./media/redbrickhealth-tutorial/tutorial_redbrickhealth_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com o estado de funcionamento RedBrick com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no estado de funcionamento RedBrick a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no estado de funcionamento RedBrick deve ser estabelecido.

No estado de funcionamento RedBrick, atribuir o valor do **nome de utilizador** no Azure AD como o valor da **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com o estado de funcionamento RedBrick, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste de estado de funcionamento RedBrick](#create-a-redbrick-health-test-user)**  - para ter um equivalente da Eduarda Almeida no estado de funcionamento RedBrick que está ligado à representação de utilizador do Azure AD.
1. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo de estado de funcionamento RedBrick.

**Para configurar o Azure AD início de sessão único com o estado de funcionamento RedBrick, execute os seguintes passos:**

1. No portal do Azure, sobre o **RedBrick do Estado de funcionamento** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/redbrickhealth-tutorial/tutorial_redbrickhealth_samlbase.png)

1. Sobre o **RedBrick de estado de funcionamento de domínio e URLs** secção, execute os seguintes passos:

    ![RedBrick URLs de domínio de estado de funcionamento e único informações de início de sessão](./media/redbrickhealth-tutorial/tutorial_redbrickhealth_url.png)

    a. Na **identificador** caixa de texto, escreva um URL: `http://www.redbrickhealth.com`
    
    b. Na **URL de resposta** caixa de texto, escreva um URL: `https://sso-intg.redbrickhealth.com/sp/ACS.saml2`
    
    Para o ambiente de produção: `https://sso.redbrickhealth.com/sp/ACS.saml2`

    c. Clique em **Mostrar definições de URL avançadas**.
    
    ![RedBrick URLs de domínio de estado de funcionamento e único informações de início de sessão](./media/redbrickhealth-tutorial/tutorial_redbrickhealth_url1.png)

    d. Na **estado de reencaminhamento** caixa de texto, escreva um URL com o seguinte padrão: `https://api-sso2.redbricktest.com/identity/sso/nbound?target=https://vanity9-sso2.redbrickdev.com/portal&connection=<companyname>conn1`
    
    > [!NOTE] 
    > Valor de estado de reencaminhamento não é real. Atualize este valor com o estado real do reencaminhamento. Contacte [equipa de suporte do Estado de funcionamento RedBrick](https://home.redbrickhealth.com/contact/) para obter este valor.

1. O aplicativo de estado de funcionamento RedBrick espera que as asserções SAML num formato específico, o que requer a adição de mapeamentos de atributos personalizado à sua configuração de atributos de token SAML. Essas declarações são específico para o cliente e depende de seus requisitos. Afirmações opcionais seguintes são exemplo apenas que pode configurar para a sua aplicação. Pode gerir os valores destes atributos do **atributos de utilizador** secção na página de integração de aplicações.

    ![Configurar o início de sessão único](./media/redbrickhealth-tutorial/attribute.png)

1. No **atributos de utilizador** secção sobre o **início de sessão único** caixa de diálogo, configurar o atributo de token de SAML conforme mostrado na imagem acima e execute os seguintes passos:

    | Nome do Atributo | Valor do Atributo |
    | ---------------| ----------------|
    | Nome principal | ********** |
    | id de cliente | ********** |
    | id de participante | ********** |
    
    > [!NOTE]
    > Estes valores são para fins de referência apenas. Tem de definir os atributos de acordo com o requisito da sua organização. Entre em contato com [equipa de suporte do Estado de funcionamento RedBrick](https://home.redbrickhealth.com/contact/) para obter mais informações sobre as declarações necessárias.
    
    a. Clique em **adicionar atributo** para abrir o **adicionar atributo** caixa de diálogo.
    
    ![Configurar o início de sessão único](./media/redbrickhealth-tutorial/tutorial_attribute_04.png)
    
    ![Configurar o início de sessão único](./media/redbrickhealth-tutorial/tutorial_attribute_05.png)
    
    b. Na **nome** caixa de texto, escreva o nome de atributo apresentado para essa linha.
    
    c. Partir do **valor** lista, digite o valor de atributo apresentado para essa linha.

    d. Deixe o **espaço de nomes** em branco.
    
    e. Clique em **OK**.

1. Sobre o **certificado de assinatura SAML** secção, clique em **Certificate(Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![O link de download de certificado](./media/redbrickhealth-tutorial/tutorial_redbrickhealth_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/redbrickhealth-tutorial/tutorial_general_400.png)

1. Na **RedBrick configuração de estado de funcionamento** secção, clique em **configurar o estado de funcionamento RedBrick** para abrir **configurar início de sessão** janela. Cópia a **ID de entidade de SAML** partir o **secção de referência rápida.**

    ![Configuração de estado de funcionamento redBrick](./media/redbrickhealth-tutorial/tutorial_redbrickhealth_configure.png) 

1. Para configurar o início de sessão único em **estado de funcionamento RedBrick** lado, terá de enviar o transferido **Certificate(Base64)** e **ID de entidade de SAML** para [RedBrick do Estado de funcionamento equipa de suporte](https://home.redbrickhealth.com/contact/). Se definir esta definição para que a ligação de SAML SSO definidas corretamente em ambos os lados.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/redbrickhealth-tutorial/create_aaduser_01.png)

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/redbrickhealth-tutorial/create_aaduser_02.png)

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/redbrickhealth-tutorial/create_aaduser_03.png)

1. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/redbrickhealth-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
  
### <a name="create-a-redbrick-health-test-user"></a>Criar um utilizador de teste de estado de funcionamento RedBrick

Nesta secção, vai criar um usuário chamado Eduarda Almeida no estado de funcionamento RedBrick. Trabalhar com [equipa de suporte do Estado de funcionamento RedBrick](https://home.redbrickhealth.com/contact/) para adicionar os utilizadores na plataforma do Estado de funcionamento RedBrick. Os utilizadores tem de ser criados e ativados antes de utilizar o início de sessão único. 

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar do Azure-início de sessão único, concedendo acesso RedBrick o estado de funcionamento.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida RedBrick o estado de funcionamento, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **estado de funcionamento RedBrick**.

    ![A ligação de estado de funcionamento RedBrick na lista de aplicações](./media/redbrickhealth-tutorial/tutorial_redbrickhealth_app.png)  

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico de estado de funcionamento RedBrick no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo de estado de funcionamento RedBrick.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[1]: ./media/redbrickhealth-tutorial/tutorial_general_01.png
[2]: ./media/redbrickhealth-tutorial/tutorial_general_02.png
[3]: ./media/redbrickhealth-tutorial/tutorial_general_03.png
[4]: ./media/redbrickhealth-tutorial/tutorial_general_04.png

[100]: ./media/redbrickhealth-tutorial/tutorial_general_100.png

[200]: ./media/redbrickhealth-tutorial/tutorial_general_200.png
[201]: ./media/redbrickhealth-tutorial/tutorial_general_201.png
[202]: ./media/redbrickhealth-tutorial/tutorial_general_202.png
[203]: ./media/redbrickhealth-tutorial/tutorial_general_203.png

