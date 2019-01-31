---
title: Evento de conclusão de eliminar o conjunto do Batch do Azure | Documentos da Microsoft
description: Referência para o conjunto do Batch eliminar evento de conclusão.
services: batch
author: laurenhughes
manager: jeconnoc
ms.assetid: ''
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 04/20/2017
ms.author: lahugh
ms.openlocfilehash: e715ccd0f5e79f9c640a3c060b0252b798748b4d
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/31/2019
ms.locfileid: "55474553"
---
# <a name="pool-delete-complete-event"></a>Evento de conclusão de eliminação de conjunto

 Este evento é emitido quando uma operação de eliminação de conjunto for concluída.

 O exemplo seguinte mostra o corpo de um evento de conclusão de eliminação de conjunto.

```
{
    "id": "myPool1",
    "startTime": "2016-09-09T22:13:48.579Z",
    "endTime": "2016-09-09T22:14:08.836Z"
}
```

|Elemento|Type|Notas|
|-------------|----------|-----------|
|ID|String|O id do conjunto.|
|startTime|DateTime|O tempo de eliminar o conjunto foi iniciada.|
|endTime|DateTime|O tempo de eliminar o conjunto foi concluído.|

## <a name="remarks"></a>Observações
Para obter mais informações sobre Estados e códigos de erro de operação de redimensionamento de conjunto, consulte [eliminar um conjunto a partir de uma conta](https://docs.microsoft.com/rest/api/batchservice/delete-a-pool-from-an-account).