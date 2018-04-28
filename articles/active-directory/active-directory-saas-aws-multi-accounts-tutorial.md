---
title: 'Tutorial: Integração do Azure Active Directory com o Amazon Web Services (AWS) para ligar a várias contas | Microsoft Docs'
description: Saiba como configurar o início de sessão entre o Azure AD e várias contas do Amazon Web Services (AWS).
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
ms.date: 03/28/2018
ms.author: jeedes
ms.openlocfilehash: 929caab0aafdad24062e372e458f16a5f36cce73
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/18/2018
---
# <a name="tutorial-azure-active-directory-integration-with-multiple-amazon-web-services-aws-accounts"></a>Tutorial: Integração do Azure Active Directory com várias contas Amazon Web Services (AWS)

Neste tutorial, saiba como integrar o Azure Active Directory (Azure AD) com várias contas do Amazon Web Services (AWS).

Integrar o Amazon Web Services (AWS) com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso para o Amazon Web Services (AWS).
- Pode permitir aos utilizadores automaticamente obter com sessão iniciada para Amazon Web Services (AWS) (Single Sign-On) com as respetivas contas do Azure AD.
- Pode gerir as contas numa localização central - portal do Azure.

Se pretender saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [que é o acesso a aplicações e início de sessão no Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o Amazon Web Services (AWS), terá dos seguintes itens:

- Uma subscrição do Azure AD basic ou premium
- Amazon Web Services (AWS) vários-início de sessão único ativada contas

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

    ![Amazon Web Services (AWS) na lista de resultados](./media/active-directory-saas-aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_addfromgallery.png)

5. Assim que a aplicação é adicionada, aceda a **propriedades** página e copie o **ID de objeto**.

    ![Amazon Web Services (AWS) na lista de resultados](./media/active-directory-saas-aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_properties.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD-início de sessão único

Nesta secção, configure e teste do Azure AD-início de sessão único com o Amazon Web Services (AWS) com base num utilizador de teste chamado "Britta Simon".

Para início de sessão trabalhar, do Azure AD tem de saber o que o utilizador homólogo nos Amazon Web Services (AWS) é um utilizador no Azure AD. Por outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado nos Amazon Web Services (AWS) tem de ser estabelecida.

Nos Amazon Web Services (AWS), atribua o valor do **nome de utilizador** no Azure AD como o valor a **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD-início de sessão único com o Amazon Web Services (AWS), tem de concluir os blocos modulares seguintes:

1. **[Configurar o Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)**  - para permitir aos utilizadores utilizar esta funcionalidade.
2. **[Teste o início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD-início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão no portal do Azure e configurar o início de sessão único na sua aplicação Amazon Web Services (AWS).

**Para configurar o Azure AD-início de sessão único com o Amazon Web Services (AWS), execute os seguintes passos:**

1. No portal do Azure, no **Amazon Web Services (AWS)** página de integração de aplicações, clique em **de sessão único-**.

    ![Configurar a ligação de início de sessão único][4]

2. No **de sessão único-** caixa de diálogo, selecione **modo** como **baseados em SAML início de sessão** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/active-directory-saas-aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_samlbase.png)

3. No **Amazon Web Services (AWS) domínios e URLs** secção, o utilizador não tem de efetuar quaisquer passos, tal como a aplicação já está pré-integrada com o Azure.

    ![Domínio Amazon Web Services (AWS) e os URLs únicos de informações de início de sessão](./media/active-directory-saas-aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_url.png)

4. A aplicação de Software do Amazon Web Services (AWS) espera as asserções de SAML num formato específico. Configure as seguintes afirmações para esta aplicação. Pode gerir os valores destes atributos a partir de "**atributos de utilizador**" secção na página de integração de aplicações. A seguinte captura de ecrã mostra um exemplo para este.

    ![Configurar o início de sessão único atributo](./media/active-directory-saas-aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_attribute.png)  

5. No **atributos de utilizador** secção no **de sessão único-** caixa de diálogo, configurar atributos token SAML, conforme mostrado na imagem acima e execute os seguintes passos:
    
    | Nome do Atributo  | Valor do Atributo | Espaço de Nomes |
    | --------------- | --------------- | --------------- |
    | RoleSessionName | user.userprincipalname | https://aws.amazon.com/SAML/Attributes |
    | Função            | user.assignedroles |  https://aws.amazon.com/SAML/Attributes |
    
    >[!TIP]
    >Terá de configurar o aprovisionamento de utilizador no Azure AD para obter todas as funções a partir da consola AWS. Consulte os passos de aprovisionamento abaixo.

    a. Clique em **adicionar atributo** para abrir o **adicionar atributo** caixa de diálogo.

    ![Configurar o início de sessão único adicionar](./media/active-directory-saas-aws-multi-accounts-tutorial/tutorial_attribute_04.png)

    ![Configurar o início de sessão único atributo](./media/active-directory-saas-aws-multi-accounts-tutorial/tutorial_attribute_05.png)

    b. No **nome** caixa de texto, escreva o nome de atributo apresentado para essa linha.

    c. Do **valor** lista, digite o valor de atributo apresentado para essa linha.

    d. No **espaço de nomes** caixa de texto, digite o valor do espaço de nomes mostrado para essa linha.
    
    d. Clique em **OK**.

6. No **certificado de assinatura de SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![A hiperligação de transferência do certificado](./media/active-directory-saas-aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_certificate.png) 

7. Clique em **guardar** botão.

    ![Configurar botão único início de sessão guardar](./media/active-directory-saas-aws-multi-accounts-tutorial/tutorial_general_400.png)

8. Numa janela do browser diferente, início de sessão no site da sua empresa Amazon Web Services (AWS) como administrador.

9. Clique em **AWS Home**.
   
    ![Configurar a home page do Single Sign-On][11]

10. Clique em **IAM** (identidade e gestão de acesso). 
   
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

14. Clique em **funções**e, em seguida, clique em **criar função**. 
    
    ![Configurar funções de início de sessão único][16]

15. No **criar função** página, execute os seguintes passos:  
    
    ![Configurar a confiança de início de sessão único][19] 

    a. Selecione **SAML 2.0 Federação** em **selecionar tipo de entidade fidedigna**.

    b. Em **escolher uma secção de SAML 2.0 fornecedor**, selecione o **fornecedor SAML** que criou anteriormente (por exemplo: *WAAD*)

    c. Selecione **programático permissões e de acesso à consola de gestão do AWS**.
  
    d. Clique em **seguinte: permissões**.

16. No **ligar políticas de permissões** caixa de diálogo, clique em **seguinte: Reveja**.  
    
    ![Configurar a política de início de sessão único][33]

17. No **revisão** caixa de diálogo, execute os seguintes passos:   
    
    ![Configurar a revisão de início de sessão único][34] 

    a. No **nome da função** caixa de texto, introduza o nome de função.

    b. No **descrição da função** caixa de texto, introduza a descrição.

    a. Clique em **criar função**.

    b. Criar funções tantos conforme necessário e mapeá-los para o fornecedor de identidade.

18. Terminar sessão atual conta AWS e inicie sessão com outra conta onde pretende configurar o início de sessão único com o Azure AD.

19. Execute o passo 9 para 17-passo para criar várias funções que pretende que a configuração para esta conta. Se tiver mais de duas contas, execute os mesmos passos para todas as contas criar funções para os mesmos.

20. Depois de todas as funções são criadas nas contas de, são apresentados no **funções** lista dessas contas.

    ![Configuração de funções](./media/active-directory-saas-aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_listofroles.png)

21. É necessário capturar todas as funções ARN e entidades fidedignas para todas as funções em todas as contas, que é necessário mapear manualmente com a aplicação do Azure AD. 

22. Clique em copiar as funções **função ARN** e **entidades fidedignas** valores. Necessitar destes valores para todas as funções que tem de criar no Azure AD.

    ![Configuração de funções](./media/active-directory-saas-aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_role_summary.png)
 
23. Executar o passo anterior para todas as funções em todas as contas e armazenar todos eles no formato **ARN de função, entidades fidedignas** num bloco de notas. 

24. Abra [Explorador do Azure AD Graph](https://developer.microsoft.com/graph/graph-explorer) na outra janela.

    a. Inicie sessão site do Explorador de gráfico com as credenciais de Global administrador/coadministrador do seu inquilino.

    b. Tem de ter permissões suficientes para criar as funções. Clique em **modificar permissões** para obter as permissões necessárias. 

    ![Caixa de diálogo do Explorador de gráfico](./media/active-directory-saas-aws-multi-accounts-tutorial/graph-explorer-new9.png)

    c. Selecione os seguintes permissões de lista (se ainda não tiver estes) e clique em "Modificar permissões" 

    ![Caixa de diálogo do Explorador de gráfico](./media/active-directory-saas-aws-multi-accounts-tutorial/graph-explorer-new10.png)

    d. Isto irá pedir-lhe para iniciar sessão novamente e aceitar o consentimento do utilizador. Após aceitar o consentimento, que tem sessão iniciada no Explorador do gráfico novamente.

    e. Alterar a lista pendente de versão para **beta**. Para obter todos os principais de serviço do seu inquilino, utilize a seguinte consulta:
    
     `https://graph.microsoft.com/beta/servicePrincipals`
        
    Se estiver a utilizar vários diretórios, em seguida, pode utilizar os seguintes padrão, que tem o seu domínio primário no mesmo `https://graph.microsoft.com/beta/contoso.com/servicePrincipals`
    
    ![Caixa de diálogo do Explorador de gráfico](./media/active-directory-saas-aws-multi-accounts-tutorial/graph-explorer-new1.png)
    
    f. Na lista de principais de serviço obtida, obtenha um que necessitar de modificar. Também pode utilizar Ctrl + F para procurar a aplicação de todos os ServicePrincipals listadas. Pode utilizar a seguinte consulta utilizando o **id de objeto** que copiou da página de propriedades do Azure AD para obter para o Principal de serviço correspondentes.
    
    `https://graph.microsoft.com/beta/servicePrincipals/<objectID>`.

    ![Caixa de diálogo do Explorador de gráfico](./media/active-directory-saas-aws-multi-accounts-tutorial/graph-explorer-new2.png)

    g. Extrair a propriedade appRoles do objeto principal do serviço. 

    ![Caixa de diálogo do Explorador de gráfico](./media/active-directory-saas-aws-multi-accounts-tutorial/graph-explorer-new3.png)

    h. Agora tem de gerar novas funções para a sua aplicação. 

    i. Abaixo JSON é um exemplo de appRoles objeto. Crie um objeto semelhante para adicionar as funções que pretende para a sua aplicação. 

    ```
    {
    "appRoles": [
        {
            "allowedMemberTypes": [
                "User"
            ],
            "description": "msiam_access",
            "displayName": "msiam_access",
            "id": "7dfd756e-8c27-4472-b2b7-38c17fc5de5e",
            "isEnabled": true,
            "origin": "Application",
            "value": null
        },
        {
            "allowedMemberTypes": [
                "User"
            ],
            "description": "Admin,WAAD",
            "displayName": "Admin,WAAD",
            "id": "4aacf5a4-f38b-4861-b909-bae023e88dde",
            "isEnabled": true,
            "origin": "ServicePrincipal",
            "value": "arn:aws:iam::12345:role/Admin,arn:aws:iam::12345:saml-provider/WAAD"
        },
        {
            "allowedMemberTypes": [
                "User"
            ],
            "description": "Auditors,WAAD",
            "displayName": "Auditors,WAAD",
            "id": "bcad6926-67ec-445a-80f8-578032504c09",
            "isEnabled": true,
            "origin": "ServicePrincipal",
            "value": "arn:aws:iam::12345:role/Auditors,arn:aws:iam::12345:saml-provider/WAAD"
        }    ]
    }
    ```

    > [!Note]
    > Só é possível adicionar novas funções depois do **msiam_access** para a operação de patch. Além disso, pode adicionar tantos funções como pretende que por sua necessidade da organização. Azure AD irá enviar o **valor** destas funções como o valor de afirmação na resposta SAML.
    
    j. Volte para o Explorador do gráfico e alterar o método de **obter** para **PATCH**. Corrigir o objeto Principal de serviço para ter pretendido funções atualizando appRoles propriedade semelhante às mostrado acima no exemplo. Clique em **executar consulta** para executar a operação de patch. Uma mensagem de êxito confirma que a criação da função para a sua aplicação Amazon Web Services.

    ![Caixa de diálogo do Explorador de gráfico](./media/active-directory-saas-aws-multi-accounts-tutorial/graph-explorer-new11.png)

25. Depois do Principal de serviço é aplicado com várias funções, pode atribuir utilizadores/grupos às respetivas funções. Isto pode ser feito ao aceder ao portal e navegando para a aplicação do Amazon Web Services. Clique em de **utilizadores e grupos** separador na parte superior. 

26. Recomendamos-lhe criar novos grupos para cada função do AWS para que pode atribuir essa função específica desse grupo. Tenha em atenção que se trata de um para um mapeamento de um grupo a uma função. Em seguida, pode adicionar os membros que pertencem a esse grupo.

27. Assim que os grupos são criados, selecione o grupo e atribuir à aplicação. 

    ![Configurar o início de sessão único adicionar](./media/active-directory-saas-aws-multi-accounts-tutorial/graph-explorer-new5.png)

28. Para atribuir a função ao grupo, selecione a função e clique em **atribuir** botão na parte inferior da página.

    ![Configurar o início de sessão único adicionar](./media/active-directory-saas-aws-multi-accounts-tutorial/graph-explorer-new6.png)

> [!Note]
> Tenha em atenção que tem de atualizar a sua sessão no portal do Azure para ver as novas funções.

### <a name="test-single-sign-on"></a>Teste o início de sessão único

Nesta secção, testar a configuração do Azure AD único início de sessão através do painel de acesso.

Quando clica no mosaico do Amazon Web Services (AWS) no painel de acesso, deve obter a página de aplicações do Amazon Web Services (AWS) com a opção para selecionar a função.

![Configurar o início de sessão único adicionar](./media/active-directory-saas-aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_test_screen.png)

Também pode verificar a resposta SAML para ver as funções que está a ser transmitidas como afirmações.

![Configurar o início de sessão único adicionar](./media/active-directory-saas-aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_test_saml.png)

Para mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](active-directory-saas-tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-aws-multi-accounts-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-aws-multi-accounts-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-aws-multi-accounts-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-aws-multi-accounts-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-aws-multi-accounts-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-aws-multi-accounts-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-aws-multi-accounts-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-aws-multi-accounts-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-aws-multi-accounts-tutorial/tutorial_general_203.png
[11]: ./media/active-directory-saas-aws-multi-accounts-tutorial/ic795031.png
[12]: ./media/active-directory-saas-aws-multi-accounts-tutorial/ic795032.png
[13]: ./media/active-directory-saas-aws-multi-accounts-tutorial/ic795033.png
[14]: ./media/active-directory-saas-aws-multi-accounts-tutorial/ic795034.png
[15]: ./media/active-directory-saas-aws-multi-accounts-tutorial/ic795035.png
[16]: ./media/active-directory-saas-aws-multi-accounts-tutorial/ic795022.png
[17]: ./media/active-directory-saas-aws-multi-accounts-tutorial/ic795023.png
[18]: ./media/active-directory-saas-aws-multi-accounts-tutorial/ic795024.png
[19]: ./media/active-directory-saas-aws-multi-accounts-tutorial/ic795025.png
[32]: ./media/active-directory-saas-aws-multi-accounts-tutorial/ic7950251.png
[33]: ./media/active-directory-saas-aws-multi-accounts-tutorial/ic7950252.png
[35]: ./media/active-directory-saas-aws-multi-accounts-tutorial/tutorial_amazonwebservices_provisioning.png
[34]: ./media/active-directory-saas-aws-multi-accounts-tutorial/ic7950253.png
[36]: ./media/active-directory-saas-aws-multi-accounts-tutorial/tutorial_amazonwebservices_securitycredentials.png
[37]: ./media/active-directory-saas-aws-multi-accounts-tutorial/tutorial_amazonwebservices_securitycredentials_continue.png
[38]: ./media/active-directory-saas-aws-multi-accounts-tutorial/tutorial_amazonwebservices_createnewaccesskey.png
[39]: ./media/active-directory-saas-aws-multi-accounts-tutorial/tutorial_amazonwebservices_provisioning_automatic.png
[40]: ./media/active-directory-saas-aws-multi-accounts-tutorial/tutorial_amazonwebservices_provisioning_testconnection.png
[41]: ./media/active-directory-saas-aws-multi-accounts-tutorial/tutorial_amazonwebservices_provisioning_on.png

