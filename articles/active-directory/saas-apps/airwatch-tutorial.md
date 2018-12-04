---
title: 'Tutorial: Integração do Azure Active Directory com AirWatch | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e AirWatch.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 96a3bb1c-96c6-40dc-8ea0-060b0c2a62e5
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: bf95b949d6fee4057f67d1e44ded36f363aa5e2b
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52848923"
---
# <a name="tutorial-azure-active-directory-integration-with-airwatch"></a>Tutorial: Integração do Azure Active Directory com AirWatch

Neste tutorial, saiba como integrar AirWatch com o Azure Active Directory (Azure AD).

Integrar AirWatch no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao AirWatch
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para AirWatch (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com AirWatch, terá dos seguintes itens:

- Uma subscrição do Azure
- Um AirWatch início de sessão único na subscrição ativado

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando AirWatch da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-airwatch-from-the-gallery"></a>Adicionando AirWatch da Galeria
Para configurar a integração da AirWatch com o Azure AD, terá de adicionar AirWatch a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar AirWatch a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

4. Na caixa de pesquisa, escreva **AirWatch**.

    ![Criar um utilizador de teste do Azure AD](./media/airwatch-tutorial/tutorial_airwatch_search.png)

5. No painel de resultados, selecione **AirWatch**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/airwatch-tutorial/tutorial_airwatch_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com AirWatch com base num utilizador de teste chamado "Eduarda Almeida."

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no AirWatch a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no AirWatch deve ser estabelecido.

Esta relação de ligação é estabelecida atribuindo o valor do **nome de utilizador** no Azure AD como o valor da **Username** no AirWatch.

Para configurar e testar o Azure AD início de sessão único com AirWatch, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste da AirWatch](#creating-a-airwatch-test-user)**  - para ter um equivalente da Eduarda Almeida na AirWatch que está ligado à representação de utilizador do Azure AD.
4. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo AirWatch.

**Para configurar o Azure AD início de sessão único com AirWatch, execute os seguintes passos:**

1. No portal do Azure, sobre o **AirWatch** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/airwatch-tutorial/tutorial_airwatch_samlbase.png)

3. Sobre o **AirWatch domínio e URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/airwatch-tutorial/tutorial_airwatch_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<subdomain>.awmdm.com/AirWatch/Login?gid=companycode`

    b. Na **identificador** caixa de texto, digite o valor como `AirWatch`

    > [!NOTE] 
    > Este valor não é a verdadeira. Atualize este valor com o URL de início de sessão real. Contacte [equipa de suporte de cliente da AirWatch](https://www.air-watch.com/company/contact-us/) para obter este valor. 
 
4. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro XML em seu computador.

    ![Configurar o início de sessão único](./media/airwatch-tutorial/tutorial_airwatch_certificate.png) 

5. Sobre o **AirWatch configuração** secção, clique em **configurar AirWatch** para abrir **configurar início de sessão** janela. Cópia a **SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configurar o início de sessão único](./media/airwatch-tutorial/tutorial_airwatch_configure.png) 

6. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/airwatch-tutorial/tutorial_general_400.png)
<CS>
7. Numa janela do browser web diferente, inicie sessão no site da sua empresa AirWatch como administrador.

8. No painel de navegação esquerdo, clique em **contas**e, em seguida, clique em **administradores**.
   
   ![Os administradores](./media/airwatch-tutorial/ic791920.png "administradores")

9. Expanda a **definições** e, em seguida, clique **dos serviços de diretório**.
   
   ![As definições](./media/airwatch-tutorial/ic791921.png "definições")

10. Clique nas **usuário** separador a **Base DN** caixa de texto, escreva o seu nome de domínio e, em seguida, clique em **guardar**.
   
   ![Usuário](./media/airwatch-tutorial/ic791922.png "utilizador")

11. Clique nas **servidor** separador.
   
   ![Servidor](./media/airwatch-tutorial/ic791923.png "Server")

12. Execute os seguintes passos:
    
    ![Carregue](./media/airwatch-tutorial/ic791924.png "carregar")   
    
    a. Como **tipo de diretório**, selecione **nenhum**.

    b. Selecione **utilizar SAML para autenticação**.

    c. Para carregar o certificado transferido, clique em **carregar**.

13. Na **pedir** secção, execute os seguintes passos:
    
    ![Pedir](./media/airwatch-tutorial/ic791925.png "do pedido")  

    a. Como **solicitar tipo de enlace**, selecione **POST**.

    b. No portal do Azure, no **configurar o início de sessão único em Airwatch** página de diálogo, copie a **SAML único início de sessão no URL do serviço** valor e, em seguida, cole-o no **identidade fornecedor de início de sessão único URL** caixa de texto.

    c. Como **formato NameID**, selecione **endereço de E-Mail**.

    d. Clique em **Guardar**.

14. Clique nas **utilizador** separador novamente.
    
    ![Usuário](./media/airwatch-tutorial/ic791926.png "utilizador")

15. Na **atributo** secção, execute os seguintes passos:
    
    ![Atributo](./media/airwatch-tutorial/ic791927.png "atributo")

    a. Na **identificador de objeto** caixa de texto, tipo **http://schemas.microsoft.com/identity/claims/objectidentifier**.

    b. Na **nome de utilizador** caixa de texto, tipo **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.

    c. Na **nome a apresentar** caixa de texto, tipo **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.

    d. Na **nome próprio** caixa de texto, tipo **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.

    e. Na **sobrenome** caixa de texto, tipo **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**.

    f. Na **E-Mail** caixa de texto, tipo **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.

    g. Clique em **Guardar**.

<CE>

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/airwatch-tutorial/create_aaduser_01.png) 

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/airwatch-tutorial/create_aaduser_02.png) 

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/airwatch-tutorial/create_aaduser_03.png) 

4. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/airwatch-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** da Eduarda Almeida.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-a-airwatch-test-user"></a>Criar um utilizador de teste da AirWatch

Para ativar a utilizadores do Azure AD iniciar sessão no AirWatch, eles têm de ser aprovisionados para AirWatch.

* Quando AirWatch, aprovisionamento é uma tarefa manual.

**Para Aprovisionar uma conta de utilizador, execute os seguintes passos:**

1. Inicie sessão no seu **AirWatch** site da empresa como administrador.
2. No painel de navegação no lado esquerdo, clique em **contas**e, em seguida, clique em **utilizadores**.
   
   ![Os utilizadores](./media/airwatch-tutorial/ic791929.png "utilizadores")
3. Na **usuários** menu, clique em **vista de lista**e, em seguida, clique em **Add \> adicionar utilizador**.
   
   ![Adicionar utilizador](./media/airwatch-tutorial/ic791930.png "adicionar utilizador")
4. Sobre o **Adicionar / Editar utilizador** caixa de diálogo, execute os seguintes passos:

   ![Adicionar utilizador](./media/airwatch-tutorial/ic791931.png "adicionar utilizador")   
   1. Tipo de **nome de utilizador**, **palavra-passe**, **Confirmar palavra-passe**, **nome próprio**, **Apelido**,  **Endereço de e-mail** de uma conta válida do Azure Active Directory que pretende aprovisionar em caixas de texto relacionadas.
   2. Clique em **Guardar**.

>[!NOTE]
>Pode utilizar quaisquer outras AirWatch utilizador conta criação ferramentas ou APIs fornecidas pelo AirWatch para aprovisionar contas de utilizador do AAD.
>  

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para AirWatch.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida a AirWatch, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **AirWatch**.

    ![Configurar o início de sessão único](./media/airwatch-tutorial/tutorial_airwatch_app.png) 

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Se pretender testar as definições de início de sessão únicas, abra o painel de acesso. Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md).


## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/airwatch-tutorial/tutorial_general_01.png
[2]: ./media/airwatch-tutorial/tutorial_general_02.png
[3]: ./media/airwatch-tutorial/tutorial_general_03.png
[4]: ./media/airwatch-tutorial/tutorial_general_04.png

[100]: ./media/airwatch-tutorial/tutorial_general_100.png

[200]: ./media/airwatch-tutorial/tutorial_general_200.png
[201]: ./media/airwatch-tutorial/tutorial_general_201.png
[202]: ./media/airwatch-tutorial/tutorial_general_202.png
[203]: ./media/airwatch-tutorial/tutorial_general_203.png

