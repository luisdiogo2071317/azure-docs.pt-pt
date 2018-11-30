---
title: Prever o estado de funcionamento do veículo e orientar hábitos - Azure | Documentos da Microsoft
description: Utilize as capacidades do Cortana Intelligence para obter informações em tempo real e preditivo sobre o estado de funcionamento do veículo e orientar hábitos.
services: machine-learning
author: marktab
ms.author: tdsp
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.component: team-data-science-process
ms.topic: article
ms.date: 03/14/2018
ms.custom: (previous author=deguhath, ms.author=deguhath)
ms.openlocfilehash: 688a821667b38423b43af87b7a26525e52d9a7aa
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/28/2018
ms.locfileid: "52444164"
---
# <a name="vehicle-telemetry-analytics-solution-playbook"></a>Playbook de solução de análise de telemetria de veículos

Superutilizadores computadores terem sido movidos para fora do laboratório e estão agora estacionados na garages. Eles são agora a ser colocados na vanguarda automóveis que contêm uma infinidade sensores. Esses sensores lhes dar a capacidade de controlar e monitorizar milhões de eventos a cada segundo. Até 2020, a maioria destes veículos será ligada à Internet. Explorar este mundo de dados fornece maior segurança, confiabilidade, e então, originando uma melhor experiência. A Microsoft faz isso sonhar numa realidade com o Cortana Intelligence.

O Cortana Intelligence é uma totalmente geridas de macrodados e de análises avançadas que pode utilizar para transformar os seus dados em ações inteligentes. O modelo de solução de análise de telemetria do Cortana Intelligence veículo demonstra como concessionários de automóveis, os fabricantes de automóveis e as companhias conseguem obter em tempo real e informações preditivas sobre o estado de funcionamento do veículo e orientar hábitos.

A solução é implementada como um [padrão de arquitetura de lambda](https://en.wikipedia.org/wiki/Lambda_architecture), que mostra todo o potencial da plataforma Cortana Intelligence para em tempo real e processamento em lotes.

## <a name="architecture"></a>Arquitetura
A arquitetura de solução de análise de telemetria de veículos é ilustrada neste diagrama:

![Diagrama da arquitetura de soluções](./media/cortana-analytics-playbook-vehicle-telemetry/fig1-vehicle-telemetry-annalytics-solution-architecture.png)


Esta solução inclui os seguintes componentes do Cortana Intelligence e demonstra a sua integração:

* **Os Hubs de eventos do Azure** ingere milhões de eventos de telemetria de veículos para o Azure.
* **O Azure Stream Analytics** fornece informações em tempo real sobre o estado de funcionamento do veículo e mantém esses dados no armazenamento de longo prazo para análise de lotes mais sofisticada.
* **O Azure Machine Learning** Deteta anomalias em tempo real e utiliza o processamento em lotes para fornecer informações preditivas.
* **O Azure HDInsight** transforma os dados em escala.
* **O Azure Data Factory** processa a orquestração, agendamento, gestão de recursos e monitorização do pipeline de processamento em lotes.
* **Power BI** fornece a esta solução um dashboard avançado para dados em tempo real e visualizações de Análise Preditiva.

Esta solução acessa duas origens de dados diferentes: 

* **Simulated sinais de veículos e diagnóstico**: o vehicle telematics simulator emite informações de diagnóstico e sinais que correspondem ao estado do veículo e o padrão de condução num determinado ponto no tempo. 
* **Catálogo de veículos**: este conjunto de dados de referência mapeia os números de VIN a modelos.

## <a name="next-steps"></a>Próximos Passos

Para explorar ainda mais esta solução, consulte [playbook de solução de análise de telemetria Vehicle: aprofunde-se na solução](cortana-analytics-playbook-vehicle-telemetry-deep-dive.md).

Para saber como configurar os relatórios do Power BI e dashboards para esta solução, veja [instruções de configuração do dashboard de solução de análise do veículo telemetria modelo Power BI](cortana-analytics-playbook-vehicle-telemetry-powerbi.md).
