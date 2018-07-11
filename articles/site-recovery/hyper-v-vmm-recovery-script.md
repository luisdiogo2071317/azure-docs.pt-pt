---
title: Adicionar um script para um plano de recuperação no Azure Site Recovery | Documentos da Microsoft
description: Saiba mais sobre os pré-requisitos para adicionar um novo script do System Center Virtual Machine Manager (VMM) para um plano de recuperação no Azure.
services: site-recovery
documentationcenter: ''
author: rajani-janaki-ram
manager: rochakm
editor: ''
ms.assetid: 72408c62-fcb6-4ee2-8ff5-cab1218773f2
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/06/2018
ms.author: rajanaki
ms.openlocfilehash: 71991347ffaf036065aae9e1a93b7eb83a14b15c
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/09/2018
ms.locfileid: "37917347"
---
# <a name="add-a-vmm-script-to-a-recovery-plan"></a>Adicionar um script VMM para um plano de recuperação

Este artigo descreve como criar um script do System Center Virtual Machine Manager (VMM) e adicioná-lo para um plano de recuperação no [do Azure Site Recovery](site-recovery-overview.md).

Publique comentários ou perguntas na parte inferior deste artigo, ou sobre o [fórum dos serviços de recuperação do Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="prerequisites"></a>Pré-requisitos

Pode utilizar scripts do PowerShell nos seus planos de recuperação. Para ser acessível a partir do plano de recuperação, tem de criar o script e colocar o script na biblioteca do VMM. Mantenha as seguintes considerações em mente enquanto escrever o script:

* Certifique-se de que os scripts usam blocos try-catch, para que as exceções são manipuladas com elegância.
    - Se ocorrer uma exceção no script, o script deixa de ser executada e a tarefa mostra como falhado.
    - Se ocorrer um erro, o restante do script não é executado.
    - Se ocorrer um erro ao executar uma ativação pós-falha não planeada, o plano de recuperação continua.
    - Se ocorrer um erro ao executar uma ativação pós-falha planeada, o plano de recuperação será interrompido. Corrija o script, verifique se funciona como esperado e em seguida, executar a recuperação planear novamente.
        - O `Write-Host` comando não funcionar num script de plano de recuperação. Se utilizar o `Write-Host` comando num script, o script falhar. Para criar a saída, crie um script de proxy que por sua vez executa o script principal. Para garantir que toda a saída é enviada por pipe de saída, utilize o **\> \>** comando.
        - O script exceder o tempo limite se ele não retorna dentro de 600 segundos.
        - Se algo for gravado para STDERR, o script é classificado como falhado. Estas informações são apresentadas nos detalhes de execução do script.

* Executam scripts num plano de recuperação no contexto da conta de serviço do VMM. Certifique-se de que esta conta tem permissões de leitura para a partilha remota, no qual o script está localizado. Teste o script seja executado com o mesmo nível de direitos de usuário como a conta de serviço do VMM.
* Cmdlets do VMM são entregues por um módulo do Windows PowerShell. O módulo é instalado ao instalar a consola do VMM. Para carregar o módulo para o seu script, utilize o seguinte comando no script: 

    `Import-Module -Name virtualmachinemanager`

    Para obter mais informações, consulte [introdução ao Windows PowerShell e o VMM](https://technet.microsoft.com/library/hh875013.aspx).
* Certifique-se de que tem pelo menos um servidor de bibliotecas na sua implementação do VMM. Por predefinição, o caminho da partilha de biblioteca para um servidor VMM está localizado localmente no servidor do VMM. O nome de pasta é MSCVMMLibrary.

  Se o caminho da partilha de biblioteca for remoto (ou se é local, mas não partilhado com MSCVMMLibrary), configure a partilha da seguinte forma, usando \\libserver2.contoso.com\share\ como exemplo:
  
  1. Abra o Editor de registo e, em seguida, aceda a **HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\Azure Site Recovery\Registration**.

  2. Altere o valor de **ScriptLibraryPath** ao  **\\\libserver2.contoso.com\share\\**. Especifique o FQDN completo. Fornece permissões para a localização da partilha. Este é o nó de raiz da partilha. Para verificar o nó de raiz, no VMM, aceda ao nó de raiz na biblioteca. O caminho que se abre é a raiz do caminho. Este é o caminho que tem de utilizar a variável.

  3. Teste o script com uma conta de utilizador que tenha o mesmo nível de direitos de usuário como a conta de serviço do VMM. Utilizar estes direitos de usuário verifica pelo autónomo, testados os scripts executados da mesma forma que eles são executados em planos de recuperação. No servidor do VMM, defina a política de execução para ignorar, da seguinte forma:

     a. Abra o **PowerShell de Windows de 64 bits** consola como um administrador.
     
     b. Introduza **Set-executionpolicy omissão**. Para obter mais informações, consulte [utilizando o cmdlet Set-ExecutionPolicy](https://technet.microsoft.com/library/ee176961.aspx).

     > [!IMPORTANT]
     > Definir **Set-executionpolicy omissão** apenas na consola do PowerShell de 64 bits. Se defini-la para a consola do PowerShell de 32 bits, os scripts não são executados.

## <a name="add-the-script-to-the-vmm-library"></a>Adicione o script à biblioteca do VMM

Se tiver um site de origem do VMM, pode criar um script no servidor do VMM. Em seguida, inclua o script no seu plano de recuperação.

1. A partilha de biblioteca, crie uma nova pasta. Por exemplo, \<nome do servidor VMM > \MSSCVMMLibrary\RPScripts. Posicione a pasta de origem e destino servidores VMM.
2. Crie o script. Por exemplo, o nome do script RPScript. Certifique-se de que o script funciona conforme esperado.
3. Coloque o script no \<nome do servidor VMM > pasta \MSSCVMMLibrary nos servidores do VMM de origem e de destino.

## <a name="add-the-script-to-a-recovery-plan"></a>Adicione o script para um plano de recuperação

Depois de adicionar VMs ou grupos de replicação para um plano de recuperação e criado o plano, pode adicionar o script para o grupo.

1. Abra o plano de recuperação.
2. Na **passo** , selecione um item. Em seguida, selecione **Script** ou **ação Manual**.
3. Especifique se pretende adicionar o script ou a ação antes ou depois do item selecionado. Para mover a posição do script ou reduzir verticalmente, selecione o **mover para cima** e **mover para baixo** botões.
4. Se adicionar um script do VMM, selecione **ativação pós-falha para o script VMM**. Na **caminho do Script**, introduza o caminho relativo para a partilha. Por exemplo, introduza **\RPScripts\RPScript.PS1**.
5. Se adicionar um runbook da automatização do Azure, especifique a conta de automatização em que se encontra o runbook. Em seguida, selecione o script de runbook do Azure que pretende utilizar.
6. Para garantir que o script funciona conforme esperado, efetue uma ativação pós-falha de teste do plano de recuperação.


## <a name="next-steps"></a>Passos Seguintes
* Saiba mais sobre [executar as ativações pós-falha](site-recovery-failover.md).

