---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 77fc95c53ba89c3905ebd1aec785e22f42339369
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/26/2018
ms.locfileid: "50164836"
---
<!--author=SharS last changed: 9/17/15-->

#### <a name="to-install-regular-hotfixes-via-windows-powershell-for-storsimple"></a>Instalar correções regulares através do Windows PowerShell para StorSimple
1. Ligar a consola de série do dispositivo. Para obter mais informações, consulte [passo 1: ligar à consola de série](../articles/storsimple/storsimple-update-device.md#step1).
2. No menu da consola de série, selecione a opção 1, **iniciar sessão com acesso total**. Escreva a palavra-passe. A palavra-passe predefinido é **Password1**.
3. Na linha de comandos, escreva:
   
    ```
    Start-HcsHotfix
    ```
   
    > [!IMPORTANT]
    >
    > Este comando só se aplica a correções regulares. Execute este comando num controlador de apenas uma, mas ambos os controladores serão atualizadas.
    > Pode observar uma ativação pós-falha de controlador durante o processo de atualização; No entanto, a ativação pós-falha não irá afetar a disponibilidade de sistema ou a operação.

4. Quando lhe for pedido, forneça o caminho para a pasta compartilhada de rede que contém os ficheiros de correção.
5. Será solicitado para confirmação. Tipo **Y** para prosseguir com a instalação da correção.

