---
title: Cancelar e eliminar uma tarefa de importar/exportar do Azure | Documentos da Microsoft
description: Aprenda a cancelar e eliminar trabalhos para o serviço de importação/exportação do Microsoft Azure.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.component: common
ms.openlocfilehash: e11cf974a5f33020c889844dd34f51612e13036e
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/06/2018
ms.locfileid: "39521018"
---
# <a name="canceling-and-deleting-azure-importexport-jobs"></a>A cancelar e eliminar tarefas de importação/exportação do Azure

 Para solicitar que uma tarefa ser cancelada antes de ser no `Packaging` de estado, chamar o [das propriedades da tarefa de atualização](/rest/api/storageimportexport/jobs#Jobs_Update) operação e o conjunto a `CancelRequested` elemento para `true`. A tarefa é cancelada numa base de melhor esforço. Se as unidades estão no processo de transferência de dados, os dados poderão continuar a ser transferidos, mesmo após o cancelamento foi solicitado.

 Uma tarefa foi cancelada é movida para o `Completed` de estado e são mantidos durante 90 dias, altura em que este é eliminado.

 Para eliminar uma tarefa, chame o [eliminar tarefa](/rest/api/storageimportexport/jobs#Jobs_Delete) operação antes da tarefa foi lançado (ou seja, enquanto a tarefa está no `Creating` Estado). Também pode eliminar uma tarefa quando estiver a ser o `Completed` estado. Depois de eliminar uma tarefa, suas informações e o estado já não estão acessíveis através da API REST ou o portal do Azure.

[!INCLUDE [storage-import-export-delete-personal-info.md](../../../includes/storage-import-export-delete-personal-info.md)]

## <a name="next-steps"></a>Passos Seguintes

* [Utilizar a API de REST do serviço de importação/exportação](storage-import-export-using-the-rest-api.md)
