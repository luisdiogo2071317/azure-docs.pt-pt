---
title: Os serviços de ambiente de trabalho remoto não está a iniciar uma VM do Azure | Documentos da Microsoft
description: Saiba como resolver problemas com os serviços de ambiente de trabalho remoto, ao ligar a uma Máquina Virtual | Documentos da Microsoft
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: cshepard
editor: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 10/23/2018
ms.author: genli
ms.openlocfilehash: a9967aec61aaab5bc6b4517407f36e2a6c7342c8
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/30/2018
ms.locfileid: "50238867"
---
# <a name="remote-desktop-services-isnt-starting-on-an-azure-vm"></a>Não é a partir de serviços de ambiente de trabalho remoto numa VM do Azure

Este artigo descreve como resolver problemas de ligação para uma Máquina Virtual do Azure (VM) quando os serviços de ambiente de trabalho remoto (TermService) não está a iniciar ou não conseguir iniciar.

>[!NOTE]
>O Azure tem dois modelos de implementação para criar e trabalhar com recursos: [Resource Manager e Clássico](../../azure-resource-manager/resource-manager-deployment-model.md). Este artigo descreve com o modelo de implementação do Resource Manager. Recomendamos que utilize este modelo para novas implementações em vez de usar o modelo de implementação clássica.

## <a name="symptoms"></a>Sintomas

Quando tentar ligar a uma VM, ocorrer os seguintes cenários:

- A captura de ecrã da VM mostra que o sistema operacional está totalmente carregado e esperar até que as credenciais.

    ![Captura de ecrã do Estado da VM](./media/troubleshoot-remote-desktop-services-issues/login-page.png)

- Remotamente ver os registos de eventos na VM com o Visualizador de eventos, verá que os serviços de ambiente de trabalho remoto (TermServ) não é a partir de ou falha ao iniciar. Segue-se um registo de exemplo:

    **Nome de registo**: sistema </br>
    **Origem**: Gestor de controlo de serviço </br>
    **Data**: 12/16/2017 11:19:36 AM</br>
    **ID de evento**: 7022</br>
    **Categoria de tarefas**: nenhum</br>
    **Nível**: erro</br>
    **Palavras-chave**: clássico</br>
    **Utilizador**: n/d</br>
    **Computador**: vm.contoso.com</br>
    **Descrição**: serviço de serviços de ambiente de trabalho remoto a suspenso sobre como iniciar. 

    Também pode utilizar a funcionalidade de consola de acesso de série para procurar por estes erros utilizando a seguinte consulta: 

        wevtutil qe system /c:1 /f:text /q:"Event[System[Provider[@Name='Service Control Manager'] and EventID=7022 and TimeCreated[timediff(@SystemTime) <= 86400000]]]" | more 

## <a name="cause"></a>Causa
 
Este problema ocorre porque os serviços de ambiente de trabalho remoto não está em execução na VM. A causa pode depender dos seguintes cenários: 

- O serviço de TermService está definido como **desativado**. 
- O serviço de TermService está a falhar ou desligar-se. 

## <a name="solution"></a>Solução

Para resolver este problema, utilize a consola de série ou [Repare a VM offline](#repair-the-vm-offline) ao anexar o disco de SO da VM para uma VM de recuperação.

### <a name="use-serial-console"></a>Utilizar a consola de série

1. Acesso a [consola de série](serial-console-windows.md) ao selecionar **suporte e resolução de problemas** > **consola de série**. Se a funcionalidade está ativada na VM, pode ligar a VM com êxito.

2. Crie um novo canal para uma instância CMD. Tipo **CMD** para iniciar o canal para obter o nome do canal.

3. Mude para o canal que executar a instância CMD. Nesse caso deve ser canal de 1.

   ```
   ch -si 1
   ```

4. Prima **Enter** novamente e introduza um nome de utilizador válido e a palavra-passe (ID de locais ou de domínio) para a VM.

5. Consulte o estado do serviço TermService.

   ```
   sc query TermService
   ```

6. Se o estado do serviço mostra **parado**, tente iniciar o serviço.

    ```
    sc start TermService
     ``` 

7. Consulte o serviço novamente para certificar-se de que o serviço é iniciado com êxito.

   ```
   sc query TermService
   ```
    Se o serviço não iniciar, siga a solução com base no erro que recebeu:

    |  Erro |  Sugestão |
    |---|---|
    |5 - ACESSO NEGADO |Consulte [TermService serviço está parado devido a erro de acesso negado](#termService-service-is-stopped-because-of-access-denied-error) |
    |1058 - ERROR_SERVICE_DISABLED  |Consulte [TermService o serviço está desativado.](#termService-service-is-disabled)  |
    |1059 - ERROR_CIRCULAR_DEPENDENCY |[Contacte o suporte](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para a sua questão rapidamente resolvida|
    |1068 - ERROR_SERVICE_DEPENDENCY_FAIL|[Contacte o suporte](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para a sua questão rapidamente resolvida|
    |1069 - ERROR_SERVICE_LOGON_FAILED  |[Contacte o suporte](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para a sua questão rapidamente resolvida    |
    |1070 - ERROR_SERVICE_START_HANG   | [Contacte o suporte](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para a sua questão rapidamente resolvida  |
    |1077 - ERROR_SERVICE_NEVER_STARTED   | Consulte [TermService o serviço está desativado](#termService-service-is-disabled)  |
    |1079 - ERROR_DIFERENCE_SERVICE_ACCOUNT   |[Contacte o suporte](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para a sua questão rapidamente resolvida |
    |1753   |[Contacte o suporte](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para a sua questão rapidamente resolvida   |

#### <a name="termservice-service-is-stopped-because-of-access-denied-error"></a>O serviço de TermService está parado devido a erro de acesso negado

1. Ligar à [consola de série](serial-console-windows.md#) e abra uma instância do PowerShell.
2. Transferir a ferramenta de Monitor do processo executando o seguinte script:

        remove-module psreadline  
        $source = "https://download.sysinternals.com/files/ProcessMonitor.zip" 
        $destination = "c:\temp\ProcessMonitor.zip" 
        $wc = New-Object System.Net.WebClient 
        $wc.DownloadFile($source,$destination) 
3. Agora a começar um rastreamento de procmon:

        procmon /Quiet /Minimized /BackingFile c:\temp\ProcMonTrace.PML 
4. Negar a reproduzir o problema ao iniciar o serviço que está a dar o acesso: 

        sc start TermService 
        
    Quando a falha, vá em frente e encerrar o rastreio de Monitor do processo:

        procmon /Terminate 
5. Recolher o arquivo **c:\temp\ProcMonTrace.PML**, abra-o utilizando procmon e, em seguida, filtrar por **resultado é o acesso NEGADO** como a seguinte captura de ecrã mostra:

    ![Filtrar por resultado no Monitor do processo](./media/troubleshoot-remote-desktop-services-issues/process-monitor-access-denined.png)

 
6. Corrigi as chaves do Registro, pastas ou ficheiros que estão na saída. Normalmente, este problema seja causado pelo registo na conta utilizada no serviço não tem permissão de ACL para aceder a esses objetos. Para saber a permissão de ACL correta para o início de sessão na conta, pode verificar uma VM em bom estado. 

#### <a name="termservice-service-is-disabled"></a>O serviço TermService está desativado

1.  Restaure o serviço para o valor de arranque predefinido:

        sc config TermService start= demand 
        
2.  Inicie o serviço:

        sc start TermService 
3.  Consultar o seu estado novamente para garantir que o serviço está em execução: consulta de sc TermService 
4.  Tente conntet a VM com o ambiente de trabalho remoto.


### <a name="repair-the-vm-offline"></a>Repare a VM offline

#### <a name="attach-the-os-disk-to-a-recovery-vm"></a>Anexar o disco do SO a uma VM de recuperação

1. [Anexar o disco do SO a uma VM de recuperação](../windows/troubleshoot-recovery-disks-portal.md).
2. Inicie uma ligação de ambiente de trabalho remoto para a VM de recuperação. Certifique-se de que o disco ligado é sinalizado de forma **Online** no console de gerenciamento de disco. Tenha em atenção a letra de unidade que está atribuída ao disco do SO anexado.
3.  Abra uma instância de linha de comandos elevada (**executar como administrador**), e, em seguida, execute o seguinte script. Partimos do princípio de que a letra de unidade que está atribuída ao disco do SO anexado é F. Substitua-o com o valor apropriado na sua VM. 

        reg load HKLM\BROKENSYSTEM F:\windows\system32\config\SYSTEM.hiv
        
        REM Set default values back on the broken service 
        reg add "HKLM\BROKENSYSTEM\ControlSet001\services\TermService" /v start /t REG_DWORD /d 3 /f
        reg add "HKLM\BROKENSYSTEM\ControlSet001\services\TermService" /v ObjectName /t REG_SZ /d "NT Authority\NetworkService“ /f
        reg add "HKLM\BROKENSYSTEM\ControlSet001\services\TermService" /v type /t REG_DWORD /d 16 /f
        reg add "HKLM\BROKENSYSTEM\ControlSet002\services\TermService" /v start /t REG_DWORD /d 3 /f
        reg add "HKLM\BROKENSYSTEM\ControlSet002\services\TermService" /v ObjectName /t REG_SZ /d "NT Authority\NetworkService" /f
        reg add "HKLM\BROKENSYSTEM\ControlSet002\services\TermService" /v type /t REG_DWORD /d 16 /f
4. [Desanexar o disco do SO e recriar a VM](../windows/troubleshoot-recovery-disks-portal.md)e, em seguida, verifique se o problema está resolvido.

## <a name="need-help-contact-support"></a>Precisa de ajuda? Contactar o suporte

Se precisar de ajuda, ainda [contacte o suporte](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para a sua questão resolvidos rapidamente.
