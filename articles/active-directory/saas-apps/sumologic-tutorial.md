---
title: 'Tutorial: Integração do Active Directory do Azure com SumoLogic | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e SumoLogic.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: fbb76765-92d7-4801-9833-573b11b4d910
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: e48c32d93da6cc84e35f2b2952827a5d06a5b319
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55149739"
---
# <a name="tutorial-azure-active-directory-integration-with-sumologic"></a>Tutorial: Integração do Active Directory do Azure com SumoLogic

Neste tutorial, saiba como integrar SumoLogic com o Azure Active Directory (Azure AD).

Integrar SumoLogic no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao SumoLogic
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para SumoLogic (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com SumoLogic, terá dos seguintes itens:

- Uma subscrição do Azure
- Um SumoLogic logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando SumoLogic da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-sumologic-from-the-gallery"></a>Adicionando SumoLogic da Galeria
Para configurar a integração do SumoLogic com o Azure AD, terá de adicionar SumoLogic a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar SumoLogic a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

1. Na caixa de pesquisa, escreva **SumoLogic**.

    ![Criar um utilizador de teste do Azure AD](./media/sumologic-tutorial/tutorial_sumologic_search.png)

1. No painel de resultados, selecione **SumoLogic**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/sumologic-tutorial/tutorial_sumologic_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com SumoLogic com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no SumoLogic a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no SumoLogic deve ser estabelecido.

SumoLogic, atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com SumoLogic, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste SumoLogic](#creating-a-sumologic-test-user)**  - para ter um equivalente da Eduarda Almeida na SumoLogic que está ligado à representação de utilizador do Azure AD.
1. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo SumoLogic.

**Para configurar o Azure AD início de sessão único com SumoLogic, execute os seguintes passos:**

1. No portal do Azure, sobre o **SumoLogic** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/sumologic-tutorial/tutorial_sumologic_samlbase.png)

1. Sobre o **SumoLogic domínio e URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/sumologic-tutorial/tutorial_sumologic_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<tenantname>.SumoLogic.com`

    b. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão:
    | |
    |--|
    | `https://<tenantname>.us2.sumologic.com` |
    | `https://<tenantname>.sumologic.com` |
    | `https://<tenantname>.us4.sumologic.com` |
    | `https://<tenantname>.eu.sumologic.com` |
    | `https://<tenantname>.au.sumologic.com` |

    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o URL de início de sessão e o identificador real. Contacte [equipa de suporte de cliente SumoLogic](https://www.sumologic.com/contact-us/) obter esses valores. 
 
1. Sobre o **certificado de assinatura SAML** secção, clique em **certificado (Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![Configurar o início de sessão único](./media/sumologic-tutorial/tutorial_sumologic_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/sumologic-tutorial/tutorial_general_400.png)

1. Sobre o **SumoLogic configuração** secção, clique em **configurar SumoLogic** para abrir **configurar início de sessão** janela. Cópia a **ID de entidade de SAML e SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configurar o início de sessão único](./media/sumologic-tutorial/tutorial_sumologic_configure.png) 

1. Numa janela do browser web diferente, inicie sessão no site da sua empresa SumoLogic como administrador.

1. Aceda a **gerenciar \> segurança**.
   
    ![Gerir](./media/sumologic-tutorial/ic778556.png "gerir")

1. Clique em **SAML**.
   
    ![Configurações de segurança globais](./media/sumologic-tutorial/ic778557.png "configurações de segurança globais")

1. Partir do **selecionar uma configuração ou criar um novo** , selecione **do Azure AD**e, em seguida, clique em **configurar**.
   
    ![Configurar SAML 2.0](./media/sumologic-tutorial/ic778558.png "configurar SAML 2.0")

1. Sobre o **configurar SAML 2.0** caixa de diálogo, execute os seguintes passos:
   
    ![Configurar SAML 2.0](./media/sumologic-tutorial/ic778559.png "configurar SAML 2.0")
   
    a. Na **nome de configuração** caixa de texto, tipo **do Azure AD**. 

    b. Selecione **modo de depuração**.

    c. Na **emissor** caixa de texto, cole o valor de **ID de entidade de SAML**, que copiou do portal do Azure. 

    d. Na **URL do pedido Authn** caixa de texto, cole o valor de **SAML único início de sessão no URL do serviço**, que copiou do portal do Azure.

    e. Abra o certificado com codificação base 64 no bloco de notas, copie o conteúdo do mesmo para a área de transferência e, em seguida, cole o certificado completo para **certificado X.509** caixa de texto.

    f. Como **atributo de correio eletrónico**, selecione **assunto de utilização SAML**.  

    g. Selecione **SP iniciou a configuração de início de sessão**.

    h. Na **caminho de início de sessão** caixa de texto, tipo **Azure** e clique em **guardar**.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [Documentação do Azure AD incorporado]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/sumologic-tutorial/create_aaduser_01.png) 

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/sumologic-tutorial/create_aaduser_02.png) 

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/sumologic-tutorial/create_aaduser_03.png) 

1. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/sumologic-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-a-sumologic-test-user"></a>Criar um utilizador de teste SumoLogic

Para habilitar os utilizadores do Azure AD iniciar sessão no SumoLogic, tem de ser aprovisionados para SumoLogic.  

* No caso de SumoLogic, o aprovisionamento é uma tarefa manual.

**Para Aprovisionar uma conta de utilizador, execute os seguintes passos:**

1. Inicie sessão no seu **SumoLogic** inquilino.

1. Aceda a **gerenciar \> utilizadores**.
   
    ![Os utilizadores](./media/sumologic-tutorial/ic778561.png "utilizadores")

1. Clique em **Adicionar**.
   
    ![Os utilizadores](./media/sumologic-tutorial/ic778562.png "utilizadores")

1. Sobre o **novo utilizador** caixa de diálogo, execute os seguintes passos:
   
    ![Novo utilizador](./media/sumologic-tutorial/ic778563.png "novo utilizador") 
 
    a. Escreva as informações relacionadas da conta do Azure AD que pretende aprovisionar para o **nome próprio**, **Apelido**, e **E-Mail** caixas de texto.
  
    b. Selecione uma função.
  
    c. Como **Status**, selecione **Active Directory**.
  
    d. Clique em **Guardar**.

>[!NOTE]
>Pode utilizar quaisquer outras SumoLogic utilizador conta criação ferramentas ou APIs fornecidas pelo SumoLogic para aprovisionar contas de utilizador do AAD. 
> 

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para SumoLogic.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida a SumoLogic, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **SumoLogic**.

    ![Configurar o início de sessão único](./media/sumologic-tutorial/tutorial_sumologic_app.png) 

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

O objetivo desta secção é testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico SumoLogic no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo SumoLogic.

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/sumologic-tutorial/tutorial_general_01.png
[2]: ./media/sumologic-tutorial/tutorial_general_02.png
[3]: ./media/sumologic-tutorial/tutorial_general_03.png
[4]: ./media/sumologic-tutorial/tutorial_general_04.png

[100]: ./media/sumologic-tutorial/tutorial_general_100.png

[200]: ./media/sumologic-tutorial/tutorial_general_200.png
[201]: ./media/sumologic-tutorial/tutorial_general_201.png
[202]: ./media/sumologic-tutorial/tutorial_general_202.png
[203]: ./media/sumologic-tutorial/tutorial_general_203.png

