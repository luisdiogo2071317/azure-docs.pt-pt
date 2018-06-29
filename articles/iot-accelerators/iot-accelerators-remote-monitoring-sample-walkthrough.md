---
title: Remoto monitorização acelerador descrição geral da solução - Azure | Microsoft Docs
description: Uma descrição geral do acelerador de solução de monitorização remota.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 11/10/2017
ms.author: dobett
ms.openlocfilehash: a8b5d9e3917c854cb255a35d3bbc901bcce52c24
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/28/2018
ms.locfileid: "37084535"
---
# <a name="remote-monitoring-solution-accelerator-overview"></a>Monitorização solução acelerador descrição geral remoto

Monitorização remota [acelerador solução](../iot-accelerators/iot-accelerators-what-are-solution-accelerators.md) implementa uma solução de monitorização ponto a ponto para várias máquinas em localizações remotas. A solução combina serviços-chave do Azure para fornecer uma implementação genérica do cenário de negócios. Pode utilizar a solução como um ponto de partida para a sua própria implementação e [personalizar](../iot-accelerators/iot-accelerators-remote-monitoring-customize.md) para satisfazer os seus requisitos empresariais específicos.

Este artigo explica alguns dos elementos-chave da solução de monitorização remota para permitir a compreender como funciona. Estes conhecimentos ajudam a:

* Resolver problemas na solução.
* Planear a forma de personalizar a solução para satisfazer os seus próprios requisitos específicos.
* Estruturar a sua própria solução de IoT que utiliza os serviços do Azure.

## <a name="logical-architecture"></a>Arquitetura lógica

O diagrama a seguir descreve os componentes lógicos do acelerador de solução de monitorização remota overlaid no [arquitetura IoT](../iot-accelerators/iot-accelerators-what-is-azure-iot.md):

![Arquitetura lógica](./media/iot-accelerators-remote-monitoring-sample-walkthrough/remote-monitoring-architecture.png)

## <a name="why-microservices"></a>Por que motivo micro-serviços?

Arquitetura de nuvem tem evoluiu e deu lugar, uma vez que a Microsoft lançou as primeira Aceleradores de solução. [Micro-serviços](https://azure.microsoft.com/blog/microservices-an-application-revolution-powered-by-the-cloud/) ter emerged como prática comprovada para alcançar a escala e a flexibilidade sem sacrificar a velocidade de desenvolvimento. Vários serviços do Microsoft utilizam este padrão da arquitetura internamente com resultados de escalabilidade e fiabilidade excelente. Os Aceleradores solução atualizado colocar estes learnings na prática, para que possa também tirar partido dos mesmos.

> [!TIP]
> Para saber mais sobre as arquiteturas de microsserviços, veja [.NET Application Architecture](https://www.microsoft.com/net/learn/architecture) (Arquitetura de Aplicações .NET) e [Microservices: An application revolution powered by the cloud](https://azure.microsoft.com/blog/microservices-an-application-revolution-powered-by-the-cloud/) (Microsserviços: uma revolução nas aplicações com tecnologia da cloud).

## <a name="device-connectivity"></a>Conectividade dos dispositivos

A solução inclui os seguintes componentes da parte de conectividade do dispositivo da arquitetura lógica:

### <a name="simulated-devices"></a>Dispositivos simulados

A solução inclui um microsserviço que lhe permite gerir um conjunto de dispositivos simulados para testar o fluxo de ponto a ponto na solução. Os dispositivos simulados:

* Gere telemetria do dispositivo para a nuvem.
* Responda a chamadas de método de nuvem para o dispositivo a partir do IoT Hub.

O microsserviço fornece um ponto final RESTful para que possa criar, iniciar e parar simulações. Cada simulação é composta por um conjunto de dispositivos virtuais de diferentes tipos que enviar telemetria e respondem a chamadas de método.

Pode aprovisionar dispositivos simulados a partir do dashboard no portal de solução.

### <a name="physical-devices"></a>Dispositivos físicos

Pode ligar dispositivos físicos à solução. Pode implementar o comportamento dos seus dispositivos simulados com os SDKs do dispositivo IoT do Azure.

Pode aprovisionar dispositivos físicos a partir do dashboard no portal de solução.

### <a name="iot-hub-and-the-iot-manager-microservice"></a>IoT Hub e o microsserviço do Gestor de IoT

O [IoT hub](../iot-hub/index.yml) ingere dados enviados a partir dos dispositivos para a nuvem e torna a mesma disponível para o `telemetry-agent` microsserviço.

O hub IoT na solução também:

* Mantém um registo de identidade que armazena os IDs e chaves de autenticação de todos os dispositivos autorizadas a ligar para o portal. Pode ativar e desativar dispositivos através do registo de identidades.
* Invoca métodos nos seus dispositivos em nome do portal da solução.
* Mantém dispositivos duplos para todos os dispositivos registados. Um dispositivo duplo armazena os valores de propriedades comunicados por um dispositivo. Um dispositivo duplo também armazena as propriedades pretendidas, definidas no portal da solução, para o dispositivo obter da próxima vez que estabelecer ligação.
* Agenda tarefas para definir propriedades para vários dispositivos ou invocar métodos em vários dispositivos.

A solução inclui o `iot-manager` microsserviço para processar as interações com o seu hub IoT, tais como:

* Criar e gerir os dispositivos de IoT.
* Gestão de dispositivos duplos.
* Invocar métodos em dispositivos.
* Gestão de credenciais de IoT.

Este serviço também é executado o IoT Hub consultas para obter os dispositivos que pertencem a grupos definidos pelo utilizador.

O microsserviço fornece um ponto de final RESTful para gerir dispositivos e os dispositivos duplos, invocar métodos e executar consultas de IoT Hub.

## <a name="data-processing-and-analytics"></a>Processamento e análise dos dados

A solução inclui os seguintes componentes no processamento de dados e parte de análise da arquitetura lógica:

### <a name="device-telemetry"></a>Telemetria do dispositivo

A solução inclui dois micro-serviços para processar a telemetria do dispositivo.

O [telemetria-agent](https://github.com/Azure/telemetry-agent-dotnet) microsserviço:

* Armazena a telemetria do BD Azure Cosmos.
* Analisa o fluxo de telemetria dos dispositivos.
* Gera alarmes, de acordo com as regras definidas.

Os alarmes são armazenados na base de dados do Azure Cosmos.

O [telemetria-agent](https://github.com/Azure/telemetry-agent-dotnet) microsserviço permite que o portal de solução ler a telemetria enviada a partir dos dispositivos. O portal de solução também utiliza este serviço para:

* Definir regras de monitorização, tais como os limiares que acionam alarmes
* Obter a lista de alarmes passados.

Utilize o ponto de final RESTful fornecido por este microsserviço para gerir telemetria, regras e alarmes.

### <a name="storage"></a>Armazenamento

O [adaptador de armazenamento](https://github.com/Azure/pcs-storage-adapter-dotnet) microsserviço é um adaptador à frente o serviço de armazenamento principal utilizado para o acelerador de solução. Proporciona uma coleção simples e o armazenamento de chave-valor.

A implementação padrão do acelerador solução utiliza a BD do Cosmos Azure como um serviço de armazenamento principal.

A base de dados de base de dados do Azure Cosmos armazena dados no acelerador de solução. O **adaptador de armazenamento** microsserviço age como um adaptador para outros micro-serviços na solução de acesso nos serviços de armazenamento.

## <a name="presentation"></a>Apresentação

A solução inclui os seguintes componentes da parte de apresentação da arquitetura lógica:

O [interface de utilizador web é uma aplicação do Javascript reagir](https://github.com/Azure/pcs-remote-monitoring-webui). A aplicação:

* Utiliza apenas Javascript reagir e é executada inteiramente no browser.
* É escovado com CSS.
* Interage com micro-serviços destinado ao públicos através de chamadas AJAX.

A interface de utilizador apresenta todas as funcionalidades de acelerador de solução e interage com outros serviços, tais como:

* O [autenticação](https://github.com/Azure/pcs-auth-dotnet) microsserviço para proteger os dados de utilizador.
* O [iothub-manager](https://github.com/Azure/iothub-manager-dotnet) microsserviço para listar e gerir os dispositivos de IoT.

O [IU-config](https://github.com/Azure/pcs-config-dotnet) microsserviço permite que a interface de utilizador armazenar e obter as definições de configuração.

## <a name="next-steps"></a>Passos Seguintes

Se pretende explorar a documentação de código e o programador da origem, comece com um repositórios do GitHub principais duas:

* [Acelerador de solução de monitorização remota com o Azure IoT (.NET)](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/).
* [Acelerador de solução de monitorização remota com o Azure IoT (Java)](https://github.com/Azure/azure-iot-pcs-remote-monitoring-java).

Diagramas de arquitetura de solução detalhadas:
* [Acelerador de solução de monitorização remota arquitetura](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Architecture).

Para obter mais informações concetuais sobre o acelerador de solução de monitorização remota, consulte [personalizar o acelerador solução](../iot-accelerators/iot-accelerators-remote-monitoring-customize.md).
