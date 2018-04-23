---
title: Resolver problemas de falhas durante a reativação pós-falha no local a partir do Azure e Proteja novamente para o Azure, posteriormente | Microsoft Docs
description: Este artigo descreve formas de resolver erros comuns no falhar no local a partir do Azure e durante reproteção
services: site-recovery
documentationcenter: ''
author: rajani-janaki-ram
manager: gauravd
editor: ''
ms.assetid: ''
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 12/19/2017
ms.author: rajanaki
ms.openlocfilehash: d487c1fcf7fb6444c2b8489df839a6450715ae0a
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/27/2018
---
# <a name="troubleshoot-errors-reported-during-the-process-of-failback"></a>Resolver problemas de erros comunicados durante o processo de reativação pós-falha
Reativação pós-falha essencialmente envolve dois passos principais. Um é para voltar a proteger máquinas virtuais do Azure no local, o segundo é realmente efetuar a reativação pós-falha a partir do Azure no local.

## <a name="troubleshoot-errors-when-reprotecting-a-virtual-machine-back-to-on-premises-after-failover"></a>Resolver erros ao trocar de uma máquina virtual no local, após a ativação pós-falha
Poderá receber um dos seguintes erros ao executar reproteção de uma máquina virtual para o Azure. Para resolver problemas, utilize os passos descritos para cada condição de erro.


### <a name="error-code-95226"></a>Código de erro 95226

*Reproteção falhou porque a máquina virtual do Azure não conseguiu aceder ao servidor de configuração no local.*

Isto acontece quando 
1. A máquina virtual do Azure não foi possível alcançar o servidor de configuração no local e, por conseguinte, pode não ser detetada e registada no servidor de configuração. 
2. O serviço de aplicação do InMage Scout na máquina virtual do Azure que tem de estar em execução para comunicar o servidor de configuração no local poderá não estar em execução ativação pós-falha de post.

Para resolver este problema
1. Tem de garantir que a rede da máquina virtual do Azure está configurada de forma a que a máquina virtual pode comunicar com o servidor de configuração no local. Para tal, configurar uma VPN de Site a Site para o seu centro de dados no local ou configurar uma ligação ExpressRoute com o peering privado na rede virtual da máquina virtual do Azure. 
2. Se já tiver uma rede configurada de forma a que a máquina virtual do Azure pode comunicar com o servidor de configuração no local, em seguida, iniciar sessão na máquina virtual e consulte o 'InMage Scout Application Service'. Se observar a que o InMage Scout Application Service não está em execução, em seguida, inicie manualmente o serviço e certifique-se de que o tipo de início do serviço está definido como automático.

### <a name="error-code-78052"></a>Código de erro 78052
Reproteção falha apresentando a mensagem de erro: *não foi possível concluir a proteção da máquina virtual.*

Isto pode acontecer devido a dois razões
1. A máquina virtual que são trocar é um Windows Server 2016. Atualmente este sistema operativo não é suportado para reativação pós-falha, mas será suportado em breve.
2. Já existe uma máquina virtual com o mesmo nome no mestre de servidor de destino estão a falhar novamente.

Para resolver este problema pode selecionar um servidor de destino mestre diferente num anfitrião diferente, para que o reproteção irá criar a máquina num anfitrião diferente, onde os nomes estão em conflito. Também pode vMotion o destino principal para outro anfitrião onde não acontece a colisão de nomes. Se a máquina virtual existente é uma máquina stray, pode apenas alterá-la para que a nova máquina virtual podem ser criada no mesmo anfitrião ESXi.

### <a name="error-code-78093"></a>Código de erro 78093

*A VM não está em execução, num estado bloqueado, ou não está acessível.*

Para voltar a proteger uma falha na ativação pós-falha da máquina virtual no local, terá de máquina virtual do Azure em execução. Isto é para que o serviço de mobilidade regista com o servidor de configuração no local e pode iniciar a replicação através da comunicação com o servidor de processos. Se o computador estiver numa rede incorreta ou em execução não (estado bloqueado ou encerramento), o servidor de configuração não consegue contactar o serviço de mobilidade na máquina virtual para começar a reproteção. Pode reiniciar a máquina virtual para que possa começar a comunicar back no local. Reinicie a tarefa de reproteção depois de iniciar a máquina virtual do Azure

### <a name="error-code-8061"></a>Código de erro 8061

*O arquivo de dados não está acessível a partir do anfitrião ESXi.*

Consulte o [mestra pré-requisitos de destino](site-recovery-how-to-reprotect.md#common-things-to-check-after-completing-installation-of-the-master-target-server) e o [suporta datastores](site-recovery-how-to-reprotect.md#what-datastore-types-are-supported-on-the-on-premises-esxi-host-during-failback) para reativação pós-falha


## <a name="troubleshoot-errors-when-performing-a-failback-of-an-azure-virtual-machine-back-to-on-premises"></a>Resolver erros ao executar uma reativação pós-falha da máquina virtual do Azure no local
Poderá receber um dos seguintes erros ao efetuar a reativação pós-falha da máquina virtual do Azure para o local. Para resolver problemas, utilize os passos descritos para cada condição de erro.

### <a name="error-code-8038"></a>Código de erro 8038

*Falha ao colocar a máquina de virtual no local devido ao erro*

Isto acontece quando as máquinas no local seja colocada de cópia de segurança num anfitrião que não tem memória suficiente aprovisionada.

Para resolver este problema

1. Pode aprovisionar mais memória no anfitrião ESXi.
1. vMotion a VM para outro anfitrião ESXi que tenha memória suficiente para a máquina virtual de arranque.