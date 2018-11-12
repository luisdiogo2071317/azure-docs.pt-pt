---
title: incluir ficheiro
description: incluir ficheiro
services: active-directory
author: daveba
ms.service: active-directory
ms.component: msi
ms.topic: include
ms.date: 05/31/2018
ms.author: daveba
ms.custom: include file
ms.openlocfilehash: 55814f5515649e0fc6d646d3580f281fd9ec37a5
ms.sourcegitcommit: f0c2758fb8ccfaba76ce0b17833ca019a8a09d46
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/06/2018
ms.locfileid: "51191426"
---
| Categoria | Limite |
| --- | --- |
| Identidades geridas atribuídas ao utilizador | <ul><li>Quando criar o utilizador atribuído identidades geridas, apenas carateres alfanuméricos (0 a 9, a-z, A-Z) e o hífen (-) são suportados. Além disso, o nome deve ser limitado a 24 carateres de comprimento para a atribuição de VM/VMSS funcionar corretamente.</li><li>Se utilizar a extensão de máquina virtual de identidade gerida, o limite suportado é atribuída de identidades geridas ao utilizador 32.  Sem a extensão de máquina virtual de identidade gerida, o limite suportado é 512 utilizador identidades atribuídas.</li>|

