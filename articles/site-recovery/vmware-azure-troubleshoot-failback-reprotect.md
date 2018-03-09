---
title: "Resolver problemas de falhas durante a reativação pós-falha de VMs do Azure para VMware no local com o Azure Site Recovery | Microsoft Docs"
description: "Este artigo descreve formas de resolver erros comuns de reativação pós-falha e só durante a reativação pós-falha para VMware a partir do Azure, utilizando o Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rajani-janaki-ram
manager: gauravd
ms.service: site-recovery
ms.topic: article
ms.date: 03/05/2017
ms.author: rajanaki
ms.openlocfilehash: 1c54ae96273880caede1f50f3a0705c41f15f26e
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/08/2018
---
# <a name="troubleshoot-failback-from-azure-to-vmware"></a>Resolver problemas de reativação pós-falha a partir do Azure para VMware

Este artigo descreve como resolver problemas que poderão surgir quando houver VMs atrás do Azure para a infraestrutura do VMware no local, após a ativação pós-falha para a utilização do Azure [do Azure Site Recovery](site-recovery-overview.md).

Reativação pós-falha essencialmente envolve dois passos principais. Após a ativação pós-falha, terá de voltar a proteger as VMs do Azure no local, para que iniciar a replicação. O segundo passo consiste em executar uma ativação pós-falha do Azure, para falhar novamente para o site no local. 

## <a name="troubleshoot-reprotection-errors"></a>Resolver erros só

Esta secção fornece detalhes sobre os erros comuns e como corrija-os.

### <a name="error-code-95226"></a>Código de erro 95226

**Reproteção falhou porque a máquina virtual do Azure não conseguiu aceder ao servidor de configuração no local.**

Este erro ocorre quando:

1. A VM do Azure não consegue contactar o servidor de configuração no local. A VM não pode ser detetada e registada no servidor de configuração. 
2. O serviço de aplicação do InMage Scout não está em execução na VM do Azure após a ativação pós-falha. O serviço é necessário para as comunicações com o servidor de configuração no local.

Para resolver este problema:

1. Certifique-se de que a rede VM do Azure permite que a VM do Azure comunicar com o servidor de configuração no local. Para tal, a configurar uma VPN de site a site para o seu datacenter no local ou configurar uma ligação ExpressRoute com o peering privado da rede virtual de VM do Azure. 
2. Se a VM pode comunicar com o servidor de configuração no local, em seguida, inicie sessão no VM e verifique o 'InMage Scout Application Service'. Se vir que este não está em execução, inicie manualmente o serviço e verifique se o tipo de início do serviço está definido como automático.

### <a name="error-code-78052"></a>Código de erro 78052

***Não foi possível concluir a proteção da máquina virtual.**

Isto pode acontecer se já existir uma VM com o mesmo nome no servidor de destino principal ao qual está a reativar novamente.

Para resolver este problema, proceda do seguinte:
1. Selecione um servidor de destino mestre diferente num anfitrião diferente, pelo que só irá criar a máquina num anfitrião diferente, onde os nomes estão em conflito. 
2. Também pode vMotion o destino principal para outro anfitrião no qual a colisão de nomes não ocorrerá. Se a VM existente é uma máquina stray, alterá-lo para que a nova VM pode ser criada no mesmo anfitrião ESXi.

### <a name="error-code-78093"></a>Código de erro 78093

**A VM não está em execução, num estado bloqueado, ou não está acessível.**

Para voltar a proteger uma falha na ativação pós-falha da VM, tem de executar a VM do Azure. Isto é para que o serviço de mobilidade regista com a configuração servidor no local e pode iniciar a replicação através da comunicação com o servidor de processos. Se a máquina está numa rede incorreta ou não está em execução (estado bloqueado ou encerramento), o servidor de configuração não consegue contactar o serviço de mobilidade na VM, para começar a só. 

1. Reinicie a VM para que possa começar a comunicar back no local.
2. Reinicie a tarefa de reproteção depois de iniciar a máquina virtual do Azure

### <a name="error-code-8061"></a>Código de erro 8061

**O arquivo de dados não está acessível a partir do anfitrião ESXi.**
 
Verifique o [mestra pré-requisitos de destino e arquivos de dados suportados](vmware-azure-reprotect.md#deploy-a-separate-master-target-server) para reativação pós-falha.


## <a name="troubleshoot-failback-errors"></a>Resolver erros de reativação pós-falha

Esta secção descreve erros comuns que poderá encontrar durante a reativação pós-falha.

### <a name="error-code-8038"></a>Código de erro 8038

**Falha ao colocar a máquina de virtual no local devido ao erro**

Isto acontece quando a VM no local seja colocada de cópia de segurança num anfitrião que não tem memória suficiente aprovisionada. Para resolver este problema:

1. Aprovisione mais memória no anfitrião ESXi.
2. Além disso, pode vMotion a VM para outro anfitrião ESXi que tenha memória suficiente para efetuar o arranque a VM.
