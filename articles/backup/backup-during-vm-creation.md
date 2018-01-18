---
title: "Ativar a cópia de segurança de VM do Azure durante a criação | Microsoft Docs"
description: "Consulte os passos para ativar a cópia de segurança da máquina virtual do Azure durante o processo de criação."
services: backup, virtual-machines
documentationcenter: 
author: markgalioto
manager: carmonm
tags: azure-resource-manager, virtual-machine-backup
ms.assetid: 
ms.service: backup, virtual-machines
ms.devlang: na
ms.topic: article
ms.workload: storage-backup-recovery
ms.date: 01/08/2018
ms.author: trinadhk
ms.openlocfilehash: 4041fc555fe4b61d10f84236dcae5156c6282fd3
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/17/2018
---
# <a name="enable-backup-during-azure-virtual-machine-creation"></a>Ativar a cópia de segurança durante a criação da máquina virtual do Azure 

O serviço de cópia de segurança do Azure fornece uma interface para criar e configurar cópias de segurança para a nuvem. Proteger os seus dados, efetuando cópias de segurança, denominadas pontos de recuperação, em intervalos regulares. O Azure Backup cria pontos de recuperação que podem ser armazenados em cofres de recuperação georredundantes. Este artigo fornece detalhes sobre como ativar a cópia de segurança durante a criação de uma máquina virtual (VM) no portal do Azure.  

## <a name="log-in-to-azure"></a>Iniciar sessão no Azure 

Se não estiver já em sessão iniciada na sua conta, inicie sessão no [portal do Azure](http://portal.azure.com).
 
## <a name="create-virtual-machine-with-backup-configured"></a>Criar máquina virtual com a cópia de segurança configurada 

1. No canto superior esquerdo do portal do Azure, clique em **novo**. 

2. Selecione **computação**e, em seguida, selecione uma imagem da máquina virtual.   

3. Introduza as informações da máquina virtual. O nome de utilizador e palavra-passe fornecida é utilizada para iniciar sessão para a máquina virtual. Quando terminar, clique em **OK**. 

4. Selecione um tamanho para a VM.  

5. Em **definições > cópia de segurança**, clique em **ativado** para trazer as definições de configuração de cópia de segurança. Pode aceitar valores predefinidos e clique em **OK** na página de definições para avançar para a página de resumo para criar a VM. Se pretender alterar os valores, siga os passos seguintes.  

6. Crie ou selecione um cofre dos serviços de recuperação, que contém as cópias de segurança da máquina virtual. Se estiver a criar um cofre dos serviços de recuperação, pode escolher um grupo de recursos para o cofre.  

    ![Página de criação de configuração de cópia de segurança no vm](./media/backup-during-vm-creation/create-vm-backup-config.png) 

    > [!NOTE] 
    > O grupo de recursos para o Cofre dos serviços de recuperação pode ser diferente do grupo de recursos para a máquina virtual.  
    > 
    > 

7. Por predefinição, uma política de cópia de segurança está selecionada para que possa proteger rapidamente a VM. Uma política de cópia de segurança especifica a frequência de criar instantâneos de cópia de segurança e quanto tempo pretende manter essas cópias de segurança. Pode aceitar a política predefinida, ou pode criar ou selecionar uma política de cópia de segurança diferentes. Para editar a política de cópia de segurança, selecione **política de cópia de segurança** e alterar os valores da política.  

8. Quando estiver satisfeito com os valores de configuração de cópia de segurança, na página de definição, clique em **OK**.  

9. Na página Resumo, uma vez a validação foi efectuada com êxito, clique em **criar** para criar uma máquina virtual que utiliza as definições de cópia de segurança configuradas. 

## <a name="initiate-a-backup-after-creating-the-vm"></a>Iniciar uma cópia de segurança depois de criar a VM 

Embora a política de cópia de segurança tiver sido criada, é recomendável criar uma cópia de segurança inicial. Para ver os detalhes de cópia de segurança para a virtual máquina uma vez as VM criação modelo depois de concluída, do **operações** definir no menu da esquerda, clique em **cópia de segurança**. Pode utilizar este para acionar uma cópia de segurança a pedido, restaurar uma VM completa ou de todos os discos, restaurar ficheiros a partir de cópia de segurança VM ou altere a política de cópia de segurança associada à máquina virtual.  

## <a name="frequently-asked-questions"></a>Perguntas mais frequentes 

### <a name="which-vm-images-enable-backup-at-the-time-of-vm-creation"></a>As imagens VM ativar a cópia de segurança no momento da criação da VM? 

São suportadas na seguinte lista de imagens de núcleos publicadas pela Microsoft para ativar a cópia de segurança durante a criação da VM. Para outras VMs, pode ativar a cópia de segurança depois da VM ser criada. Saiba mais [ativar cópia de segurança depois de criada a VM](quick-backup-vm-portal.md) 

- **Windows** -Centro de dados do Windows Server 2016, o principal do Centro de dados do Windows Server 2016, o Centro de dados do Windows Server 2012, Windows Server 2012 R2 DataCenter, Windows Server 2008 R2 SP1 
- **Ubuntu** -Ubuntu Server 1710, Ubuntu Server 1704, servidor UUbuntu 1604(LTS), Ubuntu Server 1404(LTS) 
- **VM de Redhat** -RHEL 6.7, 6.8 6.9, 7.2, 7.3, 7.4 
- **SUSE** -SP4 do SUSE Linux Enterprise Server 11, 12 SP2, 12 SP3 
- **Debian** -Debian 8, Debian 9 
- **CentOS** - CentOS 6.9, CentOS 7.3 
- **Oracle Linux** -Oracle Linux 6.7, 6.8, 6.9, 7.2, 7.3 
 
### <a name="is-backup-cost-included-in-the-vm-cost"></a>Está incluído no custo VM de custo de cópia de segurança? 

Não, os custos de cópia de segurança são separados ou distinct, de custos de máquinas virtuais. Para obter mais informações sobre os preços de cópia de segurança, consulte o [preços da cópia de segurança site](https://azure.microsoft.com/pricing/details/backup/).
 
### <a name="which-permissions-are-required-to-enable-backup-on-a-vm"></a>Quais são necessárias permissões para ativar a cópia de segurança numa VM? 

Se for um contribuinte de máquina virtual, pode ativar a cópia de segurança da VM. Se estiver a utilizar uma função personalizada, terá as seguintes permissões para ativar a cópia de segurança da VM com êxito. 

- Microsoft.RecoveryServices/Vaults/write 
- Microsoft.RecoveryServices/Vaults/read 
- Microsoft.RecoveryServices/locations/* 
- Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/*/read 
- Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read 
- Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/write 
- Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/write 
- Microsoft.RecoveryServices/Vaults/backupPolicies/read 
- Microsoft.RecoveryServices/Vaults/backupPolicies/write 
 
Se o Cofre de serviços de recuperação e máquina virtual tiverem grupos de recursos diferente, lembre-se de que tem permissões de escrita no grupo de recurso do Cofre de serviços de recuperação.  

## <a name="next-steps"></a>Passos Seguintes 

Agora que protegeram a VM, consulte os artigos seguintes para saber mais sobre as tarefas de gestão de VM e como restaurar VMs. 

- [Gerir e monitorizar as máquinas virtuais](backup-azure-manage-vms.md) 
- [Monitorizar máquinas virtuais](backup-azure-arm-restore-vms.md) 
