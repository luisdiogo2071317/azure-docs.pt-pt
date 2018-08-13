---
title: 'Tutorial: Integração do Azure Active Directory com Ceridian Dayforce HCM | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Ceridian Dayforce HCM.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 7adf1eb3-d063-45d6-96a8-fd53b329b3f3
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: 6577955b275adfda3f0cfafe99a8f95efd16403c
ms.sourcegitcommit: d16b7d22dddef6da8b6cfdf412b1a668ab436c1f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/08/2018
ms.locfileid: "39714585"
---
# <a name="tutorial-azure-active-directory-integration-with-ceridian-dayforce-hcm"></a>Tutorial: Integração do Azure Active Directory com Ceridian Dayforce HCM

Neste tutorial, saiba como integrar Ceridian Dayforce HCM com o Azure Active Directory (Azure AD).

Integrar Ceridian Dayforce HCM no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Ceridian Dayforce HCM.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Ceridian Dayforce HCM (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Ceridian Dayforce HCM, terá dos seguintes itens:

- Uma subscrição do Azure
- Um Ceridian Dayforce HCM início de sessão único na subscrição ativado

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Ceridian Dayforce HCM da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-ceridian-dayforce-hcm-from-the-gallery"></a>Adicionando Ceridian Dayforce HCM da Galeria
Para configurar a integração do Ceridian Dayforce HCM com o Azure AD, terá de adicionar Ceridian Dayforce HCM a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Ceridian Dayforce HCM a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

1. Na caixa de pesquisa, escreva **Ceridian Dayforce HCM**, selecione **Ceridian Dayforce HCM** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Ceridian Dayforce HCM na lista de resultados](./media/ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com Ceridian Dayforce HCM com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Ceridian Dayforce HCM a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Ceridian Dayforce HCM deve ser estabelecido.

Ceridian Dayforce HCM, atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com Ceridian Dayforce HCM, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on) ** - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user) ** - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste Ceridian Dayforce HCM](#create-a-ceridian-dayforce-hcm-test-user) ** - para ter um equivalente da Eduarda Almeida na Ceridian Dayforce HCM que está ligado à representação de utilizador do Azure AD.
1. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user) ** - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Testar início de sessão único](#test-single-sign-on) ** - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo Ceridian Dayforce HCM.

**Para configurar o Azure AD início de sessão único com Ceridian Dayforce HCM, execute os seguintes passos:**

1. No portal do Azure, sobre o **Ceridian Dayforce HCM** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_samlbase.png)

1. Sobre o **Ceridian Dayforce HCM domínio e URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_url.png)
    
    a. Na **URL de início de sessão** caixa de texto, escreva o URL utilizado pelos seus utilizadores para início de sessão na sua aplicação Ceridian Dayforce HCM.
    
    | Ambiente | do IdP |
    | :-- | :-- |
    | Para produção | `https://sso.dayforcehcm.com/<DayforcehcmNamespace>` |
    | Para teste | `https://ssotest.dayforcehcm.com/<DayforcehcmNamespace>` |
    
    b. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão:
    
    | Ambiente | do IdP |
    | :-- | :-- |
    | Para produção | `https://ncpingfederate.dayforcehcm.com/sp` |
    | Para teste | `https://fs-test.dayforcehcm.com/sp` |
    
    c. Na **URL de resposta** caixa de texto, escreva o URL é utilizado pelo Azure AD para publicar a resposta.
    
    | Ambiente | do IdP |
    | :-- | :-- |
    | Para produção | `https://ncpingfederate.dayforcehcm.com/sp/ACS.saml2` |
    | Para teste | `https://fs-test.dayforcehcm.com/sp/ACS.saml2` |
    
    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o identificador real, o URL de resposta e URL de início de sessão. Contacte [equipa de suporte de cliente de HCM Ceridian Dayforce](https://www.ceridian.com/support) obter esses valores.

1. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![O link de download de certificado](./media/ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_certificate.png) 

1. Seu aplicativo Ceridian Dayforce HCM espera que as asserções SAML num formato específico. Trabalhar com [equipa de suporte de Ceridian Dayforce HCM](https://www.ceridian.com/support) pela primeira vez para identificar o identificador de utilizador correto. A Microsoft recomenda utilizar o **"name"** atributo como identificador de utilizador. Pode gerir os valores destes atributos do **atributos de utilizador** secção na página de integração de aplicações. Captura de ecrã seguinte mostra um exemplo disso.  

    ![Configurar o início de sessão único](./media/ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_07.png)

1. No **atributos de utilizador** secção sobre o **início de sessão único** caixa de diálogo, configurar o atributo de token de SAML conforme mostrado na imagem acima e execute os seguintes passos:
    
    | Nome do Atributo  | Valor do Atributo |
    | --------------- | -------------------- |    
    | nome  | User.extensionattribute2 |    

    a. Clique em **adicionar atributo** para abrir o **adicionar atributo** caixa de diálogo.

    ![Configurar o início de sessão único](./media/ceridiandayforcehcm-tutorial/tutorial_attribute_04.png)

    ![Configurar o início de sessão único](./media/ceridiandayforcehcm-tutorial/tutorial_attribute_05.png)
    
    b. Na **nome** caixa de texto, escreva o nome de atributo apresentado para essa linha.

    c. Na **valor** , selecione o atributo de utilizador que pretende utilizar para a sua implementação.
    Por exemplo, se pretender utilizar o campo IDdeEmpregado como utilizador exclusivo identificador e armazenou o valor do atributo no ExtensionAttribute2, em seguida, selecione **user.extensionattribute2**.
    
    d. Clique em **OK**.

1. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/ceridiandayforcehcm-tutorial/tutorial_general_400.png)
    
1. Sobre o **Ceridian Dayforce HCM configuração** secção, clique em **configurar o Ceridian Dayforce HCM** para abrir **configurar início de sessão** janela. Cópia a **URL de fim de sessão, o ID de entidade de SAML e o SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configuração de HCM Ceridian Dayforce](./media/ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_configure.png) 

1. Para configurar o início de sessão único num **Ceridian Dayforce HCM** lado, terá de enviar o transferido **XML de metadados** e **URL de fim de sessão, o ID de entidade de SAML e o SAML único início de sessão no URL do serviço** para [equipa de suporte de Ceridian Dayforce HCM](https://www.ceridian.com/support).

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a ** Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/ceridiandayforcehcm-tutorial/create_aaduser_01.png)

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/ceridiandayforcehcm-tutorial/create_aaduser_02.png)

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/ceridiandayforcehcm-tutorial/create_aaduser_03.png)

1. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/ceridiandayforcehcm-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-ceridian-dayforce-hcm-test-user"></a>Criar um utilizador de teste Ceridian Dayforce HCM

O objetivo desta secção é criar um usuário chamado Eduarda Almeida no Ceridian Dayforce HCM. Trabalhar com o [equipa de suporte de Ceridian Dayforce HCM](https://www.ceridian.com/support) para obter utilizadores adicionados na aplicação Ceridian Dayforce HCM. 

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único, concedendo acesso para Ceridian Dayforce HCM.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida a Ceridian Dayforce HCM, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **Ceridian Dayforce HCM**.

    ![Configurar o início de sessão único](./media/ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_app.png) 

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único, concedendo acesso para Ceridian Dayforce HCM.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a Ceridian Dayforce HCM, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **Ceridian Dayforce HCM**.

    ![A ligação de Ceridian Dayforce HCM na lista de aplicações](./media/ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_app.png)  

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

O objetivo desta secção é testar a configuração do Azure AD única início de sessão com o painel de acesso.  
Quando clica no mosaico Ceridian Dayforce HCM no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo Ceridian Dayforce HCM. 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[1]: ./media/ceridiandayforcehcm-tutorial/tutorial_general_01.png
[2]: ./media/ceridiandayforcehcm-tutorial/tutorial_general_02.png
[3]: ./media/ceridiandayforcehcm-tutorial/tutorial_general_03.png
[4]: ./media/ceridiandayforcehcm-tutorial/tutorial_general_04.png

[100]: ./media/ceridiandayforcehcm-tutorial/tutorial_general_100.png

[200]: ./media/ceridiandayforcehcm-tutorial/tutorial_general_200.png
[201]: ./media/ceridiandayforcehcm-tutorial/tutorial_general_201.png
[202]: ./media/ceridiandayforcehcm-tutorial/tutorial_general_202.png
[203]: ./media/ceridiandayforcehcm-tutorial/tutorial_general_203.png

