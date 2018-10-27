---
title: Microsoft Graph para o Azure Active Directory Identity Protection | Documentos da Microsoft
description: Saiba como consultar o Microsoft Graph para obter uma lista de eventos de risco e informações associadas do Azure Active Directory.
services: active-directory
keywords: proteção de identidade do Azure Active Directory, de evento de risco, de vulnerabilidade, de política de segurança, Microsoft Graph
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: fa109ba7-a914-437b-821d-2bd98e681386
ms.service: active-directory
ms.component: conditional-access
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/26/2018
ms.author: markvi
ms.reviewer: nigu
ms.custom: seohack1
ms.openlocfilehash: eefbfcf741db3d0949910bc5da8494e087c39ec7
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/26/2018
ms.locfileid: "50139157"
---
# <a name="get-started-with-azure-active-directory-identity-protection-and-microsoft-graph"></a>Introdução ao Azure Active Directory Identity Protection e Microsoft Graph
Microsoft Graph é o Microsoft unified ponto final de API e a casa dos [do Azure Active Directory Identity Protection](../active-directory-identityprotection.md) APIs. A primeira API **identityRiskEvents**, permite-lhe consultar o Microsoft Graph para obter uma lista de [eventos de risco](../reports-monitoring/concept-risk-events.md) e informações de associados. Este artigo permite-lhe começar a consultar esta API. Para uma introdução aprofundada, a documentação completa e acesso para a API do Graph, consulte a [site do Microsoft Graph](https://graph.microsoft.io/).


Existem quatro passos para aceder aos dados de proteção de identidade através do Microsoft Graph:

1. Obter o seu nome de domínio.
2. Crie um novo registo de aplicação. 
2. Utilize este segredo e algumas outras partes de informações para autenticar para o Microsoft Graph, onde recebe um token de autenticação. 
3. Utilize este token para fazer pedidos para o ponto final de API e obter dados de proteção de identidade.

Antes de começar, terá de:

- Um inquilino do Azure AD P2

- Privilégios de administrador para criar a aplicação no Azure AD

- O nome de domínio do seu inquilino (por exemplo, contoso.onmicrosoft.com)


## <a name="retrieve-your-domain-name"></a>Obter o seu nome de domínio 

1. [Inicie sessão no](https://portal.azure.com) ao seu portal do Azure como administrador. 

2. No painel de navegação esquerdo, clique em **do Active Directory**. 
   
    ![Criar uma aplicação](./media/graph-get-started/41.png)

3. Clique em **nomes de domínio personalizado**.

    ![Nomes de domínio personalizados](./media/graph-get-started/71.png)

4. Na lista de nomes de domínio, copie o nome de domínio que é sinalizado como primário.

    ![Nomes de domínio personalizados](./media/graph-get-started/72.png)



## <a name="create-a-new-app-registration"></a>Criar um novo registo de aplicação

1. No **do Active Directory** página, além da **gerir** secção, clique em **registos das aplicações**.

    ![Criar uma aplicação](./media/graph-get-started/42.png)


2. No menu na parte superior, clique em **novo registo de aplicação**.
   
    ![Criar uma aplicação](./media/graph-get-started/43.png)

3. Sobre o **criar** página, execute os seguintes passos:
   
    ![Criar uma aplicação](./media/graph-get-started/44.png)

    a. Na **nome** caixa de texto, escreva um nome para a sua aplicação (por exemplo: aplicação de API de eventos de risco AADIP).
   
    b. Como **tipo de aplicação**, selecione **aplicação Web e / ou Web API**.
   
    c. Na **URL de início de sessão** caixa de texto, tipo `http://localhost`.

    d. Clique em **Criar**.

4. Para abrir o **definições** página, na lista de aplicações, clique em seu registo de aplicações criada recentemente. 

5. Copiar o **ID da aplicação**.


## <a name="grant-your-application-permission-to-use-the-api"></a>Conceder permissão de aplicação para utilizar a API

1. Sobre o **definições** página, clique em **permissões obrigatórias**.
   
    ![Criar uma aplicação](./media/graph-get-started/15.png)

2. Sobre o **permissões obrigatórias** página, na barra de ferramentas na parte superior, clique em **Add**.
   
    ![Criar uma aplicação](./media/graph-get-started/16.png)
   
3. Sobre o **adicionar acesso à API** página, clique em **selecionar uma API**.
   
    ![Criar uma aplicação](./media/graph-get-started/17.png)

4. Sobre o **selecionar uma API** , selecione **Microsoft Graph**e, em seguida, clique em **selecionar**.
   
    ![Criar uma aplicação](./media/graph-get-started/18.png)

5. Sobre o **adicionar acesso à API** página, clique em **selecionar permissões**.
   
    ![Criar uma aplicação](./media/graph-get-started/19.png)

6. Sobre o **ativar o acesso ao** página, clique em **ler todas as informações de risco de identidade**e, em seguida, clique em **selecionar**.
   
    ![Criar uma aplicação](./media/graph-get-started/20.png)

7. Sobre o **adicionar acesso à API** página, clique em **feito**.
   
    ![Criar uma aplicação](./media/graph-get-started/21.png)

8. Sobre o **permissões obrigatórias** página, clique em **conceder permissões**e, em seguida, clique em **Sim**.
   
    ![Criar uma aplicação](./media/graph-get-started/22.png)



## <a name="get-an-access-key"></a>Obter uma chave de acesso

1. Sobre o **definições** página, clique em **chaves**.
   
    ![Criar uma aplicação](./media/graph-get-started/23.png)

2. Sobre o **chaves** página, execute os seguintes passos:
   
    ![Criar uma aplicação](./media/graph-get-started/24.png)

    a. Na **descrição da chave** caixa de texto, digite uma descrição (por exemplo, *evento de risco AADIP*).
    
    b. Como **duração**, selecione **em 1 ano**.

    c. Clique em **Guardar**.
   
    d. Copie o valor da chave e, em seguida, cole-o num local seguro.   
   
   > [!NOTE]
   > Se perder a esta chave, terá de voltar a esta secção e crie uma nova chave. Guarde esta chave um segredo: qualquer pessoa que tenha ele pode aceder aos seus dados.
   > 
   > 

## <a name="authenticate-to-microsoft-graph-and-query-the-identity-risk-events-api"></a>Autenticar para o Microsoft Graph e consultar a API de eventos de risco de identidade
Neste ponto, deve ter:

- O nome de domínio do seu inquilino

- O ID de cliente 

- A chave 


Para se autenticar, enviar um pedido post para `https://login.microsoft.com` com os seguintes parâmetros no corpo:

- grant_type: "**client_credentials**"

-  recurso: "**https://graph.microsoft.com**"

- client_id: \<seu ID de cliente\>

- client_secret: \<sua chave\>


Se tiver êxito, esta ação devolve um token de autenticação.  
Para chamar a API, crie um cabeçalho com o seguinte parâmetro:

    `Authorization`=”<token_type> <access_token>"


Durante a autenticação, pode encontrar o tipo de token e o token de acesso no token retornado.

Envie este cabeçalho como um pedido para o seguinte URL de API: `https://graph.microsoft.com/beta/identityRiskEvents`

A resposta, se tiver êxito, é uma coleção de eventos de risco de identidade e os dados associados no formato JSON de OData, que pode ser analisado e manipulado como quiser.

Aqui está o código de exemplo para autenticar e chamar a API com o PowerShell.  
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

Parabéns, acabou de criar a primeira chamada para o Microsoft Graph!  
Agora pode consultar os eventos de risco de identidade e utilizar os dados, no entanto, o que considere adequado.

Para saber mais sobre o Microsoft Graph e como criar aplicações com a Graph API, consulte a [documentação](https://graph.microsoft.io/docs) e muito mais sobre o [site do Microsoft Graph](https://graph.microsoft.io/). Além disso, certifique-se aos Favoritos para o [API do Azure AD Identity Protection](https://graph.microsoft.io/docs/api-reference/beta/resources/identityprotection_root) página que apresenta uma lista de todas as APIs de proteção de identidade disponíveis no gráfico. À medida que adicionamos novas formas de trabalhar com a proteção de identidade através da API, irá vê-los nessa página.

Para obter informações relacionadas, consulte:

-  [O Azure Active Directory Identity Protection](../active-directory-identityprotection.md)

-  [Tipos de eventos de risco detetados pelo Azure Active Directory Identity Protection](../reports-monitoring/concept-risk-events.md)

- [Microsoft Graph](https://graph.microsoft.io/)

- [Descrição Geral do Microsoft Graph](https://graph.microsoft.io/docs)

- [Raiz de serviço do Azure AD Identity Protection](https://graph.microsoft.io/docs/api-reference/beta/resources/identityprotection_root)

