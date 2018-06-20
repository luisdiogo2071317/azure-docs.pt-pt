---
title: incluir ficheiro
description: incluir ficheiro
services: azure-policy
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 05/17/2018
ms.author: dacoulte
ms.custom: include file
ms.openlocfilehash: 298b92205e73b3623db02fb1dd803edac8379700
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34664705"
---
### <a name="sql-servers"></a>SQL Servers

|  |  |
|---------|---------|
| [Auditar se não existir um administrador do Azure Active Directory](../articles/azure-policy/scripts/audit-no-aad-admin.md) | Auditar quando não existe nenhum administrador do Azure Active Directory atribuído ao servidor SQL. |
| [Auditar a definição de deteção de ameaças do nível do Servidor](../articles/azure-policy/scripts/audit-sql-ser-threat-det-setting.md) | Faz uma auditoria às políticas de alerta de segurança da base de dados SQL se as políticas não estiverem definidas para o estado especificado. Especifica um valor que indica se a deteção de ameaças está ativada ou desativada.  |
| [Auditar as definições de auditoria do SQL Server](../articles/azure-policy/scripts/sql-server-audit.md) | Faz uma auditoria ao SQL Server para verificar se as definições de auditoria estão ativadas. |
| [Auditar a Definição de Auditoria do Nível do SQL Server](../articles/azure-policy/scripts/audit-sql-ser-leve-audit-setting.md) | Faz uma auditoria das definições de auditoria do SQL Server se essas definições não corresponderem a uma definição especificada. Especifica um valor que indica se as definições de auditoria devem estar ativadas ou desativadas. |
| [Requer a Versão do SQL Server 12.0](../articles/azure-policy/scripts/req-sql-12.md) | Requer que os servidores do SQL utilizem a versão 12.0.  |