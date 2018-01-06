---
title: "Autenticar com o Engagement móvel REST APIs"
description: Descreve como autenticar com as APIs REST do Azure Mobile Engagement
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: da82cb36-957a-4e19-a805-b44733cf6597
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 10/05/2016
ms.author: wesmc;ricksal
ms.openlocfilehash: 66bcd738b86f846eae3499b289a6629323009a44
ms.sourcegitcommit: d6984ef8cc057423ff81efb4645af9d0b902f843
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/05/2018
---
# <a name="authenticate-with-mobile-engagement-rest-apis"></a>Autenticar com o Engagement móvel REST APIs

## <a name="overview"></a>Descrição geral

Este documento descreve como obter um AD Oauth válido do Azure token para autenticar com as APIs REST do Mobile Engagement.

Presume-se que tiver uma subscrição do Azure válida e criou uma aplicação de Mobile Engagement através de um do [tutoriais de programador](mobile-engagement-windows-store-dotnet-get-started.md).

## <a name="authentication"></a>Autenticação

Um Microsoft Azure Active Directory baseado em token é utilizado para autenticação do OAuth. 

Por ordem ao pedido de autenticação de uma API, um cabeçalho de autorização tem de ser adicionado a cada pedido, o que é o seguinte formato:

    Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGmJlNmV2ZWJPamg2TTNXR1E...

> [!NOTE]
> Tokens do Azure Active Directory expirarem dentro de uma hora.
> 
> 

Existem várias formas de obter um token. Uma vez que as APIs são chamadas a partir de um serviço em nuvem, que pretende utilizar uma chave de API. Uma chave de API na terminologia do Azure é designado por uma palavra-passe de principal de serviço. O procedimento seguinte descreve uma forma de configurar manualmente.

### <a name="one-time-setup-using-script"></a>Uma única configuração (utilizando o script)

Siga o conjunto de instruções abaixo para executar o programa de configuração através de um script do PowerShell, que demora a data mínima para a configuração, mas utiliza as predefinições máximo permitidas. Opcionalmente, também pode seguir as instruções de [configuração manual](mobile-engagement-api-authentication-manual.md) para fazê-lo a partir do portal do Azure diretamente e configuração de melhorar efetue.

1. Obter a versão mais recente do Azure PowerShell de [aqui](http://aka.ms/webpi-azps). Para obter mais informações sobre as instruções de transferência, pode ver este [ligação](/powershell/azure/overview).
2. Depois de instalado o Azure PowerShell, utilize os seguintes comandos para se certificar de que tem o **módulo Azure** instalado:

    a. Certifique-se de que o módulo Azure PowerShell está disponível na lista de módulos disponíveis.

        Get-Module –ListAvailable

    ![Módulos do Azure disponíveis][1]

    b. Se não encontrar o módulo Azure PowerShell na lista acima, terá de executar:

        Import-Module Azure
3. Inicie sessão no Azure Resource Manager a partir do PowerShell, executando o seguinte comando e fornecer o nome de utilizador e palavra-passe para a sua conta do Azure: 

        Login-AzureRmAccount
4. Se tiver várias subscrições, em seguida, deve executar:

    a. Obter uma lista de todas as subscrições e copie o ID de subscrição da subscrição que pretende utilizar. Certifique-se que esta subscrição é o mesmo que tenha a aplicação de Mobile Engagement, que pretender interagir com a utilização de APIs. 

        Get-AzureRmSubscription

    b. Execute o seguinte comando fornecer o ID de subscrição para configurar a subscrição a ser utilizado.

        Select-AzureRmSubscription –SubscriptionId <subscriptionId>
5. Copiar o texto para o [New-AzureRmServicePrincipalOwner.ps1](https://raw.githubusercontent.com/matt-gibbs/azbits/master/src/New-AzureRmServicePrincipalOwner.ps1) script no seu computador local e guardá-lo como um cmdlet do PowerShell (por exemplo, `APIAuth.ps1`) e executá-lo `.\APIAuth.ps1`.
6. O script irá pedir-lhe fornecer uma entrada para **principalName**. Forneça um nome adequado aqui que pretende utilizar para criar a sua aplicação do Active Directory (por exemplo, APIAuth). 
7. Após a conclusão do script, apresentará as seguintes quatro valores que precisa de autenticar através de programação com o AD, por isso, certifique-se ao copiá-los. 

    **TenantId**, **SubscriptionId**, **ApplicationId**, e **segredo**.

    Irá utilizar o TenantId como `{TENANT_ID}`, ApplicationId como `{CLIENT_ID}` e secreta como `{CLIENT_SECRET}`.

   > [!NOTE]
   > A política de segurança predefinidas pode a bloquear a execução de scripts do PowerShell. Se assim for, temporariamente configurar a política de execução para permitir a execução do script utilizando o seguinte comando:
   > 
   > Set-ExecutionPolicy RemoteSigned
8. Eis como o conjunto de cmdlets do PS teria aspeto.
    ![][3]
9. No portal do Azure, aceda ao Active Directory, clique em **registos de aplicação** e pesquisa para a sua aplicação para se certificar de existe![][4]

### <a name="steps-to-get-a-valid-token"></a>Passos para obter um token válido

1. Chamar a API com os seguintes parâmetros e certifique-se de que substitui o INQUILINO\_ID, o cliente\_ID e o cliente\_segredo:
   
   * **O URL de pedido** como`https://login.microsoftonline.com/{TENANT_ID}/oauth2/token`
   * **Cabeçalho de HTTP. o Content-Type** como`application/x-www-form-urlencoded`
   * **Corpo do pedido de HTTP** como`grant_type=client\_credentials&client_id={CLIENT_ID}&client_secret={CLIENT_SECRET}&resource=https%3A%2F%2Fmanagement.core.windows.net%2F`
     
    Segue-se um exemplo de pedido:
    ```
    POST /{TENANT_ID}/oauth2/token HTTP/1.1
    Host: login.microsoftonline.com
    Content-Type: application/x-www-form-urlencoded
    grant_type=client_credentials&client_id={CLIENT_ID}&client_secret={CLIENT_SECRET}&reso
    urce=https%3A%2F%2Fmanagement.core.windows.net%2F
    ```
    Eis um exemplo de resposta:
    ```
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    Content-Length: 1234

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1445395811","not_before":"144
    5391911","resource":"https://management.core.windows.net/","access_token":{ACCESS_TOKEN}}
    ```
     Neste exemplo, incluído codificação do URL de parâmetros POST, `resource` valor é realmente `https://management.core.windows.net/`. Tenha o cuidado de URL também codificar `{CLIENT_SECRET}` como pode conter carateres especiais.

     > [!NOTE]
     > Para fins de teste, pode utilizar uma ferramenta de cliente HTTP como [Fiddler](http://www.telerik.com/fiddler) ou [extensões do Chrome Postman](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop) 
     > 
     > 
2. Em cada chamada à API, incluem agora o cabeçalho de autorização do pedido:
   
        Authorization: Bearer {ACCESS_TOKEN}
   
    Se obtiver um código de 401 estado devolvido, verifique o corpo da resposta, poderá indicar o token expirou. Nesse caso, obtenha um novo token.

## <a name="using-the-apis"></a>Com as APIs
Agora que tem um token válido, está pronto para tornar as chamadas de API.

1. Em cada pedido de API, terá de transmitir um token válido e não expirado que obteve na secção anterior.
2. Terá de plug-in alguns parâmetros para o pedido de URI que identifica a sua aplicação. O pedido de URI o seguinte aspeto
   
        https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/
        providers/Microsoft.MobileEngagement/appcollections/{app-collection}/apps/{app-resource-name}/
   
    Para obter os parâmetros, clique no nome da sua aplicação e clique em Dashboard e será apresentada uma página semelhante ao seguinte com todos os 3 parâmetros.
   
   * **1** `{subscription-id}`
   * **2** `{app-collection}`
   * **3** `{app-resource-name}`
   * **4** nome do seu grupo de recursos vai ser **MobileEngagement** , exceto se tiver criado um novo. 

> [!NOTE]
> <br/>
> 
> 1. Ignore o endereço de raiz de API, porque este era para as APIs anteriores.<br/>
> 2. Se criou a aplicação através do portal do Azure, em seguida, tem de utilizar o nome de recurso de aplicação que é diferente do nome da aplicação em si. Se criou a aplicação no portal do Azure, em seguida, deve utilizar o nome de aplicação em si (não há nenhum diferenciação entre o nome do recurso de aplicação e o nome da aplicação para aplicações criadas no portal do novo).  
> 
> 

<!-- Images -->
[1]: ./media/mobile-engagement-api-authentication/azure-module.png
[2]: ./media/mobile-engagement-api-authentication/mobile-engagement-api-uri-params.png
[3]: ./media/mobile-engagement-api-authentication/ps-cmdlets.png
[4]: ./media/mobile-engagement-api-authentication/search-app.png



