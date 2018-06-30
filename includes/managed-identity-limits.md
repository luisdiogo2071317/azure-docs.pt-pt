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
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37116586"
---
| Categoria | Limite |
| --- | --- |
| Utilizador atribuído identidades geridas | <ul><li>Quando criar utilizador atribuído identidades geridas, apenas carateres alfanuméricos (0-9, a-z, A-Z) e o hífen (-) são suportados. Além disso, o nome deve ser limitado a 24 carateres de comprimento para a atribuição a VM/VMSS funcione corretamente.</li><li>Se utilizar a extensão de máquina virtual de identidade gerido, o limite suportado é 32 utilizador atribuído identidades geridas.  Sem a extensão de máquina virtual de identidade gerido, o limite suportado é 512 utilizador atribuído identidades.</li>|

