---
title: "Tutorial: Integração do Azure Active Directory com o Amazon Web Services (AWS) | Microsoft Docs"
description: "Saiba como configurar o início de sessão entre o Azure Active Directory e o Amazon Web Services (AWS)."
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
ms.date: 1/16/2017
ms.author: jeedes
ms.openlocfilehash: 8d77215fd2923e22a9cc87e469cb135d035d22d9
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/19/2018
---
# <a name="tutorial-azure-active-directory-integration-with-amazon-web-services-aws"></a>Tutorial: Integração do Azure Active Directory com o Amazon Web Services (AWS)

Neste tutorial, irá aprender a integrar o Amazon Web Services (AWS) com o Azure Active Directory (Azure AD).

Integrar o Amazon Web Services (AWS) com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso para o Amazon Web Services (AWS).
- Pode permitir aos utilizadores automaticamente obter com sessão iniciada para Amazon Web Services (AWS) (Single Sign-On) com as respetivas contas do Azure AD.
- Pode gerir as contas numa localização central - portal do Azure.

Se pretender saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [que é o acesso a aplicações e início de sessão no Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o Amazon Web Services (AWS), terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Amazon Web Services (AWS)-início de sessão único ativada subscrição

> [!NOTE]
> Para testar os passos neste tutorial, não recomendamos a utilização num ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, teste do Azure AD-início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos modulares principais:

1. Adicionar Amazon Web Services (AWS) a partir da Galeria
2. Configurar e testar o Azure AD de sessão único-

## <a name="adding-amazon-web-services-aws-from-the-gallery"></a>Adicionar Amazon Web Services (AWS) a partir da Galeria
Para configurar a integração do Amazon Web Services (AWS) com o Azure AD, terá de adicionar Amazon Web Services (AWS) a partir da Galeria à sua lista de aplicações SaaS geridas.

**Para adicionar Amazon Web Services (AWS) a partir da galeria, execute os seguintes passos:**

1. No  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **do Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todas as aplicações**.

    ![O painel de aplicações da empresa][2]
    
3. Para adicionar a nova aplicação, clique em **nova aplicação** botão no topo da caixa de diálogo.

    ![O novo botão de aplicação][3]

4. Na caixa de pesquisa, escreva **Amazon Web Services (AWS)**, selecione **Amazon Web Services (AWS)** partir do painel de resultados, em seguida, clique em **adicionar** botão para adicionar a aplicação.

    ![Amazon Web Services (AWS) na lista de resultados](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD-início de sessão único

Nesta secção, configure e teste do Azure AD-início de sessão único com o Amazon Web Services (AWS) com base num utilizador de teste chamado "Britta Simon".

Para início de sessão trabalhar, do Azure AD tem de saber o que o utilizador homólogo nos Amazon Web Services (AWS) é um utilizador no Azure AD. Por outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado nos Amazon Web Services (AWS) tem de ser estabelecida.

Nos Amazon Web Services (AWS), atribua o valor do **nome de utilizador** no Azure AD como o valor a **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD-início de sessão único com o Amazon Web Services (AWS), tem de concluir os blocos modulares seguintes:

1. **[Configurar o Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)**  - para permitir aos utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD-início de sessão único com Britta Simon.
3. **[Criar um utilizador de teste do Amazon Web Services (AWS)](#create-an-amazon-web-services-aws-test-user)**  - para ter um homólogo de Britta Simon nos Amazon Web Services (AWS) que está ligada a representação do Azure AD do utilizador.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar Britta Simon utilizar o Azure AD-início de sessão único.
5. **[Teste o início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD-início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão no portal do Azure e configurar o início de sessão único na sua aplicação Amazon Web Services (AWS).

**Para configurar o Azure AD-início de sessão único com o Amazon Web Services (AWS), execute os seguintes passos:**

1. No portal do Azure, no **Amazon Web Services (AWS)** página de integração de aplicações, clique em **de sessão único-**.

    ![Configurar a ligação de início de sessão único][4]

2. No **de sessão único-** caixa de diálogo, selecione **modo** como **baseados em SAML início de sessão** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_samlbase.png)

3. No **Amazon Web Services (AWS) domínios e URLs** secção, o utilizador não tem de efetuar quaisquer passos, tal como a aplicação já está pré-integrada com o Azure.

    ![Domínio Amazon Web Services (AWS) e os URLs únicos de informações de início de sessão](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_url.png)

4. A aplicação de Software do Amazon Web Services (AWS) espera as asserções de SAML num formato específico. Configure as seguintes afirmações para esta aplicação. Pode gerir os valores destes atributos a partir de "**atributos de utilizador**" secção na página de integração de aplicações. A seguinte captura de ecrã mostra um exemplo para este.

    ![Configurar attb Single Sign-On](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_attribute.png)   

5. No **atributos de utilizador** secção no **de sessão único-** caixa de diálogo, configurar atributos token SAML, conforme mostrado na imagem acima e execute os seguintes passos:
    
    | Nome do Atributo  | Valor do Atributo | Espaço de Nomes |
    | --------------- | --------------- | --------------- |
    | RoleSessionName | user.userprincipalname | https://aws.amazon.com/SAML/Attributes |
    | Função            | user.assignedroles |  https://aws.amazon.com/SAML/Attributes |
    
    >[!TIP]
    >Terá de configurar o aprovisionamento de utilizador no Azure AD para obter todas as funções a partir da consola AWS. Consulte os passos de aprovisionamento abaixo.

    a. Clique em **adicionar atributo** para abrir o **adicionar atributo** caixa de diálogo.

    ![Configurar o início de sessão único adicionar](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_attribute_04.png)

    ![Configurar addattb Single Sign-On](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_attribute_05.png)

    b. No **nome** caixa de texto, escreva o nome de atributo apresentado para essa linha.

    c. Do **valor** lista, digite o valor de atributo apresentado para essa linha.

    d. No **espaço de nomes** caixa de texto, digite o valor do espaço de nomes mostrado para essa linha.
    
    d. Clique em **OK**.

6. No **certificado de assinatura de SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![A hiperligação de transferência do certificado](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_certificate.png) 

7. Clique em **guardar** botão.

    ![Configurar botão único início de sessão guardar](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_400.png)

8. Numa janela do browser diferente, início de sessão no site da sua empresa Amazon Web Services (AWS) como administrador.

9. Clique em **consola Home**.
   
    ![Configurar a home page do Single Sign-On][11]

10. Clique em **identidade e gestão de acesso**. 
   
    ![Configurar a identidade de início de sessão único][12]

11. Clique em **fornecedores de identidade**e, em seguida, clique em **criar fornecedor**. 
   
    ![Configurar o fornecedor de início de sessão único][13]

12. No **Configurar fornecedor** diálogo página, execute os seguintes passos: 
   
    ![Configurar o início de sessão único de caixa de diálogo][14]
 
    a. Como **tipo de fornecedor**, selecione **SAML**.

    b. No **nome do fornecedor** caixa de texto, escreva um nome de fornecedor (por exemplo: *WAAD*).

    c. Para carregar o transferido **ficheiro de metadados** no portal do Azure, clique em **Escolher ficheiro**.

    d. Clique em **passo seguinte**.

13. No **verificar informações do fornecedor** página da caixa de diálogo, clique em **criar**. 
    
    ![Configurar o início de sessão único verificar][15]

14. Clique em **funções**e, em seguida, clique em **criar nova função**. 
    
    ![Configurar funções de início de sessão único][16]

15. No **definir o nome da função** caixa de diálogo, execute os seguintes passos: 
    
    ![Configurar o nome de início de sessão único][17] 

    a. No **nome da função** caixa de texto, escreva um nome de função (por exemplo: *TestUser*). 

    b. Clique em **passo seguinte**.

16. No **selecionar tipo de função** caixa de diálogo, execute os seguintes passos: 
    
    ![Configurar o tipo de função de início de sessão único][18] 

    a. Selecione **função para acesso do fornecedor de identidade**. 

    b. No **acesso conceder Web Single Sign-On (WebSSO) para fornecedores de SAML** secção, clique em **selecione**.

17. No **estabelecer confiança** caixa de diálogo, execute os seguintes passos:  
    
    ![Configurar a confiança de início de sessão único][19] 

    a. Como fornecedor SAML, selecione o fornecedor SAML que criou anteriormente (por exemplo: *WAAD*) 
  
    b. Clique em **passo seguinte**.

18. No **verificar função confiar** caixa de diálogo, clique em **passo seguinte**. 
    
    ![Configurar a confiança da função de início de sessão único][32]

19. No **anexar política** caixa de diálogo, clique em **passo seguinte**.  
    
    ![Configurar a política de início de sessão único][33]

20. No **revisão** caixa de diálogo, execute os seguintes passos:   
    
    ![Configurar a revisão de início de sessão único][34] 

    a. Clique em **criar função**.

    b. Criar funções tantos conforme necessário e mapeá-los para o fornecedor de identidade.

21. Utilize credenciais de conta de serviço do AWS para obter as funções da conta AWS no aprovisionamento de utilizador do Azure AD. Para tal, abra a consola do AWS doméstica.

22. Clique em **serviços** -> **segurança, de identidade e de conformidade** -> **IAM**.

    ![obter as funções da conta AWS](./media/active-directory-saas-amazon-web-service-tutorial/fetchingrole1.png)

23. Selecione o **políticas** separador na secção IAM.

    ![obter as funções da conta AWS](./media/active-directory-saas-amazon-web-service-tutorial/fetchingrole2.png)

24. Crie uma nova política ao clicar no **criar política**.

    ![Criar nova política](./media/active-directory-saas-amazon-web-service-tutorial/fetchingrole3.png)
 
25. Crie a sua própria política para obter todas as funções de contas do AWS, efetuando os seguintes passos:

    ![Criar nova política](./media/active-directory-saas-amazon-web-service-tutorial/policy1.png)

    a. No **"Criar política"** secção, clique em **"JSON"** separador.

    b. O documento de política, adicione o abaixo JSON.
    
    ```
    
    {

    "Version": "2012-10-17",

    "Statement": [

    {

    "Effect": "Allow",
        
    "Action": [
        
    "iam: ListRoles"
        
    ],

    "Resource": "*"

    }

    ]

    }
    
    ```

    c. Clique em **botão de política de revisão** para validar a política.

    ![Definir a nova política](./media/active-directory-saas-amazon-web-service-tutorial/policy5.png)

26. Definir o **nova política** efetuando os seguintes passos:

    ![Definir a nova política](./media/active-directory-saas-amazon-web-service-tutorial/policy2.png)

    a. Forneça o **nome da política** como **AzureAD_SSOUserRole_Policy**.

    b. Pode fornecer **Descrição** para a política conforme **esta política irá permitir ao obter as funções do contas AWS**.
    
    c. Clique em **"Criar a política"** botão.
        
27. Crie uma nova conta de utilizador no serviço de IAM AWS, efetuando os seguintes passos:

    a. Clique em **utilizadores** navegação na consola do AWS IAM.

    ![Definir a nova política](./media/active-directory-saas-amazon-web-service-tutorial/policy3.png)
    
    b. Clique em **adicionar utilizador** botão para criar um novo utilizador.

    ![Adicionar utilizador](./media/active-directory-saas-amazon-web-service-tutorial/policy4.png)

    c. No **adicionar utilizador** secção, execute os seguintes passos:
    
    ![Adicionar utilizador](./media/active-directory-saas-amazon-web-service-tutorial/adduser1.png)
    
    * Introduza o nome de utilizador como **AzureADRoleManager**.
    
    * O tipo de acesso, selecione o **acesso programático** opção. Desta forma, o utilizador pode invocar as APIs e obter as funções a partir da conta AWS.
    
    * Clique em de **permissões seguinte** botão no canto inferior direito.

28. Agora, crie uma nova política para este utilizador, efetuando os seguintes passos:

    ![Adicionar utilizador](./media/active-directory-saas-amazon-web-service-tutorial/adduser2.png)
    
    a. Clique em de **anexar diretamente as políticas existentes** botão.

    b. Pesquisa para a política criada recentemente na secção de filtro **AzureAD_SSOUserRole_Policy**.
    
    c. Selecione o **política** e, em seguida, clique em de **seguinte: Reveja** botão.

29. Rever a política para o utilizador ligado, efetuando os seguintes passos:

    ![Adicionar utilizador](./media/active-directory-saas-amazon-web-service-tutorial/adduser3.png)
    
    a. Reveja o nome de utilizador, tipo de acesso e política mapeado para o utilizador.
    
    b. Clique em de **criar utilizador** botão no canto inferior direito para criar o utilizador.

30. Transferir as credenciais de utilizador de um utilizador, efetuando os seguintes passos:

    ![Adicionar utilizador](./media/active-directory-saas-amazon-web-service-tutorial/adduser4.png)
    
    a. Copie o utilizador **acesso ID chave** e **chave de acesso secreta**.
    
    b. Introduza estas credenciais para o utilizador do Azure AD aprovisionamento secção para obter as funções a partir da consola AWS.
    
    c. Clique em **fechar** na parte inferior.

31. Navegue para **aprovisionamento de utilizadores** secção da aplicação do Amazon Web Services no Portal de gestão do Azure AD.

    ![Adicionar utilizador](./media/active-directory-saas-amazon-web-service-tutorial/provisioning.png)

32. Introduza o **chave de acesso** e **segredo** no **segredo do cliente** e **segredo Token** campo respetivamente.

    ![Adicionar utilizador](./media/active-directory-saas-amazon-web-service-tutorial/provisioning1.png)
    
    a. Introduza a chave de acesso de utilizador do AWS no **clientsecret** campo.
    
    b. Introduza o segredo de utilizador do AWS no **segredo Token** campo.
    
    c. Clique em de **Testar ligação** botão e o utilizador deve conseguir esta ligação foi testada com êxito.

    d. Guardar a definição ao clicar no **guardar** botão na parte superior.
 
33. Agora Certifique-se de que ativa o estado de aprovisionamento **no** na secção de definições, tornando o comutador em e, em seguida, clicando no **guardar** botão na parte superior.

    ![Adicionar utilizador](./media/active-directory-saas-amazon-web-service-tutorial/provisioning2.png)

> [!TIP]
> Pode agora ler estas instruções dentro de uma versão concisa o [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir do **do Active Directory > aplicações da empresa** secção, basta clicar no **Single Sign-On** separador e aceder à documentação do embedded através de **configuração** secção na parte inferior. Pode ler mais sobre a funcionalidade de documentação incorporados aqui: [do Azure AD incorporado documentação]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção consiste em criar um utilizador de teste no portal do Azure chamado Britta Simon.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique em de **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e "Todos os utilizadores" ligações](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_02.png)

3. Para abrir o **utilizador** caixa de diálogo, clique em **adicionar** na parte superior do **todos os utilizadores** caixa de diálogo.

    ![O botão de adição](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_03.png)

4. No **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_04.png)

    a. No **nome** caixa, escreva **BrittaSimon**.

    b. No **nome de utilizador** caixa, escreva o endereço de e-mail do utilizador Britta Simon.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado no **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-an-amazon-web-services-aws-test-user"></a>Criar um utilizador de teste do Amazon Web Services (AWS)

O objetivo desta secção consiste em criar um utilizador chamado Britta Simon nos Amazon Web Services (AWS). Não necessita de um utilizador ser criado no seu sistema para SSO, por isso não terá de efetuar qualquer ação aqui Amazon Web Services (AWS).

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Britta Simon utilizar o Azure-início de sessão único, concedendo acesso para o Amazon Web Services (AWS).

![Atribuir a função de utilizador][200] 

**Para atribuir Britta Simon para Amazon Web Services (AWS), execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicações e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todas as aplicações**.

    ![Atribua o utilizador][201] 

2. Na lista de aplicações, selecione **Amazon Web Services (AWS)**.

    ![A ligação do Amazon Web Services (AWS) na lista de aplicações](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação de "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Britta Simon** na lista utilizadores.

6. Clique em **selecione** botão no **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribuir** botão no **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Teste o início de sessão único

Nesta secção, testar a configuração do Azure AD único início de sessão através do painel de acesso.

Quando clica no mosaico do Amazon Web Services (AWS) no painel de acesso, deve obter automaticamente com sessão iniciada para a aplicação do Amazon Web Services (AWS).
Para mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](active-directory-saas-tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_203.png
[11]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795031.png
[12]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795032.png
[13]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795033.png
[14]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795034.png
[15]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795035.png
[16]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795022.png
[17]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795023.png
[18]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795024.png
[19]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795025.png
[32]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950251.png
[33]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950252.png
[35]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning.png
[34]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950253.png
[36]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_securitycredentials.png
[37]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_securitycredentials_continue.png
[38]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_createnewaccesskey.png
[39]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_automatic.png
[40]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_testconnection.png
[41]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_on.png

