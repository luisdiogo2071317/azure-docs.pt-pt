---
title: incluir ficheiro
description: incluir ficheiro
services: virtual-machines
author: jonbeck7
ms.service: virtual-machines
ms.topic: include
ms.date: 07/06/2018
ms.author: azcspmt;jonbeck;cynthn
ms.custom: include file
ms.openlocfilehash: 033ae1de25fbaada0c2bce715e6bdd71818b341a
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/07/2018
ms.locfileid: "37906807"
---
<!-- F-series, Fs-series* -->

Computação otimizados tamanhos de VM de ter um rácio de CPU / memória elevado e são ideais para servidores web com tráfego médio, aplicações de rede, processos em lote e servidores de aplicações. Este artigo fornece informações sobre o número de vCPUs, discos de dados e NICs, bem como armazenamento e débito de rede da largura de banda para cada tamanho neste agrupamento.

Série Fsv2 baseia-se no processador Intel® Xeon® Platinum 8168, com uma frequência de base de núcleo de 2,7 GHz e uma frequência máxima turbo de núcleo único de 3,7 GHz. Instruções do Intel® AVX-512, que são novas nos processadores Intel de dimensionável, irão fornecer até um 2x no aumento do desempenho para cargas de trabalho de processamento de vetor em único e de dupla precisão operações de vírgula flutuante. Em outras palavras, elas são realmente rápidas para qualquer carga de trabalho computacional. 

Por um preço de lista por hora mais baixo, a série Fsv2 é o melhor valor de desempenho por preço no portefólio do Azure com base na unidade do Azure de computação (ACU) por vCPU. 

A série F tem por base o processador Intel Xeon® E5-2673 v3 (Haswell) de 2,4 GHz, o qual pode atingir velocidades de relógio de 3,1 GHz com o Intel Turbo Boost Technology 2.0. Este é o mesmo desempenho de CPU da série Dv2 de VMs.  

As VMs da série F são uma excelente opção para cargas de trabalho que exigem CPUs mais rápidas, mas não precisam de mais memória ou armazenamento temporário por vCPU.  As cargas de trabalho, como análise, servidores de jogos, servidores Web e processamento de lotes, beneficiarão do valor da série F.

A série Fs oferece todas as vantagens da série F, além do armazenamento Premium.

## <a name="fsv2-series-sup1sup"></a>Série Fsv2 <sup>1</sup>

ACU: 195 - 210

O armazenamento Premium: suportado

A cache de armazenamento Premium: suportado

| Tamanho             | da vCPU | Memória: GiB | Armazenamento (SSD) temporário GiB | Discos de dados máximos | Débito máximo do armazenamento temporário e em cache: IOPS/MBps (tamanho da cache em GiB) | NICs. Máx. / esperado de largura de banda de rede (Mbps) |
|------------------|--------|-------------|----------------|----------------|-----------------------------------------------------------------------|------------------------------------------------|
| Standard_F2s_v2  | 2      | 4           | 16             | 4              | 4000 (32)                                                             | 2 / 875                                        |
| Standard_F4s_v2  | 4      | 8           | 32             | 8              | 8000 (64)                                                             | 2 / 1,750                                      |
| Standard_F8s_v2   | 8      | 16          | 64             | 16             | 16000 (128)                                                           | 4 / 3.500                                      |
| Standard_F16s_v2 | 16     | 32          | 128            | 32             | 32000 (256)                                                           | 4 / 7.000                                      |
| Standard_F32s_v2 | 32     | 64          | 256            | 32             | 64000 (512)                                                           | 8 / 14 000                                     |
| Standard_F64s_v2 | 64     | 128         | 512            | 32             | 128000 (1024)                                                         | 8 / 28,000                                     |
| Standard_F72s_v2<sup>2, 3</sup> | 72     | 144         | 576            | 32             | 144000 (1520)                                                         | 8 / 30,000                                     |

<sup>1</sup> as VMS da série Fsv2 funcionalidade Intel® Hyper-Threading Technology

<sup>2</sup> mais do que 64 Vcpus exigem um destes SO convidados suportados: Windows Server 2016, Ubuntu 16.04 LTS, SLES 12 SP2 e Red Hat Enterprise Linux, CentOS 7.3 ou Oracle Linux 7.3 com LIS 4.2.1

<sup>3</sup> instância está isolada do hardware dedicado de um único cliente.

## <a name="fs-series-sup1sup"></a>Série FS <sup>1</sup>

ACU: 210 - 250

O armazenamento Premium: suportado

A cache de armazenamento Premium: suportado

| Tamanho | vCPU | Memória: GiB | Armazenamento (SSD) temporário GiB | Discos de dados máximos | Débito máximo do armazenamento temporário e em cache: IOPS/MBps (tamanho da cache em GiB) | Débito máximo do disco não colocado em cache: IOPS/MBps | NICs. Máx. / esperado de largura de banda de rede (Mbps) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_F1s |1 |2 |4 |4 |4,000 / 32 (12) |3,200 / 48 |2 / 750 |
| Standard_F2s |2 |4 |8 |8 |8,000 / 64 (24) |6,400 / 96 |2 / 1500 |
| Standard_F4s |4 |8 |16 |16 |16,000 / 128 (48) |12,800 / 192 |4 / 3000 |
| Standard_F8s |8 |16 |32 |32 |32,000 / 256 (96) |25,600 / 384 |8 / 6000 |
| Standard_F16s |16 |32 |64 |64 |64,000 / 512 (192) |51,200 / 768 |8 / 12000 |

MBps = 10^6 bytes por segundo e GiB = 1024^3 bytes.

<sup>1</sup> o débito máximo do disco (IOPS ou MBps) possível com uma VM da série Fs pode estar limitado pelo número, tamanho e repartição dos discos anexados.  Para obter mais detalhes, veja [Armazenamento Premium: armazenamento de alto desempenho para cargas de trabalho de máquinas virtuais do Azure](../articles/virtual-machines/windows/premium-storage.md).


<br>

## <a name="f-series"></a>Série F

ACU: 210 - 250

Armazenamento Premium: Não suportado

Cache de armazenamento Premium: Não suportado

| Tamanho         | vCPU | Memória: GiB | Armazenamento (SSD) temporário GiB | Débito do armazenamento temporário máximo: IOPS/MBps de Leitura/MBps de Escrita | Máximo do disco de dados/débito: IOPS | NICs. Máx. / esperado de largura de banda de rede (Mbps) |
|--------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_F1  | 1         | 2           | 16             | 3000 / 46 / 23                                           | 4 / 4x500                         | 2 / 750                 |
| Standard_F2  | 2         | 4           | 32             | 6000 / 93 / 46                                           | 8 / 8x500                         | 2 / 1500                     |
| Standard_F4  | 4         | 8           | 64             | 12000 / 187 / 93                                         | 16 / 16x500                         | 4 / 3000                     |
| Standard_F8  | 8         | 16          | 128            | 24000 / 375 / 187                                        | 32 / 32x500                       | 8 / 6000                     |
| Standard_F16 | 16        | 32          | 256            | 48000 / 750 / 375                                        | 64 / 64x500                       | 8 / 12000           |


<br>


