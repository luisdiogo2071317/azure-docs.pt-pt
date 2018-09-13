---
title: Remotas solução escolhas arquitetónicas da monitorização - Azure | Documentos da Microsoft
description: Este artigo descreve as opções de arquiteturais e técnicas feitas na monitorização remota
author: timlaverty
manager: camerons
ms.author: timlav
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 09/12/2018
ms.topic: conceptual
ms.openlocfilehash: 709886e77819adca961a44f65fe6402dd7d20d53
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/12/2018
ms.locfileid: "44716304"
---
# <a name="remote-monitoring-architectural-choices"></a>Escolhas arquitetónicas da monitorização remotas

O acelerador de solução de monitorização remota do Azure IoT é uma código-fonte aberto, licenciado do MIT, solution accelerator que apresenta os cenários de IoT comuns, como de conectividade do dispositivo, gestão de dispositivos e processamento de fluxo, para que os clientes podem acelerar seu desenvolvimento processo.  A solução de monitorização remota segue a arquitetura de referência do Azure IoT recomendada publicada [aqui](https://aka.ms/iotrefarchitecture).  

Este artigo descreve as opções de arquiteturais e técnicas feitas em cada um dos subsistemas para a solução de monitorização remota e discute alternativas consideradas.  É importante observar que as escolhas de técnicas feitas na solução de monitorização remota não são a única forma de implementar uma solução de IoT de monitorização remota.  A implementação técnica é uma linha de base para a criação de um aplicativo com êxito e deve ser modificada de acordo com as habilidades, experiência e as necessidades de aplicação vertical para uma implementação de solução de cliente.

## <a name="architectural-choices"></a>Opções de arquitetura

### <a name="microservices-serverless-and-cloud-native"></a>Microsserviços, sem servidor e o nativo da cloud

A arquitetura é recomendado para aplicações de IoT são cloud nativos, microsserviços, e com base em sem servidor.  Os diferentes subsistemas de uma aplicação IoT devem ser criados como serviços discretos implementáveis independentemente e capazes de dimensionar de forma independente.  Esses atributos ativar escala maior, mais flexibilidade na atualização subsistemas individuais e fornecem a flexibilidade de escolher a tecnologia apropriada numa base por subsistema.  Microsserviços podem ser implementados com várias tecnologias. Por exemplo, com tecnologia de contentor, como o Docker com a tecnologia sem servidor, como as funções do Azure ou hospedagem microsserviços em serviços de PaaS como os serviços de aplicações do Azure.

## <a name="core-subsystem-technology-choices"></a>Opções de tecnologia do subsistema de núcleo

Esta secção fornece detalhes sobre as opções de tecnologia feitas na solução de monitorização remota para cada um nos principais subsistemas.

![Diagrama de núcleo](./media/iot-accelerators-remote-monitoring-architectural-choices/subsystem.png) 

### <a name="cloud-gateway"></a>Gateway de nuvem
IoT Hub do Azure é utilizado como o gateway de nuvem de solução de monitorização remota.  O IoT Hub oferece comunicação bidirecional e segura com dispositivos. Pode saber mais sobre o IoT Hub [aqui](https://azure.microsoft.com/services/iot-hub/). Para a conectividade do dispositivo de IoT, são utilizados os SDKs do IoT Hub do Java e .NET Core.  Os SDKs oferecem wrappers em torno da API de REST do Hub IoT e manipulam os cenários, tais como as repetições.

### <a name="stream-processing"></a>Processamento de fluxos
Para transmissão em fluxo a solução de monitorização remota de processamento utiliza o Azure Stream Analytics para o processamento da regra complexa.  Para os clientes que desejam regras mais simples, também temos um microsserviço personalizado com suporte para o processamento de regras simples, embora esta configuração não fazem parte de fora da implementação da caixa. A arquitetura de referência recomenda a utilização das funções do Azure para o processamento da regra simples e do Azure Stream Analytics (ASA) para processamento da regra complexa.  

### <a name="storage"></a>Armazenamento
Para armazenamento, o acelerador de solução de monitorização remota utiliza o Azure Time Series Insights e o Azure Cosmos DB. O Azure Time Series Insights armazena as mensagens recebidas pelo IoT Hub dos seus dispositivos ligados. O solution accelerator utiliza o Azure Cosmos DB para todos os outro armazenamento como armazenamento esporádico, definições de regras, alarmes e definições de configuração. O Azure Cosmos DB é a solução de armazenamento de acesso pouco frequente recomendado para fins gerais para aplicações de IoT, embora soluções como o Azure Time Series Insights e o Azure Data Lake são adequadas para muitos casos de utilização. Com o Azure Time Series Insights pode obter informações mais detalhadas sobre os seus dados de séries temporais sensor ao detetar tendências e anomalias, que permite realizar análises de causa raiz e evitar períodos de indisponibilidade dispendiosos. 

> [!NOTE]
> O Azure Time Series Insights para o acelerador de solução de monitorização remota está atualmente em pré-visualização e só está disponível no [selecionar regiões](https://azure.microsoft.com/global-infrastructure/services/). Se implementar o acelerador de solução monitorização remota fora nestas regiões, o Cosmos DB é a opção de armazenamento predefinida.

### <a name="business-integration"></a>Integração de negócios
Integração de negócios na solução de monitorização remota está limitada a geração de alarmes, que são colocadas no armazenamento de acesso pouco frequente. Ainda mais integrações de negócios podem ser executadas ao integrar a solução no Azure Logic Apps.

### <a name="user-interface"></a>Interface do usuário
O web IU é criada com reagir de JavaScript.  React oferece uma estrutura de interface do Usuário da web de setor frequentemente utilizadas e é semelhante a outras arquiteturas populares, como o Angular.  

### <a name="runtime-and-orchestration"></a>Tempo de execução e orquestração
O tempo de execução do aplicativo escolhido para implementação de subsistema na solução de monitorização remota é contentores do Docker com o Kubernetes como o orchestrator para o dimensionamento horizontal.  Esta arquitetura permite para definição de dimensionamento individuais por subsistema entretanto incorre em custos de DevOps em manter atualizados VMs e contentores de uma perspectiva de segurança.  As alternativas para o Docker e Kubernetes incluem alojamento microsserviços em serviços de PaaS (por exemplo, serviço de aplicações do Azure) ou utilizar o Service Fabric, DCOS, Swarm, etc. como um orquestrador.

## <a name="next-steps"></a>Passos Seguintes
* Implemente a sua solução de monitorização remota [aqui](https://www.azureiotsolutions.com/).
* Explore o código do GitHub na [c#](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/) e [Java](https://github.com/Azure/azure-iot-pcs-remote-monitoring-java/).  
* Saiba mais sobre a arquitetura de referência do IoT [aqui](https://aka.ms/iotrefarchitecture).
