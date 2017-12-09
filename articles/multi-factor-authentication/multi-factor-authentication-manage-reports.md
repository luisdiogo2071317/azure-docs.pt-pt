---
title: "Relatórios de acesso e utilização de MFA do Azure | Microsoft Docs"
description: "Descreve como utilizar a funcionalidade de multi-factor Authentication do Azure - relatórios."
services: multi-factor-authentication
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
editor: curtand
ms.assetid: 3f6b33c4-04c8-47d4-aecb-aa39a61c4189
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/03/2017
ms.author: joflore
ms.reviewer: richagi
ms.openlocfilehash: 76a13467fff23ad62a857a53e0e31865b1a9fe81
ms.sourcegitcommit: 094061b19b0a707eace42ae47f39d7a666364d58
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2017
---
# <a name="reports-in-azure-multi-factor-authentication"></a>Relatórios do multi-factor Authentication do Azure

Multi-factor Authentication do Azure fornece diversos relatórios que podem ser utilizados pelo utilizador e a sua organização. Estes relatórios podem ser acedidos através do Portal de gestão do multi-factor Authentication. A tabela seguinte lista os relatórios disponíveis:

| Relatório | Descrição |
|:--- |:--- |
| Utilização |A utilização de relatórios de informações de apresentação em utilização global, resumo do utilizador e detalhes de utilizador. |
| Estado do Servidor |Este relatório apresenta o estado dos servidores de autenticação Multifator associado à sua conta. |
| Histórico de Utilizador Bloqueado |Estes relatórios mostram o histórico de pedidos de bloqueio ou desbloqueio de utilizadores. |
| Histórico de Utilizador Ignorado |Mostra o histórico de pedidos para ignorar o multi-factor Authentication para o número de telefone de um utilizador. |
| Alerta de Fraude |Mostra um histórico de alertas de fraude apresentados durante o intervalo de datas especificado. |
| Em Fila |Apresenta uma lista de relatórios em fila para processamento e o respetivo estado. Quando o relatório estiver concluído, é fornecida uma ligação para transferir ou visualizar o relatório. |

## <a name="view-reports"></a>Ver relatórios

1. Inicie sessão no [Portal do Azure](https://portal.azure.com).
2. No lado esquerdo, selecione **do Azure Active Directory** > **utilizadores e grupos** > **todos os utilizadores** > **Multifator Autenticação**.
3. Em **autenticação multifator**, selecione **definições do serviço**. Na parte inferior em **gerir definições e ver relatórios avançadas**, selecione **aceda ao portal do**.
4. No Portal de gestão do Azure multi-factor Authentication, selecione o tipo de relatório pretendido no **visualizar um relatório** secção no painel de navegação esquerdo.

<center>![Nuvem](./media/multi-factor-authentication-manage-reports/report.png)</center>

## <a name="powershell-reporting"></a>Relatórios de PowerShell

Identifica os utilizadores se tem registado para MFA através do PowerShell que se segue.

```Get-MsolUser -All | where {$_.StrongAuthenticationMethods -ne $null} | Select-Object -Property UserPrincipalName```

Identifique os utilizadores que não tenham registado para MFA através do PowerShell que se segue.

```Get-MsolUser -All | where {$_.StrongAuthenticationMethods.Count -eq 0} | Select-Object -Property UserPrincipalName```

**Recursos Adicionais**

* [Para os utilizadores](end-user/multi-factor-authentication-end-user.md)
* [Azure multi-factor Authentication no MSDN](https://msdn.microsoft.com/library/azure/dn249471.aspx)
