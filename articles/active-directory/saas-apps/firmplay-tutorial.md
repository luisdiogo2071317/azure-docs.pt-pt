---
title: 'Tutorial: Integração do Azure Active Directory com FirmPlay - defesa do funcionário para Recruiting | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e FirmPlay - defesa do funcionário para Recruiting.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: a6799629-7546-43f8-a966-956db32864b1
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/15/2017
ms.author: jeedes
ms.openlocfilehash: abc29480e5f5531857cbf973d5386727c3d7f2ac
ms.sourcegitcommit: 4eddd89f8f2406f9605d1a46796caf188c458f64
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/11/2018
ms.locfileid: "49114533"
---
# <a name="tutorial-azure-active-directory-integration-with-firmplay---employee-advocacy-for-recruiting"></a>Tutorial: Integração do Azure Active Directory com FirmPlay - defesa do funcionário para Recruiting

Neste tutorial, saiba como integrar FirmPlay - defesa do funcionário para Recruiting com o Azure Active Directory (Azure AD).

Integrar FirmPlay - defesa do funcionário para Recruiting com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao FirmPlay - defesa do funcionário para Recruiting
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para FirmPlay - defesa do funcionário para Recruiting (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – o portal de gestão do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com FirmPlay - defesa do funcionário para Recruiting, terá dos seguintes itens:

- Uma subscrição do Azure
- Um FirmPlay - defesa de funcionário de recrutamento início de sessão único na subscrição ativado


> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.


Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não deve utilizar o seu ambiente de produção, a menos que isso é necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionar FirmPlay - defesa do funcionário para Recruiting a partir da Galeria
1. Configuração e teste do Azure AD início de sessão único


## <a name="adding-firmplay---employee-advocacy-for-recruiting-from-the-gallery"></a>Adicionar FirmPlay - defesa do funcionário para Recruiting a partir da Galeria
Para configurar a integração do FirmPlay - defesa do funcionário para Recruiting com o Azure AD, terá de adicionar FirmPlay - defesa do funcionário para Recruiting partir da galeria, à sua lista de aplicações de SaaS geridas.

**Para adicionar FirmPlay - defesa do funcionário para Recruiting a partir da galeria, execute os seguintes passos:**

1. Na  **[do Portal de gestão do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
1. Clique em **adicionar** botão na parte superior da caixa de diálogo.

    ![Aplicações][3]

1. Na caixa de pesquisa, escreva **FirmPlay - defesa do funcionário para Recruiting**.

    ![Criar um utilizador de teste do Azure AD](./media/firmplay-tutorial/tutorial_firmplay_001.png)

1. No painel de resultados, selecione **FirmPlay - defesa do funcionário para Recruiting**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/firmplay-tutorial/tutorial_firmplay_0001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com FirmPlay - defesa do funcionário para Recruiting com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único trabalhar, o Azure AD precisa de saber o que o utilizador de contraparte FirmPlay - defesa do funcionário para Recruiting for para um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no FirmPlay - defesa de funcionário de recrutamento tem de ser estabelecida.

Esta relação de ligação é estabelecida atribuindo o valor do **nome de utilizador** no Azure AD como o valor da **Username** no FirmPlay - defesa do funcionário para Recruiting.

Para configurar e testar o Azure AD início de sessão único com FirmPlay - defesa do funcionário para Recruiting, precisa concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um FirmPlay - defesa do funcionário para o utilizador de teste Recruiting](#creating-a-firmplay---employee-advocacy-for-recruiting-test-user)**  - para ter um equivalente da Eduarda Almeida na FirmPlay: representantes de funcionário de recrutamento ou seja ligado à representação de ela do Azure AD.
1. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal de gestão do Azure e configurar início de sessão único no seu FirmPlay - defesa de funcionários para a aplicação de Recruiting.

**Para configurar o Azure AD início de sessão único com FirmPlay - defesa do funcionário para Recruiting, execute os seguintes passos:**

1. No portal de gestão do Azure, sobre o **FirmPlay - defesa do funcionário para Recruiting** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

1. Sobre o **início de sessão único** caixa de diálogo, como **modo** selecione **baseado em SAML início de sessão** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/firmplay-tutorial/tutorial_firmplay_01.png)

1. Sobre o **FirmPlay - defesa do funcionário para o domínio de recrutamento e URLs** na secção a **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<your-subdomain>.firmplay.com/`

    ![Configurar o início de sessão único](./media/firmplay-tutorial/tutorial_firmplay_02.png)

    > [!NOTE] 
    > Tenha em atenção que isto não é o valor real. Terá de atualizar este valor com o início de sessão no URL real. Contacte [FirmPlay - defesa de funcionários para a equipa de suporte Recruiting](mailto:engineering@firmplay.com) para obter este valor. 

1. Sobre o **certificado de assinatura SAML** secção, clique em **criar novo certificado**.

    ![Configurar o início de sessão único](./media/firmplay-tutorial/tutorial_firmplay_03.png)     

1. Sobre o **criar novo certificado** caixa de diálogo, clique no ícone de calendário e selecione um **data de expiração**. Em seguida, clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/firmplay-tutorial/tutorial_general_300.png)

1. Sobre o **certificado de assinatura SAML** secção, selecione **ativar o novo certificado** e clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/firmplay-tutorial/tutorial_firmplay_04.png)

1. No pop-up **certificado de Rollover** janela, clique em **OK**.

    ![Configurar o início de sessão único](./media/firmplay-tutorial/tutorial_general_400.png)

1. Sobre o **certificado de assinatura SAML** secção, clique em **certificado (base64)** e, em seguida, guarde o ficheiro de certificado no seu computador. 

    ![Configurar o início de sessão único](./media/firmplay-tutorial/tutorial_firmplay_05.png) 

1. Sobre o **FirmPlay - defesa de funcionários para a configuração de recrutamento** secção, clique em **configurar FirmPlay - defesa do funcionário para Recruiting** para abrir **configurar início de sessão** caixa de diálogo.

    ![Configurar o início de sessão único](./media/firmplay-tutorial/tutorial_firmplay_06.png) 

    ![Configurar o início de sessão único](./media/firmplay-tutorial/tutorial_firmplay_07.png)

1. Para obter SSO configurado para a sua aplicação, contacte [FirmPlay - defesa de funcionários para a equipa de suporte Recruiting](mailto:engineering@firmplay.com) e fornecê-los com o seguinte: 

    • O transferido **ficheiro de certificado**

    • O **URL do serviço de início de início de sessão único SAML**

    • O **ID de entidade SAML**

    • O **URL de fim de sessão**
  

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção consiste em criar um utilizador de teste no portal do Azure Management chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal de gestão do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/firmplay-tutorial/create_aaduser_01.png) 

1. Aceda a **utilizadores e grupos** e clique em **todos os utilizadores** para apresentar a lista de utilizadores.
    
    ![Criar um utilizador de teste do Azure AD](./media/firmplay-tutorial/create_aaduser_02.png) 

1. Na parte superior da caixa de diálogo, clique em **Add** para abrir o **utilizador** caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/firmplay-tutorial/create_aaduser_03.png) 

1. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/firmplay-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**. 



### <a name="creating-a-firmplay---employee-advocacy-for-recruiting-test-user"></a>Criar um FirmPlay - defesa do funcionário para o utilizador de teste Recruiting

Nesta secção, vai criar um usuário chamado Eduarda Almeida no FirmPlay - defesa do funcionário para Recruiting. Trabalhe em conjunto com [FirmPlay - defesa de funcionários para a equipa de suporte Recruiting](mailto:engineering@firmplay.com) para adicionar os utilizadores no FirmPlay - defesa de funcionários para a plataforma de Recruiting.


### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único, concedendo acesso para FirmPlay - defesa do funcionário para Recruiting.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida a FirmPlay - defesa do funcionário para Recruiting, execute os seguintes passos:**

1. No portal de gestão do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **FirmPlay - defesa do funcionário para Recruiting**.

    ![Configurar o início de sessão único](./media/firmplay-tutorial/tutorial_firmplay_50.png) 

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    


### <a name="testing-single-sign-on"></a>Teste de início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica em FirmPlay - defesa do funcionário para Recruiting mosaico no painel de acesso, deve obter automaticamente com sessão iniciada para sua FirmPlay - defesa de funcionários para a aplicação de Recruiting.


## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/firmplay-tutorial/tutorial_general_01.png
[2]: ./media/firmplay-tutorial/tutorial_general_02.png
[3]: ./media/firmplay-tutorial/tutorial_general_03.png
[4]: ./media/firmplay-tutorial/tutorial_general_04.png

[100]: ./media/firmplay-tutorial/tutorial_general_100.png

[200]: ./media/firmplay-tutorial/tutorial_general_200.png
[201]: ./media/firmplay-tutorial/tutorial_general_201.png
[202]: ./media/firmplay-tutorial/tutorial_general_202.png
[203]: ./media/firmplay-tutorial/tutorial_general_203.png