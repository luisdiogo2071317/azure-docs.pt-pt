---
title: incluir ficheiro
description: incluir ficheiro
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: include
ms.date: 09/12/2018
ms.author: raynew
ms.openlocfilehash: ae8eebf9667f2bc03fdc1082fb38c19c5c645c10
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "47060880"
---
**Nome** | **URL comercial**  | **URL de Governo** | **Descrição** |
--- | --- | --- | ---
Azure AD | ``login.microsoftonline.com`` | ``login.microsoftonline.us`` | Utilizado para gestão de identidades e controlo de acesso através do AAD 
Cópia de segurança | ``*.backup.windowsazure.com`` | ``*.backup.windowsazure.us`` | Utilizado para transferência de dados de replicação e coordenação 
Replicação | ``*.hypervrecoverymanager.windowsazure.com`` | ``*.hypervrecoverymanager.windowsazure.us``  | Utilizado para operações de gestão de replicação e coordenação 
Armazenamento | ``*.blob.core.windows.net`` | ``*.blob.core.usgovcloudapi.net``  | Utilizado para acesso à conta de armazenamento que armazena dados replicados 
Telemetria (opcional) | ``dc.services.visualstudio.com`` | ``dc.services.visualstudio.com`` | Utilizado para a telemetria 
Sincronização de hora | ``time.windows.com`` | ``time.nist.gov`` | Ssed para verificar a sincronização de hora entre o sistema e a hora global em todas as implementações.


