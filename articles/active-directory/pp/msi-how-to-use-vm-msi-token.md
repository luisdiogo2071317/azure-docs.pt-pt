---
title: "Como utilizar uma identidade de serviço geridas para utilizador atribuído ao adquirir um token de acesso numa VM."
description: "Passo a passo instruções e exemplos de utilização de um MSI utilizador atribuído a partir de uma VM do Azure para adquirir um OAuth token de acesso ao."
services: active-directory
documentationcenter: 
author: BryanLa
manager: mtillman
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/22/2017
ms.author: bryanla
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 5c9bf052ecb2e9c79e0eb627a0fd709d587125cd
ms.sourcegitcommit: a648f9d7a502bfbab4cd89c9e25aa03d1a0c412b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/22/2017
---
# <a name="acquire-an-access-token-for-a-vm-user-assigned-managed-service-identity-msi"></a>Adquirir um token de acesso para uma VM atribuída utilizador da identidade de serviço geridas (MSI)

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]Este artigo fornece vários exemplos de código e o script para a aquisição do token, bem como orientações tópicos importantes, como o tratamento de expiração do token e erros de HTTP.

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

Se planeia utilizar os Azure PowerShell exemplos neste artigo, não se esqueça de instalar a versão mais recente do [Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM).

> [!IMPORTANT]
> - Todos os código/script de exemplo neste artigo assume que o cliente está em execução numa máquina Virtual ativada de MSI. Utilize a funcionalidade VM "Ligar" no portal do Azure, para ligar remotamente à VM. Para obter mais informações sobre como ativar MSI numa VM, consulte [configurar um utilizador atribuído geridos serviço de identidade (MSI) para uma VM, utilizando a CLI do Azure](msi-qs-configure-cli-windows-vm.md), ou um dos artigos variantes (utilizando o Portal, PowerShell, um modelo ou um SDK do Azure). 

## <a name="overview"></a>Descrição geral

Uma aplicação cliente pode pedir um MSI [token de acesso só de aplicação](~/articles/active-directory/develop/active-directory-dev-glossary.md#access-token) para aceder a um recurso especificado. O token é [com base no principal de serviço MSI](msi-overview.md#how-does-it-work). Como tal, não é necessário para o cliente para se registar para obter um token de acesso na sua própria principal de serviço. O token é adequado para utilização como um token de portador no [que exigem que as credenciais de cliente de chamadas de serviço a serviço](~/articles/active-directory/active-directory-protocols-oauth-service-to-service.md).

|  |  |
| -------------- | -------------------- |
| [Obter um token através de HTTP](#get-a-token-using-http) | Detalhes de protocolo para o ponto de final de token de MSI |
| [Obter um token utilizando CURL](#get-a-token-using-curl) | Exemplo de como utilizar o ponto final de REST do MSI de um cliente de Bash/CURL |
| [Processamento de expiração do token](#handling-token-expiration) | Orientações para processar os tokens de acesso expirado |
| [Processamento de erros](#error-handling) | Orientações para processamento de erros de HTTP devolvidos do ponto final token MSI |
| [IDs de recurso para serviços do Azure](#resource-ids-for-azure-services) | Onde obter IDs de recurso para serviços do Azure suportados |

## <a name="get-a-token-using-http"></a>Obter um token através de HTTP 

A interface fundamental para adquirir um token de acesso é baseada em REST, tornando-o acessível para qualquer aplicação de cliente em execução na VM que pode efetuar chamadas de REST de HTTP. Isto é semelhante para o modelo de programação do Azure AD, exceto o cliente utiliza um ponto final localhost na máquina virtual (vs um Azure ponto final do AD).

Exemplo de pedido:

```
GET http://localhost:50342/oauth2/token?resource=https%3A%2F%2Fmanagement.azure.com%2F&client_id=712eac09-e943-418c-9be6-9fd5c91078bl HTTP/1.1
Metadata: true
```

| Elemento | Descrição |
| ------- | ----------- |
| `GET` | O verbo HTTP, com a indicação de que pretende obter dados do ponto final. Neste caso, um OAuth token de acesso ao. | 
| `http://localhost:50342/oauth2/token` | O MSI ponto final, onde 50342 é a porta predefinida e é configurável. |
| `resource` | Um cadeia parâmetro de consulta, que indica que o URI de ID de aplicação do recurso de destino. É também apresentado no `aud` afirmação (público-alvo) do token emitido. Neste exemplo pedidos de um token de acesso do Azure Resource Manager, que tem um URI de ID de aplicação de https://management.azure.com/. |
| `client_id` | Um cadeia parâmetro de consulta, que indica o ID de cliente (também conhecido como ID de aplicação) do principal de serviço que representa o MSI utilizador atribuído. Este valor é devolvido no `clientId` propriedade durante a criação de um MSI utilizador atribuído. Neste exemplo pedidos um token de ID de cliente "712eac09-e943-418c-9be6-9fd5c91078bl". |
| `Metadata` | Um HTTP pedido campo do cabeçalho, necessário para MSI como uma mitigação de ataques de falsificação de pedidos de lado do servidor (SSRF). Este valor tem de ser definido como "true", em todas as letras maiúsculas e minúsculas.

Resposta de amostra:

```
HTTP/1.1 200 OK
Content-Type: application/json
{
  "access_token": "eyJ0eXAi...",
  "expires_in": "3599",
  "expires_on": "1506484173",
  "not_before": "1506480273",
  "resource": "https://management.azure.com/",
  "token_type": "Bearer"
  "client_id":"712eac09-e943-418c-9be6-9fd5c91078bl"
}
```

| Elemento | Descrição |
| ------- | ----------- |
| `access_token` | O token de pedido de acesso. Quando chamar uma API de REST protegida, o token está incorporado a `Authorization` campo de cabeçalho de pedido como um token de "portador", permitindo que a API autenticar o autor da chamada. | 
| `expires_in` | O número de segundos que o token de acesso continua a ser válida, antes de expirar, a hora de emissão. Tempo de emissão pode ser encontrado no token de `iat` de afirmação. |
| `expires_on` | O período de tempo quando o token de acesso expira. A data é representada como o número de segundos de "1970-01-01T0:0:0Z UTC" (corresponde do token `exp` afirmação). |
| `not_before` | O período de tempo quando o token de acesso entra em vigor e pode ser aceites. A data é representada como o número de segundos de "1970-01-01T0:0:0Z UTC" (corresponde do token `nbf` afirmação). |
| `resource` | O recurso foi solicitado o token de acesso para que corresponde à `resource` parâmetro de cadeia do pedido de consulta. |
| `token_type` | O tipo de token, o que é um token de acesso de "Portador", o que significa que o recurso pode conceder acesso ao portador do token. |
| `client_id` | O ID de cliente (também conhecido como ID de aplicação) do principal de serviço que representa o MSI atribuído por utilizador, para o qual o token foi pedido. |

## <a name="get-a-token-using-curl"></a>Obter um token utilizando CURL

Não se esqueça de substituir o ID de cliente (também conhecido como ID de aplicação), do utilizador atribuído do MSI principal do serviço, para o <MSI CLIENT ID> valor o `client_id` parâmetro. Este valor é devolvido no `clientId` propriedade durante a criação de um MSI utilizador atribuído.

   ```bash
   response=$(curl http://localhost:50342/oauth2/token --data "resource=https://management.azure.com/&client_id=<MSI CLIENT ID>" -H Metadata:true -s)
   access_token=$(echo $response | python -c 'import sys, json; print (json.load(sys.stdin)["access_token"])')
   echo The MSI access token is $access_token
   ```

   Respostas de exemplo:

   ```bash
   user@vmLinux:~$ response=$(curl http://localhost:50342/oauth2/token --data "resource=https://management.azure.com/&client_id=9d484c98-b99d-420e-939c-z585174b63bl" -H Metadata:true -s)
   user@vmLinux:~$ access_token=$(echo $response | python -c 'import sys, json; print (json.load(sys.stdin)["access_token"])')
   user@vmLinux:~$ echo The MSI access token is $access_token
   The MSI access token is eyJ0eXAiOiJKV1QiLCJhbGciO...
   ```

## <a name="handling-token-expiration"></a>Processamento de expiração do token

O subsistema MSI local coloca em cache tokens. Por conseguinte, pode chamar-as vezes que é que gosta e resulta de uma chamada no-durante a transmissão para o Azure AD apenas se:
- ocorre uma falha de acerto na cache devido a nenhum token na cache
- o token expirou

Se a cache o token no seu código, estará preparado para processar cenários em que o recurso indica que o token expirou.

## <a name="error-handling"></a>Processamento de erros 

O ponto final MSI sinalizar erros através do campo do código de estado do cabeçalho de mensagem de resposta HTTP, como 4xx ou 5xx erros:

| Código de Estado | Motivo de erro | Como processar |
| ----------- | ------------ | ------------- |
| Erro de 4xx no pedido. | Um ou mais dos parâmetros estavam incorreta. | Não repita.  Analise os detalhes do erro para obter mais informações.  erros de 4xx são erros em tempo de design.|
| 5XX erro transitório de serviço. | O sistema secundárias do MSI ou o Azure Active Directory devolveu um erro transitório. | É seguro Repita após uma espera de, pelo menos, 1 segundo.  Se repetir forma demasiado rápida ou demasiadas vezes, o Azure AD pode devolver um erro de limite de velocidade (429).|

Se ocorrer um erro, o corpo da resposta correspondente HTTP contém um JSON com os detalhes do erro:

| Elemento | Descrição |
| ------- | ----------- |
| erro   | Identificador de erro. |
| error_description | Descrição verbosa do erro. **Podem alterar as descrições de erro em qualquer altura. Não escreva código que ramos com base nos valores na descrição do erro.**|

### <a name="http-response-reference"></a>Referência de resposta HTTP

Esta secção documenta as respostas de erro possíveis. A "200 OK" estado é uma resposta com êxito e o token de acesso está contido no corpo da resposta JSON, no elemento access_token.

| Código de estado | Erro | Descrição de Erro | Solução |
| ----------- | ----- | ----------------- | -------- |
| Pedido de 400 incorreta | invalid_resource | AADSTS50001: A aplicação com o nome  *\<URI\>*  não foi encontrado no inquilino com o nome  *\<ID do INQUILINO\>*. Isto pode acontecer se a aplicação não foi instalada pelo administrador do inquilino ou autorizada por qualquer utilizador no inquilino. Poderá ter enviado o pedido de autenticação para o inquilino incorreto. \ | (Apenas Linux) |
| Pedido de 400 incorreta | bad_request_102 | Cabeçalho de metadados necessários não especificado | Ambos os `Metadata` campo de cabeçalho do pedido está em falta o pedido ou está formatado incorretamente. O valor tem de ser especificado como `true`, em todas as letras maiúsculas e minúsculas. Consulte o "exemplo de pedido" no [obter um token a utilizar HTTP](#get-a-token-using-http) secção para obter um exemplo.|
| 401 não autorizado | unknown_source | Origem  *\<URI\>* | Certifique-se de que o URI do pedido HTTP GET está formatado corretamente. O `scheme:host/resource-path` parte tem de ser especificada como `http://localhost:50342/oauth2/token`. Consulte o "exemplo de pedido" no [obter um token a utilizar HTTP](#get-a-token-using-http) secção para obter um exemplo.|
|           | invalid_request | O pedido tem um parâmetro necessário em falta, inclui um valor de parâmetro inválido, inclui um parâmetro de mais do que uma vez ou caso contrário, tem um formato incorreto. |  |
|           | unauthorized_client | O cliente não está autorizado para pedir um token de acesso através deste método. | Causado por um pedido que não utiliza local loopback para chamar a extensão ou numa VM que não tem um MSI configurado corretamente. Consulte [configurar uma VM geridos serviço de identidade (MSI) no portal do Azure](msi-qs-configure-portal-windows-vm.md) se necessitar de assistência com a configuração de VM. |
|           | ACCESS_DENIED | O proprietário do recurso ou autorização servidor negou o pedido. |  |
|           | unsupported_response_type | O servidor de autorização não suporta a obtenção de um token de acesso através deste método. |  |
|           | invalid_scope | Âmbito do pedido é inválido, desconhecido ou com formato incorreto. |  |
| Erro interno do servidor 500 | desconhecido | Falha ao obter o token do Active directory. Para mais informações, consulte os registos no  *\<caminho do ficheiro\>* | Certifique-se de que foi ativado MSI da VM. Consulte [configurar uma VM geridos serviço de identidade (MSI) no portal do Azure](msi-qs-configure-portal-windows-vm.md) se necessitar de assistência com a configuração de VM.<br><br>Certifique-se também de que o URI do pedido HTTP GET está formatado corretamente, particularmente o URI especificado na cadeia de consulta do recurso. Consulte o "exemplo de pedido" no [obter um token a utilizar HTTP](#get-a-token-using-http) secção para obter um exemplo ou [que suporte do Azure AD a autenticação de serviços do Azure](msi-overview.md#azure-services-that-support-azure-ad-authentication) para obter uma lista de serviços e os respetivos IDs de recurso.

## <a name="resource-ids-for-azure-services"></a>IDs de recurso para serviços do Azure

Consulte [que suporte do Azure AD a autenticação de serviços do Azure](msi-overview.md#azure-services-that-support-azure-ad-authentication) para obter uma lista de recursos que suportam o Azure AD e foi testados com MSI e os respetivos IDs de recurso.


## <a name="next-steps"></a>Passos Seguintes

- Para ativar o MSI numa VM do Azure, consulte [configurar um utilizador atribuído geridos serviço de identidade (MSI) para uma VM, utilizando a CLI do Azure](msi-qs-configure-cli-windows-vm.md).

Utilize a seguinte secção de comentários para fornecer comentários e ajudam-nos refinar e formam o nosso conteúdo.








