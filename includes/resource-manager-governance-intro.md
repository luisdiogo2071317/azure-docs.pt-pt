---
title: incluir ficheiro
description: incluir ficheiro
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 02/19/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 2c16e82ccf259a4cc5ae8fcf35b2dd6b5d50ee2d
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/23/2018
---
Quando implementar recursos do Azure, tem imenso flexibilidade ao decidir que tipos de recursos para implementar, onde estão localizados e como configurá-los. No entanto, essa flexibilidade poderá abrir mais opções de que pretende permitir na sua organização. Tenha em conta a implementar recursos do Azure, poderá ser perguntar-se:

* Como posso satisfazer requisitos legais para soberania de dados em determinados países?
* Como posso controlar os custos?
* Como garantir que alguém não inadvertidamente, alterar um sistema crítico?
* Como controlar os custos de recursos e faturá-lo com precisão?

Este artigo aborda as perguntas. Especificamente, pode:

> [!div class="checklist"]
* Atribuir utilizadores a funções e atribua as funções para um âmbito, pelo que os utilizadores têm permissão para efetuar ações esperadas, mas não mais ações.
* Aplica políticas recomende uma convenções de recursos na sua subscrição.
* Bloquear recursos que são críticos para o sistema.
* Etiquetar recursos, pelo que pode ser controlar pelos valores que façam sentido para a sua organização.

Este artigo incida nas tarefas que efetuar para implementar a governação. Para ver um debate mais amplo dos conceitos, consulte [governação no Azure](../articles/security/governance-in-azure.md). 
