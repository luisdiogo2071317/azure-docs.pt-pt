---
title: Evento completa eliminar o conjunto do Azure Batch | Microsoft Docs
description: Referência para o conjunto do Batch eliminar eventos concluído.
services: batch
author: dlepow
manager: jeconnoc
ms.assetid: ''
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 04/20/2017
ms.author: danlep
ms.openlocfilehash: bfcbcf40efc64ab1c79ee1a86e02502c68ad6d47
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/03/2018
ms.locfileid: "30310319"
---
# <a name="pool-delete-complete-event"></a>Evento de conclusão de eliminação de conjunto

 Este evento é emitido quando concluir uma operação de eliminação do conjunto.

 O exemplo seguinte mostra o corpo de um evento de conclusão de eliminação de agrupamento.

```
{
    "id": "myPool1",
    "startTime": "2016-09-09T22:13:48.579Z",
    "endTime": "2016-09-09T22:14:08.836Z"
}
```

|Elemento|Tipo|Notas|
|-------------|----------|-----------|
|ID|Cadeia|O id do conjunto.|
|startTime|DateTime|O tempo a eliminar o conjunto foi iniciado.|
|endTime|DateTime|A hora de eliminar o conjunto foi concluída.|

## <a name="remarks"></a>Observações
Para obter mais informações sobre os Estados e códigos de erro de operação de redimensionamento do conjunto, consulte [eliminar um conjunto de uma conta](https://docs.microsoft.com/rest/api/batchservice/delete-a-pool-from-an-account).