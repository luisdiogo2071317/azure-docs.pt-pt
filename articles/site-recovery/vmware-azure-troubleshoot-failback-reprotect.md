---
title: "Resolver problemas de falhas durante a reativação pós-falha de VMs do Azure para VMware no local com o Azure Site Recovery | Microsoft Docs"
description: "Este artigo descreve formas de resolver erros comuns de reativação pós-falha e só durante a reativação pós-falha para VMware a partir do Azure utilizando o Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rajani-janaki-ram
manager: gauravd
ms.service: site-recovery
ms.topic: article
ms.date: 03/09/2018
ms.author: rajanaki
ms.openlocfilehash: 480c3524ad4fb8a8c6ea02f09b8d27f254da9b08
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/16/2018
---
# <a name="troubleshoot-failback-from-azure-to-vmware"></a>Resolver problemas de reativação pós-falha a partir do Azure para VMware

Este artigo descreve como resolver problemas que poderão surgir quando houver VMs atrás do Azure para a infraestrutura do VMware no local, após a ativação pós-falha para o Azure utilizando [do Azure Site Recovery](site-recovery-overview.md).

Reativação pós-falha essencialmente envolve dois passos principais. Para o primeiro passo, após a ativação pós-falha, terá de voltar a proteger as VMs do Azure no local para que iniciar a replicação. O segundo passo consiste em executar uma ativação pós-falha do Azure para falhar novamente para o site no local.

## <a name="troubleshoot-reprotection-errors"></a>Resolver erros só

Esta secção fornece detalhes sobre os erros só comuns e como corrija-os.

### <a name="error-code-95226"></a>Código de erro 95226

**Reproteção falhou porque a máquina virtual do Azure não conseguiu aceder ao servidor de configuração no local.**

Este erro ocorre quando:

* A VM do Azure não consegue contactar o servidor de configuração no local. A VM não pode ser detetada e registada no servidor de configuração.
* O serviço de aplicação do InMage Scout não está em execução na VM do Azure após a ativação pós-falha. O serviço é necessário para as comunicações com o servidor de configuração no local.

Para resolver este problema:

* Certifique-se de que a rede VM do Azure permite que a VM do Azure comunicar com o servidor de configuração no local. Pode configurar uma VPN de site a site para o seu centro de dados no local ou configurar uma ligação ExpressRoute do Azure com o peering privado da rede virtual de VM do Azure.
* Se a VM pode comunicar com o servidor de configuração no local, iniciar sessão para a VM. Em seguida, verifique o serviço de aplicação do InMage Scout. Se vir que este não está em execução, inicie o serviço manualmente. Que tipo de iniciar o serviço de verificação está definida como **automática**.

### <a name="error-code-78052"></a>Código de erro 78052

**Não foi possível concluir a proteção da máquina virtual.**

Este problema pode ocorrer se já existir uma VM com o mesmo nome no servidor de destino principal ao qual está a reativar novamente.

Para resolver este problema:

* Selecione um servidor de destino mestre diferente no outro anfitrião para que os cria a máquina num anfitrião diferente, onde os nomes não entrar em conflito.
* Também pode utilizar vMotion para mover o destino principal para outro anfitrião onde a colisão de nomes não ocorrerá. Se a VM existente é uma máquina stray, alterá-lo para que a nova VM pode ser criada no mesmo anfitrião ESXi.


### <a name="error-code-78093"></a>Código de erro 78093

**A VM não está em execução, num estado bloqueado, ou não está acessível.**

Para resolver este problema:

Para voltar a proteger uma VM de efetuada a ativação pós-falha, a VM do Azure deve estar em execução para que regista de serviço de mobilidade com a configuração server no local e pode iniciar a replicação através da comunicação com o servidor de processos. Se a máquina num incorreta de rede ou não está em execução (suspensos Estado ou encerrar baixo), o servidor de configuração não consegue contactar o serviço de mobilidade na VM para começar a só.

* Reinicie a VM para que possa começar a comunicar back no local.
* Reinicie a tarefa de reproteção depois de iniciar a máquina virtual do Azure.

### <a name="error-code-8061"></a>Código de erro 8061

**O arquivo de dados não está acessível a partir do anfitrião ESXi.**

Verifique o [mestra pré-requisitos de destino e arquivos de dados suportados](vmware-azure-reprotect.md#deploy-a-separate-master-target-server) para reativação pós-falha.


## <a name="troubleshoot-failback-errors"></a>Resolver erros de reativação pós-falha

Esta secção descreve erros comuns que poderá encontrar durante a reativação pós-falha.

### <a name="error-code-8038"></a>Código de erro 8038

**Falha ao colocar a máquina de virtual no local devido ao erro.**

Este problema ocorre quando a VM no local seja colocada de cópia de segurança num anfitrião que não tem memória suficiente aprovisionada. 

Para resolver este problema:

* Aprovisione mais memória no anfitrião ESXi.
* Além disso, pode utilizar vMotion para mover a VM para outro anfitrião ESXi que tenha memória suficiente para efetuar o arranque a VM.
