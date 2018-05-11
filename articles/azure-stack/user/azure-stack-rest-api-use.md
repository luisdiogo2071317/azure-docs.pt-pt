---
title: Utilize a pilha de Azure API | Microsoft Docs
description: Saiba como obter uma autenticação do Azure para tornar os pedidos de API de pilha do Azure.
services: azure-stack
documentationcenter: ''
author: cblackuk
manager: femila
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2018
ms.author: mabrigg
ms.reviewer: sijuman
ms.openlocfilehash: 2bbfe4f829ad5c42a5742fdf08f2d185af627f42
ms.sourcegitcommit: d28bba5fd49049ec7492e88f2519d7f42184e3a8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/11/2018
---
<!--  cblackuk and charliejllewellyn -->

# <a name="use-the-azure-stack-api"></a>Utilize a API de pilha do Azure

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

Pode utilizar a API de pilha do Azure para automatizar operações como syndicating itens do marketplace.

Utilizando a API necessita que o cliente autenticar relativamente ao ponto de final de início de sessão do Microsoft Azure. O ponto final devolve um token para utilizar no cabeçalho de cada pedido enviado para a API de pilha do Azure. (Microsoft Azure utiliza o Oauth 2.0.)

Este artigo fornece exemplos que utilizam o utilitário de curl para criar pedidos de pilha do Azure. Estes exemplos guiá-lo durante o processo de obtenção de um token para aceder à API de pilha do Azure. A maioria das linguagens de programação fornecem bibliotecas de Oauth 2.0, que tem robustas tarefas de gestão e o identificador de token essa atualização do token.

Observar todo o processo de utilizando a API de REST de pilha do Azure com um cliente REST genérico, como curl pode ajudar a compreender a subjacentes pedidos e mostra o que pode esperar receber num payload resposta.

Este artigo não explorar todas as opções disponíveis para obter os tokens, tais como o início de sessão interativo ou criar dedicado IDs de aplicações. Para obter mais informações, consulte [referência da API REST do Azure](https://docs.microsoft.com/rest/api/).

## <a name="get-a-token-from-azure"></a>Obter um token a partir do Azure

Criar um pedido de *corpo* formatado utilizando o tipo de conteúdo x-www-form-urlencoded para obter um token de acesso. APÓS o seu pedido para o ponto final de início de sessão e autenticação de REST do Azure.

```
POST https://login.microsoftonline.com/{tenant id}/oauth2/token
```

**ID de inquilino** encontra-se:

* O domínio de inquilino, tais como fabrikam.onmicrosoft.com
* O ID de inquilino, tais como 8eaed023-2b34-4da1-9baa-8bc8c9d6a491
* Valor predefinido para chaves de inquilino independente: comuns

### <a name="post-body"></a>Corpo da mensagem

```
grant_type=password
&client_id=1950a258-227b-4e31-a9cf-717495945fc2
&resource=https://contoso.onmicrosoft.com/4de154de-f8a8-4017-af41-df619da68155
&username=admin@fabrikam.onmicrosoft.com
&password=Password123
&scope=openid
```

Para cada valor:

  **grant_type**

  O tipo de esquema de autenticação que irá utilizar. Neste exemplo, o valor é:

  ```
  password
  ```

  **resource**

  O recurso acede ao token. Pode encontrar o recurso consultando o ponto de final de metadados da gestão de pilha do Azure. Observe o **audiências** secção

  O ponto final de gestão de pilha do Azure:

  ```
  https://management.{region}.{Azure Stack domain}/metadata/endpoints?api-version=2015-01-01
  ```

 > [!NOTE]
 > Se for um administrador tenta aceder o API de inquilinos, em seguida, deve certificar-se de que utiliza o ponto final de inquilino, por exemplo: 'https://adminmanagement.{region}.{Azure domínio pilha} / metadados/pontos finais? api-version = 2015-01-011

  Por exemplo, com o Azure pilha Development Kit como um ponto final:

  ```
  curl 'https://management.local.azurestack.external/metadata/endpoints?api-version=2015-01-01'
  ```

  Resposta:

  ```
  {
  "galleryEndpoint":"https://adminportal.local.azurestack.external:30015/",
  "graphEndpoint":"https://graph.windows.net/",
  "portalEndpoint":"https://adminportal.local.azurestack.external/",
  "authentication":{
      "loginEndpoint":"https://login.windows.net/",
      "audiences":["https://contoso.onmicrosoft.com/4de154de-f8a8-4017-af41-df619da68155"]
      }
  }
  ```

### <a name="example"></a>Exemplo

  ```
  https://contoso.onmicrosoft.com/4de154de-f8a8-4017-af41-df619da68155
  ```

  **client_id**

  Este valor é codificado para um valor predefinido:

  ```
  1950a258-227b-4e31-a9cf-717495945fc2
  ```

  Alternativas opções estão disponíveis para cenários específicos:

  
  | Aplicação | ApplicationID |
  | --------------------------------------- |:-------------------------------------------------------------:|
  | LegacyPowerShell | 0a7bdc5c-7b57-40be-9939-d4c5fc7cd417 |
  | PowerShell | 1950a258-227b-4e31-a9cf-717495945fc2 |
  | WindowsAzureActiveDirectory | 00000002-0000-0000-c000-000000000000 |
  | VisualStudio | 872cd9fa-d31f-45e0-9eab-6e460a02d1f1 |
  | AzureCLI | 04b07795-8ddb-461a-bbee-02f9e1bf7b46 |

  **username**

  Por exemplo, a conta do AAD de pilha do Azure:

  ```
  azurestackadmin@fabrikam.onmicrosoft.com
  ```

  **password**

  A palavra-passe de administrador de Azure pilha AAD.

### <a name="example"></a>Exemplo

Pedido:

```
curl -X "POST" "https://login.windows.net/fabrikam.onmicrosoft.com/oauth2/token" \
-H "Content-Type: application/x-www-form-urlencoded" \
--data-urlencode "client_id=1950a258-227b-4e31-a9cf-717495945fc2" \
--data-urlencode "grant_type=password" \
--data-urlencode "username=admin@fabrikam.onmicrosoft.com" \
--data-urlencode 'password=Password12345' \
--data-urlencode "resource=https://contoso.onmicrosoft.com/4de154de-f8a8-4017-af41-df619da68155"
```

Resposta:

```
{
  "token_type": "Bearer",
  "scope": "user_impersonation",
  "expires_in": "3599",
  "ext_expires_in": "0",
  "expires_on": "1512574780",
  "not_before": "1512570880",
  "resource": "https://contoso.onmicrosoft.com/4de154de-f8a8-4017-af41-df619da68155",
  "access_token": "eyJ0eXAiOi...truncated for readability..."
}
```

## <a name="api-queries"></a>Consultas de API

Depois de obter o token de acesso, terá de adicioná-lo como um cabeçalho para cada um dos seus pedidos de API. Para tal, terá de criar um cabeçalho **autorização** com o valor: `Bearer <access token>`. Por exemplo:

Pedido:

```
curl -H "Authorization: Bearer eyJ0eXAiOi...truncated for readability..." 'https://adminmanagement.local.azurestack.external/subscriptions?api-version=2016-05-01'
```

Resposta:

```
offerId : /delegatedProviders/default/offers/92F30E5D-F163-4C58-8F02-F31CFE66C21B
id : /subscriptions/800c4168-3eb1-406b-a4ca-919fe7ee42e8
subscriptionId : 800c4168-3eb1-406b-a4ca-919fe7ee42e8
tenantId : 9fea4606-7c07-4518-9f3f-8de9c52ab628
displayName : Default Provider Subscription
state : Enabled
subscriptionPolicies : @{locationPlacementId=AzureStack}
```

### <a name="url-structure-and-query-syntax"></a>Sintaxe de estrutura e a consulta de URL

Pedido de genérico URI, é composta por: {esquema de URI} :// {URI-host} / {caminho do recurso}? {query-string}

- **O esquema de URI**:  
O URI indica o protocolo utilizado para enviar o pedido. Por exemplo, `http` ou `https`.
- **Anfitrião URI**:  
O anfitrião Especifica o nome de domínio ou endereço IP do servidor onde o ponto final de serviço REST está alojado, tais como `graph.microsoft.com` ou `adminmanagement.local.azurestack.external`.
- **Caminho do recurso**:  
Especifica o caminho do recurso ou coleção de recursos, o que pode incluir vários segmentos utilizados pelo serviço de determinar a seleção desses recursos. Por exemplo: `beta/applications/00003f25-7e1f-4278-9488-efc7bac53c4a/owners` podem ser utilizadas para consultar a lista de proprietários de uma aplicação específica dentro da coleção de aplicações.
- **Cadeia de consulta**:  
A cadeia fornece parâmetros simples adicionais, tais como os critérios de seleção de recurso ou a versão da API.

## <a name="azure-stack-request-uri-construct"></a>Construção URI de pedido de pilha do Azure

```
{URI-scheme} :// {URI-host} / {subscription id} / {resource group} / {provider} / {resource-path} ? {OPTIONAL: filter-expression} {MANDATORY: api-version}
```

### <a name="uri-syntax"></a>Sintaxe URI

```
https://adminmanagement.local.azurestack.external/{subscription id}/resourcegroups/{resource group}/providers/{provider}/{resource-path}?{api-version}
```

### <a name="query-uri-example"></a>Exemplo URI de consulta

```
https://adminmanagement.local.azurestack.external/subscriptions/800c4168-3eb1-406b-a4ca-919fe7ee42e8/resourcegroups/system.local/providers/microsoft.infrastructureinsights.admin/regionhealths/local/Alerts?$filter=(Properties/State eq 'Active') and (Properties/Severity eq 'Critical')&$orderby=Properties/CreatedTimestamp desc&api-version=2016-05-01"
```

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre como utilizar os pontos finais RESTful do Azure, consulte [referência da API REST do Azure](https://docs.microsoft.com/rest/api/).
