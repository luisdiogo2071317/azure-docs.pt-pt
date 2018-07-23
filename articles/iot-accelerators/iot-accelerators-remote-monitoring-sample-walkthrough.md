---
title: Remoto monitorização solution accelerator descrição geral - Azure | Documentos da Microsoft
description: Uma visão geral do solution accelerator monitorização remota.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 11/10/2017
ms.author: dobett
ms.openlocfilehash: dfe584532efeab1dbc0d2928b7afb0a6695a21ee
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/20/2018
ms.locfileid: "39184950"
---
# <a name="remote-monitoring-solution-accelerator-overview"></a>Remoto descrição geral do acelerador de solução monitorização

A monitorização remota [acelerador de solução](../iot-accelerators/about-iot-accelerators.md) implementa uma solução de monitorização ponto a ponto para várias máquinas em localizações remotas. A solução combina serviços-chave do Azure para fornecer uma implementação genérica do cenário de negócios. Pode utilizar a solução como um ponto de partida para a sua própria implementação e [personalizar](../iot-accelerators/iot-accelerators-remote-monitoring-customize.md) para atender às suas próprias necessidades comerciais específicas.

Este artigo orienta-o através de alguns dos elementos-chave da solução de monitorização remota para que possa compreender como funciona. Estes conhecimentos ajudam a:

* Resolver problemas na solução.
* Planear a forma de personalizar a solução para satisfazer os seus próprios requisitos específicos.
* Estruturar a sua própria solução de IoT que utiliza os serviços do Azure.

## <a name="logical-architecture"></a>Arquitetura lógica

O diagrama seguinte descreve os componentes lógicos de monitorização remota solution accelerator sobreposto a [arquitetura de IoT](../iot-fundamentals/iot-introduction.md):

![Arquitetura lógica](./media/iot-accelerators-remote-monitoring-sample-walkthrough/remote-monitoring-architecture.png)

## <a name="why-microservices"></a>Por que motivo microsserviços?

Arquitetura de nuvem evoluiu desde o lançamento do Microsoft solution accelerators primeiro. [Microsserviços](https://azure.microsoft.com/blog/microservices-an-application-revolution-powered-by-the-cloud/) surgiram como uma prática comprovada para alcançar dimensionamento e flexibilidade sem sacrificar a rapidez do desenvolvimento. Vários serviços do Microsoft utilizam este padrão de arquitetura internamente com confiabilidade excelente e resultados de escalabilidade. Os Aceleradores de solução atualizada colocar esses aprendizados em prática para que também pode se beneficiar dos mesmos.

> [!TIP]
> Para saber mais sobre as arquiteturas de microsserviços, veja [.NET Application Architecture](https://www.microsoft.com/net/learn/architecture) (Arquitetura de Aplicações .NET) e [Microservices: An application revolution powered by the cloud](https://azure.microsoft.com/blog/microservices-an-application-revolution-powered-by-the-cloud/) (Microsserviços: uma revolução nas aplicações com tecnologia da cloud).

## <a name="device-connectivity"></a>Conectividade dos dispositivos

A solução inclui os seguintes componentes na parte de conectividade do dispositivo da arquitetura lógica:

### <a name="simulated-devices"></a>Dispositivos simulados

A solução inclui um microsserviço que lhe permite gerir um conjunto de dispositivos simulados para testar o fluxo de ponto a ponto da solução. Os dispositivos simulados:

* Gere a telemetria do dispositivo para a cloud.
* Responda às chamadas de método de cloud para dispositivo IoT Hub.

Os microsserviços fornece um ponto de extremidade RESTful para que possa criar, iniciar e parar as simulações. Cada simulação consiste num conjunto de dispositivos virtuais de diferentes tipos, que enviam telemetria e respondem a chamadas de método.

Pode aprovisionar dispositivos simulados a partir do dashboard no portal da solução.

### <a name="physical-devices"></a>Dispositivos físicos

Pode ligar dispositivos físicos à solução. Pode implementar o comportamento dos seus dispositivos simulados com o Azure IoT device SDKs.

Pode aprovisionar dispositivos físicos a partir do dashboard no portal da solução.

### <a name="iot-hub-and-the-iot-manager-microservice"></a>O IoT Hub e os microsserviços de Gestor de IoT

O [IoT hub](../iot-hub/index.yml) ingere dados enviados a partir dos dispositivos para a cloud e disponibiliza-os para o `telemetry-agent` microsserviços.

O hub IoT na solução também:

* Mantém um registo de identidades que armazena os IDs e chaves de autenticação de todos os dispositivos autorizados a ligar para o portal. Pode ativar e desativar dispositivos através do registo de identidades.
* Invoca métodos nos seus dispositivos em nome do portal da solução.
* Mantém dispositivos duplos para todos os dispositivos registados. Um dispositivo duplo armazena os valores de propriedades comunicados por um dispositivo. Um dispositivo duplo também armazena as propriedades pretendidas, definidas no portal da solução, para o dispositivo obter da próxima vez que estabelecer ligação.
* Agenda tarefas para definir propriedades para vários dispositivos ou invocar métodos em vários dispositivos.

A solução inclui o `iot-manager` microsserviços para lidar com as interações com o seu hub IoT, tais como:

* Criar e gerir dispositivos IoT.
* Gestão de dispositivos duplos.
* Invocar métodos nos dispositivos.
* Gerir credenciais de IoT.

Este serviço também é executado o IoT Hub consultas para obter os dispositivos que pertencem a grupos definidos pelo utilizador.

Os microsserviços fornece um ponto de extremidade RESTful para gerir dispositivos e os dispositivos duplos, invocar métodos e executar consultas de IoT Hub.

## <a name="data-processing-and-analytics"></a>Processamento e análise dos dados

A solução inclui os seguintes componentes no processamento de dados e a parte de análise da arquitetura lógica:

### <a name="device-telemetry"></a>Telemetria do dispositivo

A solução inclui dois microsserviços para processar a telemetria do dispositivo.

O [agente de telemetria](https://github.com/Azure/telemetry-agent-dotnet) dos microsserviços:

* Armazena a telemetria no Azure Cosmos DB.
* Analisa o fluxo de telemetria dos dispositivos.
* Gera alarmes, de acordo com as regras definidas.

Os alarmes são armazenados no Azure Cosmos DB.

O [agente de telemetria](https://github.com/Azure/telemetry-agent-dotnet) microsserviços permite que o portal de solução ler a telemetria enviada a partir dos dispositivos. O portal de solução também utiliza este serviço para:

* Definir regras de monitorização, tais como os limiares que acionam alarmes
* Obter a lista de alarmes anteriores.

Utilize o ponto de extremidade RESTful fornecido por esta microsserviços para gerir a telemetria, regras e alarmes.

### <a name="storage"></a>Armazenamento

O [adaptador de armazenamento](https://github.com/Azure/pcs-storage-adapter-dotnet) microsserviço é um adaptador na frente o serviço de armazenamento principal utilizado para o solution accelerator. Ele fornece a coleção simples e o armazenamento de chave-valor.

A implementação padrão do solution accelerator utiliza o Azure Cosmos DB como seu serviço de armazenamento principal.

A base de dados do Azure Cosmos DB armazena dados no solution accelerator. O **adaptador de armazenamento** microsserviços age como um adaptador para os microsserviços a solução para serviços de armazenamento de acesso.

## <a name="presentation"></a>Apresentação

A solução inclui os seguintes componentes na parte da arquitetura lógica de apresentação:

O [interface de utilizador web é um aplicativo de Javascript reagir](https://github.com/Azure/pcs-remote-monitoring-webui). A aplicação:

* Utiliza apenas reagir de Javascript e executada inteiramente no navegador.
* É incluído um estilo com CSS.
* Interage com os microsserviços destinado ao públicos por meio de chamadas do AJAX.

A interface do usuário apresenta todas as funcionalidades do acelerador de solução e interage com outros serviços, tais como:

* O [autenticação](https://github.com/Azure/pcs-auth-dotnet) microsserviços para proteger os dados de utilizador.
* O [iothub-manager](https://github.com/Azure/iothub-manager-dotnet) microsserviços para listar e gerir os dispositivos de IoT.

O [da interface do usuário-config](https://github.com/Azure/pcs-config-dotnet) microsserviços permite que a interface do usuário armazenar e obter as definições de configuração.

## <a name="next-steps"></a>Passos Seguintes

Se quiser explorar a documentação de código e desenvolvedor de origem, comece com um os dois repositórios principais do GitHub:

* [Solution accelerator para monitorização remota com IoT do Azure (.NET)](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/).
* [Solution accelerator para monitorização remota com o Azure IoT (Java)](https://github.com/Azure/azure-iot-pcs-remote-monitoring-java).

Diagramas de arquitetura de solução detalhadas:
* [Solution accelerator para a arquitetura de monitorização remota](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Architecture).

Para obter mais informações concetuais sobre o acelerador de solução de monitorização remota, consulte [personalizar o solution accelerator](../iot-accelerators/iot-accelerators-remote-monitoring-customize.md).
