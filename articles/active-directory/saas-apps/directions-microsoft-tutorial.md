---
title: 'Tutorial: Integração do Azure Active Directory com as direções em Microsoft | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e as direções no Microsoft.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: e0c8986f-2acd-418d-a306-437abc44b640
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: b83f452300295078e4edf797ec2ef2f0c5223031
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39047478"
---
# <a name="tutorial-azure-active-directory-integration-with-directions-on-microsoft"></a>Tutorial: Integração do Azure Active Directory com as direções em Microsoft

Neste tutorial, saiba como integrar as direções em Microsoft com o Azure Active Directory (Azure AD).

Integrar as direções em Microsoft no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD, quem tem acesso para as direções em Microsoft
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para as direções em Microsoft (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com as direções em Microsoft, precisa do seguinte:

- Uma subscrição do Azure AD
- Um direções no Microsoft logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando as direções no Microsoft da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-directions-on-microsoft-from-the-gallery"></a>Adicionando as direções no Microsoft da Galeria
Para configurar a integração entre as direções em Microsoft com o Azure AD, terá de adicionar as direções no Microsoft a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar as direções no Microsoft a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

4. Na caixa de pesquisa, escreva **as direções em Microsoft**.

    ![Criar um utilizador de teste do Azure AD](./media/directions-microsoft-tutorial/tutorial_directionsonmicrosoft_search.png)

5. No painel de resultados, selecione **as direções em Microsoft**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/directions-microsoft-tutorial/tutorial_directionsonmicrosoft_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configurar e testar o Azure AD início de sessão único com as direções em Microsoft com base num utilizador de teste chamado "Eduarda Almeida."

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte nas direções na Microsoft para um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado nas direções no Microsoft deve ser estabelecido.

Nas direções na Microsoft, atribuir o valor do **nome de utilizador** no Azure AD como o valor da **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com as direções em Microsoft, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um direções no utilizador de teste da Microsoft](#creating-a-directions-on-microsoft-test-user)**  - para ter um equivalente da Eduarda Almeida nas direções na Microsoft, que está ligada à representação de utilizador do Azure AD.
4. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão único no portal do Azure e configurar o início de sessão único em seu as direções no aplicativo da Microsoft.

**Para configurar o Azure AD início de sessão único com as direções em Microsoft, execute os seguintes passos:**

1. No portal do Azure, sobre o **as direções em Microsoft** página de integração de aplicativo, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/directions-microsoft-tutorial/tutorial_directionsonmicrosoft_samlbase.png)

3. Sobre o **as direções em Microsoft Domain e URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/directions-microsoft-tutorial/tutorial_directionsonmicrosoft_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão:
    |  |
    | --- |
    | `https://www.directionsonmicrosoft.com/user/login` |
    | `https://<subdomain>.devcloud.acquia-sites.com/<companyname>` |

    b. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão:
    |  |
    | --- |
    | `https://rhelmdirectionsonmicrosoftcomtest.devcloud.acquia-sites.com/simplesaml/<companyname>` |
    | `https://www.directionsonmicrosoft.com/simplesaml/<companyname>` |

    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o URL de início de sessão e o identificador real. Contacte [equipa de suporte de direções no cliente do Microsoft](mailto:service@DirectionsOnMicrosoft.com) obter esses valores. 
 
4. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![Configurar o início de sessão único](./media/directions-microsoft-tutorial/tutorial_directionsonmicrosoft_certificate.png) 

5. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/directions-microsoft-tutorial/tutorial_general_400.png)

6. Para configurar o início de sessão único num **as direções em Microsoft** lado, terá de enviar o transferido **XML de metadados** para [equipa de suporte as direções em Microsoft](mailto:service@DirectionsOnMicrosoft.com). Para ativar as instruções na equipe de suporte da Microsoft localizar a associação do site federado, inclua informações da sua empresa no seu e-mail.
    
    >[!NOTE]
    >Início de sessão único para as direções em Microsoft precisa de ser ativada pelos [equipa de suporte de direções no cliente do Microsoft](mailto:service@DirectionsOnMicrosoft.com). Receberá uma notificação quando o início de sessão único foi ativada.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/directions-microsoft-tutorial/create_aaduser_01.png) 

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/directions-microsoft-tutorial/create_aaduser_02.png) 

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/directions-microsoft-tutorial/create_aaduser_03.png) 

4. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/directions-microsoft-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-a-directions-on-microsoft-test-user"></a>Criar um direções no utilizador de teste da Microsoft

Não existe nenhum item de ação para configurar o aprovisionamento de utilizador para as direções em Microsoft.  

Quando um utilizador atribuído tenta iniciar sessão para as direções em Microsoft com o painel de acesso, as direções em Microsoft verifica se o usuário existe. Se nenhuma conta de utilizador disponível ainda existe, é criado automaticamente pelas direções em Microsoft.

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para as direções em Microsoft.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida para as direções em Microsoft, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **as direções em Microsoft**.

    ![Configurar o início de sessão único](./media/directions-microsoft-tutorial/tutorial_directionsonmicrosoft_app.png) 

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.
 
Quando clica as instruções apresentadas no mosaico da Microsoft no painel de acesso, deve obter automaticamente com sessão iniciada para sua as direções em aplicativos da Microsoft.

Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/directions-microsoft-tutorial/tutorial_general_01.png
[2]: ./media/directions-microsoft-tutorial/tutorial_general_02.png
[3]: ./media/directions-microsoft-tutorial/tutorial_general_03.png
[4]: ./media/directions-microsoft-tutorial/tutorial_general_04.png

[100]: ./media/directions-microsoft-tutorial/tutorial_general_100.png

[200]: ./media/directions-microsoft-tutorial/tutorial_general_200.png
[201]: ./media/directions-microsoft-tutorial/tutorial_general_201.png
[202]: ./media/directions-microsoft-tutorial/tutorial_general_202.png
[203]: ./media/directions-microsoft-tutorial/tutorial_general_203.png

