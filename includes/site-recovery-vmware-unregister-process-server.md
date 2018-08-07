---
author: Rajeswari-Mamilla
ms.service: site-recovery
ms.topic: include
ms.date: 08/06/2018
ms.author: ramamill
ms.openlocfilehash: 81390d38b4c0c38b7ac6883ae2bf18c64542fa00
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/06/2018
ms.locfileid: "39582862"
---
Os passos para anular o registo de um servidor de processos difere consoante o estado de ligação com o Servidor de Configuração.

### <a name="unregister-a-process-server-that-is-in-a-connected-state"></a>Anule o registo de um servidor de processos que está num estado ligado

1. Inicie sessão no servidor de processos como Administrador.
2. Inicie o **Painel de Controlo** e abra **Programas > Desinstalar um programa**
3. Desinstale um programa pelo nome **Configuração/Servidor de Processos do Microsoft Azure Site Recovery**
4. Depois de concluído o passo 3, pode desinstalar **Configuração/Dependências do Servidor de Processos do Microsoft Azure Site Recovery**

### <a name="unregister-a-process-server-that-is-in-a-disconnected-state"></a>Anule o registo de um servidor de processos que está num estado desligado

> [!WARNING]
> Os passos abaixo devem ser utilizados se não existir nenhuma forma de reativar a máquina virtual em que o Servidor de Processos estava instalado.

1. Inicie sessão no seu servidor de configuração como administrador.
2. Abra uma linha de comandos Administrativa e procure o diretório `%ProgramData%\ASR\home\svsystems\bin`.
3. Agora, execute o comando.

    ```
    perl Unregister-ASRComponent.pl -IPAddress <IP_of_Process_Server> -Component PS
    ```
4. O comando acima irá fornecer a lista de servidores de processo (pode ser mais do que um, em caso de entradas duplicadas) com serial number(S.No), o endereço IP (IP), o nome da VM em que servidor de processos é implementado (nome), atingir coração da VM (Heartbeat), conforme mostrado abaixo.
    ![Cmd anular o registo](media/site-recovery-vmware-unregister-process-server/Unregister-cmd.PNG)
5. Agora, introduza o número de série do servidor de processos que pretende anular o registo.
6. Isto irá remover os detalhes do servidor de processos do sistema e irá apresentar a mensagem: **anulado com êxito o nome do servidor > (endereço de IP de servidor)**

