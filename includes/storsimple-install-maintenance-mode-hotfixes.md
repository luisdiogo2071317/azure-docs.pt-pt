---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 5e4921be3116754f146ed0845513010f2642c97b
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/26/2018
ms.locfileid: "50166457"
---
<!--author=SharS last changed: 9/17/15-->

#### <a name="to-install-maintenance-mode-hotfixes-via-windows-powershell-for-storsimple"></a>Para instalar as correções do modo de manutenção através do Windows PowerShell para StorSimple
> [!IMPORTANT]
> No modo de manutenção, terá de aplicar a correção primeiro num controlador e, em seguida, no outro controlador.
> 
> 

1. Coloque o dispositivo no modo de manutenção. Ver [passo 2: modo de manutenção introduza](../articles/storsimple/storsimple-update-device.md#step2) para obter instruções sobre como inserir o modo de manutenção.
2. Para aplicar a correção, escreva:
   
     `Start-HcsHotfix` 
3. Quando lhe for pedido, forneça o caminho para a pasta compartilhada de rede que contém os ficheiros de correção.
4. Será solicitado para confirmação. Tipo **Y** para prosseguir com a instalação da correção.
5. Depois de ter aplicado a correção num controlador, inicie sessão no outro controlador. Aplique a correção, tal como fez para o controlador anterior.
6. Depois das correções são aplicadas, saia do modo de manutenção. Ver [passo 4: modo de manutenção de saída](../articles/storsimple/storsimple-update-device.md#step4) para obter instruções.

