---
title: 'Tutorial: Integração do Azure Active Directory com o Amazon Web Services (AWS) | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e o Amazon Web Services (AWS).
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 7561c20b-2325-4d97-887f-693aa383c7be
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/16/2018
ms.author: jeedes
ms.openlocfilehash: 8e91fbf0befaef9088e9afaa6e69c0cb29ad4858
ms.sourcegitcommit: 6361a3d20ac1b902d22119b640909c3a002185b3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/17/2018
ms.locfileid: "49363764"
---
# <a name="tutorial-azure-active-directory-integration-with-amazon-web-services-aws"></a>Tutorial: Integração do Azure Active Directory com o Amazon Web Services (AWS)

Neste tutorial, saiba como integrar o Amazon Web Services (AWS) com o Azure Active Directory (Azure AD).

Integrar o Amazon Web Services (AWS) com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso para o Amazon Web Services (AWS).
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Amazon Web Services (AWS) (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

![Amazon Web Services (AWS)](./media/amazon-web-service-tutorial/tutorial_amazonwebservices_image.png)

Pode configurar vários identificadores para múltiplas instâncias conforme mostrado a seguir. 

* `https://signin.aws.amazon.com/saml#1`

* `https://signin.aws.amazon.com/saml#2`

Com estes valores, do Azure AD irá remover o valor de **#** e enviar o valor correto `https://signin.aws.amazon.com/saml` como o URL de audiência no SAML Token.

**Recomendamos usar essa abordagem pelos seguintes motivos:**

a. Cada aplicativo fornecerá a o exclusivo de X509 certificado e por isso, cada instância podem ter uma data de expiração de certificado diferente e pode gerenciar isso com base no facto de conta AWS individual. Em geral rollover de certificado será fácil neste caso.

b. Pode ativar o aprovisionamento de utilizadores com aplicação AWS no Azure AD e, em seguida, o nosso serviço irá buscar o todas as funções a partir dessa conta AWS. Não é preciso adicionar manualmente ou atualizar as funções AWS na aplicação.

c. Pode atribuir o proprietário do aplicativo individualmente para a aplicação que pode gerir a aplicação diretamente no Azure AD.

> [!Note]
> Certifique-se de que utilizar apenas as aplicações de galeria

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o Amazon Web Services (AWS), terá dos seguintes itens:

- Uma subscrição do Azure
- Um Amazon Web Services (AWS) início de sessão único de subscrição ativada

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

> [!Note]
> Se pretender integrar várias contas AWS para uma conta do Azure para início de sessão único no, consulte [isso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-aws-multi-accounts-tutorial) artigo.

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando o Amazon Web Services (AWS) da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-amazon-web-services-aws-from-the-gallery"></a>Adicionando o Amazon Web Services (AWS) da Galeria
Para configurar a integração do Amazon Web Services (AWS) para o Azure AD, terá de adicionar Amazon Web Services (AWS) a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Amazon Web Services (AWS) a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![image](./media/amazon-web-service-tutorial/selectazuread.png)

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![image](./media/amazon-web-service-tutorial/a_select_app.png)
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![image](./media/amazon-web-service-tutorial/a_new_app.png)

4. Na caixa de pesquisa, escreva **Amazon Web Services (AWS)**, selecione **Amazon Web Services (AWS)** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

     ![image](./media/amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com o Amazon Web Services (AWS) com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Amazon Web Services (AWS) para um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Amazon Web Services (AWS) deve ser estabelecido.

Para configurar e testar o Azure AD início de sessão único com o Amazon Web Services (AWS), tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste do Amazon Web Services (AWS)](#create-an-amazon-web-services-aws-test-user)**  - para ter um equivalente da Eduarda Almeida na Amazon Web Services (AWS) que está ligado à representação de utilizador do Azure AD.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único na sua aplicação do Amazon Web Services (AWS).

**Para configurar o Azure AD início de sessão único com o Amazon Web Services (AWS), execute os seguintes passos:**

1. Na [portal do Azure](https://portal.azure.com/), na **Amazon Web Services (AWS)** página de integração de aplicações, selecione **início de sessão único**.

    ![image](./media/amazon-web-service-tutorial/B1_B2_Select_SSO.png)

2. Sobre o **selecionar um método de início de sessão único** caixa de diálogo, selecione **SAML** modo para ativar o início de sessão único.

    ![image](./media/amazon-web-service-tutorial/b1_b2_saml_sso.png)

3. Sobre o **definido no início de sessão único com o SAML** página, clique em **editar** botão para abrir **configuração básica de SAML** caixa de diálogo.

    ![image](./media/amazon-web-service-tutorial/b1-domains_and_urlsedit.png)

4. Sobre o **configuração básica de SAML** secção, o utilizador não tem de realizar qualquer passo como a aplicação já está pré-integrada com o Azure.

    ![image](./media/amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_url.png)

5. Quando estiver a configurar mais de uma instância, forneça o valor do identificador. Da segunda instância e posteriores, forneça o valor do identificador no seguinte formato. Utilize um **#** inscrever-se de especificar um valor exclusivo do SPN. 

    `https://signin.aws.amazon.com/saml#2`

    ![URLs de domínio do Amazon Web Services (AWS) e únicas início de sessão em informações](./media/amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_identifier.png)

6. Aplicação do Amazon Web Services (AWS) espera que as asserções SAML num formato específico. Configure as seguintes declarações para esta aplicação. Pode gerir os valores destes atributos do **atributos de utilizador e afirmações** secção na página de integração de aplicações. Sobre o **definido no início de sessão único com o SAML** página, clique em **editar** botão para abrir **atributos de utilizador e afirmações** caixa de diálogo.

    ![image](./media/amazon-web-service-tutorial/i4-attribute.png)

7. No **afirmações de utilizador** secção sobre o **atributos de utilizador e afirmações** caixa de diálogo, configurar o atributo de token de SAML conforme mostrado na imagem acima e execute os seguintes passos:
    
    | Nome  | Atributo de origem  | Espaço de Nomes |
    | --------------- | --------------- | --------------- |
    | RoleSessionName | user.userprincipalname | https://aws.amazon.com/SAML/Attributes |
    | Função            | user.assignedroles |  https://aws.amazon.com/SAML/Attributes |
    | SessionDuration             | "fornecer um valor entre 900 segundos (15 minutos) para 43200 segundos (12 horas)" |  https://aws.amazon.com/SAML/Attributes |

    a. Clique em **Adicionar nova afirmação** para abrir o **afirmações de utilizador de gerir** caixa de diálogo.

    ![image](./media/amazon-web-service-tutorial/i2-attribute.png)

    ![image](./media/amazon-web-service-tutorial/i3-attribute.png)

    b. Na **nome** caixa de texto, escreva o nome de atributo apresentado para essa linha.

    c. Introduza o **espaço de nomes** valor.

    d. Selecione a origem de dado **atributo**.

    e. Partir do **atributo de origem** lista, digite o valor de atributo apresentado para essa linha.

    f. Clique em **Guardar**.

8. No **definido no início de sessão único com o SAML** página, além do **certificado de assinatura SAML** secção, clique em **transferir** para transferir o **XML de metadados de Federação**  e guarde-o no seu computador.

    ![image](./media/amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_certificate.png) 

9. Numa janela do browser diferente, início de sessão no site da sua empresa Amazon Web Services (AWS) como administrador.

10. Clique em **AWS Home**.

    ![Configurar a página inicial do início de sessão único][11]

11. Clique em **gestão de acesso e identidades**.

    ![Configurar a identidade de início de sessão única][12]

12. Clique em **fornecedores de identidade**e, em seguida, clique em **criar fornecedor**.

    ![Configurar o fornecedor de início de sessão único][13]

13. Sobre o **configurar o fornecedor** caixa de diálogo página, execute os seguintes passos:

    ![Configurar o início de sessão único de caixa de diálogo][14]

    a. Como **tipo de fornecedor**, selecione **SAML**.

    b. Na **nome do fornecedor** caixa de texto, escreva um nome de fornecedor (por exemplo: *WAAD*).

    c. A carregar seu transferido **ficheiro de metadados** a partir do portal do Azure, clique em **Escolher ficheiro**.

    d. Clique em **passo seguinte**.

14. Sobre o **Certifique-se de informações do fornecedor** página de diálogo, clique em **criar**.

    ![Configurar o início de sessão único verificar][15]

15. Clique em **funções**e, em seguida, clique em **criar função**.

    ![Configurar funções de início de sessão únicas][16]

16. Sobre o **criar função** página, execute os seguintes passos:  

    ![Configurar a confiança de início de sessão única][19]

    a. Selecione **Federação SAML 2.0** sob **selecione o tipo de entidade fidedigna**.

    b. Sob **escolher uma secção de SAML 2.0 fornecedor**, selecione a **fornecedor de SAML** que criou anteriormente (por exemplo: *WAAD*)

    c. Selecione **permitir programática e o acesso à consola de gestão do AWS**.
  
    d. Clique em **seguinte: permissões**.

17. Sobre o **anexar políticas de permissões** caixa de diálogo, não precisa de anexar qualquer política. Clique em **seguinte: revisão**.  

    ![Configurar a política de início de sessão única][33]

18. Sobre o **revisão** caixa de diálogo, execute os seguintes passos:

    ![Configurar a revisão de início de sessão único][34]

    a. Na **nome da função** caixa de texto, introduza o seu nome de função.

    b. Na **descrição da função** caixa de texto, introduza a descrição.

    c. Clique em **criar função**.

    d. Crie tantas funções conforme necessário e mapeá-los para o fornecedor de identidade.

19. Utilize as credenciais de conta de serviço do AWS para obter as funções da conta AWS no aprovisionamento de utilizador do Azure AD. Para isso, abra a consola AWS doméstica.

20. Clique em **serviços** -> **segurança, identidade e conformidade** -> **IAM**.

    ![a obter as funções do conta AWS](./media/amazon-web-service-tutorial/fetchingrole1.png)

21. Selecione o **políticas** separador na secção IAM.

    ![a obter as funções do conta AWS](./media/amazon-web-service-tutorial/fetchingrole2.png)

22. Criar uma nova política ao clicar em **criar política** para obter as funções da conta AWS no aprovisionamento de utilizador do Azure AD.

    ![Criar nova política](./media/amazon-web-service-tutorial/fetchingrole3.png)

23. Crie a sua política para obter todas as funções das contas AWS, efetuando os seguintes passos:

    ![Criar nova política](./media/amazon-web-service-tutorial/policy1.png)

    a. Na **"Criar política de"** secção, clique em **"JSON"** separador.

    b. No documento da política, adicione o abaixo JSON.

    ```

    {

    "Version": "2012-10-17",

    "Statement": [

    {

    "Effect": "Allow",

    "Action": [

    "iam:ListRoles"

    ],

    "Resource": "*"

    }

    ]

    }

    ```

    c. Clique em **botão rever política** para validar a política.

    ![Definir a nova política](./media/amazon-web-service-tutorial/policy5.png)

24. Definir o **nova política** , efetuando os seguintes passos:

    ![Definir a nova política](./media/amazon-web-service-tutorial/policy2.png)

    a. Forneça o **nome da política** como **AzureAD_SSOUserRole_Policy**.

    b. Pode fornecer **Descrição** para a política conforme **esta política permite para buscar as funções de contas AWS**.

    c. Clique em **"Criar a política"** botão.

25. Crie uma nova conta de utilizador no AWS IAM serviço, efetuando os seguintes passos:

    a. Clique em **utilizadores** navegação na consola do AWS IAM.

    ![Definir a nova política](./media/amazon-web-service-tutorial/policy3.png)

    b. Clique em **adicionar utilizador** botão para criar um novo utilizador.

    ![Adicionar utilizador](./media/amazon-web-service-tutorial/policy4.png)

    c. Na **adicionar utilizador** secção, execute os seguintes passos:

    ![Adicionar utilizador](./media/amazon-web-service-tutorial/adduser1.png)

    * Introduza o nome de utilizador como **AzureADRoleManager**.

    * O tipo de acesso, selecione o **acesso programático** opção. Desta forma, o utilizador pode invocar as APIs e obter as funções de conta AWS.

    * Clique nas **permissões seguinte** botão no canto inferior direito.

26. Agora, crie uma nova política para este utilizador, efetuando os seguintes passos:

    ![Adicionar utilizador](./media/amazon-web-service-tutorial/adduser2.png)

    a. Clique nas **anexar as políticas existentes diretamente** botão.

    b. Pesquisa para a política recentemente criada na secção dos filtros **AzureAD_SSOUserRole_Policy**.

    c. Selecione o **diretiva** e, em seguida, clique no **próximo: revisão** botão.

27. Rever a política para o usuário anexado ao realizar os passos seguintes:

    ![Adicionar utilizador](./media/amazon-web-service-tutorial/adduser3.png)

    a. Reveja o nome de utilizador, o tipo de acesso e a política mapeado para o utilizador.

    b. Clique nas **criar utilizador** botão no canto inferior direito para criar o utilizador.

28. Transferir as credenciais de utilizador de um utilizador, efetuando os seguintes passos:

    ![Adicionar utilizador](./media/amazon-web-service-tutorial/adduser4.png)

    a. Copie o usuário **acesso ID da chave** e **chave de acesso a segredos**.

    b. Introduza estas credenciais para o utilizador do Azure AD aprovisionamento secção para obter as funções a partir da consola do AWS.

    c. Clique em **fechar** na parte inferior.

29. Navegue para **aprovisionamento de utilizadores** secção da aplicação do Amazon Web Services no Portal de gestão do Azure AD.

    ![Adicionar utilizador](./media/amazon-web-service-tutorial/provisioning.png)

30. Introduza o **chave de acesso** e **segredo** no **segredo do cliente** e **segredo de Token** campo, respetivamente.

    ![Adicionar utilizador](./media/amazon-web-service-tutorial/provisioning1.png)

    a. Introduza a chave de acesso de utilizador do AWS no **clientsecret** campo.

    b. Introduza o segredo de utilizador do AWS no **segredo de Token** campo.

    c. Clique nas **Testar ligação** botão e deve conseguir testar esta ligação com êxito.

    d. Guardar a definição ao clicar no **guardar** botão na parte superior.

31. Agora, certifique-se de que ativa o estado de aprovisionamento **no** da secção definições da, fazendo a mudança no e, em seguida, clicando no **guardar** botão na parte superior.

    ![Adicionar utilizador](./media/amazon-web-service-tutorial/provisioning2.png)

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

1. No portal do Azure, no painel esquerdo, selecione **do Azure Active Directory**, selecione **utilizadores**e, em seguida, selecione **todos os utilizadores**.

    ![image](./media/amazon-web-service-tutorial/d_users_and_groups.png)

2. Selecione **novo utilizador** na parte superior do ecrã.

    ![image](./media/amazon-web-service-tutorial/d_adduser.png)

3. Nas propriedades do utilizador, execute os seguintes passos.

    ![image](./media/amazon-web-service-tutorial/d_userproperties.png)

    a. Na **Name** campo introduza **BrittaSimon**.
  
    b. Na **nome de utilizador** tipo de campo **brittasimon@yourcompanydomain.extension**  
    Por exemplo, BrittaSimon@contoso.com

    c. Selecione **propriedades**, selecione a **palavra-passe de Show** caixa de verificação e, em seguida, anote o valor que é apresentado na caixa de palavra-passe.

    d. Selecione **Criar**.
 
### <a name="create-an-amazon-web-services-aws-test-user"></a>Criar um utilizador de teste do Amazon Web Services (AWS)

O objetivo desta secção é criar um utilizador chamado Eduarda Almeida na Amazon Web Services (AWS). Amazon Web Services (AWS) não precisa de um utilizador a ser criado no seu sistema para SSO, por isso não terá de efetuar qualquer ação aqui.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único, concedendo acesso para o Amazon Web Services (AWS).

1. No portal do Azure, selecione **aplicações empresariais**, selecione **todos os aplicativos**.

    ![image](./media/amazon-web-service-tutorial/d_all_applications.png)

2. Na lista de aplicações, selecione **Amazon Web Services (AWS)**.

    ![image](./media/amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_app.png)

3. No menu à esquerda, selecione **utilizadores e grupos**.

    ![image](./media/amazon-web-service-tutorial/d_leftpaneusers.png)

4. Selecione o **Add** botão, em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![image](./media/amazon-web-service-tutorial/d_assign_user.png)

5. Na **utilizadores e grupos** caixa de diálogo select **Eduarda Almeida** na lista de utilizadores, em seguida, clique o **selecionar** na parte inferior do ecrã.

    ![image](./media/amazon-web-service-tutorial/tutorial_amazonwebservices_users.png)

6. Na **selecionar função** caixa de diálogo selecione a função de utilizador adequada na lista, em seguida, clique no **selecione** na parte inferior do ecrã.

    ![image](./media/amazon-web-service-tutorial/tutorial_amazonwebservices_roles.png)

    >[!NOTE]
    >Depois de ativar o aprovisionamento de utilizadores com o aplicativo, deve ter de aguardar durante 30 minutos para obter todas as funções do Amazon Web Services (AWS), em seguida, tem de atualizar a página e, em seguida, enquanto estiver a atribuir a aplicação a utilizadores e grupos, verá as funções para a utilização r.

7. Na **adicionar atribuição** caixa de diálogo select a **atribuir** botão.

    ![image](./media/amazon-web-service-tutorial/tutorial_amazonwebservices_assign.png)
    
### <a name="test-single-sign-on"></a>Testar o início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico do Amazon Web Services (AWS) no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação do Amazon Web Services (AWS).
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[11]: ./media/amazon-web-service-tutorial/ic795031.png
[12]: ./media/amazon-web-service-tutorial/ic795032.png
[13]: ./media/amazon-web-service-tutorial/ic795033.png
[14]: ./media/amazon-web-service-tutorial/ic795034.png
[15]: ./media/amazon-web-service-tutorial/ic795035.png
[16]: ./media/amazon-web-service-tutorial/ic795022.png
[17]: ./media/amazon-web-service-tutorial/ic795023.png
[18]: ./media/amazon-web-service-tutorial/ic795024.png
[19]: ./media/amazon-web-service-tutorial/ic795025.png
[32]: ./media/amazon-web-service-tutorial/ic7950251.png
[33]: ./media/amazon-web-service-tutorial/ic7950252.png
[35]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning.png
[34]: ./media/amazon-web-service-tutorial/ic7950253.png
[36]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_securitycredentials.png
[37]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_securitycredentials_continue.png
[38]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_createnewaccesskey.png
[39]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_automatic.png
[40]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_testconnection.png
[41]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_on.png
