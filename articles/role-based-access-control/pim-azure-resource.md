---
title: Gerir o acesso aos recursos do Azure com o Privileged Identity Management (PIM)
description: Saiba mais sobre como gerir o acesso aos recursos do Azure com o Privileged Identity Management (PIM) e o controlo de acesso baseado em funções (RBAC).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: skwan
ms.assetid: ba06b8dd-4a74-4bda-87c7-8a8583e6fd14
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/30/2018
ms.author: rolyon
ms.reviewer: skwan
ms.openlocfilehash: 141cba29f5027ce092775d97c1abe9ecf11badf5
ms.sourcegitcommit: e0834ad0bad38f4fb007053a472bde918d69f6cb
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37436050"
---
# <a name="manage-access-to-azure-resources-with-privileged-identity-management"></a>Gerir o acesso aos recursos do Azure Privileged Identity Management

Para proteger contas com privilégios de ataques de cibersegurança maliciosos, pode utilizar o Azure Active Directory Privileged Identity Management (PIM) para reduzir o tempo de exposição dos privilégios e aumentar a visibilidade sobre seu uso por meio de relatórios e alertas. PIM faz isso ao limitar os utilizadores apenas levando em seus privilégios "just-in-time" (JIT), ou ao atribuir privilégios para uma duração abreviada após o qual os privilégios são revogados automaticamente. 

Agora pode utilizar o PIM com controlo de acesso baseado em função do Azure (RBAC) para gerir, controlar e monitorizar o acesso aos recursos do Azure. PIM pode gerir a associação de funções incorporadas e personalizadas para ajudá-lo a: 

- Ativar a pedido, "just-in-time" acesso aos recursos do Azure
- Expirar acesso a recursos automaticamente para utilizadores e grupos atribuídos
- Atribuir acesso temporário aos recursos do Azure para tarefas rápidas ou na chamada agendas
- Obtenha alertas quando os novos utilizadores ou grupos são atribuídos acesso a recursos e quando ativam atribuições elegíveis

Para obter mais informações, consulte [descrição geral do controlo de acesso baseado em funções no PIM do Azure](../active-directory/privileged-identity-management/azure-pim-resource-rbac.md).