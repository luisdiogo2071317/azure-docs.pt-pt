---
title: 'Tutorial: Integração do Azure Active Directory com o reencaminhamento de imagem | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e o reencaminhamento de imagem.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 65bb5990-07ef-4244-9f41-cd28fc2cb5a2
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 326f12ff3923855b09462d4909d37638d4010b48
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39435014"
---
# <a name="tutorial-azure-active-directory-integration-with-image-relay"></a>Tutorial: Integração do Azure Active Directory com o reencaminhamento de imagem

Neste tutorial, saiba como integrar o reencaminhamento de imagem com o Azure Active Directory (Azure AD).

Integrar o reencaminhamento de imagem no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao reencaminhamento de imagem
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para o reencaminhamento de imagem (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o reencaminhamento de imagem, precisa do seguinte:

- Uma subscrição do Azure AD
- Um reencaminhamento de imagem de logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando o reencaminhamento de imagem da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-image-relay-from-the-gallery"></a>Adicionando o reencaminhamento de imagem da Galeria
Para configurar a integração do reencaminhamento de imagem com o Azure AD, terá de adicionar o reencaminhamento de imagem a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar o reencaminhamento de imagem da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

1. Na caixa de pesquisa, escreva **reencaminhamento de imagem**.

    ![Criar um utilizador de teste do Azure AD](./media/imagerelay-tutorial/tutorial_imagerelay_search.png)

1. No painel de resultados, selecione **reencaminhamento de imagem**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/imagerelay-tutorial/tutorial_imagerelay_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com o reencaminhamento de imagem com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no reencaminhamento de imagem a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no reencaminhamento de imagem deve ser estabelecido.

Na imagem de reencaminhamento, atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com o reencaminhamento de imagem, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste de reencaminhamento de imagem](#creating-an-image-relay-test-user)**  - para ter um equivalente da Eduarda Almeida no reencaminhamento de imagem que está ligado à representação de utilizador do Azure AD.
1. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo de reencaminhamento de imagem.

**Para configurar o Azure AD início de sessão único com o reencaminhamento de imagem, execute os seguintes passos:**

1. No portal do Azure, sobre o **reencaminhamento de imagem** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/imagerelay-tutorial/tutorial_imagerelay_samlbase.png)

1. Sobre o **domínio de reencaminhamento de imagem e URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/imagerelay-tutorial/tutorial_imagerelay_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<companyname>.imagerelay.com/`

    b. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://<companyname>.imagerelay.com/sso/metadata`

    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o URL de início de sessão e o identificador real. Contacte [equipa de suporte de cliente de reencaminhamento de imagem](http://support.imagerelay.com/) obter esses valores. 
 


1. Sobre o **certificado de assinatura SAML** secção, clique em **Certificate(Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![Configurar o início de sessão único](./media/imagerelay-tutorial/tutorial_imagerelay_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/imagerelay-tutorial/tutorial_general_400.png)

1. Sobre o **configuração de reencaminhamento de imagem** secção, clique em **configurar o reencaminhamento de imagem** para abrir **configurar início de sessão** janela. Cópia a **URL do serviço de fim de sessão e SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configurar o início de sessão único](./media/imagerelay-tutorial/tutorial_imagerelay_configure.png) 

1. Na outra janela do browser, inicie sessão no site da sua empresa reencaminhamento de imagem como um administrador.

1. Na barra de ferramentas na parte superior, clique nas **utilizadores e permissões** carga de trabalho.
   
    ![Configurar o início de sessão único](./media/imagerelay-tutorial/tutorial_imagerelay_06.png) 

1. Clique em **criar nova permissão**.
   
    ![Configurar o início de sessão único](./media/imagerelay-tutorial/tutorial_imagerelay_08.png)

1. Na **início de sessão definições único** carga de trabalho, selecione a **este grupo pode apenas iniciar sessão através do início de sessão único** caixa de verificação e, em seguida, clique em **guardar**.
   
    ![Configurar o início de sessão único](./media/imagerelay-tutorial/tutorial_imagerelay_09.png) 

1. Aceda a **definições da conta**.
   
    ![Configurar o início de sessão único](./media/imagerelay-tutorial/tutorial_imagerelay_10.png) 

1. Vá para o **início de sessão único em definições** carga de trabalho.
    
     ![Configurar o início de sessão único](./media/imagerelay-tutorial/tutorial_imagerelay_11.png)

1. Sobre o **definições de SAML** caixa de diálogo, execute os seguintes passos:
    
    ![Configurar o início de sessão único](./media/imagerelay-tutorial/tutorial_imagerelay_12.png)
    
    a. Na **URL de início de sessão** caixa de texto, cole o valor de **único URL de início de sessão no serviço** que copiou do portal do Azure.

    b. Na **URL de fim de sessão** caixa de texto, cole o valor de **URL de serviço de fim de sessão único** que copiou do portal do Azure.

    c. Como **formato do nome do Id**, selecione **urn: oasis: nomes: tc: SAML:1.1:nameid-formato: endereço de correio eletrónico**.

    d. Como **opções de vinculação de mensagens em fila para pedidos de fornecedor de serviços (imagem de reencaminhamento)**, selecione **POST enlace**.

    e. Sob **x.509 certificado**, clique em **atualizar certificado**.

    ![Configurar o início de sessão único](./media/imagerelay-tutorial/tutorial_imagerelay_17.png)

    f. Abra o certificado transferido no bloco de notas, copie o conteúdo e, em seguida, cole-o na caixa de texto de certificado x.509.

    ![Configurar o início de sessão único](./media/imagerelay-tutorial/tutorial_imagerelay_18.png)

    g. Na **aprovisionamento de utilizadores ativaram** secção, selecione a **ativar o aprovisionamento de utilizadores ativaram**.

    ![Configurar o início de sessão único](./media/imagerelay-tutorial/tutorial_imagerelay_19.png)

    h. Selecione o grupo de permissões (por exemplo, **básicos de SSO**) que tem permissão para iniciar sessão apenas por meio de início de sessão único.

    ![Configurar o início de sessão único](./media/imagerelay-tutorial/tutorial_imagerelay_20.png)

    i. Clique em **Guardar**.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)


### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/imagerelay-tutorial/create_aaduser_01.png) 

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/imagerelay-tutorial/create_aaduser_02.png) 

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/imagerelay-tutorial/create_aaduser_03.png) 

1. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/imagerelay-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-an-image-relay-test-user"></a>Criar um utilizador de teste de reencaminhamento de imagem

O objetivo desta secção é criar um usuário chamado Eduarda Almeida no reencaminhamento de imagem.

**Para criar um usuário chamado Eduarda Almeida no reencaminhamento de imagem, execute os seguintes passos:**

1. Início de sessão no site da sua empresa reencaminhamento de imagem como administrador.

1. Aceda a **utilizadores e permissões** e selecione **Create SSO User**.
   
    ![Configurar o início de sessão único](./media/imagerelay-tutorial/tutorial_imagerelay_21.png) 

1. Introduza o **E-Mail**, **FirstName**, **Apelido**, e **empresa** do utilizador que pretende aprovisionar e selecione o grupo de permissões (para exemplo básicos de SSO) que é o grupo que pode iniciar sessão apenas por meio de início de sessão único.
   
    ![Configurar o início de sessão único](./media/imagerelay-tutorial/tutorial_imagerelay_22.png) 

1. Clique em **Criar**.

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para o reencaminhamento de imagem.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida a imagem de reencaminhamento, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **reencaminhamento de imagem**.

    ![Configurar o início de sessão único](./media/imagerelay-tutorial/tutorial_imagerelay_app.png) 

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

O objetivo desta secção é testar a configuração do Azure AD única início de sessão com o painel de acesso.    

Quando clica no mosaico de reencaminhamento de imagem no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo de reencaminhamento de imagem.


## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/imagerelay-tutorial/tutorial_general_01.png
[2]: ./media/imagerelay-tutorial/tutorial_general_02.png
[3]: ./media/imagerelay-tutorial/tutorial_general_03.png
[4]: ./media/imagerelay-tutorial/tutorial_general_04.png


[100]: ./media/imagerelay-tutorial/tutorial_general_100.png

[200]: ./media/imagerelay-tutorial/tutorial_general_200.png
[201]: ./media/imagerelay-tutorial/tutorial_general_201.png
[202]: ./media/imagerelay-tutorial/tutorial_general_202.png
[203]: ./media/imagerelay-tutorial/tutorial_general_203.png

