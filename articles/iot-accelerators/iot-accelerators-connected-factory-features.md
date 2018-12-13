---
title: Ligado a recursos de solução de fábrica - Azure | Documentos da Microsoft
description: Uma visão geral dos recursos da solução pré-configurada de fábrica ligada.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 04/20/2018
ms.author: dobett
ms.openlocfilehash: af2a2c84f9eb420a7ca9a8bd5909cbf856d29a5e
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/12/2018
ms.locfileid: "53309200"
---
# <a name="what-is-connected-factory-iot-solution-accelerator"></a>O que é o acelerador de solução de IoT de fábrica ligada?

Fábrica ligada é uma implementação de arquitetura de referência do Azure Industrial IoT da Microsoft, empacotada como na solução de código-fonte aberto. Pode utilizá-lo como um ponto de partida para um produto comercial. Pode implementar uma versão previamente criada da solução de fábrica ligada na sua subscrição do Azure partir [Aceleradores de solução de IoT do Azure](https://www.azureiotsolutions.com/#solutions/types/CF).

![Dashboard de solução de fábrica ligada](./media/iot-accelerators-connected-factory-features/dashboard.png)

O acelerador de solução de fábrica ligada [código está disponível no GitHub](https://github.com/Azure/azure-iot-connected-factory).

Fábrica ligada inclui as seguintes funcionalidades:

## <a name="industrial-device-interoperability"></a>Interoperabilidade de dispositivos industriais

- Ligar a recursos de industriais com uma interface de OPC UA.
- Utilize as linhas de produção simuladas (em execução OPC UA servidores em contentores do Docker) para ver a telemetria em direto dos mesmos.
- Procure o modelo de informações de OPC UA dos servidores OPC UA partir de um dashboard da cloud.

## <a name="remote-management"></a>Gestão remota

- Configure os seus ativos de OPC UA a partir do dashboard da cloud (chamada de métodos, ler e gravar dados).
- Publicar e anular a publicação de dados de telemetria dos seus recursos de OPC UA a partir de um dashboard da cloud.

## <a name="cloud-dashboard"></a>Dashboard da cloud

- Ver as pré-visualizações de telemetria diretamente no dashboard da cloud.
- Ver as tendências em dados de telemetria e criar correlações usando o painel do Explorador do Time Series Insights.
- Ver calculada eficiência geral de equipamentos (OEE) e indicadores chave de desempenho (KPIs) a partir de um dashboard do cloud.
- Hierarquias de ativos industriais do modo de exibição numa topologia de árvore, bem como num mapa interativo.
- Ver, reconhecer e fechar alertas a partir de um dashboard da cloud.

## <a name="azure-time-series-insights"></a>Azure Time Series Insights

- [O Azure Time Series Insights](../time-series-insights/time-series-insights-overview.md) baseia-se de armazenamento, visualizar e consultar grandes quantidades de dados de séries de tempo. Este serviço ligado tira partido de fábrica.
- Fábrica ligada integra-se com este serviço, permitindo que executar uma análise profunda e em tempo real de dados do seu dispositivo.

## <a name="rules-and-alerts"></a>Regras e alertas

[Configurar regras baseadas em limiares para alertas](iot-accelerators-connected-factory-configure.md).

## <a name="end-to-end-security"></a>Segurança de ponta a ponta

- Configure permissões de segurança para os utilizadores que utilizam o controlo de acesso baseado em funções (RBAC).
- Encriptação de ponto-a-ponto é implementada utilizando a autenticação de OPC UA (usando certificados X.509), bem como os tokens de segurança.

## <a name="customizability"></a>Capacidade de personalização

- Personalize a solução para satisfazer as necessidades comerciais específicas.
- Solução completa código-fonte disponível no GitHub. Consulte a [solução pré-configurada de fábrica ligada](https://github.com/Azure/azure-iot-connected-factory) repositório.

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre a solução pré-configurada de fábrica ligada ao ler os artigos seguintes:

* [Instruções da solução pré-configurada de fábrica ligada](iot-accelerators-connected-factory-sample-walkthrough.md)
* [Implementar um gateway para a Fábrica Ligada]( iot-accelerators-connected-factory-gateway-deployment.md)
