---
title: Atualização do sistema operativo para o SAP HANA no Azure (instâncias de grande) | Microsoft Docs
description: Efetuar a atualização do sistema operativo de SAP HANA no Azure (instâncias de grandes)
services: virtual-machines-linux
documentationcenter: ''
author: saghorpa
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/28/2018
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: cff9be3b074dde4a0335675663133a8df81ae62d
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37114597"
---
# <a name="operating-system-upgrade"></a>Atualização do sistema operativo
Este documento descreve os detalhes na atualização do sistema operativo nas instâncias grande HANA.

>[!NOTE]
>A atualização do SO é responsabilidade de clientes, o suporte do Microsoft operations pode ajudá-lo para as principais áreas a observar a durante a atualização. Deve consultar o fornecedor do sistema operativo, bem antes de a planear uma atualização.

No momento da unidade HLI aprovisionamento, a equipa de operações do Microsoft instalar o sistema operativo. Ao longo do tempo, é necessário para manter o sistema operativo (exemplo: a aplicação de patches, otimização, atualizar etc.) na unidade HLI.

Antes de lhe principais alterações para o sistema operativo (por exemplo, atualizar SP1 ou SP2), tem de contactar a equipa do Microsoft Operations ao abrir um pedido de suporte consultar.


Para a matriz de suporte de diferentes versões de SAP HANA com as diferentes versões de Linux, consulte [SAP nota #2235581](https://launchpad.support.sap.com/#/notes/2235581).


## <a name="known-issues"></a>Problemas conhecidos

Seguem-se alguns problemas conhecidos comuns durante a atualização:
- Na classe de SKU tipo II SKU, o software foundation (SFS) é removido após a atualização de SO. Tem de reinstalar o SFS compatíveis após a atualização de SO.
- Controladores de placa de Ethernet (ENIC e FNIC) revertido para a versão mais antiga. Tem de reinstalar a versão compatível dos controladores após a atualização.

## <a name="next-steps"></a>Passos Seguintes
- Consulte [cópia de segurança e restauro](hana-overview-high-availability-disaster-recovery.md) para o SO classe de tipo I SKU de cópia de segurança.
- Consulte [cópia de segurança de SO para tipo II SKUs](os-backup-type-ii-skus.md) para a classe de tipo II SKU.