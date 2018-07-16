---
title: 'Tutorial: Integração do Azure Active Directory com Blackboard Saiba | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Blackboard saiba.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 0b8ca505-61ea-487c-9a3e-fa50c936df0c
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2017
ms.author: jeedes
ms.openlocfilehash: 1148e6f85905888eccafde638de66215073e0eaa
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39043691"
---
# <a name="tutorial-azure-active-directory-integration-with-blackboard-learn"></a>Tutorial: Integração do Azure Active Directory com Blackboard Saiba

Neste tutorial, saiba como integrar o Blackboard Saiba com o Azure Active Directory (Azure AD).

Integrar o Blackboard Saiba com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Blackboard Saiba
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Blackboard saber (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Blackboard saiba, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Blackboard saiba logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Blackboard saiba da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-blackboard-learn-from-the-gallery"></a>Adicionando Blackboard saiba da Galeria
Para configurar a integração do Blackboard Saiba com o Azure AD, terá de adicionar Blackboard saiba partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar a Blackboard saiba a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

4. Na caixa de pesquisa, escreva **Blackboard Saiba**.

    ![Criar um utilizador de teste do Azure AD](./media/blackboard-learn-tutorial/tutorial_blackboardlearn_search.png)

5. No painel de resultados, selecione **Blackboard Saiba**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/blackboard-learn-tutorial/tutorial_blackboardlearn_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com Blackboard Saiba com base num utilizador de teste chamado "Eduarda Almeida."

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Blackboard saiba a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Blackboard saiba deve ser estabelecido.

Esta relação de ligação é estabelecida atribuindo o valor do **nome de utilizador** no Azure AD como o valor do **Username** no Blackboard saiba.

Para configurar e testar o Azure AD início de sessão único com Blackboard saiba, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste Blackboard Saiba](#creating-a-blackboard-learn-test-user)**  - para ter um equivalente da Eduarda Almeida na Blackboard saiba que está ligado à representação de utilizador do Azure AD.
4. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único na sua aplicação saiba Blackboard.

**Para configurar o Azure AD início de sessão único com Blackboard saiba, execute os seguintes passos:**

1. No portal do Azure, sobre o **Blackboard Saiba** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/blackboard-learn-tutorial/tutorial_blackboardlearn_samlbase.png)

3. Sobre o **Blackboard domínio de aprender e URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/blackboard-learn-tutorial/tutorial_blackboardlearn_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<subdomain>.blackboard.com/`

    b. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://<subdomain>.blackboard.com/auth-saml/saml/SSO/entity-id/SAML_AD`
    
    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o URL de início de sessão e o identificador real. Contacte [equipa de suporte de cliente de saiba Blackboard](https://www.blackboard.com/support/index.aspx) obter esses valores. 

4. Aplicação de saiba blackboard espera que as asserções SAML num formato específico. Configure as seguintes declarações para esta aplicação. Pode gerir os valores destes atributos do **atributos de utilizador** secção na página de integração de aplicações.
 Captura de ecrã seguinte mostra um exemplo sobre ele.

    ![Configurar o início de sessão único](./media/blackboard-learn-tutorial/tutorial_attribute.png)

5. Na **atributos de utilizador** secção na **início de sessão único** caixa de diálogo, configurar atributos de token SAML, conforme mostrado na imagem e execute os seguintes passos. Podemos mapeou Userprincipalname como o atributo de utilizador exclusivo aqui, mas pode mapeá-lo para o valor apropriado, o que distingue exclusivamente o utilizador na organização e que mapeia para o campo de nome de utilizador Blackboard saiba.
           
    | Nome do Atributo | Valor do Atributo |   
    | ---------------| ----------------|
    | urn:oid:1.3.6.1.4.1.5923.1.1.1.6 |user.userprincipalname |

    a. Clique em **adicionar atributo** para abrir o **adicionar atributo** caixa de diálogo.

    ![Configurar o início de sessão único](./media/blackboard-learn-tutorial/tutorial_attribute_04.png)
    
    ![Configurar o início de sessão único](./media/blackboard-learn-tutorial/tutorial_attribute_05.png)

    b. Na **nome** caixa de texto, escreva o nome de atributo apresentado para essa linha.

    c. Partir do **valor** lista, digite o valor de atributo apresentado para essa linha.
    
    d. Clique em **OK**.

4. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro XML em seu computador.

    ![Configurar o início de sessão único](./media/blackboard-learn-tutorial/tutorial_blackboardlearn_certificate.png)

7. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/blackboard-learn-tutorial/tutorial_general_400.png)

8. Sobre o **a configuração de saiba Blackboard** secção, clique em **configurar Blackboard Saiba** para abrir **configurar início de sessão** janela. Cópia a **ID de entidade de SAML** partir o **secção de referência rápida.**

    ![Configurar o início de sessão único](./media/blackboard-learn-tutorial/tutorial_blackboardlearn_configure.png) 

9. Para configurar o início de sessão único num **Blackboard Saiba** lado, terá de enviar o transferido **XML de metadados** e **ID de entidade de SAML** para [Blackboard Saiba suportar](https://www.blackboard.com/support/index.aspx).

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/blackboard-learn-tutorial/create_aaduser_01.png) 

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/blackboard-learn-tutorial/create_aaduser_02.png) 

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/blackboard-learn-tutorial/create_aaduser_03.png) 

4. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/blackboard-learn-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** da Eduarda Almeida.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-a-blackboard-learn-test-user"></a>Criar um utilizador de teste Blackboard Saiba
Nesta secção, vai criar um usuário chamado Eduarda Almeida no Blackboard saiba. 

Aplicação de mais de blackboard suporta apenas no tempo de aprovisionamento de utilizador. Certifique-se de que configurou as afirmações conforme descrito na secção  **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**
### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para Blackboard saber.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida para Blackboard saber, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **Blackboard Saiba**.

    ![Configurar o início de sessão único](./media/blackboard-learn-tutorial/tutorial_blackboardlearn_app.png) 

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico Blackboard saiba no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação saiba Blackboard. Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/blackboard-learn-tutorial/tutorial_general_01.png
[2]: ./media/blackboard-learn-tutorial/tutorial_general_02.png
[3]: ./media/blackboard-learn-tutorial/tutorial_general_03.png
[4]: ./media/blackboard-learn-tutorial/tutorial_general_04.png

[100]: ./media/blackboard-learn-tutorial/tutorial_general_100.png

[200]: ./media/blackboard-learn-tutorial/tutorial_general_200.png
[201]: ./media/blackboard-learn-tutorial/tutorial_general_201.png
[202]: ./media/blackboard-learn-tutorial/tutorial_general_202.png
[203]: ./media/blackboard-learn-tutorial/tutorial_general_203.png

