---
title: "Relatórios de acesso e utilização de MFA do Azure | Microsoft Docs"
description: "Descreve como utilizar a funcionalidade de multi-factor Authentication do Azure - relatórios."
services: multi-factor-authentication
documentationcenter: 
author: MicrosoftGuyJFlo
manager: mtillman
ms.assetid: 3f6b33c4-04c8-47d4-aecb-aa39a61c4189
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: joflore
ms.reviewer: richagi
ms.openlocfilehash: fb83e957a206bff29132973d2dd3e9a7b5f9f060
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/18/2017
---
# <a name="reports-in-azure-multi-factor-authentication"></a>Relatórios do multi-factor Authentication do Azure

Multi-factor Authentication do Azure fornece diversos relatórios que podem ser utilizados pelo utilizador e a sua organização acessível através do portal do Azure. A tabela seguinte lista os relatórios disponíveis:

| Relatório | Localização | Descrição |
|:--- |:--- |:--- |
| Histórico de Utilizador Bloqueado | Azure AD > servidor MFA > bloquear/desbloquear utilizadores | Mostra o histórico de pedidos de bloqueio ou desbloqueio de utilizadores. |
| Alertas de utilização e de fraude | Azure AD > inícios de sessão | Fornece informações sobre a utilização global, utilizador resumo e detalhes de utilizador bem como um histórico de alertas de fraude apresentados durante o intervalo de datas especificado. |
| Histórico de Utilizador Ignorado | Azure AD > servidor MFA > omissão de uso individual | Disponibiliza um histórico de pedidos para ignorar o multi-factor Authentication para um utilizador. |
| Estado do servidor | Azure AD > servidor MFA > Estado do servidor | Apresenta o estado dos servidores multi-factor Authentication associado à conta. |

## <a name="view-reports"></a>Ver relatórios 

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. No lado esquerdo, selecione **do Azure Active Directory** > **servidor MFA**.
3. Selecione o relatório que pretende ver.

   <center>![Nuvem](./media/multi-factor-authentication-manage-reports/report.png)</center>

## <a name="powershell-reporting"></a>Relatórios de PowerShell

Identifica os utilizadores se tem registado para MFA através do PowerShell que se segue.

```Get-MsolUser -All | where {$_.StrongAuthenticationMethods -ne $null} | Select-Object -Property UserPrincipalName```

Identifique os utilizadores que não tenham registado para MFA através do PowerShell que se segue.

```Get-MsolUser -All | where {$_.StrongAuthenticationMethods.Count -eq 0} | Select-Object -Property UserPrincipalName```

## <a name="next-steps"></a>Passos Seguintes

* [Para os utilizadores](end-user/multi-factor-authentication-end-user.md)
* [Onde pretende implementar](multi-factor-authentication-get-started.md)
