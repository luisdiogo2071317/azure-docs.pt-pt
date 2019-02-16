---
title: incluir ficheiro
description: incluir ficheiro
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 01/22/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 82bfdfd7481b47c08f76d077585f85763e58b87b
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/16/2019
ms.locfileid: "56331284"
---
**Transferências de dados de saída**: [Transferências de dados de saída](https://azure.microsoft.com/pricing/details/bandwidth/) faturação para utilização de largura de banda de incorrer em (dados que saem de datacenters do Azure).

**Transações**: A faturação para o número de transações que executar num disco gerido standard. Azure cobra $0.0036 por 100.000 transações para HDDs padrão. As transações incluem as opções de leitura e escrita para armazenamento.

SSDs padrão utilizam tamanho de unidade de e/s de 256 KB. Se os dados que está a ser transferidos são inferior a 256 KB, é considerado uma unidade de e/s. Tamanhos de e/s maiores são contabilizados como várias e/SS de tamanho 256 KB. Por exemplo, uma e/s de 1,100 KB é contabilizado como cinco unidades de e/s.

Não existe nenhum custo para transações para um disco gerido premium.

Para obter informações detalhadas sobre os preços do Managed Disks, consulte [preços de discos geridos](https://azure.microsoft.com/pricing/details/managed-disks).

### <a name="ultra-disks-vm-reservation-fee"></a>Taxa de reserva de VM de discos Ultra

VMs do Azure tem a capacidade para indicar se eles são compatíveis com o ultra discos. Capacidade de largura de banda entre a instância VM de computação e a unidade de escala de armazenamento de bloco para otimizar o desempenho e reduzir a latência de dedicado de um disco de ultra aloca VM compatível. Adicionar esse recurso nos resultados VM numa cobrança de reserva que é imposta apenas se tiver ativado a capacidade de disco ultra na VM sem anexar um disco de ultra a ele. Quando está ligado um disco ultra para o ultra disco VM compatível, não seria possível aplicar esta cobrança. É cobrado por vCPU aprovisionado na VM.

Consulte a [discos do Azure, página de preços](https://azure.microsoft.com/pricing/details/managed-disks/) para ultra discos os detalhes dos preços de disco.