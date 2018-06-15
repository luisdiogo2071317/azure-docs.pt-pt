---
title: Pedidos e respostas de autenticação
description: Autenticar para o AD para utilizar o Cofre de chaves
services: key-vault
documentationcenter: ''
author: lleonard-msft
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 4c321939-8a5b-42ca-83c4-2f5f647ca13e
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/09/2018
ms.author: alleonar
ms.openlocfilehash: 94080fb124478a4b8e196e341c335ca32321ecdf
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
ms.locfileid: "34012132"
---
# <a name="authentication-requests-and-responses"></a>Pedidos e respostas de autenticação

O Cofre de chaves do Azure suporta pedidos de formatação JSON e respostas. Pedidos para o Cofre de chaves do Azure são direcionados para um válido utilizando o URL do Cofre de chave do Azure que HTTPS com alguns parâmetros de URL e JSON codificado corpos de pedido e resposta.

Este tópico aborda informações específicas para o serviço Cofre de chaves do Azure. Para obter informações gerais sobre como utilizar interfaces de REST do Azure, incluindo autenticação/autorização e como adquirir um token de acesso, consulte [referência da API REST do Azure](https://docs.microsoft.com/rest/api/).

## <a name="request-url"></a>URL do pedido  
 Operações de gestão de chaves utilizam HTTP DELETE, GET, PATCHES, PUT e HTTP POST e operações criptográficas contra objetos da chave existentes utilizam HTTP POST. Clientes que não suportam verbos HTTP específicos, também podem utilizar POST de HTTP utilizando o cabeçalho X-HTTP-pedido para especificar o verbo pretendido; pedidos que normalmente não exigir um corpo devem incluir uma mensagem vazia ao utilizar o HTTP POST, por exemplo, quando a utilização do POST em vez de eliminação.  

 Para trabalhar com objetos no Cofre de chaves do Azure, seguem-se os URLs de exemplo:  

-   Para criar uma chave denominada TESTKEY em utilização um cofre de chaves- `PUT /keys/TESTKEY?api-version=<api_version> HTTP/1.1`  

-   Para importar uma chave denominada IMPORTEDKEY para utilizar um cofre de chaves- `POST /keys/IMPORTEDKEY/import?api-version=<api_version> HTTP/1.1`  

-   Para obter um segredo chamado MYSECRET em utilização um cofre de chaves- `GET /secrets/MYSECRET?api-version=<api_version> HTTP/1.1`  

-   Para ASSINAR um resumo de utilizar uma chave denominada TESTKEY em utilização um cofre de chaves- `POST /keys/TESTKEY/sign?api-version=<api_version> HTTP/1.1`  

 A autoridade para um pedido para um cofre de chaves é sempre da seguinte forma,  `https://{keyvault-name}.vault.azure.net/`  

 As chaves são sempre armazenadas sob o caminho de /keys, segredos são sempre armazenados sob o caminho de /secrets.  

## <a name="api-version"></a>Versão da API  
 O serviço de Cofre de chaves do Azure suporta o controlo de versões de protocolo para proporcionar compatibilidade com clientes de nível inferior, embora não todas as funcionalidades estarão disponíveis para esses clientes. Os clientes têm de utilizar o `api-version` consultar o parâmetro de cadeia para especificar a versão do protocolo que suportam como não têm predefinições.  

 Versões de protocolo do Cofre de chaves do Azure, siga uma data numeração esquema utilizando {aaaa}. {MM}. Formato de {DD}.  

## <a name="request-body"></a>Corpo do Pedido  
 De acordo com a especificação de HTTP, operações GET não pode ter um corpo do pedido e operações POST e PUT tem de ter um corpo do pedido. O corpo em operações de eliminação é opcional de HTTP.  

 Exceto indicação em contrário na descrição da operação, o tipo de conteúdo de corpo de pedido tem de ser application/json e tem de conter um compatível de objeto JSON serializado para o tipo de conteúdo.  

 Exceto indicação em contrário na descrição da operação, o cabeçalho do pedido aceitar tem de conter o tipo de suporte de dados de aplicação/json.  

## <a name="response-body"></a>Corpo da Resposta  
 Exceto indicação em contrário na descrição da operação, o tipo de conteúdo de corpo de resposta de operações com êxito ou falhadas será application/json e contém informações de erro detalhadas.  

## <a name="using-http-post"></a>Utilizando o HTTP POST  
 Alguns clientes podem não conseguir utilizam determinados verbos HTTP, tal como PATCHES ou eliminar. O Cofre de chaves do Azure suporta HTTP POST como alternativa para estes clientes desde que o cliente também inclui o cabeçalho "X-HTTP-METHOD" para o verbo específicos a HTTP original. Suporte para o POST de HTTP é indicado para cada uma das API definida neste documento.  

## <a name="error-responses"></a>Respostas de erro  
 Processamento de erros irá utilizar códigos de estado HTTP. Resultados comuns são:  

-   2xx – êxito: utilizado para o funcionamento normal. O corpo da resposta irá conter o resultado esperado  

-   3xx – redirecionamento: 304 o "não modificado" pode ser devolvido para satisfazer um GET condicional. Outros códigos 3xx podem ser utilizados no futuro para indicar as alterações DNS e caminho.  

-   4xx – erro de cliente: utilizado para pedidos incorretos, chaves em falta, erros de sintaxe, parâmetros inválidos, erros de autenticação, etc. O corpo da resposta irá conter explicação de erro detalhadas.  

-   5XX – erro de servidor: utilizado para erros de servidor interno. O corpo da resposta irá conter informações de erro resumidos.  

 O sistema é concebido para trabalhar atrás de um proxy ou firewall. Por conseguinte, um cliente pode receber outros códigos de erro.  

 O Cofre de chaves do Azure também devolve informações de erro no corpo da resposta quando ocorre um problema. O corpo da resposta é formatada JSON e assume a forma:  

```  

{  
  "error":  
  {  
    "code": "BadArgument",  
    "message":  

      "’Foo’ is not a valid argument for ‘type’."  
    }  
  }  
}  

```  

## <a name="authentication"></a>Autenticação  
 Todos os pedidos para o Cofre de chave do Azure deve ser autenticado. O Cofre de chaves do Azure suporta os tokens de acesso do Azure Active Directory que podem ser obtidos utilizando o OAuth2 [[especificação RFC6749](http://tools.ietf.org/html/rfc6749)]. 
 
 Para obter mais informações sobre a registar a sua aplicação e da autenticação a utilizar o Cofre de chaves do Azure, consulte [registar a aplicação de cliente com o Azure AD](https://docs.microsoft.com/rest/api/index#register-your-client-application-with-azure-ad).
 
 Tokens de acesso tem de ser enviados para o serviço utilizando o cabeçalho de autorização de HTTP:  

```  
PUT /keys/MYKEY?api-version=<api_version>  HTTP/1.1  
Authorization: Bearer <access_token>  

```  

 Quando não é fornecido um token de acesso, ou quando um token não é aceite pelo serviço, um erro de HTTP 401 será devolvido ao cliente e incluirá o cabeçalho WWW-Authenticate, por exemplo:  

```  
401 Not Authorized  
WWW-Authenticate: Bearer authorization="…", resource="…"  

```  

 Os parâmetros no cabeçalho WWW-Authenticate são:  

-   autorização: O endereço do serviço de autorização de OAuth2 que pode ser utilizado para obter um token de acesso para o pedido.  

-   recursos: O nome do recurso para utilizar no pedido de autorização.  

## <a name="see-also"></a>Consultar Também  
 [Sobre chaves, segredos e certificados](about-keys-secrets-and-certificates.md)
