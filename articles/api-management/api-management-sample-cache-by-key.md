---
title: A colocação em cache personalizada na API Management do Azure
description: Saiba como colocar em cache itens por chave na API Management do Azure
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: 772bc8dd-5cda-41c4-95bf-b9f6f052bc85
ms.service: api-management
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 838850d38c9df51fabcf620831371bed401e9492
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/21/2018
ms.locfileid: "29376036"
---
# <a name="custom-caching-in-azure-api-management"></a>A colocação em cache personalizada na API Management do Azure
Serviço de API Management do Azure têm suporte incorporado para [colocação em cache de resposta HTTP](api-management-howto-cache.md) utilizando o URL de recurso como a chave. A chave pode ser modificada por cabeçalhos de pedido utilizando o `vary-by` propriedades. Isto é útil para colocar em cache as respostas HTTP completos (aka representações), mas por vezes, é útil para colocar em cache apenas uma parte de uma representação. A nova [valor de pesquisa de cache](https://msdn.microsoft.com/library/azure/dn894086.aspx#GetFromCacheByKey) e [valor de arquivo de cache](https://msdn.microsoft.com/library/azure/dn894086.aspx#StoreToCacheByKey) políticas fornecem a capacidade de armazenar e obter arbitrários peças de dados a partir de definições de política. Esta capacidade também adiciona valor introduzidas anteriormente [pedido de envio](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) política porque agora pode colocar em cache as respostas de serviços externos.

## <a name="architecture"></a>Arquitetura
Dados em cache utiliza de serviço de API Management um dados partilhados por inquilino colocados em cache para que, como escala até várias unidades ainda obtiver acesso ao mesmo. No entanto, ao trabalhar com uma implementação de multirregião existem caches independentes em cada uma das regiões. É importante tratar não a cache como um arquivo de dados, onde é a única origem de algumas peça de informações. Se e mais tarde decidir tirar partido da implementação multirregião, os clientes com utilizadores que viajam poderão perder acesso a dados em cache.

## <a name="fragment-caching"></a>A colocação em cache de fragmento
Existem alguns casos em que as respostas a ser devolvidas contenham uma parte de dados é dispendioso determinar e ainda permanecem raiz para um período de tempo razoável. Por exemplo, considere um serviço criado por uma companhia aérea que fornece informações relacionadas com reservas de voo, estado de voo, etc. Se o utilizador for um membro do programa de pontos airlines, estes também teria informações relacionadas com o respetivo estado atual e acumulados mileage. Estas informações relacionadas com o utilizador poderão ser armazenadas num sistema diferente, mas pode ser preferível incluí-la no devolvido sobre o estado de trânsito e reservas de respostas. Isto pode ser feito utilizando um processo denominado fragmento a colocação em cache. A representação primária pode ser devolvida a partir do servidor de origem utilizar algum tipo de token para indicar onde as informações relacionadas com o utilizador estão a ser inserido. 

Considere a seguinte resposta JSON de uma API de back-end.

```json
{
  "airline" : "Air Canada",
  "flightno" : "871",
  "status" : "ontime",
  "gate" : "B40",
  "terminal" : "2A",
  "userprofile" : "$userprofile$"
}  
```

E recurso secundário em `/userprofile/{userid}` parecido,

```json
{ "username" : "Bob Smith", "Status" : "Gold" }
```

Para determinar as informações de utilizador adequada para incluir, a API Management tem de identificar quem é o utilizador final. Este mecanismo é dependente de implementação. Por exemplo, estiver a utilizar o `Subject` de afirmações de um `JWT` token. 

```xml
<set-variable
  name="enduserid"
  value="@(context.Request.Headers.GetValueOrDefault("Authorization","").Split(' ')[1].AsJwt()?.Subject)" />
```

Arquivos de API Management do `enduserid` valor de uma variável de contexto para utilização posterior. O passo seguinte consiste em determinar se um pedido de anterior tiver já obteve as informações de utilizador e guardá-la na cache. Para tal, utiliza a API Management do `cache-lookup-value` política.

```xml
<cache-lookup-value
key="@("userprofile-" + context.Variables["enduserid"])"
variable-name="userprofile" />
```

Se não houver nenhuma entrada na cache que corresponde ao valor da chave de, em seguida, não `userprofile` variável de contexto é criada. API Management verifica a taxa de êxito de utilizar a pesquisa de `choose` política de fluxo de controlo.

```xml
<choose>
    <when condition="@(!context.Variables.ContainsKey("userprofile"))">
        <!-- If the userprofile context variable doesn’t exist, make an HTTP request to retrieve it.  -->
    </when>
</choose>
```

Se o `userprofile` variável de contexto não existe, em seguida, API de gestão vai ter de efetuar um pedido HTTP para obtê-lo.

```xml
<send-request
  mode="new"
  response-variable-name="userprofileresponse"
  timeout="10"
  ignore-error="true">

  <!-- Build a URL that points to the profile for the current end-user -->
  <set-url>@(new Uri(new Uri("https://apimairlineapi.azurewebsites.net/UserProfile/"),
      (string)context.Variables["enduserid"]).AbsoluteUri)
  </set-url>
  <set-method>GET</set-method>
</send-request>
```

API Management utiliza o `enduserid` para construir o URL para o recurso de perfil de utilizador. Assim que a API Management tem de resposta, obtém o texto de corpo fora da resposta e armazena-a novamente para uma variável de contexto.

```xml
<set-variable
    name="userprofile"
    value="@(((IResponse)context.Variables["userprofileresponse"]).Body.As<string>())" />
```

Para evitar a gestão de API de efetuar este pedido HTTP novamente, quando o mesmo utilizador fizer outro pedido, pode especificar para armazenar o perfil de utilizador na cache.

```xml
<cache-store-value
    key="@("userprofile-" + context.Variables["enduserid"])"
    value="@((string)context.Variables["userprofile"])" duration="100000" />
```

Gestão de API armazena o valor na cache com a mesma chave exata que API Management originalmente foi efetuada uma tentativa para obtê-lo com. A duração API Management optar por armazenar o valor deve basear-se no como muitas vezes, as alterações de informações e tolerância a falhas como utilizadores são informações a desatualizados. 

É importante compreender que a obter a partir da cache ainda está a ser um fora do processo, o pedido de rede e, potencialmente, pode ainda adicionar dezenas de milissegundos para o pedido. As vantagens vêm ao determinar que as informações de perfil de utilizador demora mais que que devido à necessidade de base de dados consultas ou agregam informações de back-ends de vários.

É o último passo no processo de atualizar a resposta devolvida com as informações de perfil de utilizador.

```xml
<!-- Update response body with user profile-->
<find-and-replace
    from='"$userprofile$"'
    to="@((string)context.Variables["userprofile"])" />
```

Pode optar por incluir as aspas como parte do token de forma a que, mesmo quando a substituição não ocorre, a resposta ainda um JSON válido.  

Depois de combinar todos estes passos em conjunto, o resultado final é uma política que se assemelha o um seguinte.

```xml
<policies>
    <inbound>
        <!-- How you determine user identity is application dependent -->
        <set-variable
          name="enduserid"
          value="@(context.Request.Headers.GetValueOrDefault("Authorization","").Split(' ')[1].AsJwt()?.Subject)" />

        <!--Look for userprofile for this user in the cache -->
        <cache-lookup-value
          key="@("userprofile-" + context.Variables["enduserid"])"
          variable-name="userprofile" />

        <!-- If API Management doesn’t find it in the cache, make a request for it and store it -->
        <choose>
            <when condition="@(!context.Variables.ContainsKey("userprofile"))">
                <!-- Make HTTP request to get user profile -->
                <send-request
                  mode="new"
                  response-variable-name="userprofileresponse"
                  timeout="10"
                  ignore-error="true">

                   <!-- Build a URL that points to the profile for the current end-user -->
                    <set-url>@(new Uri(new Uri("https://apimairlineapi.azurewebsites.net/UserProfile/"),(string)context.Variables["enduserid"]).AbsoluteUri)</set-url>
                    <set-method>GET</set-method>
                </send-request>

                <!-- Store response body in context variable -->
                <set-variable
                  name="userprofile"
                  value="@(((IResponse)context.Variables["userprofileresponse"]).Body.As<string>())" />

                <!-- Store result in cache -->
                <cache-store-value
                  key="@("userprofile-" + context.Variables["enduserid"])"
                  value="@((string)context.Variables["userprofile"])"
                  duration="100000" />
            </when>
        </choose>
        <base />
    </inbound>
    <outbound>
        <!-- Update response body with user profile-->
        <find-and-replace
              from='"$userprofile$"'
              to="@((string)context.Variables["userprofile"])" />
        <base />
    </outbound>
</policies>
```

Esta abordagem de colocação em cache é principalmente utilizada em web sites em HTML é composto no lado do servidor para que pode ser composto como uma única página. Também pode ser útil em APIs onde os clientes não é possível efetuar a cache do lado do cliente HTTP ou é desejável não colocar em espera que responsabilidade no cliente.

Este tipo de fragmento a colocação em cache mesmo também pode ser feito os servidores de web de back-end através de um servidor de colocação em cache de Redis, no entanto, utilizando o serviço de API Management para efetuar este trabalho é útil quando os fragmentos em cache provenientes de back-ends diferentes que as respostas primárias.

## <a name="transparent-versioning"></a>Controlo de versões transparente
É prática comum para várias versões de implementação diferente de uma API para ser suportado num dado momento. Por exemplo, para suportar ambientes diferentes (desenvolvimento, teste, produção, etc.) ou para suportar as versões mais antigas da API para dar tempo para consumidores de API migrar para as versões mais recentes. 

Uma abordagem para lidar com isto, em vez de exigir os programadores de cliente alterar os URLs de `/v1/customers` para `/v2/customers` é para armazenar dados de perfil do consumidor qual é a versão da API atualmente pretenderem utilizar e chame o URL de back-end adequado. Para determinar o URL de back-end correto para chamar um cliente específico, é necessário consultar alguns dados de configuração. Ao colocar em cache estes dados de configuração, gestão de API pode minimizar a penalidade de desempenho de fazer esta pesquisa.

O primeiro passo consiste em determinar o identificador utilizado para configurar a versão pretendida. Neste exemplo, posso optar por associar a versão para a chave de subscrição do produto. 

```xml
<set-variable name="clientid" value="@(context.Subscription.Key)" />
```

Gestão de API, em seguida, efetua uma pesquisa de cache para ver se já obter a versão de cliente pretendidos.

```xml
<cache-lookup-value
key="@("clientversion-" + context.Variables["clientid"])"
variable-name="clientversion" />
```

Em seguida, gestão de API verifica se este não foi possível localizá-lo na cache.

```xml
<choose>
    <when condition="@(!context.Variables.ContainsKey("clientversion"))">
```

Se a gestão de API não encontrá-lo, a API Management obtém-lo.

```xml
<send-request
    mode="new"
    response-variable-name="clientconfiguresponse"
    timeout="10"
    ignore-error="true">
            <set-url>@(new Uri(new Uri(context.Api.ServiceUrl.ToString() + "api/ClientConfig/"),(string)context.Variables["clientid"]).AbsoluteUri)</set-url>
            <set-method>GET</set-method>
</send-request>
```

Extraia o texto de corpo de resposta da resposta.

```xml
<set-variable
      name="clientversion"
      value="@(((IResponse)context.Variables["clientconfiguresponse"]).Body.As<string>())" />
```

Armazene-o novamente na cache para utilização futura.

```xml
<cache-store-value
      key="@("clientversion-" + context.Variables["clientid"])"
      value="@((string)context.Variables["clientversion"])"
      duration="100000" />
```

E por fim, atualize o URL de back-end para selecionar a versão do serviço pretendido pelo cliente.

```xml
<set-backend-service
      base-url="@(context.Api.ServiceUrl.ToString() + "api/" + (string)context.Variables["clientversion"] + "/")" />
```

A política de conclua é o seguinte:

```xml
<inbound>
    <base />
    <set-variable name="clientid" value="@(context.Subscription.Key)" />
    <cache-lookup-value key="@("clientversion-" + context.Variables["clientid"])" variable-name="clientversion" />

    <!-- If API Management doesn’t find it in the cache, make a request for it and store it -->
    <choose>
        <when condition="@(!context.Variables.ContainsKey("clientversion"))">
            <send-request mode="new" response-variable-name="clientconfiguresponse" timeout="10" ignore-error="true">
                <set-url>@(new Uri(new Uri(context.Api.ServiceUrl.ToString() + "api/ClientConfig/"),(string)context.Variables["clientid"]).AbsoluteUri)</set-url>
                <set-method>GET</set-method>
            </send-request>
            <!-- Store response body in context variable -->
            <set-variable name="clientversion" value="@(((IResponse)context.Variables["clientconfiguresponse"]).Body.As<string>())" />
            <!-- Store result in cache -->
            <cache-store-value key="@("clientversion-" + context.Variables["clientid"])" value="@((string)context.Variables["clientversion"])" duration="100000" />
        </when>
    </choose>
    <set-backend-service base-url="@(context.Api.ServiceUrl.ToString() + "api/" + (string)context.Variables["clientversion"] + "/")" />
</inbound>
```

Ativar os consumidores de API controlar a versão back-end está a ser acedido por clientes, sem ter de atualizar e voltar a implementar clientes transparente é uma solução elegante que endereços muitos preocupações de controlo de versões de API.

## <a name="tenant-isolation"></a>Isolamento de inquilinos
Em implementações maiores, a multi-inquilinos algumas empresas criar grupos separados de inquilinos em diferentes implementações do hardware de back-end. Isto minimiza o número de clientes que são afetadas por um problema de hardware no back-end. Também permite novas versões de software ser implementado em fases. Idealmente, esta arquitetura de back-end deve ser transparente para os consumidores de API. Isto pode ser conseguido de forma semelhante para controlo de versões transparente porque se baseia na mesma técnica de manipular o URL de back-end com o estado de configuração por chave de API.  

Em vez de devolver uma versão da API para cada chave de subscrição preferencial, iria devolver um identificador que está relacionada com um inquilino para o grupo de hardware atribuído. Este identificador pode ser utilizado para construir o URL de back-end adequado.

## <a name="summary"></a>Resumo
A liberdade para utilizar a cache de gestão de API do Azure para armazenar qualquer tipo de dados permite eficiente acesso aos dados de configuração que podem afetar a forma de que um pedido de entrada é processado. Também pode ser utilizado para armazenar os fragmentos de dados que podem aumentar respostas, devolvidas por um API de back-end.
