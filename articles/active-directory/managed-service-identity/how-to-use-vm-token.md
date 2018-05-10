---
title: Como utilizar uma identidade de serviço gerida do Azure VM para adquirir um token de acesso
description: Passo a passo instruções e exemplos de utilização de um MSI de VM do Azure para adquirir um OAuth token de acesso ao.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/01/2017
ms.author: daveba
ms.openlocfilehash: 9300c3a45f57da7e55eed1dbdf8fd6e94b094c31
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
---
# <a name="how-to-use-an-azure-vm-managed-service-identity-msi-for-token-acquisition"></a>Como utilizar um Azure VM geridos serviço de identidade (MSI) para a aquisição do token 

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]  

Identidade de serviço geridas fornece serviços do Azure com uma identidade gerido automaticamente no Azure Active Directory. Pode utilizar esta identidade para autenticar a qualquer serviço que suporta a autenticação do Azure AD, sem ter de credenciais no seu código. 

Este artigo fornece vários exemplos de código e o script para a aquisição do token, bem como orientações tópicos importantes, como o tratamento de expiração do token e erros de HTTP. 

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

Se planeia utilizar os Azure PowerShell exemplos neste artigo, não se esqueça de instalar a versão mais recente do [Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM).


> [!IMPORTANT]
> - Todos os código/script de exemplo neste artigo assume que o cliente está em execução numa máquina Virtual com uma identidade de serviço geridas. Utilize a funcionalidade VM "Ligar" no portal do Azure, para ligar remotamente à VM. Para obter mais informações sobre como ativar MSI numa VM, consulte [configurar uma VM geridos serviço de identidade (MSI) no portal do Azure](qs-configure-portal-windows-vm.md), ou um dos artigos variantes (utilizando o PowerShell, CLI, um modelo ou um SDK do Azure). 

> [!IMPORTANT]
> - O limite de segurança de uma identidade de serviço geridos, é o recurso que está a ser utilizado. Todos os código/scripts em execução numa máquina Virtual pode pedir e obter tokens para qualquer identidade de serviço geridas disponíveis no mesmo. 

## <a name="overview"></a>Descrição geral

Uma aplicação cliente pode pedir uma identidade de serviço geridas [token de acesso só de aplicação](../develop/active-directory-dev-glossary.md#access-token) para aceder a um recurso especificado. O token é [com base no principal de serviço MSI](overview.md#how-does-it-work). Como tal, não é necessário para o cliente para se registar para obter um token de acesso na sua própria principal de serviço. O token é adequado para utilização como um token de portador no [que exigem que as credenciais de cliente de chamadas de serviço a serviço](../develop/active-directory-protocols-oauth-service-to-service.md).

|  |  |
| -------------- | -------------------- |
| [Obter um token através de HTTP](#get-a-token-using-http) | Detalhes de protocolo para o ponto de final de token de MSI |
| [Obter um token com a c#](#get-a-token-using-c) | Exemplo de como utilizar o ponto final de REST do MSI de um cliente do c# |
| [Obter um token aceda a utilizar](#get-a-token-using-go) | Exemplo de como utilizar o ponto final de REST do MSI de um cliente aceda |
| [Obter um token com o Azure PowerShell](#get-a-token-using-azure-powershell) | Exemplo de como utilizar o ponto final de REST do MSI de um cliente de PowerShell |
| [Obter um token utilizando CURL](#get-a-token-using-curl) | Exemplo de como utilizar o ponto final de REST do MSI de um cliente de Bash/CURL |
| [Lidar com colocação em cache de token](#handling-token-caching) | Orientações para processar os tokens de acesso expirado |
| [Processamento de erros](#error-handling) | Orientações para processamento de erros de HTTP devolvidos do ponto final token MSI |
| [IDs de recurso para serviços do Azure](#resource-ids-for-azure-services) | Onde obter IDs de recurso para serviços do Azure suportados |

## <a name="get-a-token-using-http"></a>Obter um token através de HTTP 

A interface fundamental para adquirir um token de acesso é baseada em REST, tornando-o acessível para qualquer aplicação de cliente em execução na VM que pode efetuar chamadas de REST de HTTP. Isto é semelhante para o modelo de programação do Azure AD, exceto o cliente utiliza um ponto final na máquina virtual (vs um Azure ponto final do AD).

Exemplo de pedido utilizando o ponto final de serviço de metadados de instância do Azure (IMDS) *(recomendado)*:

```
GET 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https://management.azure.com/' HTTP/1.1 Metadata: true
```

| Elemento | Descrição |
| ------- | ----------- |
| `GET` | O verbo HTTP, com a indicação de que pretende obter dados do ponto final. Neste caso, um OAuth token de acesso ao. | 
| `http://169.254.169.254/metadata/identity/oauth2/token` | O ponto final MSI para o serviço de metadados de instância. |
| `api-version`  | Um cadeia parâmetro de consulta, que indica a versão da API para o ponto final IMDS. Utilize a versão da API `2018-02-01` ou superior. |
| `resource` | Um cadeia parâmetro de consulta, que indica que o URI de ID de aplicação do recurso de destino. É também apresentado no `aud` afirmação (público-alvo) do token emitido. Neste exemplo solicita um token para aceder ao Gestor de recursos do Azure, que tem um URI de ID de aplicação de https://management.azure.com/. |
| `Metadata` | Um HTTP pedido campo do cabeçalho, necessário para MSI como uma mitigação de ataques de falsificação de pedidos de lado do servidor (SSRF). Este valor tem de ser definido como "true", em todas as letras maiúsculas e minúsculas.

Exemplo de pedido utilizando o Endpoint de extensão de VM de identidade de serviço geridas (MSI) *(para ser preterida)*:

```
GET http://localhost:50342/oauth2/token?resource=https%3A%2F%2Fmanagement.azure.com%2F HTTP/1.1
Metadata: true
```

| Elemento | Descrição |
| ------- | ----------- |
| `GET` | O verbo HTTP, com a indicação de que pretende obter dados do ponto final. Neste caso, um OAuth token de acesso ao. | 
| `http://localhost:50342/oauth2/token` | O MSI ponto final, onde 50342 é a porta predefinida e é configurável. |
| `resource` | Um cadeia parâmetro de consulta, que indica que o URI de ID de aplicação do recurso de destino. É também apresentado no `aud` afirmação (público-alvo) do token emitido. Neste exemplo solicita um token para aceder ao Gestor de recursos do Azure, que tem um URI de ID de aplicação de https://management.azure.com/. |
| `Metadata` | Um HTTP pedido campo do cabeçalho, necessário para MSI como uma mitigação de ataques de falsificação de pedidos de lado do servidor (SSRF). Este valor tem de ser definido como "true", em todas as letras maiúsculas e minúsculas.


Resposta de amostra:

```
HTTP/1.1 200 OK
Content-Type: application/json
{
  "access_token": "eyJ0eXAi...",
  "refresh_token": "",
  "expires_in": "3599",
  "expires_on": "1506484173",
  "not_before": "1506480273",
  "resource": "https://management.azure.com/",
  "token_type": "Bearer"
}
```

| Elemento | Descrição |
| ------- | ----------- |
| `access_token` | O token de pedido de acesso. Quando chamar uma API de REST protegida, o token está incorporado a `Authorization` campo de cabeçalho de pedido como um token de "portador", permitindo que a API autenticar o autor da chamada. | 
| `refresh_token` | Não é utilizado pelo MSI. |
| `expires_in` | O número de segundos que o token de acesso continua a ser válida, antes de expirar, a hora de emissão. Tempo de emissão pode ser encontrado no token de `iat` de afirmação. |
| `expires_on` | O período de tempo quando o token de acesso expira. A data é representada como o número de segundos de "1970-01-01T0:0:0Z UTC" (corresponde do token `exp` afirmação). |
| `not_before` | O período de tempo quando o token de acesso entra em vigor e pode ser aceites. A data é representada como o número de segundos de "1970-01-01T0:0:0Z UTC" (corresponde do token `nbf` afirmação). |
| `resource` | O recurso foi solicitado o token de acesso para que corresponde à `resource` parâmetro de cadeia do pedido de consulta. |
| `token_type` | O tipo de token, o que é um token de acesso de "Portador", o que significa que o recurso pode conceder acesso ao portador do token. |

## <a name="get-a-token-using-c"></a>Obter um token com a c#

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Net;
using System.Web.Script.Serialization; 

// Build request to acquire MSI token
HttpWebRequest request = (HttpWebRequest)WebRequest.Create(http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https://management.azure.com/");
request.Headers["Metadata"] = "true";
request.Method = "GET";

try
{
    // Call /token endpoint
    HttpWebResponse response = (HttpWebResponse)request.GetResponse();

    // Pipe response Stream to a StreamReader, and extract access token
    StreamReader streamResponse = new StreamReader(response.GetResponseStream()); 
    string stringResponse = streamResponse.ReadToEnd();
    JavaScriptSerializer j = new JavaScriptSerializer();
    Dictionary<string, string> list = (Dictionary<string, string>) j.Deserialize(stringResponse, typeof(Dictionary<string, string>));
    string accessToken = list["access_token"];
}
catch (Exception e)
{
    string errorText = String.Format("{0} \n\n{1}", e.Message, e.InnerException != null ? e.InnerException.Message : "Acquire token failed");
}

```

## <a name="get-a-token-using-go"></a>Obter um token aceda a utilizar

```
package main

import (
  "fmt"
  "io/ioutil"
  "net/http"
  "net/url"
  "encoding/json"
)

type responseJson struct {
  AccessToken string `json:"access_token"`
  RefreshToken string `json:"refresh_token"`
  ExpiresIn string `json:"expires_in"`
  ExpiresOn string `json:"expires_on"`
  NotBefore string `json:"not_before"`
  Resource string `json:"resource"`
  TokenType string `json:"token_type"`
}

func main() {
    
    // Create HTTP request for MSI token to access Azure Resource Manager
    var msi_endpoint *url.URL
    msi_endpoint, err := url.Parse("http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01")
    if err != nil {
      fmt.Println("Error creating URL: ", err)
      return 
    }
    msi_parameters := url.Values{}
    msi_parameters.Add("resource", "https://management.azure.com/")
    msi_endpoint.RawQuery = msi_parameters.Encode()
    req, err := http.NewRequest("GET", msi_endpoint.String(), nil)
    if err != nil {
      fmt.Println("Error creating HTTP request: ", err)
      return 
    }
    req.Header.Add("Metadata", "true")

    // Call MSI /token endpoint
    client := &http.Client{}
    resp, err := client.Do(req) 
    if err != nil{
      fmt.Println("Error calling token endpoint: ", err)
      return
    }

    // Pull out response body
    responseBytes,err := ioutil.ReadAll(resp.Body)
    defer resp.Body.Close()
    if err != nil {
      fmt.Println("Error reading response body : ", err)
      return
    }

    // Unmarshall response body into struct
    var r responseJson
    err = json.Unmarshal(responseBytes, &r)
    if err != nil {
      fmt.Println("Error unmarshalling the response:", err)
      return
    }

    // Print HTTP response and marshalled response body elements to console
    fmt.Println("Response status:", resp.Status)
    fmt.Println("access_token: ", r.AccessToken)
    fmt.Println("refresh_token: ", r.RefreshToken)
    fmt.Println("expires_in: ", r.ExpiresIn)
    fmt.Println("expires_on: ", r.ExpiresOn)
    fmt.Println("not_before: ", r.NotBefore)
    fmt.Println("resource: ", r.Resource)
    fmt.Println("token_type: ", r.TokenType)
}
```

## <a name="get-a-token-using-azure-powershell"></a>Obter um token com o Azure PowerShell

O exemplo seguinte demonstra como utilizar o ponto final de REST do MSI entre um cliente do PowerShell:

1. Adquirir um token de acesso.
2. Utilize o token de acesso para chamar uma API de REST do Gestor de recursos do Azure e obter informações sobre a VM. Não se esqueça de substituir o seu ID de subscrição, o nome do grupo de recursos e o nome da máquina virtual para `<SUBSCRIPTION-ID>`, `<RESOURCE-GROUP>`, e `<VM-NAME>`, respetivamente.

```azurepowershell
Invoke-WebRequest -Uri 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com%2F' -Headers @{Metadata="true"}
```

Exemplo de como analisar o token de acesso da resposta:
```azurepowershell
# Get an access token for the MSI
$response = Invoke-WebRequest -Uri http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com%2F `
                              -Headers @{Metadata="true"}
$content =$response.Content | ConvertFrom-Json
$access_token = $content.access_token
echo "The MSI access token is $access_token"

# Use the access token to get resource information for the VM
$vmInfoRest = (Invoke-WebRequest -Uri https://management.azure.com/subscriptions/<SUBSCRIPTION-ID>/resourceGroups/<RESOURCE-GROUP>/providers/Microsoft.Compute/virtualMachines/<VM-NAME>?api-version=2017-12-01 -Method GET -ContentType "application/json" -Headers @{ Authorization ="Bearer $access_token"}).content
echo "JSON returned from call to get VM info:"
echo $vmInfoRest

```

## <a name="get-a-token-using-curl"></a>Obter um token utilizando CURL

```bash
curl 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com%2F' -H Metadata:true -s
```


Exemplo de como analisar o token de acesso da resposta:

```bash
response=$(curl 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com%2F' -H Metadata:true -s)
access_token=$(echo $response | python -c 'import sys, json; print (json.load(sys.stdin)["access_token"])')
echo The MSI access token is $access_token
```

## <a name="token-caching"></a>A colocação em cache token

Enquanto o subsistema de identidade de serviço geridas (MSI) que está a ser utilizado (extensão da VM IMDS/MSI) em cache tokens, também é recomendável para implementar a colocação em cache token no seu código. Como resultado, deve preparar para cenários em que o recurso indica que o token expirou. 

Na transmissão chamadas para o Azure AD resultam apenas quando:
- Falha de acerto na cache ocorre devido a nenhum token na cache de subsistema de MSI
- o token em cache expirou

## <a name="error-handling"></a>Processamento de erros

O ponto final de identidade de serviço geridas sinalizar erros através do campo do código de estado do cabeçalho de mensagem de resposta HTTP, como 4xx ou 5xx erros:

| Código de Estado | Motivo de erro | Como processar |
| ----------- | ------------ | ------------- |
| 429 demasiados muitos pedidos. |  Limitar IMDS foi atingido o limite. | Tente novamente com término exponencial. Consulte a documentação de orientação abaixo. |
| Erro de 4xx no pedido. | Um ou mais dos parâmetros estavam incorreta. | Não repita.  Analise os detalhes do erro para obter mais informações.  erros de 4xx são erros em tempo de design.|
| 5XX erro transitório de serviço. | O sistema secundárias do MSI ou o Azure Active Directory devolveu um erro transitório. | É seguro Repita após uma espera de, pelo menos, 1 segundo.  Se a repetir a forma demasiado rápida ou demasiadas vezes, IMDS e/ou do Azure AD pode devolver um erro de limite de velocidade (429).|
| 404 não encontrado. | Ponto final IMDS está a atualizar. | Tente novamente com Expontential término. Consulte a documentação de orientação abaixo. |
| tempo limite | Ponto final IMDS está a atualizar. | Tente novamente com Expontential término. Consulte a documentação de orientação abaixo. |

Se ocorrer um erro, o corpo da resposta correspondente HTTP contém um JSON com os detalhes do erro:

| Elemento | Descrição |
| ------- | ----------- |
| erro   | Identificador de erro. |
| error_description | Descrição verbosa do erro. **Podem alterar as descrições de erro em qualquer altura. Não escreva código que ramos com base nos valores na descrição do erro.**|

### <a name="http-response-reference"></a>Referência de resposta HTTP

Esta secção documenta as respostas de erro possíveis. A "200 OK" estado é uma resposta com êxito e o token de acesso está contido no corpo da resposta JSON, no elemento access_token.

| Código de estado | Erro | Descrição de Erro | Solução |
| ----------- | ----- | ----------------- | -------- |
| Pedido de 400 incorreta | invalid_resource | AADSTS50001: A aplicação com o nome *\<URI\>* não foi encontrado no inquilino com o nome  *\<ID do INQUILINO\>*. Isto pode acontecer se a aplicação não foi instalada pelo administrador do inquilino ou autorizada por qualquer utilizador no inquilino. Poderá ter enviado o pedido de autenticação para o inquilino incorreto. \ | (Apenas Linux) |
| Pedido de 400 incorreta | bad_request_102 | Cabeçalho de metadados necessários não especificado | Ambos os `Metadata` campo de cabeçalho do pedido está em falta o pedido ou está formatado incorretamente. O valor tem de ser especificado como `true`, em todas as letras maiúsculas e minúsculas. Consulte o "exemplo de pedido" no [anterior a secção REST](#rest) para obter um exemplo.|
| 401 não autorizado | unknown_source | Origem  *\<URI\>* | Certifique-se de que o URI do pedido HTTP GET está formatado corretamente. O `scheme:host/resource-path` parte tem de ser especificada como `http://localhost:50342/oauth2/token`. Consulte o "exemplo de pedido" no [anterior a secção REST](#rest) para obter um exemplo.|
|           | invalid_request | O pedido tem um parâmetro necessário em falta, inclui um valor de parâmetro inválido, inclui um parâmetro de mais do que uma vez ou caso contrário, tem um formato incorreto. |  |
|           | unauthorized_client | O cliente não está autorizado para pedir um token de acesso através deste método. | Causado por um pedido que não utiliza local loopback para chamar a extensão ou numa VM que não tem um MSI configurado corretamente. Consulte [configurar uma VM geridos serviço de identidade (MSI) no portal do Azure](qs-configure-portal-windows-vm.md) se necessitar de assistência com a configuração de VM. |
|           | access_denied | O proprietário do recurso ou autorização servidor negou o pedido. |  |
|           | unsupported_response_type | O servidor de autorização não suporta a obtenção de um token de acesso através deste método. |  |
|           | invalid_scope | Âmbito do pedido é inválido, desconhecido ou com formato incorreto. |  |
| Erro interno do servidor 500 | desconhecido | Falha ao obter o token do Active directory. Para mais informações, consulte os registos no  *\<caminho do ficheiro\>* | Certifique-se de que foi ativado MSI da VM. Consulte [configurar uma VM geridos serviço de identidade (MSI) no portal do Azure](qs-configure-portal-windows-vm.md) se necessitar de assistência com a configuração de VM.<br><br>Certifique-se também de que o URI do pedido HTTP GET está formatado corretamente, particularmente o URI especificado na cadeia de consulta do recurso. Consulte o "exemplo de pedido" no [anterior a secção REST](#rest) por exemplo, ou [que suporte do Azure AD a autenticação de serviços do Azure](services-support-msi.md) para obter uma lista de serviços e os respetivos IDs de recurso.

## <a name="retry-guidance"></a>Repita a documentação de orientação 

Os limites de limitação aplicam-se ao número de chamadas efetuadas para o ponto final IMDS. Quando o limiar de limitação for excedido, o ponto final IMDS limita qualquer mais pedidos enquanto a limitação está em vigor. Durante este período, o ponto final IMDS irá devolver o código de estado HTTP 429 ("demasiados muitos pedidos"), e falharem os pedidos. 

Para repetir, recomendamos a estratégia de seguinte: 

| **Estratégia de repetição** | **Definições** | **Valores** | **Como funciona** |
| --- | --- | --- | --- |
|ExponentialBackoff |Contagem de repetições<br />Término mín.<br />Término máx.<br />Término delta<br />Primeira repetição rápida |5<br />0 s<br />60 s<br />2 s<br />false |Tentativa 1 – atraso de 0 s<br />Tentativa 2 – atraso de ~2 s<br />Tentativa 3 – atraso de ~6 s<br />Tentativa 4 – atraso de ~14 s<br />Tentativa 5 – atraso de ~30 s |

## <a name="resource-ids-for-azure-services"></a>IDs de recurso para serviços do Azure

Consulte [que suporte do Azure AD a autenticação de serviços do Azure](services-support-msi.md) para obter uma lista de recursos que suportam o Azure AD e foi testados com MSI e os respetivos IDs de recurso.


## <a name="related-content"></a>Conteúdo relacionado

- Para ativar o MSI numa VM do Azure, consulte [configurar uma VM geridos serviço de identidade (MSI) no portal do Azure](qs-configure-portal-windows-vm.md).

Utilize a seguinte secção de comentários para fornecer comentários e ajudam-nos refinar e formam o nosso conteúdo.








