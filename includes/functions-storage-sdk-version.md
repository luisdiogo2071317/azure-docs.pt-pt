---
title: incluir ficheiro
description: incluir ficheiro
services: functions
author: tdykstra
manager: cfowler
ms.service: functions
ms.topic: include
ms.date: 05/17/2018
ms.author: tdykstra
ms.custom: include file
ms.openlocfilehash: 97c56b07833f7d93541bb0b3747889f5a50a8203
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34675280"
---
### <a name="azure-storage-sdk-version-in-functions-1x"></a>Versão do SDK de armazenamento do Azure nas funções de 1. x

As funções, 1. x, os acionadores de armazenamento e os enlaces de utilizam a versão 7.2.1 do SDK de armazenamento do Azure ([Windowsazure](https://www.nuget.org/packages/WindowsAzure.Storage/7.2.1) pacote NuGet). Se referenciar uma versão diferente do SDK de armazenamento e vincular a um tipo de SDK de armazenamento na sua assinatura da função, o tempo de execução de funções poderá reportar que não é possível vincular desse tipo. A solução é certificar-se de que as referências do projeto [Windowsazure 7.2.1](https://www.nuget.org/packages/WindowsAzure.Storage/7.2.1).
