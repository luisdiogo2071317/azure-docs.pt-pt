---
title: Lista de permissões de porta para Avere vFXT para o Azure
description: Lista das portas utilizadas pelo vFXT Avere para o Azure
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: 394415ffc7593d058d8d0bf1c0040b88ec25073b
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/31/2018
ms.locfileid: "50634501"
---
# <a name="required-ports"></a>Portas necessárias

Portas indicadas nesta secção são utilizadas para vFXT comunicação de entrada e saída.

Nunca exponha o cluster vFXT ou a instância do controlador de cluster diretamente para a internet pública.

## <a name="api"></a>API

| Entrada: | | |
| --- | ---- | --- |
| TCP | 22  | SSH  |
| TCP | 443 | HTTPS|



| Saída: |     |       |
|----------|-----|-------|
| TCP      | 443 | HTTPS |

 
## <a name="nfs"></a>NFS

| Entrada e Saída  | | |
| --- | --- | ---|
| TCP/UDP | 111  | RPCBIND  |
| TCP/UDP | 2049 | NFS      |
| TCP/UDP | 4045 | NLOCKMGR |
| TCP/UDP | 4046 | MOUNTD   |
| TCP/UDP | 4047 | ESTADO   |

