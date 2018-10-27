---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 9bd2ee708f7d27cff5d07ca7f86d925ca6d2741d
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/26/2018
ms.locfileid: "50165261"
---
<!--author=SharS last changed: 12/01/15-->

#### <a name="to-enter-maintenance-mode"></a>Para entrar no modo de manutenção
1. No menu da consola de série, selecione a opção 1, **iniciar sessão com acesso total**.
2. Escreva a palavra-passe. A palavra-passe predefinido é **Password1**.
3. Na linha de comandos, escreva
   
     `Enter-HcsMaintenanceMode`
4. Verá uma mensagem de aviso informando que o modo de manutenção irá interromper todos os pedidos de e/s e a ligação ao portal clássico do Azure do servidor, e será solicitado para confirmação. Tipo **Y** para entrar no modo de manutenção.
   
    Os dois controladores serão reiniciado. Quando o reinício estiver concluído, a outra mensagem será apresentada indicando que o dispositivo estiver no modo de manutenção.

