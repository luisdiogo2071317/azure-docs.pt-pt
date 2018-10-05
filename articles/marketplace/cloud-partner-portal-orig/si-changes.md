---
title: Notas de versão do vendedor Insights | Documentos da Microsoft
description: Fornece informações sobre as alterações para a funcionalidade de informações de vendedor.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pbutlerm
ms.openlocfilehash: ec3784a91f8aeb7f0fedd13c9ab86a844832a578
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/05/2018
ms.locfileid: "48810255"
---
<a name="seller-insights-release-notes"></a>Notas de versão de informações do vendedor 
===============================
(Data de lançamento: 28 de Julho de 2018)

Este artigo fornece informações sobre as alterações à funcionalidade do vendedor Insights na [Cloud Partner Portal](https://cloudpartner.azure.com/#insights).


<a name="release-highlights"></a>Destaques da versão
------------------

-   *Os preços de estimado* fornecer uma vista de custos de cliente de moeda implicações de conversão.
-   *Previstos dividendos* fornecer uma vista anterior sobre dividendos potenciais.
-  *Identificadores de referência de utilização* proporcionar fidelidade de dados entre a utilização do cliente e os pedidos com dividendos
-   *Utilização num detalhamento diário* fornece mais granularidade e melhores informações sobre a utilização do cliente.


<a name="changes-to-data-structure-and-taxonomy"></a>Alterações à estrutura de dados e a taxonomia
--------------------------------------

A tabela seguinte lista as métricas que foram adicionadas ou alteradas consideravelmente – com esta versão. 

| **Novo termo**                   |    **Definição**                                                             |
|--------------------------------|  ---------------------------------------------------------------------------- |
| Preço (CC)                     | Preço de uma unidade de utilização para um determinado SKU (na moeda do cliente).       |
| Custo estimado de expandida (CC) | Estimado custos expandido para a quantidade de unidades de utilização para um determinado SKU (na moeda do cliente). Este valor pode não ser exato devido a erros de arredondamento ou o truncamento.   |
| Publicador de moeda (PC)        | Moeda preferida pelo fabricante para dividendos.                               |
| Preço estimado (PC)           | Preço estimado para uma unidade de utilização para um determinado SKU com base na conversão de câmbio sobre a utilização de data é calculado (na moeda do Editor). Este valor pode não ser exato devido a erros de arredondamento ou o truncamento.   |
| Custo estimado de expandida (PC) | Cobrança de expandida prevista para a quantidade de unidades de utilização para um determinado SKU com base na conversão de câmbio sobre a utilização de data é calculada (na moeda do Editor). Este valor pode não ser exato devido a erros de arredondamento ou o truncamento. |
| Dividendos estimado (PC)          | Pagamento estimado para a quantidade de unidades de utilização para um determinado SKU com base na conversão de câmbio na data que da utilização é calculada (na moeda do Editor). Este valor pode não ser exato devido a erros de arredondamento ou o truncamento.   |
| Referência de utilização                | O identificador para um ou mais dias de utilização do cliente para um determinado SKU associado com uma entrada no relatório dividendos. |
|  |  |
