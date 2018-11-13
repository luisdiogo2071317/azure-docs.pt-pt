---
title: Gerir um servidor de processos para recuperação após desastre de VMs de VMware e servidores físicos no Azure com o Azure Site Recovery | Documentos da Microsoft
description: Este artigo descreve gerir um servidor de processos que configurar para recuperação após desastre de VMs de VMware e de servidor físico para o Azure com o Azure Site Recovery.
author: Rajeswari-Mamilla
ms.service: site-recovery
ms.topic: conceptual
ms.date: 10/29/2018
ms.author: ramamill
ms.openlocfilehash: d99b5d1fdca39466d5e09ca077329b7ffa8622bc
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/12/2018
ms.locfileid: "51568857"
---
# <a name="manage-process-servers"></a>Gerir servidores de processos

Por predefinição, o servidor de processos utilizado quando está a replicar VMs de VMware ou servidores físicos para o Azure está instalado no computador do servidor de configuração no local. Existem duas instâncias em que precisa configurar um servidor de processo separado:

- Para grandes implementações, poderá precisar de servidores de processos no local adicionais para dimensionar a capacidade.
- Para a reativação pós-falha, terá uma temporária servidor de processos no Azure. É possível eliminar esta VM quando a reativação pós-falha é realizada. 

Este artigo resume as tarefas de gestão comuns para estes servidores de processos adicionais.

## <a name="upgrade-a-process-server"></a>Atualizar um servidor de processos

Atualize um servidor de processos em execução no local ou no Azure (para fins de reativação pós-falha), da seguinte forma:

[!INCLUDE [site-recovery-vmware-upgrade -process-server](../../includes/site-recovery-vmware-upgrade-process-server-internal.md)]

> [!NOTE]
  Normalmente, quando utiliza a imagem de galeria do Azure para criar um servidor de processos no Azure para fins de reativação pós-falha, ele é executado a versão mais recente disponível. A recuperação de Site equipas versão correções e aprimoramentos em intervalos regulares, e recomendamos que mantenha atualizados os servidores de processos.



## <a name="reregister-a-process-server"></a>Voltar a registar um servidor de processos

Se precisar de voltar a registar um servidor de processos em execução no local ou no Azure, com o servidor de configuração, faça o seguinte:

[!INCLUDE [site-recovery-vmware-register-process-server](../../includes/site-recovery-vmware-register-process-server.md)]

Depois de salvar as definições, efetue o seguinte:

1. No servidor de processos, abra uma linha de comandos de administrador.
2. Navegue até à pasta **%PROGRAMDATA%\ASR\Agent**, e execute o comando:

    ```
    cdpcli.exe --registermt
    net stop obengine
    net start obengine
    ```

## <a name="modify-proxy-settings-for-an-on-premises-process-server"></a>Modificar definições de proxy para um servidor de processos no local

Se o servidor de processos utiliza um proxy para ligar ao Site Recovery no Azure, utilize este procedimento se precisar de modificar as definições de proxy existentes.

1. Inicie sessão no computador do servidor de processo. 
2. Abra uma janela de comando do PowerShell de administrador e execute o seguinte comando:
  ```powershell
  $pwd = ConvertTo-SecureString -String MyProxyUserPassword
  Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber –ProxyUserName domain\username -ProxyPassword $pwd
  net stop obengine
  net start obengine
  ```
2. Navegue até à pasta **%PROGRAMDATA%\ASR\Agent**, e execute o seguinte comando:
  ```
  cmd
  cdpcli.exe --registermt

  net stop obengine

  net start obengine

  exit
  ```


## <a name="remove-a-process-server"></a>Remover um servidor de processos

[!INCLUDE [site-recovery-vmware-unregister-process-server](../../includes/site-recovery-vmware-unregister-process-server.md)]

## <a name="manage-anti-virus-software-on-process-servers"></a>Gerir o software de antivírus em servidores de processos

Se o software antivírus estiver ativo no servidor de processo do autónomo ou num servidor de destino mestre, exclua as seguintes pastas de software antivírus operações:


- C:\Program Files\Microsoft Azure Recovery Services Agent
- C:\ProgramData\ASR
- C:\ProgramData\ASRLogs
- C:\ProgramData\ASRSetupLogs
- C:\ProgramData\LogUploadServiceLogs
- C:\ProgramData\Microsoft Azure Site Recovery
- Diretório de instalação de servidor de processo, exemplo: C:\Program Files (x86) \Microsoft Azure Site Recovery

