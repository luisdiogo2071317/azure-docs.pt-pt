---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: d656d756759c997972eb034e194355185be93e1a
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/29/2018
ms.locfileid: "50226573"
---
Em seguida, se estiver a executar todos os servidores no cluster do Windows Server 2008 R2 ou Windows Server 2012, tem de verificar se a correção [KB2854082](http://support.microsoft.com/kb/2854082) é instalado em cada um dos servidores no local ou VMs do Azure que fazem parte do cluster. Qualquer servidor ou VM que está no cluster, mas não no grupo de disponibilidade, deve também ter esta correção instalada.

Na sessão de área de trabalho remota para cada um de nós do cluster, transfira [KB2854082](http://support.microsoft.com/kb/2854082) para um diretório local. Em seguida, instale em seqüência a correção em cada nó de cluster. Se o serviço de cluster está atualmente em execução no nó de cluster, o servidor é reiniciado no final da instalação da correção.

> [!WARNING]
> A parar o serviço de cluster ou reiniciar o servidor afeta o estado de funcionamento do quórum do cluster e do grupo de disponibilidade e pode fazer com que o seu cluster para ficar offline. Para manter a elevada disponibilidade do cluster durante a instalação, certifique-se de que:
> 
> * O cluster está num Estado de funcionamento de quórum ideais. 
> * Antes de instalar a correção em qualquer nó, todos os nós de cluster estão online.
> * Antes de instalar a correção em qualquer outro nó no cluster, permitir a instalação da correção ser executado para conclusão num nó, incluindo totalmente reiniciar o servidor.
> 
> 

