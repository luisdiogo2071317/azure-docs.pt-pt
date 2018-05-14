---
title: Atualização do sistema operativo para o SAP HANA no Azure (instâncias de grande) | Microsoft Docs
description: Efetuar a atualização do sistema operativo de SAP HANA no Azure (instâncias de grandes)
services: virtual-machines-linux
documentationcenter: ''
author: saghorpa
manager: timlt
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/11/2018
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1354b67ade78a0da1578ef9d98c3b1e8edb41cd4
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/12/2018
---
# <a name="operating-system-upgrade"></a>Atualização do sistema operativo
Este documento descreve os detalhes na atualização do sistema operativo nas instâncias grande HANA.

>[!NOTE]
>A atualização do SO é responsabilidade de clientes, o suporte do Microsoft operations pode ajudá-lo para as principais áreas a observar a durante a atualização. Deve consultar o fornecedor do sistema operativo, bem antes de a planear uma atualização.

No momento da unidade HLI aprovisionamento, a equipa de operações do Microsoft instalar o sistema operativo. Ao longo do tempo, é necessário para manter o sistema operativo (exemplo: a aplicação de patches, otimização, atualizar etc.) na unidade HLI.

Antes de lhe principais alterações para o sistema operativo (por exemplo, um SO de atualização), **tem** considere o seguinte matriz de compatibilidade. **Tem** também contacte a equipa do Microsoft Operations ao abrir um pedido de suporte consultar antes de iniciar as atividades de sistema de operativo principal como a atualização.

Para a matriz de suporte de diferentes versões de SAP HANA com as diferentes versões de Linux, consulte [SAP nota #2235581](https://launchpad.support.sap.com/#/notes/2235581).

A compatibilidade seguinte foi testada para os HLIs. Se o servidor HLI está fora da matriz de compatibilidade, contacte o suporte de operação de Microsoft.

## <a name="for-type-i-class-sku-category"></a>Para o tipo de classe posso categoria SKU

| Configuração | SUSE12 SP1 | SUSE12 SP2 | RHEL 7.2 | RHEL 7.3|
| --- | --- | --- | --- | --- |
| Firmware de servidor | 3.1(2b) | 3.1(2b) | 3.1(2b) | 3.1(2b) |
| Versão ENIC | 2.3.0.44 | 2.3.0.44 | 2.3.0.30 | 2.3.0.44 |
| Versão FNIC | 1.6.0.34 | 1.6.0.34 | 1.6.0.27 | 1.6.0.36 |
| EDAC | Desativado | Desativado | Desativado | Desativado |
| Versão de kernel | 4.4.21-69-default | 3.12.49-11-default | 3.10.0-327.el7.x86_64 | 3.10.0-693.17.1 |


## <a name="for-type-ii-class-sku-category"></a>Para o tipo II categoria SKU de classe.

| Configuração | SUSE12 SP1 | SUSE12 SP2 | RHEL 7.2 | RHEL 7.3|
| --- | --- | --- | --- | --- |
| Versão de Firmware RMC | 1.1.121  | 1.1.121  | 1.1.121  | 1.1.121 |
| Versão de Firmware BMC | 1.0.43   | 1.0.43   | 1.0.43   | 1.0.43  |
| Versão do Software Foundation Server (SFS) | 2.16    | 2.16    | 2.14/2.16   | 2.16   |
| BIOS | 5.2.6    | 5.2.6    | 5.2.6    | 5.2.6   |
| Versão i40e    | 2.0.19     | 2.0.19     | 1.5.10-k    | 1.5.10-k   |
| Versão de kernel    | 3.12.49-11.1     | 4.4.21-69.1     | 3.10.0-327    | 3.10.0-693.17.1   |


## <a name="known-issues"></a>Problemas conhecidos

Seguem-se alguns problemas conhecidos comuns durante a atualização:
- Na classe de SKU tipo II SKU, o software foundation (SFS) é removido após a atualização de SO. Tem de reinstalar o SFS compatíveis após a atualização de SO.
- Controladores de placa de Ethernet (ENIC e FNIC) revertido para a versão mais antiga. Tem de reinstalar a versão compatível dos controladores após a atualização.

## <a name="next-steps"></a>Passos Seguintes
- Consulte [cópia de segurança e restauro](hana-overview-high-availability-disaster-recovery.md) para o SO classe de tipo I SKU de cópia de segurança.
- Consulte [cópia de segurança de SO para tipo II SKUs](os-backup-type-ii-skus.md) para a classe de tipo II SKU.