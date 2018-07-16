---
title: 'Tutorial: Integração do Azure Active Directory com Agiloft | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Agiloft.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: aca13814-cdbd-46b8-93dc-1578099c5ee4
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/09/2017
ms.author: jeedes
ms.openlocfilehash: f11d705cceb05c9e9cd0b340a680684eecf4f5d9
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39054204"
---
# <a name="tutorial-azure-active-directory-integration-with-agiloft"></a>Tutorial: Integração do Azure Active Directory com Agiloft

Neste tutorial, saiba como integrar Agiloft com o Azure Active Directory (Azure AD).

Integrar Agiloft no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Agiloft.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Agiloft (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Agiloft, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Agiloft logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Agiloft da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-agiloft-from-the-gallery"></a>Adicionando Agiloft da Galeria
Para configurar a integração do Agiloft com o Azure AD, terá de adicionar Agiloft a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Agiloft a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

4. Na caixa de pesquisa, escreva **Agiloft**, selecione **Agiloft** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Agiloft na lista de resultados](./media/agiloft-tutorial/tutorial_agiloft_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com Agiloft com base num utilizador de teste chamado "Eduarda Almeida."

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Agiloft a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Agiloft deve ser estabelecido.

Agiloft, atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com Agiloft, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste Agiloft](#create-an-agiloft-test-user)**  - para ter um equivalente da Eduarda Almeida na Agiloft que está ligado à representação de utilizador do Azure AD.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo Agiloft.

**Para configurar o Azure AD início de sessão único com Agiloft, execute os seguintes passos:**

1. No portal do Azure, sobre o **Agiloft** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/agiloft-tutorial/tutorial_agiloft_samlbase.png)

3. Sobre o **Agiloft domínio e URLs** secção, execute os seguintes passos, se desejar configurar a aplicação no modo de IDP iniciado:

    ![Agiloft domínio e URLs únicas início de sessão em informações](./media/agiloft-tutorial/tutorial_agiloft_url.png)

    a. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: 
    | |
    |-|-|
    | `https://<subdomain>.saas.enterprisewizard.com/project/<KB_NAME>` |
    | `https://<subdomain>.agiloft.com/project/<KB_NAME>` |

    b. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão:
    | |
    |-|-|
    | `https://<subdomain>.saas.enterprisewizard.com:443/gui2/spsamlsso?project=<KB_NAME>` |
    | `https://<subdomain>.agiloft.com:443/gui2/spsamlsso?project=<KB_NAME>` |

4. Verifique **Mostrar definições de URL avançadas** e executar o passo seguinte, se desejar configurar a aplicação na **SP** iniciada pelo modo:

    ![Agiloft domínio e URLs únicas início de sessão em informações](./media/agiloft-tutorial/tutorial_agiloft_url1.png)

    Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: 
    | |
    |-|-|
    | `https://<subdomain>.saas.enterprisewizard.com/gui2/samlssologin.jsp?project=<KB_NAME>` |
    | `https://<subdomain>.agiloft.com/gui2/samlssologin.jsp?project=<KB_NAME>` |
     
    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o identificador de real, a URL de resposta e o URL de início de sessão. Contacte [equipa de suporte de cliente Agiloft](https://www.agiloft.com/support-login.htm) obter esses valores. 

5. Sobre o **certificado de assinatura SAML** secção, clique em **Certificate(Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![O link de download de certificado](./media/agiloft-tutorial/tutorial_agiloft_certificate.png) 

6. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/agiloft-tutorial/tutorial_general_400.png)
    
7. Sobre o **Agiloft configuração** secção, clique em **configurar Agiloft** para abrir **configurar início de sessão** janela. Cópia a **URL de fim de sessão, o ID de entidade de SAML e o SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configuração de Agiloft](./media/agiloft-tutorial/tutorial_agiloft_configure.png) 

8. Numa janela do browser web diferente, inicie sessão no site da sua empresa Agiloft como administrador.

9. Clique em **programa de configuração** (no painel da esquerda) e, em seguida **acesso**.

    ![Configuração de Agiloft](./media/agiloft-tutorial/setup1.png) 

10. Clique no botão **"Configurar SAML 2.0 início de sessão único"**. 
    
    ![Configuração de Agiloft](./media/agiloft-tutorial/setup2.png) 

11. É apresentada uma caixa de diálogo do assistente. Na caixa de diálogo, clique no SEPARADOR **"Detalhes do fornecedor de identidade"** e preencha os campos seguintes:  
    
    ![Configuração de Agiloft](./media/agiloft-tutorial/setup4.png) 

    a. Na **Id de entidade do IdP / emissor** caixa de texto, cole o valor de **ID de entidade de SAML**, que copiou do portal do Azure.

    b. Na **URL de início de sessão do IdP** caixa de texto, cole o valor de **único URL de início de sessão no serviço**, que copiou do portal do Azure.

    c. Na **URL de fim de sessão do IdP** caixa de texto, cole o valor de **URL de fim de sessão**, que copiou do portal do Azure.

    d. Abra sua **certificado com codificação base 64** no bloco de notas transferido a partir do portal do Azure, copie o conteúdo do mesmo para a área de transferência e, em seguida, cole-os para o **conteúdo de certificado X.509 de fornecido IdP** caixa de texto.

    e. Clique em **Concluir**.


> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/agiloft-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/agiloft-tutorial/create_aaduser_02.png)

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/agiloft-tutorial/create_aaduser_03.png)

4. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/agiloft-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-an-agiloft-test-user"></a>Criar um utilizador de teste Agiloft

Aplicativo oferece suporte apenas no tempo de aprovisionamento de utilizador e de utilizadores de autenticação serão criados no aplicativo automaticamente. Não existe nenhuma ação para nesta secção.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para Agiloft.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a Agiloft, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **Agiloft**.

    ![A ligação de Agiloft na lista de aplicações](./media/agiloft-tutorial/tutorial_agiloft_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico Agiloft no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo Agiloft.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/agiloft-tutorial/tutorial_general_01.png
[2]: ./media/agiloft-tutorial/tutorial_general_02.png
[3]: ./media/agiloft-tutorial/tutorial_general_03.png
[4]: ./media/agiloft-tutorial/tutorial_general_04.png

[100]: ./media/agiloft-tutorial/tutorial_general_100.png

[200]: ./media/agiloft-tutorial/tutorial_general_200.png
[201]: ./media/agiloft-tutorial/tutorial_general_201.png
[202]: ./media/agiloft-tutorial/tutorial_general_202.png
[203]: ./media/agiloft-tutorial/tutorial_general_203.png

