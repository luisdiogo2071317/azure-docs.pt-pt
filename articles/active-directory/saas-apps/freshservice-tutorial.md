---
title: 'Tutorial: Integração do Azure Active Directory ao Freshservice | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e ao Freshservice.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 3dd22b1f-445d-45c6-8eda-30207eb9a1a8
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/16/2017
ms.author: jeedes
ms.openlocfilehash: cccbe2052336012b9ac98b3e28dc6481cbf9aefb
ms.sourcegitcommit: 727a0d5b3301fe20f20b7de698e5225633191b06
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/19/2018
ms.locfileid: "39144537"
---
# <a name="tutorial-azure-active-directory-integration-with-freshservice"></a>Tutorial: Integração do Azure Active Directory ao Freshservice

Neste tutorial, saiba como integrar ao Freshservice com o Azure Active Directory (Azure AD).

Integrar ao Freshservice no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Freshservice
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada ao Freshservice (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com ao Freshservice, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Ao Freshservice início de sessão único na subscrição ativado

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando ao Freshservice da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-freshservice-from-the-gallery"></a>Adicionando ao Freshservice da Galeria
Para configurar a integração ao Freshservice com o Azure AD, terá de adicionar ao Freshservice a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar ao Freshservice a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

4. Na caixa de pesquisa, escreva **ao Freshservice**.

    ![Criar um utilizador de teste do Azure AD](./media/freshservice-tutorial/tutorial_freshservice_search.png)

5. No painel de resultados, selecione **ao Freshservice**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/freshservice-tutorial/tutorial_freshservice_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com ao Freshservice com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte ao Freshservice a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado ao Freshservice deve ser estabelecido.

Ao Freshservice, atribua o valor do **nome de utilizador** no Azure AD como o valor da **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único ao Freshservice, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste ao Freshservice](#creating-a-freshservice-test-user)**  - para ter um equivalente da Eduarda Almeida ao Freshservice que está ligado à representação de utilizador do Azure AD.
4. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único na sua aplicação ao Freshservice.

**Para configurar o Azure AD início de sessão único ao Freshservice, execute os seguintes passos:**

1. No portal do Azure, sobre o **ao Freshservice** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/freshservice-tutorial/tutorial_freshservice_samlbase.png)

3. Sobre o **ao Freshservice domínio e URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/freshservice-tutorial/tutorial_freshservice_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<democompany>.freshservice.com`

    b. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://<democompany>.freshservice.com`

    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o URL de início de sessão e o identificador real. Contacte [equipa de suporte de cliente ao Freshservice](https://support.freshservice.com/) obter esses valores. 
 
4. Sobre o **certificado de assinatura SAML** secção, copie **THUMBPRINT** valor do certificado.

    ![Configurar o início de sessão único](./media/freshservice-tutorial/tutorial_freshservice_certificate.png)

5. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/freshservice-tutorial/tutorial_general_400.png)

6. Sobre o **configuração ao Freshservice** secção, clique em **ao Freshservice configurar** para abrir **configurar início de sessão** janela. Cópia a **URL de fim de sessão e SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configurar o início de sessão único](./media/freshservice-tutorial/tutorial_freshservice_configure.png) 

7. Numa janela do browser web diferente, inicie sessão no site da sua empresa ao Freshservice como administrador.

8. No menu na parte superior, clique em **administrador**.
   
    ![Admin](./media/freshservice-tutorial/ic790814.png "Admin")

9. Na **Portal do cliente**, clique em **segurança**.
   
    ![Segurança](./media/freshservice-tutorial/ic790815.png "segurança")

10. Na **segurança** secção, execute os seguintes passos:
   
    ![Início de sessão único](./media/freshservice-tutorial/ic790816.png "início de sessão único")
   
    a. Comutador **início de sessão único**.

    b. Selecione **SAML SSO**.

    c. Na **URL de início de sessão de SAML** caixa de texto, cole o valor de **SAML único início de sessão no URL do serviço**, que copiou do portal do Azure.

    d. Na **URL de fim de sessão** caixa de texto, cole o valor de **URL de fim de sessão**, que copiou do portal do Azure.

    e. Na **impressão digital do certificado de segurança** caixa de texto, colar a **THUMBPRINT** valor do certificado que copiou do portal do Azure.

    f. Clicar em **Guardar**

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/freshservice-tutorial/create_aaduser_01.png) 

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/freshservice-tutorial/create_aaduser_02.png) 

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/freshservice-tutorial/create_aaduser_03.png) 

4. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/freshservice-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-a-freshservice-test-user"></a>Criar um utilizador de teste ao Freshservice

Para ativar a utilizadores do Azure AD iniciar sessão ao FreshService, tem de ser aprovisionados ao FreshService. No caso de ao FreshService, o aprovisionamento é uma tarefa manual.

**Para Aprovisionar uma conta de utilizador, execute os seguintes passos:**

1. Inicie sessão no seu **ao FreshService** site da empresa como administrador.

2. No menu na parte superior, clique em **administrador**.
   
    ![Admin](./media/freshservice-tutorial/ic790814.png "Admin")

3. Na **gestão de utilizadores** secção, clique em **os solicitantes**.
   
    ![Os solicitantes](./media/freshservice-tutorial/ic790818.png "os solicitantes")

4. Clique em **autor do pedido novo**.
   
    ![Os solicitantes novos](./media/freshservice-tutorial/ic790819.png "os solicitantes de novo")

5. Na **autor do pedido novo** secção, execute os seguintes passos:
   
    ![Novo autor do pedido](./media/freshservice-tutorial/ic790820.png "novo autor do pedido")   

    a. Introduza o **nome próprio** e **E-Mail** atributos de uma conta válida do Azure Active Directory que pretende aprovisionar em caixas de texto relacionadas.

    b. Clique em **Guardar**.
   
    >[!NOTE]
    >O titular da conta do Azure Active Directory obtém um e-mail, incluindo uma ligação para confirmar a conta até se tornar Active Directory
    >  

>[!NOTE]
>Pode utilizar quaisquer outras ao FreshService utilizador conta criação ferramentas ou APIs fornecidas ao FreshService para aprovisionar contas de utilizador do AAD.
>  

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida ao Freshservice, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **ao Freshservice**.

    ![Configurar o início de sessão único](./media/freshservice-tutorial/tutorial_freshservice_app.png) 

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

O objetivo desta secção é testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico ao Freshservice no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação ao Freshservice.

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/freshservice-tutorial/tutorial_general_01.png
[2]: ./media/freshservice-tutorial/tutorial_general_02.png
[3]: ./media/freshservice-tutorial/tutorial_general_03.png
[4]: ./media/freshservice-tutorial/tutorial_general_04.png

[100]: ./media/freshservice-tutorial/tutorial_general_100.png

[200]: ./media/freshservice-tutorial/tutorial_general_200.png
[201]: ./media/freshservice-tutorial/tutorial_general_201.png
[202]: ./media/freshservice-tutorial/tutorial_general_202.png
[203]: ./media/freshservice-tutorial/tutorial_general_203.png

