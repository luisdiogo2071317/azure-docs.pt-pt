---
title: incluir ficheiro
description: incluir ficheiro
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: include
ms.date: 06/28/2018
ms.author: raynew
ms.openlocfilehash: f7d6c3f68618fec839ccff06b73ba44d106999d2
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38765543"
---
| Nome | URL comercial | URL de Governo | Descrição |
|---|---|---|---|
| Azure AD | ``login.microsoftonline.com`` | ``login.microsoftonline.us`` | Utilizado para gestão de identidades e controlo de acesso através do AAD |
| Cópia de segurança | ``*.backup.windowsazure.com`` | ``*.backup.windowsazure.us`` | Utilizado para transferência de dados de replicação e coordenação |
| Replicação | ``*.hypervrecoverymanager.windowsazure.com`` | ``*.hypervrecoverymanager.windowsazure.us``  | Utilizado para operações de gestão de replicação e coordenação |
| Armazenamento | ``*.blob.core.windows.net`` | ``*.blob.core.usgovcloudapi.net``  | Utilizado para acesso à conta de armazenamento que armazena dados replicados |
| Telemetria (opcional) | ``dc.services.visualstudio.com`` | ``dc.services.visualstudio.com`` | Utilizado para a telemetria |

``time.nist.gov`` e ``time.windows.com`` são utilizados para verificar a sincronização de hora entre o sistema e a hora global em todas as implementações.


