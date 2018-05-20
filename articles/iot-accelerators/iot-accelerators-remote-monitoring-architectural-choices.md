---
title: Remotas monitorização solução da arquitetura escolhas - Azure | Microsoft Docs
description: Este artigo descreve as opções de arquitetural e técnicas efetuadas na monitorização remota
services: iot-suite
suite: iot-suite
author: timlaverty
manager: camerons
ms.author: timlav
ms.service: iot-suite
ms.date: 04/30/2018
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: 607b8aeb2f986eebddf8fe13b88e7f3bc7b4494b
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/17/2018
---
# <a name="remote-monitoring-architectural-choices"></a>Opções da arquitetura de monitorização remotas

O Azure IoT remoto monitorização (RM) é um open source, MIT licenciado, acelerador de solução apresenta cenários comuns do IoT, tais como a conectividade do dispositivo, gestão de dispositivos e o processamento de fluxo, pelo que os clientes podem acelerar o seu processo de desenvolvimento.  RM segue recomendada do Azure IoT arquitetura de referência publicada [aqui](https://azure.microsoft.com/updates/microsoft-azure-iot-reference-architecture-available/).  

Este artigo descreve as opções de arquitetural e técnicas tomadas em cada um dos subsistemas de RM e descreve alternativas consideradas.  É importante ter em atenção que as escolhas técnicas efetuadas no RM não estão a única forma de implementar uma solução de IoT de monitorização remota.  A implementação técnica é uma linha de base para a criação de uma aplicação com êxito e deverá ser modificada para caber a competências, experiência e necessidades de aplicação vertical para uma implementação de soluções do cliente.

## <a name="architectural-choices"></a>Opções da arquitetura

### <a name="microservices-serverless-and-cloud-native"></a>Micro-serviços, sem servidor e na nuvem nativo

A arquitetura Recomendamos para aplicações IoT são nuvem nativo, microsserviço, e com sem servidor de base.  Os diferentes subsistemas de uma aplicação de IoT devem ser criados como serviços discretos independentemente implementável e dimensionar de forma independente.  Estes atributos ativar escala maior, uma maior flexibilidade na atualização subsistemas individuais e fornecem a flexibilidade para escolher a tecnologia adequada numa base por subsistema.  Micro-serviços podem ser implementados com várias tecnologias. Por exemplo, utilizando a tecnologia de contentor, tais como Docker com tecnologia sem servidor, como as funções do Azure, ou ainda o alojamento micro-serviços nos serviços de PaaS, como os serviços de aplicações do Azure.

## <a name="core-subsystem-technology-choices"></a>Opções de tecnologia de subsistema de núcleo

Esta secção fornece detalhes sobre as opções de tecnologia efetuadas RM para cada um dos subsistemas de núcleos.

![Diagrama de núcleo](./media/iot-accelerators-remote-monitoring-architectural-choices/subsystem.png) 

### <a name="cloud-gateway"></a>Gateway de nuvem
IoT Hub do Azure é utilizado como o Gateway de nuvem RM.  O IoT Hub oferece comunicação segura, bidirecional com dispositivos. Pode saber mais sobre o IoT Hub [aqui](https://azure.microsoft.com/services/iot-hub/). Conectividade do dispositivo IoT, são utilizados os .NET Core e o Java SDKs do IoT Hub.  Os SDKs oferecem wrappers em torno os cenários de API de REST do IoT Hub e o identificador como repetição, 

### <a name="stream-processing"></a>Processamento de fluxos
Para RM de processamento de fluxo utiliza o Azure Stream Analytics para o processamento da regra complexas.  Para os clientes que regras mais simples, também temos um microsserviço personalizado com suporte para processamento de regras simples, apesar desta configuração não fazem parte da fora da implementação caixa. Arquitetura de referência recomenda a utilização das funções do Azure para o processamento da regra simples e o Azure Stream Analytics (ASA) para processamento da regra complexas.  

### <a name="storage"></a>Armazenamento
Para armazenamento, base de dados do Cosmos é utilizada para todas as necessidades de armazenamento: armazenamento de frio, transfira armazenamento, armazenamento de regras e alarmes. Estamos atualmente a em vias de mudar para o armazenamento de Blobs do Azure, tal como recomendado pela arquitetura de referência.  Cosmos DB é a solução de armazenamento de transfira para fins gerais recomendada para aplicações de IoT apesar de soluções, tais como informações de séries de tempo do Azure e do Azure Data Lake são adequadas para muitos casos de utilização.

### <a name="business-integration"></a>Integração de negócios
Integração de negócios no RM está limitada a geração de alarmes, que são colocados no armazenamento transfira. Integrações de negócio podem ser efetuadas mais ao integrar a solução com Azure Logic Apps.

### <a name="user-interface"></a>Interface de utilizador
Web IU baseia-se com reagir JavaScript.  Reagir oferece uma estrutura de IU da web de setor frequentemente utilizadas e é semelhante a outras arquiteturas populares, tais como Angular.  

### <a name="runtime-and-orchestration"></a>Tempo de execução e de orquestração
O Runtime de aplicação escolhido para implementação de subsistema em RM é contentores de Docker com Kubernetes (K8s) como o orchestrator para dimensionamento horizontal.  Esta arquitetura permite de definição de dimensionamento individuais por subsistema contudo implica os custos de DevOps manter atualizado VMs e contentores de uma perspetiva de segurança.  As alternativas para Docker & K8s incluem alojamento micro-serviços nos serviços de PaaS (por exemplo, App Service do Azure) ou utilizando o Service Fabric, DCOS, Swarm, etc. como do orchestrator.

## <a name="next-steps"></a>Passos Seguintes
* Implementar a sua solução RM [aqui](https://www.azureiotsuite.com/).
* Explore um código no GitHub [c#](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/) e [Java](https://github.com/Azure/azure-iot-pcs-remote-monitoring-java/).  
* Saiba mais sobre a arquitetura de referência do IoT [aqui](https://azure.microsoft.com/updates/microsoft-azure-iot-reference-architecture-available/).
