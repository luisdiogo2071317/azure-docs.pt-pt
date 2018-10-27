---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: e743128932f6a136820819c21e0386eb678c4203
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/26/2018
ms.locfileid: "50164263"
---
<!--author=jgerend last changed: 03/16/16-->

## <a name="preparing-for-updates"></a>Preparar para atualizações
Terá de efetuar os seguintes passos antes de procurar e aplicar a atualização:

1. Tire um instantâneo de cloud dos dados do dispositivo.
2. Certifique-se de que os IPs fixos do controlador são encaminhável e podem ligar-se à Internet. Estes IPs fixos será utilizado para as atualizações para o seu dispositivo de serviço. Isso pode ser testado, executando o cmdlet seguinte em cada controlador a partir da interface do Windows PowerShell do dispositivo:
   
     `Test-Connection -Source <Fixed IP of your device controller> -Destination <Any IP or computer name outside of datacenter network> `
   
    **Saída de exemplo para Test-Connection quando os IPs fixos pode ligar à Internet**

        Controller0>Test-Connection -Source 10.126.173.91 -Destination bing.com

        Source      Destination     IPV4Address      IPV6Address
        ----------------- -----------  -----------
        HCSNODE0  bing.com        204.79.197.200
        HCSNODE0  bing.com        204.79.197.200
        HCSNODE0  bing.com        204.79.197.200
        HCSNODE0  bing.com        204.79.197.200

        Controller0>Test-Connection -Source 10.126.173.91 -Destination  204.79.197.200

        Source      Destination       IPV4Address    IPV6Address
        ----------------- -----------  -----------
        HCSNODE0  204.79.197.200  204.79.197.200
        HCSNODE0  204.79.197.200  204.79.197.200
        HCSNODE0  204.79.197.200  204.79.197.200
        HCSNODE0  204.79.197.200  204.79.197.200

Depois destas pré-verificações de manuais foi concluído com êxito, pode avançar para procurar e instalar as atualizações.

