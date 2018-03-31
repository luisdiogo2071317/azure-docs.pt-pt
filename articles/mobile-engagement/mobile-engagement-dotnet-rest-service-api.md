---
title: Utilizando a API REST para aceder a APIs de serviço do Azure Mobile Engagement
description: Descreve como utilizar as APIs REST do Mobile Engagement para aceder às APIs de serviço do Azure Mobile Engagement
services: mobile-engagement
documentationcenter: mobile
author: wesmc7777
manager: erikre
editor: ''
ms.assetid: e8df4897-55ee-45df-b41e-ff187e3d9d12
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 10/05/2016
ms.author: wesmc;ricksal
ms.openlocfilehash: c3c0de39ba13ed47345b1042a9dceda075dc50b3
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/30/2018
---
# <a name="using-rest-to-access-azure-mobile-engagement-service-apis"></a>Utilizar REST para aceder a APIs de serviço do Azure Mobile Engagement
> [!IMPORTANT]
> O Azure Mobile Engagement ter extinguido em 3/31/2018. Esta página será eliminada pouco tempo depois.
> 

O Azure Mobile Engagement fornece o [API de REST do Azure Mobile Engagement](https://msdn.microsoft.com/library/azure/mt683754.aspx) para gerir dispositivos, campanhas de alcance/enviar por Push etc.

> [!NOTE]
> O serviço Azure Mobile Engagement será extinto em março de 2018 e, atualmente, apenas está disponível para os clientes existentes. Para obter mais informações, veja [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).

Se não pretender utilizar as APIs REST diretamente, fornecemos também uma [ficheiro Swagger](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-mobileengagement/2014-12-01/swagger/mobile-engagement.json) que pode utilizar com as ferramentas para gerar SDKs para o idioma preferencial. Recomendamos que utilize o [AutoRest](https://github.com/Azure/AutoRest) ferramenta para gerar o SDK do nosso ficheiro Swagger. Foi criado um SDK .NET de forma semelhante, que lhe permite interagir com estas APIs utilizando um wrapper c# e não tiver a negociação de token de autenticação e atualize si próprio. Consulte [exemplo de SDK do serviço de API .NET](mobile-engagement-dotnet-sdk-service-api.md) para aprender a utilizar o SDK do .net API
