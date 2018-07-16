---
title: Relatórios de acesso e utilização do MFA do Azure | Documentos da Microsoft
description: Descreve como utilizar a funcionalidade de multi-factor Authentication - relatórios.
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 12/15/2017
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: richagi
ms.openlocfilehash: 146a86058adc73626e532f33e9fdbc83d9cf27e8
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39048995"
---
# <a name="reports-in-azure-multi-factor-authentication"></a>Relatórios no Azure multi-factor Authentication

O Azure multi-factor Authentication fornece diversos relatórios que podem ser utilizados por e sua organização acessível através do portal do Azure. A tabela seguinte lista os relatórios disponíveis:

| Relatório | Localização | Descrição |
|:--- |:--- |:--- |
| Histórico de Utilizador Bloqueado | O Azure AD > servidor MFA > bloquear/desbloquear utilizadores | Mostra o histórico de pedidos para bloquear ou desbloquear utilizadores. |
| Alertas de fraude e de utilização | O Azure AD > inícios de sessão | Fornece informações sobre a utilização global, resumo por utilizador e detalhes de utilizador como um histórico de alertas de fraude apresentados durante o intervalo de datas especificado. |
| Utilização de componentes no local | O Azure AD > servidor MFA > relatório de atividade | Fornece informações sobre a utilização global da MFA através da extensão NPS, AD FS e o servidor MFA. |
| Histórico de Utilizador Ignorado | O Azure AD > servidor MFA > omissão de uso individual | Fornece um histórico de pedidos para ignorar o multi-factor Authentication para um utilizador. |
| Estado do servidor | O Azure AD > servidor MFA > Estado do servidor | Apresenta que o estado dos servidores multi-factor Authentication associado à sua conta. |

## <a name="view-reports"></a>Ver relatórios 

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. No lado esquerdo, selecione **do Azure Active Directory** > **servidor MFA**.
3. Selecione o relatório que pretende ver.

   <center>![Nuvem](./media/howto-mfa-reporting/report.png)</center>

## <a name="powershell-reporting"></a>Relatórios do PowerShell

Identifique os utilizadores que foram registrados para MFA com o PowerShell que se segue.

```Get-MsolUser -All | where {$_.StrongAuthenticationMethods -ne $null} | Select-Object -Property UserPrincipalName```

Identificar os utilizadores que não se registou para a MFA com o PowerShell que se segue.

```Get-MsolUser -All | where {$_.StrongAuthenticationMethods.Count -eq 0} | Select-Object -Property UserPrincipalName```

## <a name="next-steps"></a>Passos Seguintes

* [Para os utilizadores](../user-help/multi-factor-authentication-end-user.md)
* [Onde pretende implementar](concept-mfa-whichversion.md)
