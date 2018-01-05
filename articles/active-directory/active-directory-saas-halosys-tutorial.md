---
title: "Tutorial: Integração do Azure Active Directory com Halosys | Microsoft Docs"
description: "Saiba como utilizar Halosys com o Azure Active Directory para ativar o início de sessão único, aprovisionamento automatizado e muito mais!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: mtillman
ms.assetid: 42a0eb7c-5cb7-44a9-b00b-b0e7df4b63e8
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/03/2018
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: 6e8167c1152fe80813d5c13706a72badce0a0ce9
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/05/2018
---
# <a name="tutorial-azure-active-directory-integration-with-halosys"></a>Tutorial: Integração do Azure Active Directory com Halosys

Neste tutorial, irá aprender a integrar Halosys com o Azure Active Directory (Azure AD).

Integrar Halosys com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Halosys
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Halosys (Single Sign-On) com as respetivas contas do Azure AD
- Pode gerir as contas numa localização central - portal do Azure

Se pretender saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [que é o acesso a aplicações e início de sessão no Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Halosys, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Halosys início de sessão único subscrição ativado


> [!NOTE] 
> Para testar os passos neste tutorial, não recomendamos a utilização num ambiente de produção.


Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não deve utilizar o seu ambiente de produção, a menos que isto é necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, teste do Azure AD-início de sessão único num ambiente de teste.

O cenário descrito neste tutorial consiste em dois blocos modulares principais:

1. Adicionar Halosys a partir da Galeria
2. Configurar e testar o Azure AD de sessão único-


## <a name="adding-halosys-from-the-gallery"></a>Adicionar Halosys a partir da Galeria
Para configurar a integração de Halosys com o Azure AD, terá de adicionar Halosys a partir da Galeria à sua lista de aplicações SaaS geridas.

**Para adicionar Halosys a partir da galeria, execute os seguintes passos:**

1. No  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **do Azure Active Directory** ícone. 

    ![Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todas as aplicações**.

    ![Aplicações][2]
    
3. Para adicionar a nova aplicação, clique em **nova aplicação** botão no topo da caixa de diálogo.

    ![Aplicações][3]

6. Na caixa de pesquisa, escreva **Halosys**.

    ![Criar um utilizador de teste do Azure AD](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_01.png)
    
7. No painel de resultados, selecione **Halosys**e, em seguida, clique em **concluída** para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_011.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD de sessão único-
Nesta secção, configure e teste do Azure AD-início de sessão único com Halosys com base num utilizador de teste chamado "Britta Simon".

Para início de sessão trabalhar, do Azure AD tem de saber o que o utilizador homólogo no Halosys é um utilizador no Azure AD. Por outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Halosys tem de ser estabelecida.

Esta relação de ligação é estabelecida ao atribuir o valor da **nome de utilizador** no Azure AD como o valor a **Username** no Halosys.

Para configurar e testar o Azure AD-início de sessão único com Halosys, tem de concluir os blocos modulares seguintes:

1. **[Configurar o Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  - para permitir aos utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD-início de sessão único com Britta Simon.
3. **[Criar um utilizador de teste Halosys](#creating-a-halosys-test-user)**  - para ter um homólogo de Britta Simon Halosys que está ligada a representação do Azure AD da forma.
4. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar Britta Simon utilizar o Azure AD-início de sessão único.
5. **[Teste o início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configurar o Azure AD-início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão no portal e configurar o início de sessão único na sua aplicação Halosys.


**Para configurar o Azure AD-início de sessão único com Halosys, execute os seguintes passos:**

1. No portal do Azure, no **SCC ciclo de vida** página de integração de aplicações, clique em **de sessão único-**.

    ![Configurar o início de sessão único][4]

2. No **de sessão único-** caixa de diálogo, selecione **modo** como **baseados em SAML início de sessão** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/active-directory-saas-scclifecycle-tutorial/tutorial_scclifecycle_samlbase.png)

3. No **Halosys domínio e os URLs** secção, execute os seguintes passos:
    1. No **URL de início de sessão** caixa de texto, escreva um URL a utilizar o padrão do seguinte:`https://<sub-domain>.hs.com/ic7/welcome/customer/PICTtest.aspx`

    2. No **identificador** caixa de texto, escreva um URL a utilizar o padrão do seguinte:
    | |
    |--|--|
    | `https://bs1.hs.com/<entity>`|
    | `https://lifecycle.hs.com/<entity>`|
    
    > [!NOTE] 
    > Estes valores não estiverem reais. Atualize estes valores com o URL de início de sessão e o identificador real. Contacte [equipa de suporte de cliente de ciclo de vida de SCC](mailto:lifecycle.support@scc.com) para obter estes valores. 
         
4. No **certificado de assinatura de SAML** secção, selecione **XML de metadados** em **transferir**e, em seguida, guarde o ficheiro de metadados no seu computador.
   
5. Para obter o início de sessão configurado para a sua aplicação, contacte a equipa de suporte de Halosys e forneça-los com o seguinte:

  * O transferido **ficheiro de metadados**
  * O **URL SAML SSO**
    

  >[!NOTE]
  >O início de sessão único tem de ser ativado pela equipa de suporte do Halosys.


### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
Nesta secção, vai criar um utilizador de teste no portal do chamado Britta Simon.


![Criar utilizador do Azure AD][20]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No **portal do Azure**, no painel de navegação esquerdo, clique em **do Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/active-directory-saas-scclifecycle-tutorial/create_aaduser_01.png) 

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/active-directory-saas-scclifecycle-tutorial/create_aaduser_02.png) 

3. Para abrir o **utilizador** caixa de diálogo, clique em **adicionar** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/active-directory-saas-scclifecycle-tutorial/create_aaduser_03.png) 

4. No **utilizador** diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/active-directory-saas-scclifecycle-tutorial/create_aaduser_04.png) 

    a. No **nome** caixa de texto, tipo **BrittaSimon**.

    b. No **nome de utilizador** caixa de texto, tipo de **endereço de correio eletrónico** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e anote o valor da **palavra-passe**.

    d. Clique em **Criar**.


### <a name="creating-a-halosys-test-user"></a>Criar um utilizador de teste Halosys

Nesta secção, vai criar um utilizador chamado Britta Simon Halosys. . Funciona com a equipa de suporte de Halosys para adicionar os utilizadores na plataforma Halosys.


### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Britta Simon a utilizar Azure-início de sessão único, conceder o acesso ao Halosys.

![Atribua o utilizador][200] 

**Para atribuir Britta Simon a Halosys, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicações e, em seguida, navegue para a vista de diretório e aceda à **aplicações empresariais** , em seguida, clique em **todas as aplicações.**

    ![Atribua o utilizador][201] 

2. Na lista de aplicações, selecione **Halosys**.

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribua o utilizador][202] 

4. Clique em de **adicionar** botão. Em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![Atribua o utilizador][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Britta Simon** na lista utilizadores.

6. Clique em **selecione** botão no **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribuir** botão no **adicionar atribuição** caixa de diálogo.

### <a name="testing-single-sign-on"></a>Teste o início de sessão único

Nesta secção, testar a configuração do Azure AD único início de sessão através do painel de acesso.

Quando clica no mosaico Halosys no painel de acesso, deve obter automaticamente com sessão iniciada para a aplicação de Halosys. Para mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](active-directory-saas-access-panel-introduction.md).


## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](active-directory-saas-tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_205.png
 