---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 01612b32e6c1b363df8a5c70405d0c709210328e
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/26/2018
ms.locfileid: "50165702"
---
<!--author=SharS last changed: 9/17/15-->

#### <a name="to-install-maintenance-mode-updates-via-windows-powershell-for-storsimple"></a>Para instalar o modo de manutenção de atualizações através do Windows PowerShell para StorSimple
1. Se ainda não fez isso, aceder à consola de série do dispositivo e a opção selecione 1, **iniciar sessão com acesso total**. 
2. Escreva a palavra-passe. A palavra-passe predefinido é **Password1**.
3. Na linha de comandos, escreva:
   
     `Get-HcsUpdateAvailability` 
4. Será notificado se as atualizações estiverem disponíveis e se as atualizações são disruptivas ou não disruptivas. Para aplicar atualizações disruptivas, terá de colocar o dispositivo no modo de manutenção. Ver [passo 2: modo de manutenção introduza](../articles/storsimple/storsimple-update-device.md#step2) para obter instruções.
5. Quando o dispositivo está no modo de manutenção, a linha de comandos, escreva: `Start-HcsUpdate`
6. Será solicitado para confirmação. Depois de confirmar as atualizações, serão instalados no controlador que está atualmente a aceder. Depois das atualizações são instaladas, o controlador será reiniciado. 
7. Monitorize o estado das atualizações. Escreva:
   
    `Get-HcsUpdateStatus`
   
    Se o `RunInProgress` é `True`, a atualização ainda está em curso. Se `RunInProgress` é `False`, ele indica que a atualização foi concluída.  
8. Quando a atualização está instalada no controlador de atual e foi reiniciado, ligar a outro controlador e efetuar os passos 1 a 6.
9. Depois de ambos os controladores são atualizados, saia do modo de manutenção. Ver [passo 4: modo de manutenção de saída](../articles/storsimple/storsimple-update-device.md#step4) para obter instruções.

