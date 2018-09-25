---
title: incluir ficheiro
description: incluir ficheiro
services: azure-policy
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 09/18/2018
ms.author: dacoulte
ms.custom: include file
ms.openlocfilehash: 76e161be1adca4aa2ec7b682a13b0a42e4b79412
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "47003730"
---
### <a name="sql-servers"></a>SQL Servers

|  |  |
|---------|---------|
| [Auditar se não existir um administrador do Azure Active Directory](../articles/governance/policy/samples/audit-no-aad-admin.md) | Auditar quando não existe nenhum administrador do Azure Active Directory atribuído ao servidor SQL. |
| [Auditar a definição de deteção de ameaças do nível do Servidor](../articles/governance/policy/samples/audit-sql-ser-threat-det-setting.md) | Faz uma auditoria às políticas de alerta de segurança da base de dados SQL se as políticas não estiverem definidas para o estado especificado. Especifica um valor que indica se a deteção de ameaças está ativada ou desativada.  |
| [Auditar as definições de auditoria do SQL Server](../articles/governance/policy/samples/sql-server-audit.md) | Faz uma auditoria ao SQL Server para verificar se as definições de auditoria estão ativadas. |
| [Auditar a Definição de Auditoria do Nível do SQL Server](../articles/governance/policy/samples/audit-sql-ser-leve-audit-setting.md) | Faz uma auditoria das definições de auditoria do SQL Server se essas definições não corresponderem a uma definição especificada. Especifica um valor que indica se as definições de auditoria devem estar ativadas ou desativadas. |
| [Requer a Versão do SQL Server 12.0](../articles/governance/policy/samples/req-sql-12.md) | Requer que os servidores do SQL utilizem a versão 12.0.  |