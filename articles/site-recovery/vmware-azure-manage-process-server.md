---
title: Gerir um servidor de processos no Azure Site Recovery | Microsoft Docs
description: Este artigo descreve gerir um servidor de processos estar configurada para a VM de VMware e replicação do servidor físico no Azure Site Recovery.
author: AnoopVasudavan
ms.service: site-recovery
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: anoopkv
ms.openlocfilehash: d1f880a5ec9f0343891999ef3bad11279cb0cfe6
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36285535"
---
# <a name="manage-process-servers"></a>Gerir servidores de processos

Por predefinição, o servidor de processo utilizado quando replicar VMs de VMware ou servidores físicos para o Azure está instalado no computador do servidor de configuração no local. Existem duas instâncias em que terá de configurar um servidor de processo separado:

- Para grandes implementações, poderá precisar de servidores de processos adicionais no local para a capacidade de dimensionar.
- Para reativação pós-falha, é necessário um temporário servidor de processos que configurar no Azure. É possível eliminar esta VM quando a reativação pós-falha é efetuada. 

Este artigo resume as tarefas de gestão típicas para estes servidores de processos adicionais.

## <a name="upgrade-a-process-server"></a>Atualizar um servidor de processos

Atualize um servidor de processos em execução no local ou no Azure (para fins de reativação pós-falha), da seguinte forma:

[!INCLUDE [site-recovery-vmware-upgrade -process-server](../../includes/site-recovery-vmware-upgrade-process-server-internal.md)]

> [!NOTE]
  Normalmente, quando utiliza a imagem de galeria do Azure para criar um servidor de processos no Azure para fins de reativação pós-falha, que está a ser executado a versão mais recente disponível. A recuperação de Site equipas versão correções e melhoramentos regulares e recomendamos que manter atualizados os servidores de processos.



## <a name="reregister-a-process-server"></a>Voltar a registar um servidor de processos

Se precisar de voltar a registar um servidor de processos em execução no local ou no Azure, com o servidor de configuração, efetue o seguinte procedimento:

[!INCLUDE [site-recovery-vmware-register-process-server](../../includes/site-recovery-vmware-register-process-server.md)]

Após ter de guardar as definições, efetue o seguinte:

1. No servidor de processos, abra uma linha de comandos de administrador.
2. Navegue até à pasta **%PROGRAMDATA%\ASR\Agent**, e execute o comando:

    ```
    cdpcli.exe --registermt
    net stop obengine
    net start obengine
    ```

## <a name="modify-proxy-settings-for-an-on-premises-process-server"></a>Modificar as definições de proxy para um servidor de processo no local

Se o servidor de processos utiliza um proxy para ligar ao Site Recovery no Azure, utilize este procedimento se precisar de modificar as definições de proxy existente.

1. Inicie sessão no computador do servidor de processo. 
2. Abra uma janela de comando do PowerShell de administrador e execute o seguinte comando:
  ```
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


