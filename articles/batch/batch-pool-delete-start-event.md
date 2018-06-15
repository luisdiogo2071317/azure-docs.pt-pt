---
title: Azure eventos de início do Batch conjunto eliminar | Microsoft Docs
description: Referência para o evento de início de eliminação de conjunto de Batch.
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
ms.openlocfilehash: 8737b9ff6452730ff5a55fa7324e37f0fe715433
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/03/2018
ms.locfileid: "30312064"
---
# <a name="pool-delete-start-event"></a>Evento de início de eliminação de conjunto

 Este evento é emitido quando foi iniciada uma operação de eliminação do conjunto. Uma vez que a eliminação do conjunto é um evento assíncrono, que pode esperar um conjunto eliminação completa evento ser emitidos uma vez concluída a operação de eliminação.

 O exemplo seguinte mostra o corpo de um evento de início de eliminação do conjunto.

```
{
    "id": "myPool1"
}
```

|Elemento|Tipo|Notas|
|-------------|----------|-----------|
|ID|Cadeia|O id do conjunto.|