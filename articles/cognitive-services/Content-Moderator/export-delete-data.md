---
title: Exportar ou eliminar os seus dados - Content Moderator
titlesuffix: Azure Cognitive Services
description: Saiba como exportar ou eliminar os seus dados no Content Moderator.
services: cognitive-services
author: v-jaswel
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: conceptual
ms.date: 05/25/2018
ms.author: v-jaswel
ms.openlocfilehash: 9e671fa9735406ec425b105bb121d3a029ef19dc
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/26/2018
ms.locfileid: "47227236"
---
# <a name="export-or-delete-user-data-in-content-moderator"></a>Exportar ou eliminar dados de utilizador no Content Moderator

Conteúdo moderador recolhe dados de utilizador para operar o serviço, mas os clientes têm controle total sobre a exibição, exportar e eliminar os seus dados, utilizando o [IU de revisão](http://contentmoderator.cognitive.microsoft.com/) e o [APIs](https://docs.microsoft.com/azure/cognitive-services/content-moderator/api-reference).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

Para obter mais informações sobre como exportar e eliminar dados de utilizador no Content Moderator, consulte a tabela seguinte.

| Dados | Operação de exportação | Operação de Eliminação |
| ---- | ---------------- | ---------------- |
| Informações da conta (chaves de subscrição) | N/A | Eliminação com o portal do Azure (subscrições do Azure). Ou utilize o **eliminar equipe** botão no [interface do Usuário de revisão](http://contentmoderator.cognitive.microsoft.com/) página de configurações de equipe. |
| Imagens para correspondência | [Obter IDs de imagem](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f676). As imagens são armazenadas num formato de hash unidirecional de proprietário e não é possível extrair as imagens reais. | [Eliminar todas as imagens](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f686). Ou eliminar o recurso do Content Moderator no portal do Azure. |
| Termos de correspondência | [Obter todos os termos](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67e) | [Eliminar todos os termos](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67d). Ou eliminar o recurso do Content Moderator no portal do Azure. |
| Etiquetas | N/A | Utilize o **eliminar** disponível para cada marca na página de definições de revisão da interface do Usuário marca do ícone. Ou utilize o **eliminar equipe** botão no [interface do Usuário de revisão](http://contentmoderator.cognitive.microsoft.com/) página de configurações de equipe. |
| Revisões | [Obter revisão](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c2) | Utilize o **eliminar equipe** botão no [interface do Usuário de revisão](http://contentmoderator.cognitive.microsoft.com/) página de configurações de equipe.
| Utilizadores | N/A | Utilize o **eliminar** ícone disponível para todos os utilizadores a [interface do Usuário de revisão](http://contentmoderator.cognitive.microsoft.com/) página de configurações de equipe. Ou utilize o **eliminar equipe** botão no [interface do Usuário de revisão](http://contentmoderator.cognitive.microsoft.com/) página de configurações de equipe. |

