---
title: Exemplos de API de auditoria de relatórios do Azure Active Directory | Microsoft Docs
description: Como começar a utilizar a API de Relatórios do Azure Active Directory
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: de8b8ec3-49b3-4aa8-93fb-e38f52c99743
ms.service: active-directory
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: compliance-reports
ms.date: 05/30/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 240095b64f4b7b37a71f84ce8d7a3521e0ab7c03
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36223890"
---
# <a name="azure-active-directory-reporting-audit-api-samples"></a>Exemplos de API de auditoria de relatórios do Azure Active Directory
Este artigo pertence a uma coleção de artigos sobre a API do relatório do Azure Active Directory.  
Os relatórios do Azure AD disponibilizam uma API que lhe permite aceder a dados de auditoria através de código ou ferramentas relacionadas.
O âmbito deste artigo é disponibilizar código de exemplo para a **API de auditoria**.

Veja:

* [Registos de auditoria](active-directory-reporting-azure-portal.md#activity-reports) para obter mais informações concetuais
* Veja a [Introdução à API de relatórios do Azure Active Directory](active-directory-reporting-api-getting-started.md) para obter mais informações sobre a API de relatórios.

Para esclarecer dúvidas, resolver problemas ou escrever comentários, entre em contacto com a [Ajuda de Relatórios do AAD](mailto:aadreportinghelp@microsoft.com).


## <a name="prerequisites"></a>Pré-requisitos
Antes de poder utilizar os exemplos deste artigo, precisa de concluir os [pré-requisitos para aceder à API dos relatórios do Azure AD](active-directory-reporting-api-prerequisites.md).  

## <a name="known-issue"></a>Problema conhecido
A Autenticação da Aplicação não funciona se o seu inquilino estiver na região da UE. Utilize a Autenticação de Utilizador para aceder à API de Auditoria como uma solução alternativa, até resolver o problema. 

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
Depois de concluir a edição do script, execute-o e certifique-se de que os dados esperados do relatório de registos de Auditoria é devolvido.

O script devolve o resultado do relatório de auditoria no formato JSON. Também cria um ficheiro `Audits.json` com o mesmo resultado. Pode experimentar ao modificar o script para devolver dados a partir de outros relatórios e comentar os formatos de saída que não precisa.





## <a name="next-steps"></a>Passos seguintes
* Gostaria de personalizar os exemplos neste artigo? Veja a [Referência da API de auditoria do Azure Active Directory](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/directoryaudit). 
* Se pretender ver uma descrição geral completa da utilização da API de relatórios do Azure Active Directory, veja [Introdução à API de relatórios do Azure Active Directory](active-directory-reporting-api-getting-started.md).
* Se gostaria de saber mais sobre os relatórios do Azure Active Directory, veja o [Guia dos Relatórios do Azure Active Directory](active-directory-reporting-guide.md).  

