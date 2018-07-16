---
title: 'Tutorial: Integração do Azure Active Directory com SmarterU | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e SmarterU.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 95fe3212-d052-4ac8-87eb-ac5305227e85
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 32feac32af01e518ad6cede43cc4b4f00c097d3b
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39041284"
---
# <a name="tutorial-azure-active-directory-integration-with-smarteru"></a>Tutorial: Integração do Azure Active Directory com SmarterU

Neste tutorial, saiba como integrar SmarterU com o Azure Active Directory (Azure AD).

Integrar SmarterU no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao SmarterU
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para SmarterU (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com SmarterU, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um SmarterU logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando SmarterU da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-smarteru-from-the-gallery"></a>Adicionando SmarterU da Galeria
Para configurar a integração do SmarterU com o Azure AD, terá de adicionar SmarterU a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar SmarterU a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

4. Na caixa de pesquisa, escreva **SmarterU**.

    ![Criar um utilizador de teste do Azure AD](./media/smarteru-tutorial/tutorial_smarteru_search.png)

5. No painel de resultados, selecione **SmarterU**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/smarteru-tutorial/tutorial_smarteru_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com SmarterU com base num utilizador de teste chamado "Eduarda Almeida."

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no SmarterU a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no SmarterU deve ser estabelecido.

SmarterU, atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com SmarterU, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste SmarterU](#creating-a-smarteru-test-user)**  - para ter um equivalente da Eduarda Almeida na SmarterU que está ligado à representação de utilizador do Azure AD.
4. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo SmarterU.

**Para configurar o Azure AD início de sessão único com SmarterU, execute os seguintes passos:**

1. No portal do Azure, sobre o **SmarterU** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/smarteru-tutorial/tutorial_smarteru_samlbase.png)

3. Sobre o **SmarterU domínio e URLs** secção, execute os seguintes passos: 

    ![Configurar o início de sessão único](./media/smarteru-tutorial/tutorial_smarteru_url.png)

    Na **identificador** caixa de texto, escreva o URL: `https://www.smarteru.com/`

4. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![Configurar o início de sessão único](./media/smarteru-tutorial/tutorial_smarteru_certificate.png) 

5. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/smarteru-tutorial/tutorial_general_400.png)

6. Numa janela do browser web diferente, inicie sessão no site da sua empresa SmarterU como administrador.

7. Na barra de ferramentas na parte superior, clique em **definições de conta**.
   
    ![Definições da conta](./media/smarteru-tutorial/IC777326.png "definições da conta")

8. Na página de configuração da conta, execute os seguintes passos:
   
    ![Autorização externa](./media/smarteru-tutorial/IC777327.png "autorização externa") 
 
      a. Selecione **ativar a autorização externa**.
  
      b. Na **controle de logon do mestre** secção, selecione a **SmarterU** separador.
  
      c. Na **início de sessão do utilizador predefinido** secção, selecione a **SmarterU** separador.
  
      d. Selecione **ativar o Okta**.
  
      e. Copie o conteúdo do ficheiro de metadados baixado e, em seguida, cole-o para o **Okta metadados** caixa de texto.
  
      f. Clique em **Guardar**.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/smarteru-tutorial/create_aaduser_01.png) 

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/smarteru-tutorial/create_aaduser_02.png) 

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/smarteru-tutorial/create_aaduser_03.png) 

4. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/smarteru-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-a-smarteru-test-user"></a>Criar um utilizador de teste SmarterU

Para ativar a utilizadores do Azure AD iniciar sessão no SmarterU, tem de ser aprovisionados em SmarterU.

Quando SmarterU, aprovisionamento é uma tarefa manual.

**Para Aprovisionar uma conta de utilizador, execute os seguintes passos:**

1. Inicie sessão no seu **SmarterU** inquilino.

2. Aceda a **utilizadores**.

3. Na secção de utilizador, execute os seguintes passos:
   
    ![Novo utilizador](./media/smarteru-tutorial/IC777329.png "novo utilizador")  

    a. Clique em **+ utilizador**.
    
    b. Escreva os valores de atributo relacionado da conta de utilizador do Azure AD para as caixas de texto seguintes: **E-Mail principal**, **ID de funcionário**, **palavra-passe**, **Certifique-se Palavra-passe**, **nome fornecido**, **Apelido**.
    
    c. Clique em **Active Directory**. 
    
    d. Clique em **Guardar**.

>[!NOTE]
>Pode utilizar quaisquer outras SmarterU utilizador conta criação ferramentas ou APIs fornecidas pelo SmarterU para aprovisionar contas de utilizador do AAD.
> 

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para SmarterU.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida a SmarterU, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **SmarterU**.

    ![Configurar o início de sessão único](./media/smarteru-tutorial/tutorial_smarteru_app.png) 

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.
 
Quando clica no mosaico SmarterU no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo SmarterU.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 


## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/smarteru-tutorial/tutorial_general_01.png
[2]: ./media/smarteru-tutorial/tutorial_general_02.png
[3]: ./media/smarteru-tutorial/tutorial_general_03.png
[4]: ./media/smarteru-tutorial/tutorial_general_04.png

[100]: ./media/smarteru-tutorial/tutorial_general_100.png

[200]: ./media/smarteru-tutorial/tutorial_general_200.png
[201]: ./media/smarteru-tutorial/tutorial_general_201.png
[202]: ./media/smarteru-tutorial/tutorial_general_202.png
[203]: ./media/smarteru-tutorial/tutorial_general_203.png

