---
title: Ligado funcionalidades de solução de fábrica - Azure | Microsoft Docs
description: Uma descrição geral das funcionalidades do Factory ligado a solução pré-configurada.
services: iot-suite
suite: iot-suite
documentationcenter: ''
author: dominicbetts
manager: timlt
editor: ''
ms.assetid: ''
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/20/2018
ms.author: dobett
ms.openlocfilehash: 7ee86a59e7c47406ea1a890f1a9d1eb2ed4ad772
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/20/2018
---
# <a name="what-is-azure-iot-suite-connected-factory"></a>O que é o Azure IoT Suite ligado Factory?

Fábrica ligada é uma implementação Azure industriais referência do Microsoft arquitetura do IoT, empacotada como na solução de open source. Pode utilizá-lo como um ponto de partida para um produto comercial. Pode implementar uma versão pré-criadas da solução fábrica ligado na sua subscrição do Azure de [Azure IoT Suite](https://www.azureiotsuite.com/#solutions/types/CF).

![Dashboard de solução de fábrica ligado](./media/iot-accelerators-connected-factory-features/dashboard.png)

Fábrica ligada inclui as seguintes funcionalidades:

## <a name="industrial-device-interoperability"></a>Interoperabilidade de dispositivo industriais

- Ligar a recursos industriais com uma interface de OPC UA.
- Utilize as linhas simuladas de produção (em execução OPC UA servidores nos contentores do Docker) para ver a telemetria em direto dos mesmos.
- Procure o modelo de informações de OPC UA dos servidores OPC UA do dashboard do cloud.

## <a name="remote-management"></a>Gestão remota

- Configure os recursos de OPC UA do dashboard do cloud (chamada métodos, ler e escrever dados).
- Publicar e anular a publicação de dados de telemetria dos seus recursos de OPC UA de um dashboard de nuvem.

## <a name="cloud-dashboard"></a>Dashboard de nuvem

- Ver pré-visualizações de telemetria diretamente no dashboard do cloud.
- Ver tendências em dados de telemetria e crie correlações utilizar o dashboard do Explorador de Insights de séries de tempo.
- Ver calculada eficiência de equipamento geral (OEE) e indicadores chave de desempenho (KPIs) de um dashboard de nuvem.
- Vista asset industriais as hierarquias numa topologia de árvore, bem como um mapa interativo.
- Ver, reconhecer e fechar alertas a partir de um dashboard de nuvem.

## <a name="azure-time-series-insights"></a>Azure Time Series Insights

- [As informações de séries de tempo do Azure](../time-series-insights/time-series-insights-overview.md) foi concebido para armazenar, visualizar e consultar grandes quantidades de dados de séries de tempo. Ligado tira partido do Factory este serviço.
- Fábrica ligada integra-se este serviço, permitindo-lhe executar uma análise em tempo real, profunda dos seus dados do dispositivo.

## <a name="rules-and-alerts"></a>Regras e alertas

[Configurar regras baseadas em limiares para alertas](iot-accelerators-connected-factory-configure.md).

## <a name="end-to-end-security"></a>Segurança ponto a ponto

- Configure permissões de segurança para utilizadores que utilizam o controlo de acesso baseado em funções (RBAC).
- Encriptação de ponto a ponto é implementada utilizando a autenticação de OPC UA (utilizando certificados x. 509), bem como os tokens de segurança.

## <a name="customizability"></a>Customizability

- [Personalizar](../iot-suite/iot-suite-v1-guidance-on-customizing-preconfigured-solutions.md) a solução para satisfazer as necessidades comerciais específicas.
- A solução completa-código fonte está disponível no GitHub. Consulte o [ligado fábrica solução pré-configurada](https://github.com/Azure/azure-iot-connected-factory) repositório.

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre a solução de fábrica ligado pré-configurada ao ler os artigos seguintes:

* [Instruções sobre a solução pré-configurada de fábrica ligada](iot-accelerators-connected-factory-sample-walkthrough.md)
* [Implementar um gateway para a Fábrica Ligada]( iot-accelerators-connected-factory-gateway-deployment.md)
