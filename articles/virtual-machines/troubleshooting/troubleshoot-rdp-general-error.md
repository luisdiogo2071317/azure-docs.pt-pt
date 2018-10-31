---
title: Resolver problemas relacionados com um erro geral do RDP para uma VM do Windows no Azure | Documentos da Microsoft
description: Saiba como resolver problemas relacionados com um erro geral do RDP para uma VM do Windows no Azure | Documentos da Microsoft
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
ms.date: 10/30/2018
ms.author: genli
ms.openlocfilehash: 7f5e1f2141a58f666367d253d5fc313499e64c9f
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/30/2018
ms.locfileid: "50239395"
---
# <a name="troubleshoot-an-rdp-general-error-in-azure-vm"></a>Resolver problemas relacionados com um erro geral do RDP na VM do Azure

Este artigo descreve um erro geral que pode ocorrer quando efetuar uma ligação de protocolo de ambiente de trabalho remoto (RDP) para uma Máquina Virtual de Windows (VM) no Azure.

## <a name="symptom"></a>Sintoma

Quando efetuar uma ligação RDP a uma VM de janela no Azure, poderá receber a seguinte mensagem de erro geral:

**Ambiente de trabalho remoto não é possível ligar ao computador remoto para um dos seguintes motivos:**

1. **Acesso remoto para o servidor não está ativado**

2. **O computador remoto está desativado**

3. **O computador remoto não está disponível na rede**

**Certificar-se de que o computador remoto está ativado e ligado à rede e que o acesso remoto está ativado.**

## <a name="cause"></a>Causa

Esse problema pode ocorrer devido aos seguintes causas:

### <a name="cause-1"></a>Causa 1

O componente RDP está desativado, da seguinte forma:

- Ao nível do componente
- No nível de serviço de escuta
- No servidor de terminal
- A função de anfitrião de sessões de ambiente de trabalho remoto

### <a name="cause-2"></a>Causa 2

Os serviços de ambiente de trabalho remoto (TermService) não está em execução.

### <a name="cause-3"></a>Causa 3

O serviço de escuta RDP está configurado incorretamente.

## <a name="solution"></a>Solução

Para resolver este problema, [cópia de segurança de disco do sistema operativo](../windows/snapshot-copy-managed-disk.md), e [anexar o disco do sistema operativo para uma VM de resgate](troubleshoot-recovery-disks-portal-windows.md)e, em seguida, siga os passos.

### <a name="serial-console"></a>Consola de série

#### <a name="step-1-turn-on-remote-deskop"></a>Passo 1: Ativar o ambiente remoto

1. Acesso a [consola de série](serial-console-windows.md) ao selecionar **suporte e resolução de problemas** > **consola de série (pré-visualização)**. Se a funcionalidade está ativada na VM, pode ligar a VM com êxito.

2. Crie um novo canal para uma instância CMD. Tipo **CMD** para iniciar o canal para obter o nome do canal.

3. Mude para o canal que executar a instância CMD, nesse caso deve ser canal de 1.

   ```
   ch -si 1
   ```
4. Verifique os valores das chaves de registo da seguinte forma:

   1. Certifique-se de que o componente RDP está ativado.

      ```
      REM Get the local policy
      reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server " /v fDenyTSConnections

      REM Get the domain policy if any
      reg query "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v fDenyTSConnections
      ```

      Se existir a diretiva de domínio, a configuração na política local será substituída.

         - Se a política de domínio indica que o RDP é desativado (1), em seguida, política de atualização do AD do controlador de domínio.
         - Se a política de domínio afirma que o RDP é ativado (0), não é necessária nenhuma atualização.

      Se não existe a diretiva de domínio e a política local indica que o RDP está desabilitado (1), ative o protocolo RDP, utilizando o seguinte comando:

         ```
         reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v fDenyTSConnections /t REG_DWORD /d 0 /f
         ```

   2. Verifique a configuração atual do servidor de terminal.

      ```
      reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v TSEnabled
      ```

   3. Se o comando devolve 0, o servidor de terminal está desativado. Em seguida, ative o servidor de terminal da seguinte forma:

      ```
      reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v TSEnabled /t REG_DWORD /d 1 /f
      ```

   4. O módulo de servidor de Terminal está definido para drenar modo, se o servidor estiver num farm de servidor de terminal (RDS ou Citrix). Verifique o modo atual do módulo do Terminal Server.

      ```
      reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v TSServerDrainMode
      ```

   5. Se o comando devolve 1, o módulo de servidor de Terminal está definido para drenar modo. Defina o módulo para o modo de trabalho da seguinte forma:

      ```
      reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v TSServerDrainMode /t REG_DWORD /d 0 /f
      ```

   6. Verifique se pode ligar ao servidor de terminal.

      ```
      reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v TSUserEnabled
      ```

   7. Se o comando devolve 1, não é possível ligar ao servidor de terminal. Em seguida, ative a ligação da seguinte forma:

      ```
      reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v TSUserEnabled /t REG_DWORD /d 0 /f
      ```

   8. Verifique a configuração atual do serviço de escuta RDP.

      ```
      reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp" /v fEnableWinStation
      ```

   9. Se o comando devolve 0, o serviço de escuta RDP está desativado. Em seguida, ative o serviço de escuta da seguinte forma:

      ```
      reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp" /v fEnableWinStation /t REG_DWORD /d 1 /f
      ```

   10. Verifique se pode ligar ao serviço de escuta RDP.

      ```
      reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp" /v fLogonDisabled
      ```

   11. Se o comando devolve 1, não é possível ligar ao serviço de escuta RDP. Em seguida, ative a ligação da seguinte forma:

      ```
      reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp" /v fLogonDisabled /t REG_DWORD /d 0 /f
      ```

6. Reinicie a VM.

7. Saída da instância de CMD, escrevendo `exit`e, em seguida, prima **Enter** duas vezes.

8. Reinicie a VM ao escrever `restart`.

Se o problema ainda acontece, mova para o passo 2.

#### <a name="step-2-enable-remote-desktop-services"></a>Passo 2: Ativar os serviços de ambiente de trabalho remoto

Para obter mais informações, consulte [dos serviços de ambiente de trabalho remoto não está a iniciar numa VM do Azure](troubleshoot-remote-desktop-services-issues.md).

#### <a name="step-3-reset-rdp-listener"></a>Passo 3: Repor o serviço de escuta do RDP

Para obter mais informações, consulte [área de trabalho remota se desliga com frequência na VM do Azure](troubleshoot-rdp-intermittent-connectivity.md).

### <a name="offline-repair"></a>Reparação offline

#### <a name="step-1-turn-on-remote-deskop"></a>Passo 1: Ativar o ambiente remoto

1. [Anexar o disco do SO a uma VM de recuperação](../windows/troubleshoot-recovery-disks-portal.md).
2. Inicie uma ligação de ambiente de trabalho remoto para a VM de recuperação.
3. Certifique-se de que o disco é sinalizado de forma **Online** no console de gerenciamento de disco. Tenha em atenção a letra de unidade que está atribuída ao disco do SO anexado.
3. Inicie uma ligação de ambiente de trabalho remoto para a VM de recuperação.
4. Abra uma sessão de linha de comandos elevada (**executar como administrador**). Execute os seguintes scripts. Nesse script, partimos do princípio de que a letra de unidade que está atribuída ao disco do SO anexado é F. Substitua esta letra de unidade com o valor apropriado para a sua VM.

      ```
      reg load HKLM\BROKENSYSTEM F:\windows\system32\config\SYSTEM.hiv 
      reg load HKLM\BROKENSOFTWARE F:\windows\system32\config\SOFTWARE.hiv 
 
      REM Ensure that Terminal Server is enabled 

      reg add "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server" /v TSEnabled /t REG_DWORD /d 1 /f 
      reg add "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server" /v TSEnabled /t REG_DWORD /d 1 /f 

      REM Ensure Terminal Service is not set to Drain mode 
      reg add "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server" /v TSServerDrainMode /t REG_DWORD /d 0 /f 
      reg add "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server" /v TSServerDrainMode /t REG_DWORD /d 0 /f 

      REM Ensure Terminal Service has logon enabled 
      reg add "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server" /v TSUserEnabled /t REG_DWORD /d 0 /f 
      reg add "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server" /v TSUserEnabled /t REG_DWORD /d 0 /f 

      REM Ensure the RDP Listener is not disabled 
      reg add "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v fEnableWinStation /t REG_DWORD /d 1 /f 
      reg add "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v fEnableWinStation /t REG_DWORD /d 1 /f 

      REM Ensure the RDP Listener accepts logons 
      reg add "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v fLogonDisabled /t REG_DWORD /d 0 /f 
      reg add "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v fLogonDisabled /t REG_DWORD /d 0 /f 

      REM RDP component is enabled 
      reg add "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server" /v fDenyTSConnections /t REG_DWORD /d 0 /f 
      reg add "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server" /v fDenyTSConnections /t REG_DWORD /d 0 /f 
      reg add "HKLM\BROKENSOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v fDenyTSConnections /t REG_DWORD /d 0 /f 

      reg unload HKLM\BROKENSYSTEM 
      reg unload HKLM\BROKENSOFTWARE 
      ```

3. Se a VM estiver associado a um domínio, consulte a seguinte chave de registo para ver se existe uma política de grupo que desativa o RDP. 

   ```
   HKLM\BROKENSOFTWARE\Policies\Microsoft\Windows NT\Terminal Services\fDenyTSConnectionS
   ```


      Se este valor de chave é definido como 1, isso significa que o RDP está desativada pela política. Para ativar o ambiente de trabalho remoto através da política de GPO, altere a seguinte política do controlador de domínio:

   ```
   Computer Configuration\Policies\Administrative Templates: Policy definitions\Windows Components\Remote Desktop Services\Remote Desktop Session Host\Connections\Allow users to connect remotely by using Remote Desktop Services
   ```

4. Desanexe o disco da VM entra em ação.
5. [Criar uma nova VM a partir do disco](../windows/create-vm-specialized.md).

Se o problema ainda acontece, mova para o passo 2.

#### <a name="step-2-enable-remote-desktop-services"></a>Passo 2: Ativar os serviços de ambiente de trabalho remoto

Para obter mais informações, consulte [dos serviços de ambiente de trabalho remoto não está a iniciar numa VM do Azure](troubleshoot-remote-desktop-services-issues.md).

#### <a name="step-3-reset-rdp-listener"></a>Passo 3: Repor o serviço de escuta do RDP

Para obter mais informações, consulte [área de trabalho remota se desliga com frequência na VM do Azure](troubleshoot-rdp-intermittent-connectivity.md).

## <a name="need-help-contact-support"></a>Precisa de ajuda? Contactar o suporte

Se precisar de ajuda, ainda [contacte o suporte](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para a sua questão resolvidos rapidamente.
