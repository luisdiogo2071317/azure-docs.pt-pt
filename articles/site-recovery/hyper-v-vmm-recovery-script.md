---
title: "Adicionar um script para um plano de recuperação no Azure Site Recovery | Microsoft Docs"
description: "Saiba mais sobre os pré-requisitos para adicionar um novo script do System Center Virtual Machine Manager (VMM) para um plano de recuperação no Azure."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: shons
editor: 
ms.assetid: 72408c62-fcb6-4ee2-8ff5-cab1218773f2
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 12/13/2017
ms.author: ruturaj
ms.openlocfilehash: 2e00f812fb35ac9a0cb390fc6a3ba40a8678f8dd
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/21/2018
---
# <a name="add-a-vmm-script-to-a-recovery-plan"></a>Adicionar um script do VMM para um plano de recuperação

Este artigo descreve como criar um script do System Center Virtual Machine Manager (VMM) e adicione-o para um plano de recuperação no [do Azure Site Recovery](site-recovery-overview.md).

Publique comentários ou perguntas na parte inferior deste artigo ou no [fórum do Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="prerequisites"></a>Pré-requisitos

Pode utilizar scripts do PowerShell no seu plano de recuperação. Para ser acessível a partir do plano de recuperação, tem de criar o script e coloque o script na biblioteca do VMM. Mantenha as seguintes considerações em mente ao escrever o script:

* Certifique-se de que os scripts utilizam blocos try-catch, para que as exceções são processadas normalmente.
    - Se ocorrer uma excepção no script, o script deixa de ser executada e a tarefa mostra como falhado.
    - Se ocorrer um erro, o resto do script não é executado.
    - Se ocorrer um erro ao executar uma ativação pós-falha não planeada, o plano de recuperação continua.
    - Se ocorrer um erro ao executar uma ativação pós-falha planeada, deixa o plano de recuperação. Corrija o script, verifique se funciona como esperado e execute a recuperação planear novamente.
        - O `Write-Host` comando não funciona no script de plano de recuperação. Se utilizar o `Write-Host` comando num script, o script falha. Para criar a saída, crie um script de proxy que por sua vez, execute o script principal. Para garantir que todas as saídas é direcionada saída, utilize o  **\> \>**  comando.
        - O script exceder o tempo limite se esta aplicação não devolver num 600 segundos.
        - Se nada foi escrito em STDERR, o script é classificado como falhado. Estas informações são apresentadas nos detalhes da execução do script.

* Executar scripts num plano de recuperação no contexto da conta de serviço do VMM. Certifique-se de que esta conta tem permissões de leitura para a partilha remota, no qual o script está localizado. Teste o script a executar com o mesmo nível de direitos de utilizador à conta de serviço do VMM.
* Cmdlets do VMM são entregues por um módulo do Windows PowerShell. O módulo é instalado quando instalar a consola do VMM. Para carregar o módulo para o script, utilize o seguinte comando do script: 

    `Import-Module -Name virtualmachinemanager`

    Para obter mais informações, consulte [começar com o Windows PowerShell e o VMM](https://technet.microsoft.com/library/hh875013.aspx).
* Certifique-se de que tem pelo menos um servidor de biblioteca na sua implementação do VMM. Por predefinição, o caminho de partilha de biblioteca para um servidor do VMM está localizado localmente no servidor do VMM. O nome da pasta é MSCVMMLibrary.

  Se o caminho de partilha de biblioteca remoto (ou se é local, mas não partilhado com MSCVMMLibrary), configure a partilha de forma, utilizando \\libserver2.contoso.com\share\ como exemplo:
  
  1. Abra o Editor de registo e, em seguida, aceda a **HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\Azure Site Recovery\Registration**.

  2. Altere o valor de **ScriptLibraryPath** para  **\\\libserver2.contoso.com\share\\**. Especifique o FQDN completo. Fornece permissões para a localização da partilha. Este é o nó de raiz da partilha. Para verificar se o nó de raiz, no VMM, vá para o nó de raiz na biblioteca. O caminho que se abre é a raiz do caminho. Este é o caminho que tem de utilizar a variável.

  3. Teste o script utilizando uma conta de utilizador que tenha o mesmo nível de direitos de utilizador à conta de serviço do VMM. Utilizar estes direitos de utilizador verifica pelo autónomo, scripts testadas executadas da mesma forma que são executados nos planos de recuperação. No servidor do VMM, defina a política de execução para ignorar, da seguinte forma:

     a. Abra o **64-bit Windows PowerShell** consola como um administrador.
     
     b. Introduza **Set-executionpolicy ignorar**. Para obter mais informações, consulte [utilizando o cmdlet Set-ExecutionPolicy](https://technet.microsoft.com/library/ee176961.aspx).

     > [!IMPORTANT]
     > Definir **Set-executionpolicy ignorar** apenas na consola do PowerShell de 64 bits. Se definido para a consola do PowerShell de 32 bits, os scripts não executados.

## <a name="add-the-script-to-the-vmm-library"></a>Adicione o script para a biblioteca do VMM

Se tiver um site de origem do VMM, pode criar um script no servidor do VMM. Em seguida, inclua o script no seu plano de recuperação.

1. A partilha de biblioteca, crie uma nova pasta. Por exemplo, \<nome do servidor VMM > \MSSCVMMLibrary\RPScripts. Coloque a pasta na origem e servidores do VMM de destino.
2. Crie o script. Por exemplo, o nome do script RPScript. Certifique-se de que o script funciona conforme esperado.
3. Coloque o script no \<nome do servidor VMM > pasta \MSSCVMMLibrary nos servidores do VMM de origem e de destino.

## <a name="add-the-script-to-a-recovery-plan"></a>Adicione o script para um plano de recuperação

Depois de ter adicionado VMs ou grupos de replicação para um plano de recuperação e o plano de criado, pode adicionar o script para o grupo.

1. Abra o plano de recuperação.
2. No **passo** lista, selecione um item. Em seguida, selecione **Script** ou **ação Manual**.
3. Especifique se pretende adicionar o script ou ação antes ou depois do item selecionado. Para mover a posição do script ou reduzir verticalmente, selecione o **mover para cima** e **mover para baixo** botões.
4. Se adicionar um script do VMM, selecione **ativação pós-falha para o script VMM**. No **caminho do Script**, introduza o caminho relativo para a partilha. Por exemplo, introduza **\RPScripts\RPScript.PS1**.
5. Se adicionar um runbook de automatização do Azure, especifique a conta de automatização em que o runbook está localizado. Em seguida, selecione o script do runbook do Azure que pretende utilizar.
6. Para garantir que o script funciona conforme esperado, efetue uma ativação pós-falha do plano de recuperação.


## <a name="next-steps"></a>Passos Seguintes
* Saiba mais sobre [executar as ativações pós-falha](site-recovery-failover.md).

