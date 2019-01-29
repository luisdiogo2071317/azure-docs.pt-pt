---
title: 'Tutorial: Integração do Active Directory do Azure com o Amazon Web Services (AWS) para ligar várias contas | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure AD e várias contas do Amazon Web Services (AWS).
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 7561c20b-2325-4d97-887f-693aa383c7be
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/15/2018
ms.author: jeedes
ms.openlocfilehash: 48d8516a1923aaacc26db2eb9a9acfd0ddff737e
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55197522"
---
# <a name="tutorial-azure-active-directory-integration-with-multiple-amazon-web-services-aws-accounts"></a>Tutorial: Integração do Active Directory do Azure com várias contas do Amazon Web Services (AWS)

Neste tutorial, saiba como integrar o Azure Active Directory (Azure AD) com várias contas do Amazon Web Services (AWS).

Integrar o Amazon Web Services (AWS) com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso para o Amazon Web Services (AWS).
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Amazon Web Services (AWS) (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

![Amazon Web Services (AWS) na lista de resultados](./media/aws-multi-accounts-tutorial/amazonwebservice.png)

>[!NOTE]
>Tenha em atenção de ligar uma aplicação do AWS para todas as suas contas AWS não é a nossa abordagem recomendada. Em vez disso, recomendamos que utilize [isso](https://docs.microsoft.com/azure/active-directory/saas-apps/amazon-web-service-tutorial) abordagem para configurar várias instâncias de conta AWS para várias instâncias de aplicações do AWS no Azure AD.

**Tenha em atenção que não é recomendável usar essa abordagem por motivos seguintes:**

* Tem de utilizar a abordagem de API do Graph para todas as funções para a aplicação de patches. Não é recomendado utilizar a abordagem do ficheiro de manifesto.

* Temos visto relatórios que depois de adicionar funções de aplicação de ~ 1200 para uma única aplicação do AWS, qualquer operação na aplicação iniciada jogando os erros de clientes relacionados com o tamanho. Existe um limite restritivo de tamanho do objeto de aplicativo.

* Terá de atualizar manualmente a função, como as funções adicionadas em qualquer uma das contas, que é uma abordagem de Replace e não acrescentar Infelizmente. Também se as suas contas estão a crescer, em seguida, isso se torna n x n relação com contas e funções.

* Todas as contas AWS vão utilizar o mesmo ficheiro XML de metadados de Federação e no momento do certificado de rollover tiver que orientar neste exercício massivo para atualizar o certificado em todas as contas AWS ao mesmo tempo

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o Amazon Web Services (AWS), terá dos seguintes itens:

- Uma subscrição do Azure AD básica ou premium
- Amazon Web Services (AWS) vários início de sessão único de contas ativada

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando o Amazon Web Services (AWS) da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-amazon-web-services-aws-from-the-gallery"></a>Adicionando o Amazon Web Services (AWS) da Galeria
Para configurar a integração do Amazon Web Services (AWS) para o Azure AD, terá de adicionar Amazon Web Services (AWS) a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Amazon Web Services (AWS) a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![image](./media/aws-multi-accounts-tutorial/selectazuread.png)

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![image](./media/aws-multi-accounts-tutorial/a_select_app.png)
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![image](./media/aws-multi-accounts-tutorial/a_new_app.png)

4. Na caixa de pesquisa, escreva **Amazon Web Services (AWS)**, selecione **Amazon Web Services (AWS)** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

     ![image](./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_addfromgallery.png)

5. Depois do aplicativo é adicionado, vá para **propriedades** página e copie a **ID de objeto**.

    ![Amazon Web Services (AWS) na lista de resultados](./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_properties.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com o Amazon Web Services (AWS) com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Amazon Web Services (AWS) para um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Amazon Web Services (AWS) deve ser estabelecido.

No Amazon Web Services (AWS), atribuir o valor do **nome de utilizador** no Azure AD como o valor da **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com o Amazon Web Services (AWS), tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único na sua aplicação do Amazon Web Services (AWS).

**Para configurar o Azure AD início de sessão único com o Amazon Web Services (AWS), execute os seguintes passos:**

1. Na [portal do Azure](https://portal.azure.com/), na **Amazon Web Services (AWS)** página de integração de aplicações, selecione **início de sessão único**.

    ![image](./media/aws-multi-accounts-tutorial/B1_B2_Select_SSO.png)

2. Sobre o **selecionar um método de início de sessão único** caixa de diálogo, selecione **SAML** modo para ativar o início de sessão único.

    ![image](./media/aws-multi-accounts-tutorial/b1_b2_saml_sso.png)

3. Sobre o **definido no início de sessão único com o SAML** página, clique em **editar** botão para abrir **configuração básica de SAML** caixa de diálogo.

    ![image](./media/aws-multi-accounts-tutorial/b1-domains_and_urlsedit.png)

4. Sobre o **configuração básica de SAML** secção, o utilizador não tem de realizar qualquer passo como a aplicação já está pré-integrada com o Azure.

    ![image](./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_url.png)

5. Aplicação do Amazon Web Services (AWS) espera que as asserções SAML num formato específico. Configure as seguintes declarações para esta aplicação. Pode gerir os valores destes atributos do **atributos de utilizador e afirmações** secção na página de integração de aplicações. Sobre o **definido no início de sessão único com o SAML** página, clique em **editar** botão para abrir **atributos de utilizador e afirmações** caixa de diálogo.

    ![image](./media/aws-multi-accounts-tutorial/i4-attribute.png)

6. No **afirmações de utilizador** secção sobre o **atributos de utilizador e afirmações** caixa de diálogo, configurar o atributo de token de SAML conforme mostrado na imagem acima e execute os seguintes passos:
    
    | Name  | Atributo de origem  | Espaço de nomes |
    | --------------- | --------------- | --------------- |
    | RoleSessionName | user.userprincipalname | https://aws.amazon.com/SAML/Attributes |
    | Função            | user.assignedroles |  https://aws.amazon.com/SAML/Attributes |
    | SessionDuration             | "fornecer um valor entre 900 segundos (15 minutos) para 43200 segundos (12 horas)" |  https://aws.amazon.com/SAML/Attributes |

    a. Clique em **Adicionar nova afirmação** para abrir o **afirmações de utilizador de gerir** caixa de diálogo.

    ![image](./media/aws-multi-accounts-tutorial/i2-attribute.png)

    ![image](./media/aws-multi-accounts-tutorial/i3-attribute.png)

    b. Na **nome** caixa de texto, escreva o nome de atributo apresentado para essa linha.

    c. Introduza o **espaço de nomes** valor.

    d. Selecione a origem de dado **atributo**.

    e. Partir do **atributo de origem** lista, digite o valor de atributo apresentado para essa linha.

    f. Clique em **Guardar**.

7. No **definido no início de sessão único com o SAML** página, além do **certificado de assinatura SAML** secção, clique em **transferir** para transferir o **XML de metadados de Federação**  e guarde-o no seu computador.

    ![image](./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_certificate.png) 

8. Numa janela do browser diferente, início de sessão no site da sua empresa Amazon Web Services (AWS) como administrador.

9. Clique em **AWS Home**.

    ![Configurar a página inicial do início de sessão único][11]

10. Clique em **gestão de acesso e identidades**.

    ![Configurar a identidade de início de sessão única][12]

11. Clique em **fornecedores de identidade**e, em seguida, clique em **criar fornecedor**.

    ![Configurar o fornecedor de início de sessão único][13]

12. Sobre o **configurar o fornecedor** caixa de diálogo página, execute os seguintes passos:

    ![Configurar o início de sessão único de caixa de diálogo][14]

    a. Como **tipo de fornecedor**, selecione **SAML**.

    b. Na **nome do fornecedor** caixa de texto, escreva um nome de fornecedor (por exemplo: *WAAD*).

    c. A carregar seu transferido **ficheiro de metadados** a partir do portal do Azure, clique em **Escolher ficheiro**.

    d. Clique em **passo seguinte**.

13. Sobre o **Certifique-se de informações do fornecedor** página de diálogo, clique em **criar**.

    ![Configurar o início de sessão único verificar][15]

14. Clique em **funções**e, em seguida, clique em **criar função**.

    ![Configurar funções de início de sessão únicas][16]

15. Sobre o **criar função** página, execute os seguintes passos:  

    ![Configurar a confiança de início de sessão única][19]

    a. Selecione **Federação SAML 2.0** sob **selecione o tipo de entidade fidedigna**.

    b. Sob **escolher uma secção de SAML 2.0 fornecedor**, selecione a **fornecedor de SAML** que criou anteriormente (por exemplo: *WAAD*)

    c. Selecione **permitir programática e o acesso à consola de gestão do AWS**.
  
    d. Clique em **seguinte: Permissões**.

16. Sobre o **anexar políticas de permissões** caixa de diálogo, não precisa de anexar qualquer política. Clique em **seguinte: Revisão**.  

    ![Configurar a política de início de sessão única][33]

17. Sobre o **revisão** caixa de diálogo, execute os seguintes passos:

    ![Configurar a revisão de início de sessão único][34]

    a. Na **nome da função** caixa de texto, introduza o seu nome de função.

    b. Na **descrição da função** caixa de texto, introduza a descrição.

    c. Clique em **criar função**.

    d. Crie tantas funções conforme necessário e mapeá-los para o fornecedor de identidade.

18. Terminar sessão atual conta AWS e inicie sessão com outra conta em que pretende configurar o início de sessão único com o Azure AD.

19. Execute a 9 passo a passo e 17 para criar várias funções que pretende o programa de configuração para esta conta. Se tiver mais de duas contas, execute os mesmos passos para todas as contas criar funções para os mesmos.

20. Depois de todas as funções são criadas no contas, elas aparecem a **funções** lista para essas contas.

    ![Configuração de funções](./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_listofroles.png)

21. É necessário capturar todas as funções ARN e entidades fidedignas para todas as funções entre todas as contas, que é necessário mapear manualmente com a aplicação do Azure AD. 

22. Clique em funções para copiar **função ARN** e **entidades fidedignas** valores. Vai precisar destes valores para todas as funções que precisa criar no Azure AD.

    ![Configuração de funções](./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_role_summary.png)

23. Executar o passo anterior para todas as funções em todas as contas e armazenar todos eles no formato **ARN de função, entidades fidedignas** num bloco de notas.

24. Open [do Azure AD Graph](https://developer.microsoft.com/graph/graph-explorer) na outra janela.

    a. Inicie sessão para o site de API do Graph com as credenciais de Administrador Global/Co-administradores para o seu inquilino.

    b. Tem de ter permissões suficientes para criar as funções. Clique em **modificar permissões** para obter as permissões necessárias.

    ![Caixa de diálogo Graph explorer](./media/aws-multi-accounts-tutorial/graph-explorer-new9.png)

    c. Selecione seguindo as permissões da lista (se ainda não tiver estas) e clique em "Modificar permissões" 

    ![Caixa de diálogo Graph explorer](./media/aws-multi-accounts-tutorial/graph-explorer-new10.png)

    d. Isto irá pedir-lhe para iniciar sessão novamente e aceitar o consentimento. Depois de aceitar o consentimento, esteja conectado ao Explorador do gráfico novamente.

    e. Alterar o menu suspenso de versão para **beta**. Para obter todos os principais de serviço a partir do seu inquilino, utilize a seguinte consulta:

     `https://graph.microsoft.com/beta/servicePrincipals`

    Se estiver a utilizar vários diretórios, em seguida, pode utilizar seguindo o padrão, que tem o seu domínio primário no mesmo `https://graph.microsoft.com/beta/contoso.com/servicePrincipals`

    ![Caixa de diálogo Graph explorer](./media/aws-multi-accounts-tutorial/graph-explorer-new1.png)

    f. Na lista de principais de serviço obtido, obtenha um que necessitar de modificar. Também pode utilizar Ctrl + F para pesquisar o aplicativo de todos os ServicePrincipals listadas. Pode utilizar a seguinte consulta utilizando o **id de objeto** que copiou a partir da página de propriedades do Azure AD para obter ao Principal de serviço correspondentes.

    `https://graph.microsoft.com/beta/servicePrincipals/<objectID>`.

    ![Caixa de diálogo Graph explorer](./media/aws-multi-accounts-tutorial/graph-explorer-new2.png)

    g. Extrair a propriedade appRoles do objeto principal de serviço.

    ![Caixa de diálogo Graph explorer](./media/aws-multi-accounts-tutorial/graph-explorer-new3.png)

    h. Agora precisa gerar novas funções para a sua aplicação. 

    i. Abaixo JSON é um exemplo do objeto appRoles. Crie um objeto semelhante para adicionar as funções que pretende para a sua aplicação.

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
    > Só é possível adicionar novas funções depois do **msiam_access** para a operação de correção. Além disso, pode adicionar tantas funções conforme desejado por sua organização necessitam. Azure AD irá enviar a **valor** destas funções, como o valor de afirmação na resposta SAML.

    j. Voltar à sua API do Graph e altere o método da **Obtenha** para **aplicar o PATCH**. O objeto Principal de serviço para ter pretendido funções por atualizar a propriedade appRoles semelhante ao mostrado acima no exemplo de patches. Clique em **executar consulta** para executar a operação de correção. Uma mensagem de êxito confirma a criação da função para a sua aplicação do Amazon Web Services.

    ![Caixa de diálogo Graph explorer](./media/aws-multi-accounts-tutorial/graph-explorer-new11.png)

25. Depois do Principal de serviço for corrigido com mais de funções, pode atribuir utilizadores/grupos para as respetivas funções. Isso pode ser feito ao aceder ao portal e navegar para a aplicação do Amazon Web Services. Clique nas **utilizadores e grupos** separador na parte superior. 

26. Recomendamos que crie novos grupos para cada função do AWS, de modo a que possa atribuir essa função específica desse grupo. Tenha em atenção que se trata de um para um mapeamento para um grupo a uma função. Em seguida, pode adicionar os membros que pertencem a esse grupo.

27. Assim que os grupos são criados, selecione o grupo e atribuir à aplicação.

    ![Configurar o início de sessão único adicionar](./media/aws-multi-accounts-tutorial/graph-explorer-new5.png)

> [!Note]
> Grupos aninhados não são suportados durante a atribuição de grupos.

28. Para atribuir a função ao grupo, selecione a função e clique em **atribuir** botão na parte inferior da página.

    ![Configurar o início de sessão único adicionar](./media/aws-multi-accounts-tutorial/graph-explorer-new6.png)

> [!Note]
> Tenha em atenção que terá de atualizar a sua sessão no portal do Azure para ver novas funções.

### <a name="test-single-sign-on"></a>Testar o início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico do Amazon Web Services (AWS) no painel de acesso, deve obter a página de aplicativo do Amazon Web Services (AWS) com opção de selecionar a função.

![Configurar o início de sessão único adicionar](./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_test_screen.png)

Também pode verificar a resposta SAML para ver as funções que está a ser passadas como afirmações.

![Configurar o início de sessão único adicionar](./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices(aws)_test_saml.png)

Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[11]: ./media/aws-multi-accounts-tutorial/ic795031.png
[12]: ./media/aws-multi-accounts-tutorial/ic795032.png
[13]: ./media/aws-multi-accounts-tutorial/ic795033.png
[14]: ./media/aws-multi-accounts-tutorial/ic795034.png
[15]: ./media/aws-multi-accounts-tutorial/ic795035.png
[16]: ./media/aws-multi-accounts-tutorial/ic795022.png
[17]: ./media/aws-multi-accounts-tutorial/ic795023.png
[18]: ./media/aws-multi-accounts-tutorial/ic795024.png
[19]: ./media/aws-multi-accounts-tutorial/ic795025.png
[32]: ./media/aws-multi-accounts-tutorial/ic7950251.png
[33]: ./media/aws-multi-accounts-tutorial/ic7950252.png
[35]: ./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices_provisioning.png
[34]: ./media/aws-multi-accounts-tutorial/ic7950253.png
[36]: ./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices_securitycredentials.png
[37]: ./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices_securitycredentials_continue.png
[38]: ./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices_createnewaccesskey.png
[39]: ./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices_provisioning_automatic.png
[40]: ./media/aws-multi-accounts-tutorial/tutorial_amazonwebservices_provisioning_testconnection.png
[41]: ./media/aws-multi-accounts-tutorial/
