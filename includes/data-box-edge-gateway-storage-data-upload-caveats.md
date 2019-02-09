---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 02/07/2019
ms.author: alkohli
ms.openlocfilehash: ad7ceffed61665a729d4391b5f0ff2935375c253
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/08/2019
ms.locfileid: "55967508"
---
As limitações seguintes aplicam-se aos dados quando são transmitidos para o Azure.

- Sugerimos que mais de um dispositivo não deve escrever o mesmo contentor.
- Se tiver um objeto do Azure existente (como um blob ou um arquivo) na cloud com o mesmo nome que o objeto que está a ser copiado, o dispositivo irá substituir o ficheiro na cloud.
- Uma hierarquia de diretório vazio (sem quaisquer ficheiros), criada em pastas de partilha não é carregada para os contentores de Blobs.
- Para ficheiros grandes, recomendamos que utilize robocopy porque tentar novamente a operação de cópia para erros freqüentes.
