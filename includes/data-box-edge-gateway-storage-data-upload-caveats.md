---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 02/14/2019
ms.author: alkohli
ms.openlocfilehash: 8bc4b78d262cf5f30076e90b40b92c4f3237924a
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/16/2019
ms.locfileid: "56334062"
---
As limitações seguintes aplicam-se aos dados quando são transmitidos para o Azure.

- Sugerimos que mais de um dispositivo não deve escrever o mesmo contentor.
- Se tiver um objeto do Azure existente (como um blob ou um arquivo) na cloud com o mesmo nome que o objeto que está a ser copiado, o dispositivo irá substituir o ficheiro na cloud.
- Uma hierarquia de diretório vazio (sem quaisquer ficheiros), criada em pastas de partilha não é carregada para os contentores de Blobs.
- Para ficheiros grandes, recomendamos que utilize robocopy porque tentar novamente a operação de cópia para erros freqüentes.
- Se a partilha associada ao contentor de armazenamento do Azure carrega blobs que não correspondem o tipo de blobs definidos para a partilha no momento da criação, tais blobs não são atualizadas. Por exemplo, criar uma partilha de blob de blocos no dispositivo. Associe a partilha com um contentor de cloud existente com blobs de páginas. Esse compartilhamento para transferir os ficheiros de atualização. Modificar alguns dos arquivos atualizados que já são armazenados como blobs de páginas na cloud. Irá ver falhas de carregamento.
