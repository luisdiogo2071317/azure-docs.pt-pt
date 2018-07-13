---
title: Migrar do IoT Hub do Azure para as definições de diagnóstico | Documentos da Microsoft
description: Como atualizar o IoT Hub do Azure para utilizar as definições de diagnóstico do Azure em vez de operações de monitorização para monitorizar o estado das operações no hub IoT em tempo real.
author: kgremban
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 10/10/2017
ms.author: kgremban
ms.openlocfilehash: 85bdb4b4802283c591e4d7a9e8b14ae74fa44e8d
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38666727"
---
# <a name="migrate-your-iot-hub-from-operations-monitoring-to-diagnostics-settings"></a>Migrar o seu IoT Hub a partir de operações de monitorização para as definições de diagnóstico

Clientes que utilizam [monitorização de operações] [ lnk-opsmon] controlar o estado das operações no IoT Hub pode migrar esse fluxo de trabalho [definições de diagnóstico do Azure] [ lnk-diagnostics-settings], uma funcionalidade do Azure Monitor. As definições de diagnóstico fornecem informações de diagnóstico ao nível do recurso para muitos serviços do Azure.

As operações de funcionalidade de monitorização do IoT Hub foi preterida e será removida no futuro. Este artigo fornece passos para mover as cargas de trabalho de monitorização para as definições de diagnóstico de operações. Para obter mais informações sobre a linha cronológica de preterição, consulte [monitorizar as soluções de IoT do Azure com o Azure Monitor e o Azure Resource Health][lnk-blog-announcement].

## <a name="update-iot-hub"></a>Atualizar o IoT Hub

Para atualizar o seu IoT Hub no portal do Azure, primeiro ative as definições de diagnóstico, em seguida, desative a monitorização de operações.  

[!INCLUDE [iot-hub-diagnostics-settings](../../includes/iot-hub-diagnostics-settings.md)]

### <a name="turn-off-operations-monitoring"></a>Desativar a monitorização de operações

Depois de ter testado as novas definições de diagnóstico no seu fluxo de trabalho, pode desativar as funcionalidade de monitorização de operações. 

1. No menu do IoT Hub, selecione **monitorização de operações**.
1. Em cada categoria de monitorização, selecione **None**.
1. Guarde as operações de monitorização das alterações.

## <a name="update-applications-that-use-operations-monitoring"></a>Atualizar aplicações que utilizam a monitorização de operações

Os esquemas para a monitorização de operações e as definições de diagnóstico são ligeiramente diferentes. É importante que Atualize as aplicações que utilizam atuais de monitorização de operações para mapear para o esquema usado pelas definições de diagnóstico. 

Além disso, diagnósticos definições ofertas de controlo para cinco categorias de novo. Depois de Atualizar aplicativos para o esquema existente, adicione as novas categorias:

- Operações de cloud para o dispositivo duplo
- Operações de gémeos de dispositivo para a cloud
- Consultas de gémeos
- Operações de tarefas
- Métodos diretos

Para as estruturas de esquema específico, consulte [compreender o esquema para definições de diagnóstico][lnk-diagnostics-schema].

## <a name="next-steps"></a>Passos Seguintes

- [Monitorizar o estado de funcionamento do IoT Hub do Azure e diagnosticar problemas rapidamente][lnk-monitor]

[lnk-opsmon]: iot-hub-operations-monitoring.md
[lnk-diagnostics-settings]: ../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md
[lnk-diagnostics-schema]: iot-hub-monitor-resource-health.md#understand-the-logs
[lnk-blog-announcement]: https://azure.microsoft.com/blog/monitor-your-azure-iot-solutions-with-azure-monitor-and-azure-resource-health/
[lnk-monitor]: iot-hub-monitor-resource-health.md
