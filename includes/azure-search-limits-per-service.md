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
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38756290"
---
Armazenamento é limitado pelo espaço em disco ou por um limite restritivo no *número máximo* de índices, documento ou outros recursos de alto nível, o que ocorrer primeiro. A tabela seguinte documenta os limites de armazenamento. Para obter limites máximos em índices, documentos e outros objetos, consulte [limites por recurso](../articles/search/search-limits-quotas-capacity.md#index-limits).

| Recurso | Gratuito | Básica&nbsp;<sup>1</sup> | S1 | S2 | S3 | S3&nbsp;HD&nbsp;<sup>2</sup> |
| -------- | --- | --- | --- | --- | --- | --- |
| (SLA) de contrato de nível de serviço <sup>3</sup>  |Não |Sim |Sim |Sim |Sim |Sim |
| Armazenamento por partição |50 MB |2GB |25 GB |100 GB |200 GB |200 GB |
| Partições por serviço |N/A |1 |12 |12 |12 |3 |
| Tamanho da partição |N/A |2 GB |25 GB |100 GB |200 GB |200 GB |
| Réplicas |N/A |3 |12 |12 |12 |12 |

<sup>1</sup> basic possui uma partição fixa. Nesta camada de SUs adicionais são utilizados para a alocação de réplicas mais para cargas de trabalho de consulta maior.

<sup>2</sup> O S3 HD tem um limite restritivo de 3 partições, que é inferior ao limite de partição do S3. O limite de partição inferior é imposto porque a contagem de índice do S3 HD é significativamente mais elevada. Dado que existem limites de serviço para recursos informáticos (processamento e armazenamento) e conteúdo (índices e documentos), o limite de conteúdo é atingido primeiro.

<sup>3</sup> contratos de nível de serviço (SLAs) estão disponíveis para os serviços faturáveis em recursos dedicados. Recursos de serviços e pré-visualização gratuitos não tem nenhum SLA. Para os serviços faturáveis, SLAs em vigor quando Aprovisiona redundância suficiente para o seu serviço. Duas ou mais réplicas são necessárias para o SLA de consulta (ler). Três ou mais réplicas são necessárias para consulta e indexação de SLA (leitura / escrita). O número de partições não é uma consideração de SLA. 