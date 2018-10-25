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
ms.openlocfilehash: 39b793e2722766f3f28829b4dc48534054abd97e
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/24/2018
ms.locfileid: "49989249"
---
# <a name="remote-desktop-services-is-not-starting-on-an-azure-vm"></a>Os serviços de ambiente de trabalho remoto não está a iniciar uma VM do Azure

Este artigo descreve como resolver problemas de ligação para uma Máquina Virtual do Azure (VM) quando os serviços de ambiente de trabalho remoto (TermService) não está a iniciar ou falha ao iniciar.

>[!NOTE]
>O Azure tem dois modelos de implementação para criar e trabalhar com recursos: [Resource Manager e Clássico](../../azure-resource-manager/resource-manager-deployment-model.md). Este artigo descreve com o modelo de implementação do Resource Manager. Recomendamos que utilize este modelo para novas implementações em vez de usar o modelo de implementação clássica.

## <a name="symptoms"></a>Sintomas

Quando tentar ligar a uma VM, ocorrer os seguintes cenários:

- A captura de ecrã da VM mostra que o sistema operacional está totalmente carregado e esperar até que as credenciais.
- Todas as aplicações na VM estão a funcionar conforme esperado e está acessível.
- A VM está a responder para a conectividade TCP na porta Microsoft protocolo RDP (Remote Desktop) (predefinição 3389).
- Não é pedidas as credenciais quando tentar fazer uma ligação RDP.

## <a name="cause"></a>Causa

Este problema ocorre porque os serviços de ambiente de trabalho remoto não está em execução na máquina Virtual. A causa pode depender dos seguintes cenários:

- O serviço de TermService foi definido para **desativado**.
- O serviço de TermService está a falhar ou desligar-se.

## <a name="solution"></a>Solução

Para resolver este problema, utilize uma das seguintes soluções ou experimente as soluções individualmente:

### <a name="solution-1-using-the-serial-console"></a>Solução 1: Utilizar a consola de série

1. Acesso a [consola de série](serial-console-windows.md) ao selecionar **suporte e resolução de problemas** > **consola de série (pré-visualização)**. Se a funcionalidade está ativada na VM, pode ligar a VM com êxito.

2. Crie um novo canal para uma instância CMD. Tipo **CMD** para iniciar o canal para obter o nome do canal.

3. Mude para o canal que executar a instância CMD, nesse caso deve ser canal de 1.

   ```
   ch -si 1
   ```

4. Prima **Enter** novamente e escreva um nome de utilizador válido e palavra-passe (ID de locais ou de domínio) para a VM.

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

### <a name="solution-2-using-a-recovery-vm-to-enable-the-service"></a>Solução 2: Utilizar uma VM de recuperação para ativar o serviço

[Cópia de segurança de disco do SO](../windows/snapshot-copy-managed-disk.md) e [anexar o disco do SO para uma VM de resgate](../windows/troubleshoot-recovery-disks-portal.md). Em seguida, abra uma instância CMD elevada e execute os seguintes scripts na VM em ação:

>[!NOTE]
>Partimos do princípio de que a letra de unidade que está atribuída ao disco do SO anexado é F. Substitua-o com o valor apropriado na sua VM. Depois de o fazer, desanexe o disco da VM de recuperação e [voltar a criar a VM](../windows/create-vm-specialized.md). Para mais de resolução de problemas, pode usar **solução 1** porque a consola de série foi ativada.

```
reg load HKLM\BROKENSYSTEM f:\windows\system32\config\SYSTEM

REM Enable Serial Console
bcdedit /store <Volume Letter Where The BCD Folder Is>:\boot\bcd /set {bootmgr} displaybootmenu yes
bcdedit /store <Volume Letter Where The BCD Folder Is>:\boot\bcd /set {bootmgr} timeout 10
bcdedit /store <Volume Letter Where The BCD Folder Is>:\boot\bcd /set {bootmgr} bootems yes
bcdedit /store <Volume Letter Where The BCD Folder Is>:\boot\bcd /ems {<Boot Loader Identifier>} ON
bcdedit /store <Volume Letter Where The BCD Folder Is>:\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200

REM Get the current ControlSet from where the OS is booting
for /f "tokens=3" %x in ('REG QUERY HKLM\BROKENSYSTEM\Select /v Current') do set ControlSet=%x
set ControlSet=%ControlSet:~2,1%

REM Suggested configuration to enable OS Dump
set key=HKLM\BROKENSYSTEM\ControlSet00%ControlSet%\Control\CrashControl
REG ADD %key% /v CrashDumpEnabled /t REG_DWORD /d 2 /f
REG ADD %key% /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f
REG ADD %key% /v NMICrashDump /t REG_DWORD /d 1 /f

REM Set default values back on the broken service
reg add "HKLM\BROKENSYSTEM\ControlSet001\services\<Process Name>" /v start /t REG_DWORD /d <Startup Type> /f
reg add "HKLM\BROKENSYSTEM\ControlSet001\services\<Process Name>" /v ImagePath /t REG_EXPAND_SZ /d "<Image Path>" /f
reg add "HKLM\BROKENSYSTEM\ControlSet001\services\<Process Name>" /v ObjectName /t REG_SZ /d "<Startup Account>" /f
reg add "HKLM\BROKENSYSTEM\ControlSet001\services\<Process Name>" /v type /t REG_DWORD /d 16 /f
reg add "HKLM\BROKENSYSTEM\ControlSet002\services\<Process Name>" /v start /t REG_DWORD /d <Startup Type> /f
reg add "HKLM\BROKENSYSTEM\ControlSet002\services\<Process Name>" /v ImagePath /t REG_EXPAND_SZ /d "<Startup Account>" /f
reg add "HKLM\BROKENSYSTEM\ControlSet002\services\<Process Name>" /v ObjectName /t REG_SZ /d "<Startup Account>" /f
reg add "HKLM\BROKENSYSTEM\ControlSet002\services\<Process Name>" /v type /t REG_DWORD /d 16 /f

REM Enable default dependencies from the broken service
reg add "HKLM\BROKENSYSTEM\ControlSet001\services\<Driver/Service Name>" /v start /t REG_DWORD /d <Startup Type> /f
reg add "HKLM\BROKENSYSTEM\ControlSet002\services\<Driver/Service Name>" /v start /t REG_DWORD /d <Startup Type> /f
reg unload HKLM\BROKENSYSTEM
```

## <a name="need-help-contact-support"></a>Precisa de ajuda? Contactar o suporte

Se precisar de ajuda, ainda [contacte o suporte](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para a sua questão resolvidos rapidamente.
