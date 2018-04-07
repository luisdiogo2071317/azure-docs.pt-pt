---
title: Processamento de eventos em tempo real, utilizando o processamento de eventos do Azure Stream Analytics
description: Este artigo descreve a arquitetura de referência para alcançar o processamento de eventos em tempo real e o analytics com o Azure Stream Analytics.
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 01/24/2017
ms.openlocfilehash: 8a5d426d67916e010c7fff048eebdc77b93c5c38
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/06/2018
---
# <a name="reference-architecture-real-time-event-processing-with-microsoft-azure-stream-analytics"></a>Arquitetura de referência: processamento com o Microsoft Azure Stream Analytics de eventos em tempo real
A arquitetura de referência para o Azure Stream Analytics de processamento de eventos em tempo real destina-se para fornecer um blueprint genérico para a implementação de uma plataforma em tempo real como uma solução de processamento de fluxo de serviço (PaaS) com o Microsoft Azure.

## <a name="summary"></a>Resumo
Tradicionalmente, as soluções de análise foi com base nas capacidades, como ETL (extração, transformação, carregamento) e armazenamento de dados, onde os dados são armazenados antes da análise. Os requisitos de alteração, incluindo os dados mais rapidamente atrasados, são enviar este modelo existente para o limite. A capacidade de analisar os dados dentro de mover fluxos antes de armazenamento é uma solução e, embora não seja uma nova capacidade, a abordagem não foram amplamente ADOTOU em todos os verticals da indústria. 

Microsoft Azure fornece um catálogo um vasto conjunto de tecnologias de análise que são capazes de oferecer suporte a uma matriz de requisitos e cenários de solução diferente. Selecionar os serviços do Azure para implementar uma solução ponto-a-ponto, pode ser um desafio dado o leque de ofertas. Este documento foi concebido para descrever as capacidades e interoperacionalidade de vários serviços do Azure que suporta uma solução de transmissão em fluxo de eventos. Também explica alguns dos cenários em que os clientes podem beneficiar deste tipo abordagem.

## <a name="contents"></a>Conteúdos
* Resumo Executivo
* Introdução à análise em tempo real
* Proposta de valor dos dados em tempo real no Azure
* Cenários comuns para análise em tempo real
* Arquitetura e componentes
  * Origens de Dados
  * Camada de integração de dados
  * Camada de análise em tempo real
  * Camada de armazenamento de dados
  * Apresentação / consumo de camada
* Conclusão

**Autor:** Charles Feddersen, Arquiteto de solução, de centro de dados das informações de Excellence, a Microsoft Corporation

**Publicar:** Janeiro de 2015

**Revisão:** 1.0

**Transferir:** [processamento Microsoft Azure Stream Analytics de eventos em tempo real](http://download.microsoft.com/download/6/2/3/623924DE-B083-4561-9624-C1AB62B5F82B/real-time-event-processing-with-microsoft-azure-stream-analytics.pdf)

## <a name="get-help"></a>Obter ajuda
Para mais assistência, tente o [fórum do Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Passos Seguintes
* [Introdução ao Azure Stream Analytics](stream-analytics-introduction.md)
* [Começar a utilizar o Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Tarefas de escala do Azure Stream Analytics](stream-analytics-scale-jobs.md)
* [Referência do idioma de consulta do Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referência de API do REST de gestão do Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)

