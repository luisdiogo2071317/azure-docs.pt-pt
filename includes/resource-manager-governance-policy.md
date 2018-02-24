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
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/23/2018
---
[As políticas do Azure](/azure/azure-policy/) ajudam-se de que todos os recursos na subscrição cumpre os padrões empresariais. Utilize políticas para reduzir os custos, restringindo opções de implementação apenas esses tipos de recursos e os SKUs que forem aprovados. Definir regras e ações para os seus recursos e essas regras são automaticamente aplicadas durante a implementação. Por exemplo, pode controlar os tipos de recursos que estão implementados. Em alternativa, pode restringir as localizações aprovadas para recursos. Uma ação de negação de algumas políticas e algumas políticas configurar a auditoria de uma ação.

A política é complementares para o controlo de acesso baseado em funções (RBAC). RBAC centra-se no acesso do utilizador e é uma predefinição negar e explícita permitir que o sistema. Política concentra-se nas propriedades de recursos durante e após a implementação. Respetiva permitir uma predefinição e explícita negar sistema.

Existem dois conceitos para compreender com políticas - *definições de política* e *atribuições de política*. Uma definição de política descreve as condições de gestão que pretende impor. Uma atribuição de política coloca uma definição de política para a acção para um âmbito específico.

![Atribuir políticas](./media/resource-manager-governance-policy/policy-concepts.png)

O Azure oferece várias definições de política incorporadas que pode utilizar sem qualquer modificação. Passar valores de parâmetro para especificar os valores que são permitidos no seu âmbito. Se a definição de política incorporada não satisfazer os seus requisitos, pode [criar definições de política personalizada](../articles/azure-policy/create-manage-policy.md).
