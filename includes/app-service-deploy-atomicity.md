---
title: incluir ficheiro
description: incluir ficheiro
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 06/08/2018
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: ac2cf4d688b1bdc54ed2d7341f0e195d3b2fe42d
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/08/2018
ms.locfileid: "35236474"
---
## <a name="what-happens-to-my-app-during-deployment"></a>O que acontece para a minha aplicação durante a implementação?

Todos os métodos de implementação oficialmente suportado tem um aspeto em comum: efetuam as alterações aos ficheiros no `/site/home/wwwroot` pasta da sua aplicação. Estes são os mesmos ficheiros que são executados em produção. Por conseguinte, a implementação pode falhar devido a ficheiros bloqueados ou a aplicação na produção pode ter um comportamento imprevisível durante a implementação, porque nem todos os ficheiros são atualizados em simultâneo. Existem várias formas para evitar estes problemas:

- Parar a sua aplicação ou ativar o modo offline para a sua aplicação durante a implementação. Para obter mais informações, consulte [lidar com ficheiros bloqueados durante a implementação](https://github.com/projectkudu/kudu/wiki/Dealing-with-locked-files-during-deployment).
- Implementar um [testes ranhura](../articles/app-service/web-sites-staged-publishing.md) com [troca automática](../articles/app-service/web-sites-staged-publishing.md#configure-auto-swap) ativada. 
- Utilize [ser executada a partir do Zip](https://github.com/Azure/app-service-announcements/issues/84) em vez disso.
