---
title: Como criar um espaço de nomes do Service Bus no portal do Azure | Microsoft Docs
description: Crie um espaço de nomes do Service Bus com o portal do Azure.
services: service-bus-messaging
documentationcenter: .net
author: axisc
manager: timlt
editor: spelluru
ms.assetid: fbb10e62-b133-4851-9d27-40bd844db3ba
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: get-started-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 02/12/2019
ms.author: aschhab
ms.openlocfilehash: 632ef45d4db5de03369e0abb8b16590911bdffdb
ms.sourcegitcommit: de81b3fe220562a25c1aa74ff3aa9bdc214ddd65
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2019
ms.locfileid: "56233307"
---
# <a name="create-a-service-bus-namespace-using-the-azure-portal"></a>Criar um espaço de nomes do Service Bus com o portal do Azure

O espaço de nomes é um contentor para todos os componentes de mensagens. Um único espaço de nomes pode conter várias filas e tópicos, sendo que os espaços de nomes servem frequentemente de contentores da aplicação. Existem duas formas de criar um espaço de nomes do Service Bus:

1. Portal do Azure (este artigo)
2. [Modelos do Resource Manager][create-namespace-using-arm]

## <a name="create-a-namespace-in-the-azure-portal"></a>Criar um espaço de nomes no portal do Azure

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

Parabéns! Criou um espaço de nomes de Mensagens do Service Bus.

## <a name="next-steps"></a>Passos seguintes

Veja os [exemplos do GitHub][github-samples] do Service Bus, que mostram algumas das funcionalidades mais avançadas das mensagens do Service Bus.

[create-namespace-using-arm]: service-bus-resource-manager-overview.md
[github-samples]: https://github.com/Azure/azure-service-bus/tree/master/samples
