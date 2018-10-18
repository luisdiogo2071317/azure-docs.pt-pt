---
title: incluir ficheiro
description: incluir ficheiro
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: include
ms.custom: include file
ms.date: 09/27/2018
ms.author: diberry
ms.openlocfilehash: 420757c5b7f6d19bb0abe87232e75497b7b97322
ms.sourcegitcommit: 7bc4a872c170e3416052c87287391bc7adbf84ff
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/02/2018
ms.locfileid: "48019114"
---
1. Abra o Visual Studio 2017 Community Edition.
1. Crie um novo projeto de **Aplicação de Consola (.NET Core)** e designe-o `QnaMakerQuickstart`. Aceite as predefinições nas restantes definições.
1. No Explorador de Soluções, clique com o botão direito do rato no nome do projeto, **QnaMakerQuickstart**, e selecione **Gerir Pacotes NuGet...**
1. Na janela do NuGet, selecione **Browser**, procure **Newtonsoft.JSON** e instale o pacote. Este pacote é utilizado para analisar o JSON devolvido da resposta HTTP do QnaMaker. 