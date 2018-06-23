---
title: Exportar ou eliminar os dados no visão personalizada, serviços cognitivos do Azure | Microsoft Docs
description: Saiba como exportar ou eliminar os dados no visão personalizada.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: article
ms.date: 05/25/2018
ms.author: v-jaswel
ms.openlocfilehash: 49fc49b3181f56c23167cfbae18911e7db441f8c
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355381"
---
# <a name="export-or-delete-user-data-in-custom-vision"></a>Exportar ou eliminar dados de utilizador no visão personalizada

Conteúdo Moderator recolhe dados de utilizador para o serviço de funcionar, mas os clientes têm controlo total sobre a visualização, exportar e eliminar os respetivos dados, utilizando o serviço de visão personalizada [formação API](https://go.microsoft.com/fwlink/?linkid=865446).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

Para obter mais informações sobre como exportar e eliminar dados de utilizador no visão personalizada, consulte a tabela seguinte.

| Dados | Operação de exportação | Operação de Eliminação |
| ---- | ---------------- | ---------------- |
| Informações de conta (subscrição chaves) | [GetAccountInfo](https://go.microsoft.com/fwlink/?linkid=865446) | Elimine através do portal do Azure (subscrições do Azure). Ou ao utilizar o botão "Eliminar a conta" na página de definições de CustomVision.ai (subscrições de conta Microsoft) |
| Detalhes de iteração | [GetIteration](https://go.microsoft.com/fwlink/?linkid=865446) | [DeleteIteration](https://go.microsoft.com/fwlink/?linkid=865446) |
| Detalhes de desempenho de iteração | [GetIterationPerformance](https://go.microsoft.com/fwlink/?linkid=865446) | [DeleteIteration](https://go.microsoft.com/fwlink/?linkid=865446) |
| Lista de iterações | [GetIterations](https://go.microsoft.com/fwlink/?linkid=865446) | [DeleteIteration](https://go.microsoft.com/fwlink/?linkid=865446) |
| Projetos e detalhes do projeto | [GetProject](https://go.microsoft.com/fwlink/?linkid=865446) e [GetProjects](https://go.microsoft.com/fwlink/?linkid=865446) | [DeleteProject](https://go.microsoft.com/fwlink/?linkid=865446) |
| Etiquetas de imagem | [GetTag](https://go.microsoft.com/fwlink/?linkid=865446) e [GetTags](https://go.microsoft.com/fwlink/?linkid=865446) | [DeleteTag](https://go.microsoft.com/fwlink/?linkid=865446) |
| Imagens | [GetTaggedImages](https://go.microsoft.com/fwlink/?linkid=865446) (fornece o uri de transferência da imagem) e [GetUntaggedImages](https://go.microsoft.com/fwlink/?linkid=865446) (fornece o uri de transferência da imagem de) | [DeleteImages](https://go.microsoft.com/fwlink/?linkid=865446) |
| Modelos exportados | [GetExports](https://go.microsoft.com/fwlink/?linkid=865446) | Eliminado após a eliminação da conta |
