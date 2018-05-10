---
title: Integrar dados SIM na monitorização solução remota - Azure | Microsoft Docs
description: Este artigo descreve como integrar dados Telefonica SIM a solução de monitorização remota.
services: iot-suite
suite: iot-suite
author: hegate
manager: corywink
ms.author: hegate
ms.service: iot-suite
ms.date: 04/30/2018
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: c82bb8ff3d0f903e1de627f13337ae5fc633458c
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="integrate-sim-data-in-the-remote-monitoring-solution"></a>Integrar dados SIM na solução de monitorização remota

## <a name="overview"></a>Descrição geral
Dispositivos de IoT, muitas vezes, ligam para a nuvem utilizando um cartão SIM, que permite que enviem fluxos de dados a partir de qualquer lugar. A solução de monitorização remota do Azure IoT permite a integração de dados de gestão de SIM, para que os operadores também podem controlar o estado de funcionamento do dispositivo através dos dados fornecidos pelo SIM. Monitorização remota fornece sem a integração de caixa com Telefonica IoT, permitindo que os clientes utilizando a sua plataforma de conectividade de IoT sincronizar o dispositivo SIMs conectividade dados as soluções. Esta solução pode ser expandida para suportar outros fornecedores de telefone da empresa através do repositório do GitHub.
Neste tutorial, ficará a saber como:
* Integrar dados SIM a solução de monitorização remota
* Ver telemetria em tempo real
* Ver SIM dados 

## <a name="telefonica-iot-integration-setup"></a>Configuração de integração Telefonica IoT

### <a name="prerequisites"></a>Pré-requisitos
Para sincronizar os dados de conectividade na solução de monitorização remota do Azure, siga estes passos:

1.  Preencher um pedido no [site da Telefonica](https://iot.telefonica.com/contact), selecione a opção **monitorização remota do Azure**, incluindo os dados de contactos.
2.  Telefonica irá ativar a sua conta. 
3.  Se não for um cliente Telefónica ainda e pretender desfrutar isto ou outros serviços IoT conectividade nuvem pronto, visite [site da Telefonica](https://iot.telefonica.com/contact) e selecione a opção **conectividade**.

### <a name="telefonica-sim-setup"></a>Configuração de Telefonica SIM
Associação de ID de dispositivo telefónica SIM & duplo do Azure irá basear-se na propriedade de "alias" Telefónica IoT SIM. 

Navegue para [Telefónica IoT conectividade plataforma Portal](https://m2m-movistar-es.telefonica.com/) > SIM inventário > selecione o seu SIM e atualizar cada SIM "alias" com o seu deviceID duplo pretendido. 

Esta tarefa também pode ser efetuada no modo em massa (consulte como utilizador de plataforma de conectividade de IoT Telefónica)

![Atualização Telefonica](media/iot-suite-remote-monitoring-telefonica/telefonica_site.png)

Para ligar o seu dispositivo para a monitorização remota, pode seguir estes tutoriais utilizando [C](iot-suite-connecting-devices-linux.md) ou [nó](iot-suite-connecting-devices-node.md). 

## <a name="view-device-telemetry-and-sim-properties"></a>Ver telemetria do dispositivo e propriedades de SIM
Assim que a sua conta Telefonica está configurada corretamente e o seu dispositivo está ligado, pode ver detalhes do dispositivo e os dados SIM.
Os seguintes parâmetros de conectividade podem ser publicados:
* ICCID
* IP
* Presença de rede
* Estado SIM
* Localização de rede
* Tráfego de dados foram consumidos

![Dashboard](media/iot-suite-remote-monitoring-telefonica/dashboard.png)
 
## <a name="next-steps"></a>Passos Seguintes

Agora que tem uma descrição geral sobre como integrar o Azure IoT monitorização remota de dados de SIM, Eis os passos sugeridos Aceleradores de soluções:

* [Utilizar a solução de monitorização remota do Azure IoT](iot-suite-remote-monitoring-explore.md)
* [Realizar a monitorização avançada](iot-suite-remote-monitoring-monitor.md)
* [Gerir os seus dispositivos](iot-suite-remote-monitoring-manage.md)
* [Resolver problemas do dispositivo](iot-suite-remote-monitoring-maintain.md)

