---
title: Exemplos da API do relatório de atividade de início de sessão do Azure Active Directory | Microsoft Docs
description: Como começar a utilizar a API de Relatórios do Azure Active Directory
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: c41c1489-726b-4d3f-81d6-83beb932df9c
ms.service: active-directory
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: compliance-reports
ms.date: 05/31/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 52d49770014a5fb6a5eec644868e702c8a8d9ef3
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36224903"
---
# <a name="azure-active-directory-sign-in-activity-report-api-samples"></a>Exemplos da API do relatório de atividade de início de sessão do Azure Active Directory
Este artigo pertence a uma coleção de artigos sobre a API do relatório do Azure Active Directory.  
Os relatórios do Azure AD disponibilizam uma API que lhe permite aceder a dados de atividade de início de sessão através de código ou ferramentas relacionadas.  
O âmbito deste Artigo é disponibilizar código de exemplo para a **API de atividade de início de sessão**.

Veja:

* [Registos de auditoria](active-directory-reporting-azure-portal.md#activity-reports) para obter mais informações concetuais
* Veja a [Introdução à API de relatórios do Azure Active Directory](active-directory-reporting-api-getting-started.md) para obter mais informações sobre a API de relatórios.


## <a name="prerequisites"></a>Pré-requisitos
Antes de poder utilizar os exemplos deste artigo, precisa de concluir os [pré-requisitos para aceder à API dos relatórios do Azure AD](active-directory-reporting-api-prerequisites.md).  

## <a name="powershell-script"></a>Script do PowerShell

```powershell

# This script will require the Web Application and permissions setup in Azure Active Directory
$clientID       = "<appid>"             # ApplicationId
$clientSecret   = "<key>"         # Should be a ~44 character string insert your info here
$loginURL       = "https://login.windows.net/"
$tenantdomain   = "<domain>"            # For example, contoso.onmicrosoft.com
$msgraphEndpoint = "https://graph.microsoft.com"
$countOfSignInDocsToBeSavedInAFile = 2000
    
# Get an Oauth 2 access token based on client id, secret and tenant domain
$body       = @{grant_type="client_credentials";resource=$msgraphEndpoint;client_id=$clientID;client_secret=$clientSecret}
$oauth      = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body
    
if ($oauth.access_token -ne $null) {
    $headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}
    
    $url = "$msgraphEndpoint/beta/auditLogs/signIns"
    Write-Output "Fetching data using Uri: $url"
    $i=0
    $docCount=0
    Do{
        $myReport = (Invoke-WebRequest -UseBasicParsing -Headers $headerParams -Uri $url)
        $jsonReport = ($myReport.Content | ConvertFrom-Json).value
        $fetchedRecordCount = $jsonReport.Count
        $docCount = $docCount + $fetchedRecordCount
        $totalFetchedRecordCount = $totalFetchedRecordCount + $fetchedRecordCount
        Write-Output "Fetched $fetchedRecordCount records and saved into SignIns$i.json"
        if($docCount -le $countOfSignInDocsToBeSavedInAFile)
        {
            $myReport.Content | Out-File -FilePath SignIns$i.json -append  -Force       
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




## <a name="executing-the-script"></a>Executar o script
Depois de concluir a edição do script, execute-o e certifique-se de que os dados esperados do relatório de registos de início de sessão são devolvidos.

O script devolve o resultado do relatório início de sessão no formato JSON. Também cria um ficheiro `SignIns.json` com o mesmo resultado. Pode experimentar ao modificar o script para devolver dados a partir de outros relatórios e comentar os formatos de saída que não precisa.

## <a name="next-steps"></a>Passos Seguintes
* Gostaria de personalizar os exemplos neste artigo? Veja a [Referência da API de atividade de início de sessão do Azure Active Directory](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/signin). 
* Se pretender ver uma descrição geral completa da utilização da API de relatórios do Azure Active Directory, veja [Introdução à API de relatórios do Azure Active Directory](active-directory-reporting-api-getting-started.md).
* Se gostaria de saber mais sobre os relatórios do Azure Active Directory, veja o [Guia dos Relatórios do Azure Active Directory](active-directory-reporting-guide.md).  

