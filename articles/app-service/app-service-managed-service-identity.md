---
title: Gerido identidades no serviço de aplicações e funções do Azure | Documentos da Microsoft
description: Guia de referência e a configuração conceitual para identidades geridas no App Service do Azure e as funções do Azure
services: app-service
author: mattchenderson
manager: cfowler
editor: ''
ms.service: app-service
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 06/25/2018
ms.author: mahender
ms.openlocfilehash: fb9b50ecb16bd37d005403a14ea11c6d89f50dfe
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46983652"
---
# <a name="how-to-use-managed-identities-for-app-service-and-azure-functions"></a>Como utilizar identidades geridas para o serviço de aplicações e funções do Azure

> [!NOTE] 
> Serviço de aplicações no Linux e aplicações Web para contentores não suportam atualmente identidades geridas.

> [!Important] 
> Identidades geridas para o serviço de aplicações e funções do Azure não se comporte como esperado se a sua aplicação é migrada através de subscrições/inquilinos. A aplicação precisa de obter uma nova identidade, que pode ser feita por desabilitar e Reabilitar a funcionalidade. Ver [remover uma identidade](#remove) abaixo. Recursos de Downstream também tem de ter atualizadas para utilizar a nova identidade de políticas de acesso.

Este tópico mostra-lhe como criar uma identidade gerida para aplicações de serviço de aplicações e funções do Azure e como usá-lo para aceder a outros recursos. Uma identidade gerida do Azure Active Directory permite à aplicação aceder facilmente aos outros recursos protegidos do AAD, como o Azure Key Vault. A identidade é gerida pela plataforma do Azure e não necessita de aprovisionar ou girar quaisquer segredos. Para mais informações sobre identidades geridas no AAD, consulte [geridos identidades para recursos do Azure](../active-directory/managed-identities-azure-resources/overview.md).

## <a name="creating-an-app-with-an-identity"></a>Criar uma aplicação com uma identidade

A criação de uma aplicação com uma identidade requer uma propriedade adicional para ser definido no aplicativo.

### <a name="using-the-azure-portal"></a>Utilizar o portal do Azure

Para configurar uma identidade gerida no portal, primeiro criar uma aplicação como normal e, em seguida, ativar a funcionalidade.

1. Crie uma aplicação no portal, tal como faria normalmente. Navegue para o mesmo no portal.

2. Se utilizar uma aplicação de função, navegue para **funcionalidades de plataforma**. Para outros tipos de aplicação, desloque para baixo para o **definições** grupo na navegação à esquerda.

3. Selecione **identidade gerida**.

4. Comutador **registar com o Azure Active Directory** ao **no**. Clique em **Guardar**.

![Identidade gerida no serviço de aplicações](media/app-service-managed-service-identity/msi-blade.png)

### <a name="using-the-azure-cli"></a>Com a CLI do Azure

Para configurar uma identidade gerida com a CLI do Azure, terá de utilizar o `az webapp identity assign` comando em relação a uma aplicação existente. Tem três opções para executar os exemplos nesta secção:

- Uso [Azure Cloud Shell](../cloud-shell/overview.md) do portal do Azure.
- Utilize o embedded Azure Cloud Shell através do "Experimente-lo" botão do, localizado no canto superior direito de cada bloco de código abaixo.
- [Instalar a versão mais recente da CLI do Azure](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.31 ou posterior) se preferir utilizar uma consola CLI local. 

Os passos seguintes orientam-na criar uma aplicação web e atribuir-lhe uma identidade com a CLI:

1. Se estiver a utilizar a CLI do Azure numa consola local, primeiro inicie sessão no Azure com [az login](/cli/azure/reference-index#az-login). Utilize uma conta que seja associada à subscrição do Azure sob a qual pretende implementar a aplicação:

    ```azurecli-interactive
    az login
    ```
2. Crie uma aplicação web com a CLI. Para obter mais exemplos de como utilizar a CLI com o serviço de aplicações, consulte [exemplos da CLI do serviço de aplicações](../app-service/app-service-cli-samples.md):

    ```azurecli-interactive
    az group create --name myResourceGroup --location westus
    az appservice plan create --name myPlan --resource-group myResourceGroup --sku S1
    az webapp create --name myApp --resource-group myResourceGroup --plan myPlan
    ```

3. Execute o `identity assign` comando para criar a identidade para esta aplicação:

    ```azurecli-interactive
    az webapp identity assign --name myApp --resource-group myResourceGroup
    ```

### <a name="using-azure-powershell"></a>Utilizar o Azure PowerShell

Os passos seguintes orientam-na criar uma aplicação web e atribuir-lhe uma identidade com o Azure PowerShell:

1. Se for preciso, instale o Azure PowerShell com a instrução que se encontra no [Guia do Azure PowerShell](/powershell/azure/overview) e, em seguida, execute `Login-AzureRmAccount` para criar uma ligação ao Azure.

2. Crie uma aplicação web com o Azure PowerShell. Para obter mais exemplos de como utilizar o Azure PowerShell com o serviço de aplicações, consulte [exemplos do PowerShell do serviço de aplicações](../app-service/app-service-powershell-samples.md):

    ```azurepowershell-interactive
    # Create a resource group.
    New-AzureRmResourceGroup -Name myResourceGroup -Location $location
    
    # Create an App Service plan in Free tier.
    New-AzureRmAppServicePlan -Name $webappname -Location $location -ResourceGroupName myResourceGroup -Tier Free
    
    # Create a web app.
    New-AzureRmWebApp -Name $webappname -Location $location -AppServicePlan $webappname -ResourceGroupName myResourceGroup
    ```

3. Execute o `identity assign` comando para criar a identidade para esta aplicação:

    ```azurepowershell-interactive
    Set-AzureRmWebApp -AssignIdentity $true -Name $webappname -ResourceGroupName myResourceGroup 
    ```

### <a name="using-an-azure-resource-manager-template"></a>Com um modelo Azure Resource Manager

Um modelo Azure Resource Manager pode ser utilizado para automatizar a implantação dos seus recursos do Azure. Para saber mais sobre como implementar o serviço de aplicações e funções, veja [automatizando a implantação de recursos no serviço de aplicações](../app-service/app-service-deploy-complex-application-predictably.md) e [automatizando a implantação de recursos nas funções do Azure](../azure-functions/functions-infrastructure-as-code.md).

Qualquer tipo de recurso `Microsoft.Web/sites` podem ser criadas com uma identidade, incluindo a seguinte propriedade na definição do recurso:
```json
"identity": {
    "type": "SystemAssigned"
}    
```

Isso informa ao Azure para criar e gerir a identidade para a sua aplicação.

Por exemplo, uma aplicação web pode ser semelhante ao seguinte:
```json
{
    "apiVersion": "2016-08-01",
    "type": "Microsoft.Web/sites",
    "name": "[variables('appName')]",
    "location": "[resourceGroup().location]",
    "identity": {
        "type": "SystemAssigned"
    },
    "properties": {
        "name": "[variables('appName')]",
        "serverFarmId": "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]",
        "hostingEnvironment": "",
        "clientAffinityEnabled": false,
        "alwaysOn": true
    },
    "dependsOn": [
        "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]"
    ]
}
```

Quando o site for criado, ele tem as seguintes propriedades adicionais:
```json
"identity": {
    "tenantId": "<TENANTID>",
    "principalId": "<PRINCIPALID>"
}
```

Em que `<TENANTID>` e `<PRINCIPALID>` são substituídos por GUIDs. A propriedade tenantId identifica quais pertence a identidade do inquilino do AAD. O principalId é um identificador exclusivo para a identidade da aplicação de novo. Dentro do AAD, o principal de serviço tem o mesmo nome que deu à sua instância de serviço de aplicações ou funções do Azure.

## <a name="obtaining-tokens-for-azure-resources"></a>Obter os tokens para recursos do Azure

Uma aplicação pode utilizar a sua identidade para obter os tokens para outros recursos protegidos pelo AAD, como o Azure Key Vault. Estes tokens representam o aplicativo acessando o recurso e não qualquer utilizador específico do aplicativo. 

> [!IMPORTANT]
> Terá de configurar o recurso de destino para permitir o acesso a partir da sua aplicação. Por exemplo, se solicitar um token para o Key Vault, terá de certificar-se de que adicionou uma política de acesso que inclui a identidade do seu aplicativo. Caso contrário, as chamadas para o Key Vault serão rejeitadas, mesmo que eles incluem o token. Para saber mais sobre quais os recursos que suportam tokens do Azure Active Directory, veja [que o suporte do Azure AD a autenticação dos serviços Azure](../active-directory/managed-identities-azure-resources/services-support-msi.md#azure-services-that-support-azure-ad-authentication).

Há um protocolo REST simple para obter um token no serviço de aplicações e funções do Azure. Para aplicativos .NET, a biblioteca de Microsoft.Azure.Services.AppAuthentication fornece uma abstração sobre este protocolo e oferece suporte a uma experiência de desenvolvimento local.

### <a name="asal"></a>Usando a biblioteca de Microsoft.Azure.Services.AppAuthentication para .NET

Para aplicações de .NET e as funções, a maneira mais simples de trabalhar com uma identidade gerida é de pacote Microsoft.Azure.Services.AppAuthentication. Esta biblioteca também permitirá que teste seu código localmente no computador de desenvolvimento, com a sua conta de utilizador a partir do Visual Studio, o [CLI do Azure](/cli/azure), ou autenticação integrada do Active Directory. Para obter mais informações sobre as opções de desenvolvimento local com esta biblioteca, consulte a [Referência de Microsoft.Azure.Services.AppAuthentication]. Esta secção mostra-lhe como começar com a biblioteca no seu código.

1. Adicionar referências para o [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication) e [Microsoft.Azure.KeyVault](https://www.nuget.org/packages/Microsoft.Azure.KeyVault) pacotes de NuGet ao seu aplicativo.

2.  Adicione o seguinte código ao seu aplicativo:

```csharp
using Microsoft.Azure.Services.AppAuthentication;
using Microsoft.Azure.KeyVault;
// ...
var azureServiceTokenProvider = new AzureServiceTokenProvider();
string accessToken = await azureServiceTokenProvider.GetAccessTokenAsync("https://vault.azure.net");
// OR
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(azureServiceTokenProvider.KeyVaultTokenCallback));
```

Para saber mais sobre Microsoft.Azure.Services.AppAuthentication e as operações que expõe, veja a [Referência de Microsoft.Azure.Services.AppAuthentication] e o [serviço de aplicações e o Cofre de chaves com o .NET de MSI exemplo](https://github.com/Azure-Samples/app-service-msi-keyvault-dotnet).

### <a name="using-the-rest-protocol"></a>Utilizando o protocolo REST

Uma aplicação com uma identidade gerida tem duas variáveis de ambiente definidas:
- MSI_ENDPOINT
- MSI_SECRET

O **MSI_ENDPOINT** é um URL local a partir do qual a aplicação pode pedir tokens. Para obter um token para um recurso, fazer uma solicitação HTTP GET para este ponto final, incluindo os seguintes parâmetros:

> [!div class="mx-tdBreakAll"]
> |Nome do parâmetro|Em|Descrição|
> |-----|-----|-----|
> |Recurso|Consulta|O URI do recurso do recurso do AAD para que deve ser obtido de um token.|
> |versão de API|Consulta|A versão da API do token a ser utilizado. "2017-09-01" está atualmente a única versão suportada.|
> |segredo|Cabeçalho|O valor da variável de ambiente MSI_SECRET.|


Uma resposta 200 OK bem-sucedida inclui um corpo JSON com as seguintes propriedades:

> [!div class="mx-tdBreakAll"]
> |Nome da propriedade|Descrição|
> |-------------|----------|
> |access_token|O token de acesso solicitado. O serviço de web chamada pode utilizar este token para autenticar para o serviço web de recebimento.|
> |expires_on|O tempo que o token de acesso expira. A data é representada como o número de segundos de 1970-01-01T0:0:0Z UTC até a hora de expiração. Este valor é utilizado para determinar o tempo de vida de tokens em cache.|
> |Recurso|O URI de ID de aplicação do serviço web do recetor.|
> |token_type|Indica o valor de tipo de token. O único tipo que o Azure AD suporta é portador. Para obter mais informações sobre os tokens de portador, consulte [o Framework de autorização de OAuth 2.0: utilização de Token de portador (RFC 6750)](http://www.rfc-editor.org/rfc/rfc6750.txt).|


Esta resposta é igual a [resposta para o pedido de token de acesso de serviço para serviço AAD](../active-directory/develop/v1-oauth2-client-creds-grant-flow.md#service-to-service-access-token-response).

> [!NOTE] 
> Variáveis de ambiente são configuradas quando o processo é iniciado pela primeira vez, portanto, depois de ativar uma identidade gerida para a sua aplicação, poderá ter de reiniciar a aplicação ou voltar a implementar seu código, antes `MSI_ENDPOINT` e `MSI_SECRET` estão disponíveis para o seu código.

### <a name="rest-protocol-examples"></a>Exemplos de protocolo REST
Um exemplo de solicitação pode ser semelhante ao seguinte:
```
GET /MSI/token?resource=https://vault.azure.net&api-version=2017-09-01 HTTP/1.1
Host: localhost:4141
Secret: 853b9a84-5bfa-4b22-a3f3-0b9a43d9ad8a
```
E uma resposta de exemplo poderá ser semelhante ao seguinte:
```
HTTP/1.1 200 OK
Content-Type: application/json
 
{
    "access_token": "eyJ0eXAi…",
    "expires_on": "09/14/2017 00:00:00 PM +00:00",
    "resource": "https://vault.azure.net",
    "token_type": "Bearer"
}
```

### <a name="code-examples"></a>Exemplos de código
<a name="token-csharp"></a>Para fazer este pedido em c#:
```csharp
public static async Task<HttpResponseMessage> GetToken(string resource, string apiversion)  {
    HttpClient client = new HttpClient();
    client.DefaultRequestHeaders.Add("Secret", Environment.GetEnvironmentVariable("MSI_SECRET"));
    return await client.GetAsync(String.Format("{0}/?resource={1}&api-version={2}", Environment.GetEnvironmentVariable("MSI_ENDPOINT"), resource, apiversion));
}
```
> [!TIP]
> Para linguagens .NET, também pode utilizar [Microsoft.Azure.Services.AppAuthentication](#asal) em vez de criar este pedido por conta própria.

<a name="token-js"></a>Em node. js:
```javascript
const rp = require('request-promise');
const getToken = function(resource, apiver, cb) {
    var options = {
        uri: `${process.env["MSI_ENDPOINT"]}/?resource=${resource}&api-version=${apiver}`,
        headers: {
            'Secret': process.env["MSI_SECRET"]
        }
    };
    rp(options)
        .then(cb);
}
```

<a name="token-powershell"></a>No PowerShell:
```powershell
$apiVersion = "2017-09-01"
$resourceURI = "https://<AAD-resource-URI-for-resource-to-obtain-token>"
$tokenAuthURI = $env:MSI_ENDPOINT + "?resource=$resourceURI&api-version=$apiVersion"
$tokenResponse = Invoke-RestMethod -Method Get -Headers @{"Secret"="$env:MSI_SECRET"} -Uri $tokenAuthURI
$accessToken = $tokenResponse.access_token
```

## <a name="remove"></a>Remover uma identidade

Uma identidade pode ser removida ao desativar a funcionalidade com o portal, o PowerShell ou CLI da mesma forma que tenha sido criado. O protocolo de modelo REST/ARM, isso é feito ao definir o tipo como "None":

```json
"identity": {
    "type": "None"
}    
```

Remover a identidade dessa forma também irá eliminar a entidade de segurança do AAD. Identidiades atribuídas por sistema são automaticamente removidos do AAD, quando o recurso de aplicação é eliminado.

> [!NOTE] 
> Também é uma definição da aplicação que pode ser definida, WEBSITE_DISABLE_MSI, que simplesmente desativa o serviço de token de local. No entanto, ele deixa a identidade no local e as ferramentas ainda irão mostrar a identidade gerida como "ativado" ou "ativado". Como resultado, a utilização desta definição não é recomendado.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Base de dados do SQL de acesso de forma segura com uma identidade gerida](app-service-web-tutorial-connect-msi.md)

[Referência de Microsoft.Azure.Services.AppAuthentication]: https://go.microsoft.com/fwlink/p/?linkid=862452
