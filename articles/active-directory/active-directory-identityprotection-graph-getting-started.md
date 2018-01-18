---
title: "Microsoft Graph para proteção de identidade do Azure Active Directory | Microsoft Docs"
description: "Saiba como consultar Microsoft Graph para obter uma lista de eventos de risco e informações associadas do Azure Active Directory."
services: active-directory
keywords: "eventos de risco, proteção de identidade do Azure Active Directory, a política de segurança, o Microsoft Graph e vulnerabilidade"
documentationcenter: 
author: MarkusVi
manager: mtillman
ms.assetid: fa109ba7-a914-437b-821d-2bd98e681386
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/08/2017
ms.author: markvi
ms.reviewer: nigu
ms.custom: seohack1
ms.openlocfilehash: df0d89fc93f1b9c19d669c29306398a8b25ee425
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/18/2018
---
# <a name="get-started-with-azure-active-directory-identity-protection-and-microsoft-graph"></a>Introdução ao Azure Active Directory Identity Protection e o Microsoft Graph
Microsoft Graph é o Microsoft unified ponto final de API e a base [do Azure Active Directory Identity Protection](active-directory-identityprotection.md) APIs. A primeira API **identityRiskEvents**, permite-lhe consultar Microsoft Graph para uma lista de [eventos de risco](active-directory-identityprotection-risk-events-types.md) e informações de associados. Este artigo obtém a começar a consultar esta API. Para uma introdução aprofundada, documentação completa e acesso para o Explorador do gráfico, consulte o [site Microsoft Graph](https://graph.microsoft.io/).


Existem quatro passos para aceder aos dados de proteção de identidade através do Microsoft Graph:

1. Obter o seu nome de domínio.
2. Crie um novo registo de aplicação. 
2. Utilize este segredo e alguns outros tipos de informações para autenticar para o Microsoft Graph, onde recebe um token de autenticação. 
3. Utilize este token para fazer pedidos para o ponto final de API e voltar a dados da proteção de identidade.

Antes de começar, terá de:

* Privilégios de administrador para criar a aplicação no Azure AD
* O nome de domínio do inquilino (por exemplo, contoso.onmicrosoft.com)


## <a name="retrieve-your-domain-name"></a>Obter o seu nome de domínio 

1. [Inicie sessão no](https://portal.azure.com) ao portal do Azure como administrador. 

2. No painel de navegação esquerdo, clique em **do Active Directory**. 
   
    ![Criar uma aplicação](./media/active-directory-identityprotection-graph-getting-started/41.png)


3. No **gerir** secção, clique em **propriedades**.

    ![Criar uma aplicação](./media/active-directory-identityprotection-graph-getting-started/42.png)

4. Copie o seu nome de domínio.


## <a name="create-a-new-app-registration"></a>Criar um novo registo de aplicação

1. No **do Active Directory** na página de **gerir** secção, clique em **registos de aplicação**.

    ![Criar uma aplicação](./media/active-directory-identityprotection-graph-getting-started/42.png)


2. No menu na parte superior, clique em **novo registo de aplicação**.
   
    ![Criar uma aplicação](./media/active-directory-identityprotection-graph-getting-started/43.png)

3. No **criar** página, execute os seguintes passos:
   
    ![Criar uma aplicação](./media/active-directory-identityprotection-graph-getting-started/44.png)

    a. No **nome** caixa de texto, escreva um nome para a sua aplicação (por ex.: aplicação de API de eventos de risco AADIP).
   
    b. Como **tipo**, selecione **aplicação Web e / ou Web API**.
   
    c. No **URL de início de sessão** caixa de texto, tipo `http://localhost`.

    d. Clique em **Criar**.

4. Para abrir o **definições** página, na lista de aplicações, clique em seu registo de aplicação criado recentemente. 

5. Copiar o **ID da aplicação**.


## <a name="grant-your-application-permission-to-use-the-api"></a>Conceder permissão para utilizar a API a aplicação

1. No **definições** página, clique em **as permissões necessárias**.
   
    ![Criar uma aplicação](./media/active-directory-identityprotection-graph-getting-started/15.png)

2. No **as permissões necessárias** página, na barra de ferramentas na parte superior, clique em **adicionar**.
   
    ![Criar uma aplicação](./media/active-directory-identityprotection-graph-getting-started/16.png)
   
3. No **acesso à API adicionar** página, clique em **selecionar um API**.
   
    ![Criar uma aplicação](./media/active-directory-identityprotection-graph-getting-started/17.png)

4. No **selecionar um API** página, selecione **Microsoft Graph**e, em seguida, clique em **selecione**.
   
    ![Criar uma aplicação](./media/active-directory-identityprotection-graph-getting-started/18.png)

5. No **acesso à API adicionar** página, clique em **selecionar permissões**.
   
    ![Criar uma aplicação](./media/active-directory-identityprotection-graph-getting-started/19.png)

6. No **ativar o acesso ao** página, clique em **ler todas as informações de risco de identidade**e, em seguida, clique em **selecione**.
   
    ![Criar uma aplicação](./media/active-directory-identityprotection-graph-getting-started/20.png)

7. No **acesso à API adicionar** página, clique em **feito**.
   
    ![Criar uma aplicação](./media/active-directory-identityprotection-graph-getting-started/21.png)

8. No **permissões obrigatórias** página, clique em **conceder permissões**e, em seguida, clique em **Sim**.
   
    ![Criar uma aplicação](./media/active-directory-identityprotection-graph-getting-started/22.png)



## <a name="get-an-access-key"></a>Obter uma chave de acesso

1. No **definições** página, clique em **chaves**.
   
    ![Criar uma aplicação](./media/active-directory-identityprotection-graph-getting-started/23.png)

2. No **chaves** página, execute os seguintes passos:
   
    ![Criar uma aplicação](./media/active-directory-identityprotection-graph-getting-started/24.png)

    a. No **descrição da chave** caixa de texto, digite uma descrição (por exemplo, *eventos de risco AADIP*).
    
    b. Como **duração**, selecione **no 1 ano**.

    c. Clique em **Guardar**.
   
    d. Copie o valor da chave e, em seguida, cole-o numa localização segura.   
   
   > [!NOTE]
   > Se perder a esta chave, terá de voltar a esta secção e crie uma nova chave. Manter esta chave de segredo: qualquer pessoa que tenha pode aceder aos seus dados.
   > 
   > 

## <a name="authenticate-to-microsoft-graph-and-query-the-identity-risk-events-api"></a>Autenticar para o Microsoft Graph e consultar a API de eventos de risco de identidade
Neste momento, é necessário:

- O nome de domínio do inquilino

- O ID de cliente 

- A chave 


Para autenticar, enviar um pedido post para `https://login.microsoft.com` com os seguintes parâmetros no corpo do:

- grant_type: "**client_credentials**"

-  recurso: "**https://graph.microsoft.com**"

- client_id: \<seu ID de cliente\>

- client_secret: \<a chave\>


Se tiver êxito, esta ação devolve um token de autenticação.  
Para chamar a API, crie um cabeçalho com o seguinte parâmetro:

    `Authorization`=”<token_type> <access_token>"


Quando a autenticação, pode encontrar o tipo de token e um token de acesso no token devolvido.

Envie este cabeçalho como um pedido para o seguinte URL de API:`https://graph.microsoft.com/beta/identityRiskEvents`

A resposta, se tiver êxito, é uma coleção de eventos de risco de identidade e dados associados no formato JSON de OData, que pode ser analisado e processado como julgar.

Eis o código de exemplo para autenticar e chamar a API utilizando o PowerShell.  
Basta adicione o seu ID de cliente, a chave secreta e o domínio de inquilino.

    $ClientID       = "<your client ID here>"        # Should be a ~36 hex character string; insert your info here
    $ClientSecret   = "<your client secret here>"    # Should be a ~44 character string; insert your info here
    $tenantdomain   = "<your tenant domain here>"    # For example, contoso.onmicrosoft.com

    $loginURL       = "https://login.microsoft.com"
    $resource       = "https://graph.microsoft.com"

    $body       = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}
    $oauth      = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body

    Write-Output $oauth

    if ($oauth.access_token -ne $null) {
        $headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}

        $url = "https://graph.microsoft.com/beta/identityRiskEvents"
        Write-Output $url

        $myReport = (Invoke-WebRequest -UseBasicParsing -Headers $headerParams -Uri $url)

        foreach ($event in ($myReport.Content | ConvertFrom-Json).value) {
            Write-Output $event
        }

    } else {
        Write-Host "ERROR: No Access Token"
    } 


## <a name="next-steps"></a>Passos Seguintes

Parabéns, efetuadas apenas a primeira chamada para o Microsoft Graph!  
Agora pode consultar eventos de risco de identidade e utilizar os dados, no entanto, julgar.

Para obter mais informações sobre como criar aplicações utilizando a Graph API e o Microsoft Graph, veja o [documentação](https://graph.microsoft.io/docs) e muito mais informações sobre o [site Microsoft Graph](https://graph.microsoft.io/). Além disso, certifique-se marcar o [API do Azure AD Identity Protection](https://graph.microsoft.io/docs/api-reference/beta/resources/identityprotection_root) página que apresenta uma lista de todas as APIs de proteção de identidade disponíveis no gráfico. À medida que adiciona novas formas de trabalhar com a proteção de identidade através de API, irá vê-los nessa página.

Para informações relacionadas, consulte:

-  [Proteção de identidade do Azure Active Directory](active-directory-identityprotection.md)

-  [Tipos de eventos de risco detetados pelo Azure Active Directory Identity Protection](active-directory-identityprotection-risk-events-types.md)

- [Microsoft Graph](https://graph.microsoft.io/)

- [Descrição geral do Microsoft Graph](https://graph.microsoft.io/docs)

- [Raiz do serviço do Azure AD Identity Protection](https://graph.microsoft.io/docs/api-reference/beta/resources/identityprotection_root)

