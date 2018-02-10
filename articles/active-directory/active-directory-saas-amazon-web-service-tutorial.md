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
ms.openlocfilehash: 0ff14365323d66a101e5847d7959045c3f20dea2
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/09/2018
---
# <a name="tutorial-azure-active-directory-integration-with-amazon-web-services-aws"></a>Tutorial: Integração do Azure Active Directory com o Amazon Web Services (AWS)

Neste tutorial, irá aprender a integrar o Amazon Web Services (AWS) com o Azure Active Directory (Azure AD).

Integrar o Amazon Web Services (AWS) com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso para o Amazon Web Services (AWS).
- Pode permitir aos utilizadores obter inicie automaticamente para o Amazon Web Services (AWS) com as respetivas contas do Azure AD.
- Pode gerir as contas numa única localização central, o portal do Azure.

Para obter mais informações sobre a integração de aplicações SaaS com o Azure AD, consulte [que é o acesso a aplicações e início de sessão no Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o Amazon Web Services (AWS), terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Amazon Web Services (AWS)-início de sessão único ativada subscrição

> [!NOTE]
> Não recomendamos a utilização de um ambiente de produção para testar os passos neste tutorial.

Para testar os passos neste tutorial, siga estas recomendações:

- Não utilize o seu ambiente de produção, exceto se for necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação gratuita de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, teste do Azure AD-início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos modulares principais:

1. Adicionar Amazon Web Services (AWS) a partir da Galeria
2. Configurar e testar o Azure AD de sessão único-

## <a name="add-amazon-web-services-aws-from-the-gallery"></a>Adicionar Amazon Web Services (AWS) a partir da Galeria
Para configurar a integração do Amazon Web Services (AWS) com o Azure AD, terá de adicionar Amazon Web Services (AWS) a partir da Galeria à sua lista de aplicações SaaS geridas.

**Para adicionar Amazon Web Services (AWS) a partir da galeria, siga os passos seguintes:**

1. No  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, selecione o **do Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Aceda a **aplicações empresariais**. Em seguida, aceda a **todas as aplicações**.

    ![O painel de aplicações da empresa][2]
    
3. Para adicionar uma nova aplicação, selecione o **nova aplicação** botão no topo da caixa de diálogo.

    ![O novo botão de aplicação][3]

4. Na caixa de pesquisa, escreva **Amazon Web Services (AWS)**. Selecione **Amazon Web Services (AWS)** do painel de resultados e, em seguida, selecione o **adicionar** botão para adicionar a aplicação.

    ![Amazon Web Services (AWS) na lista de resultados](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD-início de sessão único

Nesta secção, configure e teste do Azure AD-início de sessão único com o Amazon Web Services (AWS) com base num utilizador de teste chamado "Britta Simon."

Para início de sessão trabalhar, do Azure AD tem de saber que o utilizador homólogo nos Amazon Web Services (AWS) é um utilizador no Azure AD. Por outras palavras, tem de estabelecer uma ligação entre um utilizador do Azure AD e um utilizador relacionado nos Amazon Web Services (AWS).

Para estabelecer a ligação, nos Amazon Web Services (AWS), atribua o valor **Username** o mesmo valor como **nome de utilizador** no Azure AD. 

Para configurar e testar o Azure AD-início de sessão único com o Amazon Web Services (AWS), execute os seguintes blocos de criação:

1. [Configurar o Azure AD-início de sessão único](#configure-azure-ad-single-sign-on) para permitir aos utilizadores utilizar esta funcionalidade.
2. [Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user) para testar do Azure AD-início de sessão único com Britta Simon.
3. [Criar um utilizador de teste do Amazon Web Services (AWS)](#create-an-amazon-web-services-aws-test-user) ter um homólogo de Britta Simon nos Amazon Web Services (AWS) que está ligada a representação do Azure AD do utilizador.
4. [Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user) para ativar Britta Simon utilizar o Azure AD-início de sessão único.
5. [Teste o início de sessão único](#test-single-sign-on) para verificar que a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD-início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão no portal do Azure e configurar o início de sessão único na sua aplicação Amazon Web Services (AWS).

**Para configurar o Azure AD-início de sessão único com o Amazon Web Services (AWS), siga os passos seguintes:**

1. No portal do Azure, no **Amazon Web Services (AWS)** página de integração de aplicações, selecione **de sessão único-**.

    ![Configurar a ligação de início de sessão único][4]

2. Para ativar o início de sessão, no **de sessão único-** caixa de diálogo a **modo** na lista pendente, selecione **baseados em SAML início de sessão**.
 
    ![Caixa de diálogo de início de sessão único](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_samlbase.png)

3. No **Amazon Web Services (AWS) domínios e URLs** secção, o utilizador não tem de efetuar quaisquer passos porque a aplicação já está pré-integrada com o Azure.

    ![Domínio da Amazon Web Services (AWS) e os URLs únicos de informações de início de sessão](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_url.png)

4. A aplicação de software do Amazon Web Services (AWS) espera as asserções de SAML num formato específico. Configure as seguintes afirmações para esta aplicação. Pode gerir os valores destes atributos do **atributos de utilizador** secção na página de integração de aplicações. A seguinte captura de ecrã mostra um exemplo:

    ![Configurar o atributo de início de sessão único](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_attribute.png)  

5. No **atributos de utilizador** secção o **de sessão único-** , configure o atributo de token SAML, como mostrado na imagem anterior e, em seguida, siga os passos seguintes:
    
    | Nome do atributo  | Valor do atributo | Espaço de Nomes |
    | --------------- | --------------- | --------------- |
    | RoleSessionName | user.userprincipalname | https://aws.amazon.com/SAML/Attributes |
    | Função            | user.assignedroles |  https://aws.amazon.com/SAML/Attributes |
    
    >[!TIP]
    >Configure o aprovisionamento de utilizador no Azure AD para obter todas as funções a partir da consola do Amazon Web Services (AWS). Consulte os seguintes passos de aprovisionamento.

    a. Para abrir o **adicionar atributo** caixa de diálogo, selecione **adicionar atributo**.

    ![Configurar o atributo de início de sessão único](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_attribute_04.png)

    ![Configurar o atributo de início de sessão único](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_attribute_05.png)

    b. No **nome** caixa, escreva o nome do atributo que é apresentado para essa linha.

    c. Do **valor** lista, digite o valor do atributo que é apresentado para essa linha.

    d. No **espaço de nomes** caixa, digite o valor de espaço de nomes que é apresentado para essa linha.
    
    d. Selecione **Ok**.

6. No **certificado de assinatura de SAML** secção, selecione **XML de metadados**. Em seguida, guarde o ficheiro de metadados no seu computador.

    ![A hiperligação de transferência do certificado](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_certificate.png) 

7. Selecione **Guardar**.

    ![Configurar o início de sessão de gravação botão](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_400.png)

8. Numa janela do browser diferente, inicie sessão no site da sua empresa Amazon Web Services (AWS) como um administrador.

9. Selecione **consola Home**.
   
    ![Configurar a home page do início de sessão único][11]

10. Selecione **identidade e gestão de acesso**. 
   
    ![Configurar a identidade de início de sessão único][12]

11. Selecione **fornecedores de identidade**. Em seguida, selecione **criar fornecedor**. 
   
    ![Configurar o fornecedor de início de sessão único][13]

12. No **Configurar fornecedor** diálogo caixa, siga os passos seguintes: 
   
    ![Configurar a caixa de diálogo de n de início de sessão único][14]
 
    a. Para **tipo de fornecedor**, selecione **SAML**.

    b. No **nome do fornecedor** caixa, escreva um nome de fornecedor (por exemplo: *WAAD*).

    c. Para carregar o transferido **ficheiro de metadados** a partir do portal do Azure, selecione **Escolher ficheiro**.

    d. Selecione **passo seguinte**.

13. No **verificar informações do fornecedor** caixa de diálogo, selecione **criar**. 
    
    ![Configurar a verificação de início de sessão único][15]

14. Selecione **funções**. Em seguida, selecione **criar nova função**. 
    
    ![Configurar funções de início de sessão único][16]

15. No **definir o nome da função** diálogo caixa, siga os passos seguintes: 
    
    ![Configurar o nome de início de sessão único][17] 

    a. No **nome da função** caixa, escreva um nome de função (por exemplo, *TestUser*). 

    b. Selecione **passo seguinte**.

16. No **selecionar tipo de função** diálogo caixa, siga os passos seguintes: 
    
    ![Configurar o tipo de função de início de sessão único][18] 

    a. Selecione **função para acesso do fornecedor de identidade**. 

    b. No **acesso conceder Web Single Sign-On (WebSSO) para fornecedores de SAML** secção, clique em **selecione**.

17. No **estabelecer confiança** diálogo caixa, siga os passos seguintes:  
    
    ![Configurar a confiança de início de sessão único][19] 

    a. Selecione o fornecedor SAML que criou anteriormente (por exemplo: *WAAD*). 
  
    b. Selecione **passo seguinte**.

18. No **verificar função confiar** caixa de diálogo, selecione **passo seguinte**. 
    
    ![Configurar a confiança da função de início de sessão único][32]

19. No **anexar política** caixa de diálogo, selecione **passo seguinte**.  
    
    ![Configurar a política de início de sessão único][33]

20. No **revisão** diálogo caixa, siga os passos seguintes:   
    
    ![Configurar a revisão de início de sessão único][34] 

    a. Selecione **criar função**.

    b. Criar funções tantos conforme necessário e, em seguida, mapeá-los para o fornecedor de identidade.

21. Utilize credenciais de conta de serviço do Amazon Web Services (AWS) para obter as funções da conta do Amazon Web Services (AWS) no aprovisionamento de utilizadores do Azure AD. Para iniciar esta tarefa, abra a consola do Amazon Web Services (AWS) raiz.

22. Selecione **serviços** > **segurança, de identidade e de conformidade** > **IAM**.

    ![Obter as funções da conta Amazon Web Services (AWS)](./media/active-directory-saas-amazon-web-service-tutorial/fetchingrole1.png)

23. Na secção IAM, selecione o **políticas** separador.

    ![Obter as funções da conta Amazon Web Services (AWS)](./media/active-directory-saas-amazon-web-service-tutorial/fetchingrole2.png)

24. Para criar uma nova política, selecione **criar política**.

    ![Criar uma nova política](./media/active-directory-saas-amazon-web-service-tutorial/fetchingrole3.png)
 
25. Para criar a sua própria política para obter todas as funções de contas do Amazon Web Services (AWS), siga os passos seguintes:

    ![Criar uma nova política](./media/active-directory-saas-amazon-web-service-tutorial/policy1.png)

    a. No **criar política** secção, selecione o **JSON** separador.

    b. O documento de política, adicione o seguinte JSON:
    
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

    c. Para validar a política, selecione o **botão de política de revisão**.

    ![Definir a nova política](./media/active-directory-saas-amazon-web-service-tutorial/policy5.png)

26. Definir o **nova política** , efetuando os seguintes passos:

    ![Definir a nova política](./media/active-directory-saas-amazon-web-service-tutorial/policy2.png)

    a. Forneça o **nome da política** como **AzureAD_SSOUserRole_Policy**.

    b. Pode fornecer o seguinte **Descrição** para a política: **esta política permite-lhe obter as funções de contas do AWS**.
    
    c. Selecione o **criar política** botão.
        
27. Para criar uma nova conta de utilizador no serviço IAM Amazon Web Services (AWS), siga os passos seguintes:

    a. Selecione **utilizadores** na consola do Amazon Web Services (AWS) IAM.

    ![Definir a nova política](./media/active-directory-saas-amazon-web-service-tutorial/policy3.png)
    
    b.To criar um novo utilizador, selecione o **adicionar utilizador** botão.

    ![Adicionar utilizador](./media/active-directory-saas-amazon-web-service-tutorial/policy4.png)

    c. No **adicionar utilizador** secção, siga os passos seguintes:
    
    ![Adicionar utilizador](./media/active-directory-saas-amazon-web-service-tutorial/adduser1.png)
    
    * Introduza **AzureADRoleManager** na caixa de nome de utilizador.
    
    * Para o tipo de acesso, selecione o **acesso programático** opção. Desta forma, o utilizador pode invocar as APIs e obter as funções da conta do Amazon Web Services (AWS).
    
    * Selecione o **permissões seguinte** botão no canto inferior direito.

28. Crie uma nova política para este utilizador, efetuando os seguintes passos:

    ![Adicionar utilizador](./media/active-directory-saas-amazon-web-service-tutorial/adduser2.png)
    
    a. Selecione o **anexar diretamente as políticas existentes** botão.

    b. Pesquisa para a política criada recentemente na secção de filtro **AzureAD_SSOUserRole_Policy**.
    
    c. Selecione o **política**. Em seguida, selecione o **seguinte: Reveja** botão.

29. Rever a política para o utilizador ligado, efetuando os seguintes passos:

    ![Adicionar utilizador](./media/active-directory-saas-amazon-web-service-tutorial/adduser3.png)
    
    a. Reveja o nome de utilizador, o tipo de acesso e a política que estão mapeadas para o utilizador.
    
    b. Para criar o utilizador, selecione o **criar utilizador** botão no canto inferior direito para criar o utilizador.

30. Transferir as credenciais de um utilizador, efetuando os seguintes passos:

    ![Adicionar utilizador](./media/active-directory-saas-amazon-web-service-tutorial/adduser4.png)
    
    a. Copie o utilizador **acesso ID chave** e **chave de acesso secreta**.
    
    b. Introduza estas credenciais para o utilizador do Azure AD aprovisionamento secção para obter as funções a partir da consola do Amazon Web Services (AWS).
    
    c. Selecione o **fechar** botão no canto inferior direito.

31. Navegue para o **aprovisionamento de utilizadores** secção da aplicação Amazon Web Services (AWS) no Portal de gestão do Azure AD.

    ![Adicionar utilizador](./media/active-directory-saas-amazon-web-service-tutorial/provisioning.png)

32. Introduza o **chave de acesso** e **segredo** no **segredo do cliente** e **segredo Token** campo respetivamente.

    ![Adicionar utilizador](./media/active-directory-saas-amazon-web-service-tutorial/provisioning1.png)
    
    a. Introduza a chave de acesso de utilizador do Amazon Web Services (AWS) no **clientsecret** campo.
    
    b. Introduza o segredo de utilizador do Amazon Web Services (AWS) no **segredo Token** campo.
    
    c. Selecione o **Testar ligação** botão. Deverá conseguir esta ligação foi testada com êxito.

    d. Guardar a definição ao selecionar o **guardar** botão na parte superior.
 
33. Certifique-se que ative o o estado de aprovisionamento **no** no **definições**. Fazê-lo ao selecionar **no**e, em seguida, selecionar o **guardar** botão na parte superior.

    ![Adicionar utilizador](./media/active-directory-saas-amazon-web-service-tutorial/provisioning2.png)

> [!TIP]
> Pode agora ler estas instruções dentro de uma versão concisa o [portal do Azure](https://portal.azure.com) enquanto estiver a configurar a aplicação. Depois de adicionar esta aplicação a partir de **do Active Directory > aplicações da empresa** secção, selecione o **Single Sign-On** separador. Em seguida, aceder a documentação incorporada através de **configuração** secção na parte inferior. Pode ler mais sobre a funcionalidade de documentação incorporados em [do Azure AD incorporado documentação]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção consiste em criar um utilizador de teste no portal do Azure chamado Britta Simon.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, siga os passos seguintes:**

1. No portal do Azure, no painel esquerdo, selecione o **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**. Em seguida, selecione **todos os utilizadores**.

    !["Os utilizadores e grupos" e "Todos os utilizadores" ligações](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_02.png)

3. Para abrir o **utilizador** caixa de diálogo, selecione **adicionar** na parte superior do **todos os utilizadores** caixa de diálogo.

    ![O botão de adição](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_03.png)

4. No **utilizador** diálogo caixa, siga os passos seguintes:

    ![A caixa de diálogo de utilizador](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_04.png)

    a. No **nome** caixa, escreva **BrittaSimon**.

    b. No **nome de utilizador** caixa, escreva o endereço de e-mail do utilizador Britta Simon.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado no **palavra-passe** caixa.

    d. Selecione **Criar**.
 
### <a name="create-an-amazon-web-services-aws-test-user"></a>Criar um utilizador de teste do Amazon Web Services (AWS)

O objetivo desta secção consiste em criar um utilizador chamado Britta Simon nos Amazon Web Services (AWS). Amazon Web Services (AWS) não necessita de um utilizador a ser criado no seu sistema para single-sign-on, por isso não terá de efetuar qualquer ação aqui.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Britta Simon utilizar o Azure-início de sessão único, concedendo-lhes acesso para o Amazon Web Services (AWS).

![Atribuir a função de utilizador][200] 

**Para atribuir Britta Simon para Amazon Web Services (AWS), siga os passos seguintes:**

1. No portal do Azure, abra a vista de aplicações. Em seguida, aceda à vista de diretório e selecione **aplicações empresariais**. Em seguida, selecione **todas as aplicações**.

    ![Atribua o utilizador][201] 

2. Na lista de aplicações, selecione **Amazon Web Services (Amazon Web Services (AWS)**.

    ![A ligação do Amazon Web Services (AWS) na lista de aplicações](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_app.png)  

3. No menu à esquerda, selecione **utilizadores e grupos**.

    ![A ligação de "Utilizadores e grupos"][202]

4. Selecione o **adicionar** botão. Em seguida, no **adicionar atribuição** caixa de diálogo, selecione **utilizadores e grupos**.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Britta Simon** na lista utilizadores.

6. No **utilizadores e grupos** caixa de diálogo, clique em de **selecione** botão. 

7. No **adicionar atribuição** caixa de diálogo, selecione o **atribuir** botão.
    
### <a name="test-single-sign-on"></a>Teste o início de sessão único

Nesta secção, teste configuração do Azure AD único início de sessão utilizando o painel de acesso.

Quando seleciona o mosaico da Amazon Web Services (AWS) no painel de acesso, deve obter automaticamente com sessão iniciada para a aplicação do Amazon Web Services (AWS). Para mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS no Azure Active Directory](active-directory-saas-tutorial-list.md)
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

