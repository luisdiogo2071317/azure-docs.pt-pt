---
title: incluir ficheiro
description: incluir ficheiro
services: search
author: HeidiSteen
ms.service: search
ms.topic: include
ms.date: 04/04/2018
ms.author: heidist
ms.custom: include file
ms.openlocfilehash: b4062aab5a453505ef4586f422a124d4bbf715cb
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/05/2018
---
Armazenamento é restrita por espaço em disco ou por um limite rígido no *número máximo* de índices, documento ou outros recursos de alto nível, o que ocorrer primeiro. A tabela seguinte documentos limites de armazenamento. Para os limites máximos em índices, documentos e outros objetos, consulte [limites por recurso](../articles/search/search-limits-quotas-capacity.md#index-limits).

| Recurso | Gratuito | Básico&nbsp;<sup>1</sup> | S1 | S2 | S3 | S3&nbsp;HD&nbsp;<sup>2</sup> |
| -------- | --- | --- | --- | --- | --- | --- |
| Nível contrato de serviço (SLA) <sup>3</sup>  |Não |Sim |Sim |Sim |Sim |Sim |
| Armazenamento por partição |50 MB |2GB |25 GB |100 GB |200 GB |200 GB |
| Partições por serviço |N/A |1 |12 |12 |12 |3 |
| Tamanho da partição |N/A |2GB |25 GB |100 GB |200 GB |200 GB |
| Réplicas |N/A |3 |12 |12 |12 |12 |

<sup>1</sup> basic tem uma partição fixa. Nesta camada SUs adicionais são utilizados para alocar mais réplicas para cargas de trabalho de consulta de aumento.

<sup>2</sup> O S3 HD tem um limite restritivo de 3 partições, que é inferior ao limite de partição do S3. O limite de partição inferior é imposto porque a contagem de índice do S3 HD é significativamente mais elevada. Dado que existem limites de serviço para recursos informáticos (processamento e armazenamento) e conteúdo (índices e documentos), o limite de conteúdo é atingido primeiro.

<sup>3</sup> contratos de nível de serviço (SLAs) são disponibilizados para serviços facturável em recursos dedicados. Funcionalidades de pré-visualização e serviços livres não ter nenhum SLA. Para os serviços faturáveis, SLAs entram em vigor quando aprovisionar redundância suficiente para o seu serviço. Dois ou mais réplicas são necessárias para o SLA de consulta (leitura). Três ou mais réplicas são necessárias para consulta e indexação SLA (leitura / escrita). O número de partições não é uma consideração de SLA. 