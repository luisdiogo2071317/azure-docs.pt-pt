---
title: Listar todos os trabalhos de importar/exportar do Azure | MicrosoftDocs
description: Saiba como listar todas as tarefas do serviço importar/exportar do Azure numa subscrição.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.subservice: common
ms.openlocfilehash: 017208c6fca7c4e55a45070f5aa21652e83e7e8d
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/31/2019
ms.locfileid: "55462931"
---
# <a name="enumerating-jobs-in-the-azure-importexport-service"></a>Enumerar tarefas no serviço importar/exportar do Azure
Para enumerar todas as tarefas numa subscrição, chamar o [lista de tarefas](/rest/api/storageimportexport/jobs#Jobs_List) operação. `List Jobs` Devolve uma lista de tarefas, bem como os seguintes atributos:

-   O tipo de tarefa (importação ou exportação)

-   O estado atual da tarefa

-   A tarefa do associado à conta de armazenamento

## <a name="next-steps"></a>Passos Seguintes

* [Utilizar a API de REST do serviço de importação/exportação](storage-import-export-using-the-rest-api.md)
