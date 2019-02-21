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
ms.openlocfilehash: 0d771f03f9f71151ef25140148d4dd4daf3d46ec
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/20/2019
ms.locfileid: "56443425"
---
**Transferências de dados de saída**: [Transferências de dados de saída](https://azure.microsoft.com/pricing/details/bandwidth/) faturação para utilização de largura de banda de incorrer em (dados que saem de datacenters do Azure).

**Transações**: A faturação para o número de transações que executar num disco gerido standard. Azure cobra $0.0036 por 100.000 transações para HDDs padrão. As transações incluem as opções de leitura e escrita para armazenamento.

SSDs padrão utilizam tamanho de unidade de e/s de 256 KB. Se os dados que está a ser transferidos são inferior a 256 KB, é considerado uma unidade de e/s. Tamanhos de e/s maiores são contabilizados como várias e/SS de tamanho 256 KB. Por exemplo, uma e/s de 1,100 KB é contabilizado como cinco unidades de e/s.

Não existe nenhum custo para transações para um disco gerido premium.

Para obter informações detalhadas sobre os preços do Managed Disks, consulte [preços de discos geridos](https://azure.microsoft.com/pricing/details/managed-disks).

### <a name="ultra-ssd-vm-reservation-fee"></a>Taxa de reserva de VM de SSD Ultra

VMs do Azure tem a capacidade para indicar se eles são compatíveis com o ultra SSD. Capacidade de largura de banda entre a instância VM de computação e a unidade de escala de armazenamento de bloco para otimizar o desempenho e reduzir a latência de dedicado de um disco de ultra aloca VM compatível. Adicionar esse recurso nos resultados VM numa cobrança de reserva que é imposta apenas se tiver ativado a capacidade de disco ultra na VM sem anexar um disco de ultra a ele. Quando está ligado um disco ultra para o ultra disco VM compatível, não seria possível aplicar esta cobrança. É cobrado por vCPU aprovisionado na VM.

Consulte a [discos do Azure, página de preços](https://azure.microsoft.com/pricing/details/managed-disks/) para ultra discos os detalhes dos preços de disco.