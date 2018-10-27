---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 36a014fbff30c32a7149a83907c9586bc3b2f01a
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/26/2018
ms.locfileid: "50165781"
---
<!--author=SharS last changed: 11/18/16-->

#### <a name="to-install-regular-updates-via-windows-powershell-for-storsimple"></a>Para instalar atualizações regulares através do Windows PowerShell para StorSimple
1. Abra a consola de série do dispositivo e selecione opção 1, **iniciar sessão com acesso total**. Escreva a palavra-passe. A palavra-passe predefinido é *Password1*. 
2. Na linha de comandos, escreva:
   
     `Get-HcsUpdateAvailability`
   
    Será notificado se as atualizações estiverem disponíveis e se as atualizações são disruptivas ou não disruptivas.
3. Na linha de comandos, escreva:
   
     `Start-HcsUpdate`
   
    Será iniciado o processo de atualização.

> [!IMPORTANT]
> * Este comando aplica-se apenas a atualizações regulares. Execute este comando num controlador de apenas uma, mas ambos os controladores serão atualizadas. 
> * Pode observar uma ativação pós-falha de controlador durante o processo de atualização; No entanto, a ativação pós-falha não irá afetar a disponibilidade de sistema ou a operação.
> 
> 

