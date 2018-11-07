---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 11/06/2018
ms.author: alkohli
ms.openlocfilehash: 67de9042af11a2b17c4b65f8225ecd0580b95466
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51263604"
---
| Porta não.| Dentro ou para fora | Âmbito de porta| Necessário|   Notas |   |
|--------|-----|-----|-----------|----------|-----------|
| TCP 80 (HTTP)|Saída|WAN |Não|Porta de saída é utilizada para acesso à internet para obter atualizações. <br>O proxy da web de saída é configurável de utilizador. |
| TCP 443 (HTTPS)|Saída|WAN|Sim|Porta de saída é utilizada para aceder aos dados na cloud.<br>O proxy da web de saída é configurável de utilizador.|   
| UDP 53 (DNS)|Saída|WAN|Em alguns casos<br>Consulte as notas|Esta porta é necessária apenas se estiver a utilizar um servidor DNS baseado na internet.<br>Recomendamos que utilize um servidor DNS local. |
| UDP 123 (NTP)|Saída|WAN|Em alguns casos<br>Consulte as notas|Esta porta é necessária apenas se estiver a utilizar um servidor NTP baseado na internet.  |
| UDP 67 (DHCP)|Saída|WAN|Em alguns casos<br>Consulte as notas|Esta porta é necessária apenas se estiver a utilizar um servidor DHCP.  |
| TCP 80 (HTTP)|Em|LAN|Sim|Esta porta é a porta de entrada para a interface do Usuário local no dispositivo para gestão local. <br>Acessar a interface do Usuário local através de HTTP será automaticamente redirecionada para HTTPS.  |
| TCP 443 (HTTPS)|Em|LAN|Sim|Esta porta é a porta de entrada para a interface do Usuário local no dispositivo para gestão local. |