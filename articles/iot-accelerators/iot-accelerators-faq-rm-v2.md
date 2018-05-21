---
title: Remoto acelerador de solução de monitorização FAQ | Microsoft Docs
description: Perguntas mais frequentes sobre acelerador de solução de monitorização remota
services: iot-suite
suite: iot-suite
documentationcenter: ''
author: dominicbetts
manager: timlt
editor: ''
ms.assetid: cb537749-a8a1-4e53-b3bf-f1b64a38188a
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/15/2018
ms.author: dobett
ms.openlocfilehash: a65f2a90aedf5aa50016a6c5e5019094f202bf55
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/20/2018
---
# <a name="frequently-asked-questions-for-remote-monitoring-solution-accelerator"></a>Perguntas mais frequentes sobre acelerador de solução de monitorização remota

Além disso, consulte geral [FAQ](iot-accelerators-faq.md).

### <a name="how-much-does-it-cost-to-provision-the-new-remote-monitoring-solution"></a>Quanto-lo de custos para aprovisionar a solução de monitorização remota novo?

O acelerador solução nova oferece duas opções de implementação:

* A *básico* opção concebida para programadores à procura de menor custo de desenvolvimento ou clientes pretende para compilar uma demonstração ou prova de conceito.
* A *padrão* opção concebida para as empresas que pretenda implementar uma infraestrutura pronta para produção.

### <a name="how-can-i-ensure-i-keep-my-costs-down-while-i-develop-my-solution"></a>Como garantir que posso mantenha os meus os custos reduzidos enquanto desenvolva a minha solução?

Além de fornecer duas implementações diferenciadas, a nova solução de monitorização remota tem uma definição para ativar ou desativar a todos os dispositivos simulados a pedido. Desativar a simulação reduz os dados ingeridos na solução e, assim, o custo global.

### <a name="what-is-the-difference-between-the-basic-and-standard-deployment-options-how-do-i-decide-between-the-two-deployment-options"></a>O que é a diferença entre as opções de implementação básico e padrão? Como decidir entre as opções de implementação de dois?

Cada opção de implementação responde às necessidades diferentes. A implementação básica é concebida para começar a desenvolver PoC e pilotos pequenos. Fornece uma arquitetura simplificada com os recursos necessários mínimos e um custo mais baixo. A implementação padrão foi concebida para criar e personalizar uma solução pronta para produção e fornece uma implementação com tenha em atenção que os elementos necessários. Para a fiabilidade e o dimensionamento, micro-serviços de aplicação são criados como contentores de Docker e implementadas utilizando um orchestrator (Kubernetes por predefinição). O orchestrator é responsável pela implementação, dimensionamento e gestão da aplicação. Deverá escolher uma opção com base nas necessidades atuais. Poderá utilizar um, o outro ou uma combinação de ambos, dependendo da fase de projeto.

### <a name="how-do-i-configure-a-dynamic-map-on-the-dashboard"></a>Como configurar a um mapa dinâmico no dashboard

Para obter mais informações, consulte [chave de mapa de atualização para ver os dispositivos num mapa dinâmico](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide#upgrade-map-key-to-see-devices-on-a-dynamic-map).

### <a name="next-steps"></a>Passos Seguintes

Também pode explorar algumas das outras funcionalidades e capacidades dos aceleradores de soluções do IoT:

* [Explore as capacidades do acelerador de solução de monitorização remota](iot-accelerators-remote-monitoring-explore.md)
* [Descrição geral do acelerador de soluções de Manutenção Preditiva](../iot-suite/iot-suite-predictive-overview.md)
* [Descrição geral da ligados da acelerador solução do Factory](iot-accelerators-connected-factory-overview.md)
* [Segurança de IoT a partir do zero](../iot-suite/securing-iot-ground-up.md)
