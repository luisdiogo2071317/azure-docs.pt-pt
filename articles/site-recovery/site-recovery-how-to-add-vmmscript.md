---
title: "Como adicionar scripts ao plano de recuperação no Azure Site Recovery | Microsoft Docs"
description: "Descreve os pré-requisitos na adição de um script novo para um plano de recuperação no VMM no Azure"
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
ms.openlocfilehash: 60c6eebd9323028c63f42bd8a0996e3c0a2a1cf1
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2018
---
# <a name="add-vmm-scripts-to-a-recovery-plan"></a>Adicione scripts do VMM para um plano de recuperação


Este artigo fornece informações sobre a criação e adição de scripts do VMM para planos de recuperação no [do Azure Site Recovery](site-recovery-overview.md).

Publique comentários ou perguntas na parte inferior deste artigo ou no [Fórum dos Serviços de Recuperação do Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="prerequisites-of-adding-a-script-to-recovery-plan"></a>Pré-requisitos da adição de um script para o plano de recuperação

Pode utilizar scripts do PowerShell no seu plano de recuperação. Terá de criá-los e coloque-os na biblioteca do VMM ser acessível a partir do plano de recuperação. Seguem-se algumas considerações ao escrever o script.

* Certifique-se de que os scripts utilizam blocos try-catch para que as exceções são processadas normalmente.
    - Se existir uma exceção no script, deixa de ser executada e a tarefa mostra como falhado.
    - Se ocorrer um erro, qualquer parte restante do script não é executado.
    - Se ocorrer um erro ao executar uma ativação pós-falha não planeada, o plano de recuperação continua.
    - Se ocorrer um erro ao executar uma ativação pós-falha planeada, deixa o plano de recuperação. Tem de corrigir o script, verifique se funciona como esperado e execute a recuperação planear novamente.
        - O comando de anfitrião de escrita não funciona no script de plano de recuperação e o script irá falhar. Para criar a saída, crie um script de proxy que por sua vez, execute o script principal. Certifique-se de que todas as saídas é direcionada utilizando o >> comando.
        - O script exceder o tempo limite se esta aplicação não devolver num 600 segundos.
        - Se nada é escrito em STDERR, o script é classificado como falhado. Estas informações são apresentadas nos detalhes da execução do script.

* Executar scripts num plano de recuperação no contexto da conta de serviço do VMM. Certifique-se que esta conta tem permissões de leitura para a partilha remota, no qual o script está localizado. Teste o script para serem executados quando o nível de privilégios de conta de serviço do VMM.
* Cmdlets do VMM são entregues por um módulo do Windows PowerShell. O módulo é instalado quando instalar a consola do VMM. Podem ser carregado para o script, utilizando o seguinte comando do script:
   - Import-Module-Name virtualmachinemanager. [Saiba mais](https://technet.microsoft.com/library/hh875013.aspx).
* Certifique-se de que tem pelo menos um servidor de biblioteca na sua implementação do VMM. Por predefinição, o caminho de partilha de biblioteca para um servidor VMM se encontra localmente num servidor do VMM, com o nome de pasta MSCVMMLibrary.
    * Se o caminho de partilha de biblioteca remoto (ou local, mas não partilhado com MSCVMMLibrary), configurar a partilha da seguinte forma (utilizando \\libserver2.contoso.com\share\ como um exemplo):
      * Abra o Editor de registo e navegue para **HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\Azure Site Recovery\Registration**.
      * Edite o valor **ScriptLibraryPath** e coloque-o como \\libserver2.contoso.com\share\. Especifique o FQDN completo. Fornece permissões para a localização da partilha. Tenha em atenção que este é o nó de raiz da partilha. **Para verificar isto, pode procurar a biblioteca no nó raiz no VMM. O caminho que abre será a raiz do caminho - aquele que terá de utilizar a variável**.
      * Certifique-se de que testa o script com uma conta de utilizador que tem as mesmas permissões à conta de serviço do VMM. Verifica se autónomo testar scripts executadas da mesma forma como estes serão nos planos de recuperação. No servidor do VMM, defina a política de execução para ignorar da seguinte forma:
        * Abra o **64-bit Windows PowerShell** consola utilizando privilégios elevados.
        * Tipo: **Set-executionpolicy ignorar**. [Saiba mais](https://technet.microsoft.com/library/ee176961.aspx).

> [!IMPORTANT]
> Deve definir a política de execução para ignorar o PowerShell apenas de 64 bits. Se tiver definido-lo para o PowerShell de 32 bits, os scripts serão exeute não.

## <a name="add-the-script-to-the-vmm-library"></a>Adicione o script para a biblioteca do VMM

Se tiver um site de origem do VMM, pode criar um script no servidor do VMM e incluí-la no seu plano de recuperação.

1. Crie uma nova pasta na partilha de biblioteca. Por exemplo, \<Nome_servidor_vmm > \MSSCVMMLibrary\RPScripts. Coloque-o na origem e servidores do VMM de destino.
2. Criar o script (por exemplo RPScript) e verificar funciona conforme esperado.
3. Coloque o script na localização \<Nome_servidor_vmm > \MSSCVMMLibrary, nos servidores do VMM de origem e de destino.

## <a name="add-the-script-to-a-recovery-plan"></a>Adicione o script para um plano de recuperação

Pode adicionar o script para o grupo depois de ter adicionado VMs ou grupos de replicação para a mesma e criado o plano.

1. Abra o plano de recuperação.
2. Clique num item de **passo** lista e, em seguida, clique em **Script** ou **ação Manual**.
3. Especifique se pretende adicionar o script ou ação antes ou depois do item selecionado. Utilize o **mover para cima** e **mover para baixo** botões, mover a posição do script ou reduzir verticalmente.
4. Se adicionar um script do VMM, selecione **ativação pós-falha para o script VMM**. No **caminho do Script**, escreva o caminho relativo para a partilha. No exemplo VMM abaixo, especifique o caminho: **\RPScripts\RPScript.PS1**.
5. Se adicionar uma run book a automatização do Azure, especifique a conta de automatização do Azure no qual está localizado o runbook e selecione o script do runbook do Azure adequada.
6. Efetue uma ativação pós-falha do plano de recuperação, para se certificar de que o script funciona conforme esperado.


## <a name="next-steps"></a>Passos Seguintes

[Saiba mais](site-recovery-failover.md) sobre como executar as ativações pós-falha.
