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
ms.openlocfilehash: 574e699a1cfca2caef0cf20872570bbb8650117b
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/23/2018
---
# <a name="authenticate-with-mobile-engagement-rest-apis"></a>Autenticar com o Engagement móvel REST APIs

## <a name="overview"></a>Descrição geral

Este documento descreve como obter um OAuth válido do Azure Active Directory (Azure AD) token para autenticar com as APIs REST do Mobile Engagement.

Este procedimento assume que tem uma subscrição do Azure válida e criou uma aplicação de Mobile Engagement utilizando um do [tutoriais de programador](mobile-engagement-windows-store-dotnet-get-started.md).

## <a name="authentication"></a>Autenticação

Um token de OAuth baseado no Microsoft Azure Active Directory é utilizado para autenticação. 

Para autenticar um pedido de API, um cabeçalho de autorização tem de ser adicionado a cada pedido. O cabeçalho de autorização é o seguinte formato:

    Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGmJlNmV2ZWJPamg2TTNXR1E...

> [!NOTE]
> Tokens do Azure Active Directory expirarem dentro de uma hora.
> 
> 

Existem várias formas de obter um token. Porque as APIs são chamadas a partir de um serviço em nuvem, que pretende utilizar uma chave de API. Uma chave de API na terminologia do Azure é designado por uma palavra-passe do Principal de serviço. O procedimento seguinte descreve uma forma de configurá-lo manualmente.

### <a name="one-time-setup-using-a-script"></a>Uma única configuração (utilizando um script)

Para executar o programa de configuração ao utilizar um script do PowerShell, execute os passos nas instruções seguintes. Um script do PowerShell exige o mínimo de tempo para a configuração, mas utiliza as predefinições máximo permitidas. 

Opcionalmente, também pode seguir as instruções de [configuração manual](mobile-engagement-api-authentication-manual.md) para fazê-lo do portal do Azure diretamente. Quando configurar a partir do portal do Azure, pode efetuar uma configuração mais detalhada.

1. Obter a versão mais recente do Azure PowerShell por [transferindo-](http://aka.ms/webpi-azps). Para mais informações sobre as instruções de transferência, consulte [esta descrição geral](/powershell/azure/overview).

2. Depois do PowerShell está instalado, utilize os seguintes comandos para se certificar de que tem o **módulo Azure** instalado:

    a. Certifique-se de que o módulo Azure PowerShell está disponível na lista de módulos disponíveis.

        Get-Module –ListAvailable

    ![Módulos do Azure disponíveis][1]

    b. Se não encontrar o módulo Azure PowerShell na lista anterior, terá de executar:

        Import-Module Azure
3. Inicie sessão no Azure Resource Manager a partir do PowerShell, executando o seguinte comando. Forneça o nome de utilizador e palavra-passe para a sua conta do Azure: 

        Login-AzureRmAccount
4. Se tiver várias subscrições, siga os passos seguintes:

    a. Obter uma lista de todas as subscrições. Em seguida, copie o **SubscriptionId** da subscrição que pretende utilizar. Certifique-se esta subscrição com a aplicação de Mobile Engagement. Se pretender utilizar esta aplicação para interagir com as APIs. 

        Get-AzureRmSubscription

    b. Execute o seguinte comando. Forneça o **SubscriptionId** para configurar a subscrição que vai utilizar:

        Select-AzureRmSubscription –SubscriptionId <subscriptionId>
5. Copiar o texto para o [New-AzureRmServicePrincipalOwner.ps1](https://raw.githubusercontent.com/matt-gibbs/azbits/master/src/New-AzureRmServicePrincipalOwner.ps1) script no seu computador local. Em seguida, guarde-o como um cmdlet do PowerShell (por exemplo, `APIAuth.ps1`), e execute-o.

         `.\APIAuth.ps1`.

6. O script pede-lhe fornecer uma entrada para **principalName**. Forneça um nome adequado que pretende utilizar para a sua aplicação do Active Directory (por exemplo, APIAuth). 

7. Após o script em execução, apresenta os seguintes quatro valores. É necessário copiá-los, porque tem de autenticar através de programação com o Active Directory: 

   - **TenantId**
   - **SubscriptionId**
   - **ApplicationId**
   - **Segredo**

   Utilize o TenantId como `{TENANT_ID}`, ApplicationId como `{CLIENT_ID}` e secreta como `{CLIENT_SECRET}`.

   > [!NOTE]
   > A política de segurança predefinidos poderão bloquear a a execução de scripts do PowerShell. Se assim for, utilize o seguinte comando para configurar temporariamente a política de execução para permitir a execução do script:
   > 
   > Set-ExecutionPolicy RemoteSigned
8. Eis o aspeto do conjunto de cmdlets do PowerShell.
    ![Cmdlets do PowerShell][3]
9. No portal do Azure, aceda ao Active Directory, selecione **registos de aplicação**, e, em seguida, procure a aplicação para se certificar de existe.
    ![Pesquisa para a sua aplicação][4]

### <a name="steps-to-get-a-valid-token"></a>Passos para obter um token válido

1. Chame a API com os seguintes parâmetros. Certifique-se de que substitui **INQUILINO\_ID**, **cliente\_ID**, e **cliente\_segredo**:
   
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
    Segue-se um exemplo de resposta:
    ```
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    Content-Length: 1234

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1445395811","not_before":"144
    5391911","resource":"https://management.core.windows.net/","access_token":{ACCESS_TOKEN}}
    ```
     Este exemplo inclui a codificação do URL dos parâmetros POST, na qual `resource` valor é realmente `https://management.core.windows.net/`. Tenha o cuidado de codificar também URL `{CLIENT_SECRET}`, porque poderá conter carateres especiais.

     > [!NOTE]
     > Para fins de teste, pode utilizar uma ferramenta de cliente HTTP, tal como [Fiddler](http://www.telerik.com/fiddler) ou [extensões do Chrome Postman](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop). 
     > 
     > 
2. Em cada chamada à API, incluem agora o cabeçalho de autorização do pedido:
   
        Authorization: Bearer {ACCESS_TOKEN}
   
    Se o pedido de devolver um código de 401 Estado, verifique o corpo da resposta. -Pode indicam que o token expirou. Nesse caso, obtenha um novo token.

## <a name="use-the-apis"></a>Utilizar as APIs
Agora que tem um token válido, está pronto para tornar as chamadas de API.

1. Em cada pedido de API, tem de transmitir um token válido e não expirado. Obter o token não expirado na secção anterior.

2. Plug-in alguns parâmetros para o pedido de URI que identifique a sua aplicação. O pedido de URI aparente ser o seguinte código:
   
        https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/
        providers/Microsoft.MobileEngagement/appcollections/{app-collection}/apps/{app-resource-name}/
   
    Para obter os parâmetros, selecione o nome da sua aplicação. Em seguida, selecione **Dashboard**. Será apresentada uma página com três parâmetros, da seguinte forma:
   
   * **1** `{subscription-id}`
   * **2** `{app-collection}`
   * **3** `{app-resource-name}`
   * **4** nome do seu grupo de recursos vai ser **MobileEngagement** , exceto se tiver criado um novo. 

> [!NOTE]
> Ignore o endereço de raiz de API, porque estava para as APIs anteriores.
> 
> Se criou a aplicação através do portal do Azure, em seguida, tem de utilizar o nome de recurso de aplicação, o que é diferente do nome da aplicação em si. Se criou a aplicação no portal do Azure, deve utilizar o nome da aplicação. (Não há nenhum diferenciação entre o nome de recurso de aplicação e o nome da aplicação para aplicações que são criadas no portal de novo.)
> 
> 

<!-- Images -->
[1]: ./media/mobile-engagement-api-authentication/azure-module.png
[2]: ./media/mobile-engagement-api-authentication/mobile-engagement-api-uri-params.png
[3]: ./media/mobile-engagement-api-authentication/ps-cmdlets.png
[4]: ./media/mobile-engagement-api-authentication/search-app.png



