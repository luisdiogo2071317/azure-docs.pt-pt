---
title: incluir ficheiro
description: incluir ficheiro
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 12/12/2018
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 7d1f75df9318c53a6d9e38c4d7b68587cf9a0d4b
ms.sourcegitcommit: d61faf71620a6a55dda014a665155f2a5dcd3fa2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/04/2019
ms.locfileid: "54057412"
---
**HDDs de máquina virtual gerida Standard**

| Tipo de Disco Standard  | S4               | S6               | S10             | S15 | S20              | S30              | S40              | S50              | S60 *             | S70 *             | S80 *             |
|---------------------|---------------------|---------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|
| Tamanho do disco em GiB          | 32             | 64             | 128            | 256  | 512            | 1,024    | 2.048     | 4095    | 8,192     | 16,384     | 32.767     |
| IOPs por disco       | Até 500              | Até 500              | Até 500              | Até 500 | Até 500              | Até 500              | Até 500             | Até 500              | Até 1.300              | Até 2000              | Até 2000              |
| Débito por disco | Até 60 MiB/seg | Até 60 MiB/seg | Até 60 MiB/seg | Até 60 MiB/seg | Até 60 MiB/seg | Até 60 MiB/seg | Até 60 MiB/seg | Até 60 MiB/seg| Até 300 MiB/seg | Até 500 MiB/seg | Até 500 MiB/seg |

**SSDs de máquina virtual gerida Standard**

| Tipo de disco Standard SSD  | E10               | E15               | E20             | E30              | E40              | E50              | E60 *             | E70 *             | E80 *             |
|---------------------|---------------------|---------------------|-----------------|------------------|------------------|------------------|-------------------|-------------------|-------------------|
| Tamanho do disco em GiB    | 128                 | 256                 | 512             | 1,024            | 2.048            | 4095            | 8,192             | 16,384            | 32.767            |
| IOPs por disco       | Até 500           | Até 500           | Até 500       | Até 500        | Até 500        | Até 500        | Até 1.300       | Até 2000       | Até 2000       |
| Débito por disco | Até 60 MB/seg     | Até 60 MB/seg     | Até 60 MB/seg | Até 60 MB/seg  | Até 60 MB/seg  | Até 60 MB/seg  | Até 300 MiB/seg | Até 500 MiB/seg | Até 500 MiB/seg |

**Discos da máquina virtual geridos premium: por limites de disco**

| Tipo de disco Premium  | P4               | P6               | P10             | P15 | P20              | P30              | P40              | P50              | P60 *             | P70 *             | P80 *             |
|---------------------|---------------------|---------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|
| Tamanho do disco em GiB           | 32             | 64             | 128            | 256  | 512            | 1,024    | 2.048     | 4095    | 8,192     | 16,384     | 32.767     |
| IOPs por disco       | Até 120 | Até 240              | Até 500              | Até 1,100 | Até 2,300              | Até 5000              | Até 7.500             | Até 7.500              | Até 12.500              | Máximo de 15 000              | Até 20 000              |
| Débito por disco | Até 25 MiB/seg | Máximo de 50 MiB/seg | Até 100 MiB/seg | Até 125 MiB/seg | Até 150 MiB/seg | Até 200 MiB/seg | Até 250 MiB/seg | Até 250 MiB/seg| Até 480 MiB/seg | Até 750 MiB/seg | Até 750 MiB/seg |

**Discos da máquina virtual geridos premium: por limites de VM**

| Recurso | Limite Predefinido |
| --- | --- |
| IOPs Máx Por VM |80.000 IOPS com GS5 VM |
| Débito máx por VM |2.000 MB/s com GS5 VM |
