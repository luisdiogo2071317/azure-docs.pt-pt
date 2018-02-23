---
title: incluir ficheiro
description: incluir ficheiro
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 02/16/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 7a0e7e19d53d8566b859004c363ec5bbc71cb733
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/21/2018
---
Antes de criar quaisquer itens, vamos rever o conceito de âmbito. O Azure oferece quatro níveis de gestão: recurso, grupo de recursos, subscrição e grupos de gestão. [Grupos de gestão](../articles/billing/billing-enterprise-mgmt-group-overview.md) são uma versão de pré-visualização. A imagem seguinte mostra um exemplo destas camadas.

![Âmbito](./media/resource-manager-governance-scope/scope-levels.png)

Aplicar definições de gestão em qualquer um destes níveis de âmbito. O nível que selecionar determina como amplamente a definição é aplicada. Níveis inferiores herdam as definições de níveis superiores. Ao aplicar uma definição para a subscrição, essa definição será aplicada a todos os grupos de recursos e recursos na sua subscrição. Ao aplicar uma definição no grupo de recursos, o que é aplicado o grupo de recursos e todos os respetivos recursos. No entanto, a outro grupo de recursos não tem essa definição.

Normalmente, faz sentido para aplicar definições de crítico em níveis superiores e requisitos específicos do projeto em níveis inferiores. Por exemplo, poderá pretender certificar-se de que todos os recursos para a sua organização são implementados em determinados regiões. Concretizar este requisito, aplicar uma política para a subscrição que especifica localizações permitidas. Como outros utilizadores na sua organização adicionam novos grupos de recursos e recursos, localizações permitidas automaticamente são impostas. Para um projeto específico, pode querer certificar-se de que todos os recursos são marcados com uma etiqueta, que identifica o projeto. Aplica uma política para o grupo de recursos para o projeto que impõe esta etiqueta. Como os utilizadores adicionam novos recursos ao grupo de recursos, a etiqueta é adicionada automaticamente. No entanto, os utilizadores podem adicionar grupos de recursos que não estão relacionadas com o projeto e a etiqueta não é imposta.
