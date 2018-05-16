---
title: Descrição geral dos aceleradores de soluções do Azure IoT | Microsoft Docs
description: Descrição dos aceleradores de soluções do Azure IoT e da sua arquitetura com ligações para recursos adicionais.
services: iot-suite
suite: iot-suite
documentationcenter: ''
author: dominicbetts
manager: timlt
editor: ''
ms.assetid: 59009f37-9ba0-4e17-a189-7ea354a858a2
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 17/01/2018
ms.author: dobett
ms.openlocfilehash: 2dc286dd228b1990e0307476d3623ffc91489f98
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="what-are-the-iot-solution-accelerators"></a>O que são os aceleradores de soluções IoT?

Os _aceleradores de soluções_ do Azure IoT são um conjunto de soluções que:

* São implementadas em minutos
* Ajudam a começar rapidamente
* Podem ser personalizadas para satisfazer os seus requisitos específicos

Os aceleradores de soluções foram concebidos de acordo com os mesmos princípios e objetivos.

O seguinte vídeo apresenta uma descrição geral do acelerador de soluções de monitorização remota:

>[!VIDEO https://channel9.msdn.com/Shows/Internet-of-Things-Show/Meet-the-new-Remote-Monitoring-accelerator-for-Azure-IoT/Player]

## <a name="solution-accelerators-overview"></a>Descrição geral dos aceleradores de soluções

Um acelerador de soluções é uma implementação de código aberto de padrões comuns de soluções IoT que pode implementar no Azure através da sua subscrição. Cada acelerador de soluções combina código personalizado e serviços do Azure para implementar um ou mais cenários de IoT específicos. Pode personalizar qualquer um destes cenários para satisfazer os seus requisitos específicos. Os cenários incluem:

* Ver dados num dashboard avançado para obter estatísticas aprofundadas e o estado da solução.
* Configurar regras e alarmes sobre telemetria dos dispositivos IoT em direto.
* Agendar tarefas de gestão de dispositivos, como atualizações de software e configuração.
* Aprovisionar os seus próprios dispositivos físicos ou simulados personalizados.
* Resolver problemas e remediar problemas nos seus grupos de dispositivos IoT.

Cada acelerador de soluções é uma implementação ponto a ponto completa que pode utilizar dispositivos simulados ou físicos para gerar a telemetria. Pode utilizar os aceleradores de soluções para:

* Funcionar como ponto de partida para as suas próprias soluções de IoT.
* Saber mais sobre os padrões comuns na conceção e desenvolvimento da solução IoT.

Estão atualmente disponíveis três aceleradores de soluções:

* [Monitorização Remota](iot-suite-remote-monitoring-explore.md)
* [Manutenção Preditiva](iot-suite-predictive-overview.md)
* [Fábrica Ligada](iot-suite-connected-factory-overview.md)

A tabela seguinte mostra a forma como as soluções mapeiam funcionalidades específicas do IoT:

| Solução | Ingestão de dados | Identidade do dispositivo | Gestão de dispositivos | Processamento na periferia | Comando e controlo | Regras e ações | Análise preditiva |
| ------------------------------------------------------------ | -- | -- | -- | -- | -- | -- | -- |
| [Monitorização Remota](iot-suite-remote-monitoring-explore.md)  |Sim |Sim |Sim |-   |Sim |Sim |-   |
| [Manutenção Preditiva](iot-suite-predictive-overview.md)   |Sim |Sim |-   |-   |Sim |Sim |Sim |
| [Fábrica Ligada](iot-suite-connected-factory-overview.md) |Sim |- |- |Sim |Sim |Sim |-   |

* *Ingestão de dados*: Entrada de dados à escala na nuvem.
* *Identidade do dispositivo*: Gerir identidades únicas de cada dispositivo e controlar o acesso do dispositivo à solução.
* *Gestão de dispositivos*: Gerir metadados de dispositivos e executar operações, como reinícios de dispositivo e atualizações de firmware.
* *Comando e controlo*: para fazer com que o dispositivo realize uma ação, envie mensagens da cloud para um dispositivo.
* *Regras e ações*: para realizar ações em dados de dispositivo para a cloud específicos, o back-end da solução utiliza regras.
* *Análise preditiva*: A solução de back-end analisa os dados do dispositivo para a cloud para prever quando as ações específicas deverão ocorrer. Por exemplo, analisar a telemetria do motor de uma aeronave para determinar quando deverá ser realizada a manutenção.

> [!NOTE]
> Para implementar um acelerador de soluções e saber mais sobre como pode personalizá-lo, visite [Aceleradores de soluções do Microsoft Azure IoT](https://www.azureiotsuite.com/).

## <a name="azure-services"></a>Serviços do Azure

Quando implementa um acelerador de soluções, o processo de aprovisionamento configura vários serviços do Azure. A tabela seguinte mostra os serviços utilizados nos aceleradores de soluções:

|                      | Monitorização Remota  | Manutenção Preditiva | Fábrica Ligada |
| -------------------- | ------------------ | ---------------------- | ----------------- |
| IoT Hub              | Sim                | Sim                    | Sim               |
| Event Hubs           |                    | Sim                    |                   |
| Time Series Insights |                    |                        | Sim               |
| Serviços de Contentores   | Sim                |                        |                   |
| Stream Analytics     |                    | Sim                    |                   |
| Aplicações Web             | Sim                | Sim                    | Sim               |
| BD do Cosmos            | Sim                | Sim                    |                    |
| Storage do Azure         |                    | Sim                    | Sim               |

> [!NOTE]
> Para obter mais informações sobre os recursos implementados no acelerador de soluções de monitorização remota, veja este [artigo](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/blob/master/README.md#basic-vs-standard-deployments) no GitHub.

* [Hub IoT do Azure](../iot-hub/index.yml). Este serviço fornece funcionalidades de mensagens do dispositivo para cloud e da cloud para dispositivo e funciona como o gateway para a cloud e outros serviços essenciais do acelerador de soluções. O serviço permite-lhe receber mensagens dos seus dispositivos de escala e enviar comandos aos seus dispositivos. O serviço também lhe permite [gerir os seus dispositivos](../iot-hub/iot-hub-device-management-overview.md). Por exemplo, pode configurar, reiniciar ou efetuar uma reposição de fábrica num ou mais dispositivos ligados ao hub.
* [Hubs de Eventos do Azure](../event-hubs/index.md). Este serviço disponibiliza ingestão de elevados volumes de eventos na cloud. Veja [Comparação do Hub IoT do Azure e Hubs de Eventos do Azure](../iot-hub/iot-hub-compare-event-hubs.md).
* [Azure Time Series Insights](../time-series-insights/index.yml). Os aceleradores de soluções utilizam este serviço para analisar e apresentar os dados de telemetria dos dispositivos.
* [Azure Container Service](../container-service/index.yml). Este serviço aloja e gere os microsserviços dos aceleradores de soluções.
* [Azure Cosmos DB](../cosmos-db/index.yml) e [Armazenamento do Azure](../storage/index.yml) para armazenamento de dados.
* [Azure Stream Analytics](../stream-analytics/index.yml). A solução pré-configurada de manutenção preditiva utiliza este serviço para processar a telemetria de entrada, executar a agregação e detetar eventos. Esta solução pré-configurada utiliza ainda o Stream Analytics para processar mensagens de informações que contêm dados como metadados ou respostas aos comandos dos dispositivos.
* [Aplicações Web do Azure](../app-service/index.yml), para alojar o código da aplicação personalizada nas soluções pré-configuradas.

Para uma descrição geral da arquitetura de uma solução IoT típica, veja [Microsoft Azure e a Internet das Coisas (IoT)](iot-suite-what-is-azure-iot.md).

## <a name="whats-new-in-solution-accelerators"></a>O que há de novo nos aceleradores de soluções?

A Microsoft está a atualizar os aceleradores de soluções para uma nova arquitetura baseada em microsserviços. A tabela abaixo mostra o estado atual dos aceleradores de soluções:

| Acelerador de soluções | Arquitetura  | Linguagens     |
| ---------------------- | ------------- | ------------- |
| Monitorização Remota      | Microsserviços | Java e .NET |
| Manutenção Preditiva | MVC           | .NET          |
| Fábrica Ligada      | MVC           | .NET          |

As secções seguintes descrevem as novidades nos aceleradores de soluções baseados em microsserviços:

### <a name="microservices"></a>Microsserviços

A nova versão do acelerador de soluções de monitorização remota utiliza uma arquitetura de microsserviços. O acelerador de soluções é composto por múltiplos microsserviços, como um *Gestor do Hub IoT* e um *Gestor de armazenamento*. Tanto a versão Java, como .NET, de cada microssserviço está disponível para transferência, juntamente com a documentação de programação associada. Para obter mais informações sobre os microsserviços, veja [Arquitetura de Monitorização Remota](iot-suite-remote-monitoring-sample-walkthrough.md).

Esta arquitetura de microsserviços é um padrão comprovado para soluções na cloud que:

* É escalável.
* Permite a extensibilidade.
* É fácil de compreender.
* Permite a substituição de serviços individuais por alternativas.

> [!TIP]
> Para saber mais sobre as arquiteturas de microsserviços, veja [.NET Application Architecture](https://www.microsoft.com/net/learn/architecture) (Arquitetura de Aplicações .NET) e [Microservices: An application revolution powered by the cloud](https://azure.microsoft.com/blog/microservices-an-application-revolution-powered-by-the-cloud/) (Microsserviços: uma revolução nas aplicações com tecnologia da cloud).

Quando implementa a versão nova da monitorização remota, tem de selecionar uma das opções de implementação seguintes:

* **Básica:** versão com custo reduzido para demonstração ou para testar implementações. Todos os microsserviços são implementados numa máquina virtual do Azure individual.
* **Standard:** implementação de infraestrutura expandida para desenvolver uma implementação de produção. O Azure Container Service implementa os microsserviços em várias máquinas virtuais do Azure. O Kubernetes orquestra os contentores do Docker que alojam os microsserviços individuais.

### <a name="language-choices-java-and-net"></a>Opções de linguagem: Java e .NET

As implementações de cada uma dos microsserviços estão disponíveis em Java e .NET. Tal como o código .NET, o código de origem Java é aberto e está disponível para personalização, de modo a satisfazer os seus requisitos específicos:

* [Repositório do GitHub de .NET de Monitorização Remota](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet)
* [Repositório do GitHub de Java de Monitorização Remota](https://github.com/Azure/azure-iot-pcs-remote-monitoring-java)

Se quiser ver implementações noutras linguagens, adicione um pedido ao [fórum de utilizadores do Azure IoT](https://feedback.azure.com/forums/321918-azure-iot).

### <a name="react-user-interface-framework"></a>Estrutura da interface de utilizador do React

A IU é criada com a biblioteca de javascript do [React](https://facebook.github.io/react/). O código de origem é aberto e está disponível para transferência e personalização.

## <a name="next-steps"></a>Passos seguintes

Agora que já tem uma noção dos aceleradores de soluções IoT, veja a abaixo os passos seguintes sugeridos para cada um dos aceleradores de soluções:

* [Explorar a solução de Monitorização Remota](iot-suite-remote-monitoring-explore.md).
* [Descrição geral do acelerador de soluções de Manutenção Preditiva](iot-suite-predictive-overview.md).
* [Introdução ao acelerador da solução de Fábrica Ligada](iot-suite-connected-factory-overview.md).

Para obter mais informações sobre as arquiteturas das soluções IoT, veja [Microsoft Azure IoT services: Reference Architecture](http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf) (Serviços do Microsoft Azure IoT: Arquitetura de Referência).
