---
title: incluir ficheiro
description: incluir ficheiro
services: virtual-machines
author: jonbeck7
ms.service: virtual-machines
ms.topic: include
ms.date: 03/09/2018
ms.author: azcspmt;jonbeck;cynthn
ms.custom: include file
ms.openlocfilehash: eaf490979167ea5cbfe5846e62444526310452f8
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36269549"
---
Algumas cargas de trabalho de base de dados, como o SQL Server ou Oracle requerem elevada da memória, armazenamento e largura de banda de e/s, mas não uma contagem de núcleos elevada. Muitas cargas de trabalho de base de dados não são intensivas em CPU. O Azure disponibiliza determinados tamanhos de VM em que pode restringir o número de vCPU VM para reduzir o custo de licenciamento de software, mantendo a mesma memória, armazenamento e largura de banda de e/s.

A contagem de vCPU pode estar limitada a meio de um ou de um trimestre do tamanho da VM original. Estes novos tamanhos VM tem um sufixo que especifica o número de vCPUs Active Directory para os tornar mais fácil para o identificar.

Por exemplo, o tamanho da VM atual Standard_GS5 vem com 32 vCPUs, 448 GB de RAM, 64 discos (até 256 TB) e 80.000 IOPs ou 2 GB/s de largura de banda de e/s. A nova VM tamanhos Standard_GS5-16 e Standard_GS5-8 é fornecido com 16 e 8 vCPUs Active Directory, respetivamente, enquanto mantém o resto das especificações de Standard_GS5 para memória, armazenamento e largura de banda de e/s.

As licenciamento taxas cobradas para SQL Server ou Oracle estão limitadas para a nova contagem de vCPU, e outros produtos devem ser-lhe cobrados com base na contagem de novos vCPU. Isto resulta num aumento de 50% para 75% o rácio entre as especificações VM para o Active Directory vCPUs (facturável). Estes novos tamanhos de VM, apenas disponíveis no Azure, permitem uma utilização da CPU mais elevada a uma fração do custo de licenciamento (por núcleo). Neste momento, o custo de computação, que inclui o licenciamento do SO, permanece o mesmo um como o tamanho original. Para obter mais informações, consulte [tamanhos de VM do Azure para cargas de trabalho de base de dados económica mais](https://azure.microsoft.com/blog/announcing-new-azure-vm-sizes-for-more-cost-effective-database-workloads/).


| Nome                | vCPU | Especificações           |
|---------------------|------|-----------------|
| Standard_M8 2ms     | 2    | Mesmo que M8ms    |
| Standard_M8 4ms     | 4    | Mesmo que M8ms    |
| Standard_M16 4ms    | 4    | Mesmo que M16ms   |
| Standard_M16 8ms    | 8    | Mesmo que M16ms   |
| Standard_M32 8ms    | 8    | Mesmo que M32ms   |
| Standard_M32 16ms   | 16   | Mesmo que M32ms   |
| Standard_M64 32ms   | 32   | Mesmo que M64ms   |
| Standard_M64 16ms   | 16   | Mesmo que M64ms   |
| Standard_M128 64ms  | 64   | Mesmo que M128ms  |
| Standard_M128 32ms  | 32   | Mesmo que M128ms  |
| Standard_E4 2s_v3   | 2    | Mesmo que E4s_v3  |
| Standard_E8 4s_v3   | 4    | Mesmo que E8s_v3  |
| Standard_E8-2s_v3   | 2    | Mesmo que E8s_v3  |
| Standard_E16 8s_v3  | 8    | Mesmo que E16s_v3 |
| Standard_E16 4s_v3  | 4    | Mesmo que E16s_v3 |
| Standard_E32 16_v3  | 16   | Mesmo que E32s_v3 |
| Standard_E32-8s_v3  | 8    | Mesmo que E32s_v3 |
| Standard_E64 32s_v3 | 32   | Mesmo que E64s_v3 |
| Standard_E64 16s_v3 | 16   | Mesmo que E64s_v3 |
| Standard_GS4-8      | 8    | Mesmo que GS4     |
| Standard_GS4-4      | 4    | Mesmo que GS4     |
| Standard_GS5-16     | 16   | Mesmo que GS5     |
| Standard_GS5-8      | 8    | Mesmo que GS5     |
| Standard_DS11-1_v2  | 1    | Mesmo que DS11_v2 |
| Standard_DS12-2_v2  | 2    | Mesmo que DS12_v2 |
| Standard_DS12-1_v2  | 1    | Mesmo que DS12_v2 |
| Standard_DS13-4_v2  | 4    | Mesmo que DS13_v2 |
| Standard_DS13-2_v2  | 2    | Mesmo que DS13_v2 |
| Standard_DS14-8_v2  | 8    | Mesmo que DS14_v2 |
| Standard_DS14-4_v2  | 4    | Mesmo que DS14_v2 |
