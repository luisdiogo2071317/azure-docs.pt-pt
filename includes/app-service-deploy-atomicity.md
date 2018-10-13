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
ms.openlocfilehash: 91a4a9ae1d3d84f1396adad07d1cda73ee3747c9
ms.sourcegitcommit: 4047b262cf2a1441a7ae82f8ac7a80ec148c40c4
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/11/2018
ms.locfileid: "49312571"
---
## <a name="what-happens-to-my-app-during-deployment"></a>O que acontece ao meu aplicativo durante a implantação?

Todos os métodos de implementação suportadas oficialmente têm algo em comum: eles fazer alterações aos ficheiros no `/site/home/wwwroot` pasta da sua aplicação. Estes são os mesmos ficheiros que são executados na produção. Por conseguinte, a implementação pode falhar devido a arquivos bloqueados ou a aplicação na produção pode ter um comportamento imprevisível durante a implementação, porque nem todos os ficheiros são atualizados em simultâneo. Existem várias formas de evitar esses problemas:

- Parar a sua aplicação ou ativar o modo offline para a sua aplicação durante a implementação. Para obter mais informações, consulte [Lidando com arquivos bloqueados durante a implementação](https://github.com/projectkudu/kudu/wiki/Dealing-with-locked-files-during-deployment).
- Implementar um [blocos de teste](../articles/app-service/web-sites-staged-publishing.md) com [troca automática](../articles/app-service/web-sites-staged-publishing.md#configure-auto-swap) ativada. 
- Uso [execução do pacote](https://github.com/Azure/app-service-announcements/issues/84) em vez disso.
