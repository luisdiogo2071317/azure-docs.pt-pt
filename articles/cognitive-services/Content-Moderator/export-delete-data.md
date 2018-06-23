---
title: Exportar ou eliminar os dados no conteúdo Moderator - serviços cognitivos do Azure | Microsoft Docs
description: Saiba como exportar ou eliminar os dados no Moderator conteúdo.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 05/25/2018
ms.author: v-jaswel
ms.openlocfilehash: 1dbb645a033c6db5ffa9003a53f30fd927131298
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355375"
---
# <a name="export-or-delete-user-data-in-content-moderator"></a>Exportar ou eliminar dados de utilizador no Moderator conteúdo

Conteúdo Moderator recolhe dados de utilizador para o serviço de funcionar, mas os clientes têm controlo total sobre a visualização, exportar e eliminar os respetivos dados, utilizando o [revisão IU](http://contentmoderator.cognitive.microsoft.com/) e [APIs](https://docs.microsoft.com/en-us/azure/cognitive-services/content-moderator/api-reference).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

Para obter mais informações sobre como exportar e eliminar dados de utilizador no Moderator conteúdo, consulte a tabela seguinte.

| Dados | Operação de exportação | Operação de Eliminação |
| ---- | ---------------- | ---------------- |
| Informações de conta (subscrição chaves) | N/A | Eliminar no portal do Azure (subscrições do Azure). Ou utilize o **eliminar equipa** clique no botão no [revisão IU](http://contentmoderator.cognitive.microsoft.com/) página de definições de agrupamento. |
| Imagens para a correspondência personalizado | [Obter IDs de imagem](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f676). As imagens estão armazenadas num formato hash unidirecional de proprietárias e não é possível extrair as imagens reais. | [Eliminar todas as imagens](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f686). Ou eliminar o recurso de Moderator conteúdo através do portal do Azure. |
| Termos de licenciamento para a correspondência personalizado | [Obter todos os termos de licenciamento](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67e) | [Eliminar todos os termos](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67d). Ou eliminar o recurso de Moderator conteúdo através do portal do Azure. |
| Etiquetas | N/A | Utilize o **eliminar** ícone disponível para cada etiqueta na página de definições de etiquetas de IU de revisão. Ou utilize o **eliminar equipa** clique no botão no [revisão IU](http://contentmoderator.cognitive.microsoft.com/) página de definições de agrupamento. |
| Revisões | [Obter revisão](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c2) | Utilize o **eliminar equipa** clique no botão no [revisão IU](http://contentmoderator.cognitive.microsoft.com/) página de definições de agrupamento.
| Utilizadores | N/A | Utilize o **eliminar** ícone disponível para cada utilizador no [revisão IU](http://contentmoderator.cognitive.microsoft.com/) página de definições de agrupamento. Ou utilize o **eliminar equipa** clique no botão no [revisão IU](http://contentmoderator.cognitive.microsoft.com/) página de definições de agrupamento. |

