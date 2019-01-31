---
title: Cancelar e eliminar uma tarefa de importar/exportar do Azure | Documentos da Microsoft
description: Aprenda a cancelar e eliminar trabalhos para o serviço de importação/exportação do Microsoft Azure.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.subservice: common
ms.openlocfilehash: 0aa1a84da874e0b0f3ed8b865eab0490f1498c1e
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/31/2019
ms.locfileid: "55451228"
---
# <a name="canceling-and-deleting-azure-importexport-jobs"></a>A cancelar e eliminar tarefas de importação/exportação do Azure

 Para solicitar que uma tarefa ser cancelada antes de ser no `Packaging` de estado, chamar o [das propriedades da tarefa de atualização](/rest/api/storageimportexport/jobs#Jobs_Update) operação e o conjunto a `CancelRequested` elemento para `true`. A tarefa é cancelada numa base de melhor esforço. Se as unidades estão no processo de transferência de dados, os dados poderão continuar a ser transferidos, mesmo após o cancelamento foi solicitado.

 Uma tarefa foi cancelada é movida para o `Completed` de estado e são mantidos durante 90 dias, altura em que este é eliminado.

 Para eliminar uma tarefa, chame o [eliminar tarefa](/rest/api/storageimportexport/jobs#Jobs_Delete) operação antes da tarefa foi lançado (ou seja, enquanto a tarefa está no `Creating` Estado). Também pode eliminar uma tarefa quando estiver a ser o `Completed` estado. Depois de eliminar uma tarefa, suas informações e o estado já não estão acessíveis através da API REST ou o portal do Azure.

[!INCLUDE [storage-import-export-delete-personal-info.md](../../../includes/storage-import-export-delete-personal-info.md)]

## <a name="next-steps"></a>Passos Seguintes

* [Utilizar a API de REST do serviço de importação/exportação](storage-import-export-using-the-rest-api.md)
