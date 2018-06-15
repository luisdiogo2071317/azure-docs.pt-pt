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
ms.openlocfilehash: 4f5dbbd1da8a221fb5f8b9641e016829987db538
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/23/2018
ms.locfileid: "29529962"
---
Antes de criar quaisquer itens, vamos rever o conceito de âmbito. O Azure oferece quatro níveis de gestão: recurso, grupo de recursos, subscrição e grupos de gestão. [Grupos de gestão](../articles/billing/billing-enterprise-mgmt-group-overview.md) são uma versão de pré-visualização. A imagem seguinte mostra um exemplo destas camadas.

![Âmbito](./media/resource-manager-governance-scope/scope-levels.png)

Aplicar definições de gestão em qualquer um destes níveis de âmbito. O nível que selecionar determina como amplamente a definição é aplicada. Níveis inferiores herdam as definições de níveis superiores. Ao aplicar uma definição para a subscrição, essa definição será aplicada a todos os grupos de recursos e recursos na sua subscrição. Ao aplicar uma definição no grupo de recursos, o que é aplicado o grupo de recursos e todos os respetivos recursos. No entanto, a outro grupo de recursos não tem essa definição.

Normalmente, faz sentido para aplicar definições de crítico em níveis superiores e requisitos específicos do projeto em níveis inferiores. Por exemplo, poderá pretender certificar-se de que todos os recursos para a sua organização são implementados em determinados regiões. Concretizar este requisito, aplicar uma política para a subscrição que especifica localizações permitidas. Como outros utilizadores na sua organização adicionam novos grupos de recursos e recursos, localizações permitidas automaticamente são impostas. 
