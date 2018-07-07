---
title: Resolver erros no Azure Active Directory reporting API | Documentos da Microsoft
description: Fornece uma resolução de erros ao chamar APIs do Azure Active Directory Reporting.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: 0030c5a4-16f0-46f4-ad30-782e7fea7e40
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: compliance-reports
ms.date: 01/15/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: a05cb2325dc193b9d96e5d454cb2d934aa926a02
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/07/2018
ms.locfileid: "37908022"
---
# <a name="troubleshoot-errors-in-azure-active-directory-reporting-api"></a>Resolver erros no Azure Active Directory reporting API

Este artigo apresenta uma lista de mensagens de erro comuns que poderão ocorrer em ao aceder a relatórios de atividade com o MS Graph API e os passos para a respetiva resolução.

### <a name="500-http-internal-server-error-while-accessing-microsoft-graph-v2-endpoint"></a>500 Erro de servidor interno de HTTP ao aceder ao ponto final V2 do Microsoft Graph

Nós não suportam atualmente o ponto de final do Microsoft Graph v2 - Certifique-se acessar os registos de atividades com o ponto de final do Microsoft Graph v1.

### <a name="error-failed-to-get-user-roles-from-ad-graph"></a>Erro: Falha ao obter funções de utilizador do AD Graph

Poderá receber esta mensagem de erro ao tentar aceder a inícios de sessão com o Explorador do gráfico. Certifique-se de que tem sessão iniciada sua conta com ambos os botões de início de sessão na IU do Explorador do gráfico, como mostrado na imagem seguinte. 

![API do Graph](./media/active-directory-reporting-troubleshoot-graph-api/graph-explorer.png)

### <a name="error-failed-to-do-premium-license-check-from-ad-graph"></a>Erro: Falha ao fazer a verificação de licença de premium partir do AD Graph 

Caso se depare com esta mensagem de erro ao tentar aceder a inícios de sessão com a API do Graph, escolha **modificar permissões** sob a sua conta na barra de navegação à esquerda e selecione **Tasks.ReadWrite** e **Directory.Read.All**. 

![Modificar as permissões da interface do Usuário](./media/active-directory-reporting-troubleshoot-graph-api/modify-permissions.png)


### <a name="error-neither-tenant-is-b2c-or-tenant-doesnt-have-premium-license"></a>Erro: Não é o inquilino B2C ou inquilino não tem licença premium

Aceder a relatórios de início de sessão requer um Azure Active Directory premium (P1) de 1 licença. Se vir esta mensagem de erro ao aceder a inícios de sessão, certifique-se de que o seu inquilino está licenciado com uma licença do Azure AD P1.

### <a name="error-user-is-not-in-the-allowed-roles"></a>Erro: Utilizador não está nas funções permitidas 

Se vir esta mensagem de erro ao tentar aceder a registos de auditoria ou inícios de sessão com a API, certifique-se de que a sua conta fizer parte do **leitor de segurança** ou **leitor de relatório** função no Azure Active Directory inquilino. 

### <a name="error-application-missing-aad-read-directory-data-permission"></a>Erro: Aplicação falta a permissão de "Read directory data" do AAD 

Siga os passos a [pré-requisitos para aceder ao Azure Active Directory reporting API](active-directory-reporting-api-prerequisites-azure-portal.md) para garantir que seu aplicativo seja executado com o conjunto correto de permissões. 

### <a name="error-application-missing-msgraph-api-read-all-audit-log-data-permission"></a>Erro: Aplicação falta a permissão "Ler todos os dados de registo de auditoria" de API de MSGraph

Siga os passos a [pré-requisitos para aceder ao Azure Active Directory reporting API](active-directory-reporting-api-prerequisites-azure-portal.md) para garantir que seu aplicativo seja executado com o conjunto correto de permissões. 

## <a name="next-steps"></a>Próximos Passos

[Utilizar a referência da API de auditoria](https://developer.microsoft.com/en-us/graph/docs/api-reference/beta/resources/directoryaudit)
[utilize a referência de API do relatório de atividade de início de sessão](https://developer.microsoft.com/en-us/graph/docs/api-reference/beta/resources/signin)