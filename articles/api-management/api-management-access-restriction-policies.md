---
title: Políticas de restrição de acesso de gestão de API do Azure | Documentos da Microsoft
description: Saiba mais sobre as políticas de restrição de acesso disponíveis para utilização na gestão de API do Azure.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: 034febe3-465f-4840-9fc6-c448ef520b0f
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: apimpm
ms.openlocfilehash: d5f5b66dee88a993347b6c1672fd9526ece09dc4
ms.sourcegitcommit: f58fc4748053a50c34a56314cf99ec56f33fd616
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/04/2018
ms.locfileid: "48269523"
---
# <a name="api-management-access-restriction-policies"></a>Políticas de restrição de acesso de gestão de API
Este tópico fornece uma referência para as seguintes políticas de gestão de API. Para informações sobre como adicionar e configurar as políticas, consulte [políticas de gestão de API](http://go.microsoft.com/fwlink/?LinkID=398186).  
  
##  <a name="AccessRestrictionPolicies"></a> Políticas de restrição de acesso  
  
-   [Cabeçalho HTTP de verificação](api-management-access-restriction-policies.md#CheckHTTPHeader) -impõe a existência e/ou valor de um cabeçalho de HTTP.  
-   [Limitar taxa de chamadas por subscrição](api-management-access-restriction-policies.md#LimitCallRate) -picos de utilização da API impede ao limitar taxa de chamadas, numa base por subscrição.  
-   [Limitar taxa de chamadas por chave](#LimitCallRateByKey) -picos de utilização da API impede ao limitar taxa de chamadas, numa base por chave.  
-   [Restringir o chamador IPs](api-management-access-restriction-policies.md#RestrictCallerIPs) -chamadas de filtros (permite/nega) provenientes de endereços IP específicos e/ou intervalos de endereços.  
-   [Definir quota de utilização por subscrição](api-management-access-restriction-policies.md#SetUsageQuota) -permite-lhe impor um renovável ou tempo de vida volume e/ou de largura de banda de quota de chamadas, numa base por subscrição.  
-   [Definir quota de utilização por chave](#SetUsageQuotaByKey) -permite-lhe impor um renovável ou tempo de vida volume e/ou de largura de banda de quota de chamadas, numa base por chave.  
-   [Validar JWT](api-management-access-restriction-policies.md#ValidateJWT) -impõe a existência e a validade de um JWT extraído de um cabeçalho de HTTP especificado ou um parâmetro de consulta especificada.  
  
##  <a name="CheckHTTPHeader"></a> Verifique o cabeçalho de HTTP  
 Utilize o `check-header` política para impor que um pedido tem um cabeçalho HTTP especificado. Opcionalmente, pode verificar se o cabeçalho tem um valor específico ou a verificação de um intervalo de valores permitidos. Se a verificação falhar, a política termina o processamento da solicitação e retorna a HTTP e o código de erro mensagem de estado especificada pela política.  
  
### <a name="policy-statement"></a>Declaração de política  
  
```xml  
<check-header name="header name" failed-check-httpcode="code" failed-check-error-message="message" ignore-case="true">  
    <value>Value1</value>  
    <value>Value2</value>  
</check-header>  
```  
  
### <a name="example"></a>Exemplo  
  
```xml  
<check-header name="Authorization" failed-check-httpcode="401" failed-check-error-message="Not authorized" ignore-case="false">  
    <value>f6dc69a089844cf6b2019bae6d36fac8</value>  
</check-header>  
```  
  
### <a name="elements"></a>Elementos  
  
|Nome|Descrição|Necessário|  
|----------|-----------------|--------------|  
|cabeçalho de verificação|Elemento raiz.|Sim|  
|valor|Valor de cabeçalho HTTP permitido. Quando forem especificados vários elementos de valor, a verificação é considerada um sucesso se qualquer um dos valores uma correspondência.|Não|  
  
### <a name="attributes"></a>Atributos  
  
|Nome|Descrição|Necessário|Predefinição|  
|----------|-----------------|--------------|-------------|  
|failed-check-error-message|Mensagem de erro a devolver no corpo da resposta HTTP, se o cabeçalho não existe ou tem um valor inválido. Esta mensagem tem de ter carateres especiais escritos corretamente.|Sim|N/A|  
|failed-check-httpcode|Código de estado HTTP a devolver se o cabeçalho não existe ou tem um valor inválido.|Sim|N/A|  
|nome do cabeçalho|O nome do cabeçalho HTTP para verificar.|Sim|N/A|  
|caso ignorar|Pode ser definido como True ou False. Se definido como verdadeiro caso é ignorado quando o valor do cabeçalho é comparado com o conjunto de valores aceitáveis.|Sim|N/A|  
  
### <a name="usage"></a>Utilização  
 Esta política pode ser utilizada na política de seguinte [secções](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) e [âmbitos](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **As secções de política:** entrada, saída  
  
-   **Âmbitos de política:** global, produto, API, operação  
  
##  <a name="LimitCallRate"></a> Limitar taxa de chamadas por subscrição  
 O `rate-limit` política impede que os picos de utilização de API numa base por subscrição, ao limitar a taxa de chamadas para um número especificado por um período de tempo especificado. Quando esta política é acionada o autor da chamada recebe um `429 Too Many Requests` código de estado de resposta.  
  
> [!IMPORTANT]
>  Esta política pode ser utilizada apenas uma vez por documento da política.  
>   
>  [As expressões de diretriz](api-management-policy-expressions.md) não pode ser utilizado em qualquer um dos atributos de política para esta política.  
  
### <a name="policy-statement"></a>Declaração de política  
  
```xml  
<rate-limit calls="number" renewal-period="seconds">  
    <api name="API name" id="API id" calls="number" renewal-period="seconds" />  
        <operation name="operation name" id="operation id" calls="number" renewal-period="seconds" />  
    </api>  
</rate-limit>  
```  
  
### <a name="example"></a>Exemplo  
  
```xml  
<policies>  
    <inbound>  
        <base />  
        <rate-limit calls="20" renewal-period="90" />  
    </inbound>  
    <outbound>  
        <base />          
    </outbound>  
</policies>  
```  
  
### <a name="elements"></a>Elementos  
  
|Nome|Descrição|Necessário|  
|----------|-----------------|--------------|  
|limite de conjunto|Elemento raiz.|Sim|  
|api|Adicione um ou mais destes elementos para impor um limite de taxa de chamada com as APIs do produto. Produto e a API de limites são aplicados de forma independente de taxa de chamadas. API pode ser referenciada através de `name` ou `id`. Se os dois atributos são fornecidos, `id` será utilizado e `name` serão ignoradas.|Não|  
|operação|Adicione um ou mais destes elementos para impor um limite de taxa de chamada nas operações dentro de uma API. Produto, API e operação de limites são aplicados de forma independente de taxa de chamadas. Operação pode ser referenciada através de `name` ou `id`. Se os dois atributos são fornecidos, `id` será utilizado e `name` serão ignoradas.|Não|  
  
### <a name="attributes"></a>Atributos  
  
|Nome|Descrição|Necessário|Predefinição|  
|----------|-----------------|--------------|-------------|  
|nome|O nome da API para o qual pretende aplicar o limite de taxa.|Sim|N/A|  
|chamadas|O número máximo total de chamadas permitida durante o intervalo de tempo especificado no `renewal-period`.|Sim|N/A|  
|período de renovação|O período de tempo em segundos após o qual repõe a quota.|Sim|N/A|  
  
### <a name="usage"></a>Utilização  
 Esta política pode ser utilizada na política de seguinte [secções](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) e [âmbitos](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **As secções de política:** entrada  
  
-   **Âmbitos de política:** produto  
  
##  <a name="LimitCallRateByKey"></a> Limitar taxa de chamadas por chave  
 O `rate-limit-by-key` política impede que os picos de utilização de API numa base por chave ao limitar a taxa de chamadas para um número especificado por um período de tempo especificado. A chave pode ter um valor de cadeia de caracteres arbitrária e é, geralmente, fornecida com uma expressão de política. Pode ser adicionada a condição de incremento opcional para especificar quais os pedidos devem ser contados para o limite. Quando esta política é acionada o autor da chamada recebe um `429 Too Many Requests` código de estado de resposta.  
  
 Para obter mais informações e exemplos desta política, consulte [limitação API Management do Azure de pedidos avançada](https://azure.microsoft.com/documentation/articles/api-management-sample-flexible-throttling/).  
  
> [!IMPORTANT]
>  Esta política pode ser utilizada apenas uma vez por documento da política.  
  
### <a name="policy-statement"></a>Declaração de política  
  
```xml  
<rate-limit-by-key calls="number"  
                   renewal-period="seconds"   
                   increment-condition="condition"   
                   counter-key="key value" />  
  
```  
  
### <a name="example"></a>Exemplo  
 No exemplo a seguir, o limite de taxa é codificado o endereço IP do chamador.  
  
```xml  
<policies>  
    <inbound>  
        <base />  
        <rate-limit-by-key  calls="10"  
              renewal-period="60"  
              increment-condition="@(context.Response.StatusCode == 200)"  
              counter-key="@(context.Request.IpAddress)"/>  
    </inbound>  
    <outbound>  
        <base />          
    </outbound>  
</policies>  
```  
  
### <a name="elements"></a>Elementos  
  
|Nome|Descrição|Necessário|  
|----------|-----------------|--------------|  
|limite de conjunto|Elemento raiz.|Sim|  
  
### <a name="attributes"></a>Atributos  
  
|Nome|Descrição|Necessário|Predefinição|  
|----------|-----------------|--------------|-------------|  
|chamadas|O número máximo total de chamadas permitida durante o intervalo de tempo especificado no `renewal-period`.|Sim|N/A|  
|chave contra-argumentação|A chave a utilizar para a política de limite de taxa.|Sim|N/A|  
|condição de incremento|Expressão booleana, especificando se a solicitação deve ser contada em direção a quota (`true`).|Não|N/A|  
|período de renovação|O período de tempo em segundos após o qual repõe a quota.|Sim|N/A|  
  
### <a name="usage"></a>Utilização  
 Esta política pode ser utilizada na política de seguinte [secções](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) e [âmbitos](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **As secções de política:** entrada  
  
-   **Âmbitos de política:** global, produto, API, operação  
  
##  <a name="RestrictCallerIPs"></a> Restringir o chamador IPs  
 O `ip-filter` (permite/nega) chamadas a partir de endereços IP específicos e/ou intervalos de endereços de filtros de política.  
  
### <a name="policy-statement"></a>Declaração de política  
  
```xml  
<ip-filter action="allow | forbid">  
    <address>address</address>  
    <address-range from="address" to="address" />  
</ip-filter>  
```  
  
### <a name="example"></a>Exemplo  
  
```xml  
<ip-filter action="allow | forbid">  
    <address>address</address>  
    <address-range from="address" to="address" />  
</ip-filter>  
```  
  
### <a name="elements"></a>Elementos  
  
|Nome|Descrição|Necessário|  
|----------|-----------------|--------------|  
|filtro de IP|Elemento raiz.|Sim|  
|Endereço|Especifica um endereço IP único no qual pretende filtrar.|Pelo menos um `address` ou `address-range` elemento é necessário.|  
|intervalo de endereços de = "endereço" para "endereço" de =|Especifica um intervalo de endereços IP em que pretende filtrar.|Pelo menos um `address` ou `address-range` elemento é necessário.|  
  
### <a name="attributes"></a>Atributos  
  
|Nome|Descrição|Necessário|Predefinição|  
|----------|-----------------|--------------|-------------|  
|intervalo de endereços de = "endereço" para "endereço" de =|Um intervalo de endereços IP para permitir ou negar o acesso de.|É necessário quando o `address-range` elemento é usado.|N/A|  
|ação de filtro de IP = "permitir &#124; proibir"|Especifica se devem ser permitidas chamadas ou não para os intervalos e endereços IP especificados.|Sim|N/A|  
  
### <a name="usage"></a>Utilização  
 Esta política pode ser utilizada na política de seguinte [secções](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) e [âmbitos](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **As secções de política:** entrada  
-   **Âmbitos de política:** global, produto, API, operação  
  
##  <a name="SetUsageQuota"></a> Definir quota de utilização por subscrição  
 O `quota` política impõe uma renovável ou tempo de vida volume e/ou de largura de banda de quota de chamadas, numa base por subscrição.  
  
> [!IMPORTANT]
>  Esta política pode ser utilizada apenas uma vez por documento da política.  
>   
>  [As expressões de diretriz](api-management-policy-expressions.md) não pode ser utilizado em qualquer um dos atributos de política para esta política.  
  
### <a name="policy-statement"></a>Declaração de política  
  
```xml  
<quota calls="number" bandwidth="kilobytes" renewal-period="seconds">  
    <api name="API name" id="API id" calls="number" renewal-period="seconds" />  
        <operation name="operation name" id="operation id" calls="number" renewal-period="seconds" />  
    </api>  
</quota>  
```  
  
### <a name="example"></a>Exemplo  
  
```xml  
<policies>  
    <inbound>  
        <base />  
        <quota calls="10000" bandwidth="40000" renewal-period="3600" />  
    </inbound>  
    <outbound>  
        <base />  
    </outbound>  
</policies>  
```  
  
### <a name="elements"></a>Elementos  
  
|Nome|Descrição|Necessário|  
|----------|-----------------|--------------|  
|quota|Elemento raiz.|Sim|  
|api|Adicione um ou mais destes elementos para impor a quota de chamadas com as APIs do produto. Produtos e quotas de chamada de API são aplicadas de forma independente. API pode ser referenciada através de `name` ou `id`. Se os dois atributos são fornecidos, `id` será utilizado e `name` serão ignoradas.|Não|  
|operação|Adicione um ou mais destes elementos para impor a quota de chamada nas operações dentro de uma API. Quotas de chamada de produto, API e operação são aplicadas de forma independente. Operação pode ser referenciada através de `name` ou `id`. Se os dois atributos são fornecidos, `id` será utilizado e `name` serão ignoradas.|Não|  
  
### <a name="attributes"></a>Atributos  
  
|Nome|Descrição|Necessário|Predefinição|  
|----------|-----------------|--------------|-------------|  
|nome|O nome da API ou operação para os quais se aplica a quota.|Sim|N/A|  
|Largura de banda|O número total máximo de quilobytes permitida durante o intervalo de tempo especificado no `renewal-period`.|Qualquer um dos `calls`, `bandwidth`, ou ambos em conjunto tem de ser especificadas.|N/A|  
|chamadas|O número máximo total de chamadas permitida durante o intervalo de tempo especificado no `renewal-period`.|Qualquer um dos `calls`, `bandwidth`, ou ambos em conjunto tem de ser especificadas.|N/A|  
|período de renovação|O período de tempo em segundos após o qual repõe a quota.|Sim|N/A|  
  
### <a name="usage"></a>Utilização  
 Esta política pode ser utilizada na política de seguinte [secções](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) e [âmbitos](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **As secções de política:** entrada  
-   **Âmbitos de política:** produto  
  
##  <a name="SetUsageQuotaByKey"></a> Definir quota de utilização por chave  
 O `quota-by-key` política impõe uma renovável ou tempo de vida volume e/ou de largura de banda de quota de chamadas, numa base por chave. A chave pode ter um valor de cadeia de caracteres arbitrária e é, geralmente, fornecida com uma expressão de política. Pode ser adicionada a condição de incremento opcional para especificar quais os pedidos devem ser contados em direção a quota. Quando esta política é acionada o autor da chamada recebe um `403 Forbidden` código de estado de resposta.
  
 Para obter mais informações e exemplos desta política, consulte [limitação API Management do Azure de pedidos avançada](https://azure.microsoft.com/documentation/articles/api-management-sample-flexible-throttling/).  
  
> [!IMPORTANT]
>  Esta política pode ser utilizada apenas uma vez por documento da política.  
>   
>  [As expressões de diretriz](api-management-policy-expressions.md) não pode ser utilizado em qualquer um dos atributos de política para esta política.  
  
### <a name="policy-statement"></a>Declaração de política  
  
```xml  
<quota-by-key calls="number"   
              bandwidth="kilobytes"   
              renewal-period="seconds"  
              increment-condition="condition"   
              counter-key="key value" />  
  
```  
  
### <a name="example"></a>Exemplo  
 No exemplo seguinte, a quota é codificada o endereço IP do chamador.  
  
```xml  
<policies>  
    <inbound>  
        <base />  
        <quota-by-key calls="10000" bandwidth="40000" renewal-period="3600"  
                      increment-condition="@(context.Response.StatusCode >= 200 && context.Response.StatusCode < 400)"  
                      counter-key="@(context.Request.IpAddress)" />  
    </inbound>  
    <outbound>  
        <base />  
    </outbound>  
</policies>  
```  
  
### <a name="elements"></a>Elementos  
  
|Nome|Descrição|Necessário|  
|----------|-----------------|--------------|  
|quota|Elemento raiz.|Sim|  
  
### <a name="attributes"></a>Atributos  
  
|Nome|Descrição|Necessário|Predefinição|  
|----------|-----------------|--------------|-------------|  
|Largura de banda|O número total máximo de quilobytes permitida durante o intervalo de tempo especificado no `renewal-period`.|Qualquer um dos `calls`, `bandwidth`, ou ambos em conjunto tem de ser especificadas.|N/A|  
|chamadas|O número máximo total de chamadas permitida durante o intervalo de tempo especificado no `renewal-period`.|Qualquer um dos `calls`, `bandwidth`, ou ambos em conjunto tem de ser especificadas.|N/A|  
|chave contra-argumentação|A chave a utilizar para a política de quota.|Sim|N/A|  
|condição de incremento|Expressão booleana, especificando se a solicitação deve ser contada em direção a quota (`true`)|Não|N/A|  
|período de renovação|O período de tempo em segundos após o qual repõe a quota.|Sim|N/A|  
  
### <a name="usage"></a>Utilização  
 Esta política pode ser utilizada na política de seguinte [secções](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) e [âmbitos](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **As secções de política:** entrada  
-   **Âmbitos de política:** global, produto, API, operação  
  
##  <a name="ValidateJWT"></a> Validar JWT  
 O `validate-jwt` política impõe a existência e a validade de um JWT extraídos de qualquer um de uma determinada cabeçalho de HTTP ou um parâmetro de consulta especificada.  
  
> [!IMPORTANT]
>  O `validate-jwt` política requer que o `exp` registada afirmação está incluída no JWT token, a menos que `require-expiration-time` atributo é especificado e definido como `false`.  
> O `validate-jwt` política suporta HS256 e RS256 algoritmos de assinatura. Para HS256 a chave deve ser fornecida inline a política na forma codificada em base64. Para RS256 a chave tem de ser fornecer por meio de um ponto de extremidade de configuração de ID aberto.  
  
### <a name="policy-statement"></a>Declaração de política  
  
```xml  
<validate-jwt   
    header-name="name of http header containing the token (use query-parameter-name attribute if the token is passed in the URL)"   
    failed-validation-httpcode="http status code to return on failure"   
    failed-validation-error-message="error message to return on failure"   
    require-expiration-time="true|false"
    require-scheme="scheme"
    require-signed-tokens="true|false"   
    clock-skew="allowed clock skew in seconds">  
  <issuer-signing-keys>  
    <key>base64 encoded signing key</key>  
    <!-- if there are multiple keys, then add additional key elements -->  
  </issuer-signing-keys>  
  <audiences>  
    <audience>audience string</audience>  
    <!-- if there are multiple possible audiences, then add additional audience elements -->  
  </audiences>  
  <issuers>  
    <issuer>issuer string</issuer>  
    <!-- if there are multiple possible issuers, then add additional issuer elements -->  
  </issuers>  
  <required-claims>  
    <claim name="name of the claim as it appears in the token" match="all|any" separator="separator character in a multi-valued claim">
      <value>claim value as it is expected to appear in the token</value>  
      <!-- if there is more than one allowed values, then add additional value elements -->  
    </claim>  
    <!-- if there are multiple possible allowed values, then add additional value elements -->  
  </required-claims>  
  <openid-config url="full URL of the configuration endpoint, e.g. https://login.constoso.com/openid-configuration" />  
  <zumo-master-key id="key identifier">key value</zumo-master-key>  
</validate-jwt>  
  
```  
  
### <a name="examples"></a>Exemplos  
  
#### <a name="azure-mobile-services-token-validation"></a>Validação de token de serviços móveis do Azure  
  
```xml  
<validate-jwt header-name="x-zumo-auth" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Supplied access token is invalid.">  
    <issuers>  
        <issuer>urn:microsoft:windows-azure:zumo</issuer>  
    </issuers>  
    <audiences>  
        <audience>Facebook</audience>  
    </audiences>  
    <issuer-signing-keys>  
        <zumo-master-key id="0">insert key here</zumo-master-key>  
    </issuer-signing-keys>  
</validate-jwt>  
```  
  
#### <a name="azure-active-directory-token-validation"></a>Validação do token do Azure Active Directory  
  
```xml  
<validate-jwt header-name="Authorization" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Access token is missing or invalid.">  
    <openid-config url="https://login.microsoftonline.com/contoso.onmicrosoft.com/.well-known/openid-configuration" />  
    <audiences>
        <audience>25eef6e4-c905-4a07-8eb4-0d08d5df8b3f</audience>
    </audiences>
    <required-claims>  
        <claim name="id" match="all">  
            <value>insert claim here</value>  
        </claim>  
    </required-claims>  
</validate-jwt>  
```  

  
#### <a name="azure-active-directory-b2c-token-validation"></a>Validação do token do Azure Active Directory B2C  
  
```xml  
<validate-jwt header-name="Authorization" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Access token is missing or invalid.">  
    <openid-config url="https://login.microsoftonline.com/tfp/contoso.onmicrosoft.com/b2c_1_signin/v2.0/.well-known/openid-configuration" />
    <audiences>
        <audience>d313c4e4-de5f-4197-9470-e509a2f0b806</audience>
    </audiences>
    <required-claims>  
        <claim name="id" match="all">  
            <value>insert claim here</value>  
        </claim>  
    </required-claims>  
</validate-jwt>  
```  
  
#### <a name="authorize-access-to-operations-based-on-token-claims"></a>Autorizar o acesso às operações com base em declarações do token  
 Este exemplo mostra como utilizar o [validar JWT](api-management-access-restriction-policies.md#ValidateJWT) política para autorizar previamente o acesso às operações com base nas afirmações de token. Para uma demonstração de configurar e utilizar esta política, consulte [Cloud Cover episódio 177: mais funcionalidades de gestão de API com Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) e avanço rápido para 13:50. Voltando para 15:00 para ver as políticas configuradas no editor de políticas e, em seguida, a 18:50 para uma demonstração de chamar uma operação a partir do portal do desenvolvedor com e sem o token de autorização necessário.  
  
```xml  
<!-- Copy the following snippet into the inbound section at the api (or higher) level to pre-authorize access to operations based on token claims -->  
<set-variable name="signingKey" value="insert signing key here" />  
<choose>  
  <when condition="@(context.Request.Method.Equals("patch",StringComparison.OrdinalIgnoreCase))">  
    <validate-jwt header-name="Authorization">  
      <issuer-signing-keys>  
        <key>@((string)context.Variables["signingKey"])</key>  
      </issuer-signing-keys>  
      <required-claims>  
        <claim name="edit">  
          <value>true</value>  
        </claim>  
      </required-claims>  
    </validate-jwt>  
  </when>  
  <when condition="@(new [] {"post", "put"}.Contains(context.Request.Method,StringComparer.OrdinalIgnoreCase))">  
    <validate-jwt header-name="Authorization">  
      <issuer-signing-keys>  
        <key>@((string)context.Variables["signingKey"])</key>  
      </issuer-signing-keys>  
      <required-claims>  
        <claim name="create">  
          <value>true</value>  
        </claim>  
      </required-claims>  
    </validate-jwt>  
  </when>  
  <otherwise>  
    <validate-jwt header-name="Authorization">  
      <issuer-signing-keys>  
        <key>@((string)context.Variables["signingKey"])</key>  
      </issuer-signing-keys>  
    </validate-jwt>  
  </otherwise>  
</choose>  
```  
  
### <a name="elements"></a>Elementos  
  
|Elemento|Descrição|Necessário|  
|-------------|-----------------|--------------|  
|jwt validar|Elemento raiz.|Sim|  
|públicos-alvo|Contém uma lista das declarações de público-alvo aceitável que pode estar presente no token. Se existirem vários valores de audiência, então cada valor é experimentada até ser todos ficam esgotados (caso em que validação falhar) ou até uma ter êxito. Tem de ser especificado pelo menos um público.|Não|  
|chaves emissor de assinatura|Uma lista das chaves de segurança com codificação Base64 utilizado para validar tokens assinados. Se existirem várias chaves de segurança, então cada chave é experimentada até ser todos ficam esgotados (caso em que validação falhar) ou até uma ter êxito (útil para rollover de token). Principais elementos tem opcional `id` atributo utilizado para correspondência `kid` de afirmação.|Não|  
|emissores|Uma lista de principais aceitáveis que emitiu o token. Se existirem vários valores de emissor, então cada valor é experimentada até ser todos ficam esgotados (caso em que validação falhar) ou até uma ter êxito.|Não|  
|configuração de openid|O elemento usado para especificar um em conformidade com ID aberto configuração ponto final do qual a assinatura de chaves e o emissor pode ser obtido.|Não|  
|declarações necessárias|Contém uma lista das declarações deve estar presente no token para o mesmo ser considerado válido. Quando o `match` atributo está definido como `all` cada valor de afirmação na política tem de estar presente no token para a validação com êxito. Quando o `match` atributo está definido como `any` , pelo menos, uma afirmação tem de estar presente no token para a validação com êxito.|Não|  
|zumo-master-key|Chave mestra para tokens emitidos pelo Mobile Services do Azure|Não|  
  
### <a name="attributes"></a>Atributos  
  
|Nome|Descrição|Necessário|Predefinição|  
|----------|-----------------|--------------|-------------|  
|defasagem|Período de tempo. Utilize para especificar a diferença de tempo esperado máximo entre os relógios de sistema do emissor de tokens e a instância de gestão de API.|Não|0 segundos|  
|failed-validation-error-message|Mensagem de erro a devolver no corpo da resposta HTTP, se o JWT não passar na validação. Esta mensagem tem de ter carateres especiais escritos corretamente.|Não|Mensagem de erro padrão depende do problema de validação, por exemplo "JWT não está presente."|  
|Falha-validação-httpcode:{0|Código de estado HTTP a devolver se a JWT não passa na validação.|Não|401|  
|nome do cabeçalho|O nome do cabeçalho HTTP que contém o token.|Seja `header-name` ou `query-parameter-name` tem de ser especificado; mas não ambos.|N/A|  
|ID|O `id` atributo a `key` elemento permite-lhe especificar a cadeia de caracteres que serão matched relativamente a `kid` afirmações no token (caso exista) obter informações sobre a chave adequada a utilizar para a validação de assinatura.|Não|N/A|  
|Correspondência|O `match` atributo o `claim` elemento Especifica se cada valor de afirmação na política tem de estar presente no token para a validação com êxito. Os valores possíveis são:<br /><br /> -                          `all` -todos os valores de afirmação na política tem de estar presente no token para a validação com êxito.<br /><br /> -                          `any` -pelo menos uma declaração valor tem de estar presente no token para a validação com êxito.|Não|all|  
|query-paremeter-name|O nome do parâmetro de consulta que contém o token.|Seja `header-name` ou `query-paremeter-name` tem de ser especificado; mas não ambos.|N/A|  
|exigir--hora de expiração|Valor booleano. Especifica se uma afirmação de expiração é necessária no token.|Não|true|
|esquema de exigir|O nome do token de esquema, por exemplo "Bearer". Quando este atributo estiver definido, a política irá garantir que o esquema especificado está presente no valor de cabeçalho de autorização.|Não|N/A|
|exigir-assinados-tokens|Valor booleano. Especifica se um token é necessário ter sessão iniciada.|Não|true|  
|separador|cadeia de caracteres. Especifica um separador (por exemplo, ",") a ser utilizado para extrair um conjunto de valores de uma afirmação com múltiplos valor.|Não|N/A| 
|url|Abra o URL de ponto final de configuração de ID de onde os metadados de configuração de Open ID podem ser obtidos. A resposta deve ser de acordo com especificações conforme definido no URL:`https://openid.net/specs/openid-connect-discovery-1_0.html#ProviderMetadata`.  Para o Azure Active Directory, utilize o seguinte URL: `https://login.microsoftonline.com/{tenant-name}/.well-known/openid-configuration` substituindo o nome do inquilino do diretório, por exemplo, `contoso.onmicrosoft.com`.|Sim|N/A|  
  
### <a name="usage"></a>Utilização  
 Esta política pode ser utilizada na política de seguinte [secções](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) e [âmbitos](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **As secções de política:** entrada  
-   **Âmbitos de política:** global, produto, API, operação  
  
## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações para trabalhar com políticas, consulte:

+ [Políticas de gestão de API](api-management-howto-policies.md)
+ [Transforme as APIs](transform-api.md)
+ [Referência de política](api-management-policy-reference.md) para uma lista completa das declarações de política e suas configurações
+ [Exemplos de política](policy-samples.md)   
