---
title: Atualizar para a V2 de pilha de cópia de segurança de VM do Azure
description: Atualizar o processo e FAQs para a pilha de cópia de segurança da VM, o modelo de implementação do Resource Manager
services: backup
author: trinadhk
manager: vijayts
tags: azure-resource-manager, virtual-machine-backup
ms.service: backup
ms.topic: conceptual
ms.date: 8/1/2018
ms.author: trinadhk
ms.openlocfilehash: 6f19a536861d236a82cc77a17570d8e3004a2ba1
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/09/2018
ms.locfileid: "48888276"
---
# <a name="upgrade-to-azure-vm-backup-stack-v2"></a>Atualizar para a pilha de cópia de segurança do Azure VM V2

O modelo de implementação do Resource Manager para a atualização para a pilha de cópia de segurança de máquina virtual (VM) fornece os seguintes aprimoramentos de recursos:

* Capacidade para ver os instantâneos tirados como parte de uma tarefa de cópia de segurança que está disponível para recuperação sem aguardar a transferência de dados para concluir. Reduz o tempo de espera para instantâneos copiar para o cofre, antes de acionar o restauro. Além disso, esta capacidade elimina o requisito de armazenamento adicional para fazer backup de VMs do premium, exceto a primeira cópia de segurança.  

* Reduz os tempos de backup e restauração ao manter instantâneos localmente, durante sete dias.

* Suporte para discos tamanhos até 4 TB.

* Capacidade de utilizar contas de armazenamento originais de uma VM não gerida, ao restaurar. Esta capacidade existe mesmo quando a VM tem discos que são distribuídos em contas de armazenamento. Ela acelera as operações de restauro para uma grande variedade de configurações de VM.
    > [!NOTE]
    > Esta capacidade não é igual a substituir a VM original.
    >

## <a name="whats-changing-in-the-new-stack"></a>O que está mudando na pilha de novo?
Atualmente, a tarefa de cópia de segurança consiste em duas fases:
1.  Criar um instantâneo VM.
2.  A transferência de um instantâneo VM para o Cofre de cópia de segurança do Azure.

Um ponto de recuperação é considerado criado apenas depois de terminado fases 1 e 2. Como parte da nova pilha, é criado um ponto de recuperação assim que o instantâneo estiver concluído. Também pode restaurar a partir deste ponto de recuperação utilizando o mesmo fluxo de restauro. Pode identificar este ponto de recuperação no portal do Azure ao utilizar "instantâneo" como o tipo de ponto de recuperação. Depois do instantâneo é transferido para o cofre, o tipo de ponto de recuperação é alterado para "instantâneo e cofre."

![Tarefa de cópia de segurança no VM pilha de cópia de segurança do Resource Manager modelo de implementação, armazenamento e o Cofre](./media/backup-azure-vms/instant-rp-flow.jpg)

Por predefinição, os instantâneos são mantidos durante sete dias. Esta funcionalidade permite que o restauro para termine mais depressa destes instantâneos. Ele reduz o tempo necessário para copiar dados do cofre para a conta de armazenamento do cliente.

## <a name="considerations-before-upgrade"></a>Considerações antes da atualização

* A atualização da pilha de cópia de segurança de VM é um direcional, ir de todas as cópias de segurança para este fluxo. Uma vez que a alteração ocorre ao nível da subscrição, todas as VMs Ir para este fluxo. Todas as novas adições de funcionalidade baseiam-se na mesma pilha. Atualmente não é possível controlar a pilha no nível de política.

* Os instantâneos são armazenados localmente para aumentar a criação do ponto de recuperação e também para acelerar as operações de restauro. Como resultado, verá os custos de armazenamento que correspondem aos instantâneos tirados durante o período de sete dias.

* Instantâneos incrementais são armazenados como blobs de páginas. Todos os clientes que utilizam discos não geridos são cobrados durante os sete dias, que os instantâneos são armazenados na conta de armazenamento local do cliente. Uma vez que as coleções de ponto de restauro utilizadas pelas cópias de segurança de VM geridos utilizam instantâneos de blob ao nível do armazenamento subjacente, para discos geridos, verá os custos correspondentes aos [preços de instantâneo de blob](https://docs.microsoft.com/rest/api/storageservices/understanding-how-snapshots-accrue-charges) e são incrementais.

* Se restaurar uma VM do premium a partir de um ponto de recuperação de instantâneo, uma localização de armazenamento temporário é utilizada enquanto a VM é criada.

* Para contas de armazenamento premium, os instantâneos tirados para contagem de pontos de recuperação imediata para o limite de 10 TB de espaço em atribuído.

> [!NOTE]
> Atualização para a pilha de cópia de segurança do Azure VM V2 para obter suporte de cópia de segurança do Azure para o [Standard SSD Managed Disks](https://azure.microsoft.com/blog/announcing-general-availability-of-standard-ssd-disks-for-azure-virtual-machine-workloads/) e máquinas virtuais com até 32 discos de dados.

## <a name="upgrade"></a>Atualizar
### <a name="the-azure-portal"></a>O portal do Azure
Se utilizar o portal do Azure, verá uma notificação no dashboard do cofre. Esta notificação se relaciona com suporte de disco grande e melhorias de velocidade de cópia de segurança e restauro. Como alternativa, pode aceder à página de propriedades do cofre para obter a opção de atualização.

![Tarefa de cópia de segurança no modelo de implementação de Gestor de recursos de pilha de cópia de segurança de VM – notificação de suporte](./media/backup-azure-vms/instant-rp-banner.png)

Para abrir uma tela para atualizar para a nova pilha, selecione a faixa.

![Tarefa de cópia de segurança na pilha de cópia de segurança de VM modelo de implementação do Resource Manager – atualizar](./media/backup-azure-vms/instant-rp.png)

### <a name="powershell"></a>PowerShell
Execute os seguintes cmdlets a partir de um PowerShell elevado terminal:
1.  Inicie sessão sua conta do Azure:

    ```
    PS C:> Connect-AzureRmAccount
    ```

2.  Selecione a subscrição que pretende registar:

    ```
    PS C:>  Get-AzureRmSubscription –SubscriptionName "Subscription Name" | Select-AzureRmSubscription
    ```

3.  Registe esta subscrição:

    ```
    PS C:>  Register-AzureRmProviderFeature -FeatureName "InstantBackupandRecovery" –ProviderNamespace Microsoft.RecoveryServices
    ```

## <a name="verify-that-the-upgrade-is-finished"></a>Certifique-se de que a atualização estiver concluída
A partir de um terminal do PowerShell elevada, execute o seguinte cmdlet:

```
Get-AzureRmProviderFeature -FeatureName "InstantBackupandRecovery" –ProviderNamespace Microsoft.RecoveryServices
```

Se disser "Registado", a sua subscrição está atualizada para o modelo de implementação de Gestor de recursos de pilha de cópia de segurança de VM.

## <a name="frequently-asked-questions"></a>Perguntas mais frequentes

As seguintes perguntas e respostas tenham sido recolhidas a partir de fóruns e questões dos clientes.

### <a name="will-upgrading-to-v2-impact-current-backups"></a>Atualizar para o V2 afetará as cópias de segurança atuais?
Se atualizar para o V2, não há nenhum impacto sobre a seus backups atuais e sem a necessidade de reconfigurar o seu ambiente. Atualização e o ambiente de cópia de segurança continua a funcionar porque esta tem.

### <a name="what-does-it-cost-to-upgrade-to-azure-vm-backup-stack-v2"></a>O que custa para atualizar para a v2 de pilha de cópia de segurança do Azure VM?
Não existe nenhum custo para atualizar a pilha para v2. Os instantâneos são armazenados localmente para acelerar a criação do ponto de recuperação e operações de restauro. Como resultado, verá os custos de armazenamento que correspondem aos instantâneos realizados durante o período de sete dias.

### <a name="does-upgrading-to-stack-v2-increase-the-premium-storage-account-snapshot-limit-by-10-tb"></a>Atualizar para a v2 de pilha aumentar o limite de instantâneos de conta de armazenamento premium, 10 TB?
Não, o limite de instantâneo total por conta de armazenamento ainda falta em 10TB.

### <a name="in-premium-storage-accounts-do-snapshots-taken-for-instant-recovery-point-occupy-the-10-tb-snapshot-limit"></a>Nas contas de armazenamento Premium, instantâneos tirados de ponto de recuperação instantânea ocupam o limite de 10 TB de instantâneo?
Sim, para contas de armazenamento premium, os instantâneos tirados de ponto de recuperação instantânea, ocupam o alocado 10 TB de espaço.

### <a name="how-does-the-snapshot-work-during-the-seven-day-period"></a>Como funciona o instantâneo durante o período de sete dias?
Por dia é obtido um instantâneo de novo. Existem sete instantâneos individuais. O serviço faz **não** fazer uma cópia no primeiro dia e adicionar as alterações para os próximos seis dias.

### <a name="is-a-v2-snapshot-an-incremental-snapshot-or-full-snapshot"></a>Um instantâneo incremental ou um instantâneo completo, é um instantâneo do v2?
Instantâneos incrementais são utilizados para discos não geridos. Para discos geridos, coleção criada por cópia de segurança do Azure utiliza instantâneos de blob e, por conseguinte, são incremental de ponto de restauro.

### <a name="how-to-get-standard-ssd-managed-disk-support-for-a-virtual-machine"></a>Como obter SSD padrão geridos pelo suporte de disco para uma máquina virtual?
Atualização para a pilha de cópia de segurança do Azure VM V2 para obter suporte de cópia de segurança do Azure para o [Standard SSD Managed Disks](https://azure.microsoft.com/blog/announcing-general-availability-of-standard-ssd-disks-for-azure-virtual-machine-workloads/). Depois de atualizar, pode também fazer backup de máquinas virtuais com até 32 discos de dados.
