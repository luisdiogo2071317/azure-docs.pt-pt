---
title: Relatórios de acesso e utilização de MFA do Azure | Microsoft Docs
description: Descreve como utilizar a funcionalidade de multi-factor Authentication do Azure - relatórios.
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 12/15/2017
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: richagi
ms.openlocfilehash: a3e7390e0df707c4898ad9573baa96b567499de1
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/08/2018
ms.locfileid: "33866613"
---
# <a name="reports-in-azure-multi-factor-authentication"></a>Relatórios do multi-factor Authentication do Azure

Multi-factor Authentication do Azure fornece diversos relatórios que podem ser utilizados pelo utilizador e a sua organização acessível através do portal do Azure. A tabela seguinte lista os relatórios disponíveis:

| Relatório | Localização | Descrição |
|:--- |:--- |:--- |
| Histórico de Utilizador Bloqueado | Azure AD > servidor MFA > bloquear/desbloquear utilizadores | Mostra o histórico de pedidos de bloqueio ou desbloqueio de utilizadores. |
| Alertas de utilização e de fraude | Azure AD > inícios de sessão | Fornece informações sobre a utilização global, utilizador resumo e detalhes de utilizador bem como um histórico de alertas de fraude apresentados durante o intervalo de datas especificado. |
| Utilização de componentes no local | Azure AD > servidor MFA > relatório de atividade | Fornece informações sobre a utilização global para a MFA através da extensão NPS, AD FS e o servidor MFA. |
| Histórico de Utilizador Ignorado | Azure AD > servidor MFA > omissão de uso individual | Disponibiliza um histórico de pedidos para ignorar o multi-factor Authentication para um utilizador. |
| Estado do servidor | Azure AD > servidor MFA > Estado do servidor | Apresenta o estado dos servidores multi-factor Authentication associado à conta. |

## <a name="view-reports"></a>Ver relatórios 

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. No lado esquerdo, selecione **do Azure Active Directory** > **servidor MFA**.
3. Selecione o relatório que pretende ver.

   <center>![Nuvem](./media/howto-mfa-reporting/report.png)</center>

## <a name="powershell-reporting"></a>Relatórios de PowerShell

Identifica os utilizadores se tem registado para MFA através do PowerShell que se segue.

```Get-MsolUser -All | where {$_.StrongAuthenticationMethods -ne $null} | Select-Object -Property UserPrincipalName```

Identifique os utilizadores que não tenham registado para MFA através do PowerShell que se segue.

```Get-MsolUser -All | where {$_.StrongAuthenticationMethods.Count -eq 0} | Select-Object -Property UserPrincipalName```

## <a name="next-steps"></a>Passos Seguintes

* [Para os utilizadores](../../multi-factor-authentication/end-user/multi-factor-authentication-end-user.md)
* [Onde pretende implementar](concept-mfa-whichversion.md)
