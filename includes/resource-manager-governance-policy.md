---
title: incluir ficheiro
description: incluir ficheiro
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 02/21/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: c04514218c7ed8dfd72b94345d2deb88e663fda1
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38740593"
---
As [Políticas do Azure](/azure/azure-policy/) ajudam-no a certificar-se de que todos os recursos na subscrição cumprem os padrões empresariais. Utilize políticas para reduzir os custos ao limitar as opções de implementação para apenas esses tipos de recursos e os SKUs que são aprovados. O utilizador define as regras e ações para os seus recursos e essas regras são aplicadas automaticamente durante a implementação. Por exemplo, pode controlar os tipos de recursos que são implementados. Em alternativa, pode restringir as localizações aprovadas para os recursos. Algumas políticas negam uma ação e algumas políticas configuram a auditoria de uma ação.

A política é complementar ao controlo de acesso baseado em funções (RBAC). O RBAC concentra-se no acesso de utilizador e é um sistema de permissão predefinida e recusa explícita. A política concentra-se nas propriedades dos recursos durante e após a implementação. A política é um sistema de permissão predefinida e recusa explícita.

Existem dois conceitos sobre políticas a compreender - *definições de política* e *atribuições de política*. Uma definição de política descreve as condições de gestão que deseja impor. Uma atribuição de política coloca uma definição de política em ação para um determinado âmbito.

![Políticas de atribuição](./media/resource-manager-governance-policy/policy-concepts.png)

O Azure disponibiliza várias definições de política incorporada que pode utilizar sem qualquer modificação. Transmita os valores de parâmetro para especificar os valores que são permitidos no seu âmbito. Se a definição de política incorporada não cumprir os seus requisitos, pode [criar definições de política personalizada](../articles/azure-policy/create-manage-policy.md).
