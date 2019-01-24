---
title: 'Tutorial: Integração do Active Directory do Azure com Graphite alojado | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Graphite alojado.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: a1ac4d7f-d079-4f3c-b6da-0f520d427ceb
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 8aefa0580e6a9a1e446dd4861a5627ea2a4d36ce
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/23/2019
ms.locfileid: "54811881"
---
# <a name="tutorial-azure-active-directory-integration-with-hosted-graphite"></a>Tutorial: Integração do Active Directory do Azure com Graphite alojado

Neste tutorial, saiba como integrar Graphite alojado no Azure Active Directory (Azure AD).

Integrar Graphite alojado no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Graphite alojado
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Graphite alojado (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Graphite alojados, terá dos seguintes itens:

- Uma subscrição do Azure
- Um alojado Graphite logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Graphite hospedado da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-hosted-graphite-from-the-gallery"></a>Adicionando Graphite hospedado da Galeria
Para configurar a integração do Graphite alojado no Azure AD, terá de adicionar Graphite alojada a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Graphite hospedado da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

1. Na caixa de pesquisa, escreva **alojada Graphite**.

    ![Criar um utilizador de teste do Azure AD](./media/hostedgraphite-tutorial/tutorial_hostedgraphite_search.png)

1. No painel de resultados, selecione **alojada Graphite**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/hostedgraphite-tutorial/tutorial_hostedgraphite_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com Graphite alojado com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Graphite alojada a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Graphite alojado tem de ser estabelecida.

Graphite alojado, atribua o valor do **nome de utilizador** no Azure AD como o valor da **nome de utilizador** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com Graphite alojados, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste alojado Graphite](#creating-a-hosted-graphite-test-user)**  - para ter um equivalente da Eduarda Almeida na Graphite hospedado, o que está ligado à representação de utilizador do Azure AD.
1. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo hospedado Graphite.

**Para configurar o Azure AD início de sessão único com Graphite alojado, execute os seguintes passos:**

1. No portal do Azure, no **alojada Graphite** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/hostedgraphite-tutorial/tutorial_hostedgraphite_samlbase.png)

1. Na **alojado o Graphite domínio e URLs** secção, se desejar configurar a aplicação no **IDP iniciada pelo modo**, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/hostedgraphite-tutorial/tutorial_hostedgraphite_url.png)

    a. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://www.hostedgraphite.com/metadata/<user id>`

    b. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://www.hostedgraphite.com/complete/saml/<user id>`

1. Na **alojado o Graphite domínio e URLs** secção, se desejar configurar a aplicação no **SP iniciada pelo modo**, execute os seguintes passos:
   
    ![Configurar o início de sessão único](./media/hostedgraphite-tutorial/tutorial_hostedgraphite_10.png)
  
    a. Clique nas **Mostrar definições de URL avançadas** opção

    b. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://www.hostedgraphite.com/login/saml/<user id>/`   

    > [!NOTE] 
    > Tenha em atenção que estes não são os valores reais. Terá de atualizar estes valores com o identificador real, o URL de resposta e URL de início de sessão. Para obter estes valores, pode ir para acesso -> configuração SAML em seu lado do aplicativo ou o contato [equipa de suporte de alojado Graphite](mailto:help@hostedgraphite.com).
    >
 
1. Sobre o **certificado de assinatura SAML** secção, clique em **Certificate(Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![Configurar o início de sessão único](./media/hostedgraphite-tutorial/tutorial_hostedgraphite_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/hostedgraphite-tutorial/tutorial_general_400.png)

1. Na **alojada configuração de Graphite** secção, clique em **configurar Graphite alojado** para abrir **configurar início de sessão** janela. Cópia a **ID de entidade de SAML e SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configurar o início de sessão único](./media/hostedgraphite-tutorial/tutorial_hostedgraphite_configure.png) 

1. Início de sessão no seu inquilino Graphite hospedado como um administrador.

1. Vá para o **página de configuração de SAML** na barra lateral (**acesso -> Configuração de SAML**).
   
    ![Configure o lado de início de sessão na aplicação única](./media/hostedgraphite-tutorial/tutorial_hostedgraphite_000.png)

1. Confirme estes URls corresponde à sua configuração feita no **alojado o Graphite domínio e URLs** seção do portal do Azure.
   
    ![Configure o lado de início de sessão na aplicação única](./media/hostedgraphite-tutorial/tutorial_hostedgraphite_001.png)

1. Na **entidade ou ID do emissor** e **URL de início de sessão de SSO** caixas de texto, cole o valor de **ID de entidade de SAML** e **SAML único início de sessão no URL do serviço** que copiou do portal do Azure. 
   
    ![Configure o lado de início de sessão na aplicação única](./media/hostedgraphite-tutorial/tutorial_hostedgraphite_002.png)
   

1. Selecione "**só de leitura**" como **predefinido a função de utilizador**.
    
    ![Configure o lado de início de sessão na aplicação única](./media/hostedgraphite-tutorial/tutorial_hostedgraphite_004.png)

1. Abra o certificado com codificação base 64 no bloco de notas transferido a partir do portal do Azure, copie o conteúdo do mesmo para a área de transferência e, em seguida, cole-os para o **certificado X.509** caixa de texto.
    
    ![Configure o lado de início de sessão na aplicação única](./media/hostedgraphite-tutorial/tutorial_hostedgraphite_005.png)

1. Clique em **guardar** botão.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [Documentação do Azure AD incorporado]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/hostedgraphite-tutorial/create_aaduser_01.png) 

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/hostedgraphite-tutorial/create_aaduser_02.png) 

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/hostedgraphite-tutorial/create_aaduser_03.png) 

1. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/hostedgraphite-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-a-hosted-graphite-test-user"></a>Criar um utilizador de teste Graphite alojado

O objetivo desta secção é criar um usuário chamado Eduarda Almeida no Graphite alojado. Graphite alojada suporta o aprovisionamento de just-in-time, que está por predefinição, ativada.

Não existe nenhum item de ação para nesta secção. Durante uma tentativa de aceder Graphite alojados, se não existir ainda será criado um novo utilizador.

>[!NOTE]
>Se precisar de criar manualmente um utilizador, terá de contactar a equipa de suporte de Graphite hospedado por <mailto:help@hostedgraphite.com>. 

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar do Azure-início de sessão único, concedendo acesso para Graphite alojado.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida a Graphite alojado, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **alojada Graphite**.

    ![Configurar o início de sessão único](./media/hostedgraphite-tutorial/tutorial_hostedgraphite_app.png) 

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

O objetivo desta secção consiste em testar a configuração de SSO do Azure AD através do painel de acesso.

Quando clica no mosaico Graphite hospedado no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo hospedado Graphite.

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/hostedgraphite-tutorial/tutorial_general_01.png
[2]: ./media/hostedgraphite-tutorial/tutorial_general_02.png
[3]: ./media/hostedgraphite-tutorial/tutorial_general_03.png
[4]: ./media/hostedgraphite-tutorial/tutorial_general_04.png

[100]: ./media/hostedgraphite-tutorial/tutorial_general_100.png

[200]: ./media/hostedgraphite-tutorial/tutorial_general_200.png
[201]: ./media/hostedgraphite-tutorial/tutorial_general_201.png
[202]: ./media/hostedgraphite-tutorial/tutorial_general_202.png
[203]: ./media/hostedgraphite-tutorial/tutorial_general_203.png

