---
title: Exportar ou eliminar os seus dados - serviço de visão personalizada
titlesuffix: Azure Cognitive Services
description: Saiba como exportar ou eliminar os seus dados no serviço de visão personalizada.
services: cognitive-services
author: v-jaswel
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: conceptual
ms.date: 05/25/2018
ms.author: v-jaswel
ms.openlocfilehash: c98c72101d893f107d4a6c3185836a75bfe41007
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/19/2018
ms.locfileid: "46367496"
---
# <a name="export-or-delete-user-data-in-custom-vision"></a>Exportar ou eliminar dados de utilizador na visão personalizada

Conteúdo moderador recolhe dados de utilizador para operar o serviço, mas os clientes têm controle total sobre a exibição, exportar e eliminar os seus dados, com o serviço de visão personalizada [treinamento API](https://go.microsoft.com/fwlink/?linkid=865446).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

Para obter mais informações sobre como exportar e eliminar dados de utilizador na visão personalizada, consulte a tabela seguinte.

| Dados | Operação de exportação | Operação de Eliminação |
| ---- | ---------------- | ---------------- |
| Informações da conta (chaves de subscrição) | [GetAccountInfo](https://go.microsoft.com/fwlink/?linkid=865446) | Elimine através do portal do Azure (subscrições do Azure). Ou com o botão "Eliminar a sua conta" na página de definições de CustomVision.ai (subscrições de conta da Microsoft) |
| Detalhes de iteração | [GetIteration](https://go.microsoft.com/fwlink/?linkid=865446) | [DeleteIteration](https://go.microsoft.com/fwlink/?linkid=865446) |
| Detalhes de desempenho de iteração | [GetIterationPerformance](https://go.microsoft.com/fwlink/?linkid=865446) | [DeleteIteration](https://go.microsoft.com/fwlink/?linkid=865446) |
| Lista de iterações | [GetIterations](https://go.microsoft.com/fwlink/?linkid=865446) | [DeleteIteration](https://go.microsoft.com/fwlink/?linkid=865446) |
| Projetos e detalhes do projeto | [GetProject](https://go.microsoft.com/fwlink/?linkid=865446) e [GetProjects](https://go.microsoft.com/fwlink/?linkid=865446) | [DeleteProject](https://go.microsoft.com/fwlink/?linkid=865446) |
| Etiquetas de imagem | [GetTag](https://go.microsoft.com/fwlink/?linkid=865446) e [GetTags](https://go.microsoft.com/fwlink/?linkid=865446) | [DeleteTag](https://go.microsoft.com/fwlink/?linkid=865446) |
| Imagens | [GetTaggedImages](https://go.microsoft.com/fwlink/?linkid=865446) (fornece o uri de transferência da imagem) e [GetUntaggedImages](https://go.microsoft.com/fwlink/?linkid=865446) (fornece o uri de transferência da imagem) | [DeleteImages](https://go.microsoft.com/fwlink/?linkid=865446) |
| Modelos exportados | [GetExports](https://go.microsoft.com/fwlink/?linkid=865446) | Eliminado após a eliminação da conta |
