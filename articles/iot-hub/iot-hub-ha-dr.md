---
title: O Azure IoT Hub elevada disponibilidade e recuperação após desastre | Documentos da Microsoft
description: Descreve os recursos do Azure e o IoT Hub que ajudam a criar soluções de IoT do Azure de elevada disponibilidade com que desastre capacidades de recuperação.
author: fsautomata
manager: ''
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 10/13/2017
ms.author: elioda
ms.openlocfilehash: 992c42511f7bc9e9af71ff552ee91bc2472ebcf8
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/20/2018
ms.locfileid: "39185708"
---
# <a name="iot-hub-high-availability-and-disaster-recovery"></a>IoT Hub elevada disponibilidade e recuperação após desastre
Como um serviço do Azure, o IoT Hub fornece elevada disponibilidade (HA) com redundâncias ao nível da região do Azure, sem qualquer trabalho adicional necessário para a solução. A plataforma Microsoft Azure também inclui recursos para ajudar a criar soluções com capacidades de recuperação (DR) após desastre ou entre regiões de disponibilidade. Se quiser fornecer global, entre regiões de elevada disponibilidade para dispositivos ou utilizadores, tirar partido destas funcionalidades de DR do Azure. O artigo [orientações técnicas de continuidade do negócio de Azure](../resiliency/resiliency-technical-guidance.md) descreve as funcionalidades incorporadas no Azure para a continuidade do negócio e de DR. O [recuperação após desastre e elevada disponibilidade para aplicações do Azure] [ Disaster recovery and high availability for Azure applications] documento fornece orientações de arquitetura sobre estratégias para aplicações do Azure alcançar a HA e DR.

## <a name="azure-iot-hub-dr"></a>DR de IoT Hub do Azure
Além de intra-região HA, IoT Hub implementa os mecanismos de ativação pós-falha para recuperação após desastre que não exigem nenhuma intervenção do usuário. IoT Hub DR automática é iniciada e tem um objetivo de tempo de recuperação (RTO) de 26 de 2 horas e os seguintes objetivos de ponto de recuperação (RPOs):

| Funcionalidade | RPO |
| --- | --- |
| Disponibilidade do serviço para operações de registo e comunicação |Possíveis perdas de CName |
| Dados de identidade no registo de identidade |0 a 5 minutos perda de dados |
| Mensagens do dispositivo para a cloud |Todas as mensagens não lidas são perdidas |
| Mensagens de monitorização de operações |Todas as mensagens não lidas são perdidas |
| Mensagens da cloud para dispositivo |0 a 5 minutos perda de dados |
| Fila de comentários do cloud-para-dispositivo |Todas as mensagens não lidas são perdidas |
| Dados do dispositivo duplo |0 a 5 minutos perda de dados |
| Tarefas principais e de dispositivo |0 a 5 minutos perda de dados |

## <a name="regional-failover-with-iot-hub"></a>Ativação pós-falha com o IoT Hub
Um tratamento completo das topologias de implementação em soluções de IoT está fora do escopo deste artigo. O artigo aborda os *ativação pós-falha regional* modelo de implementação para fins de elevada disponibilidade e recuperação após desastre.

Num modelo de ativação pós-falha, a solução, faça uma cópia final execuções principalmente num único datacenter local. Um hub de IoT secundário e o back-end são implementadas em outro datacenter local. Se o hub IoT no datacenter primário sofre uma falha ou a conectividade de rede do dispositivo para o datacenter primário for interrompida, os dispositivos utilizam um ponto de extremidade de serviço secundário. Pode melhorar a disponibilidade da solução através da implementação de um modelo de ativação pós-falha entre regiões, em vez de manter-se sempre numa única região. 

Num alto nível, implementar um modelo de ativação pós-falha com o IoT Hub, precisa do seguinte:

* **Um IoT hub e lógica de roteamento de dispositivo secundário**: se o serviço na sua região primária é interrompido, dispositivos tem de começar a ligar para a região secundária. Devido à natureza com deteção de estado da maioria dos serviços de envolvidos, é comum para os administradores de solução acionar o processo de ativação pós-falha entre regiões. A melhor maneira para comunicar o novo ponto final para dispositivos, mantendo o controle do processo, é tê-los verificar regularmente uma *concierge* serviço para o ponto final atual do Active Directory. O serviço de concierge pode ser uma aplicação web que é replicada e mantida acessível usando técnicas de redirecionamento de DNS (por exemplo, utilizando [Gestor de tráfego do Azure][Azure Traffic Manager]).
* **Replicação do registo de identidade**: possam para ser usados, o hub IoT secundário tem de conter todas as identidades de dispositivos que podem ligar à solução. A solução deve manter cópias de segurança georreplicado de identidades de dispositivos e carregá-los para o hub IoT secundário antes de mudar o ponto final do Active Directory para os dispositivos. A funcionalidade de exportação de identidade de dispositivo do IoT Hub é útil neste contexto. Para obter mais informações, consulte [Guia do programador do IoT Hub - registo de identidade][IoT Hub developer guide - identity registry].
* **Lógica de mesclagem**: quando a região primária fique disponível novamente, tudo o estado e os dados que foram criados no site secundário tem de ser migrados de volta para a região primária. Este estado e os dados em grande parte relacionadas com identidades do dispositivo e metadados de aplicação, que devem ser mesclados com o principal hub de IoT e outros arquivos de específicas da aplicação na região primária. Para simplificar este passo, deve usar operações idempotentes. Operações Idempotentes minimizar os efeitos de colaterais de distribuição eventual consistente de eventos e de duplicados ou fora de ordem entrega de eventos. Além disso, a lógica do aplicativo deve ser concebida para tolerar possíveis inconsistências ou o estado "ligeiramente" desatualizado. Esta situação pode ocorrer devido ao tempo adicional necessário para o sistema "tratá-lo" com base nos objetivos de ponto de recuperação (RPO).

## <a name="next-steps"></a>Passos Seguintes
Siga estas ligações para saber mais sobre o IoT Hub do Azure:

* [Introdução aos Hubs de IoT (Tutorial)][lnk-get-started]
* [O que é o IoT Hub do Azure?][What is Azure IoT Hub?]

[Disaster recovery and high availability for Azure applications]: ../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md
[Azure Business Continuity Technical Guidance]: https://azure.microsoft.com/documentation/articles/resiliency-technical-guidance/
[Azure Traffic Manager]: https://azure.microsoft.com/documentation/services/traffic-manager/
[IoT Hub developer guide - identity registry]: iot-hub-devguide-identity-registry.md

[lnk-get-started]: quickstart-send-telemetry-dotnet.md
[What is Azure IoT Hub?]: about-iot-hub.md
