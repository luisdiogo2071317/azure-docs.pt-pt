---
title: Listar todos os trabalhos de importar/exportar do Azure | MicrosoftDocs
description: Saiba como listar todas as tarefas do serviço importar/exportar do Azure numa subscrição.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.component: common
ms.openlocfilehash: 69daac71b69969a7ad9acfeb7095053f8138bf53
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/06/2018
ms.locfileid: "39520885"
---
# <a name="enumerating-jobs-in-the-azure-importexport-service"></a>Enumerar tarefas no serviço importar/exportar do Azure
Para enumerar todas as tarefas numa subscrição, chamar o [lista de tarefas](/rest/api/storageimportexport/jobs#Jobs_List) operação. `List Jobs` Devolve uma lista de tarefas, bem como os seguintes atributos:

-   O tipo de tarefa (importação ou exportação)

-   O estado atual da tarefa

-   A tarefa do associado à conta de armazenamento

## <a name="next-steps"></a>Passos Seguintes

* [Utilizar a API de REST do serviço de importação/exportação](storage-import-export-using-the-rest-api.md)
