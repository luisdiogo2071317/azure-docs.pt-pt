---
title: RDP falha se nenhum servidor de licenças de ambiente de trabalho remoto está disponível para fornecer uma licença no Azure | Documentos da Microsoft
description: Aprenda a solucionar problemas do RDP falhar porque não está disponível nenhum servidor de licenciamento de área de trabalho remota | Documentos da Microsoft
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
ms.openlocfilehash: 659f2866e56762e4dec70fac4ce4074fc24b18cb
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/24/2018
ms.locfileid: "49989208"
---
# <a name="rdp-fails-if-no-remote-desktop-license-server-is-available-in-azure"></a>RDP falha se nenhum servidor de licenças de ambiente de trabalho remoto está disponível no Azure

Este artigo ajuda a resolver o problema em que não é possível ligar a uma Máquina Virtual do Azure (VM), porque nenhum servidor de licenças de ambiente de trabalho remoto não está disponível para fornecer uma licença.

## <a name="symptoms"></a>Sintomas

Quando tentar ligar a uma VM, ocorrer os seguintes cenários:

- A captura de ecrã da Máquina Virtual (VM) mostra que o sistema operacional está totalmente carregado e esperar até que as credenciais.
- As seguintes mensagens de erro é exibida ao tentar estabelecer uma ligação de Microsoft protocolo RDP (Remote Desktop):

  - **A sessão remota foi desligada porque existem não existem servidores de licença de ambiente de trabalho remoto disponíveis para fornecer uma licença.**

  - **Nenhum servidor de licenças de ambiente de trabalho remoto está disponível. Os serviços de ambiente de trabalho remoto para de funcionar porque este computador passou o respetivo período de tolerância e não tem contactar a, pelo menos, um servidor de licenças do Windows Server 2008 válido. Clique nesta mensagem para abrir a configuração do servidor de anfitrião de sessão do RD para utilizar o diagnóstico de licenciamento.**

No entanto, pode ligar à VM normalmente através de uma sessão administrativa:

```
mstsc /v:<Server>[:<Port>] /admin
```

## <a name="cause"></a>Causa

Este problema ocorre se um servidor de licenças de ambiente de trabalho remoto não está disponível para fornecer uma licença para iniciar uma sessão remota. Esse problema pode ser causado por vários cenários, apesar de uma função de anfitrião de sessões de ambiente de trabalho remoto foi configurada na VM:

- Nunca houve uma função de licenciamento de ambiente de trabalho remoto no ambiente e o período de tolerância (180 dias) está acima.
- Uma licença de ambiente de trabalho remoto foi instalada no ambiente, mas nunca é ativado.
- Uma licença de ambiente de trabalho remoto no ambiente não tem licenças de acesso de cliente (CALs) injetadas para configurar a ligação.
- Uma licença de ambiente de trabalho remoto foi instalada no ambiente. Há CALs disponíveis, mas eles não foram configurados corretamente.
- Uma licença de ambiente de trabalho remoto tem CALs e ele foi ativado. No entanto, há alguns outros problemas no servidor de licenças de ambiente de trabalho remoto que impedem-lo de licenças no ambiente.

## <a name="solution"></a>Solução

Para resolver este problema, [cópia de segurança de disco do SO](../windows/snapshot-copy-managed-disk.md) e siga estes passos:

1. Ligar à VM através de uma sessão administrativa:

   ```
   mstsc /v:<Server>[:<Port>] /admin
   ```

  Se não conseguir ligar à VM através de uma sessão administrativa, pode utilizar o [consola de série](serial-console-windows.md) para aceder à VM da seguinte forma:

  1. Aceder à consola de série, selecionando **suporte e resolução de problemas** > **consola de série (pré-visualização)**. Se a funcionalidade está ativada na VM, pode ligar a VM com êxito.

  2. Crie um novo canal para uma instância CMD. Tipo **CMD** para iniciar o canal para obter o nome do canal.

  3. Mude para o canal que executar a instância CMD, nesse caso deve ser canal de 1.

    ```
    ch -si 1
    ```

  4. Prima **Enter** novamente e escreva um nome de utilizador válido e palavra-passe (ID de locais ou de domínio) para a VM.

2. Verifique se a VM tem uma função de anfitrião de sessões de ambiente de trabalho remoto ativada. Se a função estiver ativada, certifique-se de que está a funcionar corretamente. Abra uma instância CMD elevada e siga estes passos:

  1. Utilize o seguinte comando para verificar o estado da função de anfitrião de sessões de ambiente de trabalho remoto:

    ```
    reg query "HKLM\SOFTWARE\Microsoft\ServerManager\ServicingStorage\ServerComponentCache\RDS-RD-Server" /v InstallState
    ```

    Se este comando devolve um valor de 0, significa que a função está desativada, e pode ir para o passo 3.

  2. Utilize o seguinte comando para verificar as políticas e reconfigurar conforme necessário:

    ```
    reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\RCM\Licensing Core" /v LicensingMode reg query "HKLM\SYSTEM\CurrentControlSet\Services\TermService\Parameters" /v SpecifiedLicenseServers
    ```

    Se o **LicensingMode** valor é definido como qualquer outro valor que 4 (por utilizador), em seguida, defina o valor como 4:

    ```
    reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\RCM\Licensing Core" /v LicensingMode /t REG_DWORD /d 4
    ```

    Se o **SpecifiedLicenseServers** valor não existe ou tem um informações do servidor de licenças incorreto, em seguida, altere-o da seguinte forma:

    ```
    reg add "HKLM\SYSTEM\CurrentControlSet\Services\TermService\Parameters" /v SpecifiedLicenseServers /t REG_MULTI_SZ /d "<FQDN / IP License server>"
    ```

  3. Depois de efetuar quaisquer alterações no Registro, reinicie a VM.

  4. Se não tem CALs, remova a função de anfitrião de sessões de ambiente de trabalho remoto. Em seguida, o RDP será definido volta ao normal e só permite duas ligações de RDP em simultâneo para a VM.

    ```
    dism /ONLINE /Disable-feature /FeatureName:Remote-Desktop-Services
    ```

    Se a VM tem a função de licenciamento de ambiente de trabalho remoto e não é utilizado, também pode remover essa função.

    ```
    dism /ONLINE /Disable-feature /FeatureName:Licensing
    ```

  5. Certifique-se de que a VM pode ligar ao servidor de licenças de ambiente de trabalho remoto. Pode testar a conectividade com a porta 135 entre a VM e o servidor de licenças.

    ```
    telnet <FQDN / IP License Server> 135
    ```

3. Se não existe nenhum servidor de licenciamento de ambiente de trabalho remoto no ambiente e desejar uma, pode [instalar um serviço de função de licenciamento de ambiente de trabalho remoto ](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731765(v=ws.11))e, em seguida [configurar o licenciamento RDS](https://blogs.technet.microsoft.com/askperf/2013/09/20/rd-licensing-configuration-on-windows-server-2012/).

4. Se um servidor de licenças de ambiente de trabalho remoto está configurado e em bom estado, certifique-se de que o servidor de licenças de ambiente de trabalho remoto está ativado com CALs.

## <a name="need-help-contact-support"></a>Precisa de ajuda? Contactar o suporte

Se precisar de ajuda, ainda [contacte o suporte](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para a sua questão resolvidos rapidamente.
