---
title: Os registos de auditoria exemplos e definições no Azure Active Directory B2C | Microsoft Docs
description: Guia e exemplos em aceder os registos de auditoria do Azure AD B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.topic: article
ms.workload: identity
ms.date: 08/04/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 4828bf2f0faa596c8222c3a36dc3d38ec1b3bd1a
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34709823"
---
# <a name="accessing-azure-ad-b2c-audit-logs"></a>Ao aceder aos registos de auditoria do Azure AD B2C

O Azure Active Directory B2C (Azure AD B2C) emite os registos de auditoria que contém a atividade obter informações sobre recursos de B2C, emitido tokens e acesso de administrador. Este artigo fornece uma breve descrição geral das informações disponíveis através de registos de auditoria e instruções sobre como aceder a estes dados para o seu inquilino do Azure AD B2C.

> [!IMPORTANT]
> Os registos de auditoria são apenas mantidos durante sete dias. Planear transferir e armazenar os registos através de um dos métodos abaixo se precisa de um período de retenção mais longo. 

##<a name="overview-of-activities-available-in-the-b2c-category-of-audit-logs"></a>Descrição geral de atividades disponíveis na categoria de B2C de registos de auditoria
O **B2C** categoria nos registos de auditoria contém os seguintes tipos de atividades:
|Tipo de atividade |Descrição  |
|---------|---------|
|Autorização |Atividades que dizem respeito à autorização de um utilizador para aceder aos B2C recursos (por exemplo, um administrador de acesso a uma lista das políticas de B2C)         |
|Diretório |Atividades relacionadas com os atributos de diretório obtidos quando um administrador inicia sessão utilizando o Portal do Azure |
|Aplicação | Operações CRUD em aplicações do B2C |
|Chave |Operações CRUD nos chaves armazenadas no contentor de chaves do B2C |
|Recurso |Operações CRUD nos recursos de B2C (por exemplo, políticas e fornecedores de identidade)
|Autenticação |Validação de credenciais de utilizador e de emissão de tokens|

> [!NOTE]
> Atividades CRUD de objeto de utilizador, consulte o **Core diretório** categoria.

##<a name="example-activity"></a>Atividade de exemplo
O exemplo abaixo mostra os dados capturados quando um utilizador inicia sessão com um fornecedor de identidade externas: ![registos de auditoria - exemplo](./media/active-directory-b2c-reference-audit-logs/audit-logs-example.png)

##<a name="accessing-audit-logs-through-the-azure-portal"></a>Aceder a registos de auditoria através do Portal do Azure
1. Aceda ao [Portal do Azure](https://portal.azure.com). Certifique-se de que está no diretório do B2C.
2. Clique em **do Azure Active Directory** na barra de Favoritos no lado esquerdo 
    
    ![Registos de auditoria - botão do AAD](./media/active-directory-b2c-reference-audit-logs/audit-logs-portal-aad.png)

1. Em **atividade**, clique em **registos de auditoria**

    ![Registos de auditoria - secção de registos](./media/active-directory-b2c-reference-audit-logs/audit-logs-portal-section.png)

2. No **categoria** dropbox, selecione **B2C**
3. Clique em **aplicar**

    ![Registos de auditoria - categoria](./media/active-directory-b2c-reference-audit-logs/audit-logs-portal-category.png)

Verá uma lista de atividades registados nos últimos sete dias. 
- Utilize o **tipo de recurso de atividade** pendente para filtrar os tipos de atividade descritos acima
- Utilize o **intervalo de datas** pendente para filtrar o intervalo de datas das atividades mostrado
- Se clicar numa linha específica na lista, uma caixa contextual à direita irá mostrar-lhe atributos adicionais associados a atividade
- Clique em **transferir** para transferir as atividades como um ficheiro csv

##<a name="accessing-audit-logs-through-the-azure-ad-reporting-api"></a>Aceder a registos de auditoria através do Azure AD API do relatório
Os registos de auditoria são publicados no pipeline mesmo como outras atividades para o Azure Active Directory, para que possam ser acedidos através de [Azure Active Directory API do relatório](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-reporting-api-audit-reference). 

###<a name="prerequisites"></a>Pré-requisitos
Para autenticar para o Azure AD API do relatório terá primeiro de registar uma aplicação. Certifique-se que siga os passos no [pré-requisitos para o Azure AD reporting APIs de acesso](https://azure.microsoft.com/documentation/articles/active-directory-reporting-api-getting-started/).

###<a name="accesing-the-api"></a>Accesing a API
Para transferir os registos de auditoria através da API do Azure AD B2C, poderá ser útil filtrar os registos para o **B2C** categoria. Para filtrar por categoria, utilize o parâmetro de cadeia de consulta ao chamar o Azure AD reporting ponto final de API, conforme mostrado abaixo:

`https://graph.windows.net/your-b2c-tentant.onmicrosoft.com/activities/audit?api-version=beta&$filter=category eq 'B2C'`

###<a name="powershell-script"></a>Script do PowerShell
O script seguinte fornece um exemplo de utilização do PowerShell para consultar o Azure AD API do relatório e armazenar os resultados como um ficheiro JSON:

```powershell
# This script will require registration of a Web Application in Azure Active Directory (see https://azure.microsoft.com/documentation/articles/active-directory-reporting-api-getting-started/)

# Constants
$ClientID       = "your-client-application-id-here"       # Insert your application's Client ID, a Globally Unique ID (registered by Global Admin)
$ClientSecret   = "your-client-application-secret-here"   # Insert your application's Client Key/Secret string
$loginURL       = "https://login.microsoftonline.com"     
$tenantdomain   = "your-b2c-tenant.onmicrosoft.com"       # AAD B2C Tenant; for example, contoso.onmicrosoft.com
$resource       = "https://graph.windows.net"             # Azure AD Graph API resource URI
$7daysago       = "{0:s}" -f (get-date).AddDays(-7) + "Z" # Use 'AddMinutes(-5)' to decrement minutes, for example
Write-Output "Searching for events starting $7daysago"

# Create HTTP header, get an OAuth2 access token based on client id, secret and tenant domain
$body       = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}
$oauth      = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body

# Parse audit report items, save output to file(s): auditX.json, where X = 0 thru n for number of nextLink pages
if ($oauth.access_token -ne $null) {   
    $i=0
    $headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}
    $url = 'https://graph.windows.net/' + $tenantdomain + '/activities/audit?api-version=beta&$filter=category eq ''B2C''and activityDate gt ' + $7daysago 

    # loop through each query page (1 through n)
    Do{
        # display each event on the console window
        Write-Output "Fetching data using Uri: $url"
        $myReport = (Invoke-WebRequest -UseBasicParsing -Headers $headerParams -Uri $url)
        foreach ($event in ($myReport.Content | ConvertFrom-Json).value) {
            Write-Output ($event | ConvertTo-Json)
        }

        # save the query page to an output file
        Write-Output "Save the output to a file audit$i.json"
        $myReport.Content | Out-File -FilePath audit$i.json -Force
        $url = ($myReport.Content | ConvertFrom-Json).'@odata.nextLink'
        $i = $i+1
    } while($url -ne $null)
} else {
    Write-Host "ERROR: No Access Token"
}
```

