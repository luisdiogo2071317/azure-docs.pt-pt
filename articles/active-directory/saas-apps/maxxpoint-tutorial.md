---
title: 'Tutorial: Integração do Active Directory do Azure com MaxxPoint | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e MaxxPoint.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 15ba026e-96fc-4ae8-b135-0169da810e99
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/13/2017
ms.author: jeedes
ms.openlocfilehash: bc83c67a2f5cbe0f0ccaaabb0c5fafd60828fd45
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55192864"
---
# <a name="tutorial-azure-active-directory-integration-with-maxxpoint"></a>Tutorial: Integração do Active Directory do Azure com MaxxPoint

Neste tutorial, saiba como integrar MaxxPoint com o Azure Active Directory (Azure AD).

Integrar MaxxPoint no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao MaxxPoint
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para MaxxPoint (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com MaxxPoint, terá dos seguintes itens:

- Uma subscrição do Azure
- Um MaxxPoint início de sessão único na subscrição ativado

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não deve utilizar o seu ambiente de produção, a menos que isso é necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando MaxxPoint da Galeria
1. Configuração e teste do Azure AD início de sessão único


## <a name="adding-maxxpoint-from-the-gallery"></a>Adicionando MaxxPoint da Galeria
Para configurar a integração do MaxxPoint com o Azure AD, terá de adicionar MaxxPoint a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar MaxxPoint a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
1. Clique em **nova aplicação** botão na parte superior de caixa de diálogo para adicionar nova aplicação.

    ![Aplicações][3]

1. Na caixa de pesquisa, escreva **MaxxPoint**.

    ![Criar um utilizador de teste do Azure AD](./media/maxxpoint-tutorial/tutorial_maxxpoint_001.png)

1. No painel de resultados, selecione **MaxxPoint**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/maxxpoint-tutorial/tutorial_maxxpoint_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com MaxxPoint com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no MaxxPoint a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no MaxxPoint deve ser estabelecido.

Esta relação de ligação é estabelecida atribuindo o valor do **nome de utilizador** no Azure AD como o valor da **Username** no MaxxPoint.

Para configurar e testar o Azure AD início de sessão único com MaxxPoint, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste MaxxPoint](#creating-a-maxxpoint-test-user)**  - para ter um equivalente da Eduarda Almeida na MaxxPoint que está ligado à representação de ela do Azure AD.
1. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo MaxxPoint.

**Para configurar o Azure AD início de sessão único com MaxxPoint, execute os seguintes passos:**

1. No portal do Azure, sobre o **MaxxPoint** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/maxxpoint-tutorial/tutorial_general_300.png)

1. Na **MaxxPoint domínio e URLs** secção, se desejar configurar a aplicação no **IDP iniciada pelo modo**, sem a necessidade de efetuar outros passos.

    ![Configurar o início de sessão único](./media/maxxpoint-tutorial/tutorial_maxxpoint_02.png)
    
1. Sobre o **MaxxPoint domínio e URLs** secção, se desejar configurar a aplicação no **SP iniciada pelo modo**, execute os seguintes passos:
    
    ![Configurar o início de sessão único](./media/maxxpoint-tutorial/tutorial_maxxpoint_03.png)

    a. Clique em **Mostrar definições de URL avançadas** opção

    b. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://maxxpoint.westipc.com/default/sso/login/entity/<customer-id>-azure`

    > [!NOTE] 
    > Tenha em atenção que isto não é o valor real. Terá de atualizar este valor com o início de sessão no URL real. Chamar MaxxPoint equipe **888-728-0950** para obter este valor.

1. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![Configurar o início de sessão único](./media/maxxpoint-tutorial/tutorial_maxxpoint_06.png) 

1. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/maxxpoint-tutorial/tutorial_general_400.png)

1. Para obter SSO configurado para a sua aplicação, chamar a equipa de suporte MaxxPoint **888-728-0950** e irá ajudá-lo ainda mais sobre como fornecer o transferido **XML de metadados** ficheiro. 

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, simplesmente clique em **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [Documentação do Azure AD incorporado]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/maxxpoint-tutorial/create_aaduser_01.png) 

1. Aceda a **utilizadores e grupos** e clique em **todos os utilizadores** para apresentar a lista de utilizadores.
    
    ![Criar um utilizador de teste do Azure AD](./media/maxxpoint-tutorial/create_aaduser_02.png) 

1. Na parte superior da caixa de diálogo, clique em **Add** para abrir o **utilizador** caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/maxxpoint-tutorial/create_aaduser_03.png) 

1. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/maxxpoint-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**. 

### <a name="creating-a-maxxpoint-test-user"></a>Criar um utilizador de teste MaxxPoint

Nesta secção, vai criar um usuário chamado Eduarda Almeida no MaxxPoint. Contacte a equipa de suporte MaxxPoint no **888-728-0950** para adicionar os utilizadores na aplicação MaxxPoint.

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para MaxxPoint.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida a MaxxPoint, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **MaxxPoint**.

    ![Configurar o início de sessão único](./media/maxxpoint-tutorial/tutorial_maxxpoint_50.png) 

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico MaxxPoint no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo MaxxPoint.


## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/maxxpoint-tutorial/tutorial_general_01.png
[2]: ./media/maxxpoint-tutorial/tutorial_general_02.png
[3]: ./media/maxxpoint-tutorial/tutorial_general_03.png
[4]: ./media/maxxpoint-tutorial/tutorial_general_04.png

[100]: ./media/maxxpoint-tutorial/tutorial_general_100.png

[200]: ./media/maxxpoint-tutorial/tutorial_general_200.png
[201]: ./media/maxxpoint-tutorial/tutorial_general_201.png
[202]: ./media/maxxpoint-tutorial/tutorial_general_202.png
[203]: ./media/maxxpoint-tutorial/tutorial_general_203.png
