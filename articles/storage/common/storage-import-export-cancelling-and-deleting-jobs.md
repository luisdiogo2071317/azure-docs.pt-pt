---
title: Cancelar e eliminar uma tarefa de importação/exportação do Azure | Microsoft Docs
description: Saiba como cancelar e eliminar tarefas para o serviço de importação/exportação do Microsoft Azure.
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: ''
ms.assetid: fd3d66f0-1dbb-4c75-9223-307d5abaeefc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: 3524f1677baaa218b009b8498b851390c7b9da9a
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/20/2018
---
# <a name="canceling-and-deleting-azure-importexport-jobs"></a>Cancelar e eliminar tarefas de importação/exportação do Azure

 Para pedir que uma tarefa ser cancelada antes de ser no `Packaging` de estado, chame o [propriedades da tarefa de atualização](/rest/api/storageimportexport/jobs#Jobs_Update) operação e conjunto o `CancelRequested` elemento `true`. A tarefa foi cancelada uma base de melhor esforço. Se unidades estão no processo de transferência de dados, os dados podem continuar a serem transferidos, mesmo depois foi solicitado o cancelamento.

 Uma tarefa cancelada for movida para o `Completed` estado e são mantidos durante 90 dias, altura em que é eliminado.

 Para eliminar uma tarefa, chame o [tarefa eliminar](/rest/api/storageimportexport/jobs#Jobs_Delete) operação antes da tarefa foi enviada (ou seja, enquanto a tarefa está no `Creating` Estado). Também pode eliminar uma tarefa quando estiver a ser o `Completed` estado. Depois de uma tarefa é eliminada, o estado e informações já não estão acessíveis através da API REST ou o portal do Azure.

[!INCLUDE [storage-import-export-delete-personal-info.md](../../../includes/storage-import-export-delete-personal-info.md)]

## <a name="next-steps"></a>Passos Seguintes

* [Utilizar o REST API do serviço de importação/exportação](storage-import-export-using-the-rest-api.md)
