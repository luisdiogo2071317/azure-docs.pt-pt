---
title: 'Tutorial: Integração do Azure Active Directory com o ArcGIS Online | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e o ArcGIS Online.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: a9e132a4-29e7-48bf-beb9-4148e617c8a9
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/03/2018
ms.author: jeedes
ms.openlocfilehash: 12ab224481c519db36ae21dd11916649ff0bfbe3
ms.sourcegitcommit: f58fc4748053a50c34a56314cf99ec56f33fd616
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/04/2018
ms.locfileid: "48269039"
---
# <a name="tutorial-azure-active-directory-integration-with-arcgis-online"></a>Tutorial: Integração do Azure Active Directory com o ArcGIS Online

Neste tutorial, saiba como integrar o ArcGIS Online com o Azure Active Directory (Azure AD).

Integrar o ArcGIS Online com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao ArcGIS Online.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para o ArcGIS Online (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o ArcGIS Online, terá dos seguintes itens:

- Uma subscrição do Azure
- Um ArcGIS Online logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste.
O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionar ArcGIS Online a partir da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-arcgis-online-from-the-gallery"></a>Adicionar ArcGIS Online a partir da Galeria

Para configurar a integração do ArcGIS Online com o Azure AD, terá de adicionar ArcGIS Online na Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar o ArcGIS Online a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![image](./media/arcgis-tutorial/selectazuread.png)

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![image](./media/arcgis-tutorial/a_select_app.png)
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![image](./media/arcgis-tutorial/a_new_app.png)

4. Na caixa de pesquisa, escreva **ArcGIS Online**, selecione **ArcGIS Online** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

     ![image](./media/arcgis-tutorial/a_add_app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com o ArcGIS Online com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador equivalente no ArcGIS Online a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no ArcGIS Online tem de ser estabelecida.

No ArcGIS Online, atribuir o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com o ArcGIS Online, terá de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste ArcGIS Online](#create-an-arcgis-online-test-user)**  - para ter um equivalente da Eduarda Almeida ArcGIS online que está ligado à representação de utilizador do Azure AD.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo ArcGIS Online.

**Para configurar o Azure AD início de sessão único com o ArcGIS Online, execute os seguintes passos:**

1. Na [portal do Azure](https://portal.azure.com/), na **ArcGIS Online** página de integração de aplicações, selecione **início de sessão único**.

    ![image](./media/arcgis-tutorial/b1_b2_select_sso.png)

2. Clique em **modo de início de sessão único de alteração** na parte superior do ecrã para selecionar o **SAML** modo.

      ![image](./media/arcgis-tutorial/b1_b2_saml_ssso.png)

3. Sobre o **selecionar um método de início de sessão único** caixa de diálogo, clique em **selecione** para **SAML** modo para ativar o início de sessão único.

    ![image](./media/arcgis-tutorial/b1_b2_saml_sso.png)

4. Sobre o **definido no início de sessão único com o SAML** página, clique em **editar** botão para abrir **configuração básica de SAML** caixa de diálogo.

    ![image](./media/arcgis-tutorial/b1-domains_and_urlsedit.png)

5. Sobre o **configuração básica de SAML** secção, execute os seguintes passos:

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<companyname>.maps.arcgis.com`.

    b. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `<companyname>.maps.arcgis.com`.

    ![image](./media/arcgis-tutorial/b1-domains_and_urls.png)

    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o URL de início de sessão e o identificador real. Contacte [equipa de suporte do cliente Online do ArcGIS](http://support.esri.com/en/) obter esses valores.

6. Sobre o **certificado de assinatura SAML** secção, clique em **transferir** para transferir **XML de metadados de Federação** e, em seguida, guarde o ficheiro xml em seu computador.

    ![image](./media/arcgis-tutorial/federationxml.png)

7. Para automatizar a configuração de dentro **ArcGIS Online**, tem de instalar **segura de aplicações meu início de sessão da extensão de browser** clicando **instalar a extensão**.

    ![image](./media/arcgis-tutorial/install_extension.png)

8. Depois de adicionar a extensão para o navegador, clique em **ArcGIS Online de configuração** irá direcioná-lo para o aplicativo ArcGIS Online. A partir daí, forneça as credenciais de administrador a iniciar sessão em ArcGIS Online. A extensão do browser irá configurar o aplicativo para e automatizar passos 9 13 automaticamente.

9. Se desejar configurar manualmente o ArcGIS Online, abra uma nova janela de browser e inicie sessão no site da sua empresa ArcGIS como um administrador e execute os seguintes passos:

10. Clique em **editar definições**.

    ![Editar definições](./media/arcgis-tutorial/ic784742.png "editar definições")

11. Clique em **segurança**.

    ![Segurança](./media/arcgis-tutorial/ic784743.png "segurança")

12. Sob **inícios de sessão do Enterprise**, clique em **definir o fornecedor de identidade**.

    ![Inícios de sessão empresarial](./media/arcgis-tutorial/ic784744.png "inícios de sessão empresarial")

13. Sobre o **definir o fornecedor de identidade** configuração página, execute os seguintes passos:

    ![Definir o fornecedor de identidade](./media/arcgis-tutorial/ic784745.png "definir o fornecedor de identidade")

    a. Na **nome** caixa de texto, escreva o nome da sua organização.

    b. Para **serão fornecidos metadados para o fornecedor de identidade empresarial utilizando**, selecione **um ficheiro**.

    c. Para carregar o ficheiro de metadados baixado, clique em **Escolher ficheiro**.

    d. Clique em **fornecedor de identidade do conjunto**.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD 

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

1. No portal do Azure, no painel esquerdo, selecione **do Azure Active Directory**, selecione **utilizadores**e, em seguida, selecione **todos os utilizadores**.

    ![image](./media/arcgis-tutorial/d_users_and_groups.png)

2. Selecione **novo utilizador** na parte superior do ecrã.

    ![image](./media/arcgis-tutorial/d_adduser.png)

3. Nas propriedades do utilizador, execute os seguintes passos.

    ![image](./media/arcgis-tutorial/d_userproperties.png)

    a. Na **Name** campo introduza **BrittaSimon**.
  
    b. Na **nome de utilizador** tipo de campo **brittasimon@yourcompanydomain.extension**  
    Por exemplo, BrittaSimon@contoso.com

    c. Selecione **propriedades**, selecione a **palavra-passe de Show** caixa de verificação e, em seguida, anote o valor que é apresentado na caixa de palavra-passe.

    d. Selecione **Criar**.

### <a name="create-an-arcgis-online-test-user"></a>Criar um utilizador de teste ArcGIS Online

Para habilitar os utilizadores do Azure AD iniciar sessão no ArcGIS Online, tem de ser aprovisionados no ArcGIS Online.  
No caso do ArcGIS Online, o aprovisionamento é uma tarefa manual.

**Para Aprovisionar uma conta de utilizador, execute os seguintes passos:**

1. Inicie sessão no seu **ArcGIS** inquilino.

2. Clique em **membros de convite**.
   
    ![Convidar os membros](./media/arcgis-tutorial/ic784747.png "convidar os membros")

3. Selecione **adicionar membros automaticamente sem enviar um e-mail**e, em seguida, clique em **PRÓXIMA**.
   
    ![Adicionar membros automaticamente](./media/arcgis-tutorial/ic784748.png "automaticamente a adicionar membros")

4. Sobre o **membros** caixa de diálogo página, execute os seguintes passos:
   
     ![Adicionar e reveja](./media/arcgis-tutorial/ic784749.png "Add e revisão")
    
     a. Introduza o **E-Mail**, **FirstName**, e **Apelido** de uma conta do AAD válida que pretende aprovisionar.
  
     b. Clique em **adicionar e revisão**.
5. Reveja os dados que introduziu e, em seguida, clique em **adicionar membros**.
   
    ![Adicionar membro](./media/arcgis-tutorial/ic784750.png "Adicionar membro")
        
    > [!NOTE]
    > O titular da conta do Azure Active Directory irá receber um e-mail e seguir uma ligação para confirmar a respetiva conta até se tornar Active Directory.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único, concedendo acesso ao ArcGIS Online.

1. No portal do Azure, selecione **aplicações empresariais**, selecione **todos os aplicativos**.

    ![image](./media/arcgis-tutorial/d_all_applications.png)

2. Na lista de aplicações, selecione **ArcGIS Online**.

    ![image](./media/arcgis-tutorial/d_all_application.png)

3. No menu à esquerda, selecione **utilizadores e grupos**.

    ![image](./media/arcgis-tutorial/d_leftpaneusers.png)

4. Selecione o **Add** botão, em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![image](./media/arcgis-tutorial/d_assign_user.png)

4. Na **utilizadores e grupos** caixa de diálogo select **Eduarda Almeida** na lista de utilizadores, em seguida, clique o **selecionar** na parte inferior do ecrã.

5. Na **adicionar atribuição** caixa de diálogo select a **atribuir** botão.

### <a name="test-single-sign-on"></a>Testar o início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico ArcGIS Online no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo ArcGIS Online.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



