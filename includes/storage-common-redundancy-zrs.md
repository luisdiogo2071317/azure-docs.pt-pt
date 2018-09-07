---
title: incluir ficheiro
description: incluir ficheiro
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 07/11/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 4a43966180850645584043690b1be9d6ae232f6e
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/06/2018
ms.locfileid: "44027452"
---
Armazenamento com redundância de zona (ZRS) replica os dados de forma síncrona em três clusters de armazenamento numa única região. Cada cluster de armazenamento é fisicamente separado dos outros e reside na sua própria zona de disponibilidade (AZ). Cada zona de disponibilidade e o cluster ZRS dentro da mesma, é autónomo, com utilitários separados e capacidades de rede.

Armazenar os seus dados numa conta ZRS garante que será capaz de aceder e gerir os seus dados no caso de uma zona fica indisponível. O ZRS fornece excelente desempenho e baixa latência. O ZRS oferece o mesmo [destinos de escalabilidade](../articles/storage/common/storage-scalability-targets.md) como [armazenamento localmente redundante (LRS)](../articles/storage/common/storage-redundancy-lrs.md).

Considere o ZRS para cenários que exigem uma consistência forte, forte durabilidade e elevada disponibilidade, mesmo que uma indisponibilidade ou desastre natural processa um centro de dados zonal indisponível. O ZRS oferece durabilidade para objetos de armazenamento de, pelo menos, 99.9999999999% (12 9 s) ao longo de um ano.

Para obter mais informações sobre as zonas de disponibilidade, consulte [descrição geral das zonas de disponibilidade](https://docs.microsoft.com/azure/availability-zones/az-overview).