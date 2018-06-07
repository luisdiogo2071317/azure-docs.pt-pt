---
title: Integrar dados SIM na monitorização solução remota - Azure | Microsoft Docs
description: Este artigo descreve como integrar dados Telefónica SIM a solução de monitorização remota.
author: hegate
manager: ''
ms.author: hegate
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 05/15/2018
ms.topic: conceptual
ms.openlocfilehash: ae8751f429cb4b11199bd8da9d8c2e08c0b98b35
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34628288"
---
# <a name="integrate-sim-data-in-the-remote-monitoring-solution"></a>Integrar dados SIM na solução de monitorização remota

Dispositivos de IoT, muitas vezes, ligam para a nuvem utilizando um cartão SIM, que permite que enviem fluxos de dados a partir de qualquer lugar. A solução de monitorização remota do Azure IoT permite a integração de dados de conectividade gerida do IoT, para que os operadores também podem controlar o estado de funcionamento do dispositivo através dos dados fornecidos pelo SIM de IoT.

Monitorização remota fornece sem a integração de caixa com conectividade de IoT Telefónica, permitindo que os clientes utilizando a sua plataforma de conectividade de IoT sincronizar os seus dados de conectividade SIMs para as soluções do dispositivo. Esta solução pode ser expandida para suportar outros fornecedores de conectividade de IoT através do GitHub [repositório](http://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet).

Neste tutorial, ficará a saber como:

* Integrar dados Telefónica IoT SIM a solução de monitorização remota
* Ver telemetria em tempo real
* Ver SIM dados

## <a name="telefnica-iot-integration-setup"></a>Configuração de integração de telefónica IoT

### <a name="prerequisites"></a>Pré-requisitos

Esta funcionalidade adicional de monitorização remota está atualmente em pré-visualização. Para sincronizar os dados de conectividade na solução de monitorização remota do Azure, siga estes passos:

1. Preencher um pedido no [site da Telefónica](https://iot.Telefónica.com/contact), selecione a opção **monitorização remota do Azure**, incluindo os dados de contactos.
2. Telefónica ativa a sua conta.
3. Se não for um cliente Telefónica ainda e pretender desfrutar isto ou outros serviços IoT conectividade nuvem pronto, visite [site da Telefónica](https://iot.Telefónica.com/contact) e selecione a opção **conectividade**.

### <a name="telefnica-sim-setup"></a>Configuração de telefónica SIM
Associação de ID de dispositivo telefónica SIM & duplo Azure baseia-se na propriedade de "alias" Telefónica IoT SIM. 

Navegue para [Telefónica IoT conectividade plataforma Portal](https://m2m-movistar-es.telefonica.com/) > SIM inventário > selecione o seu SIM e atualizar cada SIM "alias" com o seu deviceID duplo pretendido. Esta tarefa também pode ser efetuada no modo em massa (consulte como utilizador de plataforma de conectividade de IoT Telefónica).

Esta tarefa também pode ser efetuada no modo em massa (consulte como utilizador de plataforma de conectividade de IoT Telefónica)

![Atualização telefónica](./media/iot-accelerators-remote-monitoring-telefonica-sim/telefonica_site.png)

Para ligar o seu dispositivo para a monitorização remota, pode seguir estes tutoriais utilizando [C](iot-accelerators-connecting-devices-linux.md) ou [nó](iot-accelerators-connecting-devices-node.md). 

## <a name="view-device-telemetry-and-sim-properties"></a>Ver telemetria do dispositivo e propriedades de SIM

Assim que a sua conta Telefónica está devidamente configurada e o dispositivo estiver ligado, pode ver detalhes do dispositivo e os dados SIM.

Os seguintes parâmetros de conectividade são publicados:

* ICCID
* IP
* Presença de rede
* Estado SIM
* Localização de rede
* Tráfego de dados foram consumidos

![Dashboard](./media/iot-accelerators-remote-monitoring-telefonica-sim/dashboard.png)

## <a name="next-steps"></a>Passos Seguintes

Agora que tem uma descrição geral sobre como integrar o Azure IoT monitorização remota de dados de SIM, Eis os passos sugeridos Aceleradores de soluções:

* [Utilizar a solução de monitorização remota do Azure IoT](iot-accelerators-remote-monitoring-explore.md)
* [Realizar a monitorização avançada](iot-accelerators-remote-monitoring-monitor.md)
* [Gerir os seus dispositivos](iot-accelerators-remote-monitoring-manage.md)
* [Resolver problemas do dispositivo](iot-accelerators-remote-monitoring-maintain.md)

