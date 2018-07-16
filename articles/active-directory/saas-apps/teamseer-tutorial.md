---
title: 'Tutorial: Integração do Azure Active Directory com TeamSeer | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e TeamSeer.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 6ec4806f-fe0f-4ed7-8cfa-32d1c840433f
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: 6c13bb4c10adeeba59ff8d0f2c8d7120d6fae43a
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39050651"
---
# <a name="tutorial-azure-active-directory-integration-with-teamseer"></a>Tutorial: Integração do Azure Active Directory com TeamSeer

Neste tutorial, saiba como integrar TeamSeer com o Azure Active Directory (Azure AD).

Integrar TeamSeer no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao TeamSeer
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para TeamSeer (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com TeamSeer, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um TeamSeer início de sessão único na subscrição ativado

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando TeamSeer da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-teamseer-from-the-gallery"></a>Adicionando TeamSeer da Galeria
Para configurar a integração do TeamSeer para o Azure AD, terá de adicionar TeamSeer a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar TeamSeer a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

4. Na caixa de pesquisa, escreva **TeamSeer**.

    ![Criar um utilizador de teste do Azure AD](./media/teamseer-tutorial/tutorial_teamseer_search.png)

5. No painel de resultados, selecione **TeamSeer**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/teamseer-tutorial/tutorial_teamseer_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com TeamSeer com base num utilizador de teste chamado "Eduarda Almeida."

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no TeamSeer a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no TeamSeer deve ser estabelecido.

TeamSeer, atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com TeamSeer, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste TeamSeer](#creating-a-teamseer-test-user)**  - para ter um equivalente da Eduarda Almeida na TeamSeer que está ligado à representação de utilizador do Azure AD.
4. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo TeamSeer.

**Para configurar o Azure AD início de sessão único com TeamSeer, execute os seguintes passos:**

1. No portal do Azure, sobre o **TeamSeer** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/teamseer-tutorial/tutorial_teamseer_samlbase.png)

3. Sobre o **TeamSeer domínio e URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/teamseer-tutorial/tutorial_teamseer_url.png)

     Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://www.teamseer.com/<companyid>`

    > [!NOTE] 
    > O valor não é real. Atualize o valor com o URL de início de sessão real. Contacte [equipa de suporte de cliente TeamSeer](http://pages.theaccessgroup.com/solutions_business-suite_absence-management_contact.html) para obter o valor. 
 
4. Sobre o **certificado de assinatura SAML** secção, clique em **Certificate(Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![Configurar o início de sessão único](./media/teamseer-tutorial/tutorial_teamseer_certificate.png) 

5. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/teamseer-tutorial/tutorial_general_400.png)

6. Sobre o **TeamSeer configuração** secção, clique em **configurar TeamSeer** para abrir **configurar início de sessão** janela. Cópia a **SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configurar o início de sessão único](./media/teamseer-tutorial/tutorial_teamseer_configure.png)

7. Numa janela do browser web diferente, inicie sessão no site da sua empresa TeamSeer como administrador.

8. Aceda a **administrador de RH**.
   
    ![Administrador de RH](./media/teamseer-tutorial/ic789634.png "administrador de RH")

9. Clique em **configuração**.
   
    ![Programa de configuração](./media/teamseer-tutorial/ic789635.png "configuração")

10. Clique em **configurar informações de fornecedor SAML**.
   
    ![Definições de SAML](./media/teamseer-tutorial/ic789636.png "definições de SAML")

11. Na secção de detalhes do fornecedor de SAML, execute os seguintes passos:
   
    ![Definições de SAML](./media/teamseer-tutorial/ic789637.png "definições de SAML")   

    a. Colar o **único URL de início de sessão no serviço** valor na o **URL** caixa de texto.
          
    b. Abra o certificado com codificação base 64 no bloco de notas, copie o conteúdo de-lo na sua área de transferência e, em seguida, cole-os para o **certificado público do IdP** caixa de texto.

12. Para concluir a configuração de fornecedores SAML, execute os seguintes passos:
    
    ![Definições de SAML](./media/teamseer-tutorial/ic789638.png "definições de SAML") 

    a. Na **endereços de E-Mail de teste**, escreva o endereço de e-mail do utilizador de teste. 
  
    b. Na **emissor** caixa de texto, escreva o URL do emissor do fornecedor de serviços. 
  
    c. Clique em **Guardar**.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/teamseer-tutorial/create_aaduser_01.png) 

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/teamseer-tutorial/create_aaduser_02.png) 

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/teamseer-tutorial/create_aaduser_03.png) 

4. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/teamseer-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-a-teamseer-test-user"></a>Criar um utilizador de teste TeamSeer

Para ativar a utilizadores do Azure AD iniciar sessão no TeamSeer, eles têm de ser aprovisionados para ShiftPlanning. No caso de TeamSeer, o aprovisionamento é uma tarefa manual.

**Para Aprovisionar uma conta de utilizador, execute os seguintes passos:**

1. Inicie sessão no seu **TeamSeer** site da empresa como administrador.

2. Execute os seguintes passos:
   
    ![Administrador de RH](./media/teamseer-tutorial/ic789640.png "administrador de RH")  
 
    a. Aceda a **administrador de RH \> utilizadores**.
  
    b. Clique em **executar o Assistente de novo utilizador**.

3. Na **detalhes de utilizador** secção, execute os seguintes passos:
   
    ![Detalhes de utilizador](./media/teamseer-tutorial/ic789641.png "detalhes de utilizador")

    a. Tipo de **nome próprio**, **Apelido**, **nome de utilizador (endereço de E-Mail)** de uma conta do AAD válida que pretende aprovisionar em para as caixas de texto relacionadas.
  
    b. Clique em **Seguinte**.

4. Siga na tela instruções para adicionar um novo utilizador e clique em **concluir**.

>[!NOTE]
>Pode utilizar quaisquer outras TeamSeer utilizador conta criação ferramentas ou APIs fornecidas pelo TeamSeer para aprovisionar contas de utilizador do Azure AD. 

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para TeamSeer.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida a TeamSeer, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **TeamSeer**.

    ![Configurar o início de sessão único](./media/teamseer-tutorial/tutorial_teamseer_app.png) 

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

Se pretender testar as definições de início de sessão únicas, abra o painel de acesso. Para obter mais detalhes sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/teamseer-tutorial/tutorial_general_01.png
[2]: ./media/teamseer-tutorial/tutorial_general_02.png
[3]: ./media/teamseer-tutorial/tutorial_general_03.png
[4]: ./media/teamseer-tutorial/tutorial_general_04.png

[100]: ./media/teamseer-tutorial/tutorial_general_100.png

[200]: ./media/teamseer-tutorial/tutorial_general_200.png
[201]: ./media/teamseer-tutorial/tutorial_general_201.png
[202]: ./media/teamseer-tutorial/tutorial_general_202.png
[203]: ./media/teamseer-tutorial/tutorial_general_203.png

