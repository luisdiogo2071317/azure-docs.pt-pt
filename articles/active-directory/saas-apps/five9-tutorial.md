---
title: 'Tutorial: Integração do Azure Active Directory com Five9 adaptador Plus (CTI, contacte o Centro de agentes) | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Five9 adaptador Plus (CTI, contacte o Centro de agentes).
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 88dc82ab-be0b-4017-8335-c47d00775d7b
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: jeedes
ms.openlocfilehash: c8d3364f9de298d19df35846af5f421978fea960
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52842174"
---
# <a name="tutorial-azure-active-directory-integration-with-five9-plus-adapter-cti-contact-center-agents"></a>Tutorial: Integração do Azure Active Directory com Five9 adaptador Plus (CTI, contacte o Centro de agentes)

Neste tutorial, saiba como integrar Five9 adaptador Plus (CTI, agentes do Centro de contacto) com o Azure Active Directory (Azure AD).

Integrar Five9 adaptador Plus (CTI, agentes do Centro de contacto) com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Five9 adaptador Plus (CTI, contacte o Centro de agentes)
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Five9 adaptador Plus (CTI, contacte o Centro de agentes) (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Five9 adaptador Plus (CTI, contacte o Centro de agentes), terá dos seguintes itens:

- Uma subscrição do Azure
- Um Five9 adaptador Plus (CTI, contacte o Centro de agentes) início de sessão único de subscrição ativada

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma avaliação de um mês aqui: [oferta de avaliação](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Five9 adaptador Plus (CTI, contacte o Centro de agentes) da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-five9-plus-adapter-cti-contact-center-agents-from-the-gallery"></a>Adicionando Five9 adaptador Plus (CTI, contacte o Centro de agentes) da Galeria
Para configurar a integração do Five9 adaptador Plus (CTI, agentes do Centro de contacto) com o Azure AD, terá de adicionar Five9 placa Plus (CTI, contacte o Centro de agentes) da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Five9 adaptador Plus (CTI, contacte o Centro de agentes) a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

1. Na caixa de pesquisa, escreva **Five9 adaptador Plus (CTI, contacte o Centro de agentes)**.

    ![Criar um utilizador de teste do Azure AD](./media/five9-tutorial/tutorial_five9_search.png)

1. No painel de resultados, selecione **Five9 adaptador Plus (CTI, contacte o Centro de agentes)** e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/five9-tutorial/tutorial_five9_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com Five9 Plus adaptador (CTI, agentes do Centro de contacto) com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Five9 adaptador Plus (CTI, contacte o Centro de agentes) para um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Five9 adaptador Plus (CTI, contacte o Centro de agentes) deve ser estabelecido.

Five9 Plus adaptador (CTI, contacte o Centro de agentes), atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com Five9 adaptador Plus (CTI, contacte o Centro de agentes), tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste Five9 adaptador Plus (CTI, contacte o Centro de agentes)](#creating-a-five9-plus-adapter-cti-contact-center-agents-test-user)**  - para ter um equivalente da Eduarda Almeida na Five9 Plus adaptador (CTI, contacte o Centro de agentes) que está ligada à representação de utilizador do Azure AD.
1. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo Five9 adaptador Plus (CTI, contacte o Centro de agentes).

**Para configurar o Azure AD início de sessão único com Five9 adaptador Plus (CTI, contacte o Centro de agentes), execute os seguintes passos:**

1. No portal do Azure, sobre o **Five9 adaptador Plus (CTI, contacte o Centro de agentes)** página de integração de aplicativo, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/five9-tutorial/tutorial_five9_samlbase.png)

1. Sobre o **Five9 adaptador Plus (CTI, contacte o Centro de agentes) de domínio e URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/five9-tutorial/tutorial_five9_url.png)
    
    a. Na **identificador** caixa de texto, escreva um URL com os seguintes padrões:

    |    Ambiente      |       do IdP      |
    | :-- | :-- |
    | Para "Five9 Plus adaptador para o Microsoft Dynamics CRM" | `https://app.five9.com/appsvcs/saml/metadata/alias/msdc` |
    | Para "Five9 Plus adaptador para Zendesk" | `https://app.five9.com/appsvcs/saml/metadata/alias/zd` |
    | Para "Five9 Plus adaptador para o Toolkit de área de trabalho do agente" | `https://app.five9.com/appsvcs/saml/metadata/alias/adt` |

    b. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão:

    |      Ambiente     |      do IdP      |
    | :--                  | :--           |
    | Para "Five9 Plus adaptador para o Microsoft Dynamics CRM" | `https://app.five9.com/appsvcs/saml/SSO/alias/msdc` |
    | Para "Five9 Plus adaptador para Zendesk" | `https://app.five9.com/appsvcs/saml/SSO/alias/zd` |
    | Para "Five9 Plus adaptador para o Toolkit de área de trabalho do agente" | `https://app.five9.com/appsvcs/saml/SSO/alias/adt` |

1. Sobre o **certificado de assinatura SAML** secção, clique em **Certificate(Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![Configurar o início de sessão único](./media/five9-tutorial/tutorial_five9_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/five9-tutorial/tutorial_general_400.png)

1. No **Five9 adaptador Plus (CTI, contacte o Centro de agentes) Configuration** secção, clique em **configurar Five9 Plus adaptador (CTI, contacte o Centro de agentes)** para abrir **configurar início de sessão**janela. Cópia a **URL de fim de sessão, o ID de entidade de SAML e o SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configurar o início de sessão único](./media/five9-tutorial/tutorial_five9_configure.png) 

1. Para configurar o início de sessão único num **Five9 adaptador Plus (CTI, contacte o Centro de agentes)** lado, terá de enviar o transferido **Certificate(Base64), o URL de fim de sessão, o ID de entidade de SAML e o SAML único início de sessão no URL do serviço**para [a equipa de suporte Five9 adaptador Plus (CTI, contacte o Centro de agentes)](https://www.five9.com/about/contact). Também, além disso, para configurar o SSO ainda mais, siga os passos, de acordo com o adaptador abaixo:

    a. "Five9 Plus adaptador para o Toolkit de área de trabalho do agente" Guia do administrador: [https://webapps.five9.com/assets/files/for_customers/documentation/integrations/agent-desktop-toolkit/plus-agent-desktop-toolkit-administrators-guide.pdf](https://webapps.five9.com/assets/files/for_customers/documentation/integrations/agent-desktop-toolkit/plus-agent-desktop-toolkit-administrators-guide.pdf)
    
    b. "Five9 Plus adaptador para o Microsoft Dynamics CRM" Guia do administrador: [https://webapps.five9.com/assets/files/for_customers/documentation/integrations/microsoft/microsoft-administrators-guide.pdf](https://webapps.five9.com/assets/files/for_customers/documentation/integrations/microsoft/microsoft-administrators-guide.pdf)
    
    c. "Five9 Plus adaptador para Zendesk" Guia do administrador: [https://webapps.five9.com/assets/files/for_customers/documentation/integrations/zendesk/zendesk-plus-administrators-guide.pdf](https://webapps.five9.com/assets/files/for_customers/documentation/integrations/zendesk/zendesk-plus-administrators-guide.pdf)


> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/five9-tutorial/create_aaduser_01.png) 

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/five9-tutorial/create_aaduser_02.png) 

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/five9-tutorial/create_aaduser_03.png) 

1. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/five9-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-a-five9-plus-adapter-cti-contact-center-agents-test-user"></a>Criar um utilizador de teste Five9 adaptador Plus (CTI, contacte o Centro de agentes)

Nesta secção, vai criar um usuário chamado Eduarda Almeida no Five9 adaptador Plus (CTI, contacte o Centro de agentes). Trabalhar com [equipa de suporte de Five9 adaptador Plus (CTI, contacte o Centro de agentes)](https://www.five9.com/about/contact) para adicionar os utilizadores na plataforma Five9 adaptador Plus (CTI, contacte o Centro de agentes). Os utilizadores tem de ser criados e ativados antes de utilizar o início de sessão único.

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso a Five9 adaptador Plus (CTI, contacte o Centro de agentes).

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida a Five9 adaptador Plus (CTI, contacte o Centro de agentes), execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **Five9 adaptador Plus (CTI, contacte o Centro de agentes)**.

    ![Configurar o início de sessão único](./media/five9-tutorial/tutorial_five9_app.png) 

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico Five9 adaptador Plus (CTI, contacte o Centro de agentes) no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação Five9 adaptador Plus (CTI, contacte o Centro de agentes).
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[1]: ./media/five9-tutorial/tutorial_general_01.png
[2]: ./media/five9-tutorial/tutorial_general_02.png
[3]: ./media/five9-tutorial/tutorial_general_03.png
[4]: ./media/five9-tutorial/tutorial_general_04.png

[100]: ./media/five9-tutorial/tutorial_general_100.png

[200]: ./media/five9-tutorial/tutorial_general_200.png
[201]: ./media/five9-tutorial/tutorial_general_201.png
[202]: ./media/five9-tutorial/tutorial_general_202.png
[203]: ./media/five9-tutorial/tutorial_general_203.png

