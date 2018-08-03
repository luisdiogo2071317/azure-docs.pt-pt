---
title: 'Tutorial: Integração do Azure Active Directory com o representante | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e o representante.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 5665c3ac-5689-4201-80fe-fcc677d4430d
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: d4cf47dba0501694c5ed000c087f16d36b0dcdde
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39427151"
---
# <a name="tutorial-azure-active-directory-integration-with-deputy"></a>Tutorial: Integração do Azure Active Directory com o representante

Neste tutorial, saiba como integrar o representante com o Azure Active Directory (Azure AD).

Integrar o representante no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao representante
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para representante (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o representante, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um representante de logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando o representante da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-deputy-from-the-gallery"></a>Adicionando o representante da Galeria
Para configurar a integração do representante no Azure AD, terá de adicionar representante a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar o representante da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

1. Na caixa de pesquisa, escreva **representante**.

    ![Criar um utilizador de teste do Azure AD](./media/deputy-tutorial/tutorial_deputy_search.png)

1. No painel de resultados, selecione **representante**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/deputy-tutorial/tutorial_deputy_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com o representante com base num utilizador de teste chamado "Eduarda Almeida."

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no representante a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no representante deve ser estabelecido.

Representante, atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com o representante, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste do representante](#creating-a-deputy-test-user)**  - para ter um equivalente da Eduarda Almeida no representante que está ligado à representação de utilizador do Azure AD.
1. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão único no portal do Azure e configurar o início de sessão único em seu representante de aplicativo.

**Para configurar o Azure AD início de sessão único com o representante, execute os seguintes passos:**

1. No portal do Azure, sobre o **representante** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/deputy-tutorial/tutorial_deputy_samlbase.png)

1. Na **representante domínio e URLs** secção, se desejar configurar a aplicação na **IDP** iniciada pelo modo:

    ![Configurar o início de sessão único](./media/deputy-tutorial/tutorial_deputy_url1.png)

    a. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão:
    |  |
    | ----|
    | `https://<subdomain>.<region>.au.deputy.com` |
    | `https://<subdomain>.<region>.ent-au.deputy.com` |
    | `https://<subdomain>.<region>.na.deputy.com`|
    | `https://<subdomain>.<region>.ent-na.deputy.com`|
    | `https://<subdomain>.<region>.eu.deputy.com` |
    | `https://<subdomain>.<region>.ent-eu.deputy.com` |
    | `https://<subdomain>.<region>.as.deputy.com` |
    | `https://<subdomain>.<region>.ent-as.deputy.com` |
    | `https://<subdomain>.<region>.la.deputy.com` |
    | `https://<subdomain>.<region>.ent-la.deputy.com` |
    | `https://<subdomain>.<region>.af.deputy.com` |
    | `https://<subdomain>.<region>.ent-af.deputy.com` |
    | `https://<subdomain>.<region>.an.deputy.com` |
    | `https://<subdomain>.<region>.ent-an.deputy.com` |
    | `https://<subdomain>.<region>.deputy.com` |

    b. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão:
    | |
    |----|
    | `https://<subdomain>.<region>.au.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.ent-au.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.na.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.ent-na.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.eu.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.ent-eu.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.as.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.ent-as.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.la.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.ent-la.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.af.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.ent-af.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.an.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.ent-an.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.deputy.com/exec/devapp/samlacs.` |

1. Verifique **Mostrar definições de URL avançadas**. Se desejar configurar a aplicação no **SP** iniciada pelo modo:

    ![Configurar o início de sessão único](./media/deputy-tutorial/tutorial_deputy_url2.png)

    Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<your-subdomain>.<region>.deputy.com`
    
    >[!NOTE]
    > Sufixo de região de representante é opcional, ou deverá utilizar um dos seguintes: au | ND | eu | como | la | af | um | ent au | ent ND | ent UE | ent-como | Ent-la | Ent af | Ent – uma

    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o identificador de real, a URL de resposta e o URL de início de sessão. Contacte [equipa de suporte do representante](https://www.deputy.com/call-centers-customer-support-scheduling-software) obter esses valores. 

1. Sobre o **certificado de assinatura SAML** secção, clique em **Certificate(Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![Configurar o início de sessão único](./media/deputy-tutorial/tutorial_deputy_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/deputy-tutorial/tutorial_general_400.png)
    
1. No **configuração do representante** secção, clique em **configurar representante** para abrir **configurar início de sessão** janela. Cópia a **SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configurar o início de sessão único](./media/deputy-tutorial/tutorial_deputy_configure.png) 

1. Navegue para o seguinte URL:[https://(your-subdomain).deputy.com/exec/config/system_config]( https://(your-subdomain).deputy.com/exec/config/system_config). Aceda a **definições de segurança** e clique em **editar**.
   
    ![Configurar o início de sessão único](./media/deputy-tutorial/tutorial_deputy_004.png)

1. Sobre isso **definições de segurança** página, executar passos abaixo.

    ![Configurar o início de sessão único](./media/deputy-tutorial/tutorial_deputy_005.png)
    
    a. Ativar **início de sessão Social**.
   
    b. Abra o seu certificado codificado Base64 transferido a partir do portal do Azure no bloco de notas, copie o conteúdo do mesmo para a área de transferência e, em seguida, cole-os para o **certificado de OpenSSL** caixa de texto.
   
    c. Na caixa de texto URL de SSO SAML, escreva `https://<your subdomain>.deputy.com/exec/devapp/samlacs?dpLoginTo=<saml sso url>`
    
    d. Na caixa de texto URL de SSO SAML, substitua `<your subdomain>` com o subdomínio.
   
    e. Na caixa de texto URL de SSO SAML, substitua `<saml sso url>` com o **SAML único início de sessão no URL do serviço** copiou do portal do Azure.
   
    f. Clique em **guardar as definições de**.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/deputy-tutorial/create_aaduser_01.png) 

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/deputy-tutorial/create_aaduser_02.png) 

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/deputy-tutorial/create_aaduser_03.png) 

1. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/deputy-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-a-deputy-test-user"></a>Criar um utilizador de teste do representante

Para ativar a utilizadores do Azure AD iniciar sessão no representante, tem de ser aprovisionados no representante. Em caso de representante, aprovisionamento é uma tarefa manual.

#### <a name="to-provision-a-user-account-perform-the-following-steps"></a>Para Aprovisionar uma conta de utilizador, execute os seguintes passos:
1. Inicie sessão no site da sua empresa representante como administrador.

1. No painel de navegação superior, clique em **pessoas**.
   
   ![As pessoas](./media/deputy-tutorial/tutorial_deputy_001.png "pessoas")

1. Clique nas **adicionar pessoas** botão e clique em **adicionar uma única pessoa**.
   
   ![Adicionar pessoas](./media/deputy-tutorial/tutorial_deputy_002.png "adicionar pessoas")

1. Execute os passos seguintes e clique em **guardar e convidar**.
   
   ![Novo utilizador](./media/deputy-tutorial/tutorial_deputy_003.png "novo utilizador")

   a. Na **Name** caixa de texto, nome do tipo de utilizador, como **BrittaSimon**.
   
   b. Na **E-Mail** caixa de texto, escreva o endereço de e-mail de uma conta do Azure AD que pretende aprovisionar.
   
   c. Na **trabalha na** caixa de texto, escreva o nome de empresa.
   
   d. Clique em **guardar e convidar** botão.

1. O titular da conta do AAD recebe uma mensagem de e-mail e segue uma ligação para confirmar a respetiva conta até se tornar Active Directory. Pode utilizar quaisquer outras representante utilizador conta criação ferramentas ou APIs fornecidas pelo representante para aprovisionar contas de utilizador do AAD.

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso ao representante.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida a representante, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **representante**.

    ![Configurar o início de sessão único](./media/deputy-tutorial/tutorial_deputy_app.png) 

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

O objetivo desta secção consiste em testar a configuração de SSO do Azure AD através do painel de acesso.

Quando clica no mosaico de representante no painel de acesso, deve obter automaticamente com sessão iniciada para o seu representante de aplicativo.

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[1]: ./media/deputy-tutorial/tutorial_general_01.png
[2]: ./media/deputy-tutorial/tutorial_general_02.png
[3]: ./media/deputy-tutorial/tutorial_general_03.png
[4]: ./media/deputy-tutorial/tutorial_general_04.png

[100]: ./media/deputy-tutorial/tutorial_general_100.png

[200]: ./media/deputy-tutorial/tutorial_general_200.png
[201]: ./media/deputy-tutorial/tutorial_general_201.png
[202]: ./media/deputy-tutorial/tutorial_general_202.png
[203]: ./media/deputy-tutorial/tutorial_general_203.png

