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
ms.openlocfilehash: 701373efa3c3c22eb5969705927e1c0cc6e3f36b
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/29/2018
ms.locfileid: "50215805"
---
# <a name="troubleshoot-an-rdp-general-error-in-azure-vm"></a>Resolver problemas relacionados com um erro geral do RDP na VM do Azure

Este artigo descreve um erro geral que pode ocorrer quando efetuar uma ligação de protocolo de ambiente de trabalho remoto (RDP) para uma Máquina Virtual de Windows (VM) no Azure.

## <a name="symptoms"></a>Sintomas

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

Para resolver este problema, [cópia de segurança de disco do sistema operativo](../windows/snapshot-copy-managed-disk.md), e [anexar o disco do sistema operativo para uma VM de resgate](troubleshoot-recovery-disks-portal-windows.md)e, em seguida, siga as opções de solução em conformidade ou experimente as soluções individualmente.

### <a name="serial-console"></a>Consola de série

#### <a name="step-1-turn-on-remote-desk"></a>Passo 1: Ativar o suporte técnico remoto

1. Acesso a [consola de série](serial-console-windows.md) ao selecionar **suporte e resolução de problemas** > **consola de série (pré-visualização)**. Se a funcionalidade está ativada na VM, pode ligar a VM com êxito.

2. Crie um novo canal para uma instância CMD. Tipo **CMD** para iniciar o canal para obter o nome do canal.

3. Mude para o canal que executar a instância CMD, nesse caso deve ser canal de 1.

   ```
   ch -si 1
   ```

4. Ative o ambiente de trabalho remoto através da política de GPO, alterando a seguinte política:

   ```
   Computer Configuration\Policies\Administrative Templates: Policy definitions\Windows Components\Remote Desktop Services\Remote Desktop Session Host\Connections\Allow users to connect remotely by using Remote Desktop Services
   ```

5. Algumas chaves de registo que podem causar esse problema, verifique os valores das chaves de registo da seguinte forma:

   1. Certifique-se de que o componente RDP está ativado.

      ```
      REM Get the local policy
      reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server " /v fDenyTSConnections

      REM Get the domain policy if any
      reg query "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v fDenyTSConnections
      ```

      Se existir a diretiva de domínio, a configuração na política local será substituída.

         - Se a política de domínio indica que o RDP é desativado (1), em seguida, atualizar a política do AD.
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

#### <a name="step-1-turn-on-remote-desk"></a>Passo 1: Ativar o suporte técnico remoto

> [!NOTE]  
> Partimos do princípio de que a letra de unidade que está atribuída ao disco do SO anexado é F. Substitua-o com o valor apropriado na sua VM. Os hives do sistema e o SOFTWARE tem de ser desmontados e, em seguida, montado.

1. Abra uma instância CMD elevada e execute os seguintes scripts na VM em ação:

   ```
   reg load HKLM\BROKENSYSTEM f:\windows\system32\config\SYSTEM.hiv
   reg load HKLM\BROKENSOFTWARE f:\windows\system32\config\SOFTWARE.hiv

   REM Ensure that Terminal Server is enabled
   reg add "HKLM\BROKENSYSTEM\ControlSet001\Control\Terminal Server" /v TSEnabled /t REG_DWORD /d 1 /f
   reg add "HKLM\BROKENSYSTEM\ControlSet002\Control\Terminal Server" /v TSEnabled /t REG_DWORD /d 1 /f

   REM Ensure Terminal Service is not set to Drain mode
   reg add "HKLM\BROKENSYSTEM\ControlSet001\Control\Terminal Server" /v TSServerDrainMode /t REG_DWORD /d 0 /f
   reg add "HKLM\BROKENSYSTEM\ControlSet002\Control\Terminal Server" /v TSServerDrainMode /t REG_DWORD /d 0 /f

   REM Ensure Terminal Service has logon enabled
   reg add "HKLM\BROKENSYSTEM\ControlSet001\Control\Terminal Server" /v TSUserEnabled /t REG_DWORD /d 0 /f

   REM Ensure the RDP Listener is not disabled
   reg add "HKLM\BROKENSYSTEM\ControlSet001\Control\Terminal Server\Winstations\RDP-Tcp" /v fEnableWinStation /t REG_DWORD /d 1 /f
   reg add "HKLM\BROKENSYSTEM\ControlSet002\Control\Terminal Server\Winstations\RDP-Tcp" /v fEnableWinStation /t REG_DWORD /d 1 /f

   REM Ensure the RDP Listener accepts logons
   reg add "HKLM\BROKENSYSTEM\ControlSet001\Control\Terminal Server\Winstations\RDP-Tcp" /v fLogonDisabled /t REG_DWORD /d 0 /f
   reg add "HKLM\BROKENSYSTEM\ControlSet002\Control\Terminal Server\Winstations\RDP-Tcp" /v fLogonDisabled /t REG_DWORD /d 0 /f

   REM RDP component is enabled
   reg add "HKLM\BROKENSYSTEM\ControlSet001\Control\Terminal Server" /v fDenyTSConnections /t REG_DWORD /d 0 /f
   reg add "HKLM\BROKENSYSTEM\ControlSet002\Control\Terminal Server" /v fDenyTSConnections /t REG_DWORD /d 0 /f
   reg add "HKLM\BROKENSOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v fDenyTSConnections /t REG_DWORD /d 0 /f

   reg unload HKLM\BROKENSYSTEM
   reg unload HKLM\BROKENSOFTWARE
   ```

2. Os hives do sistema e o SOFTWARE de montagem.

   ```
   reg load HKLM\BROKENSYSTEM f:\windows\system32\config\SYSTEM
   reg load HKLM\BROKENSOFTWARE f:\windows\system32\config\SOFTWARE
   ```

3. Se a máquina virtual está associado a um domínio, foi possível desativar o RDP num nível de política. Para confirmar se ele for o caso, verifique a chave de registo seguinte:

   ```
   HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services\fDenyTSConnectionS
   ```

4. Se este valor de chave é definido como 1, RDP está desativado pela política.

5. Ative o ambiente de trabalho remoto através da política de GPO, alterando a seguinte política:

   ```
   Computer Configuration\Policies\Administrative Templates: Policy definitions\Windows Components\Remote Desktop Services\Remote Desktop Session Host\Connections\Allow users to connect remotely by using Remote Desktop Services
   ```

6. Se esta chave de registo não existir, verifique a chave de registo seguinte:

   ```
   HKLM\System\CurrentControlSet\Control\Terminal Server\fDenyTSConnections
   ```

7. Se esta chave é definida como 1, RDP é a vez desativado. Altere o valor da chave para 0.
8. Desanexe o disco da VM entra em ação.
9. [Criar uma nova VM a partir do disco](../windows/create-vm-specialized.md).

Se o problema ainda acontece, mova para o passo 2.

#### <a name="step-2-enable-remote-desktop-services"></a>Passo 2: Ativar os serviços de ambiente de trabalho remoto

Para obter mais informações, consulte [dos serviços de ambiente de trabalho remoto não está a iniciar numa VM do Azure](troubleshoot-remote-desktop-services-issues.md).

#### <a name="step-3-reset-rdp-listener"></a>Passo 3: Repor o serviço de escuta do RDP

Para obter mais informações, consulte [área de trabalho remota se desliga com frequência na VM do Azure](troubleshoot-rdp-intermittent-connectivity.md).

## <a name="need-help-contact-support"></a>Precisa de ajuda? Contactar o suporte

Se precisar de ajuda, ainda [contacte o suporte](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para a sua questão resolvidos rapidamente.
