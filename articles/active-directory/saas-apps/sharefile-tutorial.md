---
title: 'Tutorial: Integração do Azure Active Directory com o Citrix ShareFile | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e o Citrix ShareFile.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: e14fc310-bac4-4f09-99ef-87e5c77288b6
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2018
ms.author: jeedes
ms.openlocfilehash: e27a1c834c48b640ab5ed7ab8d6e54f7d1784abd
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39045945"
---
# <a name="tutorial-azure-active-directory-integration-with-citrix-sharefile"></a>Tutorial: Integração do Azure Active Directory com o Citrix ShareFile

Neste tutorial, saiba como integrar o Citrix ShareFile com o Azure Active Directory (Azure AD).

Integrar o Citrix ShareFile no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Citrix ShareFile.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para o Citrix ShareFile (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o Citrix ShareFile, terá dos seguintes itens:

- Uma subscrição do Azure AD
- O Citrix ShareFile logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionar o Citrix ShareFile a partir da Galeria
2. Configurar e testar o Azure AD início de sessão único

## <a name="add-citrix-sharefile-from-the-gallery"></a>Adicionar o Citrix ShareFile a partir da Galeria
Para configurar a integração entre o Citrix ShareFile com o Azure AD, terá de adicionar o Citrix ShareFile a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar o Citrix ShareFile a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

4. Na caixa de pesquisa, escreva **Citrix ShareFile**, selecione **o Citrix ShareFile** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![O Citrix ShareFile na lista de resultados](./media/sharefile-tutorial/tutorial_sharefile_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com o Citrix ShareFile com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Citrix ShareFile a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Citrix ShareFile deve ser estabelecido.

O Citrix ShareFile, atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com o Citrix ShareFile, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste o Citrix ShareFile](#create-a-citrix-sharefile-test-user)**  - para ter um equivalente da Eduarda Almeida na Citrix ShareFile que está ligado à representação de utilizador do Azure AD.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único na sua aplicação Citrix ShareFile.

**Para configurar o Azure AD início de sessão único com o Citrix ShareFile, execute os seguintes passos:**

1. No portal do Azure, sobre o **Citrix ShareFile** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/sharefile-tutorial/tutorial_sharefile_samlbase.png)

3. Sobre o **Citrix ShareFile domínio e URLs** secção, execute os seguintes passos:

    ![Citrix ShareFile domínio e URLs únicas início de sessão em informações](./media/sharefile-tutorial/tutorial_sharefile_url.png)
    
    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<tenant-name>.sharefile.com/saml/login`

    b. Na **identificador (ID de entidade)** caixa de texto, escreva um URL com o seguinte padrão:

    | |
    |---|
    | `https://<tenant-name>.sharefile.com`|
    | `https://<tenant-name>.sharefile.com/saml/info`|
    | `https://<tenant-name>.sharefile1.com/saml/info`|
    | `https://<tenant-name>.sharefile1.eu/saml/info`|
    | `https://<tenant-name>.sharefile.eu/saml/info`|
    | |
    
    c. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão:
    | |
    |---|
    | `https://<tenant-name>.sharefile.com/saml/acs`|
    | `https://<tenant-name>.sharefile.eu/saml/<URL path>`|
    | `https://<tenant-name>.sharefile.com/saml/<URL path>`|
    | |

    > [!NOTE]
    > Estes valores não são reais. Atualize estes valores com o URL de início de sessão, identificador e o URL de resposta real. Contacte [equipa de suporte de cliente do Citrix ShareFile](https://www.citrix.co.in/products/sharefile/support.html) obter esses valores.

4. Sobre o **certificado de assinatura SAML** secção, clique em **certificado (Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![O link de download de certificado](./media/sharefile-tutorial/tutorial_sharefile_certificate.png)

5. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/sharefile-tutorial/tutorial_general_400.png)

6. Sobre o **configuração da Citrix ShareFile** secção, clique em **configurar o Citrix ShareFile** para abrir **configurar início de sessão** janela. Cópia a **URL de fim de sessão, o ID de entidade de SAML e o SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![O Citrix ShareFile configuração](./media/sharefile-tutorial/tutorial_sharefile_configure.png)

7. Numa janela do browser web diferente, inicie sessão no seu **Citrix ShareFile** site da empresa como administrador.

8. Na barra de ferramentas na parte superior, clique em **administrador**.

9. No painel de navegação esquerdo, selecione **configurar o início de sessão único**.
   
    ![Administração de contas](./media/sharefile-tutorial/ic773627.png "administração de contas")

10. Sobre o **Single Sign-On / SAML 2.0 Configuration** página de diálogo em **definições básicas**, execute os seguintes passos:
   
    ![Início de sessão único](./media/sharefile-tutorial/ic773628.png "início de sessão único")
   
    a. Clique em **ativar SAML**.
    
    b. Na **Your IDP emissor / ID de entidade** caixa de texto, cole o valor de **ID de entidade de SAML** que copiou do portal do Azure.

    c. Clique em **alteração** junto a **certificado X.509** campo e, em seguida, carregue o certificado que transferiu a partir do portal do Azure.
    
    d. Na **URL de início de sessão** caixa de texto, cole o valor de **SAML único início de sessão no URL do serviço** que copiou do portal do Azure.
    
    e. Na **URL de fim de sessão** caixa de texto, cole o valor de **URL de fim de sessão** que copiou do portal do Azure.

11. Clique em **guardar** no portal de gestão do Citrix ShareFile.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/sharefile-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/sharefile-tutorial/create_aaduser_02.png)

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/sharefile-tutorial/create_aaduser_03.png)

4. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/sharefile-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-citrix-sharefile-test-user"></a>Criar um utilizador de teste o Citrix ShareFile

Para habilitar os utilizadores do Azure AD iniciar sessão no Citrix ShareFile, tem de ser aprovisionados no Citrix ShareFile. No caso do Citrix ShareFile, o aprovisionamento é uma tarefa manual.

**Para Aprovisionar uma conta de utilizador, execute os seguintes passos:**

1. Inicie sessão no seu **Citrix ShareFile** inquilino.

2. Clique em **gerir utilizadores \> gerir utilizadores Home \> + criar funcionário**.
   
   ![Criar o funcionário](./media/sharefile-tutorial/IC781050.png "criar funcionário")

3. Sobre o **informações básicas** secção, executar passos abaixo:
   
   ![Informações básicas](./media/sharefile-tutorial/IC799951.png "informações básicas")
   
   a. Na **endereço de E-Mail** caixa de texto, escreva o endereço de e-mail da Eduarda Almeida como **brittasimon@contoso.com**.
   
   b. Na **nome próprio** caixa de texto, tipo **nome próprio** do utilizador como **Eduarda**.
   
   c. Na **sobrenome** caixa de texto, tipo **Apelido** do utilizador como **Simon**.

4. Clique em **adicionar utilizador**.
  
   >[!NOTE]
   >O titular da conta do Azure AD irá receber um e-mail e seguir uma ligação para confirmar a respetiva conta até se tornar Active Directory. Pode utilizar quaisquer outras ferramentas de criação de conta de utilizador o Citrix ShareFile ou APIs fornecidas pelo Citrix ShareFile para aprovisionar contas de utilizador do Azure AD.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único, concedendo acesso para o Citrix ShareFile.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a Citrix ShareFile, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **Citrix ShareFile**.

    ![A ligação de Citrix ShareFile na lista de aplicações](./media/sharefile-tutorial/tutorial_sharefile_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico o Citrix ShareFile no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação Citrix ShareFile.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/sharefile-tutorial/tutorial_general_01.png
[2]: ./media/sharefile-tutorial/tutorial_general_02.png
[3]: ./media/sharefile-tutorial/tutorial_general_03.png
[4]: ./media/sharefile-tutorial/tutorial_general_04.png

[100]: ./media/sharefile-tutorial/tutorial_general_100.png

[200]: ./media/sharefile-tutorial/tutorial_general_200.png
[201]: ./media/sharefile-tutorial/tutorial_general_201.png
[202]: ./media/sharefile-tutorial/tutorial_general_202.png
[203]: ./media/sharefile-tutorial/tutorial_general_203.png
