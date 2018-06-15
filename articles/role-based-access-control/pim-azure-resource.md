---
title: Gerir o acesso aos recursos do Azure com o Privileged Identity Management (PIM)
description: Saiba mais sobre como utilizar a gestão de acesso baseado em funções no PIM para aceder aos recursos do Azure.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: skwan
ms.assetid: ba06b8dd-4a74-4bda-87c7-8a8583e6fd14
ms.service: role-based-access-control
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/30/2018
ms.author: rolyon
ms.reviewer: skwan
ms.openlocfilehash: fb0a1ff3821efd7114b509b72e143d5240b61b4c
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
ms.locfileid: "34204214"
---
# <a name="manage-access-to-azure-resources-with-privileged-identity-management"></a>Gerir o acesso aos recursos do Azure Privileged Identity Management

Para proteger contas com privilégios de informático-ataques maliciosos, pode utilizar o Azure Active Directory Privileged Identity gestão (PIM) para reduzir o tempo de exposição de privilégios e aumentar a visibilidade para a sua utilização através de relatórios e alertas. PIM efetua este procedimento ao limitar os utilizadores apenas colocar nos seus privilégios "just in time" (JIT), ou atribuindo privilégios durante um período reduzido após o qual os privilégios são revogados automaticamente. 

Agora pode utilizar PIM com controlo de acesso em funções do Azure (RBAC) para gerir, controlar e monitorizar o acesso aos recursos do Azure. PIM pode gerir a associação das funções incorporadas e personalizadas para ajudá-lo: 

- Ativar a pedido, "just in time" acesso a recursos do Azure
- Expirar o acesso a recursos automaticamente para os utilizadores atribuídos e grupos
- Atribuir acesso temporário aos recursos do Azure para tarefas rápidas ou de agendas na chamada
- Obtenha alertas quando os novos utilizadores ou grupos atribuídos acesso a recursos e quando que ativam atribuições elegíveis

Para obter mais informações, consulte [Overview of Role-Based o controlo de acesso no Azure PIM](../active-directory/privileged-identity-management/azure-pim-resource-rbac.md).