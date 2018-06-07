---
title: Exemplos de API de auditoria de relatórios do Azure Active Directory | Microsoft Docs
description: Como começar com a API do Azure Active Directory Reporting Services
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.assetid: de8b8ec3-49b3-4aa8-93fb-e38f52c99743
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: compliance-reports
ms.date: 05/30/2018
ms.author: dhanyahk;rolyon
ms.reviewer: dhanyahk
ms.openlocfilehash: 77ecb1f0c4b3614c9692715edae21a09f261b277
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34588169"
---
# <a name="azure-active-directory-reporting-audit-api-samples"></a>Azure Active Directory amostras de auditoria API de relatórios
Este artigo é parte de uma coleção de artigos sobre o Azure Active Directory API do relatório.  
Relatórios do Azure AD fornecem-lhe uma API que permite-lhe aceder a dados de auditoria utilizando código ou ferramentas relacionadas.
O âmbito deste artigo é para lhe fornecer o código de exemplo para o **API de auditoria**.

Veja:

* [Registos de auditoria](active-directory-reporting-azure-portal.md#activity-reports) para obter mais informações concetuais
* [Começar a utilizar a API do Azure Active Directory Reporting](active-directory-reporting-api-getting-started.md) para obter mais informações sobre a API de relatórios.

Para perguntas, problemas ou comentários, entre em contacto com [AAD Reporting ajudar](mailto:aadreportinghelp@microsoft.com).


## <a name="prerequisites"></a>Pré-requisitos
Antes de poder utilizar os exemplos neste artigo, o que precisa para concluir a [pré-requisitos para o Azure AD API do relatório de acesso](active-directory-reporting-api-prerequisites.md).  

## <a name="known-issue"></a>Problema conhecido
Autenticação da aplicação não funcionará se o seu inquilino está na região EU. Utilize autenticação de utilizador para aceder a API de auditoria como solução, até que resolva o problema. 

## <a name="powershell-script"></a>Script do PowerShell


```powershell

# This script will require the Web Application and permissions setup in Azure Active Directory
$clientID       = "<appid>"             # ApplicationId
$clientSecret   = "<key>"         # Should be a ~44 character string insert your info here
$loginURL       = "https://login.windows.net/"
$tenantdomain   = "<domain>"            # For example, contoso.onmicrosoft.com
$msgraphEndpoint = "https://graph.microsoft.com"
$countOfAuditDocsToBeSavedInAFile = 2000
    
# Get an Oauth 2 access token based on client id, secret and tenant domain
$body       = @{grant_type="client_credentials";resource=$msgraphEndpoint;client_id=$clientID;client_secret=$clientSecret}
$oauth      = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body
    
if ($oauth.access_token -ne $null) {
    $headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}
    
    $url = "$msgraphEndpoint/beta/auditLogs/directoryAudits"
    Write-Output "Fetching data using Uri: $url"
    $i=0
    $docCount=0
    Do{
        $myReport = (Invoke-WebRequest -UseBasicParsing -Headers $headerParams -Uri $url)
        $jsonReport = ($myReport.Content | ConvertFrom-Json).value
        $fetchedRecordCount = $jsonReport.Count
        $docCount = $docCount + $fetchedRecordCount
        $totalFetchedRecordCount = $totalFetchedRecordCount + $fetchedRecordCount
        Write-Output "Fetched $fetchedRecordCount records and saved into Audits$i.json"
        if($docCount -le $countOfAuditDocsToBeSavedInAFile)
        {
            $myReport.Content | Out-File -FilePath Audits$i.json -append  -Force        
        }
        else
        {           
            $docCount=0
            $i = $i+1
        }
            
        #Get url from next link
        $url = ($myReport.Content | ConvertFrom-Json).'@odata.nextLink'         
    }while($url -ne $null)
    Write-Output "Total Fetched record count is : $totalFetchedRecordCount"
                
} else {
    Write-Host "ERROR: No Access Token"
}

```

### <a name="executing-the-powershell-script"></a>Executar o script do PowerShell
Depois de concluir a edição do script, executá-lo e certifique-se de que os dados esperados da auditoria os registos de relatório é devolvido.

O script devolve o resultado do relatório de auditoria no formato JSON. Também cria um `Audits.json` ficheiro com o mesmo resultado. Pode experimentar ao modificar o script devolver dados a partir de outros relatórios ou comente os formatos de saída que não é necessário.





## <a name="next-steps"></a>Passos Seguintes
* Gostaria de personalizar os exemplos neste artigo? Veja o [auditoria do Azure Active Directory referência da API](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/directoryaudit). 
* Se pretender ver uma descrição geral completa de utilizar o Azure Active Directory API do relatório, consulte [introdução ao Azure Active Directory API do relatório](active-directory-reporting-api-getting-started.md).
* Se quiser saber mais sobre os relatórios do Azure Active Directory, consulte o [do Azure Active Directory guia de relatórios](active-directory-reporting-guide.md).  

