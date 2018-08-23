---
title: 'Tutorial: Integração do Azure Active Directory com a Sandbox da Salesforce | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e o Salesforce Sandbox.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: ee54c39e-ce20-42a4-8531-da7b5f40f57c
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/07/2018
ms.author: jeedes
ms.openlocfilehash: 6feafba41cf65a752dd5bf0819b0b93bacff0aff
ms.sourcegitcommit: 1af4bceb45a0b4edcdb1079fc279f9f2f448140b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/09/2018
ms.locfileid: "42055149"
---
# <a name="tutorial-azure-active-directory-integration-with-salesforce-sandbox"></a>Tutorial: Integração do Azure Active Directory com a Sandbox de Salesforce

Neste tutorial, saiba como integrar o Sandbox de Salesforce com o Azure Active Directory (Azure AD).

Áreas de segurança dão-lhe a capacidade de criar várias cópias da sua organização em ambientes separados para diversas finalidades, como o desenvolvimento, teste e treinamento, sem comprometer os dados e aplicativos em sua produção de Salesforce organização.
Para obter mais detalhes, consulte [descrição geral de Sandbox](https://help.salesforce.com/articleView?id=create_test_instance.htm&language=en_us&type=5).

Integrar o Salesforce Sandbox no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Sandbox do Salesforce.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para a área de segurança do Salesforce (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com a Sandbox da Salesforce, terá dos seguintes itens:

- Uma subscrição do Azure
- Uma área de segurança do Salesforce logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionar Sandbox do Salesforce a partir da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-salesforce-sandbox-from-the-gallery"></a>Adicionar Sandbox do Salesforce a partir da Galeria

Para configurar a integração de Salesforce Sandbox para o Azure AD, terá de adicionar área de segurança do Salesforce a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar área de segurança do Salesforce a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]

3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

4. Na caixa de pesquisa, escreva **Salesforce Sandbox**, selecione **Sandbox de Salesforce** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Sandbox de Salesforce na lista de resultados](./media/salesforce-sandbox-tutorial/tutorial_salesforcesandbox_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com a Sandbox da Salesforce com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Salesforce Sandbox para um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado na Sandbox do Salesforce tem de ser estabelecida.

Na área de segurança do Salesforce, atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com a Sandbox da Salesforce, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste de área de segurança do Salesforce](#create-a-salesforce-sandbox-test-user)**  - para ter um equivalente da Eduarda Almeida na Sandbox do Salesforce que está ligado à representação de utilizador do Azure AD.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo de área de segurança do Salesforce.

**Para configurar o Azure AD início de sessão único com a Sandbox da Salesforce, execute os seguintes passos:**

1. No portal do Azure, sobre o **Salesforce Sandbox** página de integração de aplicativo, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.

    ![Caixa de diálogo de início de sessão único](./media/salesforce-sandbox-tutorial/tutorial_salesforcesandbox_samlbase.png)

3. Sobre o **Salesforce Sandbox domínio e URLs** secção, execute os seguintes passos, se desejar configurar a aplicação no **IDP** iniciada pelo modo:

   ![Domínio de Sandbox do Salesforce e URLs únicas início de sessão em informações](./media/salesforce-sandbox-tutorial/tutorial_salesforcesandbox_url1.png)

   Na **URL de resposta** caixa de texto, digite seu específicos da organização **URL de resposta**.

   > [!NOTE]
   > Atualize o valor de URL de resposta com o URL de resposta real que é explicado posteriormente neste tutorial.

4. Sobre o **certificado de assinatura SAML** secção, clique em **Certificate(RAW)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![O link de download de certificado](./media/salesforce-sandbox-tutorial/tutorial_salesforcesandbox_certificate.png)

5. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/salesforce-sandbox-tutorial/tutorial_general_400.png)

6. Na **configuração do Salesforce Sandbox** secção, clique em **configurar Sandbox de Salesforce** para abrir **configurar início de sessão** janela. Cópia a **ID de entidade de SAML e SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configurar o início de sessão único](./media/salesforce-sandbox-tutorial/tutorial_salesforcesandbox_configure.png)

7. Abra um novo separador no seu browser e inicie sessão sua conta de administrador da área de segurança do Salesforce.

8. Clique nas **programa de configuração** sob **ícone de definições** no canto superior direito da página.

    ![Configurar o início de sessão único](./media/salesforce-sandbox-tutorial/configure1.png)

9. Desloque para baixo para o **configurações** no painel de navegação esquerdo, clique em **identidade** para expandir a secção relacionada. Em seguida, clique em **definições de início de sessão único**.

    ![Configurar o início de sessão único](./media/salesforce-sandbox-tutorial/sf-admin-sso.png)

10. Sobre o **definições de início de sessão único** página, clique no **editar** botão.

    ![Configurar o início de sessão único](./media/salesforce-sandbox-tutorial/configure3.png)

11. Selecione **ativada SAML**e, em seguida, clique em **guardar**.

    ![Configurar o início de sessão único](./media/salesforce-sandbox-tutorial/sf-enable-saml.png)

12. Para configurar o seu únicas início de sessão em definições de SAML, clique em **New**.

    ![Configurar o início de sessão único](./media/salesforce-sandbox-tutorial/sf-admin-sso-new.png)

13. Sobre o **definições de início de sessão único** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/salesforce-sandbox-tutorial/sf-saml-config1.png)

    a. Selecione **SAML ativada** caixa de verificação.

    b. Na **emissor** campo, cole o valor de **ID de entidade de SAML**, que copiou do portal do Azure.

    c. Para carregar os **certificado do fornecedor de identidade**, clique em **procurar** para procurar e selecionar o ficheiro de certificado que transferiu a partir do portal do Azure.

    d. Na **URL de início de sessão do fornecedor de identidade** caixa de texto, cole o valor de **único URL de início de sessão no serviço**, que copiou do portal do Azure.

    e. Como **tipo de identidade de SAML**, escolha uma das seguintes opções:

      * Selecione **asserção contém o nome do usuário Salesforce**, se o nome de utilizador do utilizador Salesforce está sendo passado na asserção de SAML

      * Selecione **asserção contém o ID de Federação do objeto de utilizador**, se o ID de Federação do objeto de utilizador que está sendo passado na asserção de SAML
  
    f. Como **localização de identidade de SAML**, selecione **identidade é um elemento de atributo**.

    g. SFDC não suporta a fim de sessão SAML.  Como solução, cole `https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0` para o **URL de fim de sessão personalizada** caixa de texto.

    h. Clique em **Guardar**.

14. Sobre o **definições de início de sessão único** página, clique no **transferir metadados** botão.

    ![Configurar o início de sessão único](./media/salesforce-sandbox-tutorial/configure4.png)

15. Abra os metadados baixado numa janela de browser diferente e copiar o **localização** valor e cole-o no **URL de resposta** caixa de texto a **Salesforce Sandbox domínio e URLs**secção no portal do Azure.  

    ![Configurar o início de sessão único](./media/salesforce-sandbox-tutorial/configure5.png)

16. Se desejar configurar a aplicação no **SP** modo iniciado, a seguir são os pré-requisitos para isso:

    a. Deve ter um domínio verificado.

    b. Terá de configurar e ativar o seu domínio na Sandbox do Salesforce, passos para isso são explicados posteriormente neste tutorial.

    c. No portal do Azure, sobre o **Salesforce Sandbox domínio e URLs** secção, verificação **Mostrar definições de URL avançadas** e executar o passo seguinte:
  
    ![Domínio de Sandbox do Salesforce e URLs únicas início de sessão em informações](./media/salesforce-sandbox-tutorial/tutorial_salesforcesandbox_url.png)

    Na **URL de início de sessão** caixa de texto, digite o valor usando o seguinte padrão: `https://<instancename>--Sandbox.<entityid>.my.salesforce.com`

    > [!NOTE]
    > Este valor deve ser copiado a partir do portal do Salesforce Sandbox assim que tiver ativado o domínio.

17. Sobre o **certificado de assinatura SAML** secção, clique em **Certificate(RAW)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![O link de download de certificado](./media/salesforce-sandbox-tutorial/tutorial_salesforcesandbox_certificate.png)

18. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/salesforce-sandbox-tutorial/tutorial_general_400.png)

19. Na **configuração do Salesforce Sandbox** secção, clique em **configurar Sandbox de Salesforce** para abrir **configurar início de sessão** janela. Cópia a **ID de entidade de SAML e SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configurar o início de sessão único](./media/salesforce-sandbox-tutorial/tutorial_salesforcesandbox_configure.png)

20. Abra um novo separador no seu browser e inicie sessão sua conta de administrador da área de segurança do Salesforce.

21. Clique nas **programa de configuração** sob **ícone de definições** no canto superior direito da página.

    ![Configurar o início de sessão único](./media/salesforce-sandbox-tutorial/configure1.png)

22. Desloque para baixo para o **configurações** no painel de navegação esquerdo, clique em **identidade** para expandir a secção relacionada. Em seguida, clique em **definições de início de sessão único**.

    ![Configurar o início de sessão único](./media/salesforce-sandbox-tutorial/sf-admin-sso.png)

23. Sobre o **definições de início de sessão único** página, clique no **editar** botão.

    ![Configurar o início de sessão único](./media/salesforce-sandbox-tutorial/configure3.png)

24. Selecione **ativada SAML**e, em seguida, clique em **guardar**.

    ![Configurar o início de sessão único](./media/salesforce-sandbox-tutorial/sf-enable-saml.png)

25. Para configurar o seu únicas início de sessão em definições de SAML, clique em **New**.

    ![Configurar o início de sessão único](./media/salesforce-sandbox-tutorial/sf-admin-sso-new.png)

26. Se estiver a adicionar uma segunda instância, tem de ativar um domínio, como mencionado acima (SP iniciada caso). Na secção de SAML único configurações de logon, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/salesforce-sandbox-tutorial/sf-saml-config.png)

    a. Na **Name** caixa de texto, escreva o nome da configuração (por exemplo: *SPSSOWAAD_Test*).

    b. Na **emissor** campo, cole o valor de **ID de entidade de SAML**, que copiou do portal do Azure.

    c. Na **ID de entidade** caixa de texto, utilize `https://test.salesforce.com` valor para a primeira instância e da segunda instância da aplicação pode utilizar o valor de identificador específico de inquilino.

    d. Para carregar os **certificado do fornecedor de identidade**, clique em **Escolher ficheiro** para procurar e selecionar o ficheiro de certificado que transferiu a partir do portal do Azure.

    e. Como **tipo de identidade de SAML**, escolha uma das seguintes opções:

      * Selecione **asserção contém o nome do usuário Salesforce**, se o nome de utilizador do utilizador Salesforce está sendo passado na asserção de SAML

      * Selecione **asserção contém o ID de Federação do objeto de utilizador**, se o ID de Federação do objeto de utilizador que está sendo passado na asserção de SAML

      * Selecione **asserção contém o ID de uso do objeto de utilizador**, se o ID de utilizador do objeto de utilizador que está sendo passado na asserção de SAML

    f. Como **localização de identidade de SAML**, selecione **identidade é no elemento NameIdentifier da declaração de assunto**.

    g. Como **fornecedor iniciada solicitar vínculo de serviço**, selecione **HTTP POST**.

    h. Na **URL de início de sessão do fornecedor de identidade** caixa de texto, cole o valor de **único URL de início de sessão no serviço**, que copiou do portal do Azure.

    i. SFDC não suporta a fim de sessão SAML.  Como solução, cole `https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0` -o para o **URL de fim de sessão personalizada** caixa de texto.

    j. Clique em **Guardar**.

27. Para ativar o seu domínio na Sandbox do Salesforce, execute os seguintes passos:

    > [!NOTE]
    > Antes de ativar o domínio tem de criar o mesmo na área de segurança do Salesforce. Para obter mais informações, consulte [definir seu nome de domínio](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_define.htm&language=en_US). Depois de criar o domínio, certifique-se de que está configurado corretamente.

    * No painel de navegação à esquerda na Sandbox do Salesforce, clique em **definições da empresa** para expandir a secção relacionada e, em seguida, clique em **meu domínio**.

         ![Configurar o início de sessão único](./media/salesforce-sandbox-tutorial/sf-my-domain.png)

    * Na **configuração da autenticação** secção, clique em **editar**.

        ![Configurar o início de sessão único](./media/salesforce-sandbox-tutorial/sf-edit-auth-config.png)

    * Na **configuração da autenticação** secção, como **serviço de autenticação**, selecione o nome SAML único início de sessão da definição de que definiu durante a configuração de SSO na Sandbox do Salesforce e Clique em **guardar**.

        ![Configurar o início de sessão único](./media/salesforce-sandbox-tutorial/configure2.png)

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/salesforce-sandbox-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/salesforce-sandbox-tutorial/create_aaduser_02.png)

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/salesforce-sandbox-tutorial/create_aaduser_03.png)

4. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/salesforce-sandbox-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.

### <a name="create-a-salesforce-sandbox-test-user"></a>Criar um utilizador de teste de Sandbox de Salesforce

Nesta secção, um usuário chamado Eduarda Almeida é criado na área de segurança do Salesforce. Sandbox de Salesforce suporta o aprovisionamento de just-in-time, que está ativado por predefinição. Não existe nenhum item de ação para nesta secção. Se um utilizador já não existir na área de segurança do Salesforce, uma nova é criada quando tentar acessar a área de segurança do Salesforce. Sandbox de Salesforce também suporta o aprovisionamento automático de utilizadores, pode encontrar mais detalhes [aqui](salesforce-sandbox-provisioning-tutorial.md) sobre como configurar o aprovisionamento automático de utilizadores.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida a utilizar do Azure-início de sessão único, conceder acesso à área de segurança do Salesforce.

![Atribuir a função de utilizador][200]

**Para atribuir a Eduarda Almeida a área de segurança do Salesforce, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **Salesforce Sandbox**.

    ![A ligação de área de segurança do Salesforce na lista de aplicações](./media/salesforce-sandbox-tutorial/tutorial_salesforcesandbox_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar o início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico de área de segurança do Salesforce no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo de área de segurança do Salesforce.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)
* [Configurar o aprovisionamento do utilizador](salesforce-sandbox-provisioning-tutorial.md)


<!--Image references-->

[1]: ./media/salesforce-sandbox-tutorial/tutorial_general_01.png
[2]: ./media/salesforce-sandbox-tutorial/tutorial_general_02.png
[3]: ./media/salesforce-sandbox-tutorial/tutorial_general_03.png
[4]: ./media/salesforce-sandbox-tutorial/tutorial_general_04.png

[100]: ./media/salesforce-sandbox-tutorial/tutorial_general_100.png

[200]: ./media/salesforce-sandbox-tutorial/tutorial_general_200.png
[201]: ./media/salesforce-sandbox-tutorial/tutorial_general_201.png
[202]: ./media/salesforce-sandbox-tutorial/tutorial_general_202.png
[203]: ./media/salesforce-sandbox-tutorial/tutorial_general_203.png